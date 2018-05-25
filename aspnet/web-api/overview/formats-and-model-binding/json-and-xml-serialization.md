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
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON と ASP.NET Web API での XML シリアル化
====================
によって[Mike Wasson](https://github.com/MikeWasson)

この記事では、ASP.NET Web API で JSON および XML フォーマッタについて説明します。

ASP.NET Web API で、*メディア タイプ フォーマッタ*ことができるオブジェクトです。

- HTTP から読み取り CLR オブジェクトはメッセージ本文
- HTTP メッセージの本文に書き込むために CLR オブジェクト

Web API は、JSON と XML の両方のメディア タイプ フォーマッタを提供します。 フレームワークは、既定では、パイプラインにこれらのフォーマッタを挿入します。 クライアントでは、HTTP 要求の Accept ヘッダーで、JSON または XML を要求できます。

## <a name="contents"></a>目次

- [JSON のメディア タイプ フォーマッタ](#json_media_type_formatter)

    - [読み取り専用プロパティ](#json_readonly)
    - [日付](#json_dates)
    - [インデント](#json_indenting)
    - [Camel 形式の組み合わせ](#json_camelcasing)
    - [匿名と弱い型指定のオブジェクト](#json_anon)
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

によって提供される JSON の書式設定、 **JsonMediaTypeFormatter**クラスです。 既定では、 **JsonMediaTypeFormatter**を使用して、 [Json.NET](https://github.com/JamesNK/Newtonsoft.Json)ライブラリでシリアル化を実行します。 Json.NET は、サード パーティ オープン ソース プロジェクトです。

、したい場合は、構成、 **JsonMediaTypeFormatter**クラスを使用する、 **DataContractJsonSerializer** Json.NET の代わりにします。 これを行うには、設定、 **UseDataContractJsonSerializer**プロパティを**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON シリアル化

このセクションでは、既定値を使用して、JSON フォーマッタのいくつか特定の動作をについて説明します[Json.NET](https://github.com/JamesNK/Newtonsoft.Json)シリアライザー。 これは目的としていません。 Json.NET ライブラリの包括的なドキュメント詳細については、次を参照してください。、 [Json.NET ドキュメント](http://james.newtonking.com/projects/json/help/)です。

#### <a name="what-gets-serialized"></a>新機能をシリアル化を取得しますか。

既定では、シリアル化された JSON ですべてのパブリック プロパティおよびフィールドは含まれています。 プロパティまたはフィールドを省略する場合で装飾、 **JsonIgnore**属性。

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

希望する場合、&quot;オプトイン&quot;アプローチを使用してクラスを装飾する、 **DataContract**属性。 しない限り、この属性が存在する場合、メンバーは無視されます、 **DataMember**です。 使用することも**DataMember**プライベート メンバーをシリアル化します。

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>読み取り専用プロパティ

既定では、読み取り専用プロパティがシリアル化されます。

<a id="json_dates"></a>
### <a name="dates"></a>日付

既定では、Json.NET は日付を書き込みます[ISO 8601](http://www.w3.org/TR/NOTE-datetime)形式です。 "Z"サフィックスでは、UTC (協定世界時) の日付が書き込まれます。 現地時刻での日付には、タイム ゾーン オフセットが含まれます。 例:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

既定では、Json.NET には、タイム ゾーンが保持されます。 これは、DateTimeZoneHandling プロパティの設定を上書きできます。

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

使用する場合[Microsoft JSON 日付形式](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb)(`"\/Date(ticks)\/"`) ISO 8601 の代わりに設定する、 **DateFormatHandling**シリアライザーの設定のプロパティ。

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>インデント

インデントされた JSON を作成するには、設定、**書式**設定**Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Camel 形式の組み合わせ

書き込むには camel 形式の大文字と小文字の JSON プロパティ名、データ モデルを変更することがなく、設定、 **CamelCasePropertyNamesContractResolver**シリアライザーで。

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>匿名と弱い型指定のオブジェクト

アクション メソッドでは、匿名オブジェクトを取得でき、それを JSON にシリアル化することができます。 例:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

次の JSON 応答メッセージの本文が含まれます。

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Web API が疎受け取るには、クライアントからの JSON オブジェクトが構造化されている場合、要求本文を逆シリアル化できる、 **Newtonsoft.Json.Linq.JObject**型です。

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

ただし、厳密に型指定されたデータ オブジェクトを使用して通常お勧めします。 自分でデータを解析する必要はありませんし、およびモデルの検証のメリットを享受します。

XML シリアライザーが匿名型をサポートしていませんか**JObject**インスタンス。 JSON データをこれらの機能を使用する場合は、この記事で後述するよう、パイプラインから XML フォーマッタを削除する必要があります。

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML のメディア タイプ フォーマッタ

によって提供される XML 書式設定、 **XmlMediaTypeFormatter**クラスです。 既定では、 **XmlMediaTypeFormatter**を使用して、 **DataContractSerializer**クラスをシリアル化を実行します。

、したい場合は、構成、 **XmlMediaTypeFormatter**を使用する、 **XmlSerializer**の代わりに、 **DataContractSerializer**です。 これを行うには、設定、 **UseXmlSerializer**プロパティを**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer**クラスをより狭い一連の型よりもサポートしている**DataContractSerializer**が、結果の XML より詳細に制御を提供します。 使用を検討して**XmlSerializer**既存の XML スキーマに一致する必要がある場合。

### <a name="xml-serialization"></a>XML シリアル化

このセクションでは、既定値を使用して、XML フォーマッタのいくつか特定の動作をについて説明します**DataContractSerializer**です。

既定では、DataContractSerializer は、次のように動作します。

- すべてのパブリックの読み取り/書き込みプロパティおよびフィールドがシリアル化します。 プロパティまたはフィールドを省略する場合で装飾、 **IgnoreDataMember**属性。
- プライベートおよびプロテクト メンバーはシリアル化されません。
- 読み取り専用プロパティはシリアル化されません。 (ただし、読み取り専用コレクションのプロパティの内容は、シリアル化します。)
- クラス宣言に表示されているとおり、クラスおよびメンバーの名前は、XML で記述します。
- 既定の XML 名前空間が使用されます。

シリアル化をより細かく制御する場合を使用してクラスを装飾できます、 **DataContract**属性。 この属性が存在する場合は、次のように、クラスをシリアル化します。

- &quot;オプトイン&quot;アプローチ: プロパティとフィールドが既定でシリアル化されません。 プロパティまたはフィールドをシリアル化するで装飾、 **DataMember**属性。
- プライベートまたはプロテクト メンバーをシリアル化するで装飾、 **DataMember**属性。
- 読み取り専用プロパティはシリアル化されません。
- クラス名が XML で表示する方法を変更するには、設定、*名前*内のパラメーター、 **DataContract**属性。
- メンバー名が XML で表示する方法を変更するには、設定、*名前*内のパラメーター、 **DataMember**属性。
- XML 名前空間を変更するには、設定、 *Namespace*内のパラメーター、 **DataContract**クラスです。

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>読み取り専用プロパティ

読み取り専用プロパティはシリアル化されません。 読み取り専用プロパティにバッキング プライベート フィールドがある場合、プライベート フィールドをマークすることができます、 **DataMember**属性。 このアプローチが必要、 **DataContract**クラスの属性です。

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>日付

日付は ISO 8601 形式で書き込まれます。 たとえば、 &quot;2012-05-23T20:21:37.9116538Z&quot;です。

<a id="xml_indenting"></a>
### <a name="indenting"></a>インデント

インデントされた XML を書き込む設定、**インデント**プロパティを**true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>設定の種類の XML シリアライザー

別の CLR 型の別の XML シリアライザーを設定できます。 たとえばを必要とする特定のデータ オブジェクトがある**XmlSerializer**旧バージョンとの互換性のためです。 使用することができます**XmlSerializer**このオブジェクトを引き続き使用する**DataContractSerializer**他の種類。

特定の種類の XML シリアライザーを設定するには、呼び出す**SetSerializer**です。

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

指定することができます、 **XmlSerializer**または任意のオブジェクトから派生した**XmlObjectSerializer**です。

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>JSON または XML フォーマッタを削除します。

それらを使用したくない場合、フォーマッタの一覧から、JSON フォーマッタまたは XML フォーマッタを削除できます。 これを行う主な理由は次のとおりです。

- 特定のメディアの種類には、web API の応答を制限します。 たとえば、JSON の応答のみをサポートし、XML フォーマッタを削除することができます。
- 既定のフォーマッタをカスタム フォーマッタに置き換えます。 たとえば、JSON フォーマッタのカスタム実装で JSON フォーマッタを置き換えます可能性があります。

次のコードでは、既定のフォーマッタを削除する方法を示します。 これから、**アプリケーション\_開始**Global.asax で定義されたメソッドです。

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>オブジェクトの循環参照の処理

既定では、JSON と XML フォーマッタは、値としてすべてのオブジェクトを記述します。 2 つのプロパティは、同じオブジェクトを参照している場合、またはコレクションに 2 回、同じオブジェクトが表示された場合は、フォーマッタはシリアル化オブジェクト 2 回クリックします。 これは、特定の問題、オブジェクト グラフには、サイクルが含まれている場合ため、シリアライザー グラフにループが検出されたときに例外がスローされます。

次のオブジェクト モデルとコント ローラーを検討してください。

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

このアクションの呼び出しにより、フォーマッタに、ステータス コード 500 (内部サーバー エラー)、クライアントに応答に変換する例外をスローします。

JSON のオブジェクト参照を保持する場合に次のコードを追加**アプリケーション\_開始**Global.asax ファイル内のメソッド。

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

これでコント ローラーのアクションには、次のような JSON が返されます。

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

シリアライザーを追加する通知、 &quot;$id&quot;プロパティを両方のオブジェクト。 また、検出されたため、オブジェクト参照を値に置き換えます Employee.Department プロパティによって、ループが作成される: {&quot;$ref&quot;:&quot;1&quot;}。

> [!NOTE]
> オブジェクト参照は、JSON で標準ではありません。 この機能を使用する前に、クライアントが、結果を解析できるかどうかを検討してください。 サイクルをグラフから削除するには、単に方がよい場合があります。 たとえば、部門に従業員からのリンクは本当に不要この例でです。


XML 内のオブジェクト参照を保持するには、次の 2 つのオプションがあります。 追加する簡単な方法は、`[DataContract(IsReference=true)]`モデル クラス。 *IsReference*パラメーターには、オブジェクト参照が可能になります。 注意して**DataContract**を追加する必要がありますもようにオプトイン、シリアル化をにより**DataMember**プロパティに属性します。

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

これで、フォーマッタは、のような XML を生成するのには次。

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

別のオプションがある場合は、モデル クラスで属性を回避するには、: 新しい型固有の作成**DataContractSerializer**をインスタンス化し、設定*preserveObjectReferences*に**は true。** コンス トラクターでします。 このインスタンスの種類のシリアライザーとして XML メディアの種類のフォーマッタに設定します。 次のコードでは、これを行う方法を示します。

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>オブジェクトのシリアル化のテスト

Web API をデザインする際、データ オブジェクトをシリアル化する方法をテストすると便利です。 コント ローラーを作成またはコント ローラーのアクションを呼び出すことがなく、これを行うことができます。

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
