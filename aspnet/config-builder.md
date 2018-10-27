---
uid: config-builder
title: ASP.NET の構成ビルダー
author: rick-anderson
description: 外部ソースから値を web.config 以外のソースから構成データを取得する方法について説明します。
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148838"
---
# <a name="configuration-builders-for-aspnet"></a>ASP.NET の構成ビルダー

によって[Stephen Molloy](https://github.com/StephenMolloy)と[Rick Anderson](https://twitter.com/RickAndMSFT)

構成ビルダーは、外部ソースから構成値を取得する ASP.NET アプリに対して最新でアジャイルなメカニズムを提供します。

構成ビルダー:

* .NET Framework 4.7.1 で使用可能な以降。
* 構成値を読み取るための柔軟なメカニズムを提供します。
* コンテナーに移動し、フォーカスがある環境をクラウド アプリの基本的な要件の一部に対処します。
* これまで使用できなかったソースから描画することで構成データ (たとえば、Azure Key Vault と環境変数) の保護を向上させるために、.NET 構成システムで使用できます。

## <a name="keyvalue-configuration-builders"></a>キー/値の構成ビルダー

構成ビルダーによって処理できる一般的なシナリオは、キー/値のパターンに準拠する構成セクションのキー/値の基本的な交換メカニズムを提供します。 ConfigurationBuilders の .NET Framework の概念は、特定の構成のセクションでは、またはパターンに制限はありません。 ただし、多くの構成ビルダーの`Microsoft.Configuration.ConfigurationBuilders`([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)、 [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) キー/値のパターン内で機能します。

## <a name="keyvalue-configuration-builders-settings"></a>キー/値の構成ビルダーの設定

次の設定ですべてのキー/値構成ビルダーを適用する`Microsoft.Configuration.ConfigurationBuilders`します。

### <a name="mode"></a>モード

構成ビルダーでは、外部ソースのキー/値の情報を使用して、構成システムの要素を選択したキー/値の設定。 具体的には、`<appSettings/>`と`<connectionStrings/>`のセクションでは、構成ビルダーから特別な処理を受信します。 作成者は、3 つのモードで動作します。

* `Strict` 既定モードです。 このモードで構成ビルダーはよく知られているキー/値を中心とした構成セクションに対してのみ動作します。 `Strict` モードでは、セクションの各キーを列挙します。 一致する場合、外部ソース キーはあります。

   * 構成ビルダーは、外部ソースからの値で結果として得られる構成セクションの値を置き換えます。
* `Greedy` -このモードは密接に関連する`Strict`モード。 既にキーにとらわれることがなく、元の構成が存在します。

  * 構成ビルダーは、結果として得られる構成セクションに、外部ソースからのすべてのキー/値ペアを追加します。

* `Expand` -構成セクション オブジェクトに解析前に、生の XML では動作します。 文字列内のトークンの拡張と考えることができます。 パターンに一致する生の XML 文字列の一部`${token}`はトークンの拡張の候補です。 外部ソースに対応する値が見つからない場合、トークンは変更されません。 このモードでのビルダーがこれらに限定されません、`<appSettings/>`と`<connectionStrings/>`セクション。

次のマークアップから*web.config*により、 [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/)で`Strict`モード。

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

次のコードの読み取り、`<appSettings/>`と`<connectionStrings/>`前述*web.config*ファイル。

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

上記のコードでは、プロパティ値に設定します。

* 内の値、 *web.config*ファイル環境変数では、キーが設定されていない場合。
* 環境の値、変数の場合に設定します。

たとえば、`ServiceID`が含まれます。

* 「Web.config ファイルからの ServiceID 値」場合は、環境変数`ServiceID`が設定されていません。
* 値、`ServiceID`環境変数の場合に設定します。

次の図は、 `<appSettings/>` 、前のキー/値の*web.config*環境エディターでのファイル セット。

![環境のエディター](config-builder/static/env.png)

注: 終了し、環境変数に変更を表示する Visual Studio を再起動する必要があります。

### <a name="prefix-handling"></a>プレフィックスの処理

キーのプレフィックスは、ために、設定キーを簡略化できます。

* .NET Framework 構成は、複雑で入れ子になったです。
* 外部キー/値のソースは、基本的なと本質的にフラットでは一般的です。 たとえば、環境変数は入れ子にできません。

次の方法のいずれかを使用して、両方を挿入する`<appSettings/>`と`<connectionStrings/>`環境変数を使用して、構成に。

* `EnvironmentConfigBuilder`既定`Strict`モードと、構成ファイル内の適切なキー名。 上記のコードとマークアップは、このアプローチを採用します。 できます、このアプローチを使用して**いない**両方のキーが同じ名前の付いた`<appSettings/>`と`<connectionStrings/>`します。
* 2 つを使用して、`EnvironmentConfigBuilder`内`Greedy`異なるプレフィックスを持つモードと`stripPrefix`します。 この方法で、アプリが読み取ることができます`<appSettings/>`と`<connectionStrings/>`構成ファイルを更新する必要はありません。 次のセクションでは、 [stripPrefix](#stripprefix)、これを行う方法を示しています。
* 2 つを使用して、`EnvironmentConfigBuilder`内`Greedy`異なるプレフィックスを持つモード。 この方法でキー名を変更する必要がありますプレフィックスとキー名が重複することはできません。  例えば:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

上記のマークアップでは、2 つの異なるセクションの構成を設定する同じフラットなキー/値のソースを使用できます。

次の図は、`<appSettings/>`と`<connectionStrings/>`、前のキー/値の*web.config*環境エディターでのファイル セット。

![環境のエディター](config-builder/static/prefix.png)

次のコードの読み取り、`<appSettings/>`と`<connectionStrings/>`キー/値の前に含まれている*web.config*ファイル。

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

上記のコードでは、プロパティ値に設定します。

* 内の値、 *web.config*ファイル環境変数では、キーが設定されていない場合。
* 環境の値、変数の場合に設定します。

たとえば、前を使用して*web.config*ファイル、前の環境のエディターの画像および前のコードは、次の値のキー/値が設定されます。

|  キー              | [値] |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | 環境変数から AppSetting_ServiceID|
|    AppSetting_default            | Env から AppSetting_default 値 |
|       ConnStr_default         | Env から ConnStr_default val|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: ブール値、の既定値は`false`します。 

上記の XML マークアップは、アプリ設定接続文字列からを区切る、内のすべてのキーが必要です、 *web.config*指定したプレフィックスを使用するファイル。 たとえば、プレフィックス`AppSetting`に追加する必要があります、`ServiceID`キー ("AppSetting_ServiceID")。 `stripPrefix`で、プレフィックスが使用されていない、 *web.config*ファイル。 (たとえば、環境です。) で構成ビルダーのソースでプレフィックスが必要です。ほとんどの開発者が使用予定`stripPrefix`します。

アプリケーションは、通常、プレフィックスを削除します。 次*web.config*プレフィックスを削除します。

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

上記の「 *web.config*ファイル、`default`キー両方では、`<appSettings/>`と`<connectionStrings/>`。

次の図は、`<appSettings/>`と`<connectionStrings/>`、前のキー/値の*web.config*環境エディターでのファイル セット。

![環境のエディター](config-builder/static/prefix.png)

次のコードの読み取り、`<appSettings/>`と`<connectionStrings/>`キー/値の前に含まれている*web.config*ファイル。

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

上記のコードでは、プロパティ値に設定します。

* 内の値、 *web.config*ファイル環境変数では、キーが設定されていない場合。
* 環境の値、変数の場合に設定します。

たとえば、前を使用して*web.config*ファイル、前の環境のエディターの画像および前のコードは、次の値のキー/値が設定されます。

|  キー              | [値] |
| ----------------- | ------------ |
|     ServiceID           | 環境変数から AppSetting_ServiceID|
|    default            | Env から AppSetting_default 値 |
|    default         | Env から ConnStr_default val|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: 文字列、既定値は `@"\$\{(\w+)\}"`

`Expand` 、ビルダーの動作は、生の XML のようなトークンを検索`${token}`します。 既定の正規表現では検索`@"\$\{(\w+)\}"`します。 一致する文字セット`\w`が XML と多くの構成ソースを使用するよりも厳格です。 使用`tokenPattern`よりも文字以上と`@"\$\{(\w+)\}"`トークン名が必要です。

`tokenPattern`: 文字列:

* 開発者は、トークンの照合に使用される正規表現を変更できます。
* 適切な形式で、危険で非正規表現がかどうかを確認する検証は行われません。
* キャプチャ グループを含める必要があります。 全体の正規表現は、全体のトークンと一致する必要があります。 最初のキャプチャは、構成ソースを検索するため、トークンの名前である必要があります。

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>構成ビルダー Microsoft.Configuration.ConfigurationBuilders で

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* 構成ビルダーの最も簡単です。
* 環境から値を読み取ります。
* 追加の構成オプションはありません。
* `name`属性の値は任意です。

**注:** Windows コンテナー環境で、実行時に設定された変数は、エントリ ポイント プロセスの環境に挿入のみされます。 サービスとして実行するアプリまたは非エントリ ポイント プロセスでは、それ以外の場合、コンテナー内のメカニズムを通じてを注入がない限り、これらの変数を選択しないでください。 [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-コンテナーの現在のバージョンに基づく[ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41)でこれを処理、 *DefaultAppPool*のみです。 その他の Windows ベースのコンテナーのバリアントは、EntryPoint 以外のプロセスの独自性の注入メカニズムを開発する必要があります。

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> 決してのパスワードを保存、機密性の高い接続文字列、またはソース コード内の他の機密データにします。 運用シークレットを使用する必要があります、開発やテストします。

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

この構成ビルダーのような機能を提供する[ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets)します。

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) .NET Framework プロジェクトで使用できますが、シークレット ファイルを指定する必要があります。 代わりに、定義、`UserSecretsId`プロジェクト内のプロパティ ファイルを開き、正しい位置を読み取り用に、生のシークレット ファイルを作成します。 プロジェクトから外部の依存関係を保持するには、シークレットのファイルは XML 形式です。 実装の詳細は、XML の書式設定して、形式に依存する必要があります。 共有する必要がある場合、 *secrets.json* .NET Core プロジェクトでファイルを使用を検討して、 [SimpleJsonConfigBuilder](#simplejsonconfig)します。 `SimpleJsonConfigBuilder` .NET Core 見なす必要がありますも変更される可能性の実装の詳細の書式を設定します。

構成属性を`UserSecretsConfigBuilder`:

* `userSecretsId` -これは、XML のシークレット ファイルを識別するための推奨される方法です。 使用して .NET Core のような動作、`UserSecretsId`プロジェクトのプロパティをこの識別子を格納します。 文字列は一意である必要があります、GUID を指定する必要はありません。 この属性で、`UserSecretsConfigBuilder`によく知られているローカルの場所を検索 (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) にこの識別子に属しているシークレット ファイル。
* `userSecretsFile` -省略可能な属性は、シークレットを格納するファイルを指定します。 `~`アプリケーション ルートへの参照を先頭に文字を使用できます。 このいずれかの属性または`userSecretsId`属性が必要です。 両方が指定されて場合`userSecretsFile`が優先されます。
* `optional`: ブール値、既定値`true`-シークレット ファイルが見つからない場合は、例外を防止します。 
* `name`属性の値は任意です。

シークレット ファイルには、次の形式があります。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/)に格納されている値を読み取り、 [Azure Key Vault](/azure/key-vault/key-vault-whatis)します。

`vaultName` (資格情報コンテナーの名前)、または資格情報コンテナーへの URI のいずれかが必要です。 その他の属性に接続するには、どのコンテナーに関する制御を許可するで動作する環境でアプリケーションが実行されていない場合のみ必要なの`Microsoft.Azure.Services.AppAuthentication`します。 Azure サービスの認証ライブラリは、自動的に可能であれば、実行環境からの接続情報を選択に使用されます。 自動的にオーバーライドする接続文字列を提供することで接続情報を取得します。

* `vaultName` -必要な場合`uri`で指定されていません。 キー/値ペアの読み取り元となる Azure サブスクリプションでは、コンテナーの名前を指定します。
* `connectionString` 使用可能な接続文字列は- [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -指定した場合は、その他の Key Vault プロバイダーに接続する`uri`値。 Azure を指定しない場合 (`vaultName`) は、資格情報コンテナーのプロバイダーです。
* `version` -Azure Key Vault はシークレットのバージョン管理機能を提供します。 場合`version`を指定すると、ビルダーでは、このバージョンに一致するシークレットのみを取得します。
* `preloadSecretNames` -既定では、このビルダー クエリ**すべて**が初期化されるときに、key vault 名をキーします。 すべてのキー値の読み取りを防ぐためには、この属性を設定`false`します。 これを設定`false`一度に 1 つのシークレットを読み取ります。 シークレットの便利な場合は、コンテナーは、"Get"アクセスが、"List"にアクセスを一度に 1 つできますを読み取っています。 **注:** を使用する場合`Greedy`モード、`preloadSecretNames`あります`true`(既定値です)。

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/)はディレクトリのファイルの値のソースとして使用する基本的な構成ビルダー。 ファイルの名前は、キーと、コンテンツが値。 この構成ビルダーは、調整されたコンテナー環境で実行するときに役立ちます。 Docker Swarm と Kubernetes を提供するようにシステム`secrets`キー ファイルごとにこの方法では、その調整された windows コンテナーにします。

属性の詳細:

* `directoryPath` - 必須。 値を検索するパスを指定します。 機密情報が格納されている Windows の docker、 *C:\ProgramData\Docker\secrets*既定ディレクトリ。
* `ignorePrefix` -このプレフィックスで始まるファイルが除外されます。 既定値"ignore"です。
* `keyDelimiter` 既定値は`null`します。 場合は、指定された構成ビルダーは、この区切り記号でキー名を構築、ディレクトリの複数のレベルを走査します。 この値が場合`null`ディレクトリの最上位レベルだけ構成ビルダーを検索します。
* `optional` 既定値は`false`します。 ソース ディレクトリが存在しない場合に、構成ビルダーがエラーを発生するかどうかを指定します。

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> 決してのパスワードを保存、機密性の高い接続文字列、またはソース コード内の他の機密データにします。 運用シークレットを使用する必要があります、開発やテストします。

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

.NET core プロジェクトは、構成の JSON ファイルを頻繁に使用します。 [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/)ビルダーは、.NET Framework で使用する .NET Core の JSON ファイルを使用します。 この構成ビルダーは、.NET Framework の構成の特定のキー/値の領域にフラットなキー/値のソースからの基本的なマッピングを提供します。 この構成ビルダーが**いない**の階層の構成を提供します。 JSON ファイルは、複雑な階層オブジェクトではなく、ディクショナリに似ています。 複数レベルの階層的なファイルを使用できます。 このプロバイダー `flatten`s の各レベルを使用してプロパティ名を追加して深さ`:`区切り文字として。

属性の詳細:

* `jsonFile` - 必須。 読み取りに JSON ファイルを指定します。 `~`アプリのルートを参照する先頭の文字を使用できます。
* `optional` -ブール値、既定値は`true`します。 により、JSON ファイルが見つからない場合は、例外をスローします。
* `jsonMode` - `[Flat|Sectional]`. `Flat` が既定値です。 ときに`jsonMode`は`Flat`、JSON ファイルが 1 つのフラットなキー/値のソース。 `EnvironmentConfigBuilder`と`AzureKeyVaultConfigBuilder`も、1 つのフラットなキー/値のソース。 ときに、`SimpleJsonConfigBuilder`で構成されて`Sectional`モード。

  * JSON ファイルは概念的には、最上位レベルだけで複数のディクショナリに分かれています。
  * 各ディクショナリは、割り当てられている最上位のプロパティ名と一致する構成セクションにのみ適用します。 例えば:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>キー/値のカスタム構成ビルダーを実装します。

構成ビルダーが目的に合わない場合は、カスタムの 1 つを記述できます。 `KeyValueConfigBuilder`基底クラスが置換モードとプレフィックスのほとんどの問題を処理します。 実装するプロジェクトのみが必要です。

* 基本クラスから継承し、実装をキー/値ペアの基本的なソース、`GetValue`と`GetAllValues`:
* 追加、 [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/)をプロジェクトにします。

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

`KeyValueConfigBuilder`基本クラスには、職場で一貫性のある動作の多くキー/値の間で構成ビルダー。

## <a name="additional-resources"></a>その他の技術情報

* [構成ビルダーの GitHub リポジトリ](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [.NET を使用して Azure Key Vault へのサービス間認証](/azure/key-vault/service-to-service-authentication#connection-string-support)
