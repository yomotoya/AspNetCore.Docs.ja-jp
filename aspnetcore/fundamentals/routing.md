---
title: "ASP.NET Core でのルーティング"
author: ardalis
description: "ASP.NET Core ルーティング機能が、受信要求をルート ハンドラーにマップするために対応する方法を検出します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/routing
ms.openlocfilehash: ffa3178dc4e3aac3ba51c29b7efa3f71eb56bcfe
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core でのルーティング

によって[Ryan Nowak](https://github.com/rynowak)、 [Steve Smith](https://ardalis.com/)、および[Rick Anderson](https://twitter.com/RickAndMSFT)

ルーティング機能は、受信要求をルート ハンドラーにマップします。 ルートが ASP.NET アプリケーションで定義されているし、アプリの起動時に構成されています。 ルート、要求に含まれている URL から値を抽出できます必要に応じて、要求の処理のため、これらの値を使用し、ことができます。 ASP.NET アプリケーションからのルート情報を使用して、ルーティング機能もルート ハンドラーにマップする Url を生成することができます。 そのため、ルーティング URL、またはハンドラーのルート情報に基づいて指定されたルート ハンドラーに対応する URL に基づくルート ハンドラーを見つけることができます。

>[!IMPORTANT]
> このドキュメントでは、ルーティング低レベルの ASP.NET Core について説明します。 ASP.NET Core MVC ルーティングを参照してください[コント ローラー アクションへのルーティング](../mvc/controllers/routing.md)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="routing-basics"></a>ルーティングの基礎

ルーティングは*ルート*(の実装[IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) にします。

* 着信要求をマップ*ハンドラーのルーティング*

* 応答で使用される Url を生成します。

通常、アプリは、単一のルートのコレクションを持っています。 要求が到着すると、ルートのコレクションが順番に処理します。 受信要求が呼び出すことによって、要求 URL に一致するルートの検索、`RouteAsync`ルート コレクションで使用可能な各ルートのメソッドです。 これに対し、応答は、ルーティングを使用して、ルート情報に基づいて (たとえば、リダイレクトまたはリンク) の Url を生成して、したがってせずに済むようハード コード Url、保守性にも役立ちます。

接続されているルーティング、[ミドルウェア](middleware.md)によってパイプライン、`RouterMiddleware`クラスです。 [ASP.NET MVC](../mvc/overview.md)追加の構成の一部としてのミドルウェア パイプラインにルーティングします。 詳細については、スタンドアロン コンポーネントとしてルーティングを使用して、次を参照してください。[を使用して、ルーティングのミドルウェア](#using-routing-middleware)です。

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>一致する URL

URL の一致に入力方向の要求をルーティングするディスパッチによってプロセスでは、*ハンドラー*です。 この処理は通常、データを基に、URL パスが、要求内のデータを考慮に拡張できます。 ハンドラーを独立したに要求をディスパッチする機能は、サイズと、アプリケーションの複雑さをスケーリングするキーです。

受信要求を入力してください、 `RouterMiddleware`、呼び出し、`RouteAsync`シーケンス内の各ルートのメソッドです。 `IRouter`インスタンスを選択するかどうか*処理*を設定して、要求、 `RouteContext.Handler` null 以外の値に`RequestDelegate`です。 ルートは、要求のハンドラーを設定、要求を処理するルートの処理が停止し、ハンドラーが呼び出されます。 ミドルウェアが呼び出すすべてのルートが試みられたの要求に対するハンドラーが見つからない場合は、*次*され、要求パイプラインの次のミドルウェアが呼び出されます。

入力をプライマリ`RouteAsync`は、`RouteContext.HttpContext`現在の要求に関連付けられています。 `RouteContext.Handler`と`RouteContext.RouteData`ルートに一致した後に設定される出力します。

中に一致する`RouteAsync`のプロパティを設定しても、`RouteContext.RouteData`をこれまでに実行、要求の処理に基づく適切な値です。 ルート、要求に一致する場合、`RouteContext.RouteData`に関する重要な状態情報を格納する、*結果*です。

`RouteData.Values`dictionary です*ルート値*をルートから生成します。 これらの値では、通常、URL のトークン化によって決定され、ユーザー入力を受け付けるか、アプリケーション内のさらにディスパッチ意思決定を行うために使用できます。

`RouteData.DataTokens`一致したルートに関連するその他のデータのプロパティ バッグがします。 `DataTokens`アプリケーションが後でどのルートに基づく決定を行うができるように各ルートを使用してデータが一致する関連付けられた状態をサポートするために提供されます。 これらの値は開発者が定義され、操作を行います**いない**任意の方法でルーティングの動作に影響します。 さらに、データのトークンの格納値は、任意の型のルートの値は、文字列との間に簡単に変換できる必要がありますとは対照的できます。

`RouteData.Routers`要求を正常に一致するときに関与するルートの一覧を示します。 ルートは、別内にネストすることができます、`Routers`プロパティの検索一致で生成されるルートの論理ツリーのパスに反映されます。 一般に、最初の項目`Routers`ルートのコレクションは、URL の生成に使用する必要があります。 最後の項目`Routers`に一致するルート ハンドラーします。

### <a name="url-generation"></a>URL の生成

URL の生成は、処理、ルーティングでルートの値のセットに基づく URL パスを作成できます。 これにより、ハンドラーとそれらにアクセスする Url の間、論理的な分離できます。

URL の生成と同様の反復的なプロセスに従いますを呼び出すコードをユーザーまたはフレームワーク、`GetVirtualPath`ルートのコレクションのメソッドです。 各*ルート*がその`GetVirtualPath`メソッドが null でないまで順序で呼び出されます`VirtualPathData`が返されます。

プライマリの入力先`GetVirtualPath`は。

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

ルートは主にによって提供される、ルート値を使用して、`Values`と`AmbientValues`URL を生成する可能性がある値を含めるかを判断します。 `AmbientValues`ルーティングのシステムでは、現在の要求を照合から生成されたルート値のセットです。 これに対し、`Values`は現在の操作の必要な URL を生成する方法を指定するルートの値。 `HttpContext`ルートは、サービス、または現在のコンテキストに関連付けられている追加のデータを取得する必要がある場合に提供します。

ヒント: 考える`Values`の上書きのセットとして、`AmbientValues`です。 URL の生成は、同じルートまたはルートの値を使用してリンクの Url を生成するが簡単に現在の要求からのルート値を再利用しようとします。

出力`GetVirtualPath`は、`VirtualPathData`です。 `VirtualPathData`並列は`RouteData`; が含まれている、`VirtualPath`出力 URL としていくつか追加プロパティをルートで設定する必要があります。

`VirtualPathData.VirtualPath`プロパティが含まれています、*仮想パス*ルートによって生成されます。 必要に応じて、さらに、パスを処理する必要があります。 たとえば、html 形式で生成された URL を表示する場合は、前に、アプリケーションのベース パスを付加する必要があります。

`VirtualPathData.Router` URL が正常に生成されるルートへの参照です。

`VirtualPathData.DataTokens`プロパティは、URL を生成するルートに関連するその他のデータのディクショナリ。 これは、並列`RouteData.DataTokens`です。

### <a name="creating-routes"></a>ルートの作成

ルーティングを提供、`Route`クラスの標準実装として`IRouter`です。 `Route`使用して、*ルート テンプレート*URL パスに対して一致するパターンを定義するための構文と`RouteAsync`と呼びます。 `Route`URL の生成に、同じルート テンプレートを使用するときに`GetVirtualPath`と呼びます。

ほとんどのアプリケーションが呼び出すことによってルートを作成、`MapRoute`で定義されているような拡張メソッドのいずれかまたは`IRouteBuilder`です。 これらのメソッドのすべてのインスタンスを作成`Route`ルートのコレクションに追加します。

注:`MapRoute`は、ルート ハンドラー パラメーターを受け取らないによって処理されるルートを追加するだけ、`DefaultHandler`です。 既定のハンドラーはなので、`IRouter`要求を処理するためにないことがあります。 たとえば、ASP.NET MVC 通常構成のみを処理する既定のハンドラーは、使用可能なコント ローラーとアクションに一致するを要求します。 Mvc ルーティングの詳細については、次を参照してください。[コント ローラー アクションへのルーティング](../mvc/controllers/routing.md)です。

これは、例、`MapRoute`典型的な ASP.NET MVC ルート定義で使用される呼び出し。

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

このテンプレートはのような URL パスを一致`/Products/Details/17`およびルート値を抽出`{ controller = Products, action = Details, id = 17 }`です。 ルートの値が決定される URL パスをセグメントに分割されそれぞれのセグメントに一致する、*パラメーターをルート*ルート テンプレートの名前。 ルート パラメーターがという名前です。 中かっこでパラメーター名を囲むことによって定義されている`{ }`です。

上記のテンプレートは、URL パスを一致もでした`/`と値が生成されると`{ controller = Home, action = Index }`です。 これは、`{controller}`と`{action}`ルート パラメーターが既定値を持つ、`id`ルートのパラメーターは省略可能です。 等号`=`記号続く値ルートのパラメーター名は、パラメーターの既定値を定義した後です。 疑問符 ()`?`後、ルートのパラメーター名は、オプションとして、パラメーターを定義します。 既定値を持つパラメーターをルート*常に*ルートに一致する - 対応する URL パスのセグメントがない場合、省略可能なパラメーターは、ルート値を生成しないはときに、ルート値を生成します。

参照してください[ルート テンプレート参照](#route-template-reference)ルート テンプレートの機能と構文の詳細な説明をします。

この例では、*制約をルーティング*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

このテンプレートはのような URL パスを一致`/Products/Details/17`、ではなく`/Products/Details/Apples`です。 ルートのパラメーター定義`{id:int}`定義、*制約をルーティング*の`id`パラメーターをルーティングします。 ルート制約を実装`IRouteConstraint`を確認するルートの値を調べるとします。 この例のルートの値で`id`整数に変換できる必要があります。 参照してください[ルート制約参照](#route-constraint-reference)フレームワークによって提供されるルート制約の詳細についてはします。

その他のオーバー ロード`MapRoute`の値を受け入れる`constraints`、 `dataTokens`、および`defaults`です。 これらの追加パラメーターの`MapRoute`型として定義する`object`です。 これらのパラメーターの一般的な使用法では、匿名型に一致するプロパティ名がパラメーター名をルーティングに匿名型のオブジェクトを渡します。

次の 2 つの例では、同等のルートを作成します。

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

ヒント: と既定の制約を定義するためのインライン構文を単純なルートの方が便利にすることができます。 ただし、これにはインライン構文でサポートされていないデータ トークンなどの機能があります。

この例より多くの機能をいくつかを示します。

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

このテンプレートはのような URL パスを一致`/Blog/All-About-Routing/Introduction`の値を抽出および`{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`です。 既定のルートの値を`controller`と`action`テンプレートに対応するルートのパラメーターがない場合でも、ルートによって生成されます。 既定値は、ルート テンプレートで指定できます。 `article`としてルートのパラメーターが定義されている、*キャッチ オール*アスタリスクの見た目で`*`ルートのパラメーター名の前にします。 キャッチ オール ルート パラメーターは、URL パスの残りの部分をキャプチャし、空の文字列を一致もできます。

この例は、ルートの制約とデータのトークンを追加します。

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

このテンプレートはのような URL パスを一致`/Products/5`の値を抽出および`{ controller = Products, action = Details, id = 5 }`およびデータ トークン`{ locale = en-US }`です。

![[ローカル] ウィンドウのトークン](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>URL の生成

`Route`クラスがそのルート テンプレートでルートの値のセットを組み合わせることで URL の生成も実行できます。 これは、URL パスに一致する逆のプロセスでは論理的にします。

ヒント: URL の生成を理解するのには、どのような URL を生成し、ルートのテンプレートがその URL を照合する方法について検討するを想像してください。 どのような値が生成されます。 これは、URL の生成がでどのように動作するためにのみ、相当、`Route`クラスです。

この例では、基本的な ASP.NET MVC スタイルのルートを使用します。

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

ルート値を持つ`{ controller = Products, action = List }`、このルートが URL の生成`/Products/List`です。 ルートの値は、URL パスを形成する対応するルート パラメーターの置換されます。 `id`は省略可能なパラメーターのルーティング、問題の値が含まれないことはありません。

ルート値を持つ`{ controller = Home, action = Index }`、このルートが URL の生成`/`です。 それらの値に対応するセグメントを安全に省略するための既定値を指定されたルートの値に一致します。 生成されたどちらの Url がこのルート定義のラウンド トリップは、URL の生成に使用された同じルートの値を生成ことに注意してください。

ヒント: ASP.NET MVC を使用してアプリを使用する必要があります`UrlHelper`を直接ルーティングを呼び出す代わりに Url を生成します。

URL の生成処理の詳細については、次を参照してください。 [url 生成参照](#url-generation-reference)です。

## <a name="using-routing-middleware"></a>ルーティングのミドルウェアを使用してください。

NuGet パッケージ"Microsoft.AspNetCore.Routing"を追加します。

追加のサービス コンテナーにルーティング*Startup.cs*:

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

ルートを構成する必要があります、`Configure`メソッドで、`Startup`クラスです。 次のサンプルでは、これらの Api を使用します。

* `RouteBuilder`
* `Build`
* `MapGet`HTTP GET 要求だけを一致します。
* `UseRouter`

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
{
    var trackPackageRouteHandler = new RouteHandler(context =>
    {
        var routeValues = context.GetRouteData().Values;
        return context.Response.WriteAsync(
            $"Hello! Route values: {string.Join(", ", routeValues)}");
    });

    var routeBuilder = new RouteBuilder(app, trackPackageRouteHandler);

    routeBuilder.MapRoute(
        "Track Package Route",
        "package/{operation:regex(^(track|create|detonate)$)}/{id:int}");

    routeBuilder.MapGet("hello/{name}", context =>
    {
        var name = context.GetRouteValue("name");
        // This is the route handler when HTTP GET "hello/<anything>"  matches
        // To match HTTP GET "hello/<anything>/<anything>,
        // use routeBuilder.MapGet("hello/{*name}"
        return context.Response.WriteAsync($"Hi, {name}!");
    });

    var routes = routeBuilder.Build();
    app.UseRouter(routes);
}
```

次の表は、指定された Uri に応答を示しています。

| URI | 応答  |
| ------- | -------- |
| /package/create/3  | Hello! ルート値: [操作の作成]、[id、3] |
| /package/track/-3  | Hello! ルート値: [操作、トラック]、[id、-3] |
| /package/track/-3/ | Hello! ルート値: [操作、トラック]、[id、-3]  |
| /package/track/ | \<フォール スルー、一致なし > |
| GET /hello/Joe | よろしくお願い Joe! |
| POST /hello/Joe | \<フォール スルーする、一致する HTTP GET のみ > |
| GET /hello/Joe/Smith | \<フォール スルー、一致なし > |

1 つのルートを構成する場合を呼び出す`app.UseRouter`を渡して、`IRouter`インスタンス。 呼び出す必要はありません`RouteBuilder`です。

フレームワークなどのルートを作成するための拡張メソッドのセットを提供します。

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

これらのメソッドのようにいくつか`MapGet`が必要、`RequestDelegate`を提供します。 `RequestDelegate`として使用される、*ルート ハンドラー*ルートに一致する場合。 このファミリの他のメソッドは、ルートのハンドラーとして使用されるミドルウェア パイプラインを構成できるようにします。 場合、*マップ*メソッド受け取らない、ハンドラーなど`MapRoute`が使用されます、`DefaultHandler`です。

`Map[Verb]`メソッドは、HTTP 動詞、メソッド名へのルートを制限する制約を使用します。 たとえばを参照してください[MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88)と[MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180)です。

## <a name="route-template-reference"></a>ルート テンプレート リファレンス

中かっこ内のトークン (`{ }`) 定義*パラメーターをルート*ルートが一致する場合がバインドされます。 セグメントでは、ルート、1 つ以上のルート パラメーターを定義できますが、これらは、リテラル値で区切る必要があります。 たとえば`{controller=Home}{action=Index}`できません有効なルートの間でのリテラル値が存在しないため`{controller}`と`{action}`です。 これらのルートのパラメーターには、名前が必要です。 と指定可能性があります追加の属性です。

ルートのパラメーターとは別のリテラル テキスト (たとえば、 `{id}`) およびパスの区切り文字`/`URL 内のテキストに一致する必要があります。 テキストの一致するとは区別されず、Url パスのデコードされた形式に基づいて。 リテラルのルート パラメーターの区切り記号に一致する`{`または`}`、文字の繰り返しでエスケープ (`{{`または`}}`)。

省略可能なファイル拡張子を持つファイル名をキャプチャしようとする URL パターンでは、追加の考慮事項があります。 たとえば、テンプレートを使用して`files/{filename}.{ext?}`: 両方`filename`と`ext`存在、両方の値が設定されます。 だけの場合`filename`URL、ルートの一致である末尾のピリオド`.`は省略可能です。 次の Url には、このルートは一致します。

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

使用することができます、`*`をプレフィックスとして文字 - URI の残りの部分にバインドするルートのパラメーターにこれと呼ばれる、*キャッチ オール*パラメーター。 たとえば、`blog/{*slug}`の使用を開始する任意の URI が一致する`/blog`され、それに続く任意の値 (に割り当てられます`slug`ルーティング値)。 キャッチ オール パラメーターでは、空の文字列が一致もできます。

ルート パラメーターがあります*既定値*で区切られたパラメーター名の後に既定値を指定することによって指定された、`=`です。 たとえば、`{controller=Home}`定義`Home`の既定値として`controller`です。 既定値は、値がない場合、URL でパラメーターに使用されます。 既定値だけでなく、ルート パラメーターが、省略可能な可能性があります (追加することによって指定された、`?`に示すように、パラメーター名の末尾に`id?`)。 オプションの違い、ルート パラメーターで、既定値は、値を常に生成は、「既定値あり」と提供されている場合にのみ、省略可能なパラメーターは値を持ちます。

ルート パラメーターには、URL からバインド ルートの値に一致する必要がありますの制約があります。 コロンを追加する`:`およびルートのパラメーター名を指定した後、制約の名前、*インライン制約*ルートのパラメーターにします。 制約には、かっこで囲まれたものは、指定された引数が必要な場合`( )`制約名の後にします。 もう 1 つのコロンを追加することによって複数のインライン制約を指定することができます`:`および制約の名前。 制約の名前が渡される、`IInlineConstraintResolver`のインスタンスを作成するサービスを`IRouteConstraint`URL の処理で使用します。 たとえば、ルート テンプレート`blog/{article:minlength(10)}`を指定します、`minlength`引数を指定した制約`10`です。 多くの説明のルート制約と、フレームワークによって提供される制約の一覧は、次を参照してください。[ルート制約参照](#route-constraint-reference)です。

次の表は、一部のルート テンプレートとその動作について説明します。

| ルート テンプレート | 一致する URL の例 | メモ |
| -------- | -------- | ------- |
| hello  | /hello  | 1 つのパスとのみ一致します。`/hello` |
| {Page=Home} | / | 設定し、一致`Page`に`Home` |
| {Page=Home}  | /にお問い合わせください。  | 設定し、一致`Page`に`Contact` |
| {controller}/{action}/{id?} | /製品/一覧表示 | マップ`Products`コント ローラーと`List`アクション |
| {controller}/{action}/{id?} | /Products/Details/123  |  マップ`Products`コント ローラーと`Details`アクション。  `id`123 に設定します。 |
| {controller=Home}/{action=Index}/{id?} | /  |  マップ`Home`コント ローラーと`Index`メソッドです。`id`は無視されます。 |

テンプレートを使用してルーティングするために最も簡単な方法では通常です。 制約と既定値は、ルート テンプレートの外部も指定できます。

ヒント: を有効にする[ログ](xref:fundamentals/logging/index)を表示する方法などのルーティングを実装に組み込まれている`Route`要求と一致します。

## <a name="route-constraint-reference"></a>ルート制約の参照

ときに実行されるルート制約、`Route`着信 URL の構文の一致があり、ルートの値に URL パスをトークン化します。 ルート制約は一般にルート テンプレートを使用して関連付けられているルート値を検査し、単純な加えて、はい/いいえの値が許容されるかどうかについて意思決定します。 何らかのルートの制約では、ルート値外のデータを使用して、要求をルーティングできるかどうかを検討してください。 たとえば、`HttpMethodRouteConstraint`受け入れるか、HTTP 動詞に基づいて要求を拒否することができます。

>[!WARNING]
> 制約を使用しないでください**入力検証**ようにすることによってにその無効な入力を行うと、適切なエラー メッセージで 400 の代わりに 404 (Not Found) が発生したため、します。 ルート制約をするために使用する必要があります**あいまいさを解消**間、特定のルートに対する入力の妥当性検査に類似のルートです。

次の表は、何らかのルートの制約と、予期される動作について説明します。

| 制約 | 例 | 例と一致します。 | メモ |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | 任意の整数を一致します。 |
| `bool`  | `{active:bool}` | `true`, `FALSE` | 一致`true`または`false`(大文字と小文字) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | 有効な一致`DateTime`値 (インバリアント カルチャのでは、警告を参照してください) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | 有効な一致`decimal`値 (インバリアント カルチャのでは、警告を参照してください) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | 有効な一致`double`値 (インバリアント カルチャのでは、警告を参照してください) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | 有効な一致`float`値 (インバリアント カルチャのでは、警告を参照してください) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | 有効な一致`Guid`値 |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | 有効な一致`long`値 |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | 文字列は 4 文字以上である必要があります。 |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | 文字列は 8 個を超える文字である必要があります。 |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | 文字列が正確に 12 文字にする必要があります。 |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | 文字列は、少なくとも 8 ~ ない 16 を超える文字である必要があります。 |
| `min(value)` | `{age:min(18)}` | `19` | 整数値は 18 以上である必要があります。 |
| `max(value)` | `{age:max(120)}` |  `91` | 整数値は 120 個である必要があります。 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | 整数値が少なくとも 18 が 120 未満にする必要があります。 |
| `alpha` | `{name:alpha}` | `Rick` | 1 つまたは複数の英文字の文字列必要があります (`a`-`z`、大文字と小文字) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | 文字列が正規表現に一致する必要があります (正規表現の定義に関するヒントを参照してください) |
| `required`  | `{name:required}` | `Rick` |  非パラメーターの値が URL の生成中に存在するを適用するために使用 |

>[!WARNING]
> CLR 型に変換できる URL を確認するルート制約 (など`int`または`DateTime`) 常にインバリアント カルチャを使用して - URL は非ローカライズ可能な前提です。 Framework に用意されているルートの制約では、ルートの値に格納された値は変更しないでください。 URL から解析されたすべてのルートの値は、文字列として格納されます。 たとえば、 [Float ルート制約](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60)ルートの値を float に変換を試みますが、変換後の値が float に変換できることを確認にのみ使用されます。

## <a name="regular-expressions"></a>正規表現 

ASP.NET Core framework 追加`RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant`正規表現のコンス トラクターにします。 参照してください[RegexOptions 列挙体](https://docs.microsoft.com/dotnet/api/system.text.regularexpressions.regexoptions)これらのメンバーの詳細についてはします。

正規表現は、区切り記号およびルーティングと c# 言語で使用されるようなトークンを使用します。 正規表現のトークンをエスケープする必要があります。 たとえば、正規表現を使用する`^\d{3}-\d{2}-\d{4}$`にルーティングする必要があるが、`\`としてに入力した文字`\\`エスケープするために c# ソース ファイルで、`\`エスケープ文字の文字列 (を使用しない限り、[逐語的文字列リテラル](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string)です。 `{` 、 `}` 、' [' と ']' 文字は、ルーティング パラメーター区切り記号の文字をエスケープするためにそれら 2 つ入力してエスケープする必要があります。  次の表は、正規表現とエスケープされたバージョンを示します。

| 正規表現               | メモ |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | 正規表現 |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | エスケープ  |
| `^[a-z]{2}$` | 正規表現 |
| `^[[a-z]]{{2}}$` | エスケープ  |

ルーティングに使用する正規表現が始まるは多くの場合、`^`文字 (文字列の開始位置に一致)、末尾が、`$`文字 (終了、文字列の位置に一致)。 `^`と`$`文字ことを確認する正規表現の一致、全体のルート パラメーターの値。 なし、`^`と`$`文字、正規表現のたくない多くの場合は、文字列内のサブ文字列は一致します。 次の表は、いくつかの例を示していて、一致、理由、または一致するように失敗するときを説明します。

| 正規表現               | String | 一致したもの | コメント |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | hello | 可 | 部分文字列の一致 |
| `[a-z]{2}` | 123abc456 | 可 | 部分文字列の一致 |
| `[a-z]{2}` | mz | 可 | 式に一致します。 |
| `[a-z]{2}` | MZ | 可 | 大文字と小文字が区別されません。 |
| `^[a-z]{2}$` |  hello | Ｘ | 参照してください`^`と`$`上 |
| `^[a-z]{2}$` |  123abc456 | Ｘ | 参照してください`^`と`$`上 |

参照してください[.NET Framework 正規表現](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)正規表現の構文についての詳細。

既知の使用可能な値のセットへのパラメーターを制限するには、正規表現を使用します。 たとえば`{action:regex(^(list|get|create)$)}`とのみ一致する、`action`に値をルーティング`list`、 `get`、または`create`です。 制約のディクショナリの文字列に渡された場合"^ (リスト | get | を作成) $"と同じです。 いずれかの既知の制限が一致しない制約ディクショナリ (テンプレート内のインラインではありません) に渡される制約は、正規表現としても扱われます。

## <a name="url-generation-reference"></a>生成の URL 参照

次の例は、ルート値のディクショナリを指定されたルートへのリンクを生成する方法を示しています。 および`RouteCollection`です。

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

`VirtualPath`は上記の例の最後に生成された`/package/create/123`です。

2 番目のパラメーター、`VirtualPathContext`コンス トラクターのコレクションは、*アンビエント値*です。 アンビエントの値は、開発者は、特定の要求のコンテキスト内で指定する必要があります値の数を制限することによって、利便性を提供します。 現在の要求の現在のルートの値は、リンクの生成にアンビエント値と見なされます。 などの場合は、ASP.NET MVC アプリケーションで、`About`のアクション、 `HomeController`、値を指定する、コント ローラーのルートにリンクする必要はありません、`Index`アクション (のアンビエント値`Home`が適用されます)。

パラメーターが一致しないアンビエントの値は無視され、アンビエント値は、明示的に指定値よりも優先、左から右へと向かうときにも無視されます、URL にします。

明示的に指定したが、何もこれと一致しない値は、クエリ文字列に追加されます。 ルート テンプレートを使用する場合、次の表は、結果を示しています`{controller}/{action}/{id?}`です。

| アンビエント値 | 明示的な値 | 結果 |
| -------------   | -------------- | ------ |
| コント ローラー「ホーム」を = | アクション"About"= | `/Home/About` |
| コント ローラー「ホーム」を = | コント ローラー"Order"、操作を = ="About" | `/Order/About` |
| controller="Home",color="Red" | アクション"About"= | `/Home/About` |
| コント ローラー「ホーム」を = | アクション = 色の「バージョン情報」、"Red"を = | `/Home/About?color=Red`

ルート、既定値は、パラメーターに対応していない、およびその値が明示的に指定される場合は、既定値に一致する必要があります。 例:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

リンクの生成では、コント ローラーとアクションの一致する値を提供する場合、このルートのリンクがのみ生成されます。
