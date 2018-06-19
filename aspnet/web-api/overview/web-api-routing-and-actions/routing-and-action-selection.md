---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: ルーティングと ASP.NET Web API の操作の選択 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043419"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>ルーティングと ASP.NET Web API の操作の選択
====================
によって[Mike Wasson](https://github.com/MikeWasson)

この記事では、ASP.NET Web API がコント ローラー上の特定のアクションに HTTP 要求をルーティングする方法について説明します。

> [!NOTE]
> ルーティングの大まかな概要については、次を参照してください。 [ASP.NET Web API でルーティング](routing-in-aspnet-web-api.md)です。


この記事は、ルーティング処理の詳細を検索します。 Web API プロジェクトを作成すると、いくつかの要求を取得しない検索期待どおりのルーティング、できればこの記事が役立ちます。

ルーティングと、次の 3 つの主要なフェーズがあります。

1. ルート テンプレートを URI が一致します。
2. コント ローラーを選択します。
3. アクションを選択します。

独自のカスタム ビヘイビアーの使用、プロセスの一部を置き換えることができます。 この記事では、既定の動作を説明します。 最後に、動作をカスタマイズできる場所には注意してください。

## <a name="route-templates"></a>ルート テンプレート

ルート テンプレートのような URI パスが、中かっこで示されているプレース ホルダーの値を持つことができます。

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

ルートを作成するときに、プレース ホルダーの一部またはすべての既定値を指定できます。

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

URI のセグメントがプレース ホルダーを照合する方法を制限する制約を指定することもできます。

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

フレームワークでは、テンプレートへの URI パスのセグメントを照合します。 テンプレート内のリテラルは、正確に一致する必要があります。 プレース ホルダーは制約を指定していない限り、任意の値と一致します。 フレームワークでは、URI、ホスト名またはクエリのパラメーターなどの他の部分が一致しません。 フレームワークは、URI に一致するルート テーブルの最初のルートを選択します。

2 つの特殊なプレース ホルダーがある:"{controller}"と"{action}"。

- "{controller}"は、コント ローラーの名前を提供します。
- "{action}"は、アクションの名前を提供します。 Web API では、通常の規則は、"{action}"を省略する場合です。

### <a name="defaults"></a>既定値

既定値を指定する場合、ルートはこれらのセグメントが欠落している URI に一致します。 例:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

URI"`http://localhost/api/products`"このルートと一致します。 「{カテゴリ}」セグメントには、"all"の既定値が割り当てられます。

### <a name="route-dictionary"></a>ルート ディクショナリ

フレームワークでは、URI の一致が見つかると、各プレース ホルダーの値を格納するディクショナリを作成します。 キーは、中かっこを含まない、プレース ホルダー名です。 値は、URI のパスとは、既定値から取得されます。 ディクショナリが格納されている、 **IHttpRouteData**オブジェクト。

このルートの一致フェーズ中に、特別な"{controller}"と"{action}"プレース ホルダーは、他のプレース ホルダーと同じように扱われます。 単にその他の値を使用してディクショナリに格納されます。

既定値は、特殊な値を持つことができます**RouteParameter.Optional**です。 プレース ホルダーには、この値が割り当てられているを取得する場合、値はルート ディクショナリに追加されません。 例:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

"Api/products"などの URI パスに対してルート ディクショナリが含まれます。

- コント ローラー:"products など"
- カテゴリ:"all"

「Api/製品/toys/123」、ただし、ルート ディクショナリが含まれます。

- コント ローラー:"products など"
- category: "toys"
- id: "123"

既定値は、ルート テンプレートで任意の場所に表示されていない値を含めることができますも。 一致する場合は、その値がディクショナリに格納します。 例:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

URI のパスが「api/ルート/8」の場合は、2 つの値がディクショナリに含まれます。

- コント ローラー:"customers"
- id: "8"

## <a name="selecting-a-controller"></a>コント ローラーを選択します。

コント ローラーの選択はによって処理される、 **IHttpControllerSelector.SelectController**メソッドです。 このメソッドは、 **HttpRequestMessage**インスタンスを返す、 **HttpControllerDescriptor**です。 によって既定の実装が提供される、 **DefaultHttpControllerSelector**クラスです。 このクラスは、単純なアルゴリズムを使用します。

1. キー"controller"のルート ディクショナリを参照してください。
2. このキーの値を取得し、文字列、コント ローラーの種類名を取得するには、"Controller"を追加します。
3. Web API コント ローラーではこの型の名前を探します。

たとえば、ルート ディクショナリに"controller"キー/値ペアが含まれている場合 ="products"など、コント ローラーの種類が"ProductsController"します。 一致する型がない、または複数の一致がある場合、フレームワークは、クライアントにエラーを返します。

手順 3. の**DefaultHttpControllerSelector**を使用して、 **IHttpControllerTypeResolver** Web API コント ローラーの種類の一覧を取得するインターフェイスです。 既定の実装**IHttpControllerTypeResolver** (a) 実装するすべてのパブリック クラスを返します**IHttpController**、(b) は抽象化、されず (c)"Controller"で終わる名前であります。

## <a name="action-selection"></a>操作の選択

コント ローラーを選択すると、フレームワークは呼び出すことによって、操作を選択、 **IHttpActionSelector.SelectAction**メソッドです。 このメソッドは、 **HttpControllerContext**を返します、 **HttpActionDescriptor**です。

によって既定の実装が提供される、 **ApiControllerActionSelector**クラスです。 アクションを選択するには、次を検索します。

- 要求の HTTP メソッド。
- ルート テンプレートで、存在する場合、「{アクション}」プレース ホルダーです。
- コント ローラーのアクションのパラメーターです。

選択アルゴリズムを見る前に、コント ローラー アクションのいくつかの点を理解する必要があります。

**コント ローラーのメソッドは、「アクション」と見なされますか。** アクションを選択すると、フレームワークのみではパブリック インスタンス メソッド、コント ローラーでします。 また、除外[「特別な名前」](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname)メソッド (コンス トラクター、イベント、演算子のオーバー ロードなど)、およびから継承されたメソッド、 **ApiController**クラスです。

**HTTP メソッド。** のみ、フレームワークには、次のように、要求の HTTP メソッドと一致するアクションが選択されます。

1. 属性を持つ HTTP メソッドを指定できます: **AcceptVerbs**、 **HttpDelete**、 **HttpGet**、 **HttpHead**、 **HttpOptions**、 **HttpPatch**、 **HttpPost**、または**HttpPut**です。
2. それ以外の場合、コント ローラーのメソッドの名前は、"Get"、"Post"、"Put"、"Delete"、"Head"、「オプション」または「修正」で始まる場合、規則によって、アクションがサポートする HTTP メソッド。
3. 上記のいずれの場合、メソッドは POST をサポートします。

**パラメーター バインディング。** パラメーターのバインドは、Web API が、パラメーターの値を作成する方法です。 パラメーター バインディングの既定の規則を次に示します。

- 単純型は、URI から取得されます。
- 複合型は、要求本文から取得されます。

単純型には、すべてが含まれている、 [.NET Framework のプリミティブ型](https://msdn.microsoft.com/library/system.type.isprimitive)、plus **DateTime**、 **Decimal**、 **Guid**、**文字列**、および**TimeSpan**です。 アクションごとに最大で 1 つのパラメーターは、要求本文を読み取ることができます。

> [!NOTE]
> 既定のバインディング規則を上書きすることは。 参照してください[フードの下部 WebAPI パラメーター バインド](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)です。


その背景には、アクションの選択アルゴリズムを次に示します。

1. HTTP 要求メソッドに一致するコント ローラーのすべてのアクションの一覧を作成します。
2. ルート ディクショナリに、"action"エントリがある場合は、名前を持つが、この値と一致しませんアクションを削除します。
3. 次のように、uri、アクション パラメーターを一致するように再試行してください。 

    1. 各アクションの単純型の場合は、バインディングが、URI からパラメーターを取得するパラメーターの一覧を取得します。 省略可能なパラメーターを除外します。
    2. この一覧から、ルート ディクショナリまたは URI クエリ文字列内に各パラメーターの名前の一致を見つけようとします。 一致は、大文字と小文字パラメーターの順序に依存しません。
    3. リスト内のすべてのパラメーターが URI で一致するものがアクションを選択します。
    4. 複数のアクションがこれらの条件を満たしている場合は、ほとんどのパラメーターと一致すると、1 つを選択します。
4. 使って操作を無視する、 **[NonAction]** 属性。

手順 3 では最も難しい可能性があります。 基本的な考え方は、パラメーターは、URI、要求本文からまたはカスタム バインドからはその値を取得できます。 URI に由来するパラメーターの場合、URI が (ルート ディクショナリ) 経由でパスまたはクエリ文字列にそのパラメーターの値には実際に含まれていることを確認することができます。

たとえば、次の操作があるとします。

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*Id*パラメーターの URI にバインドします。 したがって、この操作では、"id", ルート ディクショナリまたはクエリ文字列内の値を含む URI が一致だけことができます。

省略可能なパラメーターは例外をためには省略可能です。 省略可能なパラメーターでは [ok]、バインドは、URI から値を取得できない場合です。

複合型では、例外を別の理由。 複合型は、カスタム バインディングから URI にのみバインドできます。 その場合は、フレームワーク前もってわかりませんパラメーターは、特定の URI にバインドされているかどうか。 を調べるにバインディングを呼び出すことはできます。 選択アルゴリズムの目的では、任意のバインディングを呼び出す前に、静的な記述からアクションを選択します。 そのため、複合型は、一致のアルゴリズムから除外されます。

アクションを選択すると、すべてのパラメーター バインドが呼び出されます。

概要:

- アクションは、要求の HTTP メソッドと一致する必要があります。
- アクション名は、存在する場合ルート ディクショナリで"action"エントリを一致する必要があります。
- アクションのすべてのパラメーターの場合は、パラメーターは、URI から取得し、パラメーター名必要がありますが見つかりませんルート ディクショナリまたは URI クエリ文字列。 (省略可能なパラメーターと複合型のパラメーターは除外されます。)
- パラメーター数が最も一致するように再試行してください。 最適なパラメーターなしでメソッドがあります。

## <a name="extended-example"></a>拡張の例

ルート:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

コントローラー:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP 要求。

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>ルートの照合

URI では、"DefaultApi"をという名前のルートと一致します。 ルート ディクショナリには、次のエントリが含まれています。

- コント ローラー:"products など"
- id: "1"

ルート ディクショナリに、クエリ文字列パラメーター、「バージョン」と「詳細」が含まれていませんが、これらも操作の選択時に考慮されます。

### <a name="controller-selection"></a>コント ローラーの選択

コント ローラーの種類には、"controller"のエントリ ルート ディクショナリから、`ProductsController`です。

### <a name="action-selection"></a>操作の選択

HTTP 要求は、GET 要求です。 GET をサポートするコント ローラー アクションは`GetAll`、 `GetById`、および`FindProductsByName`です。 ルート ディクショナリは、"action"のエントリを含まないため、操作名と一致する必要はありません。

次に、しようと、アクションのパラメーター名と一致する GET 操作だけを見ています。

| アクション | 一致するパラメーター |
| --- | --- |
| `GetAll` | none |
| `GetById` | "id" |
| `FindProductsByName` | "name" |

注意して、*バージョン*のパラメーター`GetById`とは見なされません、省略可能なパラメーターであるためです。

`GetAll`普通のメソッドに一致します。 `GetById`メソッドも一致すると、ルート ディクショナリには、"id"が含まれているためです。 `FindProductsByName`メソッドと一致しません。

`GetById`のパラメーターなしではなく、1 つのパラメーターと一致するので、メソッドが wins`GetAll`です。 このメソッドは、次のパラメーター値。

- *id* = 1
- *バージョン*1.5 を =

たとえことに注意して*バージョン*は使用されませんでした選択アルゴリズムのパラメーターの値は URI クエリ文字列から取得します。

## <a name="extension-points"></a>拡張ポイント

Web API は、ルーティング プロセスの一部の拡張ポイントを提供します。

| Interface | 説明 |
| --- | --- |
| **IHttpControllerSelector** | コント ローラーを選択します。 |
| **IHttpControllerTypeResolver** | コント ローラーの種類の一覧を取得します。 **DefaultHttpControllerSelector**がこの一覧からコント ローラーの種類を選択します。 |
| **IAssembliesResolver** | プロジェクト アセンブリの一覧を取得します。 **IHttpControllerTypeResolver**インターフェイスでは、この一覧を使用して、コント ローラーの種類を検索します。 |
| **IHttpControllerActivator** | コント ローラーの新しいインスタンスを作成します。 |
| **IHttpActionSelector** | アクションを選択します。 |
| **IHttpActionInvoker** | アクションを呼び出します。 |

これらのインターフェイスのいずれにも、独自の実装を提供するには、使用、 **Services**コレクションに、 **HttpConfiguration**オブジェクト。

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
