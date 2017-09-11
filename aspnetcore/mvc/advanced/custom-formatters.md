---
title: "ASP.NET Core MVC web Api でカスタム フォーマッタ"
author: tdykstra
description: "作成して、web Api ASP.NET Core でのカスタム フォーマッタを使用する方法を説明します。"
keywords: "ASP.NET Core web api、カスタム フォーマッタ"
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 0285b40cfacb79745d3a6488401677130f55a95b
ms.sourcegitcommit: 6ece943781d8a56784bb6160f14da85210d3fcea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="16bd6-104">ASP.NET Core MVC web Api でカスタム フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="16bd6-104">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="16bd6-105">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="16bd6-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="16bd6-106">ASP.NET Core MVC には、JSON、XML、またはプレーン テキスト形式を使用してで web Api のデータ交換の組み込みサポートがあります。</span><span class="sxs-lookup"><span data-stu-id="16bd6-106">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="16bd6-107">この記事では、カスタム フォーマッタを作成することで追加の形式のサポートを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-107">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="16bd6-108">[GitHub からサンプルのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample)です。</span><span class="sxs-lookup"><span data-stu-id="16bd6-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="16bd6-109">カスタム フォーマッタを使用する場合</span><span class="sxs-lookup"><span data-stu-id="16bd6-109">When to use custom formatters</span></span>

<span data-ttu-id="16bd6-110">カスタム フォーマッタを使用すると、[コンテンツ ネゴシエーション](xref:mvc/models/formatting)組み込みフォーマッタ (JSON、XML、およびプレーン テキスト) でサポートされていないコンテンツの種類をサポートするために処理します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-110">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="16bd6-111">たとえば、一部の web API のクライアントを処理できる場合、 [Protobuf](https://github.com/google/protobuf)より効率的になっているために、それらのクライアントで Protobuf を使用するとする可能性があります形式です。</span><span class="sxs-lookup"><span data-stu-id="16bd6-111">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="16bd6-112">連絡先の名前とアドレスを送信する web API をすることもできます[vCard](https://en.wikipedia.org/wiki/VCard)形式、連絡先データをやり取りするための一般的に使用される形式です。</span><span class="sxs-lookup"><span data-stu-id="16bd6-112">Or you might want your web API to send contact names and addresses in [vCard](https://en.wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="16bd6-113">この記事の付属のサンプル アプリでは、単純な vCard フォーマッタを実装します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-113">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="16bd6-114">カスタム フォーマッタを使用する方法の概要</span><span class="sxs-lookup"><span data-stu-id="16bd6-114">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="16bd6-115">作成し、カスタム フォーマッタを使用する手順を次に示します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-115">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="16bd6-116">クライアントに送信するデータをシリアル化する場合は、出力フォーマッタ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-116">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="16bd6-117">クライアントから受信したデータを逆シリアル化する場合は、入力のフォーマッタ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-117">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="16bd6-118">フォーマッタのインスタンスを追加、`InputFormatters`と`OutputFormatters`コレクション[MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions)です。</span><span class="sxs-lookup"><span data-stu-id="16bd6-118">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="16bd6-119">次のセクションでは、これらの各手順のガイダンスと、コード例を提供します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-119">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="16bd6-120">カスタム フォーマッタ クラスを作成する方法</span><span class="sxs-lookup"><span data-stu-id="16bd6-120">How to create a custom formatter class</span></span>

<span data-ttu-id="16bd6-121">フォーマッタを作成します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-121">To create a formatter:</span></span>

* <span data-ttu-id="16bd6-122">適切な基本クラスからクラスを派生します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-122">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="16bd6-123">コンス トラクターでは、有効なメディアの種類とエンコーディングを指定します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-123">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="16bd6-124">オーバーライド`CanReadType` / `CanWriteType`メソッド</span><span class="sxs-lookup"><span data-stu-id="16bd6-124">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="16bd6-125">オーバーライド`ReadRequestBodyAsync` / `WriteResponseBodyAsync`メソッド</span><span class="sxs-lookup"><span data-stu-id="16bd6-125">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="16bd6-126">適切な基本クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-126">Derive from the appropriate base class</span></span>

<span data-ttu-id="16bd6-127">テキストのメディアの種類 (たとえば、vCard) から取得、 [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter)または[TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter)基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="16bd6-127">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

<span data-ttu-id="16bd6-128">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]</span><span class="sxs-lookup"><span data-stu-id="16bd6-128">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]</span></span>

<span data-ttu-id="16bd6-129">派生してバイナリ型の場合、 [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter)または[OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter)基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="16bd6-129">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="16bd6-130">有効なメディアの種類とエンコーディングを指定します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-130">Specify valid media types and encodings</span></span>

<span data-ttu-id="16bd6-131">、そのコンス トラクターを追加することで有効なメディアの種類とエンコーディングを指定、`SupportedMediaTypes`と`SupportedEncodings`コレクション。</span><span class="sxs-lookup"><span data-stu-id="16bd6-131">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

<span data-ttu-id="16bd6-132">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]</span><span class="sxs-lookup"><span data-stu-id="16bd6-132">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]</span></span>

