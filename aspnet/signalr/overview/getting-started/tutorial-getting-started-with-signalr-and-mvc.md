---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'チュートリアル: SignalR 2 と MVC 5 の概要 |Microsoft Docs'
author: pfletcher
description: このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を MVC 5 アプリケーションに追加され、チャットのビューを作成しています.
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 4a4c013ff047f18f9d9b88595af7951577f3f200
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838651"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="5c1fb-104">チュートリアル: SignalR 2 と MVC 5 の概要</span><span class="sxs-lookup"><span data-stu-id="5c1fb-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="5c1fb-105">によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="5c1fb-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="5c1fb-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="5c1fb-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="5c1fb-107">このチュートリアルでは、ASP.NET SignalR 2 を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="5c1fb-108">SignalR を MVC 5 アプリケーションに追加し、チャットを送信し、メッセージを表示するビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5c1fb-109">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="5c1fb-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="5c1fb-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5c1fb-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5c1fb-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5c1fb-111">.NET 4.5</span></span>
> - <span data-ttu-id="5c1fb-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="5c1fb-112">MVC 5</span></span>
> - <span data-ttu-id="5c1fb-113">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="5c1fb-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="5c1fb-114">このチュートリアルで Visual Studio 2012 の使用</span><span class="sxs-lookup"><span data-stu-id="5c1fb-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="5c1fb-115">このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="5c1fb-116">更新プログラム、[パッケージ マネージャー](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="5c1fb-117">インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="5c1fb-118">Web Platform Installer で検索してインストール**ASP.NET と Visual Studio 2012 for Web Tools 2013.1**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="5c1fb-119">SignalR クラスの Visual Studio テンプレートなどのインストールはこの**ハブ**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="5c1fb-120">一部のテンプレート (など**OWIN Startup クラス**) はできなくなります。 これらの場合には、クラス ファイルを代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="5c1fb-121">チュートリアルのバージョン</span><span class="sxs-lookup"><span data-stu-id="5c1fb-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="5c1fb-122">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5c1fb-123">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="5c1fb-123">Questions and comments</span></span>
> 
> <span data-ttu-id="5c1fb-124">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5c1fb-125">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="5c1fb-126">概要</span><span class="sxs-lookup"><span data-stu-id="5c1fb-126">Overview</span></span>

<span data-ttu-id="5c1fb-127">このチュートリアルでは、ASP.NET SignalR 2 と ASP.NET MVC 5 でリアルタイムの web アプリケーションの開発について説明します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="5c1fb-128">チュートリアルと同じのチャット アプリケーションのコードを使用して、 [SignalR 作業開始のチュートリアル](tutorial-getting-started-with-signalr.md)が、MVC 5 アプリケーションに追加する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="5c1fb-129">このトピックでは、次の SignalR 開発タスクを学習します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="5c1fb-130">MVC 5 アプリケーションへの SignalR ライブラリを追加します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="5c1fb-131">ハブとクライアントにコンテンツをプッシュする OWIN スタートアップ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="5c1fb-132">Web ページで、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブから更新プログラムを表示します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="5c1fb-133">次のスクリーン ショットは、ブラウザーで実行されている完成したチャット アプリケーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![チャット インスタンス](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="5c1fb-135">セクション:</span><span class="sxs-lookup"><span data-stu-id="5c1fb-135">Sections:</span></span>

- [<span data-ttu-id="5c1fb-136">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="5c1fb-137">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="5c1fb-138">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="5c1fb-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="5c1fb-139">次の手順</span><span class="sxs-lookup"><span data-stu-id="5c1fb-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="5c1fb-140">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-140">Set up the Project</span></span>

<span data-ttu-id="5c1fb-141">必要条件:</span><span class="sxs-lookup"><span data-stu-id="5c1fb-141">Prerequisites:</span></span>

- <span data-ttu-id="5c1fb-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5c1fb-142">Visual Studio 2013.</span></span> <span data-ttu-id="5c1fb-143">Visual Studio がないを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)無料 Visual Studio 2013 Express 開発ツールを取得します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="5c1fb-144">このセクションでは、ASP.NET MVC 5 アプリケーションを作成し、SignalR ライブラリを追加、チャット アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="5c1fb-145">Visual Studio で、c# ASP.NET アプリケーションを作成するには、.NET Framework 4.5 を対象として、SignalRChat、という名前を付けます [ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Web を作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="5c1fb-147">`New ASP.NET Project`ダイアログ、および選択**MVC**、 をクリック**認証の変更**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Web を作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="5c1fb-149">選択**認証なし**で、**認証の変更**ダイアログ ボックスをクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![認証なし を選択します。](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="5c1fb-151">アプリケーションのさまざまな認証プロバイダーを選択した場合、`Startup.cs`クラスが作成されます。 独自に作成する必要はありません`Startup.cs`10 以下の手順でクラス。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="5c1fb-152">クリックして**OK**で、**新しい ASP.NET プロジェクト**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="5c1fb-153">開く、**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="5c1fb-154">この手順では、一連のスクリプト ファイルと SignalR の機能を有効にするアセンブリ参照をプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="5c1fb-155">**ソリューション エクスプ ローラー**、Scripts フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="5c1fb-156">SignalR のスクリプト ライブラリがプロジェクトに追加されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Scripts フォルダー](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="5c1fb-158">**ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |新しいフォルダー**、という名前の新しいフォルダーを追加および**Hubs**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="5c1fb-159">右クリックし、 **Hubs**フォルダー、をクリックして**追加 |新しい項目の**を選択、 **(Visual C#) |Web |SignalR**内のノード、**インストール済み**ペインで、 **SignalR ハブ クラス (v2)** 中央のウィンドウからという名前の新しいハブを作成および**ChatHub.cs**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="5c1fb-160">このクラスは、すべてのクライアントにメッセージを送信する SignalR サーバー ハブとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![新しいハブを作成します。](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="5c1fb-162">コードに置き換えます、 **ChatHub**クラスを次のコード。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="5c1fb-163">Startup.cs という新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="5c1fb-164">次に、ファイルの内容を変更します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="5c1fb-165">編集、`HomeController`クラス**Controllers/HomeController.cs**クラスに次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="5c1fb-166">このメソッドが戻る、**チャット**後の手順で作成するビュー。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="5c1fb-167">右クリックし、**ビュー/ホーム**フォルダー、および選択**追加.. |。ビュー**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="5c1fb-168">**ビューの追加**ダイアログ ボックスで、名前の新しいビュー**チャット**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![ビューを追加する](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="5c1fb-170">内容を置き換える**Chat.cshtml**を次のコード。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="5c1fb-171">SignalR およびその他のスクリプト ライブラリを Visual Studio プロジェクトに追加すると、パッケージ マネージャーはこのトピックに示すバージョンよりも新しい SignalR スクリプト ファイルのバージョンをインストール可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="5c1fb-172">コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンと一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="5c1fb-173">**すべて保存**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="5c1fb-174">サンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-174">Run the Sample</span></span>

1. <span data-ttu-id="5c1fb-175">F5 キーを押して、デバッグ モードで、プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="5c1fb-176">ブラウザーのアドレスの行の追加 **/ホーム/チャット**プロジェクトの既定のページの URL にします。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="5c1fb-177">ブラウザーのインスタンスとユーザー名のプロンプトで、チャット ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![ユーザー名の入力](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="5c1fb-179">ユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-179">Enter a user name.</span></span>
4. <span data-ttu-id="5c1fb-180">ブラウザーのアドレスの行から URL をコピーし、2 つのブラウザー インスタンスを開くために使用します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="5c1fb-181">各ブラウザー インスタンスでは、一意のユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="5c1fb-182">各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="5c1fb-183">すべてのブラウザー インスタンスで、コメントが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5c1fb-184">この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="5c1fb-185">ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="5c1fb-186">後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="5c1fb-187">次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![チャット ブラウザー](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="5c1fb-189">**ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="5c1fb-190">お使いのブラウザーとして Internet Explorer を使用している場合、このノードはデバッグ モードに表示されます。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="5c1fb-191">という名前のスクリプト ファイルがある**hubs** SignalR ライブラリは、実行時に動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="5c1fb-192">このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="5c1fb-193">Internet Explorer 以外のブラウザーを使用する場合は、動的もアクセスできる**hubs**ファイルを参照して直接、たとえばhttp://mywebsite/signalr/hubsします。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="5c1fb-194">コードを確認します</span><span class="sxs-lookup"><span data-stu-id="5c1fb-194">Examine the Code</span></span>

<span data-ttu-id="5c1fb-195">SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示します。 サーバーで、主要な調整オブジェクトとしてハブを作成し、メッセージを送受信する SignalR jQuery ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="5c1fb-196">SignalR ハブ</span><span class="sxs-lookup"><span data-stu-id="5c1fb-196">SignalR Hubs</span></span>

<span data-ttu-id="5c1fb-197">コード サンプルでは、 **ChatHub**クラスから派生、 **Microsoft.AspNet.SignalR.Hub**クラス。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="5c1fb-198">派生する、**ハブ**クラスは、SignalR アプリケーションを構築する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="5c1fb-199">ハブ クラスにパブリック メソッドを作成し、web ページ内のスクリプトから呼び出すことによってこれらのメソッドにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="5c1fb-200">クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="5c1fb-201">ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.addNewMessageToPage**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="5c1fb-202">**送信**メソッドがいくつかのハブの概念を示します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="5c1fb-203">クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="5c1fb-204">使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**この hub に接続されているすべてのクライアントにアクセスするプロパティ。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="5c1fb-205">クライアントで関数を呼び出す (など、`addNewMessageToPage`関数) クライアントを更新します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="5c1fb-206">SignalR と jQuery</span><span class="sxs-lookup"><span data-stu-id="5c1fb-206">SignalR and jQuery</span></span>

<span data-ttu-id="5c1fb-207">**Chat.cshtml**ビュー ファイルのコード サンプルでは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="5c1fb-208">コードで基本的なタスクでは、自動生成されたプロキシをハブをクライアントにコンテンツをプッシュするサーバーを呼び出すことができる関数を宣言して、接続を開始、ハブにメッセージを送信への参照を作成します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="5c1fb-209">次のコードでは、ハブ プロキシへの参照を宣言します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="5c1fb-210">JavaScript でサーバー クラスとそのメンバーへの参照がキャメル ケースです。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="5c1fb-211">コード サンプルを参照して、c# **ChatHub**として JavaScript でクラス**chatHub**します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="5c1fb-212">参照する場合、`ChatHub`クラス jquery では通常 pascal 形式で、c# の場合と大文字小文字の区別 ChatHub.cs クラス ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="5c1fb-213">追加、`using`を参照するステートメント、`Microsoft.AspNet.SignalR.Hubs`名前空間。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="5c1fb-214">追加し、`HubName`属性を`ChatHub`クラス、たとえば`[HubName("ChatHub")]`します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="5c1fb-215">参照を jQuery を最後に、更新、`ChatHub`クラス。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="5c1fb-216">次のコードでは、スクリプトでコールバック関数を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="5c1fb-217">サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="5c1fb-218">省略可能な呼び出し、 `htmlEncode`  ページで、スクリプト インジェクションを防止する手段として表示する前に関数を HTML に方法が表示されますが、メッセージの内容をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="5c1fb-219">次のコードでは、ハブとの接続を開く方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="5c1fb-220">コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**チャット ページ ボタン。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="5c1fb-221">このアプローチにより、イベント ハンドラーが実行される前に、接続が確立されていること。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="5c1fb-222">次の手順</span><span class="sxs-lookup"><span data-stu-id="5c1fb-222">Next Steps</span></span>

<span data-ttu-id="5c1fb-223">SignalR がリアルタイムの web アプリケーションを構築するためのフレームワークについて説明しました。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="5c1fb-224">いくつかの SignalR 開発タスクも学習しました。 SignalR を ASP.NET アプリケーションを追加する方法、ハブ クラスを作成する方法と、ハブからメッセージを送受信する方法。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="5c1fb-225">サンプルの SignalR アプリケーションを Azure にデプロイする方法のチュートリアルは、次を参照してください。 [Azure App Service Web apps を使用して SignalR](../deployment/using-signalr-with-azure-web-sites.md)します。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="5c1fb-226">Visual Studio web プロジェクトを Windows Azure の Web サイトにデプロイする方法の詳細については、次を参照してください。 [Azure App Service で ASP.NET web アプリを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)です。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="5c1fb-227">高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5c1fb-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="5c1fb-228">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="5c1fb-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="5c1fb-229">SignalR Github とサンプル</span><span class="sxs-lookup"><span data-stu-id="5c1fb-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="5c1fb-230">SignalR の Wiki</span><span class="sxs-lookup"><span data-stu-id="5c1fb-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
