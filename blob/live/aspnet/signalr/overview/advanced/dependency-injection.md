---
uid: signalr/overview/advanced/dependency-injection
title: "SignalR の依存関係の挿入 |Microsoft ドキュメント"
author: MikeWasson
description: "ここ Visual Studio 2013 .NET 4.5 SignalR で使用するソフトウェアのバージョンはの以前のバージョンについてはこのトピックの以前のバージョンをバージョン 2."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="82f56-103">SignalR の依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="82f56-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="82f56-104">によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="82f56-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="82f56-105">このトピックで使用されているソフトウェア バージョン</span><span class="sxs-lookup"><span data-stu-id="82f56-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="82f56-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="82f56-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="82f56-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="82f56-107">.NET 4.5</span></span>
> - <span data-ttu-id="82f56-108">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="82f56-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="82f56-109">このトピックの以前のバージョン</span><span class="sxs-lookup"><span data-stu-id="82f56-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="82f56-110">SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="82f56-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="82f56-111">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="82f56-111">Questions and comments</span></span>
> 
> <span data-ttu-id="82f56-112">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="82f56-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="82f56-113">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="82f56-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="82f56-114">依存関係の挿入は、簡単にテストに (モック オブジェクトを使用) するか、オブジェクトの依存関係を置き換える場合、または実行時の動作を変更する、オブジェクト間の依存関係をハード コーディングされたを削除する方法です。</span><span class="sxs-lookup"><span data-stu-id="82f56-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="82f56-115">このチュートリアルでは、SignalR ハブで依存関係の挿入を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="82f56-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="82f56-116">SignalR で IoC コンテナーを使用する方法も示しています。</span><span class="sxs-lookup"><span data-stu-id="82f56-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="82f56-117">IoC コンテナーは、依存関係の挿入の一般的なフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="82f56-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="82f56-118">依存関係の挿入とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="82f56-118">What is Dependency Injection?</span></span>

<span data-ttu-id="82f56-119">依存関係の挿入について熟知している場合は、このセクションをスキップします。</span><span class="sxs-lookup"><span data-stu-id="82f56-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="82f56-120">*依存関係の挿入*(DI) は、パターン、オブジェクトを独自の依存関係を作成できないです。</span><span class="sxs-lookup"><span data-stu-id="82f56-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="82f56-121">DI する動機に単純な例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="82f56-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="82f56-122">メッセージを記録する必要があるオブジェクトがあるとします。</span><span class="sxs-lookup"><span data-stu-id="82f56-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="82f56-123">ログ記録のインターフェイスを定義する場合があります。</span><span class="sxs-lookup"><span data-stu-id="82f56-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="82f56-124">オブジェクトを作成できます、`ILogger`メッセージ ログに記録します。</span><span class="sxs-lookup"><span data-stu-id="82f56-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="82f56-125">これは、動作しますが、最適なデザインではありません。</span><span class="sxs-lookup"><span data-stu-id="82f56-125">This works, but it's not the best design.</span></span> <span data-ttu-id="82f56-126">置換する場合`FileLogger`別`ILogger`実装では、変更する必要がある`SomeComponent`です。</span><span class="sxs-lookup"><span data-stu-id="82f56-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="82f56-127">多くの他のオブジェクトによって使用されることと`FileLogger`、それらのすべてを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="82f56-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="82f56-128">作成する場合または`FileLogger`シングルトンもする必要があります、アプリケーション全体での変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="82f56-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="82f56-129">適切な方法が「注入」するには、`ILogger`オブジェクトに — たとえば、コンス トラクターの引数を使用しています。</span><span class="sxs-lookup"><span data-stu-id="82f56-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="82f56-130">これで、オブジェクトはこれを選択するために責任を負わず`ILogger`を使用します。</span><span class="sxs-lookup"><span data-stu-id="82f56-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="82f56-131">スイッチを作成することができます`ILogger`それに依存するオブジェクトを変更することがなく実装します。</span><span class="sxs-lookup"><span data-stu-id="82f56-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="82f56-132">このパターンが呼び出された[コンス トラクター インジェクション](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)です。</span><span class="sxs-lookup"><span data-stu-id="82f56-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="82f56-133">別のパターンには、setter メソッドまたはプロパティを依存関係を設定する set アクセス操作子インジェクションです。</span><span class="sxs-lookup"><span data-stu-id="82f56-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="82f56-134">SignalR で単純な依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="82f56-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="82f56-135">チュートリアルのチャット アプリケーションを考えます[SignalR の概要](../getting-started/tutorial-getting-started-with-signalr.md)です。</span><span class="sxs-lookup"><span data-stu-id="82f56-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="82f56-136">そのアプリケーションからハブ クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="82f56-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="82f56-137">サーバー上に送信する前にチャット メッセージを保存するとします。</span><span class="sxs-lookup"><span data-stu-id="82f56-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="82f56-138">この機能を抽象化するインターフェイスを定義してにインターフェイスを挿入する DI を使用する場合があります、`ChatHub`クラスです。</span><span class="sxs-lookup"><span data-stu-id="82f56-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="82f56-139">唯一の問題は、SignalR アプリケーションがハブ; を直接作成していないことSignalR には、それらが作成されます。</span><span class="sxs-lookup"><span data-stu-id="82f56-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="82f56-140">既定では、SignalR には、パラメーターなしのコンス トラクターを持つハブ クラスが必要です。</span><span class="sxs-lookup"><span data-stu-id="82f56-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="82f56-141">ただし、簡単にハブ インスタンスを作成する関数を登録して DI を実行するこの関数を使用します。</span><span class="sxs-lookup"><span data-stu-id="82f56-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="82f56-142">関数を呼び出すことによって登録**GlobalHost.DependencyResolver.Register**です。</span><span class="sxs-lookup"><span data-stu-id="82f56-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="82f56-143">SignalR は、作成する必要があるたびに、この匿名の関数が呼び出すので、`ChatHub`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="82f56-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="82f56-144">IoC コンテナー</span><span class="sxs-lookup"><span data-stu-id="82f56-144">IoC Containers</span></span>

