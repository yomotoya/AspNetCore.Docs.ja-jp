---
uid: signalr/overview/advanced/dependency-injection
title: SignalR の依存関係の挿入 |Microsoft Docs
author: bradygaster
description: このトピックの「Visual Studio 2013 .NET 4.5 SignalR 使用されるソフトウェアのバージョンは以前のバージョンについてはこのトピック以前バージョンをバージョン 2.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 54e263e277852d2d478ce5bccd4164254498831a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836611"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="2dd9e-103">SignalR の依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="2dd9e-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="2dd9e-104">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="2dd9e-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="2dd9e-105">このトピックで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="2dd9e-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="2dd9e-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2dd9e-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="2dd9e-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2dd9e-107">.NET 4.5</span></span>
> - <span data-ttu-id="2dd9e-108">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="2dd9e-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="2dd9e-109">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="2dd9e-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="2dd9e-110">SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="2dd9e-111">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="2dd9e-111">Questions and comments</span></span>
>
> <span data-ttu-id="2dd9e-112">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2dd9e-113">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="2dd9e-114">依存関係の挿入は、簡単になります (モック オブジェクトを使用して) テストのいずれかのオブジェクトの依存関係を置換するか、実行時の動作を変更するオブジェクト間の依存関係をハードコーディングを削除する方法です。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="2dd9e-115">このチュートリアルでは、SignalR ハブの依存関係の挿入を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="2dd9e-116">SignalR で IoC コンテナーを使用する方法も示します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="2dd9e-117">IoC コンテナーは、依存関係の挿入の一般的なフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="2dd9e-118">依存関係の挿入とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-118">What is Dependency Injection?</span></span>

<span data-ttu-id="2dd9e-119">依存関係の挿入を理解している場合は、このセクションをスキップします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="2dd9e-120">*依存関係の注入*(DI) は、パターン オブジェクトが独自の依存関係の作成を担当します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="2dd9e-121">DI を顧客が簡単な例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="2dd9e-122">メッセージを記録する必要があるオブジェクトがあるとします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="2dd9e-123">ログ記録のインターフェイスを定義する場合があります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="2dd9e-124">オブジェクトを作成できます、`ILogger`メッセージを記録します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="2dd9e-125">これは、動作しますが、最適なデザインではありません。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-125">This works, but it's not the best design.</span></span> <span data-ttu-id="2dd9e-126">置換する場合`FileLogger`別`ILogger`の実装を変更する必要がある`SomeComponent`します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="2dd9e-127">多くの他のオブジェクトで使用されると`FileLogger`、それらのすべてを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="2dd9e-128">作成する場合または`FileLogger`シングルトンもする必要があります、アプリケーション全体での変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="2dd9e-129">「注入」するほうが効果的、`ILogger`オブジェクトに、たとえば、コンス トラクター引数を使用しています。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="2dd9e-130">選択するため、オブジェクトはありませんので`ILogger`を使用します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="2dd9e-131">スイッチのできます`ILogger`それに依存するオブジェクトを変更することがなく実装します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="2dd9e-132">このパターンと呼ばれます[コンス トラクターの挿入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="2dd9e-133">パターンの 1 つは、セッターの挿入、setter メソッドまたはプロパティから依存関係を設定します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="2dd9e-134">SignalR で単純な依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="2dd9e-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="2dd9e-135">チュートリアルのチャット アプリケーションを考えます[SignalR の概要](../getting-started/tutorial-getting-started-with-signalr.md)します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="2dd9e-136">そのアプリケーションからハブ クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="2dd9e-137">チャット メッセージを送信する前にサーバーに格納するとします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="2dd9e-138">この機能は、抽象化するインターフェイスを定義してにインターフェイスを挿入する DI を使用する場合があります、`ChatHub`クラス。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="2dd9e-139">唯一の問題は、SignalR アプリケーションがハブ; を直接作成していないことSignalR を作成します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="2dd9e-140">既定では、SignalR には、パラメーターなしのコンス トラクターを持つハブ クラスが必要です。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="2dd9e-141">ただし、ハブのインスタンスを作成する関数を登録して、この関数を使用して、DI を実行することができます簡単にします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="2dd9e-142">関数を呼び出すことによって登録**GlobalHost.DependencyResolver.Register**します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="2dd9e-143">SignalR は、作成する必要があるたびに、この匿名関数が呼び出すので、`ChatHub`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="2dd9e-144">IoC コンテナー</span><span class="sxs-lookup"><span data-stu-id="2dd9e-144">IoC Containers</span></span>

