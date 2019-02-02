---
title: ASP.NET Core での開発中のアプリ シークレットの安全な格納
author: rick-anderson
description: 保存して、アプリ シークレットと ASP.NET Core アプリの開発中の機密情報を取得する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2019
uid: security/app-secrets
ms.openlocfilehash: eaa2e9d1ba98d391a29a9ff55872d062df016b87
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667779"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>ASP.NET Core での開発中のアプリ シークレットの安全な格納

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、および[Scott Addie](https://github.com/scottaddie)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

このドキュメントでは、格納して、ASP.NET Core アプリの開発時に機密データを取得するための手法について説明します。 ソース コードでパスワードや他の機密データを保存しないでください。 運用シークレットを使用してはならない開発やテストします。 [Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration)により、Azure テストと運用のシークレットを格納し、保護することが可能です。

## <a name="environment-variables"></a>環境変数

環境変数は、コードまたはローカルの構成ファイルでのアプリ シークレットのストレージを回避するために使用されます。 環境変数は、以前に指定した構成のすべてのソースの構成値をオーバーライドします。

::: moniker range="<= aspnetcore-1.1"

環境変数の値の読み取りを呼び出すことによって構成[AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)で、`Startup`コンス トラクター。

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

ASP.NET Core web アプリを検討してください**個々 のユーザー アカウント**セキュリティを有効にします。 プロジェクトの既定のデータベース接続文字列が含まれている*appsettings.json*キーを持つファイル`DefaultConnection`します。 既定の接続文字列は、LocalDB では、ユーザー モードで実行し、パスワードを必要としないからです。 アプリのデプロイ時に、`DefaultConnection`キーの値を環境変数の値で上書きすることができます。 環境変数には、機密の資格情報の完全な接続文字列を格納できます。

> [!WARNING]
> 環境変数は、暗号化されていないプレーン テキストで一般的に格納されます。 コンピューターまたはプロセスが侵害された場合、環境変数は、信頼されていないパーティによってアクセスできます。 ユーザーの機密情報の漏えいを防ぐためにその他の対策は、必要な可能性があります。

## <a name="secret-manager"></a>Secret Manager

Secret Manager ツールは、ASP.NET Core プロジェクトの開発時に機密データを格納します。 このコンテキストでは、機密データの一部は、アプリのシークレットは。 アプリ シークレットは、プロジェクト ツリーから別の場所に格納されます。 アプリ シークレットは、特定のプロジェクトに関連付けられているか、いくつかのプロジェクト間で共有します。 アプリ シークレットは、ソース管理にチェックインされません。

> [!WARNING]
> Secret Manager ツールでは、保存されたシークレットを暗号化しないし、信頼できるストアとして扱うべきではありません。 開発目的でのみです。 キーと値は、ユーザー プロファイル ディレクトリ内の JSON 構成ファイルに格納されます。

## <a name="how-the-secret-manager-tool-works"></a>Secret Manager ツールのしくみ

値が格納されている場所と方法など、実装の詳細を抽象 Secret Manager ツール。 これらの実装の詳細を知ることがなくツールを使用することができます。 値は、ローカル コンピューターのシステムで保護されているユーザーのプロファイル フォルダーに JSON 構成ファイルに格納されます。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ファイル システム パス:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ファイル システム パス:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ファイル システム パス:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

上記の「ファイル パスは、置換`<user_secrets_id>`で、`UserSecretsId`で指定された値、 *.csproj*ファイル。

場所または Secret Manager ツールを使用して保存データの形式に依存するコードを記述しません。 これらの実装の詳細を変更することがあります。 たとえば、シークレットの値は暗号化されませんが、今後可能性があります。

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Secret Manager ツールをインストールします。

Secret Manager ツールは、.NET Core SDK 2.1.300 で .NET Core CLI を使用してバンドル以降です。 2.1.300 より前に、のバージョンを .NET Core SDK、ツールのインストールする必要があります。

> [!TIP]
> 実行`dotnet --version`からコマンド シェルをインストール済みの .NET Core SDK バージョン番号を参照してください。

使用されている .NET Core SDK には、ツールが含まれている場合は、警告が表示されます。

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

インストール、 [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core プロジェクトで NuGet パッケージ。 例:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

ツールのインストールを検証するコマンド シェルで次のコマンドを実行します。

```console
dotnet user-secrets -h
```

Secret Manager ツールには、サンプルの使用法、オプション、およびコマンドのヘルプが表示されます。

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> 同じディレクトリである必要があります、 *.csproj*ファイルで定義されているツールを実行する、 *.csproj*ファイルの`DotNetCliToolReference`要素。

::: moniker-end

## <a name="set-a-secret"></a>シークレットを設定します。

Secret Manager ツールは、ユーザー プロファイルに格納されているプロジェクト固有の構成設定では動作します。 ユーザー シークレットを使用する定義を`UserSecretsId`内の要素を`PropertyGroup`の *.csproj*ファイル。 値`UserSecretsId`は任意ですが、プロジェクトに一意です。 開発者は通常の GUID を生成、`UserSecretsId`します。

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> Visual Studio では、ソリューション エクスプ ローラーでプロジェクトを右クリックして**ユーザー シークレットの管理**コンテキスト メニュー。 このジェスチャを追加、`UserSecretsId`に GUID が設定されている要素、 *.csproj*ファイル。 Visual Studio を開き、 *secrets.json*テキスト エディターでファイル。 内容を置き換える*secrets.json*キーと値のペアを格納するとします。 例:
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> 使用して変更した後、JSON の構造がフラット化`dotnet user-secrets remove`または`dotnet user-secrets set`します。 たとえば、実行している`dotnet user-secrets remove "Movies:ConnectionString"`折りたたみます、`Movies`オブジェクト リテラル。 変更したファイルには、次のようになります。
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

キーとその値から成るアプリのシークレットを定義します。 シークレットは、プロジェクトの関連付け`UserSecretsId`値。 たとえば、先のディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

前の例では、コロンのあることを示します`Movies`はオブジェクトのリテラルを`ServiceApiKey`プロパティ。

Secret Manager ツールは、その他のディレクトリからも使用できます。 使用して、`--project`をファイル システム パスを指定するオプション、 *.csproj*ファイルが存在します。 例:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>複数のシークレットを設定します。

JSON をパイプしてシークレットのバッチを設定することができます、`set`コマンド。 次の例では、 *input.json*ファイルの内容はパイプを使用して、`set`コマンド。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

コマンド シェルを開き、次のコマンドを実行します。

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

コマンド シェルを開き、次のコマンドを実行します。

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

コマンド シェルを開き、次のコマンドを実行します。

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>シークレットにアクセスします。

::: moniker range=">= aspnetcore-2.0"

[ASP.NET Core 構成 API](xref:fundamentals/configuration/index) Secret Manager シークレットへのアクセスを提供します。 プロジェクトが .NET Framework を対象とする場合は、インストール、 [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet パッケージ。

ASP.NET Core 2.0 以降では、ユーザー シークレット構成ソースが自動的に追加の開発モードで、プロジェクトを呼び出すと<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>事前構成済みの既定値を持つホストの新しいインスタンスを初期化します。 `CreateDefaultBuilder` 呼び出し<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>ときに、<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>は<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

ときに`CreateDefaultBuilder`いない呼び出すことによって、ユーザー シークレット構成ソースを明示的に追加呼び出されると、<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*>で、`Startup`コンス トラクター。 呼び出す`AddUserSecrets`のみ実行すると、アプリ開発環境で次の例に示すようにします。

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[ASP.NET Core 構成 API](xref:fundamentals/configuration/index) Secret Manager シークレットへのアクセスを提供します。 インストール、 [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet パッケージ。

呼び出してユーザー シークレット構成ソースの追加[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)で、`Startup`コンス トラクター。

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

ユーザー シークレットを取得できます、 `Configuration` API:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>POCO に機密情報をマップします。

POCO (単純な .NET クラスのプロパティを持つ) への全体のオブジェクト リテラルのマッピングは、関連するプロパティを集約するために役立ちます。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

上記のシークレットを POCO をマップするには使用、 `Configuration` API の[オブジェクト グラフのバインド](xref:fundamentals/configuration/index#bind-to-an-object-graph)機能します。 次のコードにカスタム バインド`MovieSettings`POCO およびアクセス、`ServiceApiKey`プロパティの値。

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

`Movies:ConnectionString`と`Movies:ServiceApiKey`シークレットは、それぞれのプロパティにマップされます`MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>秘密情報の文字列の置換

プレーン テキストでパスワードを格納することは安全ではありません。 たとえば、データベース接続文字列に格納されている*appsettings.json*指定したユーザーのパスワードを含めることができます。

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

安全なアプローチでは、パスワードをシークレットとして格納します。 例:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

削除、`Password`キー/値ペア内の接続文字列から*appsettings.json*します。 例:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

シークレットの値を設定することができます、 [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder)オブジェクトの[パスワード](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password)接続文字列を完了するプロパティ。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>シークレットの一覧表示します。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。

```console
dotnet user-secrets list
```

次の出力が表示されます。

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

前の例では、キー名にコロンが内のオブジェクト階層を示しています。 *secrets.json*します。

## <a name="remove-a-single-secret"></a>1 つのシークレットを削除します。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

アプリの*secrets.json*に関連付けられているキー/値ペアを削除するファイルが変更された、`MoviesConnectionString`キー。

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

実行している`dotnet user-secrets list`次のメッセージが表示されます。

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>すべてのシークレットを削除します。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。

```console
dotnet user-secrets clear
```

アプリのすべてのユーザーの機密情報が削除されて、 *secrets.json*ファイル。

```json
{}
```

実行している`dotnet user-secrets list`次のメッセージが表示されます。

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>その他の技術情報

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
