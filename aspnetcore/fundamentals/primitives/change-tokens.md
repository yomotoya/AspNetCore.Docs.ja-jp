---
title: "ASP.NET Core の変更のトークンを使用して変更を検出します。"
author: guardrex
description: "変更のトークンを使用して変更を追跡する方法を説明します。"
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 94bf356fcbfab3930804485c1b65e4a0f4c52b8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>ASP.NET Core の変更のトークンを使用して変更を検出します。

作成者: [Luke Latham](https://github.com/guardrex)

A*トークンを変更*変更の追跡に使用される汎用の低レベルの構成要素です。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="ichangetoken-interface"></a>IChangeToken インターフェイス

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)変更が発生したことの通知を伝達します。 `IChangeToken`存在する、 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives)名前空間。 使用していないアプリ、 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage、参照、 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)プロジェクト ファイルでの NuGet パッケージです。

`IChangeToken`2 つのプロパティがあります。

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks)トークン プロアクティブにコールバックを発生させるかどうかを示します。 場合`ActiveChangedCallbacks`に設定されている`false`コールバックが呼び出されないと、アプリをポーリングする必要があります、`HasChanged`の変更。 変更は行われません、基になる変更リスナーが破棄または無効になっている場合は、キャンセルすることはありませんトークンのこともできます。
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged)を変更が発生したかどうかを示す値を取得します。

このインターフェイスに 1 つのメソッドでは、 [RegisterChangeCallback (アクション&lt;オブジェクト&gt;、オブジェクト)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)トークンが変更されたときに呼び出されるコールバックを登録します。 `HasChanged`コールバックが呼び出される前に設定する必要があります。

## <a name="changetoken-class"></a>ChangeToken クラス

`ChangeToken`変更が発生したことの通知を伝達するために使用する静的クラスです。 `ChangeToken`存在する、 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives)名前空間。 使用していないアプリ、 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage、参照、 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)プロジェクト ファイルでの NuGet パッケージです。

`ChangeToken` [OnChange (Func&lt;IChangeToken&gt;、アクション)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_)メソッド レジスタ、`Action`を呼び出して、トークンが変更されるたびに。
* `Func<IChangeToken>`トークンを生成します。
* `Action`トークンが変更されたときに呼び出されます。

`ChangeToken`[OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;、アクション&lt;TState&gt;、TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_)オーバー ロードを追加`TState`トークンのコンシューマーに渡されるパラメーター`Action`です。

`OnChange`返します、 [IDisposable](/dotnet/api/system.idisposable)です。 呼び出す[Dispose](/dotnet/api/system.idisposable.dispose)さらに変更をリッスンしてから、トークンを停止し、トークンのリソースを解放します。

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>ASP.NET Core での変更のトークンの使用例

変更のトークンは、オブジェクトへの変更を監視する ASP.NET Core の目立つ領域で使用されます。