<span data-ttu-id="2dd9e-145">前のコードは、単純なケースに適してします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="2dd9e-146">これを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="2dd9e-147">多くの依存関係を持つ複雑なアプリケーションでは、多くの「接続」このコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="2dd9e-148">このコードの依存関係が入れ子になった場合に特にハードを維持することはできます。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="2dd9e-149">また、単体テストは困難です。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-149">It is also hard to unit test.</span></span>

<span data-ttu-id="2dd9e-150">1 つのソリューションでは、IoC コンテナーを使用します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="2dd9e-151">IoC コンテナーは、依存関係の管理を担当するソフトウェア コンポーネントです。型、コンテナーを登録し、コンテナーを使用して、オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="2dd9e-152">コンテナーは、依存関係を自動的に検出します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="2dd9e-153">多くの IoC コンテナーでは、オブジェクトの有効期間とスコープなどを制御することもできます。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="2dd9e-154">"IoC"「の制御の反転」の略フレームワークからアプリケーション コードを呼び出す、一般的なパターンは。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="2dd9e-155">IoC コンテナーを構築、オブジェクト、コントロールの通常のフローの「反転」をします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="2dd9e-156">SignalR の IoC コンテナーの使用</span><span class="sxs-lookup"><span data-stu-id="2dd9e-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="2dd9e-157">チャット アプリケーションは、IoC コンテナーのメリットは単純すぎる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="2dd9e-158">代わりを見てみましょう、 [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample)サンプル。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="2dd9e-159">StockTicker サンプルでは、2 つの主要なクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="2dd9e-160">`StockTickerHub`:ハブ クラスはクライアント接続を管理します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="2dd9e-161">`StockTicker`:株価を保持し、定期的に更新するシングルトン。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="2dd9e-162">`StockTickerHub` 参照を保持、`StockTicker`シングルトン、中に`StockTicker`への参照を保持、 **IHubConnectionContext**の`StockTickerHub`します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="2dd9e-163">このインターフェイスを使用して通信を`StockTickerHub`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="2dd9e-164">(詳細については、次を参照してください[ASP.NET SignalR によるサーバー ブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md)。)。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="2dd9e-165">これらの依存関係を少し整理して、IoC コンテナーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="2dd9e-166">まず、少し簡略化し、`StockTickerHub`と`StockTicker`クラス。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="2dd9e-167">次のコードではコメント部分にしない必要があります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="2dd9e-168">パラメーターなしのコンス トラクターを削除`StockTickerHub`します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="2dd9e-169">代わりに、ハブを作成するのに DI を常に使用します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="2dd9e-170">StockTicker シングルトン インスタンスを削除します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="2dd9e-171">その後、StockTicker 有効期間を制御、IoC コンテナーを使用します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="2dd9e-172">パブリック コンス トラクターは、また、ください。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="2dd9e-173">次に、インターフェイスを作成して、コードをリファクタリングできます`StockTicker`します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="2dd9e-174">このインターフェイスを使用して、分離、`StockTickerHub`から、`StockTicker`クラス。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="2dd9e-175">Visual Studio でこの種のリファクタリングも簡単。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="2dd9e-176">StockTicker.cs ファイルを開きを右クリックし、`StockTicker`クラス宣言、および選択**リファクタリング**.**インターフェイスの抽出**します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="2dd9e-177">**インターフェイスの抽出**ダイアログ ボックスで、をクリックして**すべて選択**します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="2dd9e-178">その他の既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-178">Leave the other defaults.</span></span> <span data-ttu-id="2dd9e-179">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="2dd9e-180">Visual Studio は、という名前の新しいインターフェイスを作成します。 `IStockTicker`、し、変更も`StockTicker`から派生する`IStockTicker`します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="2dd9e-181">IStockTicker.cs ファイルを開き、インターフェイスを変更**パブリック**します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="2dd9e-182">`StockTickerHub`クラス、2 つのインスタンスを変更する`StockTicker`に`IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="2dd9e-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="2dd9e-183">作成、`IStockTicker`インターフェイスが厳密には必要はありませんが、DI、アプリケーションのコンポーネント間の結合の削減を支援する方法を表示したいです。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="2dd9e-184">Ninject ライブラリを追加します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-184">Add the Ninject Library</span></span>

<span data-ttu-id="2dd9e-185">.NET の多くのオープン ソース IoC コンテナーがあります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="2dd9e-186">このチュートリアルで使用します[Ninject](http://www.ninject.org/)します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="2dd9e-187">(その他の一般的なライブラリを含める[Castle Windsor](http://www.castleproject.org/)、 [Spring.Net](http://www.springframework.net/)、 [Autofac](https://code.google.com/p/autofac/)、 [Unity](https://github.com/unitycontainer/unity)、および[StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="2dd9e-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="2dd9e-188">NuGet パッケージ マネージャーを使用して、インストール、 [Ninject ライブラリ](https://nuget.org/packages/Ninject/3.0.1.10)します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="2dd9e-189">Visual Studio から、**ツール**メニューの  **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="2dd9e-190">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="2dd9e-191">SignalR の依存関係競合回避モジュールを置き換えます</span><span class="sxs-lookup"><span data-stu-id="2dd9e-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="2dd9e-192">SignalR 内 Ninject を使用するから派生したクラスを作成**DefaultDependencyResolver**します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="2dd9e-193">このクラスのオーバーライド、 **GetService**と**GetServices**メソッドの**DefaultDependencyResolver**します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="2dd9e-194">SignalR ハブのインスタンスの SignalR によって内部的に使用されるさまざまなサービスなど、実行時にさまざまなオブジェクトを作成するこれらのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="2dd9e-195">**GetService**メソッドは、型の 1 つのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="2dd9e-196">このメソッドを呼び出す、Ninject カーネルのオーバーライド**TryGet**メソッド。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="2dd9e-197">そのメソッドが null を返す場合は、既定の競合回避モジュールに戻ります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="2dd9e-198">**GetServices**メソッドは、指定した型のオブジェクトのコレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="2dd9e-199">既定のリゾルバーからの結果には、Ninject からの結果を連結するには、このメソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="2dd9e-200">Ninject バインドを構成します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="2dd9e-201">宣言型バインディングに Ninject を使用します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="2dd9e-202">アプリケーションの Startup.cs クラスを開きます (ことか手動で作成したパッケージ」の手順に従って`readme.txt`プロジェクトに認証を追加することによって作成されたか)。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="2dd9e-203">`Startup.Configuration`メソッド、Ninject 呼び出す Ninject コンテナーを作成、*カーネル*します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="2dd9e-204">このカスタム依存関係競合回避モジュールのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="2dd9e-205">バインドを作成`IStockTicker`次のようにします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="2dd9e-206">このコードは次の 2 つことを示しています。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-206">This code is saying two things.</span></span> <span data-ttu-id="2dd9e-207">最初に、アプリケーションに必要なときに、 `IStockTicker`、カーネルがのインスタンスを作成する必要があります`StockTicker`します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="2dd9e-208">2 番目、`StockTicker`クラスはシングルトン オブジェクトとして作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="2dd9e-209">Ninject では、オブジェクトの 1 つのインスタンスを作成し、要求ごとに同じインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="2dd9e-210">バインドを作成**IHubConnectionContext**次のようにします。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="2dd9e-211">このコードを返す匿名関数を creatres、 **IHubConnection**します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="2dd9e-212">**WhenInjectedInto**メソッドに指示を作成するときにのみ、この関数を使用する Ninject`IStockTicker`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="2dd9e-213">理由は、SignalR が作成される**IHubConnectionContext**インスタンスを内部的には、SignalR での作成方法をオーバーライドする必要はないです。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="2dd9e-214">この関数にのみ適用されます、`StockTicker`クラス。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="2dd9e-215">依存関係の競合回避モジュールに渡す、 **MapSignalR**ハブ構成を追加して、メソッド。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="2dd9e-216">新しいパラメーターを使用して、サンプルの Startup クラスで Startup.ConfigureSignalR メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="2dd9e-217">SignalR がで指定された競合回避モジュールを使用して**MapSignalR**既定のリゾルバーではなく。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="2dd9e-218">完全なコードを次に示します`Startup.Configuration`します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="2dd9e-219">Visual Studio で、StockTicker アプリケーションを実行するには、f5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="2dd9e-220">ブラウザーのウィンドウでに移動します。`http://localhost:*port*/SignalR.Sample/StockTicker.html`します。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="2dd9e-221">アプリケーションには、正確に前に、のと同じ機能があります。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="2dd9e-222">(詳細については、次を参照してください[ASP.NET SignalR によるサーバー ブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md)。)。動作を変更していません。テスト、保守、および進化を簡単にコードを単になりました。</span><span class="sxs-lookup"><span data-stu-id="2dd9e-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
