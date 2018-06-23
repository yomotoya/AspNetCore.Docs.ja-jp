---
title: アプリ シークレットは、ASP.NET Core での開発での安全な格納場所
author: rick-anderson
description: 保存し、アプリ シークレットは、ASP.NET Core アプリケーションの開発時に重要な情報を取得する方法を説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2018
uid: security/app-secrets
ms.openlocfilehash: d3b2de1a17012986ef8dea7aaf8636dd35d10fa1
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314176"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>アプリ シークレットは、ASP.NET Core での開発での安全な格納場所

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、および[Scott Addie](https://github.com/scottaddie)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

このドキュメントでは、格納および ASP.NET Core アプリケーションの開発中に機密データを取得するためのテクニックについて説明します。 ソース コードで、パスワードや他の機密データを格納する必要がありますしない、モードをテストまたは開発で実稼働の機密情報を使用することはできません。 格納しと Azure のテストおよび運用シークレットを保護することができます、 [Azure Key Vault の構成プロバイダー](xref:security/key-vault-configuration)です。

## <a name="environment-variables"></a>環境変数

環境変数は、アプリ シークレットは、コードまたはでローカルの構成ファイルの格納を避けるために使用されます。 環境変数は、以前に指定した構成のすべてのソースの構成値をオーバーライドします。

::: moniker range="<= aspnetcore-1.1"
環境変数の値の読み取りを呼び出すことによって構成[AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables)で、`Startup`コンス トラクター。

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

ASP.NET Core web アプリを検討してください**個々 のユーザー アカウント**セキュリティが有効にします。 プロジェクトの既定のデータベース接続文字列が含まれている*される appsettings.json*キーを持つファイル`DefaultConnection`です。 既定の接続文字列では、LocalDB では、ユーザー モードで実行され、パスワードを必要としないのです。 アプリの展開中に、`DefaultConnection`キーの値を環境変数の値で上書きすることができます。 環境変数には、機密性の高い資格情報を持つ完全な接続文字列を格納できます。

> [!WARNING]
> 環境変数は、暗号化されていないプレーン テキストに一般的に格納されます。 コンピューターまたはプロセスが侵害された場合、環境変数は、信頼されていないパーティによってアクセスできます。 追加の手段をユーザーの機密情報の漏えいを防ぐためには、必要な可能性があります。

## <a name="secret-manager"></a>シークレット マネージャー

シークレット マネージャー ツールは、ASP.NET Core プロジェクトの開発中に機密データを格納します。 このコンテキストでは、機密データの一部は、アプリのシークレットがします。 アプリ シークレットは、プロジェクト ツリーから別の場所に格納されます。 アプリ シークレットは、特定のプロジェクトに関連付けられているか、いくつかのプロジェクト間で共有します。 アプリ シークレットは、ソース管理にチェックインされません。

> [!WARNING]
> シークレット マネージャー ツールは、保存されたシークレットを暗号化しないし、信頼できるストアとして扱われるべきではありません。 これは、開発目的でのみです。 キーと値は、ユーザーのプロファイル ディレクトリに JSON 構成ファイルに格納されます。

## <a name="how-the-secret-manager-tool-works"></a>シークレット マネージャー ツールの動作

値が格納されている場所と方法など、実装の詳細を抽象シークレット Manager ツール。 これらの実装の詳細を知ることがなくツールを使用することができます。 値は、ローカル コンピューター上のシステムで保護されたユーザー プロファイル フォルダーに JSON 構成ファイルに格納されます。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ファイル システム パス。

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ファイル システム パス。

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ファイル システム パス。

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

上記の「ファイル パス、それぞれ置き換えます`<user_secrets_id>`で、`UserSecretsId`で指定された値、 *.csproj*ファイル。

場所またはシークレット マネージャー ツールを使用して保存されたデータの形式に依存するコードを記述しません。 これらの実装の詳細を変更することがあります。 たとえば、秘密の値は暗号化されませんが、後である可能性があります。

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>シークレット マネージャー ツールをインストールします。

シークレット マネージャー ツールには、.NET Core SDK 2.1.300 時点で、.NET Core CLI が付属しています。 2.1.300 より前に、のバージョンを .NET Core SDK、ツールのインストールする必要があります。

> [!TIP]
> 実行`dotnet --version`インストールされている .NET Core SDK バージョン番号を表示するコマンド シェルからです。

使用されている、.NET Core SDK には、ツールが含まれている場合は、警告が表示されます。

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

インストール、 [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core プロジェクトでの NuGet パッケージです。 例えば:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

ツールのインストールを検証するコマンド シェルで、次のコマンドを実行します。

```console
dotnet user-secrets -h
```

シークレット マネージャー ツールには、サンプルの使用方法、オプション、およびコマンドのヘルプが表示されます。

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
> 同じディレクトリにする必要があります、 *.csproj*で定義されているツールを実行するファイル、 *.csproj*ファイルの`DotNetCliToolReference`要素。
::: moniker-end

## <a name="set-a-secret"></a>シークレットを設定します。

シークレット マネージャー ツールは、ユーザー プロファイルに格納されているプロジェクトに固有の構成設定で動作します。 ユーザーの機密情報を使用して、定義、`UserSecretsId`内の要素、`PropertyGroup`の *.csproj*ファイル。 値`UserSecretsId`任意で、プロジェクトに固有です。 開発者が通常の GUID を生成、`UserSecretsId`です。

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> Visual Studio で、ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**管理ユーザーの機密情報**コンテキスト メニュー。 このジェスチャを追加、`UserSecretsId`要素、GUID に設定、 *.csproj*ファイル。 Visual Studio を開き、 *secrets.json*ファイル テキスト エディターでします。 内容を置き換える*secrets.json*を格納するキー/値ペアにします。 例: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

キーとその値で構成されるアプリのシークレットを定義します。 シークレットは、プロジェクトの関連付けられた`UserSecretsId`値。 たとえば、次のコマンドを実行するディレクトリから、 *.csproj*ファイルが存在します。

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

この例では、コロンのあることを意味`Movies`オブジェクトでリテラルでは、`ServiceApiKey`プロパティです。

シークレット マネージャー ツールは、その他のディレクトリからも使用できます。 使用して、`--project`をファイル システム パスを指定するオプション、 *.csproj*ファイルが存在します。 例えば:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>複数の機密情報を設定します。

JSON をパイプして機密データのバッチを設定することができます、`set`コマンド。 次の例で、 *input.json*ファイルの内容は、パイプを使用して、`set`コマンド。

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

::: moniker range="<= aspnetcore-1.1"
[ASP.NET Core の構成 API](xref:fundamentals/configuration/index)シークレット Manager の機密情報へのアクセスを提供します。 インストール、 [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet パッケージです。

呼び出しには、ユーザーの機密情報構成ソースを追加[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)で、`Startup`コンス トラクター。

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[ASP.NET Core の構成 API](xref:fundamentals/configuration/index)シークレット Manager の機密情報へのアクセスを提供します。 プロジェクトは .NET Framework をターゲットの場合は、インストール、 [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet パッケージです。

ASP.NET Core 2.0 以降では、ユーザーの機密情報の構成ソースが自動的に追加の開発モードで、プロジェクトを呼び出すと[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)を事前に構成された既定値を持つホストの新しいインスタンスを初期化します。 `CreateDefaultBuilder` 呼び出し[AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)ときに、 [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)は[開発](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

ときに`CreateDefaultBuilder`いないへの呼び出しには、ユーザーの機密情報構成ソースを追加するホストの構築時に呼び出されると、 [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets)で、`Startup`コンス トラクター。

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

ユーザーの機密情報を取得できます、 `Configuration` API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>シークレットの文字列の置換

パスワードがプレーン テキストで格納することは、安全ではありません。 たとえば、データベース接続文字列の格納*される appsettings.json*指定したユーザーのパスワードを含めることができます。

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

安全な方法では、シークレットとしてパスワードを格納します。 例えば:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

削除、`Password`キー/値ペア内の接続文字列から*される appsettings.json*です。 例えば:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

シークレットの値を設定することができます、 [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder)オブジェクトの[パスワード](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password)するには、接続文字列のプロパティ。

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]
::: moniker-end

## <a name="list-the-secrets"></a>シークレットを一覧します。

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

ディレクトリから次のコマンドを実行、 *.csproj*ファイルが存在します。

```console
dotnet user-secrets list
```

次の出力が表示されます。

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

前の例では、キー名にコロンは内でオブジェクト階層を表します。 *secrets.json*です。

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

アプリのすべてのユーザーの機密情報はから削除されている、 *secrets.json*ファイル。

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
