---
title: "フィルター"
author: ardalis
description: "*フィルター*のしくみと、ASP.NET Core MVC での使用方法について説明します。"
manager: wpickett
ms.author: tdykstra
ms.date: 12/12/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/filters
ms.openlocfilehash: 8549083ad42f3b81f850c0572b36dd99c4f50350
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="filters"></a>フィルター

作成者: [Tom Dykstra](https://github.com/tdykstra/)、[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC で*フィルター*を使用すると、要求処理パイプラインの特定のステージの前または後にコードを実行できます。

 組み込みフィルターは、承認 (承認されていないユーザーのリソースへのアクセスを防止する)、すべての要求が HTTPS を使用していることの保証、および応答のキャッシュ (要求パイプラインをショート サーキットしてキャッシュされた応答を返す) などのタスクを処理します。 

カスタム フィルターを作成してアプリケーションの横断的な問題を処理することができます。 アクション間でのコードの重複を避けたい場合には、フィルターが解決策になります。 たとえば、エラー処理コードを例外フィルターに統合することができます。

[GitHub のサンプルを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)。

## <a name="how-do-filters-work"></a>フィルターのしくみ

*MVC のアクション呼び出しパイプライン*内で実行されるフィルターは、*フィルター パイプライン*と呼ばれることがあります。  フィルター パイプラインは、MVC が実行するアクションを選択した後に実行されます。

![要求は、他のミドルウェア、ルーティング ミドルウェア、アクション選択、および MVC のアクション呼び出しパイプラインを通じて処理されます。 要求の処理は、クライアントに送信される応答になる前に、アクション選択、ルーティング ミドルウェア、およびさまざまな他のミドルウェアを遡って続行されます。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>フィルターの種類

フィルターの種類はそれぞれ、フィルター パイプラインの異なるステージで実行されます。

* [承認フィルター](#authorization-filters)は、最初に実行され、現在のユーザーが現在の要求に対して承認されているかどうかを判断するために使用されます。 要求が承認されていない場合は、パイプラインをショートサーキットできます。 

* [リソース フィルター](#resource-filters)は、承認後、最初に要求を処理します。  このフィルターは、残りのフィルター パイプラインの完了前と完了後にコードを実行できます。 キャッシュを実装するか、そうでない場合はパフォーマンス上の理由からフィルター パイプラインをショートサーキットする場合に便利です。 このフィルターはモデル バインドの前に実行されるため、モデル バインドに影響を与える必要があるすべてのものに役立ちます。

* [アクション フィルター](#action-filters)は、個々のアクション メソッドが呼び出される直前と直後にコードを実行できます。 アクションに渡される引数とアクションから返される結果を操作するために使用できます。

* [例外フィルター](#exception-filters)は、応答本文に何かが書き込まれる前に発生する未処理の例外にグローバル ポリシーを適用するために使用されます。

* [結果フィルター](#result-filters)は、個々のアクション結果の実行の直前と直後にコードを実行できます。 このフィルターは、アクション メソッドが正常に実行された場合にのみ実行され、ビューまたはフォーマッタの実行を囲む必要があるロジックに役立ちます。

これらのフィルターの種類がフィルター パイプラインでどのように連携しているかを、次の図に示します。

![要求は、承認フィルター、リソース フィルター、モデル バインド、アクション フィルター、アクションの実行とアクション結果の変換、例外フィルター、結果フィルター、結果の実行を介して処理されます。 終了間際に、要求は結果フィルターとリソース フィルターのみで処理されてから、クライアントに送信される応答になります。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>実装

フィルターは、異なるインターフェイス定義を介して、同期と非同期の実装をサポートします。 実行する必要があるタスクの種類に応じて、同期または非同期のバリアントを選択します。 

パイプライン ステージの前後でコードを実行できる同期フィルターは、On*Stage*Executing メソッドと On*Stage*Executed メソッドを定義します。 たとえば、`OnActionExecuting` はアクション メソッドが呼び出される前に呼び出され、`OnActionExecuted` はアクション メソッドが返された後に呼び出されます。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

非同期フィルターは、1 つの On*Stage*ExecutionAsync メソッドを定義します。 このメソッドは、フィルターのパイプライン ステージを実行する *FilterType*ExecutionDelegate デリゲートを取得します。 たとえば、`ActionExecutionDelegate` はアクション メソッドを呼び出すので、その呼び出しの前後でコードを実行できます。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

1 つのクラスで複数のフィルター ステージに対してインターフェイスを実装することができます。 たとえば、[ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) 抽象クラスは、`IActionFilter` と `IResultFilter` の両方と、それらの非同期バージョンを実装します。

> [!NOTE]
> フィルター インターフェイスの同期と非同期バージョンの両方ではなく、**いずれか**を実装します。 フレームワークは、最初にフィルターが非同期インターフェイスを実装しているかどうかをチェックして、している場合はそれを呼び出します。 していない場合は、同期インターフェイスのメソッドを呼び出します。 1 つのクラスに両方のインターフェイスを実装すると、非同期のメソッドのみが呼び出されます。 [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) などの抽象クラスを使用する場合は、同期メソッドのみを上書きするか、フィルターの種類ごとに非同期メソッドを上書きします。

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` は、`IFilter` を実装します。 そのため、`IFilterFactory` インスタンスはフィルター パイプライン内の任意の場所で `IFilter` インスタンスとして使用できます。 フレームワークは、フィルターを呼び出す準備をする際に、それを `IFilterFactory` にキャストしようとします。 そのキャストが成功した場合、呼び出される `IFilter` インスタンスを作成するために `CreateInstance` メソッドが呼び出されます。 これにより、アプリケーションの起動時に正確なフィルター パイプラインを明示的に設定する必要がないため、非常に柔軟なデザインが可能になります。

フィルターを作成するための別の方法として、独自の属性の実装で `IFilterFactory` を実装できます。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>組み込みのフィルター属性

フレームワークには、サブクラスを作成したりカスタマイズしたりできる組み込みの属性ベースのフィルターが含まれます。 たとえば、次の結果フィルターは、応答にヘッダーを追加します。

<a name="add-header-attribute"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

属性は、上記の例のように、フィルターが引数を受け取れるようにします。 この属性をコントローラーまたはアクション メソッドに追加し、HTTP ヘッダーの名前と値を指定します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

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

フィルターは、3 つの*スコープ*のいずれかでパイプラインに追加することができます。 属性を使用して、特定のアクション メソッドまたはコントローラー クラスにフィルターを追加できます。 または、フィルターを (すべてのコント ローラーとアクション用に) グローバルに登録することができます。これには、フィルターを `Startup` クラス内の `ConfigureServices` メソッドの `MvcOptions.Filters` コレクションに追加します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>実行の既定の順序

パイプラインの特定のステージに対して複数のフィルターがある場合に、スコープがフィルターの実行の既定の順序を決定します。  グローバル フィルターがクラス フィルターを囲み、クラス フィルターがメソッド フィルターを囲みます。 これは、[入れ子人形](https://wikipedia.org/wiki/Matryoshka_doll)のように、スコープが前のスコープを囲むたびに大きくなることから、"マトリョーシカ人形" 入れ子と呼ばれることがあります。 通常は、明示的に順序を決定しなくても、目的の上書き動作が得られます。

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

このシーケンスは、メソッド フィルターが、コントローラー フィルター内で入れ子になっていて、コントローラー フィルターがグローバル フィルター内で入れ子になっていることを示しています。 別の言い方をすると、非同期フィルターの On*Stage*ExecutionAsync メソッド内にいる場合、コードがスタックにある間に、厳密なスコープを持つすべてのフィルターが実行されます。

> [!NOTE]
> `Controller` 基底クラスから継承するすべてのコントローラーには、`OnActionExecuting` メソッドと `OnActionExecuted` メソッドが含まれます。 これらのメソッドは、特定のアクションに対して実行されるフィルターをラップします。`OnActionExecuting` はどのフィルターよりも前に呼び出され、`OnActionExecuted` はすべてのフィルターの後に呼び出されます。

### <a name="overriding-the-default-order"></a>既定の順序の上書き

`IOrderedFilter` を実装することで、実行の既定の順序を上書きできます。 このインターフェイスは、実行の順序を決定するために、スコープよりも優先される `Order` プロパティを公開します。 `Order` 値が低いフィルターがその *before* コードを、その `Order` の高い値よりも前に実行させます。 `Order` 値が低いフィルターがその *after* コードを、その高い `Order` の値よりも後に実行させます。 `Order` プロパティは、コンストラクター パラメーターを使用して設定できます。

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

フィルターの実行順序を決定するときに、`Order` プロパティはスコープより優先されます。 最初に順序でフィルターが並べ替えられ、次に同じ順位の優先度を決めるためにスコープが使用されます。 すべての組み込みフィルターは `IOrderedFilter` を実装し、既定の `Order` 値を 0 に設定するため、`Order` を 0 以外の値に設定しない限り、スコープによって順序が決定されます。

## <a name="cancellation-and-short-circuiting"></a>キャンセルとショート サーキット

フィルター メソッドに提供される `context` パラメーターで `Result` プロパティを設定することで、フィルター パイプラインを任意の時点でショート サーキットできます。 たとえば、次のリソース フィルターは、パイプラインの残りの部分が実行されるのを防止します。

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

次のコードでは、`ShortCircuitingResourceFilter` と `AddHeader` の両方のフィルターが `SomeResource` アクション メソッドをターゲットにしています。 ただし、`ShortCircuitingResourceFilter` が最初に実行され (これがリソース フィルターで、`AddHeader` がアクション フィルターであるため)、パイプラインの残りの部分がショートカットされるため、`AddHeader` フィルターが `SomeResource` アクションに対して実行されることはありません。 (たとえば、フィルターの種類、または `Order` プロパティの明示的な使用により) `ShortCircuitingResourceFilter` が最初に実行された場合は、両方のフィルターがアクション メソッド レベルで適用されると、この動作が同じになります。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

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

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

フィルターの種類を登録せずに `ServiceFilter` を使用すると、例外が発生します。

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` は `IFilterFactory` を実装します。これは、`IFilter` インスタンスを作成するために 1 つのメソッドを公開します。 `ServiceFilterAttribute` の場合、`IFilterFactory` インターフェイスの `CreateInstance` メソッドが実装され、サービス コンテナー (DI) から指定した種類を読み込みます。

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` は`ServiceFilterAttribute` とよく似ています (`IFilterFactory` も実装します) が、その型は DI コンテナーから直接解決されません。 代わりに、`Microsoft.Extensions.DependencyInjection.ObjectFactory` を使って型をインスタンス化します。

この違いにより、`TypeFilterAttribute` を使用して参照される型は、最初にコンテナーに登録する必要はありません (ただし、コンテナーによって満たされる依存関係があります)。 また、`TypeFilterAttribute` は必要に応じて、対象の型のコンストラクター引数を受け取ることができます。 次の例は、`TypeFilterAttribute` を使用して、型に引数を渡す方法を示しています。

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

引数を必要としないが、DI によって満たされる必要があるコンストラクターの依存関係があるフィルターがある場合、`[TypeFilter(typeof(FilterType))]` の代わりに、独自に名前を付けた属性をクラスとメソッドで使用できます。 次のフィルターは、これを実装する方法を示しています。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

このフィルターは、`[TypeFilter]` または `[ServiceFilter]` を使用する代わりに、`[SampleActionFilter]` 構文を使用して、クラスまたはメソッドに適用することができます。

## <a name="authorization-filters"></a>承認フィルター

*承認フィルター*は、アクション メソッドへのアクセスを制御し、フィルター パイプライン内で実行される最初のフィルターです。 before メソッドと after メソッドをサポートする他の多くのフィルターとは異なり、このフィルターには before メソッドしかありません。 独自の承認フレームワークを記述している場合は、カスタムの承認フィルターを記述するだけで済みます。 カスタム フィルターを記述するよりも、独自の承認ポリシーを構成するか、カスタム承認ポリシーを記述することを選びます。 組み込みフィルターの実装は、認証システムを呼び出すことしかしません。

例外を処理するものがないため (例外フィルターは例外を処理しません)、承認フィルター内で例外をスローしないでください。 代わりに、チャレンジを発行するか、別の方法を探します。

承認の詳細については、[こちら](../../security/authorization/index.md)を参照してください。

## <a name="resource-filters"></a>リソース フィルター

*リソース フィルター*は、`IResourceFilter` または `IAsyncResourceFilter` のいずれかのインターフェイスを実装します。これらの実行は、フィルター パイプラインの大部分をラップします  (これより前に実行されるのは、[承認フィルター](#authorization-filters)だけです)。リソース フィルターは、要求が行っている作業の大部分をショートサーキットする必要がある場合に、特に便利です。 たとえば、フィルターをキャッシュすることで、応答が既にキャッシュ内にある場合に、パイプラインの残りの部分を回避することができます。

前に示した[リソース フィルターのショート サーキット](#short-circuiting-resource-filter)は、リソース フィルターの一例です。 別の例は [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs) で、これはモデル バインドがフォーム データにアクセスすることを防止します。 これは、大きなファイルのアップロードを受信することがわかっていて、フォームがメモリに読み込まれないようにしたい場合に役立ちます。

## <a name="action-filters"></a>アクション フィルター

*アクション フィルター*は、`IActionFilter` または `IAsyncActionFilter` のいずれかのインターフェイスを実装します。これらの実行はアクション メソッドの実行を囲みます。

サンプルのアクション フィルターを次に示します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) が次のプロパティを提供します。

* `ActionArguments`: アクションへの入力を操作できます。
* `Controller`: コントローラー インスタンスを操作できます。 
* `Result`: これを設定することで、アクション メソッドと後続のアクション フィルターの実行をショートサーキットします。 例外をスローすることで、アクション メソッドと後続のアクション フィルターの実行も防止できますが、成功の結果ではなく、エラーとして扱われます。

[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) は、`Controller` と `Result` に加え、次のプロパティを提供します。

* `Canceled`: 別のフィルターによってアクションの実行がショートサーキットされた場合は、true になります。
* `Exception`: アクションまたは後続のアクション フィルターが例外をスローした場合は、null 以外になります。 このプロパティを null に設定すると、例外を効果的に '処理' でき、`Result` がアクション メソッドから通常に返されたかのように実行されます。

`IAsyncActionFilter` の場合、`ActionExecutionDelegate` への呼び出しによって後続のすべてのアクション フィルターとアクション メソッドが実行され、`ActionExecutedContext` が返されます。 ショートサーキットするには、`ActionExecutingContext.Result` をいくつかの結果インスタンスに割り当てます。`ActionExecutionDelegate` は呼び出さないでください。

フレームワークは、サブクラス化できる抽象 `ActionFilterAttribute` を提供します。 

アクション フィルターを使用して、自動的にモデルの状態を検証し、状態が無効な場合に、エラーを返すことができます。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` メソッドは、アクション メソッドの後に実行され、`ActionExecutedContext.Result` プロパティを通じてアクションの結果を確認および操作することができます。 別のフィルターによってアクションの実行がショートサーキットされた場合、`ActionExecutedContext.Canceled` は true に設定されます。 アクションまたは後続のアクション フィルターが例外をスローした場合、`ActionExecutedContext.Exception` は null 以外の値に設定されます。 `ActionExecutedContext.Exception` を null に設定すると、例外を効果的に '処理' でき、`ActionExectedContext.Result` がアクション メソッドから通常に返されたかのように実行されます。

## <a name="exception-filters"></a>例外フィルター

*例外フィルター*は、`IExceptionFilter` または `IAsyncExceptionFilter` のいずれかのインターフェイスを実装します。 これを使用して、アプリの共通のエラー処理ポリシーを実装できます。 

次の例外フィルターのサンプルでは、カスタムの開発エラー ビューを使用して、アプリケーションの開発中に発生する例外に関する詳細を表示します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

例外フィルターは、2 つのイベント (before と after 用) を持つのではなく、`OnException` (または `OnExceptionAsync`) だけを実装します。 

例外フィルターは、コントローラーの作成、[モデル バインド](../models/model-binding.md)、アクション フィルター、またはアクション メソッドで発生する未処理の例外を処理します。 リソース フィルター、結果フィルター、または MVC 結果の実行で発生した例外はキャッチしません。

例外を処理するには、`ExceptionContext.ExceptionHandled` プロパティを true に設定するか、応答を記述します。 これにより、例外の伝達を停止します。 例外フィルターでは、例外を "成功" に変えられないことに注意してください。 これができるのは、アクション フィルターだけです。

> [!NOTE]
> ASP.NET 1.1 では、`ExceptionHandled` を true に設定し、**さらに**応答を記述すると、応答が送信されません。 そのシナリオでは、ASP.NET Core 1.0 は応答を送信し、ASP.NET Core 1.1.2 は 1.0 の動作に戻ります。 詳細については、GitHub リポジトリで「[issue #5594](https://github.com/aspnet/Mvc/issues/5594)」を参照してください。 

例外フィルターは、MVC アクション内で発生する例外をトラップするのには適していますが、エラー処理ミドルウェアほどの柔軟性はありません。 一般的なケースにはミドルウェアを選択し、MVC アクションで選択されたのとは*異なる*方法でエラー処理を行う必要がある場合にのみフィルターを使用します。 たとえば、ご利用のアプリには、API エンドポイントとビュー/HTML の両方に対するアクション メソッドがある場合があります。 API エンドポイントは、JSON としてのエラー情報を返す可能性がある一方で、ビュー ベースのアクションがエラー ページを HTML として返す可能性があります。

フレームワークは、サブクラス化できる抽象 `ExceptionFilterAttribute` を提供します。 

## <a name="result-filters"></a>結果フィルター

*結果フィルター*は、`IResultFilter` または `IAsyncResultFilter` のいずれかのインターフェイスを実装します。これらの実行はアクション結果の実行を囲みます。 

HTTP ヘッダーを追加する結果フィルターの例を次に示します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

実行されている結果の種類は、対象のアクションに依存します。 ビューを返す MVC アクションには、実行されている `ViewResult` の一部として、すべての razor 処理が含まれます。 API メソッドは、結果の実行の一部としていくつかのシリアル化を実行できます。 アクション結果に関する詳細は、[こちら](actions.md)を参照してください。

結果フィルターは、アクションまたはアクション フィルターがアクションの結果を生成するときに、成功した結果に対してのみ実行されます。 例外フィルターが例外を処理するときには、結果フィルターは実行されません。

`OnResultExecuting` メソッドは、`ResultExecutingContext.Cancel` を true に設定することで、アクションの結果と後続の結果フィルターの実行をショートサーキットできます。 ショートサーキットする場合は、通常、空の応答が生成されないように応答オブジェクトに記述する必要があります。 例外をスローすることで、アクションの結果と後続のフィルターの実行も防止できますが、成功の結果ではなく、エラーとして扱われます。

`OnResultExecuted` メソッドを実行するときに、応答がクライアントに送信され、(例外がスローされない限り) それ以上変更できない可能性があます。 別のフィルターによってアクションの結果の実行がショートサーキットされた場合、`ResultExecutedContext.Canceled` は true に設定されます。

アクションの結果または後続の結果フィルターが例外をスローした場合、`ResultExecutedContext.Exception` は null 以外の値に設定されます。 `Exception` を null に設定すると、例外を効果的に '処理' でき、パイプラインの後方で MVC によって例外が再スローされることを防げます。 結果フィルター内の例外を処理しているときに、応答にどのデータも書き込めない場合があります。 アクションの結果がその実行の途中でスローして、クライアントに対してはヘッダーが既にフラッシュされている場合、エラー コードを送信するための信頼性の高いメカニズムはありません。

`IAsyncResultFilter` の場合、`ResultExecutionDelegate` での `await next()` への呼び出しは、後続のすべての結果フィルターとアクションの結果を実行します。 ショートサーキットするには、`ResultExecutingContext.Cancel` を true に設定します。`ResultExectionDelegate` は呼び出さないでください。

フレームワークは、サブクラス化できる抽象 `ResultFilterAttribute` を提供します。 前に示した [AddHeaderAttribute](#add-header-attribute) クラスは、結果フィルター属性の一例です。

## <a name="using-middleware-in-the-filter-pipeline"></a>フィルター パイプラインでのミドルウェアの使用

リソース フィルターは、パイプラインの後方で登場するすべての実行を囲む点で、[ミドルウェア](../../fundamentals/middleware.md)のように機能します。 ただし、フィルターは MVC の一部である点がミドルウェアとは異なります。つまり、フィルターには MVC コンテキストとコンストラクトへのアクセスがあります。

ASP.NET Core 1.1 では、フィルター パイプラインでミドルウェアを使用できます。 MVC ルート データへのアクセスを必要とする、または特定のコントローラーまたはアクションを実行する必要があるミドルウェア コンポーネントがある場合は、ミドルウェアを使用できます。

ミドルウェアをフィルターとして使用するには、フィルター パイプラインに挿入するミドルウェアを指定する `Configure` メソッドを使用して型を作成します。 ローカリゼーション ミドルウェアを使用して要求の現在のカルチャを確立する例を次に示します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

`MiddlewareFilterAttribute` を使用して、選択したコントローラーまたはアクションに対してミドルウェアを実行することも、グローバルにミドルウェアを実行することもできます。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

ミドルウェア フィルターは、フィルター パイプラインのリソース フィルターと同じステージ (モデル バインドの前、残りのパイプラインの後) で実行されます。

## <a name="next-actions"></a>次の操作

フィルターを試すには、[ここ](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)からサンプルをダウンロードして、テストおよび変更を行います。
