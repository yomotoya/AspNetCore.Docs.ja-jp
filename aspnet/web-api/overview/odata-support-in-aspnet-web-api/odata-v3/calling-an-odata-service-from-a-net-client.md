---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: .NET クライアント (c#) から OData サービスを呼び出す |Microsoft ドキュメント
author: MikeWasson
description: このチュートリアルでは、c# クライアント アプリケーションから OData サービスを呼び出す方法を示します。 チュートリアルの Visual Studio 2013 (Visual S. と連携.. で使用されているソフトウェアのバージョン
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "28042395"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="274d1-104">.NET クライアント (c#) から OData サービスを呼び出す</span><span class="sxs-lookup"><span data-stu-id="274d1-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="274d1-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="274d1-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="274d1-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="274d1-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="274d1-107">このチュートリアルでは、c# クライアント アプリケーションから OData サービスを呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="274d1-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="274d1-108">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="274d1-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="274d1-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 で動作します)</span><span class="sxs-lookup"><span data-stu-id="274d1-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="274d1-110">WCF Data Services クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="274d1-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="274d1-111">Web API 2.</span><span class="sxs-lookup"><span data-stu-id="274d1-111">Web API 2.</span></span> <span data-ttu-id="274d1-112">(この OData サービスの例は Web API 2 を使用して構築していますが、クライアント アプリケーションは Web API に依存しません)。</span><span class="sxs-lookup"><span data-stu-id="274d1-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="274d1-113">このチュートリアルでは、OData サービスを呼び出すクライアント アプリケーションの作成について詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="274d1-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="274d1-114">OData サービスは、次のエンティティを公開します。</span><span class="sxs-lookup"><span data-stu-id="274d1-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="274d1-115">次の記事では、Web API の OData サービスを実装する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="274d1-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="274d1-116">(必要はありませんただしこのチュートリアルでは、理解しておくことを読み取れません。)</span><span class="sxs-lookup"><span data-stu-id="274d1-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="274d1-117">Web API 2 OData エンドポイントを作成します。</span><span class="sxs-lookup"><span data-stu-id="274d1-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="274d1-118">Web API 2 OData エンティティ関係</span><span class="sxs-lookup"><span data-stu-id="274d1-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="274d1-119">Web API 2 の OData アクション</span><span class="sxs-lookup"><span data-stu-id="274d1-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="274d1-120">サービス プロキシを生成する</span><span class="sxs-lookup"><span data-stu-id="274d1-120">Generate the Service Proxy</span></span>

<span data-ttu-id="274d1-121">最初の手順では、サービス プロキシを生成します。</span><span class="sxs-lookup"><span data-stu-id="274d1-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="274d1-122">サービス プロキシは、OData サービスにアクセスするためのメソッドを定義する .NET クラスです。</span><span class="sxs-lookup"><span data-stu-id="274d1-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="274d1-123">プロキシは、HTTP 要求にメソッド呼び出しを変換します。</span><span class="sxs-lookup"><span data-stu-id="274d1-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="274d1-124">Visual Studio での OData サービス プロジェクトを開くことで開始します。</span><span class="sxs-lookup"><span data-stu-id="274d1-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="274d1-125">CTRL + f5 キーを押してサービスを IIS Express でローカルに実行します。</span><span class="sxs-lookup"><span data-stu-id="274d1-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="274d1-126">Visual Studio を代入するポート番号を含む、ローカル アドレスに注意してください。</span><span class="sxs-lookup"><span data-stu-id="274d1-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="274d1-127">このアドレスは、プロキシを作成するときにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="274d1-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="274d1-128">次に、Visual Studio の別のインスタンスを開き、コンソール アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="274d1-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="274d1-129">コンソール アプリケーションは、OData クライアント アプリケーションになります。</span><span class="sxs-lookup"><span data-stu-id="274d1-129">The console application will be our OData client application.</span></span> <span data-ttu-id="274d1-130">(しても、プロジェクトを追加できますサービスと同じソリューションです。)</span><span class="sxs-lookup"><span data-stu-id="274d1-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="274d1-131">残りの手順には、コンソール プロジェクトが参照してください。</span><span class="sxs-lookup"><span data-stu-id="274d1-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="274d1-132">ソリューション エクスプ ローラーで右クリック**参照**選択**サービス参照の追加**です。</span><span class="sxs-lookup"><span data-stu-id="274d1-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="274d1-133">**サービス参照の追加**ダイアログ ボックスで、OData サービスのアドレスを入力します。</span><span class="sxs-lookup"><span data-stu-id="274d1-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="274d1-134">ここで*ポート*のポート番号です。</span><span class="sxs-lookup"><span data-stu-id="274d1-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="274d1-135">**Namespace**、"ProductService"を入力します。</span><span class="sxs-lookup"><span data-stu-id="274d1-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="274d1-136">このオプションは、プロキシ クラスの名前空間を定義します。</span><span class="sxs-lookup"><span data-stu-id="274d1-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="274d1-137">**[検索]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="274d1-137">Click **Go**.</span></span> <span data-ttu-id="274d1-138">Visual Studio では、サービス内のエンティティを検出する OData メタデータ ドキュメントを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="274d1-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="274d1-139">をクリックして**OK**プロキシ クラスをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="274d1-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="274d1-140">サービスのプロキシ クラスのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="274d1-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="274d1-141">内部、`Main`メソッド、次のように、プロキシ クラスの新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="274d1-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="274d1-142">もう一度、使用して実際のポート番号、サービスが実行されています。</span><span class="sxs-lookup"><span data-stu-id="274d1-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="274d1-143">サービスをデプロイするときに、ライブのサービスの URI を使用します。</span><span class="sxs-lookup"><span data-stu-id="274d1-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="274d1-144">プロキシを更新する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="274d1-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="274d1-145">次のコードでは、コンソール ウィンドウに、要求の Uri を出力するイベント ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="274d1-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="274d1-146">この手順は必須では、興味深いことに、各クエリの Uri。</span><span class="sxs-lookup"><span data-stu-id="274d1-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="274d1-147">サービスをクエリします。</span><span class="sxs-lookup"><span data-stu-id="274d1-147">Query the Service</span></span>

<span data-ttu-id="274d1-148">次のコードは、OData サービスから製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="274d1-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="274d1-149">HTTP 要求を送信または応答を解析するためのコードを記述する必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="274d1-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="274d1-150">プロキシ クラスは、この自動的に列挙するとき、`Container.Products`内のコレクション、 **foreach**ループします。</span><span class="sxs-lookup"><span data-stu-id="274d1-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="274d1-151">アプリケーションを実行するときに、出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="274d1-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="274d1-152">ID でエンティティを取得する、`where`句。</span><span class="sxs-lookup"><span data-stu-id="274d1-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="274d1-153">このトピックの残りの部分の全体を表示しない`Main`関数は、サービスの呼び出しに必要なコードだけです。</span><span class="sxs-lookup"><span data-stu-id="274d1-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="274d1-154">クエリ オプションを適用します。</span><span class="sxs-lookup"><span data-stu-id="274d1-154">Apply Query Options</span></span>

<span data-ttu-id="274d1-155">OData 定義[クエリ オプション](../supporting-odata-query-options.md)フィルター、並べ替え、ページ データなどに使用できます。</span><span class="sxs-lookup"><span data-stu-id="274d1-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="274d1-156">サービス プロキシでは、さまざまな LINQ 式を使用してこれらのオプションを適用できます。</span><span class="sxs-lookup"><span data-stu-id="274d1-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="274d1-157">このセクションでは、簡単な例を示します。</span><span class="sxs-lookup"><span data-stu-id="274d1-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="274d1-158">詳細については、トピックを参照してください。 [LINQ に関する留意点 (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) msdn です。</span><span class="sxs-lookup"><span data-stu-id="274d1-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="274d1-159">フィルター処理 ($filter)</span><span class="sxs-lookup"><span data-stu-id="274d1-159">Filtering ($filter)</span></span>

<span data-ttu-id="274d1-160">フィルターを使用して、`where`句。</span><span class="sxs-lookup"><span data-stu-id="274d1-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="274d1-161">製品カテゴリで次の例をフィルターします。</span><span class="sxs-lookup"><span data-stu-id="274d1-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="274d1-162">このコードは、次の OData クエリに対応します。</span><span class="sxs-lookup"><span data-stu-id="274d1-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="274d1-163">プロキシに変換される、 `where` OData に句`$filter`式。</span><span class="sxs-lookup"><span data-stu-id="274d1-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="274d1-164">並べ替え ($orderby)</span><span class="sxs-lookup"><span data-stu-id="274d1-164">Sorting ($orderby)</span></span>

<span data-ttu-id="274d1-165">並べ替えるを使用して、`orderby`句。</span><span class="sxs-lookup"><span data-stu-id="274d1-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="274d1-166">次の例は、価格、最上位から最下位から並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="274d1-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="274d1-167">次に、対応する OData 要求を示します。</span><span class="sxs-lookup"><span data-stu-id="274d1-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="274d1-168">クライアント側のページング ($skip と $top)</span><span class="sxs-lookup"><span data-stu-id="274d1-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="274d1-169">大きなエンティティ セットに対して、クライアントで結果の数を制限する必要が場合があります。</span><span class="sxs-lookup"><span data-stu-id="274d1-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="274d1-170">たとえば、クライアントは、一度に 10 個のエントリを表示する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="274d1-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="274d1-171">これと呼ばれる*クライアント側のページング*です。</span><span class="sxs-lookup"><span data-stu-id="274d1-171">This is called *client-side paging*.</span></span> <span data-ttu-id="274d1-172">(も[サーバー側のページング](../supporting-odata-query-options.md#server-paging)サーバーが結果の数を制限します)。クライアント側のページングを実行するには、LINQ を使用して**Skip**と**かかる**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="274d1-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="274d1-173">次の例では、最初の 40 の結果をスキップし、[次へ] の 10 を受け取る。</span><span class="sxs-lookup"><span data-stu-id="274d1-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="274d1-174">次に、対応する OData 要求を示します。</span><span class="sxs-lookup"><span data-stu-id="274d1-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="274d1-175">($Select) を選択し、[展開] ($展開)</span><span class="sxs-lookup"><span data-stu-id="274d1-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="274d1-176">関連エンティティを含めるを使用して、`DataServiceQuery<t>.Expand`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="274d1-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="274d1-177">例については、含める、`Supplier`各`Product`:</span><span class="sxs-lookup"><span data-stu-id="274d1-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="274d1-178">次に、対応する OData 要求を示します。</span><span class="sxs-lookup"><span data-stu-id="274d1-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="274d1-179">応答の形状を変更するには、LINQ を使用**選択**句。</span><span class="sxs-lookup"><span data-stu-id="274d1-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="274d1-180">次の例では、その他のプロパティがないと、各製品の名前だけを取得します。</span><span class="sxs-lookup"><span data-stu-id="274d1-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="274d1-181">次に、対応する OData 要求を示します。</span><span class="sxs-lookup"><span data-stu-id="274d1-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="274d1-182">Select 句では、関連エンティティを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="274d1-182">A select clause can include related entities.</span></span> <span data-ttu-id="274d1-183">その場合は、呼び出す必要はありません**展開**; プロキシの拡張をここで含む自動的にします。</span><span class="sxs-lookup"><span data-stu-id="274d1-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="274d1-184">次の例では、各製品の仕入先と名前を取得します。</span><span class="sxs-lookup"><span data-stu-id="274d1-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="274d1-185">次に、対応する OData 要求を示します。</span><span class="sxs-lookup"><span data-stu-id="274d1-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="274d1-186">含まれていることを確認、 **$展開**オプション。</span><span class="sxs-lookup"><span data-stu-id="274d1-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="274d1-187">$Select および $expand の詳細については、展開しを参照してください[$ $select を使用して、展開、および Web API 2 で $value](../using-select-expand-and-value.md)です。</span><span class="sxs-lookup"><span data-stu-id="274d1-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="274d1-188">新しいエンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="274d1-188">Add a New Entity</span></span>

<span data-ttu-id="274d1-189">エンティティ セットに新しいエンティティを追加するには、呼び出す`AddToEntitySet`ここで、 *EntitySet*エンティティ セットの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="274d1-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="274d1-190">たとえば、`AddToProducts`新しい`Product`を`Products`エンティティ セット。</span><span class="sxs-lookup"><span data-stu-id="274d1-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="274d1-191">プロキシを生成するときに WCF Data Services が自動的に作成これら厳密に型指定された**AddTo**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="274d1-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="274d1-192">2 つのエンティティ間のリンクを追加するには、使用、 **AddLink**と**SetLink**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="274d1-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="274d1-193">次のコードでは、新しい仕入先と、新しい製品を追加し、それらの間のリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="274d1-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="274d1-194">使用して**AddLink**ナビゲーション プロパティがコレクションの場合。</span><span class="sxs-lookup"><span data-stu-id="274d1-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="274d1-195">製品を追加するこの例では、`Products`サプライヤーのコレクション。</span><span class="sxs-lookup"><span data-stu-id="274d1-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="274d1-196">使用して**SetLink**ナビゲーション プロパティが 1 つのエンティティの場合。</span><span class="sxs-lookup"><span data-stu-id="274d1-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="274d1-197">この例では設定するか、`Supplier`製品のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="274d1-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="274d1-198">更新/パッチ</span><span class="sxs-lookup"><span data-stu-id="274d1-198">Update / Patch</span></span>

<span data-ttu-id="274d1-199">エンティティを更新するには、呼び出し、 **UpdateObject**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="274d1-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="274d1-200">呼び出すときに、更新プログラムが実行される**SaveChanges**です。</span><span class="sxs-lookup"><span data-stu-id="274d1-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="274d1-201">既定では、WCF は、HTTP MERGE 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="274d1-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="274d1-202">**PatchOnUpdate**オプションは、HTTP PATCH を代わりに送信する WCF に指示します。</span><span class="sxs-lookup"><span data-stu-id="274d1-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="274d1-203">なぜマージとパッチを適用しますか。</span><span class="sxs-lookup"><span data-stu-id="274d1-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="274d1-204">元の HTTP 1.1 の仕様 ([RCF 2616](http://tools.ietf.org/html/rfc2616))「部分的な更新」のセマンティクスを持つ任意の HTTP メソッドを定義しませんでした。</span><span class="sxs-lookup"><span data-stu-id="274d1-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="274d1-205">部分的な更新をサポートするためには、OData の仕様は、マージ メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="274d1-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="274d1-206">2010 では、 [RFC 5789](http://tools.ietf.org/html/rfc5789)部分的な更新プログラムの更新プログラムの方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="274d1-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="274d1-207">この履歴の一部を読み取ることができる[ブログの投稿](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx)WCF データ サービス ブログ。</span><span class="sxs-lookup"><span data-stu-id="274d1-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="274d1-208">今日では、修正プログラムはマージ経由で推奨されます。</span><span class="sxs-lookup"><span data-stu-id="274d1-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="274d1-209">Web API のスキャフォールディングによって作成された OData コント ローラーには、両方の方法がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="274d1-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="274d1-210">エンティティ (PUT のセマンティクス) 全体を置換する場合は、指定、 **ReplaceOnUpdate**オプション。</span><span class="sxs-lookup"><span data-stu-id="274d1-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="274d1-211">これにより、HTTP PUT 要求を送信する WCF です。</span><span class="sxs-lookup"><span data-stu-id="274d1-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="274d1-212">エンティティを削除します。</span><span class="sxs-lookup"><span data-stu-id="274d1-212">Delete an Entity</span></span>

<span data-ttu-id="274d1-213">エンティティを削除するには、呼び出す**DeleteObject**です。</span><span class="sxs-lookup"><span data-stu-id="274d1-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="274d1-214">OData アクションを呼び出す</span><span class="sxs-lookup"><span data-stu-id="274d1-214">Invoke an OData Action</span></span>

<span data-ttu-id="274d1-215">OData では、[アクション](odata-actions.md)エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法は、します。</span><span class="sxs-lookup"><span data-stu-id="274d1-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="274d1-216">OData メタデータ ドキュメントでは、操作を説明をそれらのプロキシ クラス上で厳密に型指定されたメソッドを一切は作成されません。</span><span class="sxs-lookup"><span data-stu-id="274d1-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="274d1-217">ジェネリックを使用して、OData アクションを呼び出すことができますも**Execute**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="274d1-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="274d1-218">ただし、パラメーターと戻り値のデータ型を知っておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="274d1-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="274d1-219">たとえば、`RateProduct`アクションは、型の「評価」をという名前のパラメーターを受け取る`Int32`を返します、`double`です。</span><span class="sxs-lookup"><span data-stu-id="274d1-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="274d1-220">次のコードでは、このアクションを呼び出す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="274d1-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="274d1-221">詳細については、次を参照してください。[サービス操作の呼び出しとアクション](https://msdn.microsoft.com/library/hh230677.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="274d1-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="274d1-222">1 つのオプションは拡張を**コンテナー**アクションを起動する厳密に型指定されたメソッドを提供するクラス。</span><span class="sxs-lookup"><span data-stu-id="274d1-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
