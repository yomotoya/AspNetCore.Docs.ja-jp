---
title: ASP.NET Core のオプション パターン
author: guardrex
description: ASP.NET Core アプリの関連のある設定のグループを表すオプション パターンを使用する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: 96d7d2956fa9bf72706cde0532ee7f4ff753b72c
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126262"
---
# <a name="options-pattern-in-aspnet-core"></a>ASP.NET Core のオプション パターン

作成者: [Luke Latham](https://github.com/guardrex)

オプション パターンではクラスを使用して、関連する設定のグループを表します。 構成設定が機能別に個々のクラスに分離されるとき、アプリは次の 2 つの重要なソフトウェア エンジニアリング原則に従います。

* [ISP (Interface Segregation Principle/インターフェイス分離の原則)](http://deviq.com/interface-segregation-principle/): それが使用する構成設定にのみ依存する機能 (クラス)。
* [懸念事項の分離 (Separation of Concerns)](http://deviq.com/separation-of-concerns/): アプリのさまざまな部分の設定が互いに非依存。

[サンプル コードをご覧ください。ダウンロードも可能です](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample)) この記事はサンプル アプリを用意すると進めやすくなります。

## <a name="basic-options-configuration"></a>基本的なオプション構成

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;1 は基本的なオプション構成です。

オプション クラスはパラメーターのないパブリック コンストラクターを持った非抽象でなければなりません。 次のクラス `MyOptions` には `Option1` と `Option2` という 2 つのプロパティがあります。 既定値の設定は任意ですが、次の例のクラス コンストラクターは既定値 `Option1` を設定します。 `Option2` には、プロパティを直接初期化することで既定値が設定されます (*Models/MyOptions.cs*)。

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` クラスは [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) でサービス コンテナーに追加され、次の構成にバインドされます。

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

次のページ モデルは[コンストラクターの依存関係挿入](xref:fundamentals/dependency-injection#what-is-dependency-injection)と [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) を利用し、設定にアクセスします (*Pages/Index.cshtml.cs*)。

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

サンプルの *appsettings.json* ファイルは `option1` と `option2` の値を指定します。

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

アプリの実行時、ページ モデルの `OnGet` メソッドは文字列を返し、オプション クラス値を表示します。

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> カスタム [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) を使用して設定ファイルからオプションの構成を読み込むときには、基本パスが正しく設定されていることを確認します。
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を使用して、設定ファイルからオプションの構成を読み込むときには、基本パスを明示的に設定する必要はありません。

## <a name="configure-simple-options-with-a-delegate"></a>デリゲートで単純なオプションを構成する

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;2 では、デリゲートで単純なオプションを構成します。

デリゲートを使用し、オプション値を設定します。 サンプル アプリでは、`MyOptionsWithDelegateConfig` クラス (*Models/MyOptionsWithDelegateConfig.cs*) を使用しています。

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

次のコードでは、2 番目の `IConfigureOptions<TOptions>` サービスがサービス コンテナーに追加されます。 デリゲートを利用し、`MyOptionsWithDelegateConfig` とのバインディングを構成します。

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

複数の構成プロバイダーを追加できます。 構成プロバイダーは NuGet パッケージで利用できます。 登録順序で適用されます。

[Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) を呼び出すたびに `IConfigureOptions<TOptions>` サービスがサービス コンテナーに追加されます。 先の例では、値 `Option1` と `Option2` が両方とも *appsettings.json* で指定されていますが、構成されているデリゲートにより値 `Option1` と `Option2` がオーバーライドされます。

複数の構成サービスが有効になっているとき、最後に指定された構成ソースが*優先*され、それにより構成値が設定されます。 アプリの実行時、ページ モデルの `OnGet` メソッドは文字列を返し、オプション クラス値を表示します。

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>サブオプション構成

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;3 はサブオプション構成です。

アプリでは、アプリの特定の機能グループ (クラス) に関連するオプション クラスを作成する必要があります。 構成値を必要とするアプリの各パーツには、そのパーツが使用する構成値へのアクセスのみを与える必要があります。

オプションを構成にバインドするとき、オプション タイプの各プロパティはフォーム `property[:sub-property:]` の構成キーにバインドされます。 たとえば、`MyOptions.Option1` プロパティはキー `Option1` にバインドされます。このキーは *appsettings.json* の `option1` プロパティから読み込まれます。

次のコードでは、3 番目の `IConfigureOptions<TOptions>` サービスがサービス コンテナーに追加されます。 `MySubOptions` を *appsettings.json* ファイルのセクション `subsection` にバインドします。

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

拡張メソッド `GetSection` は、[Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet パッケージを必要とします。 アプリで [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 以降) を使用している場合、このパッケージは自動的に含まれます。

サンプルの *appsettings.json* ファイルは、`suboption1` と `suboption2` のキーで `subsection` メンバーを定義します。

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions` クラスは、`SubOption1` プロパティと `SubOption2` プロパティを定義し、サブオプション値を保持します (*Models/MySubOptions.cs*)。

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

ページ モデルの `OnGet` メソッドは、文字列とサブオプション値を返します (*Pages/Index.cshtml.cs*)。

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

アプリの実行時、`OnGet` メソッドは文字列を返し、サブオプション クラス値を表示します。

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>ビュー モデルまたは直接的なビュー挿入で与えられるオプション

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;4 は、ビュー モデルまたは直接的なビュー挿入で与えられるオプションです。

オプションはビュー モデルで提供するか、ビューに `IOptions<TOptions>` を直接挿入することで提供できます (*Pages/Index.cshtml.cs*)。

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

直接挿入の場合、`@inject` ディレクティブで `IOptions<MyOptions>` を挿入します。

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

アプリの実行時、レンダリングされたページにオプション値が表示されます。

![オプション値の Option1: value1_from_json と Option2: -1 はモデルから読み込まれるか、ビューへの挿入によって読み込まれます。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>IOptionsSnapshot で構成データを再読み込みする

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;5 では、`IOptionsSnapshot` で構成データを再読み込みする方法を確認できます。

*ASP.NET Core 1.1 以降が必要です。*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) では、最小の処理オーバーヘッドでオプションを再読み込みできます。 ASP.NET Core 1.1 では、`IOptionsSnapshot` は [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) のスナップショットであり、データ ソースの変更に基づいてモニターが変更をトリガーするたびに自動的に更新されます。 ASP.NET Core 2.0 以降では、オプションは、要求の有効期間中にアクセスされ、キャッシュされたとき、要求につき 1 回計算されます。

次の例では、*appsettings.json* の変更後、新しい `IOptionsSnapshot` が作成されます (*Pages/Index.cshtml.cs*)。 サーバーに複数の要求が届くと、ファイルが変更され、構成が再読み込みされるまで、*appsettings.json* ファイルによって提供される定数値が返されます。

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

次のイメージでは、初期値の `option1` と `option2` が *appsettings.json* ファイルから読み込まれます。

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

*appsettings.json* ファイルの値を `value1_from_json UPDATED` と `200` に変更します。 *appsettings.json* ファイルを保存します。 ブラウザーを更新し、オプション値が更新されていることを確認します。

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>IConfigureNamedOptions による名前付きオプションのサポート

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)の例 &num;6 は、[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) による名前付きオプションのサポートです。

*ASP.NET Core 2.0 以降が必要です。*

*名前付きオプション*をサポートすることで、アプリでは名前付きオプション構成が区別されます。 サンプル アプリでは、名前付きオプションは [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) で宣言されます。このオプションは、拡張メソッド [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) を呼び出します。

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

サンプル アプリは [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) で名前付きオプションにアクセスします (*Pages/Index.cshtml.cs*)。

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

サンプル アプリを実行すると、名前付きオプションが返されます。

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` 値が構成から与えられます。これは *appsettings.json* ファイルから読み込まれます。 `named_options_2` 値は次により提供されます。

* `Option1` の `ConfigureServices` の `named_options_2` デリゲート。
* `MyOptions` クラスによって提供される `Option2` の既定値。

名前付きオプション インスタンスはすべて、[OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) メソッドで構成します。 次のコードでは、すべての名前付き構成インスタンスの `Option1` が共通値で構成されます。 `Configure` メソッドに次のコードを手動で追加します。

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

コードを追加した後、サンプル アプリを実行すると、次の結果が生成されます。

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> ASP.NET Core 2.0 以降、すべてのオプションが名前付きインスタンスになります。 既存の `IConfigureOption` インスタンスは、`string.Empty` である、`Options.DefaultName` インスタンスを対象とするものとして処理されます。 `IConfigureNamedOptions` はまた、`IConfigureOptions` を実装します。 [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([参照元](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) の既定の実装には、それぞれを適切に使用するためのロジックが与えられます。 名前付きオプション `null` は、特定の名前付きオプションの代わりにすべての名前付きインスタンスを対象にするときに使用されます ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) と [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) ではこの規則が使用されます)。

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*ASP.NET Core 2.0 以降が必要です。*

[IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1) でポスト構成を設定します。 ポスト構成は、すべての [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) 構成の後に実行されます。

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) は、名前付きオプションのポスト構成に利用できます。

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

すべての名前付き構成インスタンスをポスト構成するには、[PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) を使用します。

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>オプション ファクトリ、監視、キャッシュ

`TOptions` インスタンスが変わるとき、[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) が通知に使用されます。 `IOptionsMonitor` は、再読み込み可能なオプション、変更通知、`IPostConfigureOptions` に対応しています。

[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 以降) は、新しいオプション インスタンスを作成します。 [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) メソッドが 1 つ含まれています。 既定の実装では、登録されている `IConfigureOptions` と `IPostConfigureOptions` がすべて受け取られ、先にすべての構成を実行し、その後、ポスト構成を実行します。 `IConfigureNamedOptions` と `IConfigureOptions` が区別され、適切なインターフェイスのみが呼び出されます。

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 以降) は `IOptionsMonitor` によって `TOptions` インスタンスのキャッシュに使用されます。 `IOptionsMonitorCache` は、値が再計算されるよう、モニターのオプション インスタンスを無効にします ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。 [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd) を利用し、手動で値を入力することもできます。 [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) メソッドは、すべての名前付きインスタンスをオンデマンドで再作成するときに使用されます。

## <a name="accessing-options-during-startup"></a>スタートアップ時にオプションにアクセスする

サービスは `Configure` メソッドの実行前に構築されるため、`IOptions` は `Configure` で使用できます。 オプションにアクセスするためにサービス プロバイダーが `ConfigureServices` に組み込まれている場合、サービス プロバイダーの構築後に与えられるオプション構成が含まれていないことがあります。 そのため、サービス登録の順序に起因し、オプションの状態に一貫性がないことがあります。

オプションは一般的に構成から読み込まれるため、`Configure` と `ConfigureServices` の両方の起動時に構成を利用できます。 起動中に構成を利用する方法については、「[アプリケーションの起動](xref:fundamentals/startup)」というトピックをご覧ください。

## <a name="see-also"></a>関連項目

* [構成](xref:fundamentals/configuration/index)
