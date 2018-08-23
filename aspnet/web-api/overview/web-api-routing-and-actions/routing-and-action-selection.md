---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: ルーティングと ASP.NET Web API の操作の選択 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/27/2012
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: b4912d3ee1e13651f2a63d54d7dbfd92e00f85f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838885"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>ルーティングと ASP.NET Web API の操作の選択
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

この記事では、ASP.NET Web API が特定のコント ローラーのアクションに HTTP 要求をルーティングする方法について説明します。

> [!NOTE]
> ルーティングの大まかな概要については、次を参照してください。 [ASP.NET Web API におけるルーティング](routing-in-aspnet-web-api.md)します。


この記事では、ルーティング処理の詳細を検索します。 Web API プロジェクトを作成して、一部の要求を取得しない検索期待どおりのルーティング、うまくいけばこの記事では役立ちます。

ルーティングと、次の 3 つの主要なフェーズがあります。

1. ルート テンプレートの URI と照合します。
2. コント ローラーを選択します。
3. アクションを選択します。

プロセスの一部を独自のカスタム動作と置き換えることができます。 この記事では、既定の動作を取り上げます。 最後に、私には動作をカスタマイズできる場所に注意してください。

## <a name="route-templates"></a>ルート テンプレート

ルート テンプレートのように URI のパスが、プレース ホルダーの値を中かっこで示されることができます。

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

ルートを作成するときに、プレース ホルダーの一部またはすべてに対して既定値を指定できます。

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

URI セグメントがプレース ホルダーを照合する方法を制限する制約を指定することもできます。

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

フレームワークは、テンプレートの URI パスのセグメントの一致を試みます。 テンプレート内のリテラルは、正確に一致する必要があります。 プレース ホルダーは制約を指定しない限り、任意の値と一致します。 フレームワークが URI ホスト名またはクエリ パラメーターなどの他の部分と一致しません。 フレームワークは、URI と一致するルート テーブルの最初のルートを選択します。

2 つの特殊なプレース ホルダーがある。"{controller}"と"{action}"。

- "{controller}"は、コント ローラーの名前を提供します。
- "{action}"は、アクションの名前を提供します。 Web api では、通常の規則は、"{action}"を省略する場合は。

### <a name="defaults"></a>既定値

既定値を指定する場合、ルートはこれらのセグメントが不足している URI に一致します。 例えば:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

URI"`http://localhost/api/products`"このルートと一致します。 "{Category}"セグメントには、「すべて」の既定値が割り当てられます。

### <a name="route-dictionary"></a>ルート ディクショナリ

フレームワークには、URI の一致が見つかると、各プレース ホルダーの値を格納しているディクショナリを作成します。 キーは、プレース ホルダー名は、中かっこは含まれません。 値は、URI のパスとは、既定値から取得されます。 ディクショナリが格納されている、 **IHttpRouteData**オブジェクト。

このルートの一致フェーズ中に、特別な"{controller}"と"{action}"プレース ホルダーは、他のプレース ホルダーと同じように扱われます。 単にその他の値をディクショナリに格納されます。

既定値は、特殊な値を持つことができます**RouteParameter.Optional**します。 プレース ホルダーには、この値が割り当てられている取得する場合、値はルート ディクショナリに追加されません。 例えば:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

URI のパス「api/製品」では、ルート ディクショナリが含まれます。

- コント ローラー:"products"
- カテゴリ:"all"

「Api/製品/toys/123」、ただし、ルート ディクショナリが含まれます。

- コント ローラー:"products"
- category: "toys"
- id:「123」

既定値は、ルート テンプレートで任意の場所に表示されていない値を含めることができますも。 ルートが一致すると、その値がディクショナリに格納します。 例えば:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

URI のパスが「api/ルート/8」の場合は、2 つの値がディクショナリに含まれます。

- コント ローラー:"customers"
- id:「8」

## <a name="selecting-a-controller"></a>コント ローラーを選択します。

コント ローラーの選択がによって処理される、 **IHttpControllerSelector.SelectController**メソッド。 このメソッドは、 **HttpRequestMessage**インスタンスを返す、 **HttpControllerDescriptor**します。 によって既定の実装が提供される、 **DefaultHttpControllerSelector**クラス。 このクラスは、単純なアルゴリズムを使用します。

1. キー"controller"のルート ディクショナリを参照してください。
2. このキーの値を取得し、コント ローラーの種類の名前を取得するには、"Controller"という文字列を追加します。
3. この型の名前と Web API コント ローラーを探します。

