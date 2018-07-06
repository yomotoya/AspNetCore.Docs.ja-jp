---
uid: web-api/overview/error-handling/exception-handling
title: ASP.NET Web API での例外処理 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807662"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="50533-102">ASP.NET Web API での例外処理</span><span class="sxs-lookup"><span data-stu-id="50533-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="50533-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="50533-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="50533-104">この記事では、エラーと ASP.NET Web API での例外処理について説明します。</span><span class="sxs-lookup"><span data-stu-id="50533-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="50533-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="50533-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="50533-106">例外フィルター</span><span class="sxs-lookup"><span data-stu-id="50533-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="50533-107">例外フィルターを登録します。</span><span class="sxs-lookup"><span data-stu-id="50533-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="50533-108">Http エラー</span><span class="sxs-lookup"><span data-stu-id="50533-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="50533-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="50533-109">HttpResponseException</span></span>

<span data-ttu-id="50533-110">Web API コント ローラーがキャッチされない例外をスローするとどうなりますか。</span><span class="sxs-lookup"><span data-stu-id="50533-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="50533-111">既定では、ほとんどの例外は、ステータス コード 500 内部サーバー エラーの HTTP 応答に変換されます。</span><span class="sxs-lookup"><span data-stu-id="50533-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="50533-112">**HttpResponseException**型が特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="50533-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="50533-113">この例外は、例外のコンス トラクターで指定した任意の HTTP ステータス コードを返します。</span><span class="sxs-lookup"><span data-stu-id="50533-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="50533-114">次のメソッドに見つからない、404 が返されます場合など、 *id*パラメーターが無効です。</span><span class="sxs-lookup"><span data-stu-id="50533-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="50533-115">応答をより細かく制御することができますも全体の応答メッセージの構築とで、 **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="50533-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="50533-116">例外フィルター</span><span class="sxs-lookup"><span data-stu-id="50533-116">Exception Filters</span></span>

<span data-ttu-id="50533-117">Web API が記述することで例外を処理する方法をカスタマイズすることができます、*例外フィルター*します。</span><span class="sxs-lookup"><span data-stu-id="50533-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="50533-118">コント ローラー メソッドがある未処理の例外をスローした場合、例外フィルターが実行される*いない*、 **HttpResponseException**例外。</span><span class="sxs-lookup"><span data-stu-id="50533-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="50533-119">**HttpResponseException** HTTP 応答を返すためには、具体的には設計されているので、型が特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="50533-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="50533-120">例外フィルターの実装、 **System.Web.Http.Filters.IExceptionFilter**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="50533-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="50533-121">例外フィルターを記述する最も簡単な方法がから派生するには、 **System.Web.Http.Filters.ExceptionFilterAttribute**クラスし、オーバーライド、 **OnException**メソッド。</span><span class="sxs-lookup"><span data-stu-id="50533-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="50533-122">ASP.NET Web API での例外フィルターは、ASP.NET mvc と似ています。</span><span class="sxs-lookup"><span data-stu-id="50533-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="50533-123">ただし、個別に、別の名前空間と関数で宣言されています。</span><span class="sxs-lookup"><span data-stu-id="50533-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="50533-124">具体的には、 **HandleErrorAttribute** MVC で使用されるクラスでは、Web API コント ローラーによってスローされた例外は処理されません。</span><span class="sxs-lookup"><span data-stu-id="50533-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="50533-125">ここでは、変換を行うフィルター **NotImplementedException**例外に HTTP 状態コード、501 Not Implemented:</span><span class="sxs-lookup"><span data-stu-id="50533-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="50533-126">**応答**のプロパティ、 **HttpActionExecutedContext**オブジェクトには、クライアントに送信される HTTP 応答メッセージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="50533-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="50533-127">例外フィルターを登録します。</span><span class="sxs-lookup"><span data-stu-id="50533-127">Registering Exception Filters</span></span>

<span data-ttu-id="50533-128">Web API 例外フィルターを登録するいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="50533-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="50533-129">アクションによって</span><span class="sxs-lookup"><span data-stu-id="50533-129">By action</span></span>
- <span data-ttu-id="50533-130">コント ローラーによって</span><span class="sxs-lookup"><span data-stu-id="50533-130">By controller</span></span>
- <span data-ttu-id="50533-131">グローバルに</span><span class="sxs-lookup"><span data-stu-id="50533-131">Globally</span></span>

