---
title: ASP.NET Core Web API のカスタム フォーマッタ
author: rick-anderson
description: ASP.NET Core で Web API のカスタム フォーマッタを作成して使用する方法を説明します。
ms.author: tdykstra
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: a038cd9c05950333fce9e72f67d6721198fae4d3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206316"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="4ebf2-103">ASP.NET Core Web API のカスタム フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="4ebf2-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="4ebf2-104">著者: [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4ebf2-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4ebf2-105">ASP.NET Core MVC には、JSON、XML、プレーン テキスト形式を使用して、Web API でデータを交換するためのサポートが組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="4ebf2-106">この記事では、カスタム フォーマッタを作成して、追加形式のサポートを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="4ebf2-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="4ebf2-108">カスタム フォーマッタを使用するタイミング</span><span class="sxs-lookup"><span data-stu-id="4ebf2-108">When to use custom formatters</span></span>

<span data-ttu-id="4ebf2-109">組み込みフォーマッタ (JSON、XML、プレーン テキスト) でサポートされていないコンテンツの種類を[コンテンツ ネゴシエーション](xref:web-api/advanced/formatting#content-negotiation) プロセスでサポートする場合は、カスタム フォーマッタを使用します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-109">Use a custom formatter when you want the [content negotiation](xref:web-api/advanced/formatting#content-negotiation) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="4ebf2-110">たとえば、Web API の一部のクライアントが [Protobuf](https://github.com/google/protobuf) 形式を処理できる場合、より効率的であるため、それらのクライアントで Protobuf を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span> <span data-ttu-id="4ebf2-111">あるいは、Web API を使用して、[vCard](https://wikipedia.org/wiki/VCard) 形式 (連絡先データを交換する場合に一般的に使用される) で連絡先の名前とアドレスを送信することもできます。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="4ebf2-112">この記事で提供するサンプル アプリでは単純な vCard フォーマッタを実装します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="4ebf2-113">カスタム フォーマッタの使用方法の概要</span><span class="sxs-lookup"><span data-stu-id="4ebf2-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="4ebf2-114">カスタム フォーマッタを作成して使用する手順を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="4ebf2-115">クライアントに送信するデータをシリアル化する場合は、出力フォーマッタ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="4ebf2-116">クライアントから受信したデータを逆シリアル化する場合は、入力フォーマッタ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-116">Create an input formatter class if you want to deserialize data received from the client.</span></span>
* <span data-ttu-id="4ebf2-117">フォーマッタのインスタンスを [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions) の `InputFormatters` および `OutputFormatters` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="4ebf2-118">次のセクションでは、これらの各手順のガイダンスとコード例を提供します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="4ebf2-119">カスタム フォーマッタ クラスの作成方法</span><span class="sxs-lookup"><span data-stu-id="4ebf2-119">How to create a custom formatter class</span></span>

<span data-ttu-id="4ebf2-120">フォーマッタを作成する場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-120">To create a formatter:</span></span>

* <span data-ttu-id="4ebf2-121">クラスを適切な基底クラスから派生させます。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="4ebf2-122">コンストラクターで有効なメディアの種類とエンコーディングを指定します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="4ebf2-123">`CanReadType`/`CanWriteType` メソッドのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="4ebf2-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="4ebf2-124">`ReadRequestBodyAsync`/`WriteResponseBodyAsync` メソッドのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="4ebf2-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="4ebf2-125">適切な基底クラスからの派生</span><span class="sxs-lookup"><span data-stu-id="4ebf2-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="4ebf2-126">メディアの種類がテキスト (vCard など) の場合は、[TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) または [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) 基底クラスから派生させます。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-126">For text media types (for example, vCard), derive from the [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="4ebf2-127">種類がバイナリである場合は、[InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) または [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) 基底クラスから派生させます。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-127">For binary types, derive from the [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="4ebf2-128">有効なメディアの種類とエンコーディングの指定</span><span class="sxs-lookup"><span data-stu-id="4ebf2-128">Specify valid media types and encodings</span></span>

<span data-ttu-id="4ebf2-129">コンストラクターで、`SupportedMediaTypes` および `SupportedEncodings` コレクションに追加して、有効なメディアの種類とエンコーディングを指定します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-129">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]
> <span data-ttu-id="4ebf2-130">フォーマッタ クラスでコンストラクターの依存関係の挿入を行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-130">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="4ebf2-131">たとえば、コンストラクターにロガー パラメーターを追加して、ロガーを取得することはできません。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-131">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="4ebf2-132">サービスにアクセスするには、メソッドに渡されるコンテキスト オブジェクトを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-132">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="4ebf2-133">[以下](#read-write)のコード例でこの方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-133">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="4ebf2-134">CanReadType/CanWriteType のオーバーライド</span><span class="sxs-lookup"><span data-stu-id="4ebf2-134">Override CanReadType/CanWriteType</span></span>

<span data-ttu-id="4ebf2-135">`CanReadType` または `CanWriteType` メソッドをオーバーライドして、逆シリアル化またはシリアル化できる種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-135">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="4ebf2-136">たとえば、vCard テキストを `Contact` の種類からのみ作成できます (その逆も可)。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-136">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="4ebf2-137">CanWriteResult メソッド</span><span class="sxs-lookup"><span data-stu-id="4ebf2-137">The CanWriteResult method</span></span>

<span data-ttu-id="4ebf2-138">一部のシナリオでは、`CanWriteType` の代わりに `CanWriteResult` をオーバーライドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-138">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="4ebf2-139">次のすべての条件が満たされている場合は、`CanWriteResult` を使用します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-139">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="4ebf2-140">アクション メソッドはモデル クラスを返します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-140">Your action method returns a model class.</span></span>
* <span data-ttu-id="4ebf2-141">実行時に返される可能性がある派生クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-141">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="4ebf2-142">アクションが返した派生クラスを実行時に把握する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-142">You need to know at runtime which derived class was returned by the action.</span></span>

<span data-ttu-id="4ebf2-143">たとえば、アクション メソッド シグネチャが `Person` の種類を返したとします。ただし、この場合、`Person` から派生した `Student` または `Instructor` の種類を返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-143">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="4ebf2-144">フォーマッタで `Student` オブジェクトのみを処理する場合は、`CanWriteResult` メソッドに提供されたコンテキスト オブジェクトで [Object](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) の種類を確認します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-144">If you want your formatter to handle only `Student` objects, check the type of [Object](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="4ebf2-145">アクション メソッドが `IActionResult` を返す場合は、`CanWriteResult` を使用する必要がないことに注意してください。この場合、`CanWriteType` メソッドはランタイム型を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-145">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="4ebf2-146">ReadRequestBodyAsync/WriteResponseBodyAsync のオーバーライド</span><span class="sxs-lookup"><span data-stu-id="4ebf2-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span>

<span data-ttu-id="4ebf2-147">`ReadRequestBodyAsync` または `WriteResponseBodyAsync` で実際に逆シリアル化またはシリアル化を行います。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-147">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="4ebf2-148">次の例で強調表示されている行は、依存関係の挿入コンテナーからサービスを取得する方法を示しています (コンストラクター パラメーターからは取得できません)。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-148">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="4ebf2-149">カスタム フォーマッタを使用するように MVC を構成する方法</span><span class="sxs-lookup"><span data-stu-id="4ebf2-149">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="4ebf2-150">カスタム フォーマッタを使用するには、`InputFormatters` または `OutputFormatters` コレクションにフォーマッタ クラスのインスタンスを追加します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-150">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="4ebf2-151">フォーマッタは、挿入した順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-151">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="4ebf2-152">最初のものが優先されます。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-152">The first one takes precedence.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ebf2-153">次の手順</span><span class="sxs-lookup"><span data-stu-id="4ebf2-153">Next steps</span></span>

<span data-ttu-id="4ebf2-154">単純な vCard の入力と出力フォーマッタを実装するサンプル アプリケーションについては、[こちら](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-154">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span> <span data-ttu-id="4ebf2-155">アプリケーションでは、次の例のように vCard の読み取りと書き込みを行います。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-155">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="4ebf2-156">vCard の出力を表示するには、アプリケーションを実行し、Accept ヘッダー "text/vcard" を指定して Get 要求を `http://localhost:63313/api/contacts/` (Visual Studio から実行する場合) または `http://localhost:5000/api/contacts/` (コマンドラインから実行する場合) に送信します。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-156">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="4ebf2-157">連絡先のメモリ内コレクションに vCard を追加するには、本文に vCard テキストを指定し、Content-Type ヘッダー"text/vcard" を指定して、同じ URL に Post 要求を送信します (書式は前述の例と同じ)。</span><span class="sxs-lookup"><span data-stu-id="4ebf2-157">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
