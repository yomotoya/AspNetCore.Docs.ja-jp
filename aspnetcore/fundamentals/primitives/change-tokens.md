---
title: ASP.NET Core で変更トークンを使用して変更を検出する
author: guardrex
description: 変更トークンを使用して変更を追跡する方法を説明します。
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 06751e713fbd579a944333cc3c3b2c0c0ad51eba
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>ASP.NET Core で変更トークンを使用して変更を検出する

作成者: [Luke Latham](https://github.com/guardrex)

*変更トークン*は、変更の追跡に使用される汎用の低レベルの構成ブロックです。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="ichangetoken-interface"></a>IChangeToken インターフェイス

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) は、変更が発生したことの通知を伝達します。 `IChangeToken` は、[Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 名前空間に含まれています。 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) メタパッケージを使用しないアプリケーションの場合、プロジェクト ファイル内の [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet パッケージを参照します。

`IChangeToken` に次の 2 つのプロパティがあります。

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) は、トークンがプロアクティブにコールバックを発生させるかどうかを示します。 `ActiveChangedCallbacks` が `false` に設定されている場合、コールバックは呼び出されず、アプリは変更のために `HasChanged` をポーリングする必要があります。 変更がまったく発生しない場合、または基になる変更リスナーが破棄されるか無効になっている場合、トークンがまったくキャンセルされない可能性もあります。
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) は、変更が発生したかどうかを示す値を取得します。

このインターフェイスには、[RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback) という 1 つのメソッドがあります。このメソッドは、トークンが変更されたときに呼び出されるコールバックを登録します。 コールバックが呼び出される前に `HasChanged` を設定する必要があります。

## <a name="changetoken-class"></a>ChangeToken クラス