> [!NOTE]  
> <span data-ttu-id="16bd6-133">フォーマッタ クラスでコンス トラクターの依存関係の挿入を行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="16bd6-133">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="16bd6-134">たとえば、コンス トラクターにロガーのパラメーターを追加することによって、ロガーを取得できません。</span><span class="sxs-lookup"><span data-stu-id="16bd6-134">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="16bd6-135">サービスにアクセスするには、メソッドに渡されるコンテキスト オブジェクトを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="16bd6-135">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="16bd6-136">コード例[下](#read-write)これを行う方法を示します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-136">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="16bd6-137">CanReadType/CanWriteType をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="16bd6-137">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="16bd6-138">逆シリアル化したり、オーバーライドすることからシリアル化の種類を指定、`CanReadType`または`CanWriteType`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="16bd6-138">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="16bd6-139">たとえば、のみできますからテキストを vCard を作成する、`Contact`型およびその逆です。</span><span class="sxs-lookup"><span data-stu-id="16bd6-139">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

<span data-ttu-id="16bd6-140">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]</span><span class="sxs-lookup"><span data-stu-id="16bd6-140">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]</span></span>

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="16bd6-141">CanWriteResult メソッド</span><span class="sxs-lookup"><span data-stu-id="16bd6-141">The CanWriteResult method</span></span>

<span data-ttu-id="16bd6-142">一部のシナリオでは、オーバーライドする必要がある`CanWriteResult`の代わりに`CanWriteType`です。</span><span class="sxs-lookup"><span data-stu-id="16bd6-142">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="16bd6-143">使用して`CanWriteResult`次の条件に該当する場合。</span><span class="sxs-lookup"><span data-stu-id="16bd6-143">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="16bd6-144">アクション メソッドでは、モデル クラスを返します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-144">Your action method returns a model class.</span></span>
  * <span data-ttu-id="16bd6-145">実行時に返される可能性がありますが、派生クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="16bd6-145">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="16bd6-146">クラスには、アクションが返した派生する実行時に把握する必要があります。</span><span class="sxs-lookup"><span data-stu-id="16bd6-146">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="16bd6-147">たとえば、アクション メソッドのシグネチャを返します、`Person`型であり、これが返す可能性があります、`Student`または`Instructor`から派生する型`Person`です。</span><span class="sxs-lookup"><span data-stu-id="16bd6-147">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="16bd6-148">場合は、フォーマッタのみを処理する`Student`、オブジェクトの型を調べて[オブジェクト](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object)に提供されたコンテキスト オブジェクトで、`CanWriteResult`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="16bd6-148">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="16bd6-149">使用する必要はないことに注意してください`CanWriteResult`アクション メソッドが返す場合`IActionResult`以外の場合は、`CanWriteType`メソッドはランタイムの型を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="16bd6-149">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="16bd6-150">ReadRequestBodyAsync/WriteResponseBodyAsync をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="16bd6-150">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="16bd6-151">逆シリアル化またはでシリアル化の実際の作業を行う`ReadRequestBodyAsync`または`WriteResponseBodyAsync`です。</span><span class="sxs-lookup"><span data-stu-id="16bd6-151">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="16bd6-152">次の例で強調表示された行は、(コンス トラクターのパラメーターから取得できません) 依存性の注入コンテナーからサービスを取得する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="16bd6-152">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

<span data-ttu-id="16bd6-153">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="16bd6-153">[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]</span></span>

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="16bd6-154">カスタム フォーマッタを使用する MVC を構成する方法</span><span class="sxs-lookup"><span data-stu-id="16bd6-154">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="16bd6-155">カスタム フォーマッタを使用するフォーマッタ クラスのインスタンスを追加、`InputFormatters`または`OutputFormatters`コレクション。</span><span class="sxs-lookup"><span data-stu-id="16bd6-155">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

<span data-ttu-id="16bd6-156">[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="16bd6-156">[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]</span></span>

<span data-ttu-id="16bd6-157">フォーマッタは、挿入した順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="16bd6-157">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="16bd6-158">最初の 1 つが優先されます。</span><span class="sxs-lookup"><span data-stu-id="16bd6-158">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="16bd6-159">次のステップ</span><span class="sxs-lookup"><span data-stu-id="16bd6-159">Next steps</span></span>

<span data-ttu-id="16bd6-160">参照してください、[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample)を実装する単純な vCard 入力と出力のフォーマッタ。</span><span class="sxs-lookup"><span data-stu-id="16bd6-160">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="16bd6-161">アプリケーションは、次の例のように見える Vcard を読み書き。</span><span class="sxs-lookup"><span data-stu-id="16bd6-161">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="16bd6-162">VCard 出力、アプリケーションを実行しを Get 要求と共に送信します Accept ヘッダー"テキスト/vcard"を表示する`http://localhost:63313/api/contacts/`(Visual Studio から実行している) 場合、または`http://localhost:5000/api/contacts/`(コマンドラインから実行している) 場合。</span><span class="sxs-lookup"><span data-stu-id="16bd6-162">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="16bd6-163">VCard をアドレス帳のメモリ内コレクションに追加するには、上記の例のように書式設定、本文にテキストを vCard およびコンテンツ タイプ ヘッダー"テキスト/vcard"とは、同じ URL に Post 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="16bd6-163">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
