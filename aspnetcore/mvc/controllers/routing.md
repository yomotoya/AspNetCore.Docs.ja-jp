---
title: "コント ローラー アクションへのルーティング"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: d6e230351eb2f4c8549b54d75fd8e345718e6109
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2017
---
# <a name="routing-to-controller-actions"></a>コント ローラー アクションへのルーティング

によって[Ryan Nowak](https://github.com/rynowak)と[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC ルーティングを使用して[ミドルウェア](../../fundamentals/middleware.md)に受信した要求の Url と一致し、アクションにマップします。 ルートは、スタートアップ コードまたは属性で定義されます。 ルートは、アクションへの URL パスの照合方法について説明します。 ルートは、応答で送信 (リンク用) の Url を生成するも使われます。 

アクションは従来どおりルーティングするか、または属性にルーティングします。 コント ローラーまたはアクションのルートを配置すると、その属性がルーティングされます。 参照してください[ルーティングの混在](#routing-mixed-ref-label)詳細についてはします。

このドキュメントでは、MVC とルーティング、およびどの標準的な MVC アプリの作成の間のやり取りを説明します。 ルーティング機能を使用します。 参照してください[ルーティング](xref:fundamentals/routing)高度なルーティングの詳細。

## <a name="setting-up-routing-middleware"></a>ミドルウェアのルーティングを設定します。

*構成*方法のようなコードを確認する可能性があります。

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

呼び出しの内部`UseMvc`、`MapRoute`と呼びますが単一のルートの作成に使用される、`default`ルート。 ほとんどのアプリで MVC はのようなテンプレートでルートを使用、`default`ルート。

ルート テンプレート`"{controller=Home}/{action=Index}/{id?}"`のような URL パスを照合できる`/Products/Details/5`およびルート値を抽出`{ controller = Products, action = Details, id = 5 }`でパスをトークン化します。 MVC はコント ローラーがという名前を検索を試みます`ProductsController`アクションを実行および`Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

この例ではモデル バインディングが使用するようの値`id = 5`を設定する、`id`パラメーターを`5`この操作を呼び出すときにします。 参照してください、[モデル バインド](../models/model-binding.md)詳細についてはします。

使用して、`default`ルート。

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

ルート テンプレート。

* `{controller=Home}`定義`Home`既定値として`controller`

* `{action=Index}`定義`Index`既定値として`action`

* `{id?}`定義`id`オプションとして

既定値と省略可能なルートのパラメーターは、一致の URL パスに存在する必要はありません。 参照してください[ルート テンプレート参照](../../fundamentals/routing.md#route-template-reference)ルート テンプレートの構文の詳細な説明をします。

`"{controller=Home}/{action=Index}/{id?}"`URL パスを照合できる`/`ルートの値を生成および`{ controller = Home, action = Index }`です。 値は、`controller`と`action`ように既定値の使用`id`URL パスに対応するセグメントが存在しないために、値を生成しません。 MVC ではこれらのルート値を使用して、`HomeController`と`Index`アクション。

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

このコント ローラーの定義とルート テンプレートを使用して、`HomeController.Index`次の URL パスのいずれかのアクションが実行されます。

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

便利なメソッド`UseMvcWithDefaultRoute`:

```csharp
app.UseMvcWithDefaultRoute();
```

置換に使用できます。

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc`および`UseMvcWithDefaultRoute`のインスタンスに追加`RouterMiddleware`ミドルウェア パイプラインにします。 MVC は、ミドルウェアと直接やり取りしませんし、要求を処理するルーティングを使用します。 MVC がのインスタンスを使用して、ルートに接続されている`MvcRouteHandler`です。 内のコード`UseMvc`は、次に似ています。

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc`すべてのルートを直接定義されていません、プレース ホルダーのルートのコレクションに追加、`attribute`ルート。 オーバー ロードは、`UseMvc(Action<IRouteBuilder>)`を使用して、独自のルートを追加し、属性のルーティングをサポートします。  `UseMvc`および属性ルートのプレース ホルダーが追加のすべてのバリエーションの - 属性のルーティングの構成方法に関係なく使用可能なは常に`UseMvc`です。 `UseMvcWithDefaultRoute`既定のルートを定義し、属性のルーティングをサポートします。 [属性がルーティング](#attribute-routing-ref-label)セクションでは、属性のルーティングの詳細についてを説明します。

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>従来のルーティング

`default`ルート。

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

例を示します、*従来のルーティング*です。 このスタイルを呼び出して*従来のルーティング*を作成できるので、*規約*の URL パスの場合。

* 最初のパス セグメントは、コント ローラー名にマップします。

* 2 つ目は、アクション名にマップされます。

* 3 番目のセグメントは省略可能なための`id`モデルのエンティティにマップします。

これを使用して`default`ルート、URL path`/Products/List`にマップ、`ProductsController.List`アクション、および`/Blog/Article/17`にマップ`BlogController.Article`です。 このマッピングは、コント ローラーとアクションの名前に基づいて**のみ**名前空間、ソース ファイルの場所、またはメソッド パラメーターには基づいていません。

> [!TIP]
> 既定のルートで従来のルーティングを使用するを定義する各アクションの新しい URL パターンを考えつくしなくても、アプリケーションをすばやく構築することができます。 CRUD スタイルのアクションがあるアプリケーション、Url のコント ローラー間で一貫性を持つことができますをコードを容易にし、UI をより予測可能なためです。

> [!WARNING]
> `id`は省略可能なによって定義されたルート テンプレート、URL の一部として提供された ID を持たないユーザーの操作を実行できることを意味します。 通常何が起こる場合`id`URL が省略されているに設定することは、`0`結果として、データベースの照合にエンティティがないと、モデル バインディングによって`id == 0`です。 属性のルーティングすれば、および他のいくつかのアクションに必要な ID の詳細に制御します。 省略可能なパラメーターなどのドキュメントは、慣例`id`正しい使用法に表示される可能性がある時期。

## <a name="multiple-routes"></a>複数のルート

内の複数のルートを追加する`UseMvc`に呼び出しを追加して`MapRoute`です。 これにより、複数の規則を定義したりするなど、特定の操作専用従来のルートを追加します。

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`blog`ルートここでは、*専用従来ルート*、従来のルーティング システムを使用しますが、特定の操作は専用のことを意味します。 `controller`と`action`パラメーターとしてルート テンプレートに表示されていない、既定値を持てるおよびためこのルートは、アクションにマップする常に`BlogController.Article`です。

ルート コレクション内のルートは、並べられ、追加された順序で処理されます。 この例では、`blog`ルートは、前に実行されます、`default`ルート。

> [!NOTE]
> *従来のルートを専用*多くの場合と同様にキャッチ オール ルート パラメーターを使用して`{*article}`URL パスの残りの部分をキャプチャします。 これにより、ルート 'すぎる最長一致' 他のルートに一致させるための Url と一致することを意味します。 後で、ルート テーブルのこの問題を解決するには、'最長一致' のルートを配置します。

### <a name="fallback"></a>フォールバック

要求の処理の一環として、MVC は、アプリケーション内でコント ローラーとアクションを検索するルートの値を使用できることを確認します。 アクションをルートの値が一致しない場合、ルートとは見なされません、一致して次のルートが試行されます。 これと呼ばれる*フォールバック*を従来のルートと重なる場合を簡略化するためのものでは、します。

### <a name="disambiguating-actions"></a>アクションの明確化

2 つのアクションは、ルーティングで一致すると、MVC 必要がありますあいまいさを解消、例外をスローするか、または '最高' 候補を選択します。 例:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

このコント ローラーには、URL パスに一致する、2 つのアクションが定義されて`/Products/Edit/17`し、データをルーティング`{ controller = Products, action = Edit, id = 17 }`です。 これは、MVC コント ローラーの一般的なパターン場所`Edit(int)`、製品を編集するためのフォームを表示および`Edit(int, Product)`ポストされたフォームを処理します。 可能な MVC 必要がありますを選択する`Edit(int, Product)`要求が HTTP の場合`POST`と`Edit(int)`HTTP 動詞がそれ以外の場合。

`HttpPostAttribute` ( `[HttpPost]` ) の実装は、 `IActionConstraint` HTTP 動詞が場合に選択する操作のみを許可する`POST`です。 存在、`IActionConstraint`により、 `Edit(int, Product)` 'より良い' の一致よりも`Edit(int)`ため、`Edit(int, Product)`最初に試行されます。

カスタムを作成する必要がありますのみ`IActionConstraint`が特殊なシナリオでは、実装のような属性の役割を理解しておく必要`HttpPostAttribute`-その他の HTTP 動詞のような属性が定義されます。 従来のルーティングでは操作の一部であるときに、同じアクション名を使用する一般的な`show form -> submit form`ワークフローです。 このパターンの利便性を確認した後より明白になります、[理解 IActionConstraint](#understanding-iactionconstraint)セクションです。

複数のルートが一致するし、MVC は、'最高' のルートを見つけることはできませんがスローされます、`AmbiguousActionException`です。

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>ルート名

文字列`"blog"`と`"default"`次の例では、ルート名。


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

ルート名は、名前付きのルートが URL の生成に使用できるようにするに、ルートの論理名を付けます。 URL の作成は、ルートの順序を行うことができる複雑になり、URL の生成時にこれが大幅に合理化されます。 ルート名は一意のアプリケーション全体である必要があります。

ルート名なしに影響がある URL に一致するか、要求の処理URL の生成にのみ使用されます。 [ルーティング](xref:fundamentals/routing)詳細は特定の MVC ヘルパーの URL の生成を含む URL の生成にします。

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>属性のルーティング

属性のルーティング アクションをルート テンプレートに直接マップするのに一連の属性を使用します。 次の例では、`app.UseMvc();`で使用される、`Configure`メソッドとルートがありませんが渡されます。 `HomeController`はどのような既定のルートのような Url のセットに一致`{controller=Home}/{action=Index}/{id?}`が一致します。

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

`HomeController.Index()` URL パスのいずれかで実行するアクション`/`、 `/Home`、または`/Home/Index`です。

> [!NOTE]
> この例では、属性のルーティングと従来のルーティングとプログラミングの大きな違いが強調表示されます。 さらに、ルートを指定の入力属性のルーティングが必要です。従来の既定のルートでは、簡潔にルートを処理します。 ただし、属性のルーティングにより、(およびが必要です) 各アクションに適用するルート テンプレートを正確に制御します。

コント ローラー名とアクション名をルーティングする属性を持つ再生**ありません**アクションが選択されているロールです。 この例では、前の例と同じ Url を検索します。

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> 上記のルート テンプレートのルートのパラメーターを定義しない`action`、 `area`、および`controller`です。 実際には、これらのルートのパラメーターは、属性のルートでは許可されません。 ルート テンプレートは、アクションに関連付けは既に、ために考えないから賢明を URL からアクション名を解析します。

## <a name="attribute-routing-with-httpverb-attributes"></a>Http [動詞] 属性を持つルーティング属性します。

属性がルーティングすることもできますの使用、`Http[Verb]`など属性`HttpPostAttribute`です。 ルート テンプレートを受け入れるすべてこれらの属性のことができます。 この例では、同じルート テンプレートに一致する 2 つのアクションを示します。

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

ような URL パスの`/products`、 `ProductsApi.ListProducts` HTTP 動詞がときにアクションが実行されます`GET`と`ProductsApi.CreateProduct`HTTP 動詞がときに実行される`POST`です。 ルート属性で定義されたルート テンプレートのセットに対して URL に一致属性が最初にルーティングします。 ルート テンプレートに一致すると、一度`IActionConstraint`のどのアクションを実行するかの制約が適用されます。

> [!TIP]
> まれを使用する REST API を作成するときに`[Route(...)]`がアクション メソッド。 詳細なを使用することをお勧め`Http*Verb*Attributes`API がサポートする内容を正確に指定します。 どのようなパスと HTTP 動詞にマップする特定の論理操作を把握するには、REST Api のクライアントが予想されます。

属性ルートは、特定のアクションに適用するため、簡単にルート テンプレートの定義の一部として必要なパラメーターです。 この例では`id`が URL パスの一部として必要です。

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)`のような URL パスで実行するアクション`/products/3`のではなくのような URL パス`/products`です。 参照してください[ルーティング](../../fundamentals/routing.md)ルート テンプレートと関連するオプションの詳細についてはします。

## <a name="route-name"></a>ルート名

次のコード定義、*ルート名*の`Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

ルート名は、特定のルートに基づく URL の生成に使用できます。 ルート名は、ルーティングの動作と一致する URL に影響を与えるなく、URL の生成にのみ使用します。 ルート名は一意のアプリケーション全体である必要があります。

> [!NOTE]
> これは、従来*既定のルート*を定義する、`id`パラメーターをオプションとして (`{id?}`)。 Api を正確に指定するには、この機能を許可するなどの利点を持つ`/products`と`/products/5`さまざまな操作にディスパッチされます。

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>結合ルート

属性がルーティング小さい反復的なコント ローラーのルート属性は、個々 のアクションでのルート属性と結合されます。 コント ローラーで定義されたルート テンプレートの前は、アクションのルート テンプレートに付加されます。 コント ローラーのルート属性を配置すると、**すべて**コント ローラーのアクションは、属性のルーティングを使用します。

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

この例の URL パスでは`/products`に適合する`ProductsApi.ListProducts`、URL パス`/products/5`に適合する`ProductsApi.GetProduct(int)`です。 これらの操作にのみ一致する HTTP`GET`で装飾されているため、`HttpGetAttribute`です。

ルート テンプレートを起点とするアクションに適用されて、`/`は取得と組み合わせないでコント ローラーに適用されたルート テンプレート。 この例と同様の URL パスのセットに一致、*既定のルート*です。

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a>属性ルートの順序

対照的に定義された順序で実行される従来のルートでは、属性のルーティングは、ツリーを構築し、同時にすべてのルートに一致します。 これとして動作のルート エントリが、最適な順序付け; に配置されていた場合最も固有のルートには、一般的なルートの前に実行する機会があります。

たとえば、ルートなど`blog/search/{topic}`のようにルートよりも特定`blog/{*article}`です。 論理的には、`blog/search/{topic}`ルート '実行' 最初に、既定では、実用的な唯一の順序付けされているためです。 従来のルーティングを使用して、開発者が目的の順序にルートを配置するためです。

属性ルートは、注文を構成することができますを使用して、`Order`フレームワークが提供するルート属性のすべてのプロパティです。 ルートは昇順に従って処理のソート、`Order`プロパティです。 既定の順序は`0`します。 設定を使用して、ルート`Order = -1`順序が設定されていないルートの前に実行されます。 設定を使用して、ルート`Order = 1`既定ルートの順序付けした後に実行されます。

> [!TIP]
> に応じて回避`Order`です。 URL 空間には、明示的な順序の値を正しくルーティングが必要とする場合は、クライアントとの混乱を招く可能性があります。 一般に属性のルーティングが URL に一致する正しいルートが選択されます。 上書きは、適用するよりも通常簡単にルート名を使用して、URL の生成に使用される既定の順序が動作していない場合、`Order`プロパティです。

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>トークンの置換ルート テンプレートで ([コント ローラー]、[アクション]、[領域])

属性ルートをサポートして便宜上、*トークンの置換*角かっこで囲み、トークンを (`[`、 `]`)。 トークン`[action]`、 `[area]`、および`[controller]`はアクション名、領域名とルートが定義されているアクションからコント ローラー名の値に置き換えられます。 この例ではコメント」の説明に従って、アクションによって URL パスが一致することができます。

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

トークンの置換は、属性のルートの構築の最後のステップとして発生します。 上記の例は、次のコードと同じ動作にします。

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

継承を持つ属性ルートを組み合わせることもできます。 これは、トークンの置換と組み合わせる非常に強力です。

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

トークンの置換は、属性のルートが定義したルート名にも適用されます。 `[Route("[controller]/[action]", Name="[controller]_[action]")]`アクションごとに一意のルート名が生成されます。

トークンの置換がリテラルの区切り記号に一致する`[`または`]`、文字の繰り返しでエスケープ (`[[`または`]]`)。

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>複数のルート

属性の操作と同じに到達する複数のルートを定義するルーティングをサポートします。 動作を模倣するためには、これの最も一般的な使用法、*従来型の既定のルート*次の例で示すようにします。

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

コント ローラーに複数のルート属性を適用するには、それぞれ 1 つがアクション メソッドのルート属性の各に結合を意味します。

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

ときに複数のルート属性 (実装する`IActionConstraint`) は、各アクションの制約がルート テンプレートを定義する属性を使用して結合し、アクションに配置されます。

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> 操作で複数のルートを使用してできるように見えます強力、シンプルかつ適切に定義されたアプリケーションの URL のスペースを保持することをお勧めします。 既存のクライアントをサポートする例については、必要な場所にのみ、アクションで複数のルートを使用します。

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>属性のルートの省略可能なパラメーター、既定値、および制約を指定します。

属性ルートは、省略可能なパラメーター、既定値、および制約を指定する従来のルートとして同じインライン構文をサポートします。

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

参照してください[ルート テンプレート参照](../../fundamentals/routing.md#route-template-reference)ルート テンプレートの構文の詳細な説明をします。

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>使用してカスタム ルート属性`IRouteTemplateProvider`

すべてのフレームワークで提供されるルート属性 ( `[Route(...)]`、`[HttpGet(...)]`など) を実装、`IRouteTemplateProvider`インターフェイスです。 MVC は、アプリが起動し、実装するものを使用するとコント ローラー クラスとアクション メソッドの属性の検索`IRouteTemplateProvider`ルートの初期セットを構築します。

実装できます`IRouteTemplateProvider`を独自のルート属性を定義します。 各`IRouteTemplateProvider`順序や名前をカスタム ルート テンプレートでルートが 1 つを定義することができます。

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

上記の例からの属性が自動的に設定、`Template`に`"api/[controller]"`とき`[MyApiController]`を適用します。

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>アプリケーション モデルを使用して属性ルートをカスタマイズするには

*アプリケーション モデル*ですべての MVC のルーティングし、操作を実行するために使用するメタデータの起動時に作成されたオブジェクト モデルです。 *アプリケーション モデル*のすべてのルート属性から収集されたデータが含まれています (を通じて`IRouteTemplateProvider`)。 記述することができます*規則*ルーティングの動作方法をカスタマイズするスタートアップ時に、アプリケーション モデルを変更します。 このセクションでは、アプリケーション モデルを使用してルーティングをカスタマイズする方法の簡単な例を示します。

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>ルーティングの混在: 属性のルーティングと従来のルーティング

MVC アプリケーションでは、従来のルーティングやルーティングの属性の使用を混在させることができます。 通常、ブラウザーの場合、HTML ページを提供しているコント ローラーに対して従来のルートを使用し、REST Api を提供しているコント ローラーのルーティングを属性であります。

アクションは従来どおりルーティングするか、または属性にルーティングします。 コント ローラーまたはアクションのルートを配置すると、その属性がルーティングされます。 属性ルートを定義する操作は、従来のルートと逆を介して到達できません。 **どの**コント ローラーのルート属性がルーティング コント ローラー属性のすべてのアクションをによりします。

> [!NOTE]
> ルーティングのシステムの 2 種類のような特徴は、URL に一致するルート テンプレート後に適用するプロセスです。 従来のルーティングでは、一致したルートの値は、すべてのルーティング アクションを従来のルックアップ テーブルからアクションとコント ローラーを選択に使用されます。 属性、ルーティングの各テンプレートの操作に関連付けられてし、これ以上の参照は必要ありません。

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>URL の生成

MVC アプリケーションでは、ルーティングの URL の生成機能を使用して、アクションへの URL リンクを生成します。 Url の生成、ハードコーディングする Url、堅牢性と保守が容易なので、コードがなくなります。 このセクションでは、MVC によって提供される URL の生成機能について説明し、URL の生成のしくみに関する基本事項を取り上げますのみです。 参照してください[ルーティング](../../fundamentals/routing.md)URL 生成の詳細な説明をします。

`IUrlHelper`インターフェイスは、基になるインフラストラクチャの部分 MVC ルーティングの URL の生成とします。 インスタンスを検出します`IUrlHelper`を介して使用できる、`Url`プロパティ コント ローラー、ビュー、およびコンポーネントの表示にします。

この例では、`IUrlHelper`を介してインターフェイスを使用、`Controller.Url`プロパティを別のアクションへの URL を生成します。

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

アプリケーションで従来の既定値が使用されている場合、ルーティングの値、`url`変数は、URL パスの文字列になります`/UrlGeneration/Destination`です。 渡された値 (アンビエント値) の現在の要求からのルート値を組み合わせることによってルーティングが URL パスが作成された`Url.Action`とルート テンプレートにこれらの値を代入します。

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

ルート テンプレート内の各ルート パラメーター値とアンビエント値と一致する名前に置換値があります。 ルート パラメーター値がないは、既定値を使用できますが、いずれかまたは省略可能である場合は省略される場合 (の場合と`id`この例では)。 任意の必要なルートのパラメーターは、対応する値を持っていない場合、URL の生成は失敗します。 ルートの URL の生成に失敗した場合は、すべてのルートが試行されたか、一致が見つかるまでに次のルートが試されます。

この例の`Url.Action`上では従来のルーティングが URL 生成の動作属性がルーティングと同じように概念が異なる場合。 従来のルーティングでルートの値を使用して、展開、テンプレートと、ルートの値を`controller`と`action`通常はルーティングに一致する Url に従うためこの方法でそのテンプレートに表示、*規約*. 属性、ルーティングのルートの値を`controller`と`action`テンプレート - に表示に使用するテンプレートを検索する代わりに使用することはできません。

この例では、属性のルーティングを使用します。

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC ルーティング属性のすべてのアクションのルックアップ テーブルを構築およびと一致する、`controller`と`action`URL の生成に使用するルート テンプレートを選択する値。 上記のサンプルで`custom/url/to/destination`が生成されます。

### <a name="generating-urls-by-action-name"></a>アクション名によって Url の生成

`Url.Action` (`IUrlHelper` . `Action`) 関連するすべてのオーバー ロードがすべて新機能にリンクするコント ローラー名とアクション名を指定して、指定することを考えに基づいているとします。

> [!NOTE]
> 使用する場合`Url.Action`、現在のルートの値を`controller`と`action`- の値を指定した`controller`と`action`両方の一部である*アンビエント値***と***値*です。 メソッド`Url.Action`の現在の値は常に使用`action`と`controller`と現在のアクションにルーティングする URL パスが生成されます。

アンビエントの値で提供されていません。 URL を生成するときに情報を入力する値を使用する試行をルーティングします。 ようにルートを使用して`{a}/{b}/{c}/{d}`とアンビエント値`{ a = Alice, b = Bob, c = Carol, d = David }`ルーティングせず、追加の値の URL を生成するための十分な情報には、-、パラメーターが値を持つため、すべてをルーティングします。 値を追加した場合は`{ d = Donovan }`、値`{ d = David }`は無視されます、し、生成された URL パスが`Alice/Bob/Carol/Donovan`です。

> [!WARNING]
> URL パスは、階層。 値を追加した場合に上記の例で`{ c = Cheryl }`、両方の値`{ c = Carol, d = David }`は無視されます。 ここでの値が不要になったある`d`URL の生成は失敗します。 目的の値を指定する必要があります`c`と`d`です。  既定のルートでこの問題をヒットする予定である (`{controller}/{action}/{id?}`)-として実際には、この動作が発生することはほとんどありませんが、`Url.Action`常に明示的に指定する、`controller`と`action`値。

オーバー ロードをより長い`Url.Action`取得することも、追加*ルート値*以外の場合、ルート パラメーターの値を提供するオブジェクト`controller`と`action`です。 最もよく表示されると共に使用`id`など`Url.Action("Buy", "Products", new { id = 17 })`です。 慣例により、*ルート値*オブジェクトは、通常、匿名型のオブジェクトがある可能性も、`IDictionary<>`または*plain old .NET オブジェクト*です。 ルート パラメーターが一致しないすべての追加のルートの値は、クエリ文字列に格納されます。

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> 絶対 URL を作成するを受け入れるオーバー ロードを使用して、 `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>ルートで Url の生成

上記のコードでは、URL の生成、コント ローラーとアクション名を渡すことによって示されています。 `IUrlHelper`用意されています、`Url.RouteUrl`メソッドのファミリです。 これらのメソッドはのような`Url.Action`はの現在の値をコピーしないで`action`と`controller`ルートの値にします。 最も一般的な使用状況は一般的に、URL を生成する、特定のルートを使用するルート名を指定する*せず*コント ローラーまたはアクション名を指定します。

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>Html 形式で Url の生成

`IHtmlHelper`提供、`HtmlHelper`メソッド`Html.BeginForm`と`Html.ActionLink`を生成する`<form>`と`<a>`要素それぞれします。 これらのメソッドを使用して、 `Url.Action` URL を生成する方法のような引数を受け入れるとします。 `Url.RouteUrl`の仲間`HtmlHelper`は`Html.BeginRouteForm`と`Html.RouteLink`同様の機能であります。

TagHelpers で Url を生成する、 `form` TagHelper と`<a>`TagHelper です。 これらの両方を使用して`IUrlHelper`実装するためです。 参照してください[フォームを使用する](../views/working-with-forms.md)詳細についてはします。

ビュー、内部、`IUrlHelper`を介して使用できますが、`Url`上記でカバーされないアドホック URL の生成に任意のプロパティです。

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>アクションの結果で URL を生成します。

上記の例を使用して表示が`IUrlHelper`アクションの結果の一部として URL を生成するコント ローラーの最も一般的な使用方法ですが、コント ローラーにします。

`ControllerBase`と`Controller`の基本クラスが別のアクションを参照するアクション結果の便利なメソッドを提供します。 一般的な使用法の 1 つでは、ユーザー入力を受け入れた後リダイレクトです。

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

メソッドに類似のパターンに従うアクション結果のファクトリ メソッド`IUrlHelper`です。

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>従来の専用のルートの特殊なケース

ルート定義と呼ばれる特殊なを使用できる従来のルーティング、*専用従来ルート*です。 次の例ではルートの名前`blog`専用従来ルートです。

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

これらのルート定義を使用して`Url.Action("Index", "Home")`URL パスが生成されます`/`で、`default`ルートでなく、なぜですか? ルートの値を推測することがあります`{ controller = Home, action = Index }`使用する場合、URL の生成に十分な`blog`、ありの結果となる`/blog?action=Index&controller=Home`です。

従来の専用のルーティングがルートがなることを防止する対応するルートのパラメーターがありません。 既定値の特別な動作に依存"すぎる最長"の URL を生成します。 既定値は、ここでは`{ controller = Blog, action = Article }`、およびどちら`controller`も`action`ルートのパラメーターとして表示されます。 ルーティング URL の生成を実行するときに指定された値は既定値に一致する必要があります。 URL の生成を使用して`blog`ために失敗値`{ controller = Home, action = Index }`が一致しない`{ controller = Blog, action = Article }`です。 ルーティングし、フォールバックして再試行してください`default`、これは成功します。

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>区分

[領域](areas.md)MVC 機能として、別のルーティングでの名前空間 (コント ローラーのアクション) と (views) 用のフォルダー構造をグループに関連する機能を整理するために使用します。 領域の使用を使用すると同じ名前の複数のコント ローラーを持つ異なるを持っていれば、*領域*です。 別のルート パラメーターを追加することでルーティングするために階層を作成する領域を使用して`area`に`controller`と`action`です。 このセクションでは、領域 - と対話するルーティング方法について説明を参照してください[領域](areas.md)詳細ビューでの領域の使用方法についてはします。

次の例は、従来の既定のルートを使用する MVC を構成し、*領域ルート*という名前の領域の`Blog`:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

ような URL パスを照合するときに`/Manage/Users/AddUser`、最初のルートは、ルート値を生成`{ area = Blog, controller = Users, action = AddUser }`です。 `area`ルートの値がの既定値によって生成される`area`実際には、によって作成されたルート`MapAreaRoute`は、次に相当します。

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute`既定値との制約の両方を使用してルートを作成`area`ここで指定された領域名を使用して`Blog`です。 既定値は、ルートが常に生成することにより、 `{ area = Blog, ... }`、制約が値を必要`{ area = Blog, ... }`URL 生成のためです。

> [!TIP]
> 従来のルーティングは、順序によって異なります。 一般に、領域なしルートより限定的であるので、ルート テーブルの前半の領域を持つルートを配置してください。

上記の例を使用して、ルートの値は、次の操作を一致は。

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute`コント ローラーを表すものは、領域の一部として、ということにこのコント ローラーは、`Blog`領域。 せずコント ローラー、`[Area]`属性が任意の領域のメンバーではないと、**いない**一致する場合に、`area`ルーティングでルートの値が指定されてです。 次の例では、表示されている最初のコント ローラーのみがルートの値を照合できる`{ area = Blog, controller = Users, action = AddUser }`です。

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> 各コント ローラーの名前空間が完全を期すのためここで示すように、それ以外の場合、コント ローラーが競合して、コンパイラ エラーが生成される名前付けされます。 クラスの名前空間は、MVC のルーティングに影響を与えるありません。

最初の 2 つのコント ローラーの領域のメンバーであるし、して、それぞれの領域名を指定した場合にのみ一致する、`area`値をルーティングします。 3 番目のコント ローラーは、任意の領域、およびときの値がありませんが唯一の一致のメンバーではない`area`ルーティングによって提供されます。

> [!NOTE]
> 照合の観点から*値はありません*がない場合、`area`値は、同じ場合は、値は、`area`が null または空の文字列。

場合、ルートの値の領域内で操作を実行するときに`area`として使用できる、*アンビエント値*の URL の生成に使用するルーティングします。 つまり、既定では領域機能する*付箋*URL 生成の次の例に示すようにします。

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Understanding IActionConstraint

> [!NOTE]
> このセクションでは、フレームワークの内部構造と MVC を実行するアクションを選択する方法に deep dive です。 一般的なアプリケーションは、カスタムを必要はありません。`IActionConstraint`

既に使用した可能性があります`IActionConstraint`インターフェイスとあまり詳しくない場合でもです。 `[HttpGet]`属性および同様`[Http-VERB]`属性は、実装`IActionConstraint`アクション メソッドの実行を制限するためにします。

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

既定の従来ルートの URL パスを想定して`/Products/Edit`と値が生成される`{ controller = Products, action = Edit }`、一致するよう**両方**次に示す操作のです。 `IActionConstraint`と言うの候補を考慮はこれらのアクションの両方のルート データを一致している両方の用語です。

ときに、`HttpGetAttribute`を実行することだとは*Edit()*の一致するものは、*取得*し、その他の HTTP 動詞の一致するものではありません。 `Edit(...)`アクションが定義されている、すべての制約がないし、すべての HTTP 動詞と一致するためです。 これと仮定した場合、 `POST` - のみ`Edit(...)`と一致します。 `GET`両方のアクションできますも一致 - ただし、アクションと、`IActionConstraint`は常と見なされます*向上*なしアクションよりもします。 そのため`Edit()`が`[HttpGet]`より具体的と見なされ、両方のアクションを一致させる場合に選択されます。

概念的には、`IActionConstraint`の形式は、*オーバー ロード*、同じ名前のメソッドをオーバー ロードではなく、同じ URL に一致するアクション間でオーバー ロードには、します。 属性のルーティングを使用しても`IActionConstraint`両方を検討している候補となるさまざまなコント ローラーから、操作に実現できます。

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>IActionConstraint を実装します。

実装する最も簡単な方法、`IActionConstraint`から派生するクラスを作成するのには、`System.Attribute`アクションとコント ローラー上に配置します。 MVC は自動的に検出、`IActionConstraint`属性として適用されます。 アプリケーション モデルを使用するには、制約を適用してがある可能性が最も柔軟な方法でメタプログラムの適用方法を可能にします。

次の例では、制約がに基づいてアクションを選択、*国コード*ルート データから。 [GitHub の全サンプル](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs)です。

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

実装を担当する場合、`Accept`メソッドと、'Order' 制約の実行を選択します。 ここでは、`Accept`メソッドを返します。`true`アクションが一致するものを示すときに、`country`値の一致をルーティングします。 これに対し、`RouteValueAttribute`フォールバック非属性を操作することができます。 例を定義するかどうか、`en-US`アクション次のような国コード`fr-FR`がフォールバックしてより汎用的なコント ローラーを持たない`[CountrySpecific(...)]`適用します。

`Order`プロパティ決定*ステージ*制約の一部であります。 アクション制約の実行に基づきグループ分け、`Order`です。 たとえば、すべてのフレームワーク提供 HTTP メソッドの属性を使用して同じ`Order`値の同じ段階で実行されるようにします。 多くの段階、目的のポリシーを実装する必要があることができます。

> [!TIP]
> 値を決定する`Order`HTTP メソッドより前に、制約を適用するかどうかについて考えます。 低い数値が最初に実行します。