たとえば、ルート ディクショナリに"controller"キーと値のペアが含まれている場合 =「製品」、コント ローラーの種類が"ProductsController"します。 一致する型がない、または複数の一致がある場合、フレームワークは、クライアントにエラーを返します。

手順 3. の**DefaultHttpControllerSelector**を使用して、 **IHttpControllerTypeResolver**インターフェイスが Web API コント ローラーの種類の一覧を取得します。 既定の実装**IHttpControllerTypeResolver** (a) 実装するすべてのパブリック クラスを返します**IHttpController**、(b) はない抽象化、および (c)"Controller"で終わる名前があります。

## <a name="action-selection"></a>アクションの選択

コント ローラーを選択すると、フレームワークが呼び出すことによってアクションを選択、 **IHttpActionSelector.SelectAction**メソッド。 このメソッドは、 **HttpControllerContext**を返します、 **HttpActionDescriptor**します。

によって既定の実装が提供される、 **ApiControllerActionSelector**クラス。 アクションを選択するには、次の場所になります。

- 要求の HTTP メソッド。
- 存在する場合はルート テンプレートで"{action}"プレース ホルダーです。
- コント ローラーのアクションのパラメーター。

選択アルゴリズムを見る前に、コント ローラー アクションのいくつかの点を理解する必要があります。

**コント ローラーのメソッドは、「アクション」と見なされますか。** アクションを選択すると、フレームワークのみパブリック インスタンス メソッドを調べ、コント ローラー。 また、バインドファイル[「特別な名前」](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname)メソッド (コンス トラクター、イベント、演算子のオーバー ロードおよびなど) とから継承されたメソッド、 **ApiController**クラス。

**HTTP メソッド。** フレームワークには、次のように、要求の HTTP メソッドに一致するアクションのみ選択されます。

1. 属性を使用する HTTP メソッドを指定できます: **AcceptVerbs**、 **HttpDelete**、 **HttpGet**、 **HttpHead**、 **HttpOptions**、 **HttpPatch**、 **HttpPost**、または**HttpPut**します。
2. それ以外の場合、コント ローラー メソッドの名前は、"Get"、"Post"、"Put"、"Delete"、"Head"、「オプション」または「パッチ」で始まる場合、規則によって、アクションがサポートする HTTP メソッド。
3. 上記のいずれの場合、メソッドは POST をサポートします。

**パラメーター バインディング。** パラメーターのバインドでは、Web API がパラメーターの値を作成する方法です。 パラメーター バインディングの既定のルールを次に示します。

- 単純型は、URI から取得されます。
- 複合型は、要求本文から取得されます。

