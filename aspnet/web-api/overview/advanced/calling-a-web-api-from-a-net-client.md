---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: .NET クライアント (c#) から Web API を呼び出す |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6b034642f1616a9d5c6495b65d4adeeae9a6f2c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821392"
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="c4192-102">.NET クライアント (c#) から Web API を呼び出す</span><span class="sxs-lookup"><span data-stu-id="c4192-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="c4192-103">作成者[Mike Wasson](https://github.com/MikeWasson)および[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4192-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4192-104">[完成したプロジェクトをダウンロード](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)します。</span><span class="sxs-lookup"><span data-stu-id="c4192-104">[Download Completed Project](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="c4192-105">[ダウンロードの方法はこちらをご覧ください。](/aspnet/core/tutorials/#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="c4192-105">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="c4192-106">このチュートリアルでは [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) を使用して .NET アプリケーションから web API を呼び出す方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="c4192-106">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="c4192-107">このチュートリアルでは、次の web API を使用するクライアント アプリケーションについて書かれています。</span><span class="sxs-lookup"><span data-stu-id="c4192-107">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="c4192-108">アクション</span><span class="sxs-lookup"><span data-stu-id="c4192-108">Action</span></span> | <span data-ttu-id="c4192-109">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="c4192-109">HTTP method</span></span> | <span data-ttu-id="c4192-110">相対 URI</span><span class="sxs-lookup"><span data-stu-id="c4192-110">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c4192-111">ID によって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="c4192-111">Get a product by ID</span></span> | <span data-ttu-id="c4192-112">GET</span><span class="sxs-lookup"><span data-stu-id="c4192-112">GET</span></span> | <span data-ttu-id="c4192-113">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="c4192-113">/api/products/*id*</span></span> |
| <span data-ttu-id="c4192-114">新しい成果物を作成します。</span><span class="sxs-lookup"><span data-stu-id="c4192-114">Create a new product</span></span> | <span data-ttu-id="c4192-115">POST</span><span class="sxs-lookup"><span data-stu-id="c4192-115">POST</span></span> | <span data-ttu-id="c4192-116">/api/products</span><span class="sxs-lookup"><span data-stu-id="c4192-116">/api/products</span></span> |
| <span data-ttu-id="c4192-117">製品を更新します。</span><span class="sxs-lookup"><span data-stu-id="c4192-117">Update a product</span></span> | <span data-ttu-id="c4192-118">PUT</span><span class="sxs-lookup"><span data-stu-id="c4192-118">PUT</span></span> | <span data-ttu-id="c4192-119">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="c4192-119">/api/products/*id*</span></span> |
| <span data-ttu-id="c4192-120">製品を削除します。</span><span class="sxs-lookup"><span data-stu-id="c4192-120">Delete a product</span></span> | <span data-ttu-id="c4192-121">Del</span><span class="sxs-lookup"><span data-stu-id="c4192-121">DELETE</span></span> | <span data-ttu-id="c4192-122">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="c4192-122">/api/products/*id*</span></span> |

<span data-ttu-id="c4192-123">ASP.NET Web API を使用して、この API を実装する方法については [CRUD 操作をサポートする Web API の作成](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c4192-123">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="c4192-124">簡潔に示す目的のため、このチュートリアルではクライアント アプリケーションとして Windows コンソール アプリケーションを使用します。</span><span class="sxs-lookup"><span data-stu-id="c4192-124">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="c4192-125">**HttpClient** は Windows Phone や Windows ストア アプリでも同様にサポートされています。</span><span class="sxs-lookup"><span data-stu-id="c4192-125">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="c4192-126">より詳しい情報については [複数のプラットフォームを使用してポータブル ライブラリの Web API クライアント コードの記述。](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c4192-126">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="c4192-127">コンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4192-127">Create the Console Application</span></span>

<span data-ttu-id="c4192-128">Visual Studio で、**HttpClientSample** という名前の新しい Windows コンソール アプリを作成し、次のコードに貼り付けます。
</span><span class="sxs-lookup"><span data-stu-id="c4192-128">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="c4192-129">上記のコードでは、完全なクライアント アプリです。</span><span class="sxs-lookup"><span data-stu-id="c4192-129">The preceding code is the complete client app.</span></span>

<span data-ttu-id="c4192-130">`RunAsync` が実行され、完了するまでブロックされます。</span><span class="sxs-lookup"><span data-stu-id="c4192-130">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="c4192-131">**HttpClient**メソッドはネットワーク I/O として振る舞うため、多くの場合、非同期です。</span><span class="sxs-lookup"><span data-stu-id="c4192-131">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="c4192-132">すべての非同期タスクは `RunAsync` 内で完了します。</span><span class="sxs-lookup"><span data-stu-id="c4192-132">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="c4192-133">通常、アプリは、メイン スレッドをブロックしませんが、このアプリはユーザーとの対話を許可しません。</span><span class="sxs-lookup"><span data-stu-id="c4192-133">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="c4192-134">Web API クライアント ライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c4192-134">Install the Web API Client Libraries</span></span>

<span data-ttu-id="c4192-135">NuGet パッケージ マネージャーを使用して、Web API クライアント ライブラリのパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c4192-135">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="c4192-136">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]** > **[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="c4192-136">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="c4192-137">パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="c4192-137">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="c4192-138">上記のコマンドは、プロジェクトに次の NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="c4192-138">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="c4192-139">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="c4192-139">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="c4192-140">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="c4192-140">Newtonsoft.Json</span></span>

<span data-ttu-id="c4192-141">Json.NET は、.NET 用の人気のある高パフォーマンス JSON フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="c4192-141">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="c4192-142">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="c4192-142">Add a Model Class</span></span>

<span data-ttu-id="c4192-143">確認、`Product`クラス。</span><span class="sxs-lookup"><span data-stu-id="c4192-143">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="c4192-144">このクラスでは、web API によって使用されるデータ モデルと一致します。</span><span class="sxs-lookup"><span data-stu-id="c4192-144">This class matches the data model used by the web API.</span></span> <span data-ttu-id="c4192-145">アプリは **HttpClient** を使用して HTTP レスポンスから `Product` インスタンスを読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="c4192-145">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="c4192-146">アプリで逆シリアル化コードを記述する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c4192-146">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="c4192-147">作成し、HttpClient の初期化</span><span class="sxs-lookup"><span data-stu-id="c4192-147">Create and Initialize HttpClient</span></span>

<span data-ttu-id="c4192-148">静的なを調べる**HttpClient**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c4192-148">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="c4192-149">**HttpClient**は 1 回インスタンス化するためのものであり、アプリケーションの有効期間にわたって再利用します。</span><span class="sxs-lookup"><span data-stu-id="c4192-149">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="c4192-150">次の条件により**SocketException**エラー。</span><span class="sxs-lookup"><span data-stu-id="c4192-150">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="c4192-151">新しいを作成する**HttpClient**要求ごとのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="c4192-151">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="c4192-152">負荷の高いサーバー。</span><span class="sxs-lookup"><span data-stu-id="c4192-152">Server under heavy load.</span></span>

<span data-ttu-id="c4192-153">新しいを作成する**HttpClient**要求ごとにインスタンスが使用可能なソケットを使い果たしてしまうことができます。</span><span class="sxs-lookup"><span data-stu-id="c4192-153">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="c4192-154">次のコードを初期化します、 **HttpClient**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="c4192-154">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="c4192-155">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="c4192-155">The preceding code:</span></span>

* <span data-ttu-id="c4192-156">HTTP 要求のベース URI を設定します。</span><span class="sxs-lookup"><span data-stu-id="c4192-156">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="c4192-157">サーバー アプリで使用されるポートにポート番号を変更します。</span><span class="sxs-lookup"><span data-stu-id="c4192-157">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="c4192-158">サーバー アプリ用にポートを使用しない場合、アプリは動作しません。</span><span class="sxs-lookup"><span data-stu-id="c4192-158">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="c4192-159">"Application/json"に Accept ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="c4192-159">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="c4192-160">このヘッダーを設定するには、JSON 形式でデータを送信するサーバーがように指示します。</span><span class="sxs-lookup"><span data-stu-id="c4192-160">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="c4192-161">リソースを取得する GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4192-161">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="c4192-162">次のコードでは、製品の GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4192-162">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="c4192-163">**GetAsync**メソッドは、HTTP GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4192-163">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="c4192-164">ときに、メソッドが完了したら、それを返します、 **HttpResponseMessage** HTTP 応答を格納しています。</span><span class="sxs-lookup"><span data-stu-id="c4192-164">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="c4192-165">応答にステータス コードが成功コードの場合は、応答本文には、製品の JSON 表現が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c4192-165">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="c4192-166">呼び出す**ReadAsAsync** JSON ペイロードを逆シリアル化する、`Product`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="c4192-166">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="c4192-167">**ReadAsAsync**応答本文は、任意の大きさができるため、メソッドは非同期です。</span><span class="sxs-lookup"><span data-stu-id="c4192-167">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="c4192-168">**HttpClient** HTTP 応答には、エラー コードが含まれている場合、例外はスローされません。</span><span class="sxs-lookup"><span data-stu-id="c4192-168">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="c4192-169">代わりに、 **IsSuccessStatusCode**プロパティは**false**状態がエラー コードの場合。</span><span class="sxs-lookup"><span data-stu-id="c4192-169">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="c4192-170">HTTP エラー コードを例外として処理する場合は、呼び出す[HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx)応答オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c4192-170">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="c4192-171">`EnsureSuccessStatusCode` 状態コードが 200 の範囲外になった場合に例外をスローします&ndash;299 します。</span><span class="sxs-lookup"><span data-stu-id="c4192-171">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="c4192-172">なお**HttpClient**の他の理由から例外をスローすることができます&mdash;の例では、要求がタイムアウトします。</span><span class="sxs-lookup"><span data-stu-id="c4192-172">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="c4192-173">逆シリアル化するメディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="c4192-173">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="c4192-174">ときに**ReadAsAsync**と呼ばれるは、パラメーターなしで、既定のセットを使用して*メディア フォーマッタ*応答本体を読み取る。</span><span class="sxs-lookup"><span data-stu-id="c4192-174">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="c4192-175">既定のフォーマッタは、JSON、XML、およびフォームの url エンコード データをサポートします。</span><span class="sxs-lookup"><span data-stu-id="c4192-175">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="c4192-176">既定のフォーマッタを使用する代わりに、フォーマッタの一覧を行うことができます、 **ReadAsAsync**メソッド。</span><span class="sxs-lookup"><span data-stu-id="c4192-176">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="c4192-177">フォーマッタの一覧を使用しては、カスタム メディアの種類のフォーマッタがある場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="c4192-177">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="c4192-178">詳細については、次を参照してください[ASP.NET Web API 2 のメディア フォーマッタ。](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="c4192-178">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="c4192-179">リソースを作成する POST 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4192-179">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="c4192-180">次のコードを含む POST 要求を送信する、 `Product` JSON 形式でインスタンス。</span><span class="sxs-lookup"><span data-stu-id="c4192-180">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="c4192-181">**PostAsJsonAsync**メソッド。</span><span class="sxs-lookup"><span data-stu-id="c4192-181">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="c4192-182">オブジェクトを JSON にシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="c4192-182">Serializes an object to JSON.</span></span>
* <span data-ttu-id="c4192-183">POST 要求では、JSON ペイロードを送信します。</span><span class="sxs-lookup"><span data-stu-id="c4192-183">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="c4192-184">要求が成功するとします。</span><span class="sxs-lookup"><span data-stu-id="c4192-184">If the request succeeds:</span></span>

* <span data-ttu-id="c4192-185">201 (Created) 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="c4192-185">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="c4192-186">応答は、Location ヘッダーで、作成したリソースの URL を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4192-186">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="c4192-187">リソースを更新する PUT 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4192-187">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="c4192-188">次のコードでは、製品を更新する PUT 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4192-188">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="c4192-189">**PutAsJsonAsync**メソッドのように機能**PostAsJsonAsync**POST の代わりに PUT 要求を送信する点を除いて、します。</span><span class="sxs-lookup"><span data-stu-id="c4192-189">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="c4192-190">リソースを削除する DELETE 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4192-190">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="c4192-191">次のコードでは、製品を削除する DELETE 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="c4192-191">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="c4192-192">など、GET、DELETE 要求に要求本文はありません。</span><span class="sxs-lookup"><span data-stu-id="c4192-192">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="c4192-193">DELETE で JSON または XML 形式を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c4192-193">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="c4192-194">サンプルをテストします。</span><span class="sxs-lookup"><span data-stu-id="c4192-194">Test the sample</span></span>

<span data-ttu-id="c4192-195">クライアント アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="c4192-195">To test the client app:</span></span>

1. <span data-ttu-id="c4192-196">[ダウンロード](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server)サーバー アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="c4192-196">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="c4192-197">[ダウンロードの方法はこちらをご覧ください。](/aspnet/core/tutorials/#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="c4192-197">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="c4192-198">サーバー アプリが動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="c4192-198">Verify the server app is working.</span></span> <span data-ttu-id="c4192-199">Exaxmple の`http://localhost:64195/api/products`製品の一覧を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="c4192-199">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="c4192-200">HTTP 要求のベース URI を設定します。</span><span class="sxs-lookup"><span data-stu-id="c4192-200">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="c4192-201">サーバー アプリで使用されるポートにポート番号を変更します。</span><span class="sxs-lookup"><span data-stu-id="c4192-201">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="c4192-202">クライアント アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="c4192-202">Run the client app.</span></span> <span data-ttu-id="c4192-203">次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="c4192-203">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
