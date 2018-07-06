---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: .NET クライアント (c#) から OData サービスを呼び出す |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは、c# クライアント アプリケーションから OData サービスを呼び出す方法を示します。 チュートリアルの Visual Studio 2013 (Visual S. 連動.. で使用されているソフトウェア バージョン
ms.author: aspnetcontent
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 72dca7dc2ec27f15c9f0526621a713267f5835f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819556"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="3950d-104">.NET クライアント (c#) から OData サービスを呼び出す</span><span class="sxs-lookup"><span data-stu-id="3950d-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="3950d-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3950d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3950d-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="3950d-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="3950d-107">このチュートリアルでは、c# クライアント アプリケーションから OData サービスを呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3950d-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3950d-108">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="3950d-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3950d-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 で動作)</span><span class="sxs-lookup"><span data-stu-id="3950d-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="3950d-110">WCF Data Services クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="3950d-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="3950d-111">Web API 2.</span><span class="sxs-lookup"><span data-stu-id="3950d-111">Web API 2.</span></span> <span data-ttu-id="3950d-112">(この OData サービスの例は Web API 2 を使用して構築していますが、クライアント アプリケーションは Web API に依存しません)。</span><span class="sxs-lookup"><span data-stu-id="3950d-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="3950d-113">このチュートリアルで説明します、OData サービスを呼び出すクライアント アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3950d-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="3950d-114">OData サービスでは、次のエンティティを公開します。</span><span class="sxs-lookup"><span data-stu-id="3950d-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="3950d-115">次の記事では、Web api OData サービスを実装する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3950d-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="3950d-116">(ただし、このチュートリアルを理解しておくことを読み取るする必要はありません)。</span><span class="sxs-lookup"><span data-stu-id="3950d-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="3950d-117">Web API 2 OData エンドポイントの作成</span><span class="sxs-lookup"><span data-stu-id="3950d-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="3950d-118">Web API 2 OData エンティティ関係</span><span class="sxs-lookup"><span data-stu-id="3950d-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="3950d-119">Web API 2 の OData アクション</span><span class="sxs-lookup"><span data-stu-id="3950d-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="3950d-120">サービス プロキシを生成する</span><span class="sxs-lookup"><span data-stu-id="3950d-120">Generate the Service Proxy</span></span>

<span data-ttu-id="3950d-121">最初の手順では、サービス プロキシを生成します。</span><span class="sxs-lookup"><span data-stu-id="3950d-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="3950d-122">サービス プロキシは、OData サービスにアクセスするためのメソッドを定義する .NET クラスです。</span><span class="sxs-lookup"><span data-stu-id="3950d-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="3950d-123">プロキシでは、メソッド呼び出しの HTTP 要求に変換します。</span><span class="sxs-lookup"><span data-stu-id="3950d-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="3950d-124">Visual Studio での OData サービスのプロジェクトを開くことで開始します。</span><span class="sxs-lookup"><span data-stu-id="3950d-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="3950d-125">IIS Express でローカルでサービスを実行するには、CTRL + F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="3950d-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="3950d-126">Visual Studio による割り当てポート番号を含む、ローカル アドレスに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3950d-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="3950d-127">このアドレスは、プロキシを作成するときにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3950d-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="3950d-128">次に、Visual Studio の別のインスタンスを開き、コンソール アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3950d-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="3950d-129">コンソール アプリケーションは、OData クライアント アプリケーションになります。</span><span class="sxs-lookup"><span data-stu-id="3950d-129">The console application will be our OData client application.</span></span> <span data-ttu-id="3950d-130">(追加することできますも、プロジェクト、サービスと同じソリューションにします。)</span><span class="sxs-lookup"><span data-stu-id="3950d-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="3950d-131">残りの手順は、コンソール プロジェクトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3950d-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="3950d-132">ソリューション エクスプ ローラーで右クリックして**参照**選択**サービス参照の追加**します。</span><span class="sxs-lookup"><span data-stu-id="3950d-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="3950d-133">**サービス参照の追加**ダイアログ ボックスで、OData サービスのアドレスを入力します。</span><span class="sxs-lookup"><span data-stu-id="3950d-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="3950d-134">場所*ポート*のポート番号です。</span><span class="sxs-lookup"><span data-stu-id="3950d-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="3950d-135">**Namespace**、"ProductService"を入力します。</span><span class="sxs-lookup"><span data-stu-id="3950d-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="3950d-136">このオプションは、プロキシ クラスの名前空間を定義します。</span><span class="sxs-lookup"><span data-stu-id="3950d-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="3950d-137">**[検索]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3950d-137">Click **Go**.</span></span> <span data-ttu-id="3950d-138">Visual Studio では、サービス内のエンティティを検出する OData メタデータ ドキュメントを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="3950d-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="3950d-139">クリックして**OK**をプロジェクトにプロキシ クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="3950d-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="3950d-140">サービスのプロキシ クラスのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="3950d-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="3950d-141">内で、`Main`メソッドでは、次のように、プロキシ クラスの新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="3950d-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="3950d-142">ここでも、サービスが実行されているを実際のポート番号に使用します。</span><span class="sxs-lookup"><span data-stu-id="3950d-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="3950d-143">サービスをデプロイするときに、ライブ サービスの URI を使用します。</span><span class="sxs-lookup"><span data-stu-id="3950d-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="3950d-144">プロキシを更新する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3950d-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="3950d-145">次のコードは、コンソール ウィンドウへの要求 Uri を出力するイベント ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="3950d-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="3950d-146">この手順は必須では、興味深いことに、各クエリの Uri。</span><span class="sxs-lookup"><span data-stu-id="3950d-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="3950d-147">サービスをクエリします。</span><span class="sxs-lookup"><span data-stu-id="3950d-147">Query the Service</span></span>

<span data-ttu-id="3950d-148">次のコードでは、OData サービスから製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="3950d-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="3950d-149">HTTP 要求を送信または応答を解析するコードを記述する必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3950d-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="3950d-150">プロキシ クラスが列挙するときは自動的にこの、`Container.Products`内のコレクション、 **foreach**ループします。</span><span class="sxs-lookup"><span data-stu-id="3950d-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="3950d-151">アプリケーションを実行すると、出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3950d-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="3950d-152">ID でエンティティを取得する、`where`句。</span><span class="sxs-lookup"><span data-stu-id="3950d-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="3950d-153">このトピックの残りの部分全体を見せしません`Main`関数は、サービスの呼び出しに必要なコードだけです。</span><span class="sxs-lookup"><span data-stu-id="3950d-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="3950d-154">クエリ オプションを適用します。</span><span class="sxs-lookup"><span data-stu-id="3950d-154">Apply Query Options</span></span>

<span data-ttu-id="3950d-155">OData 定義[クエリ オプション](../supporting-odata-query-options.md)フィルター、並べ替え、ページのデータなどに使用できます。</span><span class="sxs-lookup"><span data-stu-id="3950d-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="3950d-156">サービス プロキシには、さまざまな LINQ 式を使用してこれらのオプションを適用できます。</span><span class="sxs-lookup"><span data-stu-id="3950d-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="3950d-157">このセクションでは、簡単な例を紹介します。</span><span class="sxs-lookup"><span data-stu-id="3950d-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="3950d-158">詳細については、トピックを参照してください。 [LINQ に関する留意点 (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) msdn です。</span><span class="sxs-lookup"><span data-stu-id="3950d-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="3950d-159">フィルター処理 ($filter)</span><span class="sxs-lookup"><span data-stu-id="3950d-159">Filtering ($filter)</span></span>

<span data-ttu-id="3950d-160">フィルターを使用して、`where`句。</span><span class="sxs-lookup"><span data-stu-id="3950d-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="3950d-161">製品カテゴリで次の例のフィルター。</span><span class="sxs-lookup"><span data-stu-id="3950d-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="3950d-162">このコードは、次の OData クエリに対応します。</span><span class="sxs-lookup"><span data-stu-id="3950d-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="3950d-163">プロキシに変換しますが、 `where` OData 句`$filter`式。</span><span class="sxs-lookup"><span data-stu-id="3950d-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="3950d-164">並べ替え ($orderby)</span><span class="sxs-lookup"><span data-stu-id="3950d-164">Sorting ($orderby)</span></span>

<span data-ttu-id="3950d-165">並べ替えるには、使用、`orderby`句。</span><span class="sxs-lookup"><span data-stu-id="3950d-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="3950d-166">次の例は、高いものからの価格で並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="3950d-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="3950d-167">対応する OData 要求を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3950d-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="3950d-168">クライアント側のページング ($skip および $top)</span><span class="sxs-lookup"><span data-stu-id="3950d-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="3950d-169">大きなエンティティ セットの場合は、クライアントは、結果の数を制限する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3950d-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="3950d-170">たとえば、クライアントは、一度に 10 個のエントリを表示する場合があります。</span><span class="sxs-lookup"><span data-stu-id="3950d-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="3950d-171">これは呼び出されます*クライアント側のページング*します。</span><span class="sxs-lookup"><span data-stu-id="3950d-171">This is called *client-side paging*.</span></span> <span data-ttu-id="3950d-172">(も[サーバー側ページング](../supporting-odata-query-options.md#server-paging)サーバーが結果の数を制限します)。クライアント側のページングを実行するには、LINQ を使用して**スキップ**と**かかる**メソッド。</span><span class="sxs-lookup"><span data-stu-id="3950d-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="3950d-173">次の例では、40 の結果をスキップし、[次へ] の 10 を受け取る。</span><span class="sxs-lookup"><span data-stu-id="3950d-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="3950d-174">対応する OData 要求を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3950d-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="3950d-175">($Select) を選択し、[展開] (展開 $)</span><span class="sxs-lookup"><span data-stu-id="3950d-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="3950d-176">関連エンティティを含めるには使用、`DataServiceQuery<t>.Expand`メソッド。</span><span class="sxs-lookup"><span data-stu-id="3950d-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="3950d-177">などの`Supplier`各`Product`:</span><span class="sxs-lookup"><span data-stu-id="3950d-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="3950d-178">対応する OData 要求を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3950d-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="3950d-179">応答の形状を変更するには、LINQ を使用して、**選択**句。</span><span class="sxs-lookup"><span data-stu-id="3950d-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="3950d-180">次の例では、その他のプロパティを持たない、各製品の名前だけを取得します。</span><span class="sxs-lookup"><span data-stu-id="3950d-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="3950d-181">対応する OData 要求を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3950d-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="3950d-182">Select 句では、関連エンティティを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3950d-182">A select clause can include related entities.</span></span> <span data-ttu-id="3950d-183">その場合、呼び出さない**展開**; プロキシで拡張をここでは、自動的にします。</span><span class="sxs-lookup"><span data-stu-id="3950d-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="3950d-184">次の例では、各製品の提供元と名前を取得します。</span><span class="sxs-lookup"><span data-stu-id="3950d-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="3950d-185">対応する OData 要求を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3950d-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="3950d-186">それに含まれる通知、 **$展開**オプション。</span><span class="sxs-lookup"><span data-stu-id="3950d-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="3950d-187">$Select および $expand の詳細については、展開しを参照してください[$select を使用して、$ の展開、および Web API 2 で $value](../using-select-expand-and-value.md)します。</span><span class="sxs-lookup"><span data-stu-id="3950d-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="3950d-188">新しいエンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="3950d-188">Add a New Entity</span></span>

<span data-ttu-id="3950d-189">エンティティ セットには、新しいエンティティを追加するには、呼び出す`AddToEntitySet`ここで、 *EntitySet*エンティティ セットの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="3950d-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="3950d-190">たとえば、`AddToProducts`新しい`Product`を`Products`エンティティ セット。</span><span class="sxs-lookup"><span data-stu-id="3950d-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="3950d-191">プロキシを生成するときに WCF Data Services が自動的に作成これら厳密に型指定された**AddTo**メソッド。</span><span class="sxs-lookup"><span data-stu-id="3950d-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="3950d-192">2 つのエンティティ間のリンクを追加するには、使用、 **AddLink**と**SetLink**メソッド。</span><span class="sxs-lookup"><span data-stu-id="3950d-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="3950d-193">次のコードでは、新しい仕入先と新しい製品を追加し、し、それらの間のリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="3950d-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="3950d-194">使用**AddLink**ナビゲーション プロパティは、コレクションです。</span><span class="sxs-lookup"><span data-stu-id="3950d-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="3950d-195">この例で追加する製品、`Products`サプライヤーのコレクション。</span><span class="sxs-lookup"><span data-stu-id="3950d-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="3950d-196">使用**SetLink**ナビゲーション プロパティが 1 つのエンティティ。</span><span class="sxs-lookup"><span data-stu-id="3950d-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="3950d-197">この例では設定、`Supplier`製品のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="3950d-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="3950d-198">更新/パッチ</span><span class="sxs-lookup"><span data-stu-id="3950d-198">Update / Patch</span></span>

<span data-ttu-id="3950d-199">エンティティを更新するには、呼び出し、 **UpdateObject**メソッド。</span><span class="sxs-lookup"><span data-stu-id="3950d-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="3950d-200">呼び出すときに、更新プログラムが実行される**SaveChanges**します。</span><span class="sxs-lookup"><span data-stu-id="3950d-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="3950d-201">既定では、WCF は、HTTP MERGE 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="3950d-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="3950d-202">**PatchOnUpdate**オプションを代わりに、HTTP PATCH を送信する WCF に指示します。</span><span class="sxs-lookup"><span data-stu-id="3950d-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="3950d-203">マージと PATCH なぜでしょうか。</span><span class="sxs-lookup"><span data-stu-id="3950d-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="3950d-204">元の HTTP 1.1 の仕様 ([RCF 2616](http://tools.ietf.org/html/rfc2616))「部分更新」のセマンティクスを持つ任意の HTTP メソッドを定義しませんでした。</span><span class="sxs-lookup"><span data-stu-id="3950d-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="3950d-205">部分的な更新をサポートするためには、OData 仕様には、MERGE メソッドが定義されています。</span><span class="sxs-lookup"><span data-stu-id="3950d-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="3950d-206">2010 では、 [RFC 5789](http://tools.ietf.org/html/rfc5789)部分的な更新プログラムの PATCH メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="3950d-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="3950d-207">この履歴の一部を読み取ることができます[ブログの投稿](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx)WCF Data Services のブログにします。</span><span class="sxs-lookup"><span data-stu-id="3950d-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="3950d-208">今日では、修正プログラムがマージに適しています。</span><span class="sxs-lookup"><span data-stu-id="3950d-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="3950d-209">Web API のスキャフォールディングによって作成された OData コント ローラーには、両方の方法がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="3950d-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="3950d-210">エンティティ (PUT セマンティクス) 全体を置換する場合は、指定、 **ReplaceOnUpdate**オプション。</span><span class="sxs-lookup"><span data-stu-id="3950d-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="3950d-211">これにより、HTTP PUT 要求を送信する WCF です。</span><span class="sxs-lookup"><span data-stu-id="3950d-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="3950d-212">エンティティを削除します。</span><span class="sxs-lookup"><span data-stu-id="3950d-212">Delete an Entity</span></span>

<span data-ttu-id="3950d-213">エンティティを削除する**DeleteObject**します。</span><span class="sxs-lookup"><span data-stu-id="3950d-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="3950d-214">OData アクションを呼び出す</span><span class="sxs-lookup"><span data-stu-id="3950d-214">Invoke an OData Action</span></span>

<span data-ttu-id="3950d-215">Odata では、[アクション](odata-actions.md)エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。</span><span class="sxs-lookup"><span data-stu-id="3950d-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="3950d-216">アクションの説明が、OData メタデータ ドキュメントをそれらのプロキシ クラス上で、厳密に型指定されたメソッドは作成されません。</span><span class="sxs-lookup"><span data-stu-id="3950d-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="3950d-217">ジェネリックを使用して OData アクションを呼び出すことができますも**Execute**メソッド。</span><span class="sxs-lookup"><span data-stu-id="3950d-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="3950d-218">ただし、パラメーターと戻り値のデータ型を把握する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3950d-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="3950d-219">たとえば、`RateProduct`アクションは、型の「評価」という名前のパラメーターを受け取る`Int32`を返します、`double`します。</span><span class="sxs-lookup"><span data-stu-id="3950d-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="3950d-220">次のコードでは、この操作を呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3950d-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="3950d-221">詳細については、次を参照してください。[呼び出すサービス操作とアクション](https://msdn.microsoft.com/library/hh230677.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="3950d-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="3950d-222">1 つのオプションは、拡張する、**コンテナー**処理を実行する厳密に型指定されたメソッドを提供するクラス。</span><span class="sxs-lookup"><span data-stu-id="3950d-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