<span data-ttu-id="50533-132">特定のアクションにフィルターを適用するには、アクションに属性として、フィルターを追加します。</span><span class="sxs-lookup"><span data-stu-id="50533-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="50533-133">すべてのコント ローラーのアクションをフィルターを適用するには、コント ローラー クラスに属性としてフィルターを追加します。</span><span class="sxs-lookup"><span data-stu-id="50533-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="50533-134">すべての Web API コント ローラーにグローバルにフィルターを適用するフィルターのインスタンスを追加、 **GlobalConfiguration.Configuration.Filters**コレクション。</span><span class="sxs-lookup"><span data-stu-id="50533-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="50533-135">このコレクション内の例外フィルターは、任意の Web API コント ローラー アクションに適用されます。</span><span class="sxs-lookup"><span data-stu-id="50533-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="50533-136">配置内で Web API の構成コードの「ASP.NET MVC 4 Web アプリケーション」プロジェクト テンプレートをプロジェクトの作成に使用する場合、`WebApiConfig`クラスは、アプリである\_開始フォルダー。</span><span class="sxs-lookup"><span data-stu-id="50533-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="50533-137">Http エラー</span><span class="sxs-lookup"><span data-stu-id="50533-137">HttpError</span></span>

<span data-ttu-id="50533-138">**HttpError**オブジェクトは、応答本文にエラー情報を返す一貫した方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="50533-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="50533-139">次の例では、HTTP 状態コード 404 (Not Found) を返す方法を示しますで、 **HttpError**応答の本文。</span><span class="sxs-lookup"><span data-stu-id="50533-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="50533-140">**CreateErrorResponse**拡張メソッドで定義されている、 **System.Net.Http.HttpRequestMessageExtensions**クラス。</span><span class="sxs-lookup"><span data-stu-id="50533-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="50533-141">内部的には、 **CreateErrorResponse**作成、 **HttpError**をインスタンス化し、作成、 **HttpResponseMessage**を格納している、 **httpエラー**.</span><span class="sxs-lookup"><span data-stu-id="50533-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="50533-142">この例でメソッドが成功した場合、製品、HTTP 応答で返します。</span><span class="sxs-lookup"><span data-stu-id="50533-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="50533-143">HTTP 応答に含まれる、要求された製品が存在しない場合は、 **HttpError**要求本文にします。</span><span class="sxs-lookup"><span data-stu-id="50533-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="50533-144">応答は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="50533-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="50533-145">注意、 **HttpError**がこの例では JSON にシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="50533-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="50533-146">使用する利点の 1 つ**HttpError**を同一になったことが[コンテンツ ネゴシエーション](../formats-and-model-binding/content-negotiation.md)およびシリアル化は、他の厳密に型指定されたモデルを処理します。</span><span class="sxs-lookup"><span data-stu-id="50533-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="50533-147">Http エラーとモデルの検証</span><span class="sxs-lookup"><span data-stu-id="50533-147">HttpError and Model Validation</span></span>

<span data-ttu-id="50533-148">モデルの検証のためには、モデルの状態を渡すことができます**CreateErrorResponse**応答に検証エラーを含める。</span><span class="sxs-lookup"><span data-stu-id="50533-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="50533-149">この例では、次の応答を返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="50533-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="50533-150">モデルの検証の詳細については、次を参照してください。 [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="50533-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="50533-151">HttpResponseException での http エラーの使用</span><span class="sxs-lookup"><span data-stu-id="50533-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="50533-152">前の例では、返されます、 **HttpResponseMessage** 、コント ローラー アクションからのメッセージを使用することも**HttpResponseException**を返す、 **HttpError**します。</span><span class="sxs-lookup"><span data-stu-id="50533-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="50533-153">これにより、まだを返すときに通常の成功の場合、厳密に型指定されたモデルを返すできます**HttpError**エラーがある場合。</span><span class="sxs-lookup"><span data-stu-id="50533-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
