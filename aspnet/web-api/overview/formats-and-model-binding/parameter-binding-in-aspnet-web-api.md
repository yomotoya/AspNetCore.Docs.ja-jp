---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: ASP.NET Web API のパラメーター バインド |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7519ad92334690817ae64b994762fd68e76ebc9e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377810"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="16ae2-102">ASP.NET Web API のパラメーター バインド</span><span class="sxs-lookup"><span data-stu-id="16ae2-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="16ae2-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="16ae2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="16ae2-104">呼ばれるプロセス パラメーターの値を設定する必要があります Web API コント ローラーのメソッドを呼び出す、*バインド*します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="16ae2-105">この記事では、Web API が、パラメーターをバインドする方法と、バインディング プロセスをカスタマイズする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="16ae2-106">既定では、Web API は、次の規則を使用して、パラメーターをバインドします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="16ae2-107">パラメーターが「単純な」型の場合、Web API は、URI から値を取得しようとします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="16ae2-108">単純な種類には、.NET[プリミティブ型](https://msdn.microsoft.com/library/system.type.isprimitive.aspx)(**int**、 **bool**、**二重**など)、plus **TimeSpan**、 **DateTime**、 **Guid**、 **decimal**、および**文字列**、 *plus*任意文字列から変換できる型コンバーターを使用して入力します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="16ae2-109">(詳細については後での型コンバーター。)</span><span class="sxs-lookup"><span data-stu-id="16ae2-109">(More about type converters later.)</span></span>
- <span data-ttu-id="16ae2-110">複合型、Web API では、メッセージ本文から値を読み取るしようとして、使用して、[メディア タイプ フォーマッタ](media-formatters.md)します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="16ae2-111">たとえば、一般的な Web API コント ローラー メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="16ae2-112">*Id*パラメーターは、&quot;単純&quot;型であるため、Web API は要求 URI の値を取得しようとしています。</span><span class="sxs-lookup"><span data-stu-id="16ae2-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="16ae2-113">*項目*パラメーターが、複雑な型、Web API は、メディアの種類のフォーマッタを使用して、要求本文から値を読み取るようにします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="16ae2-114">URI から値を取得するには、Web API は、ルート データと、URI クエリ文字列で検索します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="16ae2-115">ルーティング システムは、URI を解析し、ルートに一致する場合は、ルート データが設定されます。</span><span class="sxs-lookup"><span data-stu-id="16ae2-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="16ae2-116">詳細については、次を参照してください。[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="16ae2-117">この記事の残りの部分で、モデル バインディング プロセスをカスタマイズする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="16ae2-118">複合型は、ただし、可能であれば、メディア タイプ フォーマッタを使用してを考慮します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="16ae2-119">HTTP の重要な原則は、リソースは、コンテンツ ネゴシエーションを使用して、リソースの表現を指定する、メッセージの本文で送信されます。</span><span class="sxs-lookup"><span data-stu-id="16ae2-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="16ae2-120">メディア タイプ フォーマッタは、正確に、この目的用に設計されています。</span><span class="sxs-lookup"><span data-stu-id="16ae2-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="16ae2-121">[FromUri] を使用します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-121">Using [FromUri]</span></span>

<span data-ttu-id="16ae2-122">URI から複合型を読み取る Web API を強制的には、追加、 **[FromUri]** 属性をパラメーター。</span><span class="sxs-lookup"><span data-stu-id="16ae2-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="16ae2-123">次の例では、定義、`GeoPoint`と共に取得するコント ローラー メソッドの型、 `GeoPoint` URI から。</span><span class="sxs-lookup"><span data-stu-id="16ae2-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="16ae2-124">クライアントは、クエリ文字列で、緯度と経度の値を配置することができ、Web API を構築を使用してそれらが、`GeoPoint`します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="16ae2-125">例えば:</span><span class="sxs-lookup"><span data-stu-id="16ae2-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="16ae2-126">[FromBody] を使用します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-126">Using [FromBody]</span></span>

<span data-ttu-id="16ae2-127">要求本文から単純型を読み取る Web API を強制的には、追加、 **[FromBody]** パラメーターに属性します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="16ae2-128">この例では、Web API は、の値を読み取るメディア タイプ フォーマッタを使用が*名前*要求本文から。</span><span class="sxs-lookup"><span data-stu-id="16ae2-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="16ae2-129">クライアント要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="16ae2-130">パラメーターでは、[FromBody] が持つ、Web API は、Content-type ヘッダーを使用して、フォーマッタを選択します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="16ae2-131">この例では、コンテンツの種類は&quot;、application/json&quot;要求本文で、未加工の JSON 文字列 (JSON オブジェクトではありません)。</span><span class="sxs-lookup"><span data-stu-id="16ae2-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="16ae2-132">メッセージ本文から読み取る最大で 1 つのパラメーターが許可されます。</span><span class="sxs-lookup"><span data-stu-id="16ae2-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="16ae2-133">したがってこれは機能しません。</span><span class="sxs-lookup"><span data-stu-id="16ae2-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="16ae2-134">このルールの理由では、1 回だけ読み取り可能な非バッファー ストリームで、要求本文を格納する可能性があること。</span><span class="sxs-lookup"><span data-stu-id="16ae2-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="16ae2-135">型コンバーター</span><span class="sxs-lookup"><span data-stu-id="16ae2-135">Type Converters</span></span>

<span data-ttu-id="16ae2-136">(Web API は、URI からバインドを試みます) するために、単純型としてクラスを扱う Web API を作成して行うことができます、 **TypeConverter**と文字列の変換を提供します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="16ae2-137">次のコードは、 `GeoPoint` 、地理的ポイントを表すクラスと**TypeConverter**の文字列に変換する`GeoPoint`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="16ae2-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="16ae2-138">`GeoPoint`クラスがで修飾された、 **[TypeConverter]** 属性を型コンバーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="16ae2-139">(この例は、Mike Stall のブログの投稿によって考え出されました[MVC/WebAPI のアクション シグネチャでのカスタム オブジェクトにバインドする方法](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx))。</span><span class="sxs-lookup"><span data-stu-id="16ae2-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="16ae2-140">Web API を扱うようになりました`GeoPoint`として単純型、つまりバインドを試みます`GeoPoint`URI からのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="16ae2-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="16ae2-141">含める必要はありません **[FromUri]** パラメーターにします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="16ae2-142">クライアントは、このような URI を持つメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="16ae2-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="16ae2-143">モデル バインダー</span><span class="sxs-lookup"><span data-stu-id="16ae2-143">Model Binders</span></span>

<span data-ttu-id="16ae2-144">型コンバーターよりもより柔軟なオプションでは、カスタム モデル バインダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="16ae2-145">モデル バインダーを使用ルート データからなど、HTTP 要求、アクションの説明および生の値にアクセス権があります。</span><span class="sxs-lookup"><span data-stu-id="16ae2-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="16ae2-146">モデル バインダーを作成するには、実装、 **IModelBinder**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="16ae2-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="16ae2-147">このインターフェイスは、1 つのメソッドを定義します**BindModel**:。</span><span class="sxs-lookup"><span data-stu-id="16ae2-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="16ae2-148">ここでは、モデル バインダーを`GeoPoint`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="16ae2-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="16ae2-149">モデル バインダーからの生の入力値を取得する、*値プロバイダー*します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="16ae2-150">この設計では、2 つの異なる機能を分離します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="16ae2-151">値プロバイダーは、HTTP 要求を受け取り、キー/値ペアのディクショナリを設定します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="16ae2-152">モデル バインダーでは、このディクショナリを使用してモデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="16ae2-153">Web API の既定値のプロバイダーは、ルート データとクエリ文字列から値を取得します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="16ae2-154">たとえば、URI は、 `http://localhost/api/values/1?location=48,-122`、値プロバイダーが次のキー/値ペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="16ae2-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="16ae2-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="16ae2-156">場所 = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="16ae2-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="16ae2-157">(これは既定のルート テンプレートを前提に話&quot;api/{controller}/{id}&quot;)。</span><span class="sxs-lookup"><span data-stu-id="16ae2-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="16ae2-158">バインドするパラメーターの名前が格納されている、 **ModelBindingContext.ModelName**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="16ae2-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="16ae2-159">モデル バインダー ディクショナリ内でこの値を持つキーを探します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="16ae2-160">値が存在しに変換できる場合、 `GeoPoint`、モデル バインダーにバインドされている値を割り当てます、 **ModelBindingContext.Model**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="16ae2-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="16ae2-161">モデル バインダーは単純型の変換に限定されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="16ae2-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="16ae2-162">この例では、モデル バインダーは、既知の場所のテーブルで最初にし、型変換を使用して失敗した場合。</span><span class="sxs-lookup"><span data-stu-id="16ae2-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="16ae2-163">**モデル バインダーの設定**</span><span class="sxs-lookup"><span data-stu-id="16ae2-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="16ae2-164">モデル バインダーを設定するいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="16ae2-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="16ae2-165">最初に、追加、 **[拡大する ModelBinder]** 属性をパラメーター。</span><span class="sxs-lookup"><span data-stu-id="16ae2-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="16ae2-166">追加することも、 **[拡大する ModelBinder]** 属性を型にします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="16ae2-167">Web API は、その型のすべてのパラメーターの指定したモデル バインダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="16ae2-168">最後にモデル バインダー プロバイダーを追加することができます、 **HttpConfiguration**します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="16ae2-169">モデル バインダー プロバイダーは、単に、モデル バインダーを作成するファクトリ クラスです。</span><span class="sxs-lookup"><span data-stu-id="16ae2-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="16ae2-170">派生することによって、プロバイダーを作成することができます、 [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx)クラス。</span><span class="sxs-lookup"><span data-stu-id="16ae2-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="16ae2-171">ただし、モデル バインダーは、1 つの型を処理する場合は、組み込みの使いやすい**SimpleModelBinderProvider**、これはこの目的に設計されています。</span><span class="sxs-lookup"><span data-stu-id="16ae2-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="16ae2-172">この方法を次のコードに示します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="16ae2-173">モデル バインド プロバイダーでは、使用する必要がありますを追加、 **[拡大する ModelBinder]** 属性をモデル バインダーとメディア タイプ フォーマッタではないのどちらを使用することを Web API に指示する、パラメーター。</span><span class="sxs-lookup"><span data-stu-id="16ae2-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="16ae2-174">これで、属性でモデル バインダーの型を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="16ae2-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="16ae2-175">値プロバイダー</span><span class="sxs-lookup"><span data-stu-id="16ae2-175">Value Providers</span></span>

<span data-ttu-id="16ae2-176">モデル バインダーが値プロバイダーから値を取得することをお話しします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="16ae2-177">カスタム値プロバイダーを作成するには、実装、 **IValueProvider**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="16ae2-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="16ae2-178">要求の cookie の値を取得する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="16ae2-179">派生することによって、値プロバイダー ファクトリを作成する必要も、 **ValueProviderFactory**クラス。</span><span class="sxs-lookup"><span data-stu-id="16ae2-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="16ae2-180">追加する値プロバイダー ファクトリ、 **HttpConfiguration**次のようにします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="16ae2-181">Web API がすべて、値プロバイダーのために合成モデル バインダーを呼び出すと**ValueProvider.GetValue**、モデル バインダーは、生成できる最初の値プロバイダーから値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="16ae2-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="16ae2-182">使用して、パラメーターのレベルで、値プロバイダー ファクトリを設定することができます、 **ValueProvider**属性は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="16ae2-183">これは、Web API で、指定した値プロバイダー ファクトリ モデル バインドを使用して、その他の登録済みの値プロバイダーを使用しないことを示します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="16ae2-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="16ae2-184">HttpParameterBinding</span></span>

<span data-ttu-id="16ae2-185">モデル バインダーは、特定のインスタンスの全般的なメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="16ae2-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="16ae2-186">確認する場合、 **[拡大する ModelBinder]** 属性が表示されます、抽象型から派生すること**ParameterBindingAttribute**クラス。</span><span class="sxs-lookup"><span data-stu-id="16ae2-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="16ae2-187">このクラスは、1 つのメソッドを定義します**GetBinding**、返された、 **HttpParameterBinding**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="16ae2-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="16ae2-188">**HttpParameterBinding**パラメーターを値にバインドを担当します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="16ae2-189">場合に **[拡大する ModelBinder]**、属性を返します、 **HttpParameterBinding**を使用して実装する**IModelBinder**実際のバインドを実行します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="16ae2-190">独自に実装することも**HttpParameterBinding**します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="16ae2-191">たとえばから Etag を取得する`if-match`と`if-none-match`要求のヘッダー。</span><span class="sxs-lookup"><span data-stu-id="16ae2-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="16ae2-192">Etag を表すクラスの定義から始めます。</span><span class="sxs-lookup"><span data-stu-id="16ae2-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="16ae2-193">ETag を取得するかどうかを示す列挙体も定義します、`if-match`ヘッダーまたは`if-none-match`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="16ae2-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="16ae2-194">ここでは、 **HttpParameterBinding** ETag 型のパラメーターにバインドされますを目的のヘッダーから ETag を取得します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="16ae2-195">**ExecuteBindingAsync**メソッドは、バインドします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="16ae2-196">このメソッド内にバインドされたパラメーター値を追加、 **ActionArgument**ディクショナリで、 **HttpActionContext**します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="16ae2-197">場合、 **ExecuteBindingAsync**メソッドは、要求メッセージの本文を読み取り、上書き、 **WillReadBody**プロパティが true を返します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="16ae2-198">要求本文には、読み取りしか実行 1 回、Web API 規則を適用する 1 つだけにバインドするようにするバッファリングされていないストリームは、メッセージ本文を読み取ることができます可能性があります。</span><span class="sxs-lookup"><span data-stu-id="16ae2-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="16ae2-199">カスタムを適用する**HttpParameterBinding**から派生した属性を定義する**ParameterBindingAttribute**します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="16ae2-200">`ETagParameterBinding`、1 つずつ、2 つの属性を定義します`if-match`ヘッダーと 1 つずつ`if-none-match`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="16ae2-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="16ae2-201">抽象基本クラスから派生させます。</span><span class="sxs-lookup"><span data-stu-id="16ae2-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="16ae2-202">使用するコント ローラー メソッドを次に示します、`[IfNoneMatch]`属性。</span><span class="sxs-lookup"><span data-stu-id="16ae2-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="16ae2-203">それに**ParameterBindingAttribute**、カスタムを追加するための別のフック**HttpParameterBinding**します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="16ae2-204">**HttpConfiguration**オブジェクト、 **ParameterBindingRules**プロパティは、型の anomymous 関数のコレクション (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**)。</span><span class="sxs-lookup"><span data-stu-id="16ae2-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="16ae2-205">たとえば、ETag のパラメーターを GET メソッドを使用するルールを追加できます`ETagParameterBinding`で`if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="16ae2-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="16ae2-206">関数が返す必要があります`null`のパラメーターのバインドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="16ae2-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="16ae2-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="16ae2-207">IActionValueBinder</span></span>

<span data-ttu-id="16ae2-208">パラメーター バインディング プロセス全体が、プラグ可能なサービスによって制御される**IActionValueBinder**します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="16ae2-209">既定の実装**IActionValueBinder**は次の処理します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="16ae2-210">探して、 **ParameterBindingAttribute**パラメーターにします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="16ae2-211">これが含まれています **[FromBody]**、 **[FromUri]**、および **[拡大する ModelBinder]**、またはカスタム属性。</span><span class="sxs-lookup"><span data-stu-id="16ae2-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="16ae2-212">それ以外の場合、ファイルの場所**HttpConfiguration.ParameterBindingRules**を null 以外を返す関数の**HttpParameterBinding**します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="16ae2-213">それ以外の場合、先ほど説明した既定の規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="16ae2-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="16ae2-214">パラメーターの型が「簡単」した場合、または型コンバーターが場合は、URI からバインドします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="16ae2-215">これは配置することに相当、 **[FromUri]** パラメーターの属性。</span><span class="sxs-lookup"><span data-stu-id="16ae2-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="16ae2-216">それ以外の場合、メッセージ本文からパラメーターの読み取りを再試行してください。</span><span class="sxs-lookup"><span data-stu-id="16ae2-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="16ae2-217">これは配置することに相当 **[FromBody]** パラメーターにします。</span><span class="sxs-lookup"><span data-stu-id="16ae2-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="16ae2-218">全体を置き換えることができる場合は、 **IActionValueBinder**カスタム実装サービス。</span><span class="sxs-lookup"><span data-stu-id="16ae2-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16ae2-219">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="16ae2-219">Additional Resources</span></span>

[<span data-ttu-id="16ae2-220">カスタム パラメーター バインドのサンプル</span><span class="sxs-lookup"><span data-stu-id="16ae2-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="16ae2-221">Mike Stall は、Web API のパラメーター バインディングの適切な一連のブログを作成しました。</span><span class="sxs-lookup"><span data-stu-id="16ae2-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="16ae2-222">どのように Web API は、パラメーターのバインド</span><span class="sxs-lookup"><span data-stu-id="16ae2-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="16ae2-223">Web API の MVC スタイルのパラメーターのバインド</span><span class="sxs-lookup"><span data-stu-id="16ae2-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="16ae2-224">MVC または Web API でのアクション シグネチャでのカスタム オブジェクトにバインドする方法</span><span class="sxs-lookup"><span data-stu-id="16ae2-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="16ae2-225">Web API でカスタム値プロバイダーを作成する方法</span><span class="sxs-lookup"><span data-stu-id="16ae2-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="16ae2-226">内部的には web API パラメーターのバインド</span><span class="sxs-lookup"><span data-stu-id="16ae2-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