すべての単純型が含まれます、 [.NET Framework のプリミティブ型](https://msdn.microsoft.com/library/system.type.isprimitive)、plus **DateTime**、 **Decimal**、 **Guid**、**文字列**、および**TimeSpan**します。 アクションごとに最大で 1 つのパラメーターは、要求本文を読み取ることができます。

> [!NOTE]
> 既定のバインディング規則を上書きすることになります。 参照してください[内部的には、WebAPI パラメーター バインド](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)します。


を踏まえて、アクションの選択アルゴリズムを示します。

1. HTTP 要求メソッドに一致するコント ローラーのすべてのアクションの一覧を作成します。
2. ルート ディクショナリに、"action"エントリがある場合は、名前がこの値と一致しないアクションを削除します。
3. 次のように、URI にアクション パラメーターを照合しようとしてください。 

    1. アクションごとに、単純型は、バインディングが、URI からパラメーターを取得するパラメーターの一覧を取得します。 省略可能なパラメーターを除外します。
    2. この一覧から、ルート ディクショナリで、または URI クエリ文字列では、各パラメーター名と一致するを検索してください。 一致は、大文字と小文字を区別しない、パラメーターの順序に依存しません。
    3. リスト内のすべてのパラメーターが URI に一致するものがアクションを選択します。
    4. その 1 つのアクションよりがこれらの条件を満たしている場合は、1 つと、ほとんどのパラメーターに一致するを選択します。
4. アクションは、無視、 **[NonAction]** 属性。

ステップ 3 は、おそらく最もわかりにくいです。 基本的な考え方は、パラメーターは、URI から、要求本文またはカスタム バインドからはその値を取得できます。 URI から取得したパラメーターの場合、URI に実際には、パス (ルート ディクショナリ) 経由で、またはクエリ文字列にそのパラメーターの値が含まれていることを確認します。

たとえば、次のアクションがあるとします。

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*Id*パラメーターが URI にバインドします。 したがって、このアクションでは、"id", ルート ディクショナリまたはクエリ文字列内のいずれかの値を含む URI が一致のみできます。

省略可能なために、省略可能なパラメーターは、例外は。 省略可能なパラメーターでは、[ok] 場合は、バインドは、URI から値を取得することはできません。

複合型では、例外を別の理由。 複合型は、カスタム バインディングを介して URI にのみバインドできます。 その場合は、フレームワークことはできません、あらかじめ調べておくパラメーターは、特定の URI にバインドされているかどうか。 確認するには、バインディングを呼び出し、必要があります。 選択アルゴリズムの目的では、すべてのバインドを呼び出す前に、静的な説明からアクションを選択します。 そのため、複合型は、一致のアルゴリズムから除外されます。

アクションを選択すると、すべてのパラメーター バインドが呼び出されます。

概要:

- アクションは、要求の HTTP メソッドと一致する必要があります。
- アクション名は、存在する場合、ルート ディクショナリで"action"エントリを一致する必要があります。
- アクションのすべてのパラメーターの場合は、パラメーターは、URI から取得し、パラメーター名いる必要がありますルート ディクショナリまたは URI クエリ文字列。 (省略可能なパラメーターと複雑な種類のパラメーターは除外されます。)
- パラメーターの数、最も一致するようにしてみてください。 最も一致するパラメーターなしのメソッドがあります。

## <a name="extended-example"></a>詳細な例

ルート:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

コントローラー:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP 要求:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>ルートの照合

URI では、"DefaultApi"という名前のルートと一致します。 ルート ディクショナリには、次のエントリが含まれています。

- コント ローラー:"products"
- id: "1"

クエリ文字列パラメーター、"version"と「詳細」を含まないルート ディクショナリが、これらのアクションの選択中にも考慮されます。

### <a name="controller-selection"></a>コント ローラーの選択

コント ローラーの種類は、ルート ディクショナリの"controller"エントリから`ProductsController`します。

### <a name="action-selection"></a>アクションの選択

HTTP 要求は、GET 要求です。 GET をサポートするコント ローラー アクションが`GetAll`、 `GetById`、および`FindProductsByName`します。 ルート ディクショナリは、"action"のエントリを含まないため、操作名と一致する必要はありません。

次に、私たち見てみてください、アクションのパラメーターの名前に一致するようにのみ、GET アクション。

| アクション | 一致するパラメーター |
| --- | --- |
| `GetAll` | none |
| `GetById` | "id" |
| `FindProductsByName` | "name" |

注意、*バージョン*パラメーターの`GetById`とは見なされません、オプションのパラメーターがあるためです。

`GetAll`メソッドに普通に一致します。 `GetById`メソッドも一致すると、ルート ディクショナリには、"id"が含まれています。 `FindProductsByName`メソッドと一致しません。

`GetById`のパラメーターなしではなく、1 つのパラメーターに一致するため、メソッドが wins`GetAll`します。 メソッドは、次のパラメーター値で呼び出されます。

- *id* = 1
- *バージョン*1.5 を =

*バージョン*使用されませんでした選択アルゴリズムでパラメーターの値は、URI クエリ文字列から取得します。

## <a name="extension-points"></a>拡張ポイント

Web API は、ルーティングのプロセスの一部の拡張ポイントを提供します。

| Interface | 説明 |
| --- | --- |
| **IHttpControllerSelector** | コント ローラーを選択します。 |
| **IHttpControllerTypeResolver** | コント ローラーの種類の一覧を取得します。 **DefaultHttpControllerSelector**はこの一覧から、コント ローラーの種類を選択します。 |
| **IAssembliesResolver** | プロジェクト アセンブリの一覧を取得します。 **IHttpControllerTypeResolver**インターフェイスでは、このリストを使用して、コント ローラーの種類を検索します。 |
| **IHttpControllerActivator** | コント ローラーの新しいインスタンスを作成します。 |
| **IHttpActionSelector** | アクションを選択します。 |
| **IHttpActionInvoker** | アクションを呼び出します。 |

これらのインターフェイスのいずれかの独自の実装を提供するには、使用、**サービス**コレクションに、 **HttpConfiguration**オブジェクト。

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
