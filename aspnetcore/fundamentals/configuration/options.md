---
title: "ASP.NET Core のオプションのパターン"
author: guardrex
description: "ASP.NET Core アプリケーションに関連する設定のグループを表し、オプションのパターンを使用する方法を検出します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: aab96b5313a8632950e51f5586612c1d0d3d176e
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2018
---
# <a name="options-pattern-in-aspnet-core"></a>ASP.NET Core のオプションのパターン

作成者: [Luke Latham](https://github.com/guardrex)

オプション パターンではオプション クラスを使用して、関連する設定のグループを表します。 構成設定については、個別のオプション クラスに分離機能によって、アプリは 2 つの重要なソフトウェア エンジニア リング原則に従って。

* [インターフェイスの分離の原則 (ISP)](http://deviq.com/interface-segregation-principle/): 構成の設定に依存する機能 (クラス) が使用する構成設定のみに依存します。
* [関心の分離](http://deviq.com/separation-of-concerns/): アプリのさまざまな部分の設定は、依存または相互に結合がありません。

[表示またはダウンロードするサンプル コード](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) この記事では、次のサンプル アプリを使用しやすくします。

## <a name="basic-options-configuration"></a>基本的なオプションの構成

基本的なオプション構成については、例として説明&num;では 1、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。

オプション クラスは非抽象である必要がありますパブリック パラメーターなしのコンス トラクターを使用します。 次のクラスでは、 `MyOptions`、2 つのプロパティを持つ`Option1`と`Option2`です。 既定値の設定オプションですが、次の例では、クラス コンス トラクターの既定値を設定する`Option1`です。 `Option2`既定値が直接にプロパティを初期化することによって設定されて (*Models/MyOptions.cs*)。

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions`クラスをサービス コンテナーに追加[IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)構成にバインドされているとします。

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

次は、モデルの使用をページ[コンス トラクターの依存性の注入](xref:fundamentals/dependency-injection#what-is-dependency-injection)で[IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1)設定にアクセスする (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

サンプルの*される appsettings.json*ファイルの値を指定する`option1`と`option2`:

[!code-json[Main](options/sample/appsettings.json)]

実行時に、アプリは、ページのモデルの`OnGet`メソッド オプション クラス値を示す文字列を返します。

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>デリゲートで単純なオプションを構成します。

例として、デリゲートを使用して簡単なオプションの構成が示されている&num;2 に、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。

デリゲートを使用すると、オプションの値を設定できます。 サンプル アプリは、`MyOptionsWithDelegateConfig`クラス (*Models/MyOptionsWithDelegateConfig.cs*)。

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

次のコードでは、1 秒あたり`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。 使用してバインディングを構成するデリゲートを使用して、 `MyOptionsWithDelegateConfig`:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

複数の構成プロバイダーを追加することができます。 NuGet パッケージの構成プロバイダーを利用できます。 これらは、登録されていることを適用しています。

各呼び出し[構成&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure)を追加、`IConfigureOptions<TOptions>`サービスをサービス コンテナーにします。 前の例では、値で`Option1`と`Option2`で指定された両方*される appsettings.json*の値が、`Option1`と`Option2`構成済みのデリゲートによって上書きされます。

最後の構成ソースが指定された 1 つ以上の構成サービスを有効にすると、 *wins*と構成値を設定します。 実行時に、アプリは、ページのモデルの`OnGet`メソッド オプション クラス値を示す文字列を返します。

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>サブオプション構成

サブオプション構成が例として示されている&num;3 に、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。

アプリでは、アプリ内の特定の機能グループ (クラス) に関連するオプション クラスを作成する必要があります。 構成値を必要とするアプリの一部のみを使用する構成値へのアクセスが必要です。

フォームの構成キー オプションの種類の各プロパティがバインドされているバインドするときのオプションの構成に、`property[:sub-property:]`です。 たとえば、`MyOptions.Option1`プロパティがキーにバインドされる`Option1`からこれは読み取り、`option1`プロパティ*される appsettings.json*です。

次のコードでは、3 つ目の`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。 バインドされている`MySubOptions`セクションに`subsection`の*される appsettings.json*ファイル。

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection`拡張メソッドが必要です、 [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet パッケージです。 アプリで使用する場合、 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage、パッケージが自動的に含まれます。

サンプルの*される appsettings.json*ファイルを定義、`subsection`のキーを持つメンバー`suboption1`と`suboption2`:

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions`クラス、プロパティを定義`SubOption1`と`SubOption2`サブ オプションの値を保持するために、(*Models/MySubOptions.cs*)。

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

ページのモデルの`OnGet`メソッド サブ オプション値を使用して文字列を返します (*Pages/Index.cshtml.cs*)。

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

アプリを実行すると、`OnGet`メソッド クラス値を表示するには、サブ オプション文字列を返します。

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>直接ビュー インジェクションまたはビューのモデルによって提供されるオプション

直接ビュー インジェクションまたはビューのモデルによって提供されるオプションは、例として説明&num;4 で、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。

ビュー モデルまたは挿入することで、オプションを指定することができます`IOptions<TOptions>`ビューに直接 (*Pages/Index.cshtml.cs*)。

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

直接挿入には、挿入`IOptions<MyOptions>`で、`@inject`ディレクティブ。

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

アプリを実行すると、レンダリングされるページのオプションの値のとおりです。

![オプション値オプション 1: value1_from_json と・ オプション 2:-1 は、ビューに挿入して、モデルから読み込まれます。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>IOptionsSnapshot で構成データを再読み込み

構成データを再読み込みする`IOptionsSnapshot`の例に示す&num;に 5、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。

*ASP.NET Core 1.1 以降が必要です。*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1)最小限処理のオーバーヘッドを使用してオプションを再読み込みをサポートしています。 ASP.NET Core 1.1 で`IOptionsSnapshot`のスナップショットは、 [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)され、更新プログラムに自動的にするたびに、モニターがトリガー データ ソースの変更に基づいて変更します。 ASP.NET Core 2.0 以降では、1 回の要求アクセスされ、要求の有効期間中にキャッシュされる場合にオプションが 1 回計算されます。

次の例は、新しい方法について説明します。`IOptionsSnapshot`後に作成された*される appsettings.json*変更 (*Pages/Index.cshtml.cs*)。 サーバーに複数の要求がによって提供される定数の値を返す、*される appsettings.json*ファイルのファイルが変更され、構成を再読み込みされるまでです。

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

次の図は、初期`option1`と`option2`から読み込まれた値、*される appsettings.json*ファイル。

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

値を変更、*される appsettings.json*ファイルの名前を`value1_from_json UPDATED`と`200`です。 保存、*される appsettings.json*ファイル。 ブラウザーを更新するオプションの値が更新されるを参照してください。

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>名前付き IConfigureNamedOptions とオプションのサポート

名前付きのオプション サポート[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1)例として説明されて&num;6 インチ、[サンプル アプリ](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)です。

*ASP.NET Core 2.0 以降が必要です。*

*オプションを名前付き*サポートが名前付きのオプションの構成の間で区別するためにアプリを許可します。 サンプル アプリでは名前付きのオプションで宣言されて、 [ConfigureNamedOptions&lt;TOptions&gt;です。構成](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure)メソッド。

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

サンプル アプリを使用する名前付きのオプションにアクセスする[IOptionsSnapshot&lt;TOptions&gt;です。取得](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get)(*Pages/Index.cshtml.cs*)。

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

サンプル アプリを実行するには、名前付きのオプションが返されます。

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1`値がから提供される構成から読み込まれますが、*される appsettings.json*ファイル。 `named_options_2`値は、によってを提供されます。

* `named_options_2`で委任`ConfigureServices`の`Option1`します。
* 既定値`Option2`によって提供される、`MyOptions`クラスです。

名前付きのオプションのすべてのインスタンスを構成する、 [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)メソッドです。 次のコードは構成`Option1`すべてに共通の値を持つ構成インスタンスという名前です。 次のコードに手動で追加、`Configure`メソッド。

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

コードを追加した後、サンプル アプリを実行するには、次の結果が生成されます。

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> ASP.NET Core 2.0 以降では、すべてのオプションは、名前付きインスタンス。 既存の`IConfigureOption`インスタンスはターゲットとして扱われます、 `Options.DefaultName` 、インスタンス`string.Empty`です。 `IConfigureNamedOptions`実装も`IConfigureOptions`します。 既定の実装、 [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([参照ソース](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) 各を適切に使用するロジックを備えています。 `null`名前付きのオプションを使用して、特定の名前付きインスタンスではなく名前付きインスタンスのすべての対象 ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)と[PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)この規則を使用)。

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*ASP.NET Core 2.0 以降が必要です。*

設定と postconfiguration [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1)です。 Postconfiguration が実行される結局[IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)構成が行われます。

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure)は名前付きのオプションの構成後に使用できます。

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

使用して[PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)名前付きインスタンスの構成の後にすべてを構成します。

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>オプションのファクトリ、監視、およびキャッシュ

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)通知を使用するときに`TOptions`インスタンスを変更します。 `IOptionsMonitor`再読み込みオプションをサポートする、変更通知、および`IPostConfigureOptions`です。

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1)は新規に作成するオプションのインスタンスに (ASP.NET Core 2.0 またはそれ以降) があります。 1 つが[作成](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)メソッドです。 既定の実装は、すべての登録済み`IConfigureOptions`と`IPostConfigureOptions`すべて実行し、最初に構成、その後で、後の構成します。 間で区別するため`IConfigureNamedOptions`と`IConfigureOptions`のみ適切なインターフェイスを呼び出します。

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances. `IOptionsMonitorCache`値が再計算されるようにモニターでオプションのインスタンスを無効になります ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。 値が手動でに導入されたも[TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd)です。 [クリア](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear)要求時に、すべての名前付きインスタンスを再作成する必要がありますとメソッドを使用します。

## <a name="accessing-options-during-startup"></a>スタートアップ時のオプションにアクセスします。

`IOptions`使用できる`Configure`する前にサービスが組み込まれているため、`Configure`メソッドを実行します。 サービス プロバイダーが組み込まれている場合`ConfigureServices`オプションにアクセスすることはありませんが含まれてオプションの構成をサービス プロバイダーが構築された後に入力します。 そのため、オプションが一致しない状態は、サービス登録の順序により存在している可能性があります。

オプションは、通常、構成から読み込まれた、ために、両方のスタートアップの構成を使用できます`Configure`と`ConfigureServices`です。 起動中に構成を使用する例については、次を参照してください。、[アプリケーションの起動](xref:fundamentals/startup)トピックです。

## <a name="see-also"></a>関連項目

* [構成](xref:fundamentals/configuration/index)
