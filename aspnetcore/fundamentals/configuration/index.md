---
title: ASP.NET Core の構成
author: rick-anderson
description: 構成 API を使用して、ASP.NET Core アプリを構成する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 70e9e73eeb5d08baf9ef190ebfbda998ace60d77
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278326"
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core の構成

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Mark Michaelis](http://intellitect.com/author/mark-michaelis/)、[Steve Smith](https://ardalis.com/)、[Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)

構成 API は、名前と値のペアのリストに基づく ASP.NET Core Web アプリの構成方法を提供します。 構成は実行時に複数のソースから読み取られます。 名前と値の組は、複数レベルの階層にグループ化することができます。

以下の構成プロバイダーがあります。

* ファイル形式 (INI、JSON、および XML)。
* コマンド ライン引数。
* 環境変数。
* メモリ内 .NET オブジェクト。
* 暗号化されていない[シークレット マネージャー](xref:security/app-secrets)の記憶域。
* [Azure Key Vault](xref:security/key-vault-configuration) などの暗号化されたユーザー ストア。
* カスタム プロバイダー (インストール済みまたは作成済み)。

各構成値は文字列キーにマップされます。 設定をカスタム [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) オブジェクト (プロパティを持つ単純な .NET クラス) に逆シリアル化するための組み込みのバインド サポートがあります。

オプション パターンではオプション クラスを使用して、関連する設定のグループを表します。 オプション パターンの使用の詳細については、「[オプション](xref:fundamentals/configuration/options)」トピックを参照してください。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="json-configuration"></a>JSON 構成

次のコンソール アプリでは JSON 構成プロバイダーを使用します。

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

アプリは以下の構成設定を読み取り、表示します。

[!code-json[](index/sample/ConfigJson/appsettings.json)]

構成は、コロン (`:`) で区切られたノードの名前と値のペアの階層リストで構成されます。 値を取得するには、対応する項目のキーを使用して `Configuration` インデクサーにアクセスします。

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

JSON 形式の構成ソースの配列を操作する場合は、コロンで区切られた文字列の一部として配列インデックスを使用します。 次の例では、上記の `wizards` 配列の最初の項目の名前を取得します。

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

組み込みの[構成](/dotnet/api/microsoft.extensions.configuration)プロバイダーに書き込まれた名前と値のペアは永続化**されません**。 ただし、値を保存するカスタム プロバイダーを作成することができます。 [カスタム構成プロバイダー](xref:fundamentals/configuration/index#custom-config-providers)に関する記述を参照してください。

上記のサンプルでは、構成インデクサーを使用して値を読み取ります。 `Startup` の外部の構成にアクセスする場合は、*オプション パターン*を使用します。 詳細については、「[オプション](xref:fundamentals/configuration/options)」トピックを参照してください。

## <a name="xml-configuration"></a>XML 構成

XML 形式の構成ソースの配列を操作するには、各要素に `name` インデックスを指定します。 インデックスを使用して値にアクセスします。

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a>環境別の構成

開発、テスト、運用などの異なる環境に応じて異なる構成を設定するのが一般的です。 ASP.NET Core 2.x アプリの `CreateDefaultBuilder` 拡張メソッド (ASP.NET Core 1.x アプリの場合は、`AddJsonFile` および `AddEnvironmentVariables` を直接使用) では、JSON ファイルとシステム構成ソースを読み取るために構成プロバイダーを追加します。

* *appsettings.json*
* *appsettings.\<EnvironmentName>.json*
* 環境変数

ASP.NET Core 1.x アプリは、`AddJsonFile` および [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_) を呼び出す必要があります。

パラメーターの説明については、「[AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions)」を参照してください。 `reloadOnChange` は、ASP.NET Core 1.1 以降でのみサポートされます。

構成ソースは指定された順に読み取られます。 先のコードでは、環境変数が最後に読み取られます。 環境を使用して設定された構成値で、2 つの前述のプロバイダーで設定された値が置き換えられます。

次の *appsettings.Staging.json* ファイルをご覧ください。

[!code-json[](index/sample/appsettings.Staging.json)]

次のコードで、`Configure` は `MyConfig` の値を読み取ります。

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

通常、環境は `Development`、`Staging`、または `Production` に設定されます。 詳細については、「[Use multiple environments](xref:fundamentals/environments)」(複数の環境の使用) を参照してください。

構成に関する考慮事項:

* [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) では、構成データを変更時に再読み込みできます。
* 構成キーでは大文字と小文字が区別**されません**。
* 構成プロバイダー コードやプレーンテキストの構成ファイルには、パスワードなどの機密データは格納**しないでください**。 開発環境やテスト環境では運用シークレットを使用しないでください。 プロジェクトの外部にシークレットを指定してください。そうすれば、誤ってリソース コード リポジトリにコミットされることはありません。 [複数の環境の使用方法](xref:fundamentals/environments)および[開発中のアプリ シークレットの安全な格納場所](xref:security/app-secrets)の管理の詳細を確認してください。
* 環境変数で指定された階層の構成値では、コロン (`:`) が一部のプラットフォームで動作しない可能性があります。 二重のアンダースコア (`__`) はすべてのプラットフォームでサポートされています。
* 構成 API とやり取りするときは、コロン (`:`) がすべてのプラットフォームで動作します。

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>メモリ内プロバイダーと POCO クラスへのバインディング

次の例では、メモリ内プロバイダーを使用して、クラスにバインドする方法を示します。

[!code-csharp[](index/sample/InMemory/Program.cs)]

構成値は文字列として返されますが、バインディングすることでプロジェクトを構築できます。 バインディングすることで POCO オブジェクトや、オブジェクト グラフ全体を取得できます。

### <a name="getvalue"></a>GetValue

次の例では、[GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) 拡張メソッドを示します。

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

ConfigurationBinder の `GetValue<T>` メソッドでは、既定値 (サンプルでは 80) を指定できます。 `GetValue<T>` は単純なシナリオ用であり、セクション全体にはバインドされません。 `GetValue<T>` は、特定の型に変換された `GetSection(key).Value` からスカラー値を取得します。

## <a name="bind-to-an-object-graph"></a>オブジェクト グラフにバインドする

クラス内の各オブジェクトに再帰的にバインドすることができます。 次の `AppSettings` クラスがあるとします。

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

次のサンプルでは `AppSettings` クラスにバインドします。

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** 以上では、セクション全体を操作する `Get<T>` を使用できます。 `Get<T>` は `Bind` を使用するよりも便利な場合があります。 次のコードでは、上記のサンプルで `Get<T>` を使用する方法を示します。

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

以下の *appsettings.json* ファイルを使用します。

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

プログラムは `Height 11` を表示します。

次のコードを使用して、構成の単体テストを行うことができます。

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a>Entity Framework のカスタム プロバイダーを作成する

このセクションでは、EF を使用してデータベースから名前と値のペアを読み取る基本的な構成プロバイダーが作成されます。

データベースに構成値を格納するための `ConfigurationValue` エンティティを定義します。

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

構成した値を格納し、その値にアクセスするための `ConfigurationContext` を追加します。

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

[IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) を実装するクラスを作成します。

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

[ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider) から継承して、カスタム構成プロバイダーを作成します。 構成プロバイダーは、空の場合にデータベースを初期化します。

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

サンプルを実行すると、データベースからの強調表示された値 ("value_from_ef_1" および "value_from_ef_2") が表示されます。

構成ソースを追加するための `EFConfigSource` 拡張メソッドを使用できます。

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

次のコードに、カスタム `EFConfigProvider` の使用方法を示します。

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

サンプルでは JSON プロバイダーの後にカスタム `EFConfigProvider` を追加するため、データベースからのすべての設定で *appsettings.json* ファイルからの設定がオーバーライドされることに注意してください。

以下の *appsettings.json* ファイルを使用します。

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

次のような出力が表示されます。

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>CommandLine 構成プロバイダー

[CommandLine 構成プロバイダー](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)は、実行時に構成するコマンドライン引数のキーと値のペアを受け取ります。

[CommandLine 構成サンプルを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>CommandLine 構成プロバイダーをセットアップして使用する

# <a name="basic-configurationtabbasicconfiguration"></a>[基本構成](#tab/basicconfiguration/)

コマンド ライン構成をアクティブにするには、[ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) のインスタンスで `AddCommandLine` 拡張メソッドを呼び出します。

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

コードを実行すると、次のような出力が表示されます。

```console
MachineName: MairaPC
Left: 1980
```

コマンド ラインで引数のキーと値のペアを渡すと、`Profile:MachineName` と `App:MainWindow:Left` の値が変更されます。

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

コンソール ウィンドウには次のように表示されます。

```console
MachineName: BartPC
Left: 1979
```

他の構成プロバイダーによって提供された構成をコマンドライン構成でオーバーライドするには、`ConfigurationBuilder` で最後に `AddCommandLine` を呼び出します。

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

一般的な ASP.NET Core 2.x アプリでは、便利な静的メソッド `CreateDefaultBuilder` を使用してホストを構築します。

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` では、省略可能構成を *appsettings.json*、*appsettings.{Environment}.json*、[user secrets](xref:security/app-secrets) (`Development` 環境の場合)、環境変数、およびコマンドライン引数から読み込みます。 CommandLine 構成プロバイダーは最後に呼び出されます。 最後にプロバイダーを呼び出すことにより、実行時に渡されたコマンドライン引数で、前に呼び出された他の構成プロバイダーによって設定された構成をオーバーライドできます。

次のような *appsettings* ファイルの場合:

* `reloadOnChange` が有効です。
* コマンドライン引数と *appsettings* ファイルに同じ設定が含まれています。
* 一致するコマンドライン引数を含む *appsettings* ファイルは、アプリの起動後に変更されます。

上記の条件がすべて真になると、コマンドライン引数がオーバーライドされます。

ASP.NET Core 2.x アプリでは、`CreateDefaultBuilder` ではなく [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) を使用できます。 `WebHostBuilder` を使用する場合は、[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) によって構成を手動で設定します。 詳細については、ASP.NET Core 1.x のタブを参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) を作成し、`AddCommandLine` メソッドを呼び出して CommandLine 構成プロバイダーを使用します。 最後にプロバイダーを呼び出すことにより、実行時に渡されたコマンドライン引数で、前に呼び出された他の構成プロバイダーによって設定された構成をオーバーライドできます。 `UseConfiguration` メソッドを使用して、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に構成を適用します。

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>引数

コマンド ラインで渡される引数は、次の表に示すように 2 つの形式のいずれかに準拠する必要があります。

| 引数の形式                                                     | 例        |
| ------------------------------------------------------------------- | :------------: |
| 単一引数: 等号 (`=`) で区切られたキーと値のペア | `key1=value`   |
| 2 つの引数のシーケンス: スペースで区切られたキーと値のペア    | `/key1 value1` |

**単一引数**

値は等号 (`=`) の後に配置する必要があります。 値を null にすることができます (例: `mykey=`)。

キーにはプレフィックスが付く場合があります。

| キーのプレフィックス               | 例         |
| ------------------------ | :-------------: |
| プレフィックスなし                | `key1=value1`   |
| 単一のダッシュ (`-`)&#8224; | `-key2=value2`  |
| 2 つのダッシュ (`--`)        | `--key3=value3` |
| スラッシュ (`/`)      | `/key4=value4`  |

&#8224;単一のダッシュ プレフィックス (`-`) が付いたキーは、[スイッチ マッピング](#switch-mappings)で指定する必要があります (以下を参照)。

コマンド例:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

注: `-key2` が構成プロバイダーに指定された[スイッチ マッピング](#switch-mappings)に表示されていない場合は、`FormatException` がスローされます。

**2 つの引数のシーケンス**

値を null にすることはできません。値はスペースで区切られたキーの後に配置する必要があります。

キーにはプレフィックスを付ける必要があります。

| キーのプレフィックス               | 例         |
| ------------------------ | :-------------: |
| 単一のダッシュ (`-`)&#8224; | `-key1 value1`  |
| 2 つのダッシュ (`--`)        | `--key2 value2` |
| スラッシュ (`/`)      | `/key3 value3`  |

&#8224;単一のダッシュ プレフィックス (`-`) が付いたキーは、[スイッチ マッピング](#switch-mappings)で指定する必要があります (以下を参照)。

コマンド例:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

注: `-key1` が構成プロバイダーに指定された[スイッチ マッピング](#switch-mappings)に表示されていない場合は、`FormatException` がスローされます。

### <a name="duplicate-keys"></a>重複キー

重複キーが指定されている場合は、最後のキーと値のペアが使用されます。

### <a name="switch-mappings"></a>スイッチ マッピング

`ConfigurationBuilder` を使用して構成を手動でビルドする場合、スイッチ マッピング ディクショナリを `AddCommandLine` メソッドに追加できます。 スイッチ マッピングでは、キー名の交換ロジックが許可されます。

スイッチ マッピング ディクショナリが使用されている場合、そのディレクトリで、コマンドライン引数によって指定されたキーと一致するキーが確認されます。 ディクショナリでコマンドライン キーが見つかった場合は、構成を設定するためにディクショナリの値 (キー交換) が返されます。 スイッチ マッピングは、単一のダッシュ (`-`) が前に付いたすべてのコマンドライン キーに必要です。

スイッチ マッピング ディクショナリ キーの規則:

* スイッチはダッシュ (`-`) または二重ダッシュ (`--`) で開始する必要があります。
* スイッチ マッピング ディクショナリに重複キーを含めることはできません。

以下の例では、`GetSwitchMappings` メソッドを使用します。これにより、コマンドライン引数で単一ダッシュ (`-`) キー プレフィックスを使用でき、先頭にサブキーのプレフィックスが付かないようにすることができます。

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

コマンドライン引数を指定しないと、`AddInMemoryCollection` に指定されたディクショナリで構成値が設定されます。 次のコマンドでアプリを実行します。

```console
dotnet run
```

コンソール ウィンドウには次のように表示されます。

```console
MachineName: RickPC
Left: 1980
```

構成設定を渡す場合は、以下のコマンドを使用します。

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

コンソール ウィンドウには次のように表示されます。

```console
MachineName: DahliaPC
Left: 1984
```

スイッチ マッピング ディクショナリが作成されると、以下の表に示すデータが含まれます。

| キー            | [値]                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

ディクショナリを使用してキーの切り替えを示すために、以下のコマンドを実行します。

```console
dotnet run -MachineName=ChadPC -Left=1988
```

コマンドライン キーが交換されます。 コンソール ウィンドウに、`Profile:MachineName` と `App:MainWindow:Left` の構成値が表示されます。

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>web.config ファイル

IIS または IIS-Express でアプリをホストする場合は、*web.config* ファイルが必要です。 *web.config* を設定することで、[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) を有効にし、アプリを起動して他の IIS 設定とモジュールを構成することができます。 *web.config* ファイルが存在せず、プロジェクト ファイルに `<Project Sdk="Microsoft.NET.Sdk.Web">` が含まれている場合、プロジェクトを発行すると、発行先 (*publish* フォルダー) に *web.config* ファイルが作成されます。 詳細については、「[IIS を使用した Windows での ASP.NET Core のホスト](xref:host-and-deploy/iis/index#webconfig-file)」を参照してください。

## <a name="access-configuration-during-startup"></a>起動中に構成にアクセスする

起動中に `ConfigureServices` または `Configure` 内の構成にアクセスするには、[アプリケーションの起動](xref:fundamentals/startup)に関するトピックに示されている例を参照してください。

## <a name="adding-configuration-from-an-external-assembly"></a>外部アセンブリからの構成の追加

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) の実装により、アプリの `Startup` クラスの外部にある外部アセンブリからの起動時に拡張機能をアプリに追加できるようになります。 詳細については、「[Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration)」(外部アセンブリからアプリを拡張する) を参照してください。

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a>Razor ページまたは MVC ビューで構成にアクセスする

Razor ページまたは MVC ビューで構成設定にアクセスするには、[Microsoft.Extensions.Configuration 名前空間](/dotnet/api/microsoft.extensions.configuration)に [using ディレクティブ](xref:mvc/views/razor#using) ([C# リファレンス: using ディレクティブ](/dotnet/csharp/language-reference/keywords/using-directive)) を追加して、[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) をページまたはビューに注入します。

Razor ページ: 

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

MVC ビュー: 

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a>補足メモ

* 依存関係挿入 (DI) は、`ConfigureServices` が呼び出されるまでセットアップされません。
* 構成システムは DI に対応していません。
* `IConfiguration` には次の 2 つ特性があります。
  * `IConfigurationRoot` ルート ノードに使用されます。 再読み込みをトリガーできます。
  * `IConfigurationSection` 構成値のセクションを表します。 `GetSection` および `GetChildren` メソッドは `IConfigurationSection` を返します。
  * 構成を再度読み込むとき、あるいは各プロバイダーにアクセスする場合、[IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) を使用します。 いずれの状況も一般的ではありません。

## <a name="additional-resources"></a>その他の技術情報

* [オプション](xref:fundamentals/configuration/options)
* [複数の環境の使用](xref:fundamentals/environments)
* [開発中のアプリ シークレットの安全な格納](xref:security/app-secrets)
* [ASP.NET Core でのホスティング](xref:fundamentals/host/index)
* [依存性の注入](xref:fundamentals/dependency-injection)
* [Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration)
