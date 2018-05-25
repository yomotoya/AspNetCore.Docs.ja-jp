---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON と ASP.NET Web API での XML シリアル化 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="5d2e0-102">JSON と ASP.NET Web API での XML シリアル化</span><span class="sxs-lookup"><span data-stu-id="5d2e0-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="5d2e0-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5d2e0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5d2e0-104">この記事では、ASP.NET Web API で JSON および XML フォーマッタについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="5d2e0-105">ASP.NET Web API で、*メディア タイプ フォーマッタ*ことができるオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="5d2e0-106">HTTP から読み取り CLR オブジェクトはメッセージ本文</span><span class="sxs-lookup"><span data-stu-id="5d2e0-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="5d2e0-107">HTTP メッセージの本文に書き込むために CLR オブジェクト</span><span class="sxs-lookup"><span data-stu-id="5d2e0-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="5d2e0-108">Web API は、JSON と XML の両方のメディア タイプ フォーマッタを提供します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="5d2e0-109">フレームワークは、既定では、パイプラインにこれらのフォーマッタを挿入します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="5d2e0-110">クライアントでは、HTTP 要求の Accept ヘッダーで、JSON または XML を要求できます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="5d2e0-111">目次</span><span class="sxs-lookup"><span data-stu-id="5d2e0-111">Contents</span></span>

