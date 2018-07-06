---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: ASP.NET Web API のパラメーター バインド |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97785db962691c25bac03f904924321af2f6b500
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810083"
---
<a name="parameter-binding-in-aspnet-web-api"></a>ASP.NET Web API のパラメーター バインド
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

呼ばれるプロセス パラメーターの値を設定する必要があります Web API コント ローラーのメソッドを呼び出す、*バインド*します。 この記事では、Web API が、パラメーターをバインドする方法と、バインディング プロセスをカスタマイズする方法について説明します。

既定では、Web API は、次の規則を使用して、パラメーターをバインドします。

- パラメーターが「単純な」型の場合、Web API は、URI から値を取得しようとします。 単純な種類には、.NET[プリミティブ型](https://msdn.microsoft.com/library/system.type.isprimitive.aspx)(**int**、 **bool**、**二重**など)、plus **TimeSpan**、 **DateTime**、 **Guid**、 **decimal**、および**文字列**、 *plus*任意文字列から変換できる型コンバーターを使用して入力します。 (詳細については後での型コンバーター。)
- 複合型、Web API では、メッセージ本文から値を読み取るしようとして、使用して、[メディア タイプ フォーマッタ](media-formatters.md)します。

たとえば、一般的な Web API コント ローラー メソッドを次に示します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Id*パラメーターは、&quot;単純&quot;型であるため、Web API は要求 URI の値を取得しようとしています。 *項目*パラメーターが、複雑な型、Web API は、メディアの種類のフォーマッタを使用して、要求本文から値を読み取るようにします。

URI から値を取得するには、Web API は、ルート データと、URI クエリ文字列で検索します。 ルーティング システムは、URI を解析し、ルートに一致する場合は、ルート データが設定されます。 詳細については、次を参照してください。[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)します。

この記事の残りの部分で、モデル バインディング プロセスをカスタマイズする方法を示します。 複合型は、ただし、可能であれば、メディア タイプ フォーマッタを使用してを考慮します。 HTTP の重要な原則は、リソースは、コンテンツ ネゴシエーションを使用して、リソースの表現を指定する、メッセージの本文で送信されます。 メディア タイプ フォーマッタは、正確に、この目的用に設計されています。

## <a name="using-fromuri"></a>[FromUri] を使用します。

URI から複合型を読み取る Web API を強制的には、追加、 **[FromUri]** 属性をパラメーター。 次の例では、定義、`GeoPoint`と共に取得するコント ローラー メソッドの型、 `GeoPoint` URI から。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

クライアントは、クエリ文字列で、緯度と経度の値を配置することができ、Web API を構築を使用してそれらが、`GeoPoint`します。 例えば:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>[FromBody] を使用します。

要求本文から単純型を読み取る Web API を強制的には、追加、 **[FromBody]** パラメーターに属性します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

この例では、Web API は、の値を読み取るメディア タイプ フォーマッタを使用が*名前*要求本文から。 クライアント要求の例を次に示します。

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

パラメーターでは、[FromBody] が持つ、Web API は、Content-type ヘッダーを使用して、フォーマッタを選択します。 この例では、コンテンツの種類は&quot;、application/json&quot;要求本文で、未加工の JSON 文字列 (JSON オブジェクトではありません)。

メッセージ本文から読み取る最大で 1 つのパラメーターが許可されます。 したがってこれは機能しません。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

このルールの理由では、1 回だけ読み取り可能な非バッファー ストリームで、要求本文を格納する可能性があること。

## <a name="type-converters"></a>型コンバーター

(Web API は、URI からバインドを試みます) するために、単純型としてクラスを扱う Web API を作成して行うことができます、 **TypeConverter**と文字列の変換を提供します。

次のコードは、 `GeoPoint` 、地理的ポイントを表すクラスと**TypeConverter**の文字列に変換する`GeoPoint`インスタンス。 `GeoPoint`クラスがで修飾された、 **[TypeConverter]** 属性を型コンバーターを指定します。 (この例は、Mike Stall のブログの投稿によって考え出されました[MVC/WebAPI のアクション シグネチャでのカスタム オブジェクトにバインドする方法](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx))。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Web API を扱うようになりました`GeoPoint`として単純型、つまりバインドを試みます`GeoPoint`URI からのパラメーター。 含める必要はありません **[FromUri]** パラメーターにします。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

クライアントは、このような URI を持つメソッドを呼び出すことができます。

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>モデル バインダー

型コンバーターよりもより柔軟なオプションでは、カスタム モデル バインダーを作成します。 モデル バインダーを使用ルート データからなど、HTTP 要求、アクションの説明および生の値にアクセス権があります。

モデル バインダーを作成するには、実装、 **IModelBinder**インターフェイス。 このインターフェイスは、1 つのメソッドを定義します**BindModel**:。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

ここでは、モデル バインダーを`GeoPoint`オブジェクト。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

モデル バインダーからの生の入力値を取得する、*値プロバイダー*します。 この設計では、2 つの異なる機能を分離します。

- 値プロバイダーは、HTTP 要求を受け取り、キー/値ペアのディクショナリを設定します。
- モデル バインダーでは、このディクショナリを使用してモデルを作成します。

Web API の既定値のプロバイダーは、ルート データとクエリ文字列から値を取得します。 たとえば、URI は、 `http://localhost/api/values/1?location=48,-122`、値プロバイダーが次のキー/値ペアを作成します。

- id = &quot;1&quot;
- 場所 = &quot;48,122&quot;

(これは既定のルート テンプレートを前提に話&quot;api/{controller}/{id}&quot;)。

バインドするパラメーターの名前が格納されている、 **ModelBindingContext.ModelName**プロパティ。 モデル バインダー ディクショナリ内でこの値を持つキーを探します。 値が存在しに変換できる場合、 `GeoPoint`、モデル バインダーにバインドされている値を割り当てます、 **ModelBindingContext.Model**プロパティ。

モデル バインダーは単純型の変換に限定されないことに注意してください。 この例では、モデル バインダーは、既知の場所のテーブルで最初にし、型変換を使用して失敗した場合。

**モデル バインダーの設定**

モデル バインダーを設定するいくつかの方法はあります。 最初に、追加、 **[拡大する ModelBinder]** 属性をパラメーター。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

追加することも、 **[拡大する ModelBinder]** 属性を型にします。 Web API は、その型のすべてのパラメーターの指定したモデル バインダーを使用します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

最後にモデル バインダー プロバイダーを追加することができます、 **HttpConfiguration**します。 モデル バインダー プロバイダーは、単に、モデル バインダーを作成するファクトリ クラスです。 派生することによって、プロバイダーを作成することができます、 [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx)クラス。 ただし、モデル バインダーは、1 つの型を処理する場合は、組み込みの使いやすい**SimpleModelBinderProvider**、これはこの目的に設計されています。 この方法を次のコードに示します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

モデル バインド プロバイダーでは、使用する必要がありますを追加、 **[拡大する ModelBinder]** 属性をモデル バインダーとメディア タイプ フォーマッタではないのどちらを使用することを Web API に指示する、パラメーター。 これで、属性でモデル バインダーの型を指定する必要はありません。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>値プロバイダー

モデル バインダーが値プロバイダーから値を取得することをお話しします。 カスタム値プロバイダーを作成するには、実装、 **IValueProvider**インターフェイス。 要求の cookie の値を取得する例を次に示します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

派生することによって、値プロバイダー ファクトリを作成する必要も、 **ValueProviderFactory**クラス。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

追加する値プロバイダー ファクトリ、 **HttpConfiguration**次のようにします。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web API がすべて、値プロバイダーのために合成モデル バインダーを呼び出すと**ValueProvider.GetValue**、モデル バインダーは、生成できる最初の値プロバイダーから値を受け取ります。

使用して、パラメーターのレベルで、値プロバイダー ファクトリを設定することができます、 **ValueProvider**属性は、次のようにします。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

これは、Web API で、指定した値プロバイダー ファクトリ モデル バインドを使用して、その他の登録済みの値プロバイダーを使用しないことを示します。

## <a name="httpparameterbinding"></a>HttpParameterBinding

モデル バインダーは、特定のインスタンスの全般的なメカニズムです。 確認する場合、 **[拡大する ModelBinder]** 属性が表示されます、抽象型から派生すること**ParameterBindingAttribute**クラス。 このクラスは、1 つのメソッドを定義します**GetBinding**、返された、 **HttpParameterBinding**オブジェクト。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding**パラメーターを値にバインドを担当します。 場合に **[拡大する ModelBinder]**、属性を返します、 **HttpParameterBinding**を使用して実装する**IModelBinder**実際のバインドを実行します。 独自に実装することも**HttpParameterBinding**します。

たとえばから Etag を取得する`if-match`と`if-none-match`要求のヘッダー。 Etag を表すクラスの定義から始めます。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

ETag を取得するかどうかを示す列挙体も定義します、`if-match`ヘッダーまたは`if-none-match`ヘッダー。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

ここでは、 **HttpParameterBinding** ETag 型のパラメーターにバインドされますを目的のヘッダーから ETag を取得します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync**メソッドは、バインドします。 このメソッド内にバインドされたパラメーター値を追加、 **ActionArgument**ディクショナリで、 **HttpActionContext**します。

> [!NOTE]
> 場合、 **ExecuteBindingAsync**メソッドは、要求メッセージの本文を読み取り、上書き、 **WillReadBody**プロパティが true を返します。 要求本文には、読み取りしか実行 1 回、Web API 規則を適用する 1 つだけにバインドするようにするバッファリングされていないストリームは、メッセージ本文を読み取ることができます可能性があります。


カスタムを適用する**HttpParameterBinding**から派生した属性を定義する**ParameterBindingAttribute**します。 `ETagParameterBinding`、1 つずつ、2 つの属性を定義します`if-match`ヘッダーと 1 つずつ`if-none-match`ヘッダー。 抽象基本クラスから派生させます。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

使用するコント ローラー メソッドを次に示します、`[IfNoneMatch]`属性。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

それに**ParameterBindingAttribute**、カスタムを追加するための別のフック**HttpParameterBinding**します。 **HttpConfiguration**オブジェクト、 **ParameterBindingRules**プロパティは、型の anomymous 関数のコレクション (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**)。 たとえば、ETag のパラメーターを GET メソッドを使用するルールを追加できます`ETagParameterBinding`で`if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

関数が返す必要があります`null`のパラメーターのバインドは適用されません。

## <a name="iactionvaluebinder"></a>IActionValueBinder

パラメーター バインディング プロセス全体が、プラグ可能なサービスによって制御される**IActionValueBinder**します。 既定の実装**IActionValueBinder**は次の処理します。

1. 探して、 **ParameterBindingAttribute**パラメーターにします。 これが含まれています **[FromBody]**、 **[FromUri]**、および **[拡大する ModelBinder]**、またはカスタム属性。
2. それ以外の場合、ファイルの場所**HttpConfiguration.ParameterBindingRules**を null 以外を返す関数の**HttpParameterBinding**します。
3. それ以外の場合、先ほど説明した既定の規則を使用します。 

    - パラメーターの型が「簡単」した場合、または型コンバーターが場合は、URI からバインドします。 これは配置することに相当、 **[FromUri]** パラメーターの属性。
    - それ以外の場合、メッセージ本文からパラメーターの読み取りを再試行してください。 これは配置することに相当 **[FromBody]** パラメーターにします。

全体を置き換えることができる場合は、 **IActionValueBinder**カスタム実装サービス。

## <a name="additional-resources"></a>その他のリソース

[カスタム パラメーター バインドのサンプル](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike Stall は、Web API のパラメーター バインディングの適切な一連のブログを作成しました。

- [どのように Web API は、パラメーターのバインド](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Web API の MVC スタイルのパラメーターのバインド](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [MVC または Web API でのアクション シグネチャでのカスタム オブジェクトにバインドする方法](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Web API でカスタム値プロバイダーを作成する方法](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [内部的には web API パラメーターのバインド](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
