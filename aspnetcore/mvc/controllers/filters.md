---
title: ASP.NET Core フィルター
author: ardalis
description: フィルターのしくみと ASP.NET Core MVC でそれを使用する方法について説明します。
ms.author: riande
ms.date: 08/15/2018
uid: mvc/controllers/filters
ms.openlocfilehash: e20d934a17337d404249220d703ac4bb7164dfa6
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402160"
---
# <a name="filters-in-aspnet-core"></a>ASP.NET Core フィルター

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra/)、[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC で*フィルター*を使用すると、要求処理パイプラインの特定のステージの前または後にコードを実行できます。

> [!IMPORTANT]
> このトピックは Razor ページには適用**されません**。 ASP.NET Core 2.1 以降では、Razor ページの [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) と [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) に対応しています。 詳細については、[Razor ページのフィルター メソッド](xref:razor-pages/filter)に関するページを参照してください。

 組み込みのフィルターでは次のようなタスクが処理されます。

 * 許可 (ユーザーに許可が与えられていないリソースの場合、アクセスを禁止する)。
 * すべての要求で HTTPS を使用する。
 * 応答キャッシュ (要求パイプラインを迂回し、キャッシュされている応答を返す)。 

横断的な問題を処理するカスタム フィルターを作成できます。 フィルターでは、アクション間のコード重複を回避できます。 たとえば、エラー処理例外フィルターではエラー処理を統合できます。

[GitHub のサンプルを表示またはダウンロードしてください](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。

## <a name="how-do-filters-work"></a>フィルターのしくみ

*MVC のアクション呼び出しパイプライン*内で実行されるフィルターは、*フィルター パイプライン*と呼ばれることがあります。  フィルター パイプラインは、MVC が実行するアクションを選択した後に実行されます。

![要求は、他のミドルウェア、ルーティング ミドルウェア、アクション選択、および MVC のアクション呼び出しパイプラインを通じて処理されます。 要求の処理は、クライアントに送信される応答になる前に、アクション選択、ルーティング ミドルウェア、およびさまざまな他のミドルウェアを遡って続行されます。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>フィルターの種類

フィルターの種類はそれぞれ、フィルター パイプラインの異なるステージで実行されます。

* [承認フィルター](#authorization-filters)は、最初に実行され、現在のユーザーが現在の要求に対して承認されているかどうかを判断するために使用されます。 要求が承認されていない場合は、パイプラインをショートサーキットできます。 

* [リソース フィルター](#resource-filters)は、承認後、最初に要求を処理します。  このフィルターは、残りのフィルター パイプラインの完了前と完了後にコードを実行できます。 キャッシュを実装するか、そうでない場合はパフォーマンス上の理由からフィルター パイプラインをショートサーキットする場合に便利です。 モデル バインドに変化が及ぶように、モデル バインドの前に実行されます。

* [アクション フィルター](#action-filters)は、個々のアクション メソッドが呼び出される直前と直後にコードを実行できます。 アクションに渡される引数とアクションから返される結果を操作するために使用できます。

* [例外フィルター](#exception-filters)は、応答本文に何かが書き込まれる前に発生する未処理の例外にグローバル ポリシーを適用するために使用されます。

* [結果フィルター](#result-filters)は、個々のアクション結果の実行の直前と直後にコードを実行できます。 アクション メソッドが正常に実行された場合にのみ実行されます。 ビューまたはフォーマッタ実行を取り囲む必要があるロジックに便利です。

これらのフィルターの種類がフィルター パイプラインでどのように連携しているかを、次の図に示します。

![要求は、承認フィルター、リソース フィルター、モデル バインド、アクション フィルター、アクションの実行とアクション結果の変換、例外フィルター、結果フィルター、結果の実行を介して処理されます。 終了間際に、要求は結果フィルターとリソース フィルターのみで処理されてから、クライアントに送信される応答になります。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>実装

フィルターは、異なるインターフェイス定義を介して、同期と非同期の実装をサポートします。 

パイプライン ステージの前後でコードを実行できる同期フィルターは、On*Stage*Executing メソッドと On*Stage*Executed メソッドを定義します。 たとえば、`OnActionExecuting` はアクション メソッドが呼び出される前に呼び出され、`OnActionExecuted` はアクション メソッドが返された後に呼び出されます。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

非同期フィルターは、1 つの On*Stage*ExecutionAsync メソッドを定義します。 このメソッドは、フィルターのパイプライン ステージを実行する *FilterType*ExecutionDelegate デリゲートを取得します。 たとえば、`ActionExecutionDelegate` はアクション メソッドまたは次のアクション フィルターを呼び出すので、その呼び出しの前後でコードを実行できます。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

1 つのクラスで複数のフィルター ステージに対してインターフェイスを実装することができます。 たとえば、[ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) クラスは、`IActionFilter` と `IResultFilter` と、それらの非同期バージョンを実装します。

> [!NOTE]
> フィルター インターフェイスの同期と非同期バージョンの両方ではなく、**いずれか**を実装します。 フレームワークは、最初にフィルターが非同期インターフェイスを実装しているかどうかをチェックして、している場合はそれを呼び出します。 していない場合は、同期インターフェイスのメソッドを呼び出します。 1 つのクラスに両方のインターフェイスを実装すると、非同期のメソッドのみが呼び出されます。 [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) などの抽象クラスを使用する場合は、同期メソッドのみをオーバーライドするか、フィルターの種類ごとに非同期メソッドをオーバーライドします。

### <a name="ifilterfactory"></a>IFilterFactory

[IFilterFactory](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory) は [IFilterMetadata](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifiltermetadata) を実装します。 そのため、`IFilterFactory` インスタンスはフィルター パイプライン内の任意の場所で `IFilterMetadata` インスタンスとして使用できます。 フレームワークは、フィルターを呼び出す準備をする際に、それを `IFilterFactory` にキャストしようとします。 そのキャストが成功した場合、呼び出される `IFilterMetadata` インスタンスを作成するために [CreateInstance](/dotnet/api/microsoft.aspnetcore.mvc.filters.ifilterfactory.createinstance) メソッドが呼び出されます。 これにより、アプリの起動時に正確なフィルター パイプラインを明示的に設定する必要がないため、柔軟なデザインが可能になります。

フィルターを作成するための別の方法として、独自の属性の実装で `IFilterFactory` を実装できます。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>組み込みのフィルター属性

フレームワークには、サブクラスを作成したりカスタマイズしたりできる組み込みの属性ベースのフィルターが含まれます。 たとえば、次の結果フィルターは、応答にヘッダーを追加します。

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

属性は、上記の例のように、フィルターが引数を受け取れるようにします。 この属性をコントローラーまたはアクション メソッドに追加し、HTTP ヘッダーの名前と値を指定します。

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

`Index` アクションの結果を次に示します。応答ヘッダーが右下に表示されています。

![("Author Steve Smith @ardalis" が含まれている) 応答ヘッダーを表示している Microsoft Edge の開発者ツール](filters/_static/add-header.png)

フィルター インターフェイスのいくつかには対応する属性があり、カスタムの実装に基底クラスとして使用できます。

フィルター属性:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` と `ServiceFilterAttribute` については、[この記事で後ほど](#dependency-injection)説明します。

## <a name="filter-scopes-and-order-of-execution"></a>フィルターのスコープと実行の順序

フィルターは、3 つの*スコープ*のいずれかでパイプラインに追加することができます。 属性を使用して、特定のアクション メソッドまたはコントローラー クラスにフィルターを追加できます。 あるいは、すべてのコントローラーとアクションに対してフィルターをグローバル登録できます。 `ConfigureServices` の `MvcOptions.Filters` コレクションに追加することでフィルターをグローバルに追加できます。

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>実行の既定の順序

パイプラインの特定のステージに対して複数のフィルターがある場合に、スコープがフィルターの実行の既定の順序を決定します。  グローバル フィルターがクラス フィルターを囲み、クラス フィルターがメソッド フィルターを囲みます。 これは、[入れ子人形](https://wikipedia.org/wiki/Matryoshka_doll)のように、スコープが前のスコープを囲むたびに大きくなることから、"マトリョーシカ人形" 入れ子と呼ばれることがあります。 通常は、明示的に順序を決定しなくても、目的のオーバーライド動作が得られます。

この入れ子の結果として、フィルターの *after* コードが *before* コードと逆の順序で実行されます。 シーケンスは次のようになります。

* グローバルに適用されるフィルターの *before* コード
  * コントローラーに適用されるフィルターの *before* コード
    * アクション メソッドに適用されるフィルターの *before* コード
    * アクション メソッドに適用されるフィルターの *after* コード
  * コントローラーに適用されるフィルターの *after* コード
* グローバルに適用されるフィルターの *after* コード
  
同期アクション フィルターに対して呼び出されるフィルター メソッドの順序を示す例を次に示します。

| シーケンス | フィルターのスコープ | フィルター メソッド |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | コントローラー | `OnActionExecuting` |
| 3 | メソッド | `OnActionExecuting` |
| 4 | メソッド | `OnActionExecuted` |
| 5 | コントローラー | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

このシーケンスが示すもの:

* メソッド フィルターは、コントローラー フィルター内で入れ子になります。
* コントローラー フィルターは、グローバル フィルター内で入れ子になります。 

別の言い方をすると、非同期フィルターの On*Stage*ExecutionAsync メソッド内にいる場合、コードがスタックにある間に、厳密なスコープを持つすべてのフィルターが実行されます。

> [!NOTE]
> `Controller` 基底クラスから継承するすべてのコントローラーには、`OnActionExecuting` メソッドと `OnActionExecuted` メソッドが含まれます。 これらのメソッドは、特定のアクションに対して実行されるフィルターをラップします。`OnActionExecuting` はどのフィルターよりも前に呼び出され、`OnActionExecuted` はすべてのフィルターの後に呼び出されます。

### <a name="overriding-the-default-order"></a>既定の順序のオーバーライド

`IOrderedFilter` を実装することで、実行の既定の順序をオーバーライドできます。 このインターフェイスは、実行の順序を決定するために、スコープよりも優先される `Order` プロパティを公開します。 `Order` 値が低いフィルターがその *before* コードを、その `Order` の高い値よりも前に実行させます。 `Order` 値が低いフィルターがその *after* コードを、その高い `Order` の値よりも後に実行させます。 `Order` プロパティは、コンストラクター パラメーターを使用して設定できます。

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

上記の例で示されているのと同じ 3 つのアクション フィルターがあるが、コントローラーとグローバル フィルターの `Order` プロパティがそれぞれ 1 と 2 に設定されている場合、実行の順序が逆になります。

| シーケンス | フィルターのスコープ | `Order` プロパティ | フィルター メソッド |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | メソッド | 0 | `OnActionExecuting` |
| 2 | コントローラー | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | コントローラー | 1  | `OnActionExecuted` |
| 6 | メソッド | 0  | `OnActionExecuted` |

フィルターの実行順序を決定するときに、`Order` プロパティはスコープより優先されます。 最初に順序でフィルターが並べ替えられ、次に同じ順位の優先度を決めるためにスコープが使用されます。 組み込みのフィルターはすべて `IOrderedFilter` を実装し、既定の `Order` 値を 0 に設定します。 組み込みのフィルターの場合、`Order` をゼロ以外の値に設定しない限り、スコープによって順序が決定されます。

## <a name="cancellation-and-short-circuiting"></a>キャンセルとショート サーキット

フィルター メソッドに提供される `context` パラメーターで `Result` プロパティを設定することで、フィルター パイプラインを任意の時点でショート サーキットできます。 たとえば、次のリソース フィルターは、パイプラインの残りの部分が実行されるのを防止します。

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

次のコードでは、`ShortCircuitingResourceFilter` と `AddHeader` の両方のフィルターが `SomeResource` アクション メソッドをターゲットにしています。 `ShortCircuitingResourceFilter`:

* これはリソース フィルターであり、`AddHeader` はアクション フィルターであるため、最初に実行されます。
* パイプラインの残りの部分は迂回されます。

そのため、`SomeResource` アクションの場合、`AddHeader` フィルターが実行されることはありません。 `ShortCircuitingResourceFilter` が最初に実行された場合は、両方のフィルターがアクション メソッド レベルで適用されると、この動作が同じになります。 そのフィルターの種類が原因で、あるいは `Order` プロパティの明示的な使用により、`ShortCircuitingResourceFilter` が最初に実行されます。

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>依存関係の挿入

フィルターは種類ごとまたはインスタンスごとに追加できます。 インスタンスを追加する場合、そのインスタンスがすべての要求に対して使用されます。 種類を追加すると、種類でアクティブ化されます。つまり、要求ごとにインスタンスが作成され、[依存関係の挿入](../../fundamentals/dependency-injection.md) (DI) によってコンストラクターの依存関係が設定されます。 種類ごとにフィルターを追加するのは、`filters.Add(new TypeFilterAttribute(typeof(MyFilter)))` に相当します。

属性として実装され、コントローラー クラスまたはアクション メソッドに直接追加されるフィルターは、[依存関係の挿入](../../fundamentals/dependency-injection.md) (DI) によって提供されるコンストラクターの依存関係を持つことはできません。 これは、属性には、適用される場所で提供される独自のコンストラクター パラメーターが必要だからです。 これは、属性のしくみの制限です。

フィルターに DI からアクセスする必要のある依存関係がある場合、サポートされているいくつかの方法があります。 次のいずれかを使用して、クラスまたはアクション メソッドにフィルターを適用できます。

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* 属性に実装された `IFilterFactory`

> [!NOTE]
> DI から取得できる依存関係の 1 つに、ロガーがあります。 ただし、必要な[組み込みフレームワークのログ機能](xref:fundamentals/logging/index)は既に提供されている場合があるため、ログ目的のためだけにフィルターを作成して使用することは避けてください。 フィルターにログ記録を追加する場合は、MVC アクションやその他のフレームワーク イベントではなく、ビジネス ドメインの懸案事項や、フィルターに固有の動作を重視する必要があります。

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

`ServiceFilter` は DI からフィルターのインスタンスを取得します。 `ConfigureServices` でコンテナーにフィルターを追加し、`ServiceFilter` 属性でそれを参照します。

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

フィルターの種類を登録せずに `ServiceFilter` を使用すると、例外が発生します。

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` は、`IFilterFactory` を実装します。 `IFilterFactory` は、`IFilterMetadata` インスタンスを作成するために `CreateInstance` メソッドを公開します。 `CreateInstance` メソッドは、サービス コンテナー (DI) から、指定した型を読み込みます。

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` は`ServiceFilterAttribute` と似ていますが、その型は DI コンテナーから直接解決されません。 `Microsoft.Extensions.DependencyInjection.ObjectFactory` を使って型をインスタンス化します。

この違いにより:

* `TypeFilterAttribute` を利用して参照される型は、先にコンテナーに登録する必要がありません。  コンテナーによって依存関係が満たされています。 
* `TypeFilterAttribute` は必要に応じて、型のコンストラクター引数を受け取ることができます。 

次の例は、`TypeFilterAttribute` を使用して、型に引数を渡す方法を示しています。

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

### <a name="ifilterfactory-implemented-on-your-attribute"></a>属性に実装された IFilterFactory

次のようなフィルターがある場合:

* 引数を必要としない。
* DI で満たす必要があるコンストラクター依存関係がある。

`[TypeFilter(typeof(FilterType))]` の代わりに、クラスとメソッドで独自の名前付き属性を使用できます。 次のフィルターは、これを実装する方法を示しています。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

このフィルターは、`[TypeFilter]` または `[ServiceFilter]` を使用する代わりに、`[SampleActionFilter]` 構文を使用して、クラスまたはメソッドに適用することができます。

## <a name="authorization-filters"></a>承認フィルター

*承認フィルター:
* アクション メソッドへのアクセスを制御します。
* フィルター パイプライン内で実行される最初のフィルターです。 
* before メソッドが与えられ、after メソッドは与えられません。 

独自の承認フレームワークを記述している場合は、カスタムの承認フィルターを記述するだけで済みます。 カスタム フィルターを記述するよりも、独自の承認ポリシーを構成するか、カスタム承認ポリシーを記述することを選びます。 組み込みフィルターの実装は、認証システムを呼び出すことしかしません。

例外を処理するものがないため (例外フィルターは例外を処理しません)、承認フィルター内で例外をスローしないでください。 例外が発生した場合、チャレンジ発行を検討してください。

承認の詳細については、[こちら](../../security/authorization/index.md)を参照してください。

## <a name="resource-filters"></a>リソース フィルター

* `IResourceFilter` または `IAsyncResourceFilter` のインターフェイスを実装します。
* その実行では、多くのフィルター パイプラインがラップされます。 
* [承認フィルター](#authorization-filters)のみ、リソース フィルターの前に実行されます。

リソース フィルターは、要求が行っている作業の大部分を迂回する場合に便利です。 たとえば、フィルターをキャッシュすることで、応答がキャッシュ内にある場合に、パイプラインの残りの部分を回避することができます。

前に示した[リソース フィルターのショート サーキット](#short-circuiting-resource-filter)は、リソース フィルターの一例です。 もう 1 つの例が [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) です。

* モデル バインドがフォーム データにアクセスすることを禁止します。 
* 大きなファイルのアップロードや、メモリにフォームが読み込まれないようにするときに便利です。

## <a name="action-filters"></a>アクション フィルター

*アクション フィルター*:

* `IActionFilter` または `IAsyncActionFilter` のインターフェイスを実装します。
* この実行はアクション メソッドの実行を取り囲みます。

サンプルのアクション フィルターを次に示します。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) が次のプロパティを提供します。

* `ActionArguments`: アクションへの入力を操作できます。
* `Controller`: コントローラー インスタンスを操作できます。 
* `Result`: これを設定することで、アクション メソッドと後続のアクション フィルターの実行をショートサーキットします。 例外をスローすることで、アクション メソッドと後続のアクション フィルターの実行も防止できますが、成功の結果ではなく、エラーとして扱われます。

[ActionExecutedContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) は、`Controller` と `Result` に加え、次のプロパティを提供します。

* `Canceled`: 別のフィルターによってアクションの実行がショートサーキットされた場合は、true になります。
* `Exception`: アクションまたは後続のアクション フィルターが例外をスローした場合は、null 以外になります。 このプロパティを null に設定すると、例外を効果的に '処理' でき、`Result` がアクション メソッドから通常に返されたかのように実行されます。

`IAsyncActionFilter` の場合、`ActionExecutionDelegate` の呼び出しによって:

* 後続のすべてのアクション フィルターとアクション メソッドが実行されます。
* `ActionExecutedContext` が返されます。 

ショートサーキットするには、`ActionExecutingContext.Result` をいくつかの結果インスタンスに割り当てます。`ActionExecutionDelegate` は呼び出さないでください。

フレームワークは、サブクラス化できる抽象 `ActionFilterAttribute` を提供します。 

アクション フィルターを使用してモデルの状態を検証し、状態が無効な場合に、エラーを返すことができます。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` メソッドは、アクション メソッドの後に実行され、`ActionExecutedContext.Result` プロパティを通じてアクションの結果を確認および操作することができます。 別のフィルターによってアクションの実行がショートサーキットされた場合、`ActionExecutedContext.Canceled` は true に設定されます。 アクションまたは後続のアクション フィルターが例外をスローした場合、`ActionExecutedContext.Exception` は null 以外の値に設定されます。 `ActionExecutedContext.Exception` を null に設定すると:

* 例外が効果的に '処理' されます。
* `ActionExectedContext.Result` は、アクション メソッドから通常どおり返されたかのように実行されます。

## <a name="exception-filters"></a>例外フィルター

*例外フィルター*は、`IExceptionFilter` または `IAsyncExceptionFilter` のいずれかのインターフェイスを実装します。 これを使用して、アプリの共通のエラー処理ポリシーを実装できます。 

次の例外フィルターのサンプルでは、カスタムの開発エラー ビューを使用して、アプリの開発中に発生する例外に関する詳細を表示します。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

例外フィルター:

* before イベントと after イベントが与えられません。 
* `OnException` または `OnExceptionAsync` を実装します。 
* コントローラーの作成、[モデル バインド](../models/model-binding.md)、アクション フィルター、またはアクション メソッドで発生する未処理の例外を処理します。 
* リソース フィルター、結果フィルター、または MVC 結果の実行で発生した例外はキャッチしません。

例外を処理するには、`ExceptionContext.ExceptionHandled` プロパティを true に設定するか、応答を記述します。 これにより、例外の伝達を停止します。 例外フィルターでは、例外を "成功" に変えられません。 これができるのは、アクション フィルターだけです。

> [!NOTE]
> ASP.NET Core 1.1 では、`ExceptionHandled` を true に設定し、**さらに**応答を記述すると、応答が送信されません。 そのシナリオでは、ASP.NET Core 1.0 は応答を送信し、ASP.NET Core 1.1.2 は 1.0 の動作に戻ります。 詳細については、GitHub リポジトリで「[issue #5594](https://github.com/aspnet/Mvc/issues/5594)」を参照してください。 

例外フィルター:

* MVC アクション内で発生する例外のトラップに適しています。
* エラー処理ミドルウェアほど柔軟ではありません。 

例外処理にはミドルウェアを選択してください。 選択された MVC アクションに基づき、*異なる*方法でエラーを処理する必要がある場合にのみ、例外フィルターを使用します。 たとえば、ご利用のアプリには、API エンドポイントとビュー/HTML の両方に対するアクション メソッドがある場合があります。 API エンドポイントは、JSON としてのエラー情報を返す可能性がある一方で、ビュー ベースのアクションがエラー ページを HTML として返す可能性があります。

`ExceptionFilterAttribute` はサブクラス化できます。 

## <a name="result-filters"></a>結果フィルター

* `IResultFilter` または `IAsyncResultFilter` のインターフェイスを実装します。
* この実行はアクション結果の実行を取り囲みます。 

HTTP ヘッダーを追加する結果フィルターの例を次に示します。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

実行されている結果の種類は、対象のアクションに依存します。 ビューを返す MVC アクションには、実行されている `ViewResult` の一部として、すべての razor 処理が含まれます。 API メソッドは、結果の実行の一部としていくつかのシリアル化を実行できます。 アクション結果に関する詳細は、[こちら](actions.md)を参照してください。

結果フィルターは、アクションまたはアクション フィルターがアクションの結果を生成するときに、成功した結果に対してのみ実行されます。 例外フィルターが例外を処理するときには、結果フィルターは実行されません。

`OnResultExecuting` メソッドは、`ResultExecutingContext.Cancel` を true に設定することで、アクションの結果と後続の結果フィルターの実行をショートサーキットできます。 ショートサーキットする場合は、通常、空の応答が生成されないように応答オブジェクトに記述する必要があります。 例外をスローする:

* アクション結果と後続フィルターの実行が回避されます。
* 結果は成功ではなく、失敗として処理されます。

`OnResultExecuted` メソッドを実行するときに、応答がクライアントに送信され、(例外がスローされない限り) それ以上変更できない可能性があます。 別のフィルターによってアクションの結果の実行がショートサーキットされた場合、`ResultExecutedContext.Canceled` は true に設定されます。

アクションの結果または後続の結果フィルターが例外をスローした場合、`ResultExecutedContext.Exception` は null 以外の値に設定されます。 `Exception` を null に設定すると、例外を効果的に '処理' でき、パイプラインの後方で MVC によって例外が再スローされることを防げます。 結果フィルター内の例外を処理しているときに、応答にどのデータも書き込めない場合があります。 アクションの結果がその実行の途中でスローして、クライアントに対してはヘッダーが既にフラッシュされている場合、エラー コードを送信するための信頼性の高いメカニズムはありません。

`IAsyncResultFilter` の場合、`ResultExecutionDelegate` での `await next` への呼び出しは、後続のすべての結果フィルターとアクションの結果を実行します。 ショートサーキットするには、`ResultExecutingContext.Cancel` を true に設定します。`ResultExectionDelegate` は呼び出さないでください。

フレームワークは、サブクラス化できる抽象 `ResultFilterAttribute` を提供します。 前に示した [AddHeaderAttribute](#add-header-attribute) クラスは、結果フィルター属性の一例です。

## <a name="using-middleware-in-the-filter-pipeline"></a>フィルター パイプラインでのミドルウェアの使用

リソース フィルターは、パイプラインの後方で登場するすべての実行を囲む点で、[ミドルウェア](xref:fundamentals/middleware/index)のように機能します。 ただし、フィルターは MVC の一部である点がミドルウェアとは異なります。つまり、フィルターには MVC コンテキストとコンストラクトへのアクセスがあります。

ASP.NET Core 1.1 では、フィルター パイプラインでミドルウェアを使用できます。 MVC ルート データへのアクセスを必要とする、または特定のコントローラーまたはアクションを実行する必要があるミドルウェア コンポーネントがある場合は、ミドルウェアを使用できます。

ミドルウェアをフィルターとして使用するには、フィルター パイプラインに挿入するミドルウェアを指定する `Configure` メソッドを使用して型を作成します。 ローカリゼーション ミドルウェアを使用して要求の現在のカルチャを確立する例を次に示します。

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

`MiddlewareFilterAttribute` を使用して、選択したコントローラーまたはアクションに対してミドルウェアを実行することも、グローバルにミドルウェアを実行することもできます。

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

ミドルウェア フィルターは、フィルター パイプラインのリソース フィルターと同じステージ (モデル バインドの前、残りのパイプラインの後) で実行されます。

## <a name="next-actions"></a>次の操作

フィルターを試すには、[ここ](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)からサンプルをダウンロードして、テストおよび変更を行います。