- [<span data-ttu-id="5d2e0-112">JSON のメディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="5d2e0-113">読み取り専用プロパティ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="5d2e0-114">日付</span><span class="sxs-lookup"><span data-stu-id="5d2e0-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="5d2e0-115">インデント</span><span class="sxs-lookup"><span data-stu-id="5d2e0-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="5d2e0-116">Camel 形式の組み合わせ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="5d2e0-117">匿名と弱い型指定のオブジェクト</span><span class="sxs-lookup"><span data-stu-id="5d2e0-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="5d2e0-118">XML のメディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="5d2e0-119">読み取り専用プロパティ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="5d2e0-120">日付</span><span class="sxs-lookup"><span data-stu-id="5d2e0-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="5d2e0-121">インデント</span><span class="sxs-lookup"><span data-stu-id="5d2e0-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="5d2e0-122">設定の種類の XML シリアライザー</span><span class="sxs-lookup"><span data-stu-id="5d2e0-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="5d2e0-123">JSON または XML フォーマッタを削除します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="5d2e0-124">オブジェクトの循環参照の処理</span><span class="sxs-lookup"><span data-stu-id="5d2e0-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="5d2e0-125">オブジェクトのシリアル化のテスト</span><span class="sxs-lookup"><span data-stu-id="5d2e0-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="5d2e0-126">JSON のメディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="5d2e0-127">によって提供される JSON の書式設定、 **JsonMediaTypeFormatter**クラスです。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="5d2e0-128">既定では、 **JsonMediaTypeFormatter**を使用して、 [Json.NET](https://github.com/JamesNK/Newtonsoft.Json)ライブラリでシリアル化を実行します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="5d2e0-129">Json.NET は、サード パーティ オープン ソース プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="5d2e0-130">、したい場合は、構成、 **JsonMediaTypeFormatter**クラスを使用する、 **DataContractJsonSerializer** Json.NET の代わりにします。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="5d2e0-131">これを行うには、設定、 **UseDataContractJsonSerializer**プロパティを**true**:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="5d2e0-132">JSON シリアル化</span><span class="sxs-lookup"><span data-stu-id="5d2e0-132">JSON Serialization</span></span>

<span data-ttu-id="5d2e0-133">このセクションでは、既定値を使用して、JSON フォーマッタのいくつか特定の動作をについて説明します[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)シリアライザー。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="5d2e0-134">これは目的としていません。 Json.NET ライブラリの包括的なドキュメント詳細については、次を参照してください。、 [Json.NET ドキュメント](http://james.newtonking.com/projects/json/help/)です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="5d2e0-135">新機能をシリアル化を取得しますか。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-135">What Gets Serialized?</span></span>

<span data-ttu-id="5d2e0-136">既定では、シリアル化された JSON ですべてのパブリック プロパティおよびフィールドは含まれています。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="5d2e0-137">プロパティまたはフィールドを省略する場合で装飾、 **JsonIgnore**属性。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="5d2e0-138">希望する場合、&quot;オプトイン&quot;アプローチを使用してクラスを装飾する、 **DataContract**属性。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="5d2e0-139">しない限り、この属性が存在する場合、メンバーは無視されます、 **DataMember**です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="5d2e0-140">使用することも**DataMember**プライベート メンバーをシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="5d2e0-141">読み取り専用プロパティ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-141">Read-Only Properties</span></span>

<span data-ttu-id="5d2e0-142">既定では、読み取り専用プロパティがシリアル化されます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="5d2e0-143">日付</span><span class="sxs-lookup"><span data-stu-id="5d2e0-143">Dates</span></span>

<span data-ttu-id="5d2e0-144">既定では、Json.NET は日付を書き込みます[ISO 8601](http://www.w3.org/TR/NOTE-datetime)形式です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="5d2e0-145">"Z"サフィックスでは、UTC (協定世界時) の日付が書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="5d2e0-146">現地時刻での日付には、タイム ゾーン オフセットが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="5d2e0-147">例:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="5d2e0-148">既定では、Json.NET には、タイム ゾーンが保持されます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="5d2e0-149">これは、DateTimeZoneHandling プロパティの設定を上書きできます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="5d2e0-150">使用する場合[Microsoft JSON 日付形式](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb)(`"\/Date(ticks)\/"`) ISO 8601 の代わりに設定する、 **DateFormatHandling**シリアライザーの設定のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="5d2e0-151">インデント</span><span class="sxs-lookup"><span data-stu-id="5d2e0-151">Indenting</span></span>

<span data-ttu-id="5d2e0-152">インデントされた JSON を作成するには、設定、**書式**設定**Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="5d2e0-153">Camel 形式の組み合わせ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-153">Camel Casing</span></span>

<span data-ttu-id="5d2e0-154">書き込むには camel 形式の大文字と小文字の JSON プロパティ名、データ モデルを変更することがなく、設定、 **CamelCasePropertyNamesContractResolver**シリアライザーで。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="5d2e0-155">匿名と弱い型指定のオブジェクト</span><span class="sxs-lookup"><span data-stu-id="5d2e0-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="5d2e0-156">アクション メソッドでは、匿名オブジェクトを取得でき、それを JSON にシリアル化することができます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="5d2e0-157">例:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="5d2e0-158">次の JSON 応答メッセージの本文が含まれます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="5d2e0-159">Web API が疎受け取るには、クライアントからの JSON オブジェクトが構造化されている場合、要求本文を逆シリアル化できる、 **Newtonsoft.Json.Linq.JObject**型です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="5d2e0-160">ただし、厳密に型指定されたデータ オブジェクトを使用して通常お勧めします。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="5d2e0-161">自分でデータを解析する必要はありませんし、およびモデルの検証のメリットを享受します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="5d2e0-162">XML シリアライザーが匿名型をサポートしていませんか**JObject**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="5d2e0-163">JSON データをこれらの機能を使用する場合は、この記事で後述するよう、パイプラインから XML フォーマッタを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="5d2e0-164">XML のメディア タイプ フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="5d2e0-165">によって提供される XML 書式設定、 **XmlMediaTypeFormatter**クラスです。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="5d2e0-166">既定では、 **XmlMediaTypeFormatter**を使用して、 **DataContractSerializer**クラスをシリアル化を実行します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="5d2e0-167">、したい場合は、構成、 **XmlMediaTypeFormatter**を使用する、 **XmlSerializer**の代わりに、 **DataContractSerializer**です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="5d2e0-168">これを行うには、設定、 **UseXmlSerializer**プロパティを**true**:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="5d2e0-169">**XmlSerializer**クラスをより狭い一連の型よりもサポートしている**DataContractSerializer**が、結果の XML より詳細に制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="5d2e0-170">使用を検討して**XmlSerializer**既存の XML スキーマに一致する必要がある場合。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="5d2e0-171">XML シリアル化</span><span class="sxs-lookup"><span data-stu-id="5d2e0-171">XML Serialization</span></span>

<span data-ttu-id="5d2e0-172">このセクションでは、既定値を使用して、XML フォーマッタのいくつか特定の動作をについて説明します**DataContractSerializer**です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="5d2e0-173">既定では、DataContractSerializer は、次のように動作します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="5d2e0-174">すべてのパブリックの読み取り/書き込みプロパティおよびフィールドがシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="5d2e0-175">プロパティまたはフィールドを省略する場合で装飾、 **IgnoreDataMember**属性。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="5d2e0-176">プライベートおよびプロテクト メンバーはシリアル化されません。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="5d2e0-177">読み取り専用プロパティはシリアル化されません。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="5d2e0-178">(ただし、読み取り専用コレクションのプロパティの内容は、シリアル化します。)</span><span class="sxs-lookup"><span data-stu-id="5d2e0-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="5d2e0-179">クラス宣言に表示されているとおり、クラスおよびメンバーの名前は、XML で記述します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="5d2e0-180">既定の XML 名前空間が使用されます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-180">A default XML namespace is used.</span></span>

<span data-ttu-id="5d2e0-181">シリアル化をより細かく制御する場合を使用してクラスを装飾できます、 **DataContract**属性。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="5d2e0-182">この属性が存在する場合は、次のように、クラスをシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="5d2e0-183">&quot;オプトイン&quot;アプローチ: プロパティとフィールドが既定でシリアル化されません。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="5d2e0-184">プロパティまたはフィールドをシリアル化するで装飾、 **DataMember**属性。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="5d2e0-185">プライベートまたはプロテクト メンバーをシリアル化するで装飾、 **DataMember**属性。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="5d2e0-186">読み取り専用プロパティはシリアル化されません。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="5d2e0-187">クラス名が XML で表示する方法を変更するには、設定、*名前*内のパラメーター、 **DataContract**属性。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="5d2e0-188">メンバー名が XML で表示する方法を変更するには、設定、*名前*内のパラメーター、 **DataMember**属性。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="5d2e0-189">XML 名前空間を変更するには、設定、 *Namespace*内のパラメーター、 **DataContract**クラスです。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="5d2e0-190">読み取り専用プロパティ</span><span class="sxs-lookup"><span data-stu-id="5d2e0-190">Read-Only Properties</span></span>

<span data-ttu-id="5d2e0-191">読み取り専用プロパティはシリアル化されません。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="5d2e0-192">読み取り専用プロパティにバッキング プライベート フィールドがある場合、プライベート フィールドをマークすることができます、 **DataMember**属性。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="5d2e0-193">このアプローチが必要、 **DataContract**クラスの属性です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="5d2e0-194">日付</span><span class="sxs-lookup"><span data-stu-id="5d2e0-194">Dates</span></span>

<span data-ttu-id="5d2e0-195">日付は ISO 8601 形式で書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="5d2e0-196">たとえば、 &quot;2012-05-23T20:21:37.9116538Z&quot;です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="5d2e0-197">インデント</span><span class="sxs-lookup"><span data-stu-id="5d2e0-197">Indenting</span></span>

<span data-ttu-id="5d2e0-198">インデントされた XML を書き込む設定、**インデント**プロパティを**true**:</span><span class="sxs-lookup"><span data-stu-id="5d2e0-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="5d2e0-199">設定の種類の XML シリアライザー</span><span class="sxs-lookup"><span data-stu-id="5d2e0-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="5d2e0-200">別の CLR 型の別の XML シリアライザーを設定できます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="5d2e0-201">たとえばを必要とする特定のデータ オブジェクトがある**XmlSerializer**旧バージョンとの互換性のためです。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="5d2e0-202">使用することができます**XmlSerializer**このオブジェクトを引き続き使用する**DataContractSerializer**他の種類。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="5d2e0-203">特定の種類の XML シリアライザーを設定するには、呼び出す**SetSerializer**です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="5d2e0-204">指定することができます、 **XmlSerializer**または任意のオブジェクトから派生した**XmlObjectSerializer**です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="5d2e0-205">JSON または XML フォーマッタを削除します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="5d2e0-206">それらを使用したくない場合、フォーマッタの一覧から、JSON フォーマッタまたは XML フォーマッタを削除できます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="5d2e0-207">これを行う主な理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="5d2e0-208">特定のメディアの種類には、web API の応答を制限します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="5d2e0-209">たとえば、JSON の応答のみをサポートし、XML フォーマッタを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="5d2e0-210">既定のフォーマッタをカスタム フォーマッタに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="5d2e0-211">たとえば、JSON フォーマッタのカスタム実装で JSON フォーマッタを置き換えます可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="5d2e0-212">次のコードでは、既定のフォーマッタを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="5d2e0-213">これから、**アプリケーション\_開始**Global.asax で定義されたメソッドです。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="5d2e0-214">オブジェクトの循環参照の処理</span><span class="sxs-lookup"><span data-stu-id="5d2e0-214">Handling Circular Object References</span></span>

<span data-ttu-id="5d2e0-215">既定では、JSON と XML フォーマッタは、値としてすべてのオブジェクトを記述します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="5d2e0-216">2 つのプロパティは、同じオブジェクトを参照している場合、またはコレクションに 2 回、同じオブジェクトが表示された場合は、フォーマッタはシリアル化オブジェクト 2 回クリックします。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="5d2e0-217">これは、特定の問題、オブジェクト グラフには、サイクルが含まれている場合ため、シリアライザー グラフにループが検出されたときに例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="5d2e0-218">次のオブジェクト モデルとコント ローラーを検討してください。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="5d2e0-219">このアクションの呼び出しにより、フォーマッタに、ステータス コード 500 (内部サーバー エラー)、クライアントに応答に変換する例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="5d2e0-220">JSON のオブジェクト参照を保持する場合に次のコードを追加**アプリケーション\_開始**Global.asax ファイル内のメソッド。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="5d2e0-221">これでコント ローラーのアクションには、次のような JSON が返されます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="5d2e0-222">シリアライザーを追加する通知、 &quot;$id&quot;プロパティを両方のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="5d2e0-223">また、検出されたため、オブジェクト参照を値に置き換えます Employee.Department プロパティによって、ループが作成される: {&quot;$ref&quot;:&quot;1&quot;}。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="5d2e0-224">オブジェクト参照は、JSON で標準ではありません。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="5d2e0-225">この機能を使用する前に、クライアントが、結果を解析できるかどうかを検討してください。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="5d2e0-226">サイクルをグラフから削除するには、単に方がよい場合があります。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="5d2e0-227">たとえば、部門に従業員からのリンクは本当に不要この例でです。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="5d2e0-228">XML 内のオブジェクト参照を保持するには、次の 2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="5d2e0-229">追加する簡単な方法は、`[DataContract(IsReference=true)]`モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="5d2e0-230">*IsReference*パラメーターには、オブジェクト参照が可能になります。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="5d2e0-231">注意して**DataContract**を追加する必要がありますもようにオプトイン、シリアル化をにより**DataMember**プロパティに属性します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="5d2e0-232">これで、フォーマッタは、のような XML を生成するのには次。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="5d2e0-233">別のオプションがある場合は、モデル クラスで属性を回避するには、: 新しい型固有の作成**DataContractSerializer**をインスタンス化し、設定*preserveObjectReferences*に**は true。** コンス トラクターでします。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="5d2e0-234">このインスタンスの種類のシリアライザーとして XML メディアの種類のフォーマッタに設定します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="5d2e0-235">次のコードでは、これを行う方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="5d2e0-236">オブジェクトのシリアル化のテスト</span><span class="sxs-lookup"><span data-stu-id="5d2e0-236">Testing Object Serialization</span></span>

<span data-ttu-id="5d2e0-237">Web API をデザインする際、データ オブジェクトをシリアル化する方法をテストすると便利です。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="5d2e0-238">コント ローラーを作成またはコント ローラーのアクションを呼び出すことがなく、これを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5d2e0-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