* ファイルへの変更の監視[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)の[ウォッチ](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch)メソッドを作成、`IChangeToken`指定したファイルまたはフォルダーを監視します。
* `IChangeToken`トークンは、変更のキャッシュ立ち退きをトリガーするキャッシュ エントリに追加することができます。
* `TOptions` 、既定値を変更します[OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1)の実装[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 1 つまたは複数を受け入れるオーバー ロードを持つ[IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)。インスタンス。 各インスタンスを返します、`IChangeToken`オプションの変更の追跡、変更通知のコールバックを登録します。

## <a name="monitoring-for-configuration-changes"></a>構成の変更の監視

既定では、ASP.NET Core テンプレートを使用して[JSON 構成ファイル](xref:fundamentals/configuration/index#json-configuration)(*される appsettings.json*、 *appsettings です。Development.json*、および*appsettings です。Production.json*) アプリの構成設定を読み込めません。

使用してこれらのファイルは、構成、 [AddJsonFile (IConfigurationBuilder、String、Boolean、ブール値)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_)拡張メソッドを[ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder)を受け入れる、`reloadOnChange`パラメーター (ASP.NETコア 1.1 以降)。 `reloadOnChange`ファイルの変更の構成を再読み込みするかどうかを示します。 この設定を参照してください、 [WebHost](/dotnet/api/microsoft.aspnetcore.webhost)便利なメソッド[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([参照ソース](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193))。

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

ファイル ベースの構成がによって表される[FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource)です。 `FileConfigurationSource`使用して[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([参照ソース](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) ファイルを監視します。

既定では、`IFileMonitor`によって提供される、 [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([参照ソース](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)) を使用する[FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher)構成ファイルを監視するには変更します。

サンプル アプリでは、構成の変更を監視するための 2 つの実装を示します。 どちらの場合、*される appsettings.json*ファイルまたは環境のバージョンのファイルが変更された、各実装は、カスタム コードを実行します。 サンプル アプリは、コンソールにメッセージを書き込みます。

構成ファイルの`FileSystemWatcher`1 つの構成ファイルの変更を複数のトークンのコールバックをトリガーできます。 サンプルの実装は、構成ファイルのファイル ハッシュをチェックして、この問題を防ぐ。 カスタム コードを実行する前に、構成ファイルの少なくとも 1 つが変更されたことにより、ファイル ハッシュを確認しています。 このサンプルでは、SHA1 ファイルのハッシュでは (*Utilities/Utilities.cs*)。

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   再試行は、指数バックオフで実装されます。 再実行は、ファイルのロックが発生する一時的に妨げるファイルのいずれかで新しいハッシュを計算するために存在します。

### <a name="simple-startup-change-token"></a>シンプルなスタートアップ変更トークン

トークンのコンシューマーを登録`Action`構成の再読み込みトークンへのコールバック変更通知 (*Startup.cs*)。

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()`トークンを提供します。 コールバックを`InvokeChanged`メソッド。

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

`state`に渡すコールバックに使用される、`IHostingEnvironment`です。 これは、正しいを調べるのに役立ちます*appsettings*構成 JSON ファイルを監視する*appsettings&lt; 。環境&gt;.json*です。 ファイルのハッシュを防ぐために使用される、`WriteConsole`ステートメントの構成ファイルが 1 回のみ変更されたときの複数のトークン コールバックにより複数回実行します。

このシステムを実行する限り、アプリが実行されていると、ユーザーを無効にすることはできません。

### <a name="monitoring-configuration-changes-as-a-service"></a>サービスとしての構成の変更の監視

このサンプルを実装します。

* 基本的なスタートアップ トークンを監視します。
* サービスを監視します。
* 有効にして、監視を無効にするメカニズム。

このサンプルの確立、`IConfigurationMonitor`インターフェイス (*Extensions/ConfigurationMonitor.cs*)。

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

実装済みのクラスのコンス トラクター `ConfigurationMonitor`、変更通知のコールバックを登録します。

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()`トークンを提供します。 `InvokeChanged`コールバック メソッドです。 `state`がこのインスタンスでは、監視の状態を説明する文字列。 2 つのプロパティが使用されます。

* `MonitoringEnabled`コールバックが、カスタム コードを実行する必要があるかどうかを示します。
* `CurrentState`UI で使用するための現在の監視状態を説明します。

`InvokeChanged`メソッドは点を除けば、前の方法に似ています。

* しない限り、そのコードを実行しない`MonitoringEnabled`は`true`します。
* セット、`CurrentState`コードを実行した時刻を記録する説明メッセージにプロパティの文字列。
* 現在のノート`state`でその`WriteConsole`出力します。

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

インスタンス`ConfigurationMonitor`でサービスとして登録されて`ConfigureServices`の*Startup.cs*:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

インデックス ページは、ユーザーを制御する構成の監視を提供します。 インスタンス`IConfigurationMonitor`に挿入された、 `IndexModel`:

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

ボタンを有効にし、監視を無効に。

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

ときに`OnPostStartMonitoring`がトリガーされると、監視が有効になっているし、現在の状態がオフになっています。 ときに`OnPostStopMonitoring`がトリガーされると、監視は無効になり、状態が監視されていない行われていることを反映するように設定します。

## <a name="monitoring-cached-file-changes"></a>キャッシュされたファイルの変更の監視

ファイルの内容がメモリ内のキャッシュを使用することができます[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)です。 メモリ内キャッシュに記載されて、[のメモリ内キャッシュ](xref:performance/caching/memory)トピックです。 以下に示す実装など、追加の手順を考慮せず*古い*ソース データが変更された場合 (古い) データがキャッシュから返されます。

いないを考慮して、キャッシュされたソース ファイルの状態を更新するときに、[スライディング期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間は、古いデータをキャッシュにつながります。 データの各要求が、スライド式有効期限を更新しますが、ファイルがキャッシュに再読み込みされません。 ファイルのキャッシュされたコンテンツを使用するアプリの機能は、可能性のある古いコンテンツが受信されるとします。

シナリオをキャッシュ ファイルの変更のトークンを使用して、キャッシュの内容を古いファイルができなくなります。 サンプル アプリでは、アプローチの実装を示します。

このサンプルでは`GetFileContent`に。

* ファイルの内容を返します。
* カバーの場合、ファイルのロックが一時的に妨げられてファイル読み取り中に指数バックオフでの再試行アルゴリズムを実装します。

*Utilities/Utilities.cs*:

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

A`FileService`がキャッシュされたファイル参照の処理を作成します。 `GetFileContent`メモリ内キャッシュからファイル コンテンツを取得して、呼び出し元に戻すしようとサービスのメソッドの呼び出し (*Services/FileService.cs*)。

キャッシュされたコンテンツが見つからない場合、キャッシュ キーを使用して、次の操作は取得されます。

1. 使用して、ファイルの内容を取得`GetFileContent`です。
1. 変更トークンは、プロバイダーから取得した、ファイルと[IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch)です。 ファイルが変更されたときに、トークンのコールバックがトリガーされます。
1. ファイルの内容はと共にキャッシュされる、[スライディング期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間。 変更のトークンが割り当てられて[MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken)がキャッシュされているときに、ファイルが変更された場合、キャッシュ エントリを削除します。

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService`がメモリのキャッシュ サービスと共にサービス コンテナーに登録 (*Startup.cs*)。

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

ページのモデルは、サービスを使用して、ファイルの内容を読み込みます (*Pages/Index.cshtml.cs*)。

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken クラス

表す 1 つまたは複数の`IChangeToken`1 つのオブジェクトのインスタンスを使用して、 [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken)クラス ([参照ソース](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs))。

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged`複合トークン レポートに対して`true`トークンを表すいずれかの場合は`HasChanged`は`true`します。 `ActiveChangeCallbacks`複合トークン レポートに対して`true`トークンを表すいずれかの場合は`ActiveChangeCallbacks`は`true`します。 複数の同時変更イベントが発生すると、正確に 1 回、複合変更コールバックが呼び出されます。

## <a name="see-also"></a>関連項目

* [メモリ内キャッシュ](xref:performance/caching/memory)
* [分散キャッシュの使用](xref:performance/caching/distributed)
* [変更トークンを使用する変更の検出](xref:fundamentals/primitives/change-tokens)
* [応答キャッシュ](xref:performance/caching/response)
* [応答キャッシュ ミドルウェア](xref:performance/caching/middleware)
* [キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