<span data-ttu-id="82f56-145">上記のコードは、単純な場合に適しています。</span><span class="sxs-lookup"><span data-stu-id="82f56-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="82f56-146">これを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="82f56-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="82f56-147">多くの依存関係を持つ複雑なアプリケーションでは、多くの「配線」このコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="82f56-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="82f56-148">依存関係が入れ子になった場合に特に、このコードは、維持するために難しくなります。</span><span class="sxs-lookup"><span data-stu-id="82f56-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="82f56-149">また、単体テストに困難です。</span><span class="sxs-lookup"><span data-stu-id="82f56-149">It is also hard to unit test.</span></span>

<span data-ttu-id="82f56-150">1 つのソリューションでは、IoC コンテナーを使用します。</span><span class="sxs-lookup"><span data-stu-id="82f56-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="82f56-151">IoC コンテナーは、依存関係の管理を担当するソフトウェア コンポーネントです。型のコンテナーを登録し、コンテナーを使用して、オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="82f56-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="82f56-152">コンテナーは、依存関係を自動的に判別されます。</span><span class="sxs-lookup"><span data-stu-id="82f56-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="82f56-153">多くの IoC コンテナーを使用すると、オブジェクトの有効期間、スコープなどを制御できます。</span><span class="sxs-lookup"><span data-stu-id="82f56-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="82f56-154">"IoC"が「コントロールの反転」の略フレームワークがアプリケーション コードを呼び出す、一般的なパターンはします。</span><span class="sxs-lookup"><span data-stu-id="82f56-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="82f56-155">IoC コンテナーは、通常の制御フローの「反転」に、オブジェクトに構築します。</span><span class="sxs-lookup"><span data-stu-id="82f56-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="82f56-156">SignalR で IoC コンテナーの使用</span><span class="sxs-lookup"><span data-stu-id="82f56-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="82f56-157">チャット アプリケーションは、IoC コンテナーから利点を得るが簡単すぎます可能性があります。</span><span class="sxs-lookup"><span data-stu-id="82f56-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="82f56-158">代わりに、見てみましょう、 [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample)サンプルです。</span><span class="sxs-lookup"><span data-stu-id="82f56-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="82f56-159">StockTicker サンプルでは、次の 2 つの主要なクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="82f56-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="82f56-160">`StockTickerHub`:、ハブ クラスのクライアント接続を管理します。</span><span class="sxs-lookup"><span data-stu-id="82f56-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="82f56-161">`StockTicker`: 株価を保持し、定期的に更新するシングルトンです。</span><span class="sxs-lookup"><span data-stu-id="82f56-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="82f56-162">`StockTickerHub`参照を保持している、`StockTicker`シングルトン、中に`StockTicker`への参照を保持している、 **IHubConnectionContext**の`StockTickerHub`です。</span><span class="sxs-lookup"><span data-stu-id="82f56-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="82f56-163">このインターフェイスを使用して通信するために`StockTickerHub`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="82f56-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="82f56-164">(詳細については、次を参照してください[サーバーは、ASP.NET SignalR でブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md)。)。</span><span class="sxs-lookup"><span data-stu-id="82f56-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="82f56-165">IoC コンテナー ビットこれらの依存関係の解決に使用できます。</span><span class="sxs-lookup"><span data-stu-id="82f56-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="82f56-166">最初に、簡略化してみましょう、`StockTickerHub`と`StockTicker`クラスです。</span><span class="sxs-lookup"><span data-stu-id="82f56-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="82f56-167">次のコードでをコメント アウトしました部分しない必要があります。</span><span class="sxs-lookup"><span data-stu-id="82f56-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="82f56-168">パラメーターなしのコンス トラクターからの削除`StockTickerHub`です。</span><span class="sxs-lookup"><span data-stu-id="82f56-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="82f56-169">代わりに、ハブを作成するのに DI を常に使用します。</span><span class="sxs-lookup"><span data-stu-id="82f56-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="82f56-170">StockTicker、シングルトン インスタンスを削除します。</span><span class="sxs-lookup"><span data-stu-id="82f56-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="82f56-171">その後、StockTicker 有効期間を制御するのに IoC コンテナーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="82f56-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="82f56-172">パブリック コンス トラクターは、また、ください。</span><span class="sxs-lookup"><span data-stu-id="82f56-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="82f56-173">次に、インターフェイスを作成することで、コードをリファクタリングできます`StockTicker`です。</span><span class="sxs-lookup"><span data-stu-id="82f56-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="82f56-174">このインターフェイスを使用して、分離、`StockTickerHub`から、`StockTicker`クラスです。</span><span class="sxs-lookup"><span data-stu-id="82f56-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="82f56-175">Visual Studio でこの種類のリファクタリング簡単です。</span><span class="sxs-lookup"><span data-stu-id="82f56-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="82f56-176">StockTicker.cs ファイルを開きを右クリックし、`StockTicker`クラス宣言、および選択**リファクター**しています.**インターフェイスの抽出**です。</span><span class="sxs-lookup"><span data-stu-id="82f56-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="82f56-177">**インターフェイスの抽出**ダイアログ ボックスで、をクリックして**すべて選択**です。</span><span class="sxs-lookup"><span data-stu-id="82f56-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="82f56-178">他の既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="82f56-178">Leave the other defaults.</span></span> <span data-ttu-id="82f56-179">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82f56-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="82f56-180">Visual Studio という名前の新しいインターフェイスを作成する`IStockTicker`、変更のほか`StockTicker`から派生する`IStockTicker`です。</span><span class="sxs-lookup"><span data-stu-id="82f56-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="82f56-181">IStockTicker.cs ファイルを開き、変更するためのインターフェイス**パブリック**です。</span><span class="sxs-lookup"><span data-stu-id="82f56-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="82f56-182">`StockTickerHub`クラスを 2 つのインスタンスを変更、`StockTicker`に`IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="82f56-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="82f56-183">作成する、`IStockTicker`インターフェイスは必ずしも必要はありませんが、アプリケーションのコンポーネント間の結合を削減する DI どう役立つかを表示する必要です。</span><span class="sxs-lookup"><span data-stu-id="82f56-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="82f56-184">Ninject ライブラリを追加します。</span><span class="sxs-lookup"><span data-stu-id="82f56-184">Add the Ninject Library</span></span>

<span data-ttu-id="82f56-185">.NET の多くのオープン ソース IoC コンテナーがあります。</span><span class="sxs-lookup"><span data-stu-id="82f56-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="82f56-186">このチュートリアルを使用します[Ninject](http://www.ninject.org/)です。</span><span class="sxs-lookup"><span data-stu-id="82f56-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="82f56-187">(その他の一般的なライブラリを含む[城 Windsor](http://www.castleproject.org/)、 [Spring.Net](http://www.springframework.net/)、 [Autofac](https://code.google.com/p/autofac/)、 [Unity](https://github.com/unitycontainer/unity)、および[StructureMap](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="82f56-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="82f56-188">NuGet パッケージ マネージャーを使用して、インストール、 [Ninject ライブラリ](https://nuget.org/packages/Ninject/3.0.1.10)です。</span><span class="sxs-lookup"><span data-stu-id="82f56-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="82f56-189">Visual Studio から、**ツール**メニュー選択**ライブラリ パッケージ マネージャー** | **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="82f56-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="82f56-190">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="82f56-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="82f56-191">SignalR の依存関係競合回避モジュールを交換します。</span><span class="sxs-lookup"><span data-stu-id="82f56-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="82f56-192">SignalR 内 Ninject を使用するから派生するクラスを作成**DefaultDependencyResolver**です。</span><span class="sxs-lookup"><span data-stu-id="82f56-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="82f56-193">このクラスは、オーバーライド、 **GetService**と**GetServices**のメソッド**DefaultDependencyResolver**です。</span><span class="sxs-lookup"><span data-stu-id="82f56-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="82f56-194">SignalR ハブ インスタンスの SignalR によって内部的に使用されるさまざまなサービスなど、実行時にさまざまなオブジェクトを作成するこれらのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="82f56-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="82f56-195">**GetService**メソッドは、型の 1 つのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="82f56-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="82f56-196">呼び出す Ninject カーネルのこのメソッドをオーバーライド**TryGet**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="82f56-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="82f56-197">そのメソッドが null を返す場合は、既定のリゾルバーにフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="82f56-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="82f56-198">**GetServices**メソッドは、指定した型のオブジェクトのコレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="82f56-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="82f56-199">既定の競合回避モジュールからの結果に Ninject から結果を連結するには、このメソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="82f56-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="82f56-200">Ninject バインディングを構成します。</span><span class="sxs-lookup"><span data-stu-id="82f56-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="82f56-201">宣言型バインディングに Ninject を使用します。</span><span class="sxs-lookup"><span data-stu-id="82f56-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="82f56-202">アプリケーションの Startup.cs クラスを開きます (いるいずれかの手動で作成したパッケージの指示に従って`readme.txt`認証をプロジェクトに追加することによって作成されたか)。</span><span class="sxs-lookup"><span data-stu-id="82f56-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="82f56-203">`Startup.Configuration`メソッド、Ninject 呼び出す Ninject コンテナーを作成、*カーネル*です。</span><span class="sxs-lookup"><span data-stu-id="82f56-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="82f56-204">このカスタム依存関係競合回避モジュールのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="82f56-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="82f56-205">バインディングを作成する`IStockTicker`次のようにします。</span><span class="sxs-lookup"><span data-stu-id="82f56-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="82f56-206">このコードは、次の 2 つというメッセージがします。</span><span class="sxs-lookup"><span data-stu-id="82f56-206">This code is saying two things.</span></span> <span data-ttu-id="82f56-207">最初に、アプリケーションに必要なときに、 `IStockTicker`、カーネルがのインスタンスを作成する必要があります`StockTicker`です。</span><span class="sxs-lookup"><span data-stu-id="82f56-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="82f56-208">2 番目、`StockTicker`クラスは、シングルトン オブジェクトとして作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="82f56-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="82f56-209">Ninject は、オブジェクトの 1 つのインスタンスを作成し、各要求に対して同じインスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="82f56-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="82f56-210">バインディングを作成する**IHubConnectionContext**次のようにします。</span><span class="sxs-lookup"><span data-stu-id="82f56-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="82f56-211">このコード creatres を返す匿名関数、 **IHubConnection**です。</span><span class="sxs-lookup"><span data-stu-id="82f56-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="82f56-212">**WhenInjectedInto**メソッドに指示を作成するときにのみ、この関数を使用する Ninject`IStockTicker`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="82f56-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="82f56-213">その理由は、SignalR が作成される**IHubConnectionContext**インスタンスを内部的には、SignalR がそれらを作成する方法をオーバーライドする必要はないです。</span><span class="sxs-lookup"><span data-stu-id="82f56-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="82f56-214">この関数にのみ適用されます、`StockTicker`クラスです。</span><span class="sxs-lookup"><span data-stu-id="82f56-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="82f56-215">依存関係リゾルバーを渡す、 **MapSignalR**ハブの構成を追加して、メソッド。</span><span class="sxs-lookup"><span data-stu-id="82f56-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="82f56-216">新しいパラメーターを使用して、サンプルのスタートアップ クラスで Startup.ConfigureSignalR メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="82f56-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="82f56-217">SignalR で指定された競合回避モジュールを使用するようになりました**MapSignalR**既定の競合回避モジュールの代わりにします。</span><span class="sxs-lookup"><span data-stu-id="82f56-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="82f56-218">完全なコードの一覧を次に示します`Startup.Configuration`です。</span><span class="sxs-lookup"><span data-stu-id="82f56-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="82f56-219">Visual Studio で StockTicker アプリケーションを実行するには、f5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="82f56-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="82f56-220">ブラウザー ウィンドウに移動`http://localhost:*port*/SignalR.Sample/StockTicker.html`です。</span><span class="sxs-lookup"><span data-stu-id="82f56-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="82f56-221">アプリケーションには、正確に前に、のと同じ機能があります。</span><span class="sxs-lookup"><span data-stu-id="82f56-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="82f56-222">(詳細については、次を参照してください[サーバーは、ASP.NET SignalR でブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md)。)。動作を変更していません。行ったコードのテスト、保守、および発展しやすくします。</span><span class="sxs-lookup"><span data-stu-id="82f56-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
