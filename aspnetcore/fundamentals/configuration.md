---
title: "ASP.NET Core の構成"
author: rick-anderson
description: "構成 API を使用して、複数のソースからの ASP.NET Core アプリケーションを構成する方法を説明します。"
keywords: "ASP.NET Core、構成では、JSON の構成"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: d626768fe1a485705e104a5c758cbdb0b46685a3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core の構成

[Rick Anderson](https://twitter.com/RickAndMSFT)、[マーク Michaelis](http://intellitect.com/author/mark-michaelis/)、 [Steve Smith](https://ardalis.com/)、 [Daniel Roth](https://github.com/danroth27)、および[Luke Latham](https://github.com/guardrex)

構成 API は、名前と値のペアの一覧で、アプリケーションを構成する方法を提供します。 構成は実行時に複数のソースから読み取られます。 名前と値のペアは、複数レベルの階層化することができます。 構成プロバイダーがあります。

* ファイル形式 (INI、JSON、および XML)
* コマンドライン引数
* 環境変数
* メモリ内の .NET オブジェクト
* 暗号化されたユーザー ストア
* [Azure Key Vault](xref:security/key-vault-configuration)
* カスタム プロバイダーをインストールまたは作成します。

各構成値は、文字列のキーにマップされます。 カスタム設定を逆シリアル化する組み込みのバインディング サポートは[POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)オブジェクト (シンプルな .NET クラスのプロパティを持つ)。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="simple-configuration"></a>単純な構成

次のコンソール アプリケーションは、JSON の構成プロバイダーを使用します。

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

アプリでは、読み取りし、次の構成設定を表示します。

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

構成は、ノードが、コロンで区切られた名前と値のペアの階層リストで構成されます。 値を取得するには、アクセス、`Configuration`インデクサーに対して、対応する項目のキー。

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

JSON 形式構成ソース内の配列を使用するには、コロンで区切られた文字列の一部として、配列インデックスを使用します。 次の例は、上記の最初の項目の名前を取得`wizards`配列。

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

組み込みに書き込まれる名前と値のペアで`Configuration`プロバイダーは**いない**永続化して、ただし、プロバイダーを作成できます、カスタム値を保存します。 参照してください[カスタム構成プロバイダー](xref:fundamentals/configuration#custom-config-providers)です。

上記のサンプルでは、構成のインデクサーを使用して値を読み取ります。 外部アクセス構成を`Startup`を使用して、[オプション パターン](xref:fundamentals/configuration#options-config-objects)です。 *オプション パターン*この記事で後述します。

通常をさまざまな環境、開発、テスト、および運用環境などの別の構成設定があります。 `CreateDefaultBuilder` ASP.NET Core 2.x アプリケーションでも拡張メソッド (またはを使用して`AddJsonFile`と`AddEnvironmentVariables`ASP.NET Core 1.x アプリ内で直接) JSON ファイルおよびシステム構成ソースを読み取るための構成プロバイダーを追加します。

* *appsettings.json*
* *appsettings です。\<EnvironmentName > .json*
* 環境変数

参照してください[AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions)パラメーターの説明。 `reloadOnChange`ASP.NET Core 1.1 以降のみサポートされます。 

構成ソースは、指定された順序で読み取られます。 上記のコードでは、環境変数が前回読み取られます。 環境を使用して設定、構成値には、2 つの以前のプロバイダーで設定されていると交換してください。

環境が通常のいずれかに設定されている`Development`、 `Staging`、または`Production`です。 参照してください[複数の環境で作業](environments.md)詳細についてはします。

構成に関する考慮事項:

* `IOptionsSnapshot`変更されたとき、構成データを再読み込みできます。 使用して`IOptionsSnapshot`構成データを再読み込みする必要がある場合。  参照してください[IOptionsSnapshot](#ioptionssnapshot)詳細についてはします。
* 構成キーは、大文字小文字を区別します。
* ベスト プラクティスは、ローカルの環境は、何も展開された構成ファイルで設定をオーバーライドできるようにする、最後に、環境変数を指定するです。
* **決して**プロバイダーの構成コードまたは構成ファイルをプレーン テキストでパスワードまたは他の機密データを格納します。 環境をテストしたり、開発に実稼働のシークレットを使用しないでください。 代わりに、リポジトリに誤ってコミットするため、プロジェクト ツリーの外部のシークレットを指定します。 詳細については[複数の環境で作業](environments.md)および管理する[アプリ シークレットは、開発中の安全な保管](../security/app-secrets.md)です。
* 場合`:`することはできません、システムで環境変数の使用を置き換えます`:`で`__`(二重のアンダー スコア)。

<a name="options-config-objects"></a>

## <a name="using-options-and-configuration-objects"></a>オプションと構成オブジェクトを使用します。

オプションのパターンでは、カスタム オプション クラスを使用して、関連する設定のグループを表します。 アプリ内で各機能の分離型のクラスを作成することをお勧めします。 分離されたクラスに従ってください。

* [インターフェイスの分離の原則 (ISP)](http://deviq.com/interface-segregation-principle/) : クラスが使用する構成設定のみに依存します。
* [関心の分離](http://deviq.com/separation-of-concerns/): アプリのさまざまな部分の設定が依存するまたは相互に結合します。

オプション クラスは非抽象である必要がありますパブリック パラメーターなしのコンス トラクターを使用します。 例:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name="options-example"></a>

次のコードでは、JSON の構成プロバイダーは有効です。 `MyOptions`クラスをサービス コンテナーに追加および構成にバインドします。

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

次[コント ローラー](../mvc/controllers/index.md)使用[コンス トラクター依存性の注入](xref:fundamentals/dependency-injection#what-is-dependency-injection)で[ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1)設定にアクセスします。

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

次の*される appsettings.json*ファイル。

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

`HomeController.Index`メソッドを返します。`option1 = value1_from_json, option2 = 2`です。

一般的なアプリは、構成全体を 1 つのオプション ファイルにバインドされません。 後で使用する方法を示します`GetSection`セクションにバインドします。

次のコードでは、1 秒あたり`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。 使用してバインディングを構成するデリゲートを使用して、`MyOptions`です。

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

複数の構成プロバイダーを追加することができます。 NuGet パッケージの構成プロバイダーを利用できます。 登録されている順序で適用されます。

各呼び出し`Configure<TOptions>`を追加、`IConfigureOptions<TOptions>`サービスをサービス コンテナーにします。 前の例では、値で`Option1`と`Option2`で指定された両方*される appsettings.json* --の値が、`Option1`構成済みのデリゲートによってオーバーライドされます。 

"Wins"最後の構成ソースが指定された 1 つ以上の構成サービスを有効にすると、(構成値を設定します)。 上記のコードでは、`HomeController.Index`メソッドを返します。`option1 = value1_from_action, option2 = 2`です。

フォームの構成キー オプションの種類の各プロパティがバインドされているオプションを構成にバインドすると、`property[:sub-property:]`です。 たとえば、`MyOptions.Option1`プロパティがキーにバインドされる`Option1`からこれは読み取り、`option1`プロパティ*される appsettings.json*です。 この記事で後述サブプロパティ サンプルが表示されます。

次のコードでは、3 つ目の`IConfigureOptions<TOptions>`サービスは、サービス コンテナーに追加します。 バインドされている`MySubOptions`セクションに`subsection`の*される appsettings.json*ファイル。

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

注: この拡張メソッドが必要です、 `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet パッケージです。

以下を使用して*される appsettings.json*ファイル。

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

`MySubOptions`クラス。

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

次の`Controller`:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

`subOption1 = subvalue1_from_json, subOption2 = 200`返されます。

ビューのモデルでのオプションを指定したり挿入したりできます`IOptions<TOptions>`ビューに直接。

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name="in-memory-provider"></a>

## <a name="ioptionssnapshot"></a>IOptionsSnapshot

*ASP.NET Core 1.1 以上が必要です。*

`IOptionsSnapshot`構成ファイルが変更されたときに、構成データを再読み込みをサポートします。 最小限のオーバーヘッドがあります。 使用して`IOptionsSnapshot`で`reloadOnChange: true`、オプションにバインドされた`IConfiguration`および変更されたときに再読み込みします。

次の例は、新しい方法について説明します。`IOptionsSnapshot`後に作成された*config.json*変更します。 同じサーバーに要求が返される時刻*config.json*が**いない**を変更します。 後の最初の要求*config.json*変更新しい時刻が表示されます。

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

次の図は、サーバーの出力を示しています。

![ブラウザーのイメージを示す"最終更新日: 2016 11 月 22 日 4時 43分 PM"](configuration/_static/first.png)

ブラウザーを更新するメッセージの値または表示される時刻は変更されません (ときに*config.json*が変更されていない)。

変更し、保存、 *config.json*し、ブラウザーを更新します。

![ブラウザーのイメージを示す"e: に最後に更新された 2016 11 月 22 日 4時 53分 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>メモリ内のプロバイダーと POCO クラスへのバインディング

次の例では、メモリ内のプロバイダーを使用して、クラスにバインドする方法を示します。

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

構成値は、文字列として返されますが、バインド、オブジェクトの作成を有効にします。 バインディングでは、POCO オブジェクトやオブジェクトも全体のグラフを取得することができます。 次の例は、バインドする方法を示しています。`MyWindow`および ASP.NET Core MVC アプリのオプションのパターンを使用します。

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

カスタム クラスをバインド`ConfigureServices`ホストを作成するときに。

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

設定を表示、 `HomeController`:

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a>GetValue

次の例は、 [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_)拡張メソッド。

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

ConfigurationBinder の`GetValue<T>`メソッドでは、既定値 (このサンプルでは 80) を指定することができます。 `GetValue<T>`単純なシナリオでは、セクション全体にバインドされません。 `GetValue<T>`スカラー値を取得`GetSection(key).Value`特定の種類に変換します。

## <a name="binding-to-an-object-graph"></a>オブジェクト グラフへのバインド

クラス内の各オブジェクトを再帰的にバインドをできます。 次の検討`AppOptions`クラス。

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

次の例にバインド、`AppOptions`クラス。

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1**以上使用できると`Get<T>`、セクション全体と連携します。 `Get<T>`使用するよりも多くのも、`Bind`です。 次のコードは、使用する方法を示しています。`Get<T>`と上のサンプル。

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

以下を使用して*される appsettings.json*ファイル。

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

プログラムは、表示`Height 11`です。

次のコードは、単位に使用できる構成をテストします。

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

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a>Entity Framework のカスタム プロバイダーの基本的なサンプル

このセクションでは、EF を使用してデータベースから名前/値ペアを表示する基本的な構成プロバイダーが作成されます。 

定義、`ConfigurationValue`データベースの構成値を格納するためのエンティティ。

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

追加、`ConfigurationContext`を保存し、構成された値にアクセスします。

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

実装するクラスを作成する[IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

継承することで、カスタム構成プロバイダーを作成する[ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider)です。  構成プロバイダーは、空であるときに、データベースを初期化します。

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

サンプルを実行する、強調表示されているデータベースからの値 ("value_from_ef_1"および"value_from_ef_2") が表示されます。

追加することができます、`EFConfigSource`構成ソースを追加するための拡張メソッド。

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

次のコードは、ユーザー設定を使用する方法を示しています`EFConfigProvider`:。

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

サンプルを追加、カスタム注`EFConfigProvider`JSON プロバイダーの後にそのため、データベースから設定設定を上書きするから、*される appsettings.json*ファイル。

以下を使用して*される appsettings.json*ファイル。

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

次が表示されます。

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>コマンドライン構成プロバイダー

[コマンドライン構成プロバイダー](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider)実行時に構成のキー/値ペアのコマンドライン引数を受け取ります。

[表示またはダウンロードするコマンドラインのサンプルの構成](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a>プロバイダーを設定します。

# <a name="basic-configurationtabbasicconfiguration"></a>[基本構成](#tab/basicconfiguration)

コマンドライン構成をアクティブに呼び出して、`AddCommandLine`拡張メソッドのインスタンスを[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

コードを実行するには、次の出力が表示されます。

```console
MachineName: MairaPC
Left: 1980
```

値を変更するコマンド ラインでキーと値のペアの引数を渡す`Profile:MachineName`と`App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

コンソール ウィンドウが表示されます。

```console
MachineName: BartPC
Left: 1979
```

コマンドライン構成とその他の構成プロバイダーによって提供される構成を上書きするには、呼び出す`AddCommandLine`の最後に`ConfigurationBuilder`:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x アプリケーションの一般的な便利な静的メソッドを使用して`CreateDefaultBuilder`ホストを作成します。

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

`CreateDefaultBuilder`オプションの構成を読み込む*される appsettings.json*、 *appsettings {。環境} .json*、[ユーザー シークレット](xref:security/app-secrets)(で、`Development`環境)、環境変数とコマンドライン引数。 コマンドライン構成プロバイダーは、最後に呼び出されます。 最後に、プロバイダーを呼び出すことにより、構成セットを他の構成プロバイダーをオーバーライドする実行時に渡されるコマンドライン引数が以前に呼び出されます。

注意してください*appsettings*ファイル`reloadOnChange`を有効にします。 構成値が一致する場合にコマンドライン引数がオーバーライドされる、 *appsettings*アプリが開始した後にファイルが変更されました。

> [!NOTE]
> 使用する代わりに、`CreateDefaultBuilder`メソッドを使用してホストを作成する[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)と構成を手動で構築および[ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ASP.NET Core ではサポートされて 2.x です。 詳細については、ASP.NET Core 1.x タブを参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

作成、 [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)を呼び出すと、`AddCommandLine`コマンドライン構成プロバイダーを使用する方法です。 最後に、プロバイダーを呼び出すことにより、構成セットを他の構成プロバイダーをオーバーライドする実行時に渡されるコマンドライン引数が以前に呼び出されます。 構成の適用[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)で、`UseConfiguration`メソッド。

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>引数

コマンドラインで渡される引数は、次の表に示すように 2 つの形式のいずれかに従う必要があります。

| 引数の形式                                                     | 例        |
| ------------------------------------------------------------------- | :------------: |
| 1 つの引数: キー値のペアは等号で区切られた (`=`) | `key1=value`   |
| 2 つの引数の順序: キー値の組み合わせをスペースで区切って    | `/key1 value1` |

**1 つの引数**

値は、等号を従う必要があります (`=`)。 値を null にすることができます (たとえば、 `mykey=`)。

キーは、プレフィックスがあります。

| キーのプレフィックス               | 例         |
| ------------------------ | :-------------: |
| プレフィックスなし                | `key1=value1`   |
| 単一のダッシュ (`-`) & #8224 です。 | `-key2=value2`  |
| 2 個のダッシュ (`--`)        | `--key3=value3` |
| スラッシュ (`/`)      | `/key4=value4`  |

& #8224 です。単一 dash プレフィックスを持つキー (`-`) で指定する必要があります[マッピングを切り替える](#switch-mappings)、以下に説明します。

コマンドの例:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

メモ: 場合`-key1`に存在しない、[マッピングを切り替える](#switch-mappings)構成プロバイダーに指定された、`FormatException`がスローされます。

**2 つの引数の順序**

値は null にすることはできません、スペースで区切られたキーに従う必要があります。

キーには、プレフィックスが必要です。

| キーのプレフィックス               | 例         |
| ------------------------ | :-------------: |
| 単一のダッシュ (`-`) & #8224 です。 | `-key1 value1`  |
| 2 個のダッシュ (`--`)        | `--key2 value2` |
| スラッシュ (`/`)      | `/key3 value3`  |

& #8224 です。単一 dash プレフィックスを持つキー (`-`) で指定する必要があります[マッピングを切り替える](#switch-mappings)、以下に説明します。

コマンドの例:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

メモ: 場合`-key1`に存在しない、[マッピングを切り替える](#switch-mappings)構成プロバイダーに指定された、`FormatException`がスローされます。

### <a name="duplicate-keys"></a>重複するキー

重複するキーを指定しない場合は、最後のキー/値ペアが使用されます。

### <a name="switch-mappings"></a>スイッチのマッピング

構成を手動で構築するとき`ConfigurationBuilder`、必要に応じて、スイッチのマッピングのディクショナリを指定することができます、`AddCommandLine`メソッドです。 スイッチのマッピングを使用すると、キー名の交換ロジックを提供できます。

スイッチのマッピングのディクショナリを使用すると、コマンドライン引数によって提供されるキーに一致するキーのディクショナリがチェックされます。 コマンド ライン キーがディクショナリ内で見つかった場合、ディクショナリの値 (キーの交換) は、構成設定に渡されます。 スイッチ マッピングは任意のコマンド ライン キー ダッシュを 1 つ付いて (`-`)。

マッピングのディクショナリ キーの規則が切り替わります。

* スイッチがダッシュで開始する必要があります (`-`) または 2 つの破線 (`--`)。
* スイッチのマッピングのディクショナリでは、重複するキーを含めることはできません。

次の例で、`GetSwitchMappings`メソッドにより、1 つのダッシュを使用する、コマンドライン引数 (`-`) プレフィックスをキーし、サブキーの先頭のプレフィックスを回避します。

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

コマンドライン引数を指定せず、辞書が渡された`AddInMemoryCollection`構成値を設定します。 次のコマンドを使用して、アプリを実行します。

```console
dotnet run
```

コンソール ウィンドウが表示されます。

```console
MachineName: RickPC
Left: 1980
```

構成設定を渡すには、次を使用します。

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

コンソール ウィンドウが表示されます。

```console
MachineName: DahliaPC
Left: 1984
```

スイッチのマッピングのディクショナリを作成した後、次の表に示すようにデータが含まれています。

| キー            | 値                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

ディクショナリを使用して、キーの切り替えを示すためには、次のコマンドを実行します。

```console
dotnet run -MachineName=ChadPC -Left=1988
```

コマンド ライン キーが交換されます。 コンソール ウィンドウに表示の構成値`Profile:MachineName`と`App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>Web.config ファイル

A *web.config* IIS または IIS Express でアプリをホストしている場合は、ファイルが必要です。 *web.config*アプリを起動するように IIS で AspNetCoreModule をオンにします。 設定*web.config*アプリを起動し、その他の IIS の設定とモジュールを構成するために IIS の AspNetCoreModule を有効にします。 Visual Studio を使用して削除すると*web.config*、Visual Studio は、新しく作成します。

## <a name="additional-notes"></a>補足メモ

* 依存関係の挿入 (DI) が設定されていないまで後`ConfigureServices`が呼び出されます。
* 構成システムでは DI に注意してください。
* `IConfiguration`2 つの特殊化があります。
  * `IConfigurationRoot`ルート ノードに使用します。 再読み込みをトリガーできます。
  * `IConfigurationSection`構成値のセクションを表します。 `GetSection`と`GetChildren`を返し、`IConfigurationSection`です。

## <a name="additional-resources"></a>その他の技術情報

* [複数の環境の使用](environments.md)
* [開発中のアプリ シークレットの安全な保存](../security/app-secrets.md)
* [ASP.NET Core でのホスティング](xref:fundamentals/hosting)
* [依存性の注入](dependency-injection.md)
* [Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration)
