---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: ASP.NET Web API でのバインディング パラメーター |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042431"
---
<a name="parameter-binding-in-aspnet-web-api"></a>ASP.NET Web API でのバインディング パラメーター
====================
によって[Mike Wasson](https://github.com/MikeWasson)

呼ばれるプロセスのパラメーターの値を設定する必要があります Web API は、コント ローラーのメソッドを呼び出す、*バインディング*です。 この記事では、Web API が、パラメーターをバインドする方法と、バインディング プロセスをカスタマイズする方法について説明します。

既定では、Web API は、次の規則を使用して、パラメーターをバインドします。

- パラメーターが「単純」型の場合は、Web API は、URI から値を取得しようとします。 単純型は、.NET[プリミティブ型](https://msdn.microsoft.com/library/system.type.isprimitive.aspx)(**int**、 **bool**、**二重**など)、および**TimeSpan**、 **DateTime**、 **Guid**、 **decimal**、および**文字列**、 *plus*任意文字列から変換できる型コンバーターで入力します。 (後の型コンバーターの詳細です。)
- メッセージ本文から値を読み取るしようとすると、複合型、Web API を使用して、[メディア タイプ フォーマッタ](media-formatters.md)です。

たとえば、典型的な Web API コント ローラーのメソッドを次に示します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Id*パラメーターは、&quot;単純&quot;型であるため、Web API は、要求の URI から値を取得しようとしています。 *項目*パラメーターには Web API は、メディア タイプ フォーマッタを使用して、要求本文から値を読み取るために、複合型。

URI から値を取得するには、Web API は、ルート データと URI クエリ文字列で検索します。 ルーティングのシステムは、URI を解析しをルートと一致するルート データは設定されます。 詳細については、次を参照してください。[ルーティングとアクションの選択](../web-api-routing-and-actions/routing-and-action-selection.md)です。

この記事の残りの部分で、モデル バインディング プロセスをカスタマイズする方法を示します。 複合型のただし、可能な限り、メディア タイプ フォーマッタを使用を検討します。 HTTP の重要な原則は、リソースは、コンテンツ ネゴシエーションを使用して、リソースの表現を指定する、メッセージの本文で送信されます。 メディア タイプ フォーマッタは、この目的だけのために設計されました。

## <a name="using-fromuri"></a>[FromUri] を使用します。

Web API を URI から複合型を読み取るには、追加、 **[FromUri]** 属性をパラメーター。 次の例では定義、`GeoPoint`型を取得するコント ローラー メソッドと共に、 `GeoPoint` URI からです。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

クライアントは、クエリ文字列に緯度と経度の値を配置することができ、Web API が構築するために使用されます、`GeoPoint`です。 例:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>[FromBody] を使用します。

要求本文から単純型の読み取りに Web API には、追加、 **[FromBody]** パラメーターに属性します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

この例では、Web API は使用して、メディア タイプ フォーマッタの値を読み取る*名前*要求の本体からです。 クライアント要求の例を次に示します。

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

パラメーターでは、[FromBody] が持つ、Web API は、Content-type ヘッダーを使用して、フォーマッタを選択します。 コンテンツの種類は、この例では&quot;アプリケーション/json&quot;要求本文は、生の JSON 文字列 (JSON オブジェクトではありません)。

最大で 1 つのパラメーターは、メッセージ本文からの読み取りが許可されます。 したがってこれは機能しません。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

このルールの理由は、された要求本文を 1 回のみ読み取ることができます、バッファリングされないストリームに保存する可能性があります。

## <a name="type-converters"></a>型コンバーター

作成することで Web API (Web API は、URI からバインドしようとしています) できるように、単純型としてクラスを処理を行うことができます、 **TypeConverter**し、文字列の変換を提供します。

次のコードは、 `GeoPoint` 、地理的なポイントを表すクラスと**TypeConverter**を文字列から変換する`GeoPoint`インスタンス。 `GeoPoint`でクラスを装飾、 **[TypeConverter]** 属性を型コンバーターを指定します。 (この例は、Mike Stall のブログの投稿で触発されました[MVC/WebAPI のアクション シグネチャでのカスタム オブジェクトをバインドする方法](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx))。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

これで、Web API は扱います`GeoPoint`として単純型、つまりバインドを試みます`GeoPoint`URI からのパラメーターです。 含める必要はありません **[FromUri]** パラメーターにします。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

クライアントは、次のような URI を持つメソッドを呼び出すことができます。

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>モデル バインダー

実行する型コンバーターよりもより柔軟なオプションでは、カスタム モデル バインダーを作成します。 モデル バインダーをルート データからなど、HTTP 要求、アクションの説明、および生の値にアクセス権があります。

モデル バインダーを作成するには、実装、 **IModelBinder**インターフェイスです。 このインターフェイスは、1 つのメソッドを定義**BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

ここでは、モデル バインダーを`GeoPoint`オブジェクト。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

モデル バインダーから生の入力値を取得する、*値プロバイダー*です。 この設計は、2 つの異なる関数を区切ります。

- 値プロバイダーは、HTTP 要求を受け取りし、キー/値ペアのディクショナリを設定します。
- モデル バインダーでは、このディクショナリを使用してモデルを作成します。

Web API の既定値のプロバイダーは、ルート データおよびクエリ文字列から値を取得します。 たとえば、次の URI が`http://localhost/api/values/1?location=48,-122`、値プロバイダーは、次のキー/値ペアを作成します。

- id = &quot;1&quot;
- 場所 = &quot;48,122&quot;

(これは既定のルート テンプレートを想定して&quot;api/{controller}/{id}&quot;)。

バインドするパラメーターの名前が格納されている、 **ModelBindingContext.ModelName**プロパティです。 モデル バインダー ディクショナリ内でこの値を持つキーを探します。 値が存在しに変換できる場合、 `GeoPoint`、モデル バインダーにバインドされた値が割り当てられます、 **ModelBindingContext.Model**プロパティです。

モデル バインダーの単純型の変換に制限がないことに注意してください。 この例ではモデル バインダーは、既知の場所のテーブル内で最初に検索し、型の変換を使用して、失敗した場合。

**モデル バインダーを設定**

モデル バインダーを設定するいくつかの方法はあります。 最初に、追加、 **[拡大する ModelBinder]** 属性をパラメーター。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

追加することも、 **[拡大する ModelBinder]** 属性を型にします。 Web API は、その型のすべてのパラメーターの指定したモデル バインダーを使用します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

最後にモデル バインダー プロバイダーを追加することができます、 **HttpConfiguration**です。 モデル バインダー プロバイダーは、単に、モデル バインダーを作成するファクトリ クラスです。 派生することで、プロバイダーを作成することができます、 [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx)クラスです。 ただし場合、モデル バインダーは、1 つの型を処理することが容易に組み込みを使用して**SimpleModelBinderProvider**、この目的になっています。 この方法を次のコードに示します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

モデル バインド プロバイダーを使用する必要がありますを追加、 **[拡大する ModelBinder]** 属性をパラメーターへのモデル バインダーおよびメディア タイプ フォーマッタではないのどちらを使用するように Web API を通知します。 ただし、属性にモデル バインダーの型を指定する必要はありませんようになりました。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>値プロバイダー

説明したようにモデル バインダーが値プロバイダーから値を取得します。 カスタム値プロバイダーを記述するには、実装、 **IValueProvider**インターフェイスです。 要求の cookie の値を取得する例を次に示します。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

派生することで、値プロバイダー ファクトリを作成する必要があります、 **ValueProviderFactory**クラスです。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

追加する値プロバイダー ファクトリ、 **HttpConfiguration**次のようにします。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web API がすべての値プロバイダーのために合成モデル バインダーを呼び出すと**ValueProvider.GetValue**、モデル バインダーは、それを生成できる最初の値プロバイダーから値を受け取ります。

使用して、パラメーター レベル値プロバイダー ファクトリを設定することができます、 **ValueProvider**属性が次のようにします。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

これは、Web API で指定された値プロバイダー ファクトリ モデル バインディングを使用して、その他の登録済みの値プロバイダーを使用しないことを示しています。

## <a name="httpparameterbinding"></a>HttpParameterBinding

モデル バインダーは、一般的なメカニズムの特定のインスタンスです。 確認する場合、 **[拡大する ModelBinder]** 属性が表示されます、抽象型から派生して**ParameterBindingAttribute**クラスです。 このクラスは、1 つのメソッドを定義**GetBinding**、返された、 **HttpParameterBinding**オブジェクト。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding**パラメーターの値にバインドを担当します。 場合、 **[拡大する ModelBinder]**、属性を返します、 **HttpParameterBinding**を使用して実装、 **IModelBinder**を実際のバインドを実行します。 独自に実装することも**HttpParameterBinding**です。

たとえばから Etag を取得する`if-match`と`if-none-match`要求のヘッダー。 Etag を表すクラスを定義することで始めます。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

ETag を取得するかどうかを示すために列挙体を定義してありますも、`if-match`ヘッダーまたは`if-none-match`ヘッダー。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

ここでは、 **HttpParameterBinding**を目的のヘッダーから ETag を取得し、ETag 型のパラメーターにバインドします。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync**メソッドでは、バインドします。 このメソッド内にバインドされたパラメーター値を追加、 **ActionArgument**ディクショナリで、 **HttpActionContext**です。

> [!NOTE]
> 場合、 **ExecuteBindingAsync**メソッドが要求メッセージの本文を読み取り、上書き、 **WillReadBody**プロパティが true を返します。 要求本文には、読み取り可能なのみ 1 回、Web API 規則を適用する 1 つのバインド、バッファリングされていないストリームは、メッセージ本文を読み取ることができます可能性があります。


カスタムを適用する**HttpParameterBinding**から派生する属性を定義する**ParameterBindingAttribute**です。 `ETagParameterBinding`、1 つずつ、2 つの属性を定義してあります`if-match`ヘッダーと 1 つずつ`if-none-match`ヘッダー。 抽象基本クラスから派生させます。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

ここでは、コント ローラー メソッドを使用する、`[IfNoneMatch]`属性。

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

それに**ParameterBindingAttribute**、カスタムを追加するための別のフック**HttpParameterBinding**です。 **HttpConfiguration**オブジェクト、 **ParameterBindingRules**プロパティは、型の anomymous 関数のコレクション (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**)。 たとえば、GET メソッド上の任意の ETag パラメーターを使用する規則を追加`ETagParameterBinding`で`if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

関数が返す必要があります`null`のパラメーターのバインドは適用されません。

## <a name="iactionvaluebinder"></a>IActionValueBinder

全体のパラメーター バインディング プロセスは、プラグ可能なサービスによって制御されます**IActionValueBinder**です。 既定の実装**IActionValueBinder**は次の実行します。

1. 探して、 **ParameterBindingAttribute**パラメーターにします。 これが含まれます **[FromBody]**、 **[FromUri]**、および **[拡大する ModelBinder]**、またはカスタム属性です。
2. それ以外の場合、ファイルの場所**HttpConfiguration.ParameterBindingRules**を null 以外を返す関数の**HttpParameterBinding**です。
3. それ以外の場合、先ほど説明した既定の規則を使用します。 

    - パラメーターの型「単純」が、型コンバーターがいる場合は、URI からバインドします。 これは配置することに相当、 **[FromUri]** パラメーターの属性です。
    - それ以外の場合、メッセージ本文からパラメーターの読み取りを再試行してください。 これは配置することに相当 **[FromBody]** パラメーターにします。

全体を置換できなかった場合は、 **IActionValueBinder**サービス、カスタム実装です。

## <a name="additional-resources"></a>その他のリソース

[カスタム パラメーター バインディングのサンプル](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Web API のパラメーター バインド詳細な一連のブログ投稿については、Mike Stall:

- [どのように Web API は、パラメーターのバインド](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Web API の MVC スタイル パラメーターのバインド](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [MVC または Web API のアクション シグネチャでのカスタム オブジェクトをバインドする方法](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Web API でカスタム値プロバイダーを作成する方法](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [内部で web API パラメーターのバインド](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
