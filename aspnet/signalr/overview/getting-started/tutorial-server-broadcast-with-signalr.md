---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'チュートリアル: SignalR 2 によるサーバー ブロードキャスト |Microsoft Docs'
author: tdykstra
description: このチュートリアルでは、ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。
ms.author: riande
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a6014e604613492db91b2dc6f846c3c73d938d99
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099300"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="7caf6-103">チュートリアル: SignalR 2 によるブロードキャスト サーバー</span><span class="sxs-lookup"><span data-stu-id="7caf6-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="7caf6-104">このチュートリアルでは、ASP.NET SignalR 2 を使用してサーバー ブロードキャストの機能を提供する web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="7caf6-105">サーバー ブロードキャストでは、サーバーがクライアントに送信される通信を開始することを意味します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="7caf6-106">このチュートリアルで作成するアプリケーションでは、株価情報、サーバー ブロードキャストの機能の一般的なシナリオをシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="7caf6-107">定期的に、サーバーはランダムに株価を更新し、接続されているすべてのクライアントに、更新プログラムをブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="7caf6-108">ブラウザー、数字、およびシンボルで、**変更**と**%** 通知をサーバーからの応答で列を動的に変更します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="7caf6-109">同じ URL に他のブラウザーを開いた場合、同じデータやデータに同じ変更を同時にすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Web を作成します。](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="7caf6-111">このチュートリアルでしました。</span><span class="sxs-lookup"><span data-stu-id="7caf6-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7caf6-112">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="7caf6-112">Create the project</span></span>
> * <span data-ttu-id="7caf6-113">サーバー コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-113">Set up the server code</span></span>
> * <span data-ttu-id="7caf6-114">サーバー コードを調べる</span><span class="sxs-lookup"><span data-stu-id="7caf6-114">Examine the server code</span></span>
> * <span data-ttu-id="7caf6-115">クライアント コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-115">Set up the client code</span></span>
> * <span data-ttu-id="7caf6-116">クライアント コードを調べる</span><span class="sxs-lookup"><span data-stu-id="7caf6-116">Examine the client code</span></span>
> * <span data-ttu-id="7caf6-117">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="7caf6-117">Test the application</span></span>
> * <span data-ttu-id="7caf6-118">ログの有効化</span><span class="sxs-lookup"><span data-stu-id="7caf6-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7caf6-119">アプリケーションの構築の手順を実行しない場合は、新しい空の ASP.NET Web アプリケーション プロジェクトで SignalR.Sample パッケージをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="7caf6-120">このチュートリアルの手順を実行せず、NuGet パッケージをインストールした場合の手順に従ってください必要があります、 *readme.txt*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="7caf6-121">OWIN startup を追加する必要があるパッケージを実行するには、クラスの呼び出し、`ConfigureSignalR`インストールされたパッケージ内のメソッド。</span><span class="sxs-lookup"><span data-stu-id="7caf6-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="7caf6-122">OWIN startup クラスを追加しない場合、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="7caf6-123">参照してください、 [StockTicker のサンプルをインストール](#install-the-stockticker-sample)この記事の「します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7caf6-124">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="7caf6-124">Prerequisites</span></span>

 * <span data-ttu-id="7caf6-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)で、 **ASP.NET および web 開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="7caf6-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="7caf6-126">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="7caf6-126">Create the project</span></span>

<span data-ttu-id="7caf6-127">このセクションでは、Visual Studio 2017 を使用して、空の ASP.NET Web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="7caf6-128">Visual Studio で ASP.NET Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web を作成します。](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="7caf6-130">**新しい ASP.NET Web アプリケーション - SignalR.StockTicker**  ウィンドウのままに**空**を選択し、選択**OK**。</span><span class="sxs-lookup"><span data-stu-id="7caf6-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="7caf6-131">サーバー コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-131">Set up the server code</span></span>

<span data-ttu-id="7caf6-132">このセクションでは、サーバーで実行されるコードを設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="7caf6-133">在庫クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-133">Create the Stock class</span></span>

<span data-ttu-id="7caf6-134">まずを作成、 *Stock*モデル クラスを格納および転送については、在庫を使用します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="7caf6-135">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **クラス**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="7caf6-136">クラスの名前*Stock*し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="7caf6-137">コードに置き換えます、 *Stock.cs*このコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="7caf6-138">素材を作成するときに設定する 2 つのプロパティは`Symbol`(たとえば、Microsoft の MSFT) と`Price`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="7caf6-139">その他のプロパティが設定する方法とタイミングに依存`Price`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="7caf6-140">初めて設定する`Price`に、値が伝達される`DayOpen`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="7caf6-141">その後、設定すると`Price`、アプリを計算、`Change`と`PercentChange`プロパティ値の差に基づいて`Price`と`DayOpen`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="7caf6-142">StockTickerHub と StockTicker クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="7caf6-143">サーバーからクライアントへの対話を処理するために、SignalR Hub API を使用します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="7caf6-144">A`StockTickerHub`から派生したクラス、`SignalRHub`クラスはクライアントからの受信接続とメソッドの呼び出しを処理します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-144">A `StockTickerHub` class that derives from the `SignalRHub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="7caf6-145">株価データを管理および実行する必要も、`Timer`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="7caf6-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="7caf6-146">`Timer`オブジェクトが価格の更新プログラムのクライアント接続の独立したを定期的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="7caf6-147">これらの関数を配置することはできません、`Hub`クラス、ハブには、一時的なためです。</span><span class="sxs-lookup"><span data-stu-id="7caf6-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="7caf6-148">アプリを作成、`Hub`接続と、クライアントからサーバーへの呼び出しのように、ハブの各タスクのクラスのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="7caf6-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="7caf6-149">株価データを保持し、価格を更新、価格の更新プログラムにブロードキャスト メカニズムでは、別のクラスで実行します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="7caf6-150">クラスは、名前を付けます`StockTicker`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-150">You'll name the class `StockTicker`.</span></span>

![StockTicker からブロードキャスト](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="7caf6-152">1 つのインスタンスが欲しい、`StockTicker`それぞれからの参照を設定する必要がありますので、サーバー上で実行するにはクラス`StockTickerHub`シングルトン インスタンス`StockTicker`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="7caf6-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="7caf6-153">`StockTicker`クラスは、株価データと、更新をトリガーすることから、クライアントにブロードキャストする必要がありますが、`StockTicker`されていない、`Hub`クラス。</span><span class="sxs-lookup"><span data-stu-id="7caf6-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="7caf6-154">`StockTicker`クラスは、SignalR ハブの接続コンテキスト オブジェクトへの参照を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="7caf6-155">クライアントにブロードキャストする SignalR 接続のコンテキスト オブジェクトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="7caf6-156">StockTickerHub.cs を作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="7caf6-157">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7caf6-158">**新しい項目の追加 - SignalR.StockTicker**、**インストール済み** > **Visual C#**   >  **Web** >  **SignalR**選び**SignalR ハブ クラス (v2)** します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="7caf6-159">クラスの名前*StockTickerHub*し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="7caf6-160">この手順で作成、 *StockTickerHub.cs*クラス ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="7caf6-161">同時に、プロジェクトに SignalR をサポートする一連のスクリプト ファイルとアセンブリ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="7caf6-162">コードに置き換えます、 *StockTickerHub.cs*このコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="7caf6-163">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-163">Save the file.</span></span>

<span data-ttu-id="7caf6-164">アプリでは、[ハブ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)メソッドは、クライアントは、サーバー上で呼び出すことができますを定義するクラス。</span><span class="sxs-lookup"><span data-stu-id="7caf6-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="7caf6-165">1 つのメソッドを定義する:`GetAllStocks()`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="7caf6-166">クライアントは、最初に、サーバーに接続するときは、その現在の価格の株式のすべての一覧を取得するには、このメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="7caf6-167">メソッドは同期的に実行して、返す`IEnumerable<Stock>`メモリからデータを返すためです。</span><span class="sxs-lookup"><span data-stu-id="7caf6-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="7caf6-168">指定したかどうか、メソッドがデータベース検索など、web サービス呼び出しの待機を含むは何かの手順を実行してデータを取得する必要がある`Task<IEnumerable<Stock>>`非同期処理を有効にする戻り値として。</span><span class="sxs-lookup"><span data-stu-id="7caf6-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="7caf6-169">詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーの非同期的に実行するタイミング](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods)します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="7caf6-170">`HubName`属性は、アプリが、クライアントでの JavaScript コードのハブを参照する方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="7caf6-171">クライアントに既定の名前は、この属性を使用しない場合は、例ではこのクラスの名前のキャメル ケース バージョン`stockTickerHub`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="7caf6-172">後述するように作成するとき、`StockTicker`クラス、アプリは静的でそのクラスのシングルトン インスタンスを作成`Instance`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="7caf6-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="7caf6-173">シングルトン インスタンスが`StockTicker`はクライアントの数が接続または切断に関係なく、メモリ内にします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="7caf6-174">そのインスタンスがどのような`GetAllStocks()`メソッドを使用して現在の株価情報を返します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="7caf6-175">StockTicker.cs を作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="7caf6-176">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **クラス**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="7caf6-177">クラスの名前*StockTicker*し、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="7caf6-178">コードに置き換えます、 *StockTicker.cs*このコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="7caf6-179">すべてのスレッドが StockTicker コードの同じインスタンスを実行するため StockTicker クラスはスレッド セーフであることにあります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="7caf6-180">サーバー コードを調べる</span><span class="sxs-lookup"><span data-stu-id="7caf6-180">Examine the server code</span></span>

<span data-ttu-id="7caf6-181">サーバー コードを確認する場合に役立つアプリの動作を理解します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="7caf6-182">静的フィールドに、シングルトン インスタンスを格納します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="7caf6-183">静的な初期化`_instance`をバックするフィールド、`Instance`クラスのインスタンスのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="7caf6-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="7caf6-184">コンス トラクターはプライベートなので、アプリを作成できるクラスの唯一のインスタンスになります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="7caf6-185">アプリは[遅延初期化](/dotnet/framework/performance/lazy-initialization)の`_instance`フィールド。</span><span class="sxs-lookup"><span data-stu-id="7caf6-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="7caf6-186">パフォーマンス上の理由はありません。</span><span class="sxs-lookup"><span data-stu-id="7caf6-186">It's not for performance reasons.</span></span> <span data-ttu-id="7caf6-187">インスタンスの作成はスレッド セーフであるかどうかを確認するになります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="7caf6-188">クライアントがサーバーに接続するたびに個別のスレッドで実行される StockTickerHub クラスの新しいインスタンスがから StockTicker のシングルトン インスタンスを取得する、`StockTicker.Instance`の静的プロパティを見た前に、`StockTickerHub`クラス。</span><span class="sxs-lookup"><span data-stu-id="7caf6-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="7caf6-189">ConcurrentDictionary の株価データを格納します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="7caf6-190">コンス トラクターによって初期化、`_stocks`をサンプルの株価データ コレクションと`GetAllStocks`株式を返します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="7caf6-191">既に説明したよう株式のこのコレクションがによって返される`StockTickerHub.GetAllStocks`、メソッドは、サーバーで、`Hub`クライアントが呼び出すことができるクラス。</span><span class="sxs-lookup"><span data-stu-id="7caf6-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="7caf6-192">Stocks コレクションとは見なさ、 [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx)スレッド セーフの型。</span><span class="sxs-lookup"><span data-stu-id="7caf6-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="7caf6-193">別の方法として使用できます、[ディクショナリ](https://msdn.microsoft.com/library/xfhwa508.aspx)オブジェクトし、それを変更するときに明示的にディクショナリをロックします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="7caf6-194">このサンプル アプリケーションは [ok] メモリ内でアプリケーション データを保存して、アプリを破棄しますと、データが失われる、`StockTicker`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="7caf6-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="7caf6-195">実際のアプリケーションでは、データベースのようなバックエンド データ ストアと動作します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="7caf6-196">株価を定期的に更新</span><span class="sxs-lookup"><span data-stu-id="7caf6-196">Periodically updating stock prices</span></span>

<span data-ttu-id="7caf6-197">コンス トラクターは、の起動時、`Timer`を定期的にランダムな単位で株価を更新するメソッドを呼び出すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="7caf6-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="7caf6-198">`Timer` 呼び出し`UpdateStockPrices`、state パラメーターで null を渡します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="7caf6-199">価格を更新する前に、アプリは、ロック取得、`_updateStockPricesLock`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="7caf6-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="7caf6-200">別のスレッドが価格を更新中で既にかどうかと、続いて、コードを確認します`TryUpdateStockPrice`の一覧で、各株にします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="7caf6-201">`TryUpdateStockPrice`メソッドは、株価を変更するかどうかを決定し、これを変更する量。</span><span class="sxs-lookup"><span data-stu-id="7caf6-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="7caf6-202">株式の価格が変更された場合、アプリが呼び出す`BroadcastStockPrice`接続されているクライアントをすべてに株価の変更をブロードキャストします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="7caf6-203">`_updatingStockPrices`フラグが指定されている[揮発性](https://msdn.microsoft.com/library/x13ttww7.aspx)はスレッド セーフであるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="7caf6-204">実際のアプリケーションで、`TryUpdateStockPrice`メソッドは、価格を検索する web サービスを呼び出すとします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="7caf6-205">このコードでは、アプリは、ランダムに変更するのに乱数ジェネレーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="7caf6-206">StockTicker クラスは、クライアントにブロードキャストできるように、SignalR のコンテキストを取得します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="7caf6-207">料金の変更がここで送信されるため、`StockTicker`オブジェクトを呼び出す必要があるオブジェクトは、`updateStockPrice`接続されているすべてのクライアントのメソッド。</span><span class="sxs-lookup"><span data-stu-id="7caf6-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="7caf6-208">`Hub`クラス、クライアントのメソッドを呼び出すための API は用意したが、`StockTicker`から派生していない、`Hub`クラスし、への参照がありません。`Hub`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="7caf6-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="7caf6-209">接続されているクライアントは、ブロードキャスト、`StockTicker`クラスの SignalR コンテキスト インスタンスを取得するには、`StockTickerHub`クラスし、クライアントでメソッドの呼び出しに使用します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="7caf6-210">コードが、コンス トラクターに渡すを参照する、シングルトン クラス インスタンスを作成するときに、SignalR コンテキストへの参照を取得し、コンス トラクター内に配置、`Clients`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="7caf6-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="7caf6-211">理由は 2 つのコンテキストを 1 回だけ取得する理由: コンテキストの取得は、高価なタスクでありを指定すると、アプリで、クライアントに送信されるメッセージの目的の順序を維持すると、それを取得します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="7caf6-212">取得、`Clients`プロパティのコンテキストと基本的には、`StockTickerClient`プロパティでは、クライアント メソッドを呼び出すコードの場合と同様、同じ外観を記述することができます、`Hub`クラス。</span><span class="sxs-lookup"><span data-stu-id="7caf6-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="7caf6-213">たとえば、すべてのクライアントにブロードキャストすることが書き込み`Clients.All.updateStockPrice(stock)`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="7caf6-214">`updateStockPrice`で呼び出しているメソッド`BroadcastStockPrice`がまだ存在しません。</span><span class="sxs-lookup"><span data-stu-id="7caf6-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="7caf6-215">後で追加するクライアントで実行されるコードを記述するときにします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="7caf6-216">参照することができます`updateStockPrice`ここため`Clients.All`は動的で、アプリが実行時に式を評価することを意味します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="7caf6-217">このメソッドの呼び出しが実行されると、SignalR は送信メソッド名とパラメーターの値をクライアントにクライアントがという名前のメソッドと`updateStockPrice`アプリはそのメソッドを呼び出すし、パラメーターの値を渡します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="7caf6-218">`Clients.All` 意味は、すべてのクライアントに送信します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="7caf6-219">SignalR では、その他のオプションのクライアントまたはクライアントに送信するグループを指定できます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="7caf6-220">詳細については、次を参照してください。 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="7caf6-221">SignalR のルートを登録します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-221">Register the SignalR route</span></span>

<span data-ttu-id="7caf6-222">サーバーは、途中受信および SignalR への直接する URL を把握する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="7caf6-223">そのためには、OWIN startup クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="7caf6-224">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7caf6-225">**新しい項目の追加 - SignalR.StockTicker**選択**インストール済み** > **Visual C#**   >  **Web**と選び**OWIN Startup クラス**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="7caf6-226">クラスの名前*スタートアップ*選択と**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="7caf6-227">既定のコードを置き換える、 *Startup.cs*このコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="7caf6-228">サーバー コードの設定を今すぐが完了しました。</span><span class="sxs-lookup"><span data-stu-id="7caf6-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="7caf6-229">次のセクションでは、クライアントを設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="7caf6-230">クライアント コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-230">Set up the client code</span></span>

<span data-ttu-id="7caf6-231">このセクションでは、クライアントで実行されるコードを設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="7caf6-232">ページの HTML と JavaScript ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="7caf6-233">データを HTML ページが表示され、JavaScript ファイルは、データを整理します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="7caf6-234">StockTicker.html を作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-234">Create StockTicker.html</span></span>

<span data-ttu-id="7caf6-235">まず、HTML クライアントを追加します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="7caf6-236">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **HTML ページ**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="7caf6-237">ファイルに名前を*StockTicker*選択と**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="7caf6-238">既定のコードを置き換える、 *StockTicker.html*このコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="7caf6-239">HTML では、5 つの列、ヘッダー行では、5 つの列にまたがる 1 つのセルを持つデータ行とテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="7caf6-240">「読み込み中...」アプリの起動時に一時的にデータ行を示しています。</span><span class="sxs-lookup"><span data-stu-id="7caf6-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="7caf6-241">JavaScript コードはその行を削除して、サーバーから取得した株価データでその場所の行に追加します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="7caf6-242">スクリプト タグを指定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-242">The script tags specify:</span></span>

    * <span data-ttu-id="7caf6-243">JQuery スクリプト ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-243">The jQuery script file.</span></span>

    * <span data-ttu-id="7caf6-244">SignalR コア スクリプト ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="7caf6-245">SignalR プロキシ スクリプト ファイルです。</span><span class="sxs-lookup"><span data-stu-id="7caf6-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="7caf6-246">後で作成する StockTicker スクリプト ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="7caf6-247">アプリは、SignalR プロキシ スクリプト ファイルを動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="7caf6-248">「/Signalr ハブ」の URL を指定し、Hub クラスは、ここでは、上のメソッドをプロキシのメソッドを定義します。、`StockTickerHub.GetAllStocks`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="7caf6-249">使用してこの JavaScript ファイルが手動で生成できる場合は、 [SignalR ユーティリティ](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="7caf6-250">動的ファイルの作成を無効にすることを忘れないでください、`MapHubs`メソッドの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="7caf6-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="7caf6-251">**ソリューション エクスプ ローラー**、展開**スクリプト**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="7caf6-252">JQuery と SignalR 用のスクリプト ライブラリは、プロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7caf6-253">パッケージ マネージャーは、以降のバージョンの SignalR スクリプトでインストールされます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="7caf6-254">プロジェクト内のスクリプト ファイルのバージョンに対応するコード ブロック内のスクリプト参照を更新します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="7caf6-255">**ソリューション エクスプ ローラー**、右クリックして*StockTicker.html*、し、**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="7caf6-256">StockTicker.js を作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-256">Create StockTicker.js</span></span>

<span data-ttu-id="7caf6-257">JavaScript ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="7caf6-258">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **JavaScript ファイル**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="7caf6-259">ファイルに名前を*StockTicker*選択と**OK**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="7caf6-260">このコードを追加、 *StockTicker.js*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7caf6-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="7caf6-261">クライアント コードを調べる</span><span class="sxs-lookup"><span data-stu-id="7caf6-261">Examine the client code</span></span>

<span data-ttu-id="7caf6-262">クライアント コードを確認する場合に役立つクライアント コードがアプリケーションを動作させるサーバー コードと対話する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="7caf6-263">接続の開始</span><span class="sxs-lookup"><span data-stu-id="7caf6-263">Starting the connection</span></span>

<span data-ttu-id="7caf6-264">`$.connection` SignalR プロキシを参照します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="7caf6-265">コードのプロキシへの参照を取得する、`StockTickerHub`クラスし、内に配置、`ticker`変数。</span><span class="sxs-lookup"><span data-stu-id="7caf6-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="7caf6-266">プロキシの名前が名前によって設定された、`HubName`属性。</span><span class="sxs-lookup"><span data-stu-id="7caf6-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="7caf6-267">SignalR を呼び出すことによって、ファイル内のコードの最後の行が SignalR 接続を初期化しますすべての変数と関数を定義した後`start`関数。</span><span class="sxs-lookup"><span data-stu-id="7caf6-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="7caf6-268">`start`関数が非同期的に実行し、返します、 [jQuery 遅延オブジェクト](http://api.jquery.com/category/deferred-object/)します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="7caf6-269">アプリケーションが非同期のアクションが完了したときに呼び出す関数を指定する元に戻す関数を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="7caf6-270">すべての素材を取得します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-270">Getting all the stocks</span></span>

<span data-ttu-id="7caf6-271">`init`関数呼び出し、`getAllStocks`サーバー上の機能を在庫テーブルを更新するサーバーが返す情報を使用しています。</span><span class="sxs-lookup"><span data-stu-id="7caf6-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="7caf6-272">、既定では、あるメソッド名は pascal 形式で表記するサーバーの場合でも、クライアントにキャメル ケースを使用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="7caf6-273">キャメル ケースのルールは、オブジェクトではなく、メソッドにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="7caf6-274">たとえばを参照してください`stock.Symbol`と`stock.Price`ではなく、`stock.symbol`または`stock.price`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="7caf6-275">`init`メソッドでは、アプリで、呼び出すことによって、サーバーから受信した各株オブジェクトのテーブル行に HTML が作成されます`formatStock`の書式設定のプロパティを`stock`オブジェクトし、を呼び出した`supplant`内のプレースホルダーを置換するには`rowTemplate`変数で、`stock`オブジェクト プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="7caf6-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="7caf6-276">結果の HTML は、株価のテーブルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="7caf6-277">呼び出す`init`としてそれに渡すことによって、`callback`後非同期に実行される関数`start`関数が完了するとします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="7caf6-278">呼び出した場合`init`呼び出した後、別の JavaScript ステートメントとして`start`、完了、接続を確立するのには start 関数を待たずにすぐに実行があるため、関数は失敗します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="7caf6-279">その場合は、`init`関数は呼び出しを試みる、`getAllStocks`関数アプリは、サーバー接続を確立する前にします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="7caf6-280">更新の株価の取得</span><span class="sxs-lookup"><span data-stu-id="7caf6-280">Getting updated stock prices</span></span>

<span data-ttu-id="7caf6-281">呼び出すサーバー株の価格が変更されたとき、`updateStockPrice`接続されているクライアントにします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="7caf6-282">アプリのクライアントのプロパティに関数を追加する、`stockTicker`使用できるようにする呼び出しをサーバーからのプロキシ。</span><span class="sxs-lookup"><span data-stu-id="7caf6-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="7caf6-283">`updateStockPrice`関数形式のテーブルに、サーバーから受信した株価オブジェクトの行と同様、`init`関数。</span><span class="sxs-lookup"><span data-stu-id="7caf6-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="7caf6-284">テーブルに行を追加することではなく、テーブル内の株式の現在の行を検索し、その行を新しいに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="7caf6-285">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="7caf6-285">Test the application</span></span>

<span data-ttu-id="7caf6-286">確認するためのアプリをテストすることができますが動作します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="7caf6-287">株価の変動をライブの株価のテーブルを表示するすべてのブラウザー ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="7caf6-288">ツールバーで、有効にする**スクリプトのデバッグ**しデバッグ モードでアプリを実行する [再生] ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![ユーザー モードをデバッグし、[再生] を選択のスクリーン ショット。](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="7caf6-290">表示するブラウザー ウィンドウが開き、**在庫表の Live**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="7caf6-291">ストックの表に、最初に行が示されます「読み込み中...」、その後、しばらくすると、アプリは、初期の株価データを表示し、株価を起動して変更します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="7caf6-292">ブラウザーから URL をコピーし、他の 2 つのブラウザーを開き、アドレス バーに Url を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="7caf6-293">初期の株価表示は、最初のブラウザーと同じと同時に変更が行われます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="7caf6-294">すべてのブラウザーを閉じ、新しいブラウザーを開いて、同じ URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="7caf6-295">StockTicker シングルトン オブジェクトは、サーバーで実行する継続します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="7caf6-296">**在庫表の Live**株式が変更し続けていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="7caf6-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="7caf6-297">図形を変更、初期テーブルのゼロは表示されません。</span><span class="sxs-lookup"><span data-stu-id="7caf6-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="7caf6-298">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="7caf6-299">ログの有効化</span><span class="sxs-lookup"><span data-stu-id="7caf6-299">Enable logging</span></span>

<span data-ttu-id="7caf6-300">SignalR では、トラブルシューティングで支援するために、クライアントで有効にできる組み込みのログ記録関数があります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="7caf6-301">このセクションでは、ログ記録を有効にし、ログを知る方法を次のトランスポート メソッドの SignalR を使用して表示する例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7caf6-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="7caf6-302">[Websocket](http://en.wikipedia.org/wiki/WebSocket)IIS 8 と現在のブラウザーでサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="7caf6-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="7caf6-303">[サーバー送信イベント](http://en.wikipedia.org/wiki/Server-sent_events)、Internet Explorer 以外のブラウザーでサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="7caf6-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="7caf6-304">[フレームの永久](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)、Internet Explorer でサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="7caf6-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="7caf6-305">[長いポーリングの Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)、すべてのブラウザーでサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="7caf6-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="7caf6-306">特定の接続では、SignalR は、サーバーとクライアントの両方をサポートする最適なトランスポートの方法を選択します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="7caf6-307">開いている*StockTicker.js*します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="7caf6-308">ファイルの最後に、接続を初期化するコードの直前のログ記録を有効にするコードの強調表示された行を追加します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="7caf6-309">キーを押して**F5**プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="7caf6-310">ブラウザーの開発者ツール ウィンドウを開きし、ログを表示するコンソールを選択します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="7caf6-311">新しい接続の転送方法をネゴシエートする SignalR のログを表示するページを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="7caf6-312">Windows 8 (IIS 8) で Internet Explorer 10 を実行している場合、トランスポート メソッドが**Websocket**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="7caf6-313">Windows 7 (IIS 7.5) で Internet Explorer 10 を実行している場合、トランスポート メソッドが**iframe**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="7caf6-314">Windows 8 (IIS 8) で Firefox 19 を実行している場合、トランスポート メソッドが**Websocket**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="7caf6-315">Firefox の場合、コンソール ウィンドウを取得する Firebug アドインのインストールします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="7caf6-316">Windows 7 (IIS 7.5) で Firefox 19 を実行している場合、トランスポート メソッドが**サーバーによって送信される**イベント。</span><span class="sxs-lookup"><span data-stu-id="7caf6-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="7caf6-317">StockTicker サンプルをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-317">Install the StockTicker sample</span></span>

<span data-ttu-id="7caf6-318">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) StockTicker アプリケーションをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="7caf6-319">NuGet パッケージには、ゼロから作成された簡略化されたバージョンより多くの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="7caf6-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="7caf6-320">チュートリアルのこのセクションでは、NuGet パッケージをインストールし、新機能とそれらを実装するコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7caf6-321">このチュートリアルの前の手順を実行せず、パッケージをインストールする場合は、プロジェクトに OWIN startup クラスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="7caf6-322">NuGet パッケージの場合は、この readme.txt ファイルには、この手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="7caf6-323">SignalR.Sample NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="7caf6-324">**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[NuGet パッケージの管理]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="7caf6-325">**NuGet パッケージ マネージャー。SignalR.StockTicker**、**参照**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="7caf6-326">**パッケージ ソース**、 **nuget.org**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="7caf6-327">入力*SignalR.Sample*検索ボックスを選び**Microsoft.AspNet.SignalR.Sample** > **インストール**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="7caf6-328">**ソリューション エクスプ ローラー**、展開、 *SignalR.Sample*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="7caf6-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="7caf6-329">SignalR.Sample パッケージをインストールすると、フォルダーとその内容が作成されます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="7caf6-330">*SignalR.Sample*フォルダーを右クリックして*StockTicker.html*、し、**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7caf6-331">SignalR.Sample NuGet のインストール パッケージは内にある jQuery のバージョンを変更可能性があります、*スクリプト*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="7caf6-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="7caf6-332">新しい*StockTicker.html*でパッケージをインストールするファイル、 *SignalR.Sample*フォルダーが、元のを実行する場合は、パッケージをインストールするjQueryバージョンとの同期に*StockTicker.html*ファイルを再び、最初にスクリプト タグ内の jQuery 参照を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="7caf6-333">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="7caf6-333">Run the application</span></span>

 <span data-ttu-id="7caf6-334">初めてのアプリで紹介したテーブルには、便利な機能が必要があります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="7caf6-335">完全な株価表示器のアプリケーションの新機能を示しています。 株価データおよび増加し、分類の色を変更する株価を表示するウィンドウを水平方向にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="7caf6-336">**F5 キー**を押してアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="7caf6-337">初めてアプリを実行して、「市場」は"closed"静的なテーブルとティッカー ウィンドウ スクロールはありませんを参照してください。</span><span class="sxs-lookup"><span data-stu-id="7caf6-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="7caf6-338">選択**Open Market**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-338">Select **Open Market**.</span></span>

    ![ライブ ティッカーのスクリーン ショット。](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="7caf6-340">**株式ティッカーの Live**を水平方向にスクロール ボックスを起動し、サーバーがランダムに株価の変更を定期的にブロードキャストする開始します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="7caf6-341">株式の価格が変更されるたびに、アプリでは、両方を更新、**在庫表の Live**と**株式ティッカーの Live**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="7caf6-342">株式の価格の変更は、正の値は、アプリには、背景が緑の在庫が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="7caf6-343">変更が負の値と、アプリには、背景が赤の素材が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="7caf6-344">選択**市場を閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="7caf6-345">表に、更新が停止します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-345">The table updates stop.</span></span>

    * <span data-ttu-id="7caf6-346">スクロール、ティッカーを停止します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="7caf6-347">選択**リセット**します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-347">Select **Reset**.</span></span>

    * <span data-ttu-id="7caf6-348">すべての株価データはリセットされます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-348">All stock data is reset.</span></span>

    * <span data-ttu-id="7caf6-349">アプリでは、料金の変更を開始する前に初期状態を復元します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="7caf6-350">ブラウザーから URL をコピーし、他の 2 つのブラウザーを開き、アドレス バーに Url を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="7caf6-351">各ブラウザーで同時に動的に更新される同じデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="7caf6-352">コントロールのいずれかを選択するとすべてのブラウザーと同じ方法を同時に応答します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="7caf6-353">ライブの株式相場表示</span><span class="sxs-lookup"><span data-stu-id="7caf6-353">Live Stock Ticker display</span></span>

<span data-ttu-id="7caf6-354">**株式ティッカーの Live**ディスプレイが順不同のリストで、`<div>`要素が 1 行に CSS スタイルで書式設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="7caf6-355">アプリを初期化し、テーブルと同様、ティッカーの更新: 内のプレース ホルダーを置き換えることで、`<li>`テンプレート文字列と動的に追加する、`<li>`要素を`<ul>`要素。</span><span class="sxs-lookup"><span data-stu-id="7caf6-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="7caf6-356">アプリでは、jQuery を使用してスクロール`animate`関数内で順序付けられていない一覧の左余白を変更するため、`<div>`します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="7caf6-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="7caf6-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="7caf6-358">株価情報の HTML コード:</span><span class="sxs-lookup"><span data-stu-id="7caf6-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="7caf6-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="7caf6-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="7caf6-360">株式ティッカーの CSS コード:</span><span class="sxs-lookup"><span data-stu-id="7caf6-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="7caf6-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="7caf6-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="7caf6-362">JQuery コードがスクロールします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="7caf6-363">クライアントが呼び出すことができる、サーバーで追加のメソッド</span><span class="sxs-lookup"><span data-stu-id="7caf6-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="7caf6-364">アプリに柔軟性を追加するには、追加のメソッドが、アプリが呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="7caf6-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="7caf6-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="7caf6-366">`StockTickerHub`クラスは、クライアントが呼び出すことができる 4 つの追加のメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="7caf6-367">アプリによる呼び出し`OpenMarket`、 `CloseMarket`、および`Reset`ページの上部にあるボタンに応答します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="7caf6-368">これらは、すべてのクライアントに直ちに伝達の状態に変更をトリガーする 1 つのクライアントのパターンを示します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="7caf6-369">これらの各メソッドでメソッドを呼び出し、`StockTicker`市場の状態の変更の原因し、新しい状態をブロードキャストするクラス。</span><span class="sxs-lookup"><span data-stu-id="7caf6-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="7caf6-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="7caf6-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="7caf6-371">`StockTicker`クラス、アプリは、市場の状態を保持、`MarketState`を返すプロパティを`MarketState`列挙値。</span><span class="sxs-lookup"><span data-stu-id="7caf6-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="7caf6-372">ロック ブロック内ではそれぞれの市場の状態を変更する方法、`StockTicker`クラスはスレッド セーフにするには。</span><span class="sxs-lookup"><span data-stu-id="7caf6-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="7caf6-373">このコードはスレッド セーフかどうかを確認する、`_marketState`をバックするフィールド、`MarketState`プロパティが指定されている`volatile`:</span><span class="sxs-lookup"><span data-stu-id="7caf6-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="7caf6-374">`BroadcastMarketStateChange`と`BroadcastMarketReset`メソッドは、クライアントで定義されている別のメソッドを呼び出す点を除いて、既に説明しましたが BroadcastStockPrice メソッドに似ています。</span><span class="sxs-lookup"><span data-stu-id="7caf6-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="7caf6-375">サーバーが呼び出すことができるクライアントの追加の関数</span><span class="sxs-lookup"><span data-stu-id="7caf6-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="7caf6-376">`updateStockPrice`関数は、テーブルとティッカーの表示の両方を処理するため、`jQuery.Color`を赤と緑の色をフラッシュします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="7caf6-377">新しい関数で*SignalR.StockTicker.js*を有効にして、市場の状態に基づいてボタンを無効にします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="7caf6-378">これらも停止または開始、**株式ティッカーの Live**水平方向にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="7caf6-379">多くの関数に追加されているため`ticker.client`、アプリでは、 [jQuery 関数を拡張する](http://api.jquery.com/jQuery.extend/)に追加します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="7caf6-380">接続の確立後に追加のクライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="7caf6-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="7caf6-381">クライアント接続を確立した後、追加の作業を行うがあります。</span><span class="sxs-lookup"><span data-stu-id="7caf6-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="7caf6-382">市場が開いているか、適切な呼び出しを閉じてとかどうか`marketOpened`または`marketClosed`関数。</span><span class="sxs-lookup"><span data-stu-id="7caf6-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="7caf6-383">ボタンにサーバー メソッドの呼び出しをアタッチします。</span><span class="sxs-lookup"><span data-stu-id="7caf6-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="7caf6-384">サーバーのメソッドは、アプリが接続を確立した後でまでボタンまでがワイヤード (有線) はありません。</span><span class="sxs-lookup"><span data-stu-id="7caf6-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="7caf6-385">これは、コードが利用可能になる前にサーバーのメソッドを呼び出すことはできませんのでです。</span><span class="sxs-lookup"><span data-stu-id="7caf6-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7caf6-386">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7caf6-386">Additional resources</span></span>

<span data-ttu-id="7caf6-387">このチュートリアルでは、接続されているすべてのクライアントをサーバーからメッセージをブロードキャストする SignalR アプリケーションをプログラミングする方法を学習できました。</span><span class="sxs-lookup"><span data-stu-id="7caf6-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="7caf6-388">ここで任意のクライアントからの通知に応答して定期的にメッセージをブロードキャストすることができます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="7caf6-389">マルチ スレッドのシングルトン インスタンスの概念は、マルチ プレーヤー オンライン ゲーム シナリオでサーバーの状態を維持するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="7caf6-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="7caf6-390">例については、次を参照してください。 [SignalR に基づいて ShootR ゲーム](https://github.com/NTaylorMullen/ShootR)します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="7caf6-391">ピア ツー ピア通信シナリオを示すチュートリアルについては、次を参照してください。 [SignalR の概要](introduction-to-signalr.md)と[SignalR によるリアルタイムの更新](tutorial-high-frequency-realtime-with-signalr.md)します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="7caf6-392">詳細については、SignalR は、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="7caf6-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="7caf6-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="7caf6-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="7caf6-394">SignalR プロジェクト</span><span class="sxs-lookup"><span data-stu-id="7caf6-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="7caf6-395">SignalR GitHub とサンプル</span><span class="sxs-lookup"><span data-stu-id="7caf6-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="7caf6-396">SignalR の Wiki</span><span class="sxs-lookup"><span data-stu-id="7caf6-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="7caf6-397">次の手順</span><span class="sxs-lookup"><span data-stu-id="7caf6-397">Next steps</span></span>

<span data-ttu-id="7caf6-398">このチュートリアルでしました。</span><span class="sxs-lookup"><span data-stu-id="7caf6-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7caf6-399">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="7caf6-399">Created the project</span></span>
> * <span data-ttu-id="7caf6-400">サーバー コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-400">Set up the server code</span></span>
> * <span data-ttu-id="7caf6-401">サーバー コードを調べる</span><span class="sxs-lookup"><span data-stu-id="7caf6-401">Examined the server code</span></span>
> * <span data-ttu-id="7caf6-402">クライアント コードを設定します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-402">Set up the client code</span></span>
> * <span data-ttu-id="7caf6-403">クライアント コードを調べる</span><span class="sxs-lookup"><span data-stu-id="7caf6-403">Examined the client code</span></span>
> * <span data-ttu-id="7caf6-404">アプリケーションのテスト</span><span class="sxs-lookup"><span data-stu-id="7caf6-404">Tested the application</span></span>
> * <span data-ttu-id="7caf6-405">ログ記録を有効になっています。</span><span class="sxs-lookup"><span data-stu-id="7caf6-405">Enabled logging</span></span>

<span data-ttu-id="7caf6-406">ASP.NET SignalR 2 を使用するリアルタイムの web アプリケーションを作成する方法については、次の記事に進んでください。</span><span class="sxs-lookup"><span data-stu-id="7caf6-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7caf6-407">SignalR によるリアルタイムの web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="7caf6-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)