`ChangeToken` は、変更が発生したことの通知を伝達するために使用される静的クラスです。 `ChangeToken` は、[Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 名前空間に含まれています。 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) メタパッケージを使用しないアプリケーションの場合、プロジェクト ファイル内の [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet パッケージを参照します。

`ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) メソッドは、トークンが変更されるたびに呼び出される `Action` を登録します。
* `Func<IChangeToken>` はトークンを生成します。
* `Action` は、トークンが変更されたときに呼び出されます。

`ChangeToken` には、[OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) オーバーロードがあり、これはトークン コンシューマー `Action` に渡される追加の `TState` パラメーターを取ります。

`OnChange` は、[IDisposable](/dotnet/api/system.idisposable) を返します。 [Dispose](/dotnet/api/system.idisposable.dispose) を呼び出すと、トークンがその後の変更のリッスンを停止し、トークンのリソースを解放します。

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>ASP.NET Core での変更のトークンの使用例

変更トークンは、オブジェクトの変更を監視する ASP.NET Core の主要な領域で使用されます。

* ファイルへの変更を監視するために、[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) の [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) メソッドは、監視するために指定されたファイルまたはフォルダーの `IChangeToken` を作成します。
* `IChangeToken` トークンをキャッシュ エントリに追加して、変更時にキャッシュの削除をトリガーすることができます。
* `TOptions` の変更のために、[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) の既定の [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) の実装には、1 つ以上の [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) インスタンスを受け入れるオーバーロードがあります。 各インスタンスは、オプションの変更を追跡するために変更通知のコールバックを登録する `IChangeToken` を返します。

## <a name="monitoring-for-configuration-changes"></a>構成の変更の監視

既定では、ASP.NET Core テンプレートは、[JSON 構成ファイル](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*、*appsettings.Development.json*、および *appsettings.Production.json*) を使用して、アプリの構成設定を読み込みます。

これらのファイルは、`reloadOnChange` パラメーター (ASP.NET Core 1.1 以降) を受け入れる [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 上で [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) 拡張メソッドを使用して構成されます。 `reloadOnChange` は、ファイルの変更時に構成を再読み込みするかどうかを示します。 [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) の簡易メソッド [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) でこの設定を参照します。

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

ファイル ベースの構成は、[FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource) によって表されます。 `FileConfigurationSource` はファイルのモニターに、[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) を使用します。

既定では、`IFileMonitor` は [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) によって提供されます。これは、[FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) を使用して、構成ファイルの変更を監視します。

サンプル アプリは、構成の変更を監視するための 2 つの実装を示します。 *appsettings.json* ファイルが変更されるか、ファイルの環境バージョンが変更された場合、各実装はカスタム コードを実行します。 サンプル アプリは、コンソールにメッセージを書き込みます。

構成ファイルの `FileSystemWatcher` は、1 つの構成ファイルの変更に対して複数のトークンのコールバックをトリガーできます。 サンプルの実装は、構成ファイルのファイル ハッシュをチェックして、この問題を防ぎます。 ファイル ハッシュを確認することで、カスタム コードを実行する前に、構成ファイルの少なくとも 1 つが変更されたことを確認します。 このサンプルでは、SHA1 ファイル ハッシュ (*Utilities/Utilities.cs*) を使用します。

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   再試行は、指数バックオフを使用して実装されます。 再実行が提供されるのは、いずれかのファイルで新しいハッシュの計算を一時的に妨げるファイルのロックが発生する場合があるためです。

### <a name="simple-startup-change-token"></a>シンプルなスタートアップ変更トークン

構成再読み込みトークン (*Startup.cs*) への変更通知のためにトークン コンシューマー `Action` のコールバックを登録します。

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` はトークンを提供します。 コールバックは、`InvokeChanged` メソッドです。

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

`IHostingEnvironment` で渡すためにコールバックの `state` が使用されます。 これは、監視する正しい *appsettings* 構成 JSON ファイル *appsettings.&lt;Environment&gt;.json* を調べるために役立ちます。 ファイル ハッシュは、構成ファイルが 1 回のみ変更されたときの複数のトークンのコールバックのために `WriteConsole` ステートメントが複数回実行されるのを防ぐために使用されます。

このシステムは、アプリが実行されている限り実行され、ユーザーが無効にすることはできません。

### <a name="monitoring-configuration-changes-as-a-service"></a>サービスとしての構成の変更の監視

サンプルは次のものを実装します。

* 基本的なスタートアップ トークンの監視
* サービスとしての監視
* 監視を有効および無効にするメカニズム。

このサンプルは、`IConfigurationMonitor` インターフェイス (*Extensions/ConfigurationMonitor.cs*) を確立します。

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

実装済みのクラスのコンストラクター `ConfigurationMonitor` は、変更通知のコールバックを登録します。

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` はトークンを提供します。 `InvokeChanged` はコールバック メソッドです。 このインスタンスの `state` は、監視の状態にアクセスするために使用される `IConfigurationMonitor` インスタンスへの参照です。 次の 2 つのプロパティが使用されます。

* `MonitoringEnabled` は、コールバックでカスタム コードを実行する必要があるかどうかを示します。
* `CurrentState` は、UI で使用するための現在の監視状態を説明します。

`InvokeChanged` メソッドは、次の点を除けば、前の方法に似ています。

* `MonitoringEnabled` が `true` ではない限りコードを実行しません。
* `WriteConsole` 出力の現在の `state` に注意してください。

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

インスタンス `ConfigurationMonitor` は、*Startup.cs* の `ConfigureServices` でサービスとして登録されます。

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

インデックス ページは、ユーザーに構成監視の制御を提供します。 `IConfigurationMonitor` のインスタンスは、`IndexModel` に挿入されます。

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

ボタンによって監視を有効および無効にします。

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

`OnPostStartMonitoring` がトリガーされると、監視が有効になり、現在の状態がクリアされます。 `OnPostStopMonitoring` がトリガーされると、監視が無効になり、監視が発生していないことを反映するように状態が設定されます。

## <a name="monitoring-cached-file-changes"></a>キャッシュされたファイルの変更の監視

[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) を使用して、ファイルの内容をメモリ内にキャッシュすることができます。 メモリ内キャッシュの説明については、「[メモリ内キャッシュ](xref:performance/caching/memory)」トピックを参照してください。 以下に示す実装など、追加の手順を実行しないと、ソース データが変更された場合に*期限切れの* (古い) データがキャッシュから返されます。

[スライド式有効期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間の更新時にキャッシュされたソース ファイルのステータスを考慮しないと、古いキャッシュ データにつながります。 データの各要求が、スライド式有効期限を更新しますが、ファイルがキャッシュに再読み込みされることはありません。 ファイルのキャッシュされた内容を使用するアプリの機能は、古い内容を受け取る可能性があります。

ファイル キャッシュのシナリオで変更トークンを使用すると、キャッシュのファイルの内容が古くなるのを防止できます。 サンプル アプリは、このアプローチの実装を示しています。

このサンプルでは、`GetFileContent` を使用して次の操作を実行します。

* ファイルの内容を返します。
* 指数バックオフを使用する再試行アルゴリズムを実装し、ファイル ブロックがファイルの読み取りを一時的に妨げるケースに対処します。

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

`FileService` はキャッシュされたファイルの参照の処理するために作成されます。 サービスの `GetFileContent` メソッドの呼び出しは、メモリ内キャッシュからファイルの内容を取得して、呼び出し元 (*Services/FileService.cs*) に戻そうとします。

キャッシュ キーを使用してキャッシュされた内容が見つからない場合、次の操作が実行されます。

1. `GetFileContent` を使用してファイルの内容が取得されます。
1. 変更トークンは、[IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) を使用してファイル プロバイダーから取得します。 ファイルが変更されたときに、トークンのコールバックがトリガーされます。
1. ファイルの内容は、[スライド式有効期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)の期間キャシュされます。 ファイルがキャッシュされている間に変更された場合、キャッシュ エントリを削除するために、[MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) を使用して変更トークンがアタッチされます。

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` がメモリ キャッシュ サービス (*Startup.cs*) と共にサービス コンテナーに登録されます。

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

ページのモデルは、サービス (*Pages/Index.cshtml.cs*) を使用して、ファイルの内容を読み込みます。

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken クラス

1 つのオブジェクト内で 1 つまたは複数の `IChangeToken` インスタンスを表すために、[CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) クラスを使用します。

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

複合トークンの `HasChanged` は、いずれかの表現されているトークン `HasChanged` が `true` である場合に、`true` を報告します。 複合トークンの `ActiveChangeCallbacks` は、いずれかの表現されているトークン `ActiveChangeCallbacks` が `true` である場合に、`true` を報告します。 複数の同時変更イベントが発生すると、複合変更コールバックが正確に 1 回呼び出されます。

## <a name="see-also"></a>関連項目

* [メモリ内キャッシュ](xref:performance/caching/memory)
* [分散キャッシュの使用](xref:performance/caching/distributed)
* [応答キャッシュ](xref:performance/caching/response)
* [応答キャッシュ ミドルウェア](xref:performance/caching/middleware)
* [キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
