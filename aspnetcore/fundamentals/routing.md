---
title: "ASP.NET Core のルーティング"
author: ardalis
description: "ASP.NET Core のルーティング機能が受信要求をルート ハンドラーにマッピングするしくみについて説明します。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/routing
ms.openlocfilehash: d35c24347e8e06ed85e2af8addcc1f8cf28dc47a
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core のルーティング

[Ryan Nowak](https://github.com/rynowak)、[Steve Smith](https://ardalis.com/)、[Rick Anderson](https://twitter.com/RickAndMSFT) 作

ルーティング機能は受信要求をルート ハンドラーにマッピングします。 ルートは ASP.NET アプリに定義され、アプリの起動時に構成されます。 ルートは、要求に含まれている URL から値を任意で抽出できます。その値を要求処理に利用できます。 ルーティング機能は、ASP.NET アプリからのルート情報を利用し、ルート ハンドラーにマッピングする URL を生成することもできます。 そのため、ルーティングでは URL に基づいて、つまり、ルート ハンドラー情報によって指定されるルート ハンドラーに対応する URL に基づいてルート ハンドラーを見つけることができます。

>[!IMPORTANT]
> 本文では、ASP.NET Core ルーティングについて詳しく取り上げます。 ASP.NET Core MVC ルーティングについては、「[コントローラー アクションへのルーティング](../mvc/controllers/routing.md)」を参照してください。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="routing-basics"></a>ルーティングの基本

ルーティングにおける*ルート* ([IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter) の実装) の利用目的:

* 受信要求を*ルート ハンドラー*にマッピングする

* 応答で使用される URL を生成する

一般的に、アプリにはルート コレクションが 1 つ与えられます。 要求が届くと、そのルート コレクションが順を追って処理されます。 受信要求は、ルート コレクションのルートごとに `RouteAsync` メソッドを呼び出し、要求 URL に一致するルートを探します。 それに対し、応答はルーティングを利用し、ルート情報に基づいて URL (リダイレクションやリンクの URL など) を生成できます。そのため、URL をハードコーディングする必要がなく、保守管理が楽になります。

ルーティングは `RouterMiddleware` クラスにより[ミドルウェア](xref:fundamentals/middleware/index) パイプラインに接続されます。 [ASP.NET Core MVC](xref:mvc/overview) は、ミドルウェア パイプラインにその構成の一部としてルーティングを追加します。 スタンドアロン コンポーネントとしてルーティングを利用する方法については、「[ルーティング ミドルウェアの使用](#using-routing-middleware)」を参照してください。

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>URL 一致

URL 一致というプロセスでは、ルーティングによって、受信要求が*ハンドラー*に配布されます。 このプロセスは通常、URL パスのデータに基づきますが、要求内のあらゆるデータを考慮することもできます。 アプリケーションの規模を大きくしたり、より複雑にしたりするとき、要求を個別のハンドラーに配布する機能が鍵となります。

受信要求は `RouterMiddleware` に入ります。これが各ルートで順に `RouteAsync` メソッドを呼び出します。 `IRouter` インスタンスは、`RouteContext.Handler` を非 null の `RequestDelegate` に設定することで、要求を*処理する*かどうかを選択します。 要求に適したハンドラーがルートに見つかると、ルート処理が停止します。ハンドラーが呼び出され、要求を処理します。 すべてのルートが試され、要求に適したハンドラーが見つからなかった場合、ミドルウェアは*次*を呼び出し、要求パイプラインの次のミドルウェアが呼び出されます。

`RouteAsync` に最初に入力されるのが、現在の要求に関連付けられている `RouteContext.HttpContext` です。 `RouteContext.Handler` と `RouteContext.RouteData` は、ルート一致後に設定される出力です。

また、`RouteAsync` で一致が見つかると、それまでに完了した要求処理に基づき、`RouteContext.RouteData` のプロパティが適切な値に設定されます。 ルートが要求に一致すると、`RouteContext.RouteData` に*結果*に関する重要なステータス情報が追加されます。

`RouteData.Values` は、ルートから生成された*ルート値*のディクショナリです。 この値は通常、URL のトークン化で決定されます。この値を利用してユーザー入力を受け取ったり、アプリケーション内で決定をさらに配布したりできます。

`RouteData.DataTokens` は、一致したルートに関連する追加データのプロパティ バッグです。 一致したルートに基づいてアプリケーションが後で決定を行えるように、`DataTokens` が与えられます。これがステータス データと各ルートの関連付けを支援します。 この値は開発者が定義するものです。ルーティングの動作に影響を与えることは**ありません**。 また、データ トークンに格納された値はどのような種類でも構いません。一方、ルート値は文字列に簡単に変換できる種類でなければなりません。

`RouteData.Routers` は、過去に要求に一致したルートの一覧です。 ルートはルートの中に入れ子にすることができます。`Routers` プロパティは、結果的に一致をもたらしたルートの論理ツリーを通るパスを表します。 一般的に、`Routers` の最初の項目はルート コレクションであり、これは URL 生成に使用するものです。 `Routers` の最後の項目は、一致したルート ハンドラーです。

### <a name="url-generation"></a>URL 生成

URL 生成は、ルーティングにおいて、一連のルート値に基づいて URL パスを作成するプロセスです。 これにより、ハンドラーとそれにアクセスする URL が論理的に分離します。

URL 生成は同様の繰り返しプロセスに従いますが、最初にユーザーまたはフレームワーク コードでルート コレクションの `GetVirtualPath` メソッドを呼び出します。 その結果、非 null の `VirtualPathData` が返されるまで各*ルート*で順番に `GetVirtualPath` メソッドが呼び出されます。

`GetVirtualPath` の第一入力:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

ルートは `Values` と `AmbientValues` が与えるルート値を第一に利用し、どこで URL を生成できるか、どの値を含めるかを決定します。 `AmbientValues` は、現在の要求をルーティング システムと照合することで生成された一連のルート値です。 対照的に、`Values` は、現在の操作に適した URL の生成方法を指定するルート値です。 ルートに現在のコンテキストに関連付けられているサービスまたは追加データを指定する必要がある場合、`HttpContext` が指定されます。

ヒント: `Values` は、`AmbientValues` のオーバーライド セットであると考えてください。 URL 生成では、現在の要求からのルート値を再利用するように試行されます。同じルートまたはルート値を利用することで、リンクの URL 生成を簡単にするのが目的です。

`GetVirtualPath` の出力は `VirtualPathData` です。 `VirtualPathData` は `RouteData` の並列です。これには出力 URL として `VirtualPath` が含まれ、また、ルートで設定する追加プロパティがいくつか含まれます。

`VirtualPathData.VirtualPath` プロパティには、ルートによって生成された*仮想パス*が含まれます。 必要に応じて、パスをさらに処理する必要があります。 たとえば、生成された URL を HTML で表示するには、アプリケーションの基礎パスを先頭に追加する必要があります。

`VirtualPathData.Router` は URL を正常に生成したルートの参照です。

`VirtualPathData.DataTokens` プロパティは、URL を生成したルートに関連する追加データのディクショナリです。 これは `RouteData.DataTokens` の並列です。

### <a name="creating-routes"></a>ルートの作成

ルーティングにより、`IRouter` の標準実装として `Route` クラスが与えられます。 `Route` は*ルート テンプレート*構文を利用し、`RouteAsync` の呼び出し時に URL パスに対して照合するパターンを定義します。 `Route` は同じルート テンプレートを利用し、`GetVirtualPath` の呼び出し時に URL を生成します。

ほとんどのアプリケーションは、`MapRoute` を呼び出すか、`IRouteBuilder` で定義されている同様の拡張メソッドの 1 つを呼び出してルートを作成します。 これらのメソッドはいずれも、`Route` のインスタンスを作成し、それをルート コレクションに追加します。

注: `MapRoute` はルート ハンドラー パラメーターを受け取りません。`DefaultHandler` によって処理されるルートを追加するだけです。 既定のハンドラーは `IRouter` であるため、要求を処理しないように決定されることがあります。 たとえば、ASP.NET MVC は通常、利用できるコントローラーやアクションに一致する要求のみを処理する既定のハンドラーとして構成されます。 MVC にルーティングする方法については、「[コントローラー アクションへのルーティング](../mvc/controllers/routing.md)」を参照してください。

一般的な ASP.NET MVC ルート定義によって使用される `MapRoute` 呼び出しの例:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

このテンプレートは `/Products/Details/17` のような URL パスを照合し、ルート値 `{ controller = Products, action = Details, id = 17 }` を抽出します。 ルート値は、URL パスをセグメントに分割し、各セグメントとルート テンプレートの*ルート パラメーター*を照合することで決定されます。 ルート パラメーターが指定されます。 括弧 `{ }` でパラメーターを囲むことで定義されます。

上のテンプレートは URL パス `/` を照合し、値 `{ controller = Home, action = Index }` を生成します。 これは、ルート パラメーターの `{controller}` と `{action}` に既定値が与えられ、`id` ルート パラメーターが任意であるためです。 ルート パラメーター名の後の等号 `=` とそれに続く値により、パラメーターの既定値が定義されます。 ルート パラメーター名の後の疑問符 `?` により、パラメーターがオプションとして定義されます。 既定値のルート パラメーターはルートが一致すると、ルート値を*常に*生成します。オプションのパラメーターは、対応する URL パス セグメントがなかった場合、ルート値を生成しません。

ルート テンプレートの機能と構文については、「[route-template-reference](#route-template-reference)」(ルート テンプレート参照) に詳しい説明があります。

*ルート制約*が含まれる例:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

このテンプレートは `/Products/Details/Apples` ではなく、`/Products/Details/17` のような URL パスを照合します。 ルート パラメーター定義 `{id:int}` により、`id` ルート パラメーターの*ルート制約*が定義されます。 ルート制約は `IRouteConstraint` を実装し、ルート値を調べて正しいことを確認します。 この例では、ルート値 `id` は整数に変換できなければなりません。 フレームワークによって提供されるルート制約については、「[ルート制約参照](#route-constraint-reference)」に詳しい説明があります。

`MapRoute` の追加オーバーロードは、`constraints`、`dataTokens`、`defaults` の値を受け取ります。 `MapRoute` のこのような追加パラメーターは `object` 型として定義されます。 このようなパラメーターは一般的に、匿名型のオブジェクトを渡し、匿名型のプロパティ名がルート パラメーター名と一致するというような使われ方をします。

同等のルートを作成する 2 つの例:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

ヒント: 制約と既定値を定義するインライン構文は単純なルートの場合に便利です。 ただし、インライン構文でサポートされないデータ トークンなどの機能があります。

その他の機能を示す例:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

このテンプレートは `/Blog/All-About-Routing/Introduction` のような URL パスを照合し、値 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }` を抽出します。 `controller` と `action` の既定のルート値は、テンプレートに対応するルート パラメーターがなくても、ルートにより生成されます。 既定値はルート テンプレートで指定できます。 `article` ルート パラメーターは、ルート パラメーター名の前にアスタリスク `*` があるとき、*キャッチオール*として定義されます。 キャッチオール ルート パラメーターは、URL パスの残りの部分をキャプチャします。空の文字列も照合できます。

ルート制約とデータ トークンを追加する例:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

このテンプレートは `/Products/5` のような URL パスを照合し、値 `{ controller = Products, action = Details, id = 5 }` とデータ トークン `{ locale = en-US }` を抽出します。

![ローカル変数ウィンドウ トークン](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>URL 生成

`Route` クラスは、一連のルート値をそのルート テンプレートと組み合わせる方法でも URL を生成できます。 論理的には、URL パスの照合とは逆のプロセスになります。

ヒント: URL 生成を理解する方法として、どのような URL を生成したいのか考えてください。そして、ルート テンプレートがその URL にどのように照合されるのか考えてください。 どのような値が生成されるでしょうか。 `Route` クラスにおける URL 生成のしくみと大体同じになります。

基本的な ASP.NET MVC スタイルのルートの使用例:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

ルート値が `{ controller = Products, action = List }` のとき、このルートは URL `/Products/List` を生成します。 ルート値が対応ルート パラメーターの代替となり、URL パスが作られます。 `id` は任意のルート パラメーターであるため、値がなくても問題ありません。

ルート値が `{ controller = Home, action = Index }` のとき、このルートは URL `/` を生成します。 与えられたルート値が既定値に一致することで、その値に対応するセグメントを省略しても問題が発生しません。 生成されたいずれの URL もこのルート定義でラウンドトリップし、URL の生成に利用された同じルート値を生成することに注意してください。

ヒント: アプリで ASP.NET MVC が使用される場合、ルーティングを直接呼び出す代わりに、`UrlHelper` を使用して URL を生成する必要があります。

URL 生成処理の詳細については、「[URL 生成参照](#url-generation-reference)」を参照してください。

## <a name="using-routing-middleware"></a>ルーティング ミドルウェアの使用

NuGet パッケージ "Microsoft.AspNetCore.Routing" を追加します。

*Startup.cs* でサービス コンテナーにルーティングを追加した例:

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

ルートは `Startup` クラスの `Configure` メソッドに設定する必要があります。 下のサンプルで使用されている API:

* `RouteBuilder`
* `Build`
* `MapGet` HTTP GET 要求のみを照合
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

下の表は、応答と所与の URI をまとめたものです。

| URI | 応答  |
| ------- | -------- |
| /package/create/3  | Hello! Route values: [operation, create], [id, 3] |
| /package/track/-3  | Hello! Route values: [operation, track], [id, -3] |
| /package/track/-3/ | Hello! Route values: [operation, track], [id, -3]  |
| /package/track/ | \<Fall through, no match> |
| GET /hello/Joe | Hi, Joe! |
| POST /hello/Joe | \<Fall through, matches HTTP GET only> |
| GET /hello/Joe/Smith | \<Fall through, no match> |

ルートを 1 つ構成する場合、`app.UseRouter` を呼び出し、`IRouter` インスタンスを渡します。 `RouteBuilder` を呼び出す必要はありません。

このフレームワークは、ルートを作成するために次のような拡張メソッドを提供します。

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

`MapGet` など、メソッドの一部は `RequestDelegate` を必要とします。 `RequestDelegate` は、ルートが一致したとき、*ルート ハンドラー*として使用されます。 この仲間の他のメソッドでは、ルート ハンドラーとして使用されるミドルウェア パイプラインを構成できます。 *Map* メソッドが `MapRoute` のようなハンドラーを受け取らない場合、`DefaultHandler` が使用されます。

`Map[Verb]` メソッドは制約を利用し、メソッド名の HTTP Verb にルートを制限します。 例が必要な場合、「[MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88)」や「[MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180)」をご覧ください。

## <a name="route-template-reference"></a>ルート テンプレート参照

中括弧 (`{ }`) 内のトークンは*ルート パラメーター*を定義します。ルートが一致した場合、これがバインドされます。 1 つのルート セグメントで複数のルート パラメーターを定義できますが、リテラル値で区切る必要があります。 たとえば、`{controller=Home}{action=Index}` は有効なルートになりません。`{controller}` と `{action}` の間にリテラル値がないためです。 このようなルート パラメーターには名前を付ける必要があります。付加的な属性を指定することもあります。

ルート パラメーター以外のリテラル テキスト (`{id}` など) とパス区切り `/` は URL のテキストに一致する必要があります。 テキスト照合は復号された URL パスを基盤とし、大文字と小文字が区別されます。 リテラル ルート パラメーターの区切り `{` または `}` を照合するには、その文字を繰り返すことでエスケープします (`{{` や `}}`)。

URL パターンで任意のファイル拡張子が付いたファイル名をキャプチャするとき、追加の考慮事項があります。 たとえば、テンプレート `files/{filename}.{ext?}` の使用です。`filename` と `ext` が両方存在するとき、両方の値が入力されます。 URL に `filename` だけが存在する場合、末尾のピリオド `.` は任意のため、このルートは一致となります。 次の URL はこのルートに一致します。

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

ルート パラメーターのプレフィックスとして `*` 文字を使用し、URI の残りの部分にバインドできます。これは*キャッチオール* パラメーターと呼ばれています。 たとえば、`blog/{*slug}` は、`/blog` で始まり、その後に (ルート値 `slug` に割り当てられる) 任意の値が続くあらゆる URI に一致します。 キャッチオール パラメーターは空の文字列に一致することもあります。

ルート パラメーターには、*既定値*が含まれることがあります。パラメーター名の後に既定値を指定し、`=` で区切ることで指名されます。 たとえば、`{controller=Home}` では、`controller` の既定値として `Home` が定義されます。 パラメーターの URL に値がない場合、既定値が使用されます。 既定値だけでなく、ルート パラメーターもオプションになることがあります (`id?` のように、パラメーター名の終わりに `?` を追加して指定します)。 オプションと "既定値がある" ことの違いは、既定値を含むルート パラメーターは常に値を生成するのに対し、オプションのパラメーターには値が指定されたときにのみ値が与えられることにあります。

ルート パラメーターには、URL からバインドされるルート値に一致しなければならないという制約も含まれることがあります。 コロン `:` と制約名をルート パラメーター名の後に追加すると、ルート パラメーターの*インライン制約*が指定されます。 その制約で引数が要求される場合、制約名の後に括弧 `( )` で指定されます。 別のコロン `:` と制約名を追加することで、複数のインライン制約を指定できます。 制約名が `IInlineConstraintResolver` サービスに渡され、URL 処理で使用する `IRouteConstraint` のインスタンスが作成されます。 たとえば、ルート テンプレート `blog/{article:minlength(10)}` によって、制約 `minlength` と引数 `10` が指定されます。 ルート制約の詳細とこのフレームワークで与えられる制約の一覧については、「[ルート制約参照](#route-constraint-reference)」を参照してください。

次の表は、一部のルート テンプレートとその動作をまとめたものです。

| ルート テンプレート | 一致する URL の例 | メモ |
| -------- | -------- | ------- |
| hello  | /hello  | パス `/hello` にのみ一致 |
| {Page=Home} | / | 一致し、`Page` が `Home` に設定される |
| {Page=Home}  | /Contact  | 一致し、`Page` が `Contact` に設定される |
| {controller}/{action}/{id?} | /Products/List | コントローラー `Products` とアクション `List` にマッピングされる |
| {controller}/{action}/{id?} | /Products/Details/123  |  コントローラー `Products` とアクション `Details` にマッピングされ、  `id` が 123 に設定される |
| {controller=Home}/{action=Index}/{id?} | /  |  コントローラー `Home` とメソッド `Index` にマッピングされ、`id` が無視される |

一般的に、テンプレートの利用が最も簡単なルーティングの手法となります。 ルート テンプレート以外では、制約と既定値も指定できます。

ヒント: [ログ](xref:fundamentals/logging/index) を有効にすると、`Route` など、組み込みのルーティング実装が要求を照合するしくみを確認できます。

## <a name="route-constraint-reference"></a>ルート制約参照

ルート制約は、`Route` が入ってきた URL の構文に一致し、URL パスをルート値にトークン化したときに実行されます。 ルート制約では、通常、ルート テンプレート経由で関連付けらるルート値を調べ、値が許容できるかどうかを単純な「はい/いいえ」で決定します。 一部のルート制約では、ルート値以外のデータを使用し、要求をルーティングできるかどうかが考慮されます。 たとえば、`HttpMethodRouteConstraint` はその HTTP Verb に基づいて要求を承認または却下します。

>[!WARNING]
> **入力検証**には制約を使用しないでください。無効な入力に起因し、400 と適切なエラー メッセージではなく、404 (Not Found) が表示されます。 ルート制約は、特定のルートに対する入力の妥当性を検証するためではなく、似たようなルートの**違いを明らかにする**ために使用してください。

次の表は、一部のルート制約とそれに求められる動作をまとめたものです。

| 制約 | 例 | 一致の例 | メモ |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | あらゆる整数に一致する |
| `bool`  | `{active:bool}` | `true`, `FALSE` | `true` または `false` に一致する (大文字と小文字が区別されます) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | 有効な `DateTime` 値に一致する (インバリアント カルチャで - 警告参照) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | 有効な `decimal` 値に一致する (インバリアント カルチャで - 警告参照) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | 有効な `double` 値に一致する (インバリアント カルチャで - 警告参照) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | 有効な `float` 値に一致する (インバリアント カルチャで - 警告参照) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | 有効な `Guid` 値に一致する |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | 有効な `long` 値に一致する |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | 4 文字以上の文字列であることが必要 |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | 8 文字以内の文字列であることが必要 |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | 厳密に 12 文字の文字列であることが必要 |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | 8 文字以上、16 文字以内の文字列であることが必要 |
| `min(value)` | `{age:min(18)}` | `19` | 18 以上の整数値であることが必要 |
| `max(value)` | `{age:max(120)}` |  `91` | 120 以下の整数値であることが必要 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | 18 以上、120 以下の整数値であることが必要 |
| `alpha` | `{name:alpha}` | `Rick` | 1 つまたは複数のアルファベット文字で構成される文字列が必要 (`a`-`z`、大文字と小文字を区別) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | 正規表現に一致する文字列が必要 (正規表現の定義についてはヒントを参照) |
| `required`  | `{name:required}` | `Rick` |  URL 生成中、非パラメーターが提示されるように強制する |

>[!WARNING]
> URL の妥当性を検証するルート制約は、CLR 型 (`int` や `DateTime` など) に変換できます。常にインバリアント カルチャを使用してください。URL がローカライズ不可であると見なされます。 フレームワークから提供されるルート制約がルート値に格納されている値を変更することはありません。 URL から解析されたルート値はすべて文字列として格納されます。 たとえば、[浮動小数ルート制約](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60)はルート値を浮動小数に変換しますが、変換された値は、浮動小数に変換できることを検証するためにだけ利用されます。

## <a name="regular-expressions"></a>正規表現 

ASP.NET Core フレームワークでは、正規表現コンストラクターに `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` が追加されます。 これらのメンバーの説明については、「[RegexOptions Enumeration](https://docs.microsoft.com/dotnet/api/system.text.regularexpressions.regexoptions)」(RegexOptions 列挙) を参照してください。

正規表現では、ルーティングや C# 言語で使用されるものに似た区切り記号とトークンが使用されます。 正規表現トークンはエスケープする必要があります。 たとえば、ルーティングで正規表現 `^\d{3}-\d{2}-\d{4}$` を使用するには、C# ソース ファイルで `\` 文字を `\\` のように入力し、文字列エスケープ文字である `\` をエスケープする必要があります ([逐語的な文字列リテラル](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string)を使用しない限り)。 `{`、`}`、'['、']' 文字は、二回入力することでエスケープし、ルーティング パラメーター区切り文字をエスケープする必要があります。  次の表は、正規表現とエスケープ適用後の表示をまとめたものです。

| 正規表現               | メモ |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | 正規表現 |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | エスケープ適用後  |
| `^[a-z]{2}$` | 正規表現 |
| `^[[a-z]]{{2}}$` | エスケープ適用後  |

ルーティングで使用される正規表現は、多くの場合、`^` 文字で始まり (文字列の開始位置に一致)、`$` 文字で終わります (文字列の終了文字に一致)。 `^` 文字と `$` 文字により、正規表現はルート パラメーター値全体に一致します。 `^` 文字と `$` 文字がなければ、正規表現は意図に反して文字列内のあらゆるサブ文字列に一致します。 下の表では例をいくつか挙げ、それが一致する/一致しない理由をまとめています。

| 正規表現               | String | 一致したもの | コメント |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | hello | 可 | サブ文字列の一致 |
| `[a-z]{2}` | 123abc456 | 可 | サブ文字列の一致 |
| `[a-z]{2}` | mz | 可 | 一致する表現 |
| `[a-z]{2}` | MZ | 可 | 大文字と小文字の使い方が違う |
| `^[a-z]{2}$` |  hello | Ｘ | 上の `^` と `$` を参照 |
| `^[a-z]{2}$` |  123abc456 | Ｘ | 上の `^` と `$` を参照 |

正規表現構文の詳細については、「[.NET Framework 正規表現](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)」を参照してください。

既知の入力可能値の集まりにパラメーターを制限するには、正規表現を使用します。 たとえば、`{action:regex(^(list|get|create)$)}` の場合、`action` ルート値は `list`、`get`、`create` とのみ照合されます。 制約ディクショナリに渡された場合、文字列 "^(list|get|create)$" で同じものになります。 (テンプレート内でインラインではなく) 制約ディクショナリに渡された制約が既知の制約に一致しない場合も、正規表現として扱われます。

## <a name="url-generation-reference"></a>URL 生成参照

下の例では、ルートのリンクを生成する方法を確認できます。ルート値のディクショナリと `RouteCollection` が指定されています。

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

上のサンプルの終わりで生成された `VirtualPath` は `/package/create/123` です。

`VirtualPathContext` コンストラクターの 2 番目のパラメーターは*アンビエント値*の集合です。 アンビエント値は、開発者が特定の文脈で要求における値を指定しなければならないよう、値の数を制限するので便利です。 現在の要求の現在のルート値は、リンク生成の場合、アンビエント値として見なされます。 たとえば、ASP.NET MVC アプリで `HomeController` の `About` アクション中、コントローラー ルート値を指定し、`Index` アクションにリンクする必要はありません (`Home` のアンビエント値が使用されます)。

パラメーターに一致しないアンビエント値は無視されます。明示的に指定された値によりオーバーライドされるときもアンビエント値は無視されます (URL の左から右へ)。

明示的に指定されているが何も一致しない値はクエリ文字列に追加されます。 次の表は、ルート テンプレート `{controller}/{action}/{id?}` の使用時の結果をまとめたものです。

| アンビエント値 | 明示的な値 | 結果 |
| -------------   | -------------- | ------ |
| controller="Home" | action="About" | `/Home/About` |
| controller="Home" | controller="Order",action="About" | `/Order/About` |
| controller="Home",color="Red" | action="About" | `/Home/About` |
| controller="Home" | action="About",color="Red" | `/Home/About?color=Red`

パラメーターに対応しない既定値がルートにあり、その値が明示的に指定される場合、それは既定値に一致する必要があります。 例:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

リンク生成では、コントローラーとアクションについて一致する値が指定されるとき、このルートのリンクのみが生成されます。
