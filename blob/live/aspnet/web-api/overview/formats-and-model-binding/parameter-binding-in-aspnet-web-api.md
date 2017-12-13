---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "ASP.NET Web API でのバインディング パラメーター |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ad052570fb2f168da657cd1263d8342a59d4cab0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="1a2c7-102">ASP.NET Web API でのバインディング パラメーター</span><span class="sxs-lookup"><span data-stu-id="1a2c7-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="1a2c7-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1a2c7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1a2c7-104">呼ばれるプロセスのパラメーターの値を設定する必要があります Web API は、コント ローラーのメソッドを呼び出す、*バインディング*です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="1a2c7-105">この記事では、Web API が、パラメーターをバインドする方法と、バインディング プロセスをカスタマイズする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="1a2c7-106">既定では、Web API は、次の規則を使用して、パラメーターをバインドします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="1a2c7-107">パラメーターが「単純」型の場合は、Web API は、URI から値を取得しようとします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="1a2c7-108">単純型は、.NET[プリミティブ型](https://msdn.microsoft.com/en-us/library/system.type.isprimitive.aspx)(**int**、 **bool**、**二重**など)、および**TimeSpan**、 **DateTime**、 **Guid**、 **decimal**、および**文字列**、 *plus*任意文字列から変換できる型コンバーターで入力します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/en-us/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="1a2c7-109">(後の型コンバーターの詳細です。)</span><span class="sxs-lookup"><span data-stu-id="1a2c7-109">(More about type converters later.)</span></span>
- <span data-ttu-id="1a2c7-110">メッセージ本文から値を読み取るしようとすると、複合型、Web API を使用して、[メディア タイプ フォーマッタ](media-formatters.md)です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="1a2c7-111">たとえば、典型的な Web API コント ローラーのメソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="1a2c7-112">*Id*パラメーターは、&quot;単純&quot;型であるため、Web API は、要求の URI から値を取得しようとしています。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="1a2c7-113">*項目*パラメーターには Web API は、メディア タイプ フォーマッタを使用して、要求本文から値を読み取るために、複合型。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="1a2c7-114">URI から値を取得するには、Web API は、ルート データと URI クエリ文字列で検索します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="1a2c7-115">ルーティングのシステムは、URI を解析しをルートと一致するルート データは設定されます。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="1a2c7-116">詳細については、次を参照してください。[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="1a2c7-117">この記事の残りの部分で、モデル バインディング プロセスをカスタマイズする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="1a2c7-118">複合型のただし、可能な限り、メディア タイプ フォーマッタを使用を検討します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="1a2c7-119">HTTP の重要な原則は、リソースは、コンテンツ ネゴシエーションを使用して、リソースの表現を指定する、メッセージの本文で送信されます。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="1a2c7-120">メディア タイプ フォーマッタは、この目的だけのために設計されました。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="1a2c7-121">[FromUri] を使用します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-121">Using [FromUri]</span></span>

<span data-ttu-id="1a2c7-122">Web API を URI から複合型を読み取るには、追加、 **[FromUri]**属性をパラメーター。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="1a2c7-123">次の例では定義、`GeoPoint`型を取得するコント ローラー メソッドと共に、 `GeoPoint` URI からです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="1a2c7-124">クライアントは、クエリ文字列に緯度と経度の値を配置することができ、Web API が構築するために使用されます、`GeoPoint`です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="1a2c7-125">例:</span><span class="sxs-lookup"><span data-stu-id="1a2c7-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="1a2c7-126">[FromBody] を使用します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-126">Using [FromBody]</span></span>

<span data-ttu-id="1a2c7-127">要求本文から単純型の読み取りに Web API には、追加、 **[FromBody]**パラメーターに属性します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="1a2c7-128">この例では、Web API は使用して、メディア タイプ フォーマッタの値を読み取る*名前*要求の本体からです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="1a2c7-129">クライアント要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="1a2c7-130">パラメーターでは、[FromBody] が持つ、Web API は、Content-type ヘッダーを使用して、フォーマッタを選択します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="1a2c7-131">コンテンツの種類は、この例では&quot;アプリケーション/json&quot;要求本文は、生の JSON 文字列 (JSON オブジェクトではありません)。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="1a2c7-132">最大で 1 つのパラメーターは、メッセージ本文からの読み取りが許可されます。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="1a2c7-133">したがってこれは機能しません。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="1a2c7-134">このルールの理由は、された要求本文を 1 回のみ読み取ることができます、バッファリングされないストリームに保存する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="1a2c7-135">型コンバーター</span><span class="sxs-lookup"><span data-stu-id="1a2c7-135">Type Converters</span></span>

<span data-ttu-id="1a2c7-136">作成することで Web API (Web API は、URI からバインドしようとしています) できるように、単純型としてクラスを処理を行うことができます、 **TypeConverter**し、文字列の変換を提供します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="1a2c7-137">次のコードは、 `GeoPoint` 、地理的なポイントを表すクラスと**TypeConverter**を文字列から変換する`GeoPoint`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="1a2c7-138">`GeoPoint`でクラスを装飾、 **[TypeConverter]**属性を型コンバーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="1a2c7-139">(この例は、Mike Stall のブログの投稿で触発されました[MVC/WebAPI のアクション シグネチャでのカスタム オブジェクトをバインドする方法](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx))。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="1a2c7-140">これで、Web API は扱います`GeoPoint`として単純型、つまりバインドを試みます`GeoPoint`URI からのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="1a2c7-141">含める必要はありません**[FromUri]**パラメーターにします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="1a2c7-142">クライアントは、次のような URI を持つメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="1a2c7-143">モデル バインダー</span><span class="sxs-lookup"><span data-stu-id="1a2c7-143">Model Binders</span></span>

<span data-ttu-id="1a2c7-144">実行する型コンバーターよりもより柔軟なオプションでは、カスタム モデル バインダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="1a2c7-145">モデル バインダーをルート データからなど、HTTP 要求、アクションの説明、および生の値にアクセス権があります。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="1a2c7-146">モデル バインダーを作成するには、実装、 **IModelBinder**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="1a2c7-147">このインターフェイスは、1 つのメソッドを定義**BindModel**:</span><span class="sxs-lookup"><span data-stu-id="1a2c7-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="1a2c7-148">ここでは、モデル バインダーを`GeoPoint`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="1a2c7-149">モデル バインダーから生の入力値を取得する、*値プロバイダー*です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="1a2c7-150">この設計は、2 つの異なる関数を区切ります。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="1a2c7-151">値プロバイダーは、HTTP 要求を受け取りし、キー/値ペアのディクショナリを設定します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="1a2c7-152">モデル バインダーでは、このディクショナリを使用してモデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="1a2c7-153">Web API の既定値のプロバイダーは、ルート データおよびクエリ文字列から値を取得します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="1a2c7-154">たとえば、次の URI が`http://localhost/api/values/1?location=48,-122`、値プロバイダーは、次のキー/値ペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="1a2c7-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="1a2c7-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="1a2c7-156">場所 = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="1a2c7-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="1a2c7-157">(これは既定のルート テンプレートを想定して&quot;api/{controller}/{id}&quot;)。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="1a2c7-158">バインドするパラメーターの名前が格納されている、 **ModelBindingContext.ModelName**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="1a2c7-159">モデル バインダー ディクショナリ内でこの値を持つキーを探します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="1a2c7-160">値が存在しに変換できる場合、 `GeoPoint`、モデル バインダーにバインドされた値が割り当てられます、 **ModelBindingContext.Model**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="1a2c7-161">モデル バインダーの単純型の変換に制限がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="1a2c7-162">この例ではモデル バインダーは、既知の場所のテーブル内で最初に検索し、型の変換を使用して、失敗した場合。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="1a2c7-163">**モデル バインダーを設定**</span><span class="sxs-lookup"><span data-stu-id="1a2c7-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="1a2c7-164">モデル バインダーを設定するいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="1a2c7-165">最初に、追加、 **[拡大する ModelBinder]**属性をパラメーター。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="1a2c7-166">追加することも、 **[拡大する ModelBinder]**属性を型にします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="1a2c7-167">Web API は、その型のすべてのパラメーターの指定したモデル バインダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="1a2c7-168">最後にモデル バインダー プロバイダーを追加することができます、 **HttpConfiguration**です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="1a2c7-169">モデル バインダー プロバイダーは、単に、モデル バインダーを作成するファクトリ クラスです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="1a2c7-170">派生することで、プロバイダーを作成することができます、 [ModelBinderProvider](https://msdn.microsoft.com/en-us/library/system.web.http.modelbinding.modelbinderprovider.aspx)クラスです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/en-us/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="1a2c7-171">ただし場合、モデル バインダーは、1 つの型を処理することが容易に組み込みを使用して**SimpleModelBinderProvider**、この目的になっています。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="1a2c7-172">この方法を次のコードに示します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="1a2c7-173">モデル バインド プロバイダーを使用する必要がありますを追加、 **[拡大する ModelBinder]**属性をパラメーターへのモデル バインダーおよびメディア タイプ フォーマッタではないのどちらを使用するように Web API を通知します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="1a2c7-174">ただし、属性にモデル バインダーの型を指定する必要はありませんようになりました。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="1a2c7-175">値プロバイダー</span><span class="sxs-lookup"><span data-stu-id="1a2c7-175">Value Providers</span></span>

<span data-ttu-id="1a2c7-176">説明したようにモデル バインダーが値プロバイダーから値を取得します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="1a2c7-177">カスタム値プロバイダーを記述するには、実装、 **IValueProvider**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="1a2c7-178">要求の cookie の値を取得する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="1a2c7-179">派生することで、値プロバイダー ファクトリを作成する必要があります、 **ValueProviderFactory**クラスです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="1a2c7-180">追加する値プロバイダー ファクトリ、 **HttpConfiguration**次のようにします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="1a2c7-181">Web API がすべての値プロバイダーのために合成モデル バインダーを呼び出すと**ValueProvider.GetValue**、モデル バインダーは、それを生成できる最初の値プロバイダーから値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="1a2c7-182">使用して、パラメーター レベル値プロバイダー ファクトリを設定することができます、 **ValueProvider**属性が次のようにします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="1a2c7-183">これは、Web API で指定された値プロバイダー ファクトリ モデル バインディングを使用して、その他の登録済みの値プロバイダーを使用しないことを示しています。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="1a2c7-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="1a2c7-184">HttpParameterBinding</span></span>

<span data-ttu-id="1a2c7-185">モデル バインダーは、一般的なメカニズムの特定のインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="1a2c7-186">確認する場合、 **[拡大する ModelBinder]**属性が表示されます、抽象型から派生して**ParameterBindingAttribute**クラスです。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="1a2c7-187">このクラスは、1 つのメソッドを定義**GetBinding**、返された、 **HttpParameterBinding**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="1a2c7-188">**HttpParameterBinding**パラメーターの値にバインドを担当します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="1a2c7-189">場合、 **[拡大する ModelBinder]**、属性を返します、 **HttpParameterBinding**を使用して実装、 **IModelBinder**を実際のバインドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="1a2c7-190">独自に実装することも**HttpParameterBinding**です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="1a2c7-191">たとえばから Etag を取得する`if-match`と`if-none-match`要求のヘッダー。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="1a2c7-192">Etag を表すクラスを定義することで始めます。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="1a2c7-193">ETag を取得するかどうかを示すために列挙体を定義してありますも、`if-match`ヘッダーまたは`if-none-match`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="1a2c7-194">ここでは、 **HttpParameterBinding**を目的のヘッダーから ETag を取得し、ETag 型のパラメーターにバインドします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="1a2c7-195">**ExecuteBindingAsync**メソッドでは、バインドします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="1a2c7-196">このメソッド内にバインドされたパラメーター値を追加、 **ActionArgument**ディクショナリで、 **HttpActionContext**です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="1a2c7-197">場合、 **ExecuteBindingAsync**メソッドが要求メッセージの本文を読み取り、上書き、 **WillReadBody**プロパティが true を返します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="1a2c7-198">要求本文には、読み取り可能なのみ 1 回、Web API 規則を適用する 1 つのバインド、バッファリングされていないストリームは、メッセージ本文を読み取ることができます可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="1a2c7-199">カスタムを適用する**HttpParameterBinding**から派生する属性を定義する**ParameterBindingAttribute**です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="1a2c7-200">`ETagParameterBinding`、1 つずつ、2 つの属性を定義してあります`if-match`ヘッダーと 1 つずつ`if-none-match`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="1a2c7-201">抽象基本クラスから派生させます。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="1a2c7-202">ここでは、コント ローラー メソッドを使用する、`[IfNoneMatch]`属性。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="1a2c7-203">それに**ParameterBindingAttribute**、カスタムを追加するための別のフック**HttpParameterBinding**です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="1a2c7-204">**HttpConfiguration**オブジェクト、 **ParameterBindingRules**プロパティは、型の anomymous 関数のコレクション (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**)。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="1a2c7-205">たとえば、GET メソッド上の任意の ETag パラメーターを使用する規則を追加`ETagParameterBinding`で`if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="1a2c7-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="1a2c7-206">関数が返す必要があります`null`のパラメーターのバインドは適用されません。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="1a2c7-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="1a2c7-207">IActionValueBinder</span></span>

<span data-ttu-id="1a2c7-208">全体のパラメーター バインディング プロセスは、プラグ可能なサービスによって制御されます**IActionValueBinder**です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="1a2c7-209">既定の実装**IActionValueBinder**は次の実行します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="1a2c7-210">探して、 **ParameterBindingAttribute**パラメーターにします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="1a2c7-211">これが含まれます**[FromBody]**、 **[FromUri]**、および**[拡大する ModelBinder]**、またはカスタム属性です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="1a2c7-212">それ以外の場合、ファイルの場所**HttpConfiguration.ParameterBindingRules**を null 以外を返す関数の**HttpParameterBinding**です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="1a2c7-213">それ以外の場合、先ほど説明した既定の規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="1a2c7-214">パラメーターの型「単純」が、型コンバーターがいる場合は、URI からバインドします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="1a2c7-215">これは配置することに相当、 **[FromUri]**パラメーターの属性です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="1a2c7-216">それ以外の場合、メッセージ本文からパラメーターの読み取りを再試行してください。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="1a2c7-217">これは配置することに相当**[FromBody]**パラメーターにします。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="1a2c7-218">全体を置換できなかった場合は、 **IActionValueBinder**サービス、カスタム実装です。</span><span class="sxs-lookup"><span data-stu-id="1a2c7-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a2c7-219">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="1a2c7-219">Additional Resources</span></span>

[<span data-ttu-id="1a2c7-220">カスタム パラメーター バインディングのサンプル</span><span class="sxs-lookup"><span data-stu-id="1a2c7-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="1a2c7-221">Web API のパラメーター バインド詳細な一連のブログ投稿については、Mike Stall:</span><span class="sxs-lookup"><span data-stu-id="1a2c7-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="1a2c7-222">どのように Web API は、パラメーターのバインド</span><span class="sxs-lookup"><span data-stu-id="1a2c7-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="1a2c7-223">Web API の MVC スタイル パラメーターのバインド</span><span class="sxs-lookup"><span data-stu-id="1a2c7-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="1a2c7-224">MVC または Web API のアクション シグネチャでのカスタム オブジェクトをバインドする方法</span><span class="sxs-lookup"><span data-stu-id="1a2c7-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="1a2c7-225">Web API でカスタム値プロバイダーを作成する方法</span><span class="sxs-lookup"><span data-stu-id="1a2c7-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="1a2c7-226">内部で web API パラメーターのバインド</span><span class="sxs-lookup"><span data-stu-id="1a2c7-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
