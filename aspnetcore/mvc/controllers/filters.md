---
title: "フィルター"
author: ardalis
description: "学習方法 * フィルター * 作業と ASP.NET Core MVC での使用方法です。"
keywords: "ASP.NET Core、フィルター、mvc のフィルター、アクション フィルター、承認フィルター、リソースのフィルター、結果のフィルター、例外フィルター"
ms.author: tdykstra
manager: wpickett
ms.date: 12/12/2016
ms.topic: article
ms.assetid: 531bda08-aa5b-4471-8f08-96add22c8683
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/filters
ms.openlocfilehash: 6baeb472770daf1d54b2d9ea894fc710f4f40780
ms.sourcegitcommit: 4693cb02d845adf2efa00e07ad432c81867bfa12
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/08/2017
---
# <a name="filters"></a>フィルター

によって[Tom Dykstra](https://github.com/tdykstra/)と[Steve Smith](http://ardalis.com)

*フィルター* ASP.NET Core MVC で使用すると、要求処理パイプラインの特定の段階の前後にコードを実行します。

 承認 (リソースのユーザーが承認されていないへのアクセスを防止) などの組み込みフィルター ハンドル タスクは、HTTPS、および応答のキャッシュ (キャッシュされた応答を返す要求パイプラインをショート サーキット) のすべての要求が使用することを確認できます。 

アプリケーションの横断的関心事を処理するカスタム フィルターを作成することができます。 コードをアクション間で重複しないようにするには、いつでも、フィルターは、ソリューションになります。 たとえば、例外フィルターでエラー処理コードを統合できます。

[GitHub からサンプルのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)です。

## <a name="how-do-filters-work"></a>フィルターのしくみ

フィルターを実行、 *MVC アクションの呼び出しのパイプライン*、場合によって呼ば、*フィルター パイプライン*です。  フィルター パイプラインは、MVC を実行するアクションを選択した後に実行されます。

![その他のミドルウェア、ミドルウェアのルーティング、アクションの選択、および MVC アクションの呼び出しパイプラインを通じて要求が処理されます。 操作の選択、ミドルウェアのルーティング、およびさまざまなその他のミドルウェアを介して返さクライアントに送信される応答になる前に、要求の処理が続行されます。](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>フィルターの種類

各フィルターの種類は、フィルター パイプラインのさまざまな段階で実行されます。

* [承認フィルター](#authorization-filters)最初に実行され、現在のユーザーが現在の要求に対する承認されたかどうかを判断するために使用します。 要求が承認されていない場合は、パイプラインをショート サーキット、ことができます。 

* [リソース フィルター](#resource-filters)最初に承認後に要求を処理します。  フィルター パイプラインおよびパイプラインの残りの部分が完了した後、残りの前にコードを実行できます。 キャッシュを実装するか、またはパフォーマンス上の理由から、フィルター パイプラインをショート サーキットそれ以外の場合に便利です。 モデル バインディングの前に実行されるため便利モデル バインドに影響を与える必要があるすべてのものです。

* [アクション フィルター](#action-filters)直前と個々 のアクション メソッドが呼び出された後、コードを実行できます。 アクションに渡された引数とアクションから返される結果の操作に使用できます。

* [例外フィルター](#exception-filters)ものが、応答の本体に書き込まれる前に発生する未処理の例外へのグローバル ポリシーを適用するために使用します。

* [結果フィルター](#result-filters)直前と直後に個々 のアクション結果の実行、コードを実行できます。 アクション メソッドが正常に実行された場合にのみ実行されビューまたはフォーマッタの実行を囲む必要がありますロジックに便利です。

次の図は、これらの種類のフィルターがフィルター パイプラインでどのように対話する方法を示します。

![要求は、承認フィルター、リソースのフィルター、モデル バインド、アクション フィルター、アクションの実行とアクション結果の変換、例外フィルター、結果のフィルター、およびを介して結果実行処理されます。 途中ではのみによって要求が処理結果のフィルターとリソースのフィルターをクライアントに送信された応答になる前にします。](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>実装

フィルターには、別のインターフェイス定義を介して同期および非同期の両方の実装がサポートされます。 実行する必要があるタスクの種類に応じて、同期または非同期のバリエーションを選択します。 

同期フィルターを実行できるコードの両方の前後に、パイプラインのステージを定義*ステージ*実行し、*ステージ*メソッドを実行します。 たとえば、`OnActionExecuting`アクション メソッドが呼び出される前に呼び出されますがおよび`OnActionExecuted`は、アクション メソッドが返された後に呼び出されます。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

非同期のフィルターに 1 つを定義する*ステージ*ExecutionAsync メソッドです。 このメソッドは、 *FilterType*ExecutionDelegate デリゲートが、フィルターのパイプラインのステージを実行します。 たとえば、`ActionExecutionDelegate`呼び出しアクション メソッドを実行してコードのメソッドを呼び出す前後にします。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

1 つのクラスで複数のフィルターのステージのインターフェイスを実装することができます。 たとえば、 [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute)抽象クラスを実装する`IActionFilter`と`IResultFilter`、だけでなく、非同期の同等です。

> [!NOTE]
> 実装**か**同期または非同期バージョンの両方ではなく、フィルターのインターフェイスです。 フレームワークは、フィルターは、非同期インターフェイスを実装しを呼び出す場合は、かどうかを最初に確認します。 それ以外の場合は、同期インターフェイスのメソッドを呼び出します。 1 つのクラスに両方のインターフェイスを実装する場合は、非同期のメソッドが呼び出されます。 同様に、抽象クラスを使用するときに[ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute)のみ同期メソッドまたは async メソッドの各フィルターの種類には、オーバーライドします。

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` は、`IFilter` を実装します。 したがって、`IFilterFactory`としてインスタンスを使用することができます、`IFilter`フィルター パイプライン内の任意の場所のインスタンス。 フレームワークは、フィルターを呼び出します準備しようとしてキャスト、`IFilterFactory`です。 そのキャストが成功した場合、`CreateInstance`を作成するメソッドが呼び出された、`IFilter`呼び出されるインスタンス。 これにより、正確なフィルター パイプラインがアプリケーションの起動時に明示的に設定する必要がないために、非常に柔軟なデザインが提供されます。

実装できます`IFilterFactory`フィルターを作成するための別の方法として、独自の属性の実装に。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>組み込みのフィルター属性

フレームワークには、サブクラスできる組み込みの属性に基づくフィルターが含まれます。 およびカスタマイズします。 たとえば、次の結果のフィルターは、応答に、ヘッダーを追加します。

<a name=add-header-attribute></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

属性は、上記の例のように、引数を受け入れるようにフィルターを許可します。 コント ローラーまたはアクション メソッドにこの属性を追加し、HTTP ヘッダーの値と名前を指定はします。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

結果、`Index`アクションを次に示します: 応答ヘッダーが右下に表示されます。

![開発者ツールの Microsoft Edge 作成者 Steve Smith を含め、応答ヘッダーの表示@ardalis](filters/_static/add-header.png)

カスタムの実装のための基本クラスとして使用できる対応する属性を持つフィルター インターフェイスのいくつかとします。

属性をフィルター処理します。

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute`および`ServiceFilterAttribute`については説明[この記事で後述](#dependency-injection)です。

## <a name="filter-scopes-and-order-of-execution"></a>フィルターのスコープと実行の順序

フィルターは、3 つのいずれかのパイプラインに追加することができます*スコープ*です。 属性を使用して、特定のアクション メソッドまたはコント ローラー クラスにフィルターを追加できます。 グローバル (すべてのコント ローラーとアクション) フィルターを登録するに追加することや、`MvcOptions.Filters`内のコレクション、`ConfigureServices`メソッドで、`Startup`クラス。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>実行の既定の順序

パイプラインの特定のステージの複数のフィルターがある場合は、スコープはフィルターの実行の既定の順序を決定します。  グローバル フィルターは、さらにメソッドのフィルターを囲むクラスのフィルターを囲みます。 これとも呼ば「ロシア人形」入れ子のように前のスコープを囲むスコープ増加するたびに、[入れ子人形](https://en.wikipedia.org/wiki/Matryoshka_doll)です。 通常、順序を明示的に判別することがなく、目的のオーバーライド動作を取得します。

この入れ子の結果として、*後*フィルターのコードは逆の順序で実行される、*する前に*コード。 シーケンスは、次のようになります。

* *する前に*グローバルに適用されるフィルターのコード
  * *する前に*コント ローラーに適用されるフィルターのコード
    * *する前に*アクション メソッドに適用されるフィルターのコード
    * *後*アクション メソッドに適用されるフィルターのコード
  * *後*コント ローラーに適用されるフィルターのコード
* *後*グローバルに適用されるフィルターのコード
  
同期のアクション フィルターのフィルターのメソッドが呼び出される順序を示す例を次に示します。

| シーケンス | フィルターのスコープ | Filter メソッド |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | コントローラー | `OnActionExecuting` |
| 3 | メソッド | `OnActionExecuting` |
| 4 | メソッド | `OnActionExecuted` |
| 5 | コントローラー | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

このシーケンスは、メソッドのフィルターが、コント ローラー フィルター内に入れ子にし、コント ローラーのフィルターがグローバル フィルター内で入れ子になったことを示します。 配置する別、場合は、非同期フィルター内の内部*ステージ*ExecutionAsync メソッド、すべて厳密なスコープを持つフィルターの実行がスタック上に、コード中にします。

> [!NOTE]
> 継承するすべてのコント ローラー、`Controller`基底クラスが含まれています`OnActionExecuting`と`OnActionExecuted`メソッドです。 これらのメソッドは、特定のアクションに対して実行されたフィルターをラップ: `OnActionExecuting` 、フィルターの前に呼び出されますと`OnActionExecuted`すべてのフィルターの後に呼び出されます。

### <a name="overriding-the-default-order"></a>既定の順序をオーバーライドします。

実行の既定の順序を上書きするには、実装して`IOrderedFilter`です。 このインターフェイスを公開、`Order`プロパティ スコープの実行の順序を決定するよりも優先です。 フィルターが低い`Order`値になりますその*する前に*より高い値を持つフィルターの前に実行されるコード`Order`です。 フィルターが低い`Order`値になります、*後*、高いでフィルターの後に実行されるコード`Order`値。 設定することができます、`Order`コンス トラクターのパラメーターを使用してプロパティ。

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

上記の例がセットに示すように 3 つのアクション フィルターが同じ、`Order`コント ローラーとグローバル プロパティがそれぞれ 1 および 2 をフィルター処理、実行の順序は逆にします。

| シーケンス | フィルターのスコープ | `Order` プロパティ | Filter メソッド |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | メソッド | 0 | `OnActionExecuting` |
| 2 | コントローラー | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | コントローラー | 1  | `OnActionExecuted` |
| 6 | メソッド | 0  | `OnActionExecuted` |

`Order`プロパティは、フィルターが実行される順序を決定するときにスコープを登場します。 フィルターは順で最初に並べ替えとの結び付きを解除するスコープを使用します。 すべての組み込みフィルターを実装`IOrderedFilter`既定の設定と`Order`値を 0、スコープが設定していない場合、順序を決定するために`Order`0 以外の値にします。

## <a name="cancellation-and-short-circuiting"></a>取り消し処理および短いサーキット

任意の時点で、フィルター パイプラインをショート サーキットを設定してできます、`Result`プロパティを`context`フィルター メソッドに渡されたパラメーター。 たとえば、次のリソースのフィルターを防ぎますパイプラインの残りの部分を実行します。

<a name=short-circuiting-resource-filter></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

次のコードでは、両方の`ShortCircuitingResourceFilter`と`AddHeader`フィルター対象、`SomeResource`アクション メソッド。 ただし、ため、`ShortCircuitingResourceFilter`最初に実行 (リソースをフィルターになっているためと`AddHeader`アクション フィルターは、)、パイプラインの残りの部分を short-circuits および、`AddHeader`のフィルターが実行されない、`SomeResource`アクション。 この動作になります、同じフィルターの両方が提供される、アクション メソッド レベルで適用された場合、`ShortCircuitingResourceFilter`最初の実行 (のためのフィルターの種類または明示的な使用`Order`プロパティのインスタンス)。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>依存関係の挿入

タイプまたはインスタンスによって、フィルターを追加できます。 インスタンスを追加する場合、そのインスタンスはすべての要求に対して使用されます。 種類を追加する場合は、型でアクティブ化される、つまり、要求ごとにインスタンスが作成されコンス トラクターの依存関係が設定されますをそのされます[依存性の注入](../../fundamentals/dependency-injection.md)(DI)。 型でフィルターを追加するは等価`filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`です。

属性として実装され、コント ローラー クラスまたはアクション メソッドに直接追加するフィルターによって提供されるコンス トラクターの依存関係を持つことはできません[依存性の注入](../../fundamentals/dependency-injection.md)(DI)。 これは、属性には、適用することを指定したコンス トラクターのパラメーターが必要なためです。 これは、属性の機能についての制限です。

フィルターには、DI からアクセスする必要のある依存関係がある、サポートされているいくつかの方法があります。 次のいずれかを使用して、クラスまたはアクション メソッドに、フィルターを適用できます。

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory`属性の実装

> [!NOTE]
> DI から取得する 1 つの依存関係は、ロガーです。 ただし、回避を作成して、ログ記録の目的で、単にフィルターを使用すると、以降、[組み込みフレームワークのログ記録機能](../../fundamentals/logging.md)必要なものを既に指定可能性があります。 フィルターにログ記録を追加する場合は、ビジネス ドメインの懸案事項や、フィルターではなく MVC アクションまたはその他のフレームワーク イベントに固有の動作を重視必要があります。

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

A `ServiceFilter` DI からフィルターのインスタンスを取得します。 コンテナーにフィルターを追加する`ConfigureServices`でその参照、`ServiceFilter`属性

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

使用して`ServiceFilter`例外フィルターの種類の結果を登録せず。

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute`実装する`IFilterFactory`、作成するための 1 つのメソッドを公開する、`IFilter`インスタンス。 場合、 `ServiceFilterAttribute`、`IFilterFactory`インターフェイスの`CreateInstance`をサービス コンテナー (DI) から、指定した型を読み込むメソッドを実装します。

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute`よく似ています`ServiceFilterAttribute`(も実装`IFilterFactory`)、その型は、DI コンテナーから直接解決されませんが、します。 型をインスタンス化を使用して、代わりに、`Microsoft.Extensions.DependencyInjection.ObjectFactory`です。

この違いにより、型を使用して参照されている、`TypeFilterAttribute`最初、コンテナーに登録する必要はありません (ただし、コンテナーによって実行される依存関係があります)。 また、`TypeFilterAttribute`対象の型のコンス トラクター引数を受け入れる必要に応じてことができます。 次の例を使用して、型引数を渡す方法を示します`TypeFilterAttribute`:

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

かどうか、引数を必要としないフィルターがある DI によって設定される必要があるコンス トラクターの依存関係があり、使用することが、独自の名前付き属性クラスとメソッドの代わりに`[TypeFilter(typeof(FilterType))]`)。 次のフィルターは、これを実装する方法を示しています。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

クラスまたはメソッドを使用してこのフィルターを適用することができます、`[SampleActionFilter]`を使用することではなく、構文`[TypeFilter]`または`[ServiceFilter]`です。

## <a name="authorization-filters"></a>承認フィルター

*承認フィルター*フィルター パイプライン内で実行される最初のフィルターは、アクション メソッドへのアクセスを制御します。 のみがある前に、と後のメソッドをサポートするほとんどのフィルターとは異なり、メソッドの前にします。 必要がありますだけ記述するカスタム承認フィルターを記述するかどうかは、独自の承認フレームワーク。 承認ポリシーの構成またはカスタム フィルターの記述に、カスタム承認ポリシーを上書きしてもかまいません。 組み込みのフィルターの実装はだけ認証システムを呼び出すことをします。

例外の処理は何も行われませんので承認フィルター内の例外をスローする必要があります (例外フィルターされませんそれらを処理)。 代わりに、チャレンジを発行するか、別の方法します。

詳細については[承認](../../security/authorization/index.md)です。

## <a name="resource-filters"></a>リソースのフィルター

*リソース フィルター*実装するか、`IResourceFilter`または`IAsyncResourceFilter`インターフェイス、およびそれらの実行は、フィルター パイプラインのほとんどをラップします。 (だけ[承認フィルター](#authorization-filters)前に実行します)。リソースのフィルターは、ショート サーキットのほとんどの要求を行って作業をする必要がある場合に特に便利です。 たとえば、キャッシュのフィルターは、応答がキャッシュ内に既に場合、パイプラインの残りの部分を回避できます。

[短いサーキット リソース フィルター](#short-circuiting-resource-filter)リソース フィルターの一例は、前に説明します。 別の例は、 [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs)、モデル バインディング フォーム データへのアクセスを防ぐことができます。 しようとしている大きなファイルのアップロードを受信し、フォームがメモリに読み込まれるようにするのにわかっている場合に便利です。

## <a name="action-filters"></a>アクション フィルター

*アクション フィルター*実装するか、`IActionFilter`または`IAsyncActionFilter`インターフェイス、およびそれらの実行アクション メソッドの実行で囲まれます。

サンプルのアクション フィルターを次に示します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext)次のプロパティを提供します。

* `ActionArguments`-アクションへの入力を操作することができます。
* `Controller`-コント ローラー インスタンスを操作することができます。 
* `Result`-アクション メソッドと後続のアクション フィルターの実行を short-circuits これを設定します。 例外がスローされることもできなくなります、後続のフィルター、アクション メソッドの実行が成功した結果ではなくエラーとして扱われます。

[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext)提供`Controller`と`Result`さらに、次のプロパティ。

* `Canceled`-アクションの実行がショート サーキットで別のフィルターの場合は true になります。
* `Exception`-アクションまたは後続のアクション フィルターは、例外をスローした場合、null になります。 効果的に null には、このプロパティの設定 'handles'、例外、および`Result`は通常、アクション メソッドから返されたかのように実行されます。

`IAsyncActionFilter`への呼び出し、`ActionExecutionDelegate`の後続のアクション フィルターとを返すアクション メソッドの実行、`ActionExecutedContext`です。 ショート サーキット、割り当て、`ActionExecutingContext.Result`なんらかの結果をインスタンス化し、呼び出すことはありません、`ActionExecutionDelegate`です。

フレームワークは、抽象`ActionFilterAttribute`サブクラス化できます。 

アクション フィルターを使用して、自動的にモデルの状態を検証し、状態が有効でない場合、エラーが発生することができます。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` 、アクション メソッドはからメソッドが実行されるを参照してくださいおよびにより、アクションの結果を操作する、`ActionExecutedContext.Result`プロパティです。 `ActionExecutedContext.Canceled`アクションの実行がショート サーキットで別のフィルターの場合は true に設定されます。 `ActionExecutedContext.Exception`アクションまたは後続のアクション フィルターは、例外をスローした場合、null 以外の値に設定されます。 設定`ActionExecutedContext.Exception`を効果的に null に ''、例外を処理、および`ActionExectedContext.Result`通常アクション メソッドから返されたかのように実行されます。

## <a name="exception-filters"></a>例外フィルター

*例外フィルター*実装するか、`IExceptionFilter`または`IAsyncExceptionFilter`インターフェイスです。 実装する一般的なエラーが、アプリのポリシーを処理に使用できます。 

次のサンプル例外フィルターは、カスタム開発エラー ビューを使用して、アプリケーションは開発中に発生する例外に関する詳細を表示します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

例外フィルターがない 2 つのイベント (の前に、と後に) - のみを実装する`OnException`(または`OnExceptionAsync`)。 

例外フィルターは、コント ローラーの作成で発生する未処理の例外を処理[モデル バインディング](../models/model-binding.md)、アクション フィルター、またはアクション メソッド。 リソース フィルター、結果のフィルターまたは MVC 結果実行で発生する例外をキャッチするされません。

例外を処理するには、設定、`ExceptionContext.ExceptionHandled`プロパティを true または応答を書き込みます。 これは、例外の伝達を停止します。 例外フィルターが「成功」に例外を有効にできないことに注意してください。 アクション フィルターのみ行うことができます。

> [!NOTE]
> ASP.NET 1.1 に設定した場合、応答は送信されません`ExceptionHandled`を true に**と**応答を書き込みます。 シナリオでは、ASP.NET Core 1.0 は応答の送信、および ASP.NET Core 1.1.2 は 1.0 の動作に戻ります。 詳細については、次を参照してください。[発行 #5594](https://github.com/aspnet/Mvc/issues/5594) GitHub リポジトリにします。 

例外フィルターは、MVC アクション内で発生する例外をトラップするのに便利ですが取り込まエラー ミドルウェアを処理する柔軟性がありません。 [全般] の場合、ミドルウェアを優先し、エラー処理のためにのみ必要があるフィルターを使用して*異なる*する MVC アクションの選択に基づいて。 たとえば、アプリには、両方の API エンドポイントとのビューと HTML のアクション メソッドがあります。 ビュー ベースのアクションが html エラー ページを返すときに、API エンドポイントは、JSON としてのエラー情報を返す可能性があります。

フレームワークは、抽象`ExceptionFilterAttribute`サブクラス化できます。 

## <a name="result-filters"></a>結果フィルター

*結果フィルター*どちらかを実装、`IResultFilter`または`IAsyncResultFilter`インターフェイス、およびそれらの実行アクションの結果の実行で囲まれます。 

HTTP ヘッダーを追加する結果フィルターの例を次に示します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

実行されている結果の種類は、対象のアクションによって異なります。 処理の一部としてすべての razor にはビューを返す MVC アクションが含まれます、`ViewResult`実行されています。 API メソッドは、結果の実行の一部としていくつかのシリアル化を実行できます。 詳細については[アクションの結果](actions.md)

結果フィルターは、アクションまたはアクション フィルターは、アクションの結果を生成するときにのみ成功した結果を得るには、実行されます。 例外フィルター、例外を処理するときに、結果のフィルターは実行されません。

`OnResultExecuting`メソッドは、アクションの結果および以降の結果のフィルターの実行をショート サーキットを設定して`ResultExecutingContext.Cancel`true に設定します。 ショート サーキットの空の応答が生成されないようにする場合は、一般に、応答オブジェクトを記述してください。 例外をスロー、アクションの結果と後続のフィルターの実行を防ぐもが、正常な結果ではなくエラーとして扱われます。

ときに、`OnResultExecuted`メソッドを実行する応答がクライアントに送信された可能性があり、(例外がスローされました) しない限り、さらに変更することはできません。 `ResultExecutedContext.Canceled`アクションの結果実行されたショート サーキットで別のフィルターの場合は true に設定されます。

`ResultExecutedContext.Exception`アクションの結果または以降の結果のフィルターは、例外をスローした場合、null 以外の値に設定されます。 設定`Exception`に null を効率的に例外を処理およびパイプラインの後続の MVC によって再スローされてからの例外を防止します。 結果フィルター内の例外を処理している、ときに、応答にすべてのデータを書き込むことができません。 アクションの結果をその実行を途中でスロー ヘッダーが既にクライアントにフラッシュされた場合は、信頼性の高いメカニズムはエラー コードを送信することはありません。

`IAsyncResultFilter`への呼び出し`await next()`上、`ResultExecutionDelegate`以降の結果フィルターとアクションの結果を実行します。 ショート サーキットの設定、`ResultExecutingContext.Cancel`に true を設定して、呼び出すことはありません、`ResultExectionDelegate`です。

フレームワークは、抽象`ResultFilterAttribute`サブクラス化できます。 [AddHeaderAttribute](#add-header-attribute)前に示したクラスは、結果のフィルター属性の例を示します。

## <a name="using-middleware-in-the-filter-pipeline"></a>フィルター パイプラインにミドルウェアを使用します。

リソース フィルターの動作と同様に[ミドルウェア](../../fundamentals/middleware.md)を後で、パイプラインに付属しているすべての実行に囲まれています。 フィルターとは異なり、ミドルウェアから MVC、MVC コンテキストおよび構成体へのアクセスがあることを意味の一部であります。

ASP.NET Core 1.1 では、フィルター パイプラインにミドルウェアを使用できます。 MVC ルート データ、またはいずれかのコント ローラーまたはアクションを実行する必要がありますへのアクセスが必要なミドルウェア コンポーネントがある場合は、する可能性があります。

フィルターとしてミドルウェアを使用するを持つ型を作成、`Configure`フィルター パイプラインに挿入するミドルウェアを指定するメソッド。 要求の現在のカルチャを確立するために、ローカリゼーション ミドルウェアを使用する例を次に示します。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

使用してできます、`MiddlewareFilterAttribute`選択したコント ローラーまたはアクションのミドルウェアを実行する、またはグローバルに。

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

ミドルウェアのフィルターが、パイプラインの残りの部分の前後モデル バインディングの前にリソースをフィルターとして、フィルター パイプラインの同じ段階で実行します。

## <a name="next-actions"></a>次の操作

フィルターを使用する実験[ダウンロード、テストし、サンプルを変更して](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)です。
