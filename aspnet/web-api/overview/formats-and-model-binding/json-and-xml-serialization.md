---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON と ASP.NET Web API での XML シリアル化 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e7fbcd41d64651255763c7629f0232788dcb3d30
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400922"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON と ASP.NET Web API での XML シリアル化
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

この記事では、ASP.NET Web API で JSON と XML フォーマッタについて説明します。

ASP.NET Web api を*メディア タイプ フォーマッタ*ことができるオブジェクトです。

- HTTP からの読み取りの CLR オブジェクトはメッセージ本文
- HTTP メッセージの本文に CLR オブジェクトを書き込む

Web API は、JSON と XML の両方のメディア タイプ フォーマッタを提供します。 フレームワークは、既定では、パイプラインにこれらのフォーマッタを挿入します。 クライアントが HTTP 要求の Accept ヘッダーでは、JSON または XML を要求できます。

## <a name="contents"></a>目次

- [JSON のメディア タイプ フォーマッタ](#json_media_type_formatter)

    - [読み取り専用プロパティ](#json_readonly)
    - [日付](#json_dates)
    - [インデント](#json_indenting)
    - [Camel 形式の規則](#json_camelcasing)
    - [匿名と厳密に型指定のオブジェクト](#json_anon)
- [XML のメディア タイプ フォーマッタ](#xml_media_type_formatter)

    - [読み取り専用プロパティ](#xml_readonly)
    - [日付](#xml_dates)
    - [インデント](#xml_indenting)
    - [設定の種類の XML シリアライザー](#xml_pertype)
- [JSON または XML フォーマッタを削除します。](#removing_the_json_or_xml_formatter)
- [オブジェクトの循環参照の処理](#handling_circular_object_references)
- [オブジェクトのシリアル化のテスト](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>JSON のメディア タイプ フォーマッタ

によって提供される JSON 形式、 **JsonMediaTypeFormatter**クラス。 既定では、 **JsonMediaTypeFormatter**を使用して、 [Json.NET](https://github.com/JamesNK/Newtonsoft.Json)ライブラリでシリアル化を実行します。 Json.NET は、サード パーティ製オープン ソース プロジェクトです。

構成することができる場合は、 **JsonMediaTypeFormatter**クラスを使用する、 **DataContractJsonSerializer** Json.NET の代わりにします。 これを行うには、設定、 **UseDataContractJsonSerializer**プロパティを**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON シリアル化

このセクションは、既定値を使用して、JSON フォーマッタのいくつかの特定の動作を説明します[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)シリアライザー。 これは意図するもの、Json.NET ライブラリの包括的なドキュメント詳細については、次を参照してください。、 [Json.NET のドキュメント](http://james.newtonking.com/projects/json/help/)します。

#### <a name="what-gets-serialized"></a>どのようなシリアル化を取得しますか。

既定では、すべてのパブリック プロパティとフィールドはシリアル化された JSON に含まれます。 プロパティまたはフィールドを省略する装飾することで、 **JsonIgnore**属性。

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

使用する場合、&quot;オプトイン&quot;アプローチ、使用して、クラスを装飾、 **DataContract**属性。 所有していない場合、メンバーは無視されますこの属性が存在する場合、 **DataMember**します。 使用することも**DataMember**をプライベート メンバーをシリアル化します。

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>読み取り専用プロパティ

既定では、読み取り専用プロパティがシリアル化します。

<a id="json_dates"></a>
### <a name="dates"></a>日付

既定では、Json.NET が日付を書き込みます[ISO 8601](http://www.w3.org/TR/NOTE-datetime)形式。 "Z"サフィックスでは、UTC (世界協定時刻) の日付が書き込まれます。 日付現地時刻にはでは、タイム ゾーン オフセットが含まれます。 例えば:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

既定では、Json.NET には、タイム ゾーンが保持されます。 これは、DateTimeZoneHandling プロパティを設定してオーバーライドできます。

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

使いたい場合[Microsoft JSON 日付形式](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb)(`"\/Date(ticks)\/"`) ISO 8601 形式ではなく設定、 **DateFormatHandling**シリアライザーの設定のプロパティ。

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>インデント

インデントが設定された JSON を作成するには、設定、**書式**設定**Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Camel 形式の規則

Camel 形式の組み合わせでの JSON プロパティ名、データ モデルを変更することがなくを作成するには、設定、 **CamelCasePropertyNamesContractResolver**シリアライザーで。

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>匿名と厳密に型指定のオブジェクト

アクション メソッドでは、匿名のオブジェクトを取得でき、JSON にシリアル化することができます。 例えば:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

応答メッセージの本文には、次の JSON が含まれます。

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

要求本文を逆シリアル化できる、web API が疎に受け取るには、クライアントからの JSON オブジェクトが構造化されている場合、 **Newtonsoft.Json.Linq.JObject**型。

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

ただし、厳密に型指定されたデータ オブジェクトを使用して、通常はお勧めします。 自分でデータを解析する必要はありませんし、およびモデルの検証の利点があります。

XML シリアライザーでは、匿名型はサポートしませんまたは**JObject**インスタンス。 JSON データをこれらの機能を使用する場合は、この記事の後半で説明、パイプラインから XML フォーマッタを削除する必要があります。

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML のメディア タイプ フォーマッタ

XML 書式設定される、 **XmlMediaTypeFormatter**クラス。 既定では、 **XmlMediaTypeFormatter**を使用して、 **DataContractSerializer**クラスをシリアル化を実行します。

構成することができる場合は、 **XmlMediaTypeFormatter**を使用する、 **XmlSerializer**の代わりに、 **DataContractSerializer**します。 これを行うには、設定、 **UseXmlSerializer**プロパティを**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer**クラスより狭い一連のよりも型をサポートしている**DataContractSerializer**が、結果の XML をより詳細に制御を提供します。 使用を検討して**XmlSerializer**既存の XML スキーマと一致する必要がある場合。

### <a name="xml-serialization"></a>XML シリアル化

このセクションは、既定値を使用して、XML フォーマッタのいくつかの特定の動作を説明します**DataContractSerializer**します。

既定では、DataContractSerializer は、次のように動作します。

- すべてのパブリックな読み取り/書き込みプロパティとフィールドがシリアル化します。 プロパティまたはフィールドを省略する装飾することで、 **IgnoreDataMember**属性。
- プライベートおよびプロテクト メンバーはシリアル化されません。
- 読み取り専用プロパティはシリアル化されません。 (ただし、読み取り専用コレクションのプロパティの内容がシリアル化されます。)
- クラスとメンバーの名前は、クラス宣言に表示されているとおり、XML で記述されます。
- 既定の XML 名前空間が使用されます。

さらに、シリアル化制御を必要がある場合は、使用して、クラスを修飾することができます、 **DataContract**属性。 この属性が存在する場合は、クラスは、次のようにシリアル化します。

- &quot;オプトイン&quot;方法: プロパティとパブリック フィールドが既定でシリアル化されません。 プロパティまたはフィールドをシリアル化する装飾することで、 **DataMember**属性。
- プライベートまたはプロテクト メンバーをシリアル化するで装飾、 **DataMember**属性。
- 読み取り専用プロパティはシリアル化されません。
- クラス名が XML で表示する方法を変更するには、設定、*名前*パラメーター、 **DataContract**属性。
- メンバー名が XML で表示する方法を変更するには、設定、*名前*パラメーター、 **DataMember**属性。
- XML 名前空間を変更するには、設定、 *Namespace*パラメーター、 **DataContract**クラス。

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>読み取り専用プロパティ

読み取り専用プロパティはシリアル化されません。 読み取り専用プロパティにプライベート バッキング フィールドがある場合、プライベート フィールドをマークすることができます、 **DataMember**属性。 このアプローチが必要です、 **DataContract**クラスの属性。

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>日付

日付は ISO 8601 形式で記述されます。 たとえば、 &quot;2012-05-23T20:21:37.9116538Z&quot;します。

<a id="xml_indenting"></a>
### <a name="indenting"></a>インデント

インデントされた XML を作成するには、設定、**インデント**プロパティを**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>設定の種類の XML シリアライザー

CLR のさまざまな種類の異なる XML シリアライザーを設定できます。 たとえばを必要とする特定のデータ オブジェクトがある**XmlSerializer**旧バージョンとの互換性のためです。 使用することができます**XmlSerializer**このオブジェクトを引き続き使用する**DataContractSerializer**他の種類。

XML シリアライザーを特定の型を設定するには、呼び出す**SetSerializer**します。

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

指定することができます、 **XmlSerializer**または任意のオブジェクトから派生した**XmlObjectSerializer**します。

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>JSON または XML フォーマッタを削除します。

それらを使用したくない場合、フォーマッタの一覧から、JSON フォーマッタまたは XML フォーマッタを削除できます。 これを行う主な理由は次のとおりです。

- 特定のメディアの種類には、web API の応答を制限します。 たとえば、JSON の応答のみをサポートし、XML フォーマッタを削除することがあります。
- 既定のフォーマッタをカスタム フォーマッタに置き換えます。 たとえば、JSON フォーマッタのカスタム実装で、JSON フォーマッタを置き換えることができます。

次のコードでは、既定のフォーマッタを削除する方法を示します。 これから、**アプリケーション\_開始**Global.asax で定義されたメソッド。

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>オブジェクトの循環参照の処理

既定では、JSON および XML フォーマッタは、値としてすべてのオブジェクトを作成します。 2 つのプロパティは、同一のオブジェクトを参照している場合、またはコレクションに 2 回同じオブジェクトを表示する場合は、フォーマッタは、オブジェクトを 2 回シリアルです。 これは、特定の問題をオブジェクト グラフには、サイクルが含まれている場合ため、シリアライザー グラフにループを検出した場合に例外がスローされます。

次のオブジェクト モデルとコント ローラーを検討してください。

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

このアクションの呼び出しにより、フォーマッタに、ステータス コード 500 (Internal Server Error) 応答をクライアントに変換される例外をスローします。

JSON 内のオブジェクト参照を保持するために次のコードを追加**アプリケーション\_開始**Global.asax ファイル内のメソッド。

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

これでコント ローラー アクションには、次のような JSON が返されます。

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

シリアライザーを追加しますが、 &quot;$id&quot;両方のオブジェクトへのプロパティ。 オブジェクト参照で値が置き換えられますので、Employee.Department プロパティは、ループを作成します。 また、検出: {&quot;$ref&quot;:&quot;1&quot;}。

> [!NOTE]
> オブジェクト参照は、json 標準ではありません。 この機能を使用する前に、クライアントが、結果を解析できるかどうかを検討してください。 サイクルをグラフから削除するには、単に方がよい場合があります。 たとえば、部門には、従業員からのリンクは本当に必要ありませんこの例では。


XML 内のオブジェクト参照を保持するには、2 つのオプションがあります。 簡単な方法は、追加する`[DataContract(IsReference=true)]`モデル クラスにします。 *IsReference*パラメーターには、オブジェクト参照が可能になります。 注意して**DataContract**オプトイン、シリアル化は、追加する必要がありますも**DataMember**プロパティに属性します。

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

フォーマッタのような XML が生成されますので次。

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

別のオプションがある場合は、モデル クラスで属性を回避するには、: 新しい型固有の作成**DataContractSerializer**インスタンスし、設定*preserveObjectReferences*に**は true。** コンス トラクターでします。 XML のメディア タイプ フォーマッタの型のシリアライザーとしてこのインスタンスを設定します。 次のコードでは、これを行う方法を示します。

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>オブジェクトのシリアル化のテスト

Web API を設計する際、データ オブジェクトをシリアル化する方法をテストすると便利です。 コント ローラーを作成したり、コント ローラー アクションの起動せずに、これを行うことができます。

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
