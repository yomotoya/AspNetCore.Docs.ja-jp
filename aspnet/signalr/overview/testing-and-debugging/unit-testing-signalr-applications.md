---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "単体テストの SignalR アプリケーションを |Microsoft ドキュメント"
author: pfletcher
description: "この記事では、SignalR 2.0 の単体テスト機能を使用する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: e55efd644dd4b6fb57061ffb89a5c041136c7b5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="86293-103">単体テストの SignalR アプリケーション</span><span class="sxs-lookup"><span data-stu-id="86293-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="86293-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="86293-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="86293-105">この記事では、SignalR 2 の単体テスト機能の使用について説明します。</span><span class="sxs-lookup"><span data-stu-id="86293-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="86293-106">このトピックで使用されているソフトウェア バージョン</span><span class="sxs-lookup"><span data-stu-id="86293-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="86293-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="86293-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="86293-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="86293-108">.NET 4.5</span></span>
> - <span data-ttu-id="86293-109">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="86293-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="86293-110">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="86293-110">Questions and comments</span></span>
> 
> <span data-ttu-id="86293-111">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="86293-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="86293-112">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="86293-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="86293-113">単体テストの SignalR アプリケーション</span><span class="sxs-lookup"><span data-stu-id="86293-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="86293-114">SignalR 2 で単体テスト機能を使用して SignalR アプリケーションの単体テストを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="86293-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="86293-115">SignalR 2 が含まれています、 [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx)インターフェイスで、テストのハブ メソッドをシミュレートするモック オブジェクトを作成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="86293-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="86293-116">このセクションで追加で作成したアプリケーションの単体テスト、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)を使用して[XUnit.net](https://github.com/xunit/xunit)と[Moq](https://github.com/Moq/moq4)です。</span><span class="sxs-lookup"><span data-stu-id="86293-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="86293-117">テストの制御に使用される XUnit.net作成に使用される Moq、[模擬](http://en.wikipedia.org/wiki/Mock_object)テスト用のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="86293-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="86293-118">必要な場合、他のモック フレームワークを使用できます。[NSubstitute](http://nsubstitute.github.io/)もをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="86293-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="86293-119">このチュートリアルは、モック オブジェクトを 2 つの方法で設定する方法を示します: 最初を使用して、`dynamic`オブジェクト (.NET Framework 4 で導入)、および秒のインターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="86293-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="86293-120">目次</span><span class="sxs-lookup"><span data-stu-id="86293-120">Contents</span></span>

<span data-ttu-id="86293-121">このチュートリアルでは、次のセクションでは、含まれています。</span><span class="sxs-lookup"><span data-stu-id="86293-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="86293-122">動的を使用した単体テスト</span><span class="sxs-lookup"><span data-stu-id="86293-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="86293-123">単体テストの種類</span><span class="sxs-lookup"><span data-stu-id="86293-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="86293-124">動的を使用した単体テスト</span><span class="sxs-lookup"><span data-stu-id="86293-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="86293-125">このセクションで作成したアプリケーションの単体テストを追加、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)動的オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="86293-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="86293-126">インストール、 [XUnit ランナー拡張子](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099)for Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="86293-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="86293-127">完了するか、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)から完成したアプリケーションをダウンロードまたは[MSDN コード ギャラリー](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)です。</span><span class="sxs-lookup"><span data-stu-id="86293-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="86293-128">はじめにアプリケーションのダウンロード版を使用している場合は開きます**パッケージ マネージャー コンソール** をクリック**復元**SignalR パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="86293-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![パッケージを復元します。](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="86293-130">単体テストのソリューションにプロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="86293-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="86293-131">ソリューションを右クリックして**ソリューション エクスプ ローラー**選択**追加**、**新しいプロジェクト.**.下にある、 **c#**ノードで、選択、 **Windows**ノード。</span><span class="sxs-lookup"><span data-stu-id="86293-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="86293-132">選択**クラス ライブラリ**です。</span><span class="sxs-lookup"><span data-stu-id="86293-132">Select **Class Library**.</span></span> <span data-ttu-id="86293-133">新しいプロジェクトの名前**TestLibrary**  をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="86293-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![テスト ライブラリを作成します。](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="86293-135">テスト ライブラリ プロジェクトで SignalRChat プロジェクトへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="86293-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="86293-136">右クリックし、 **TestLibrary**プロジェクトし、選択**追加**、**参照しています.**.選択、**プロジェクト**ノードの下、**ソリューション**ノード、およびチェック**SignalRChat**です。</span><span class="sxs-lookup"><span data-stu-id="86293-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="86293-137">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="86293-137">Click **OK**.</span></span>

    ![プロジェクト参照を追加します。](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="86293-139">SignalR、Moq、XUnit パッケージを追加、 **TestLibrary**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="86293-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="86293-140">**Package Manager Console**、設定、**プロジェクトの既定の**ドロップダウンを**TestLibrary**です。</span><span class="sxs-lookup"><span data-stu-id="86293-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="86293-141">コンソール ウィンドウで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="86293-141">Run the following commands in the console window:</span></span>

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![パッケージをインストールします。](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="86293-143">テスト ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="86293-143">Create the test file.</span></span> <span data-ttu-id="86293-144">右クリックし、 **TestLibrary**プロジェクトし、クリックして**追加しています.**、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="86293-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="86293-145">新しいクラスの名前を**Tests.cs**です。</span><span class="sxs-lookup"><span data-stu-id="86293-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="86293-146">Tests.cs の内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="86293-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="86293-147">使用して、テスト用クライアントを作成、上記のコードで、`Mock`オブジェクトから、 [Moq](https://github.com/Moq/moq4)型のライブラリ、 [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 に割り当てる`dynamic`の種類パラメーターです。)`IHubCallerConnectionContext`インターフェイスは、プロキシ オブジェクトを使用するクライアントのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="86293-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="86293-148">`broadcastMessage`関数は呼び出すことができるように、次に、モック クライアントの定義、`ChatHub`クラスです。</span><span class="sxs-lookup"><span data-stu-id="86293-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="86293-149">テスト エンジンは、呼び出し、`Send`のメソッド、`ChatHub`を呼び出して、モック クラス`broadcastMessage`関数。</span><span class="sxs-lookup"><span data-stu-id="86293-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="86293-150">キーを押して、ソリューションをビルド**F6**です。</span><span class="sxs-lookup"><span data-stu-id="86293-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="86293-151">単体テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="86293-151">Run the unit test.</span></span> <span data-ttu-id="86293-152">Visual Studio で、次のように選択します。**テスト**、 **Windows**、**テスト エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="86293-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="86293-153">テスト エクスプ ローラー ウィンドウで右クリック**HubsAreMockableViaDynamic**選択**選択したテストの実行**です。</span><span class="sxs-lookup"><span data-stu-id="86293-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![テスト エクスプローラー](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="86293-155">テスト エクスプ ローラー ウィンドウで、下のウィンドウをチェックして、テストが成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="86293-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="86293-156">テストが成功した、ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="86293-156">The window will show that the test passed.</span></span>

    ![テスト成功](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="86293-158">単体テストの種類</span><span class="sxs-lookup"><span data-stu-id="86293-158">Unit testing by type</span></span>

<span data-ttu-id="86293-159">このセクションで作成したアプリケーションのテストを追加するが、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)をテストするメソッドを含むインターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="86293-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="86293-160">手順 1. ~ 7. を完了、[単体テストの動的な](#dynamic)上記のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="86293-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="86293-161">Tests.cs の内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="86293-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="86293-162">署名を定義するインターフェイスを作成、上記のコードで、`broadcastMessage`メソッドで、テスト エンジンがモック クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="86293-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="86293-163">モック クライアントを使用してを作成し、`Mock`型のオブジェクト[IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 に割り当てる`dynamic`型パラメーターです)。`IHubCallerConnectionContext`インターフェイスは、プロキシ オブジェクトを使用するクライアントのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="86293-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="86293-164">インスタンスを作成、テスト`ChatHub`、しのモック版を作成、`broadcastMessage`メソッドを呼び出すことによってさらに呼び出される、`Send`ハブのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="86293-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="86293-165">キーを押して、ソリューションをビルド**F6**です。</span><span class="sxs-lookup"><span data-stu-id="86293-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="86293-166">単体テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="86293-166">Run the unit test.</span></span> <span data-ttu-id="86293-167">Visual Studio で、次のように選択します。**テスト**、 **Windows**、**テスト エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="86293-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="86293-168">テスト エクスプ ローラー ウィンドウで右クリック**HubsAreMockableViaDynamic**選択**選択したテストの実行**です。</span><span class="sxs-lookup"><span data-stu-id="86293-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![テスト エクスプローラー](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="86293-170">テスト エクスプ ローラー ウィンドウで、下のウィンドウをチェックして、テストが成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="86293-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="86293-171">テストが成功した、ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="86293-171">The window will show that the test passed.</span></span>

    ![テスト成功](unit-testing-signalr-applications/_static/image8.png)
