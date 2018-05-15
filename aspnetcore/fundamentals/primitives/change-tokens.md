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
ms.openlocfilehash: 3055eec91adc412b596d4cc73e8523e18ff63331
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/19/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="6668a-103">ASP.NET Core で変更トークンを使用して変更を検出する</span><span class="sxs-lookup"><span data-stu-id="6668a-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="6668a-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6668a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6668a-105">*変更トークン*は、変更の追跡に使用される汎用の低レベルの構成ブロックです。</span><span class="sxs-lookup"><span data-stu-id="6668a-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="6668a-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="6668a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="6668a-107">IChangeToken インターフェイス</span><span class="sxs-lookup"><span data-stu-id="6668a-107">IChangeToken interface</span></span>

<span data-ttu-id="6668a-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) は、変更が発生したことの通知を伝達します。</span><span class="sxs-lookup"><span data-stu-id="6668a-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="6668a-109">`IChangeToken` は、[Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 名前空間に含まれています。</span><span class="sxs-lookup"><span data-stu-id="6668a-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="6668a-110">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) メタパッケージを使用しないアプリケーションの場合、プロジェクト ファイル内の [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet パッケージを参照します。</span><span class="sxs-lookup"><span data-stu-id="6668a-110">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="6668a-111">`IChangeToken` に次の 2 つのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="6668a-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="6668a-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) は、トークンがプロアクティブにコールバックを発生させるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="6668a-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="6668a-113">`ActiveChangedCallbacks` が `false` に設定されている場合、コールバックは呼び出されず、アプリは変更のために `HasChanged` をポーリングする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6668a-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="6668a-114">変更がまったく発生しない場合、または基になる変更リスナーが破棄されるか無効になっている場合、トークンがまったくキャンセルされない可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="6668a-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="6668a-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) は、変更が発生したかどうかを示す値を取得します。</span><span class="sxs-lookup"><span data-stu-id="6668a-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="6668a-116">このインターフェイスには、[RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback) という 1 つのメソッドがあります。このメソッドは、トークンが変更されたときに呼び出されるコールバックを登録します。</span><span class="sxs-lookup"><span data-stu-id="6668a-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="6668a-117">コールバックが呼び出される前に `HasChanged` を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6668a-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="6668a-118">ChangeToken クラス</span><span class="sxs-lookup"><span data-stu-id="6668a-118">ChangeToken class</span></span>

<span data-ttu-id="6668a-119">`ChangeToken` は、変更が発生したことの通知を伝達するために使用される静的クラスです。</span><span class="sxs-lookup"><span data-stu-id="6668a-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="6668a-120">`ChangeToken` は、[Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) 名前空間に含まれています。</span><span class="sxs-lookup"><span data-stu-id="6668a-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="6668a-121">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) メタパッケージを使用しないアプリケーションの場合、プロジェクト ファイル内の [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet パッケージを参照します。</span><span class="sxs-lookup"><span data-stu-id="6668a-121">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="6668a-122">`ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) メソッドは、トークンが変更されるたびに呼び出される `Action` を登録します。</span><span class="sxs-lookup"><span data-stu-id="6668a-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>
* <span data-ttu-id="6668a-123">`Func<IChangeToken>` はトークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="6668a-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="6668a-124">`Action` は、トークンが変更されたときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="6668a-125">`ChangeToken` には、[OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) オーバーロードがあり、これはトークン コンシューマー `Action` に渡される追加の `TState` パラメーターを取ります。</span><span class="sxs-lookup"><span data-stu-id="6668a-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="6668a-126">`OnChange` は、[IDisposable](/dotnet/api/system.idisposable) を返します。</span><span class="sxs-lookup"><span data-stu-id="6668a-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="6668a-127">[Dispose](/dotnet/api/system.idisposable.dispose) を呼び出すと、トークンがその後の変更のリッスンを停止し、トークンのリソースを解放します。</span><span class="sxs-lookup"><span data-stu-id="6668a-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="6668a-128">ASP.NET Core での変更のトークンの使用例</span><span class="sxs-lookup"><span data-stu-id="6668a-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="6668a-129">変更トークンは、オブジェクトの変更を監視する ASP.NET Core の主要な領域で使用されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="6668a-130">ファイルへの変更を監視するために、[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) の [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) メソッドは、監視するために指定されたファイルまたはフォルダーの `IChangeToken` を作成します。</span><span class="sxs-lookup"><span data-stu-id="6668a-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="6668a-131">`IChangeToken` トークンをキャッシュ エントリに追加して、変更時にキャッシュの削除をトリガーすることができます。</span><span class="sxs-lookup"><span data-stu-id="6668a-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="6668a-132">`TOptions` の変更のために、[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) の既定の [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) の実装には、1 つ以上の [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) インスタンスを受け入れるオーバーロードがあります。</span><span class="sxs-lookup"><span data-stu-id="6668a-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="6668a-133">各インスタンスは、オプションの変更を追跡するために変更通知のコールバックを登録する `IChangeToken` を返します。</span><span class="sxs-lookup"><span data-stu-id="6668a-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="6668a-134">構成の変更の監視</span><span class="sxs-lookup"><span data-stu-id="6668a-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="6668a-135">既定では、ASP.NET Core テンプレートは、[JSON 構成ファイル](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*、*appsettings.Development.json*、および *appsettings.Production.json*) を使用して、アプリの構成設定を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="6668a-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="6668a-136">これらのファイルは、`reloadOnChange` パラメーター (ASP.NET Core 1.1 以降) を受け入れる [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) 上で [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) 拡張メソッドを使用して構成されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="6668a-137">`reloadOnChange` は、ファイルの変更時に構成を再読み込みするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="6668a-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="6668a-138">[WebHost](/dotnet/api/microsoft.aspnetcore.webhost) の簡易メソッド [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) でこの設定を参照します。</span><span class="sxs-lookup"><span data-stu-id="6668a-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="6668a-139">ファイル ベースの構成は、[FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource) によって表されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="6668a-140">`FileConfigurationSource` はファイルのモニターに、[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) を使用します。</span><span class="sxs-lookup"><span data-stu-id="6668a-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) to monitor files.</span></span>

<span data-ttu-id="6668a-141">既定では、`IFileMonitor` は [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) によって提供されます。これは、[FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) を使用して、構成ファイルの変更を監視します。</span><span class="sxs-lookup"><span data-stu-id="6668a-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="6668a-142">サンプル アプリは、構成の変更を監視するための 2 つの実装を示します。</span><span class="sxs-lookup"><span data-stu-id="6668a-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="6668a-143">*appsettings.json* ファイルが変更されるか、ファイルの環境バージョンが変更された場合、各実装はカスタム コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="6668a-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="6668a-144">サンプル アプリは、コンソールにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="6668a-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="6668a-145">構成ファイルの `FileSystemWatcher` は、1 つの構成ファイルの変更に対して複数のトークンのコールバックをトリガーできます。</span><span class="sxs-lookup"><span data-stu-id="6668a-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="6668a-146">サンプルの実装は、構成ファイルのファイル ハッシュをチェックして、この問題を防ぎます。</span><span class="sxs-lookup"><span data-stu-id="6668a-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="6668a-147">ファイル ハッシュを確認することで、カスタム コードを実行する前に、構成ファイルの少なくとも 1 つが変更されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="6668a-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="6668a-148">このサンプルでは、SHA1 ファイル ハッシュ (*Utilities/Utilities.cs*) を使用します。</span><span class="sxs-lookup"><span data-stu-id="6668a-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="6668a-149">再試行は、指数バックオフを使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="6668a-150">再実行が提供されるのは、いずれかのファイルで新しいハッシュの計算を一時的に妨げるファイルのロックが発生する場合があるためです。</span><span class="sxs-lookup"><span data-stu-id="6668a-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="6668a-151">シンプルなスタートアップ変更トークン</span><span class="sxs-lookup"><span data-stu-id="6668a-151">Simple startup change token</span></span>

<span data-ttu-id="6668a-152">構成再読み込みトークン (*Startup.cs*) への変更通知のためにトークン コンシューマー `Action` のコールバックを登録します。</span><span class="sxs-lookup"><span data-stu-id="6668a-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="6668a-153">`config.GetReloadToken()` はトークンを提供します。</span><span class="sxs-lookup"><span data-stu-id="6668a-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="6668a-154">コールバックは、`InvokeChanged` メソッドです。</span><span class="sxs-lookup"><span data-stu-id="6668a-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="6668a-155">`IHostingEnvironment` で渡すためにコールバックの `state` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="6668a-156">これは、監視する正しい *appsettings* 構成 JSON ファイル *appsettings.&lt;Environment&gt;.json* を調べるために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="6668a-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="6668a-157">ファイル ハッシュは、構成ファイルが 1 回のみ変更されたときの複数のトークンのコールバックのために `WriteConsole` ステートメントが複数回実行されるのを防ぐために使用されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="6668a-158">このシステムは、アプリが実行されている限り実行され、ユーザーが無効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="6668a-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="6668a-159">サービスとしての構成の変更の監視</span><span class="sxs-lookup"><span data-stu-id="6668a-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="6668a-160">サンプルは次のものを実装します。</span><span class="sxs-lookup"><span data-stu-id="6668a-160">The sample implements:</span></span>

* <span data-ttu-id="6668a-161">基本的なスタートアップ トークンの監視</span><span class="sxs-lookup"><span data-stu-id="6668a-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="6668a-162">サービスとしての監視</span><span class="sxs-lookup"><span data-stu-id="6668a-162">Monitoring as a service.</span></span>
* <span data-ttu-id="6668a-163">監視を有効および無効にするメカニズム。</span><span class="sxs-lookup"><span data-stu-id="6668a-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="6668a-164">このサンプルは、`IConfigurationMonitor` インターフェイス (*Extensions/ConfigurationMonitor.cs*) を確立します。</span><span class="sxs-lookup"><span data-stu-id="6668a-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="6668a-165">実装済みのクラスのコンストラクター `ConfigurationMonitor` は、変更通知のコールバックを登録します。</span><span class="sxs-lookup"><span data-stu-id="6668a-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="6668a-166">`config.GetReloadToken()` はトークンを提供します。</span><span class="sxs-lookup"><span data-stu-id="6668a-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="6668a-167">`InvokeChanged` はコールバック メソッドです。</span><span class="sxs-lookup"><span data-stu-id="6668a-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="6668a-168">このインスタンスの `state` は、監視の状態を説明する文字列です。</span><span class="sxs-lookup"><span data-stu-id="6668a-168">The `state` in this instance is a string that describes the monitoring state.</span></span> <span data-ttu-id="6668a-169">次の 2 つのプロパティが使用されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-169">Two properties are used:</span></span>

* <span data-ttu-id="6668a-170">`MonitoringEnabled` は、コールバックでカスタム コードを実行する必要があるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="6668a-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="6668a-171">`CurrentState` は、UI で使用するための現在の監視状態を説明します。</span><span class="sxs-lookup"><span data-stu-id="6668a-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="6668a-172">`InvokeChanged` メソッドは、次の点を除けば、前の方法に似ています。</span><span class="sxs-lookup"><span data-stu-id="6668a-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="6668a-173">`MonitoringEnabled` が `true` ではない限りコードを実行しません。</span><span class="sxs-lookup"><span data-stu-id="6668a-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="6668a-174">`CurrentState` プロパティ文字列をコードが実行された時刻を記録する説明メッセージに設定します。</span><span class="sxs-lookup"><span data-stu-id="6668a-174">Sets the `CurrentState` property string to a descriptive message that records the time that the code ran.</span></span>
* <span data-ttu-id="6668a-175">`WriteConsole` 出力の現在の `state` に注意してください。</span><span class="sxs-lookup"><span data-stu-id="6668a-175">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="6668a-176">インスタンス `ConfigurationMonitor` は、*Startup.cs* の `ConfigureServices` でサービスとして登録されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-176">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="6668a-177">インデックス ページは、ユーザーに構成監視の制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="6668a-177">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="6668a-178">`IConfigurationMonitor` のインスタンスは、`IndexModel` に挿入されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-178">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="6668a-179">ボタンによって監視を有効および無効にします。</span><span class="sxs-lookup"><span data-stu-id="6668a-179">A button enables and disables monitoring:</span></span>

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="6668a-180">`OnPostStartMonitoring` がトリガーされると、監視が有効になり、現在の状態がクリアされます。</span><span class="sxs-lookup"><span data-stu-id="6668a-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="6668a-181">`OnPostStopMonitoring` がトリガーされると、監視が無効になり、監視が発生していないことを反映するように状態が設定されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="6668a-182">キャッシュされたファイルの変更の監視</span><span class="sxs-lookup"><span data-stu-id="6668a-182">Monitoring cached file changes</span></span>

<span data-ttu-id="6668a-183">[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) を使用して、ファイルの内容をメモリ内にキャッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="6668a-183">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="6668a-184">メモリ内キャッシュの説明については、「[メモリ内キャッシュ](xref:performance/caching/memory)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6668a-184">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="6668a-185">以下に示す実装など、追加の手順を実行しないと、ソース データが変更された場合に*期限切れの* (古い) データがキャッシュから返されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-185">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="6668a-186">[スライド式有効期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間の更新時にキャッシュされたソース ファイルのステータスを考慮しないと、古いキャッシュ データにつながります。</span><span class="sxs-lookup"><span data-stu-id="6668a-186">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="6668a-187">データの各要求が、スライド式有効期限を更新しますが、ファイルがキャッシュに再読み込みされることはありません。</span><span class="sxs-lookup"><span data-stu-id="6668a-187">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="6668a-188">ファイルのキャッシュされた内容を使用するアプリの機能は、古い内容を受け取る可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6668a-188">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="6668a-189">ファイル キャッシュのシナリオで変更トークンを使用すると、キャッシュのファイルの内容が古くなるのを防止できます。</span><span class="sxs-lookup"><span data-stu-id="6668a-189">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="6668a-190">サンプル アプリは、このアプローチの実装を示しています。</span><span class="sxs-lookup"><span data-stu-id="6668a-190">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="6668a-191">このサンプルでは、`GetFileContent` を使用して次の操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="6668a-191">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="6668a-192">ファイルの内容を返します。</span><span class="sxs-lookup"><span data-stu-id="6668a-192">Return file content.</span></span>
* <span data-ttu-id="6668a-193">指数バックオフを使用する再試行アルゴリズムを実装し、ファイル ブロックがファイルの読み取りを一時的に妨げるケースに対処します。</span><span class="sxs-lookup"><span data-stu-id="6668a-193">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="6668a-194">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="6668a-194">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="6668a-195">`FileService` はキャッシュされたファイルの参照の処理するために作成されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-195">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="6668a-196">サービスの `GetFileContent` メソッドの呼び出しは、メモリ内キャッシュからファイルの内容を取得して、呼び出し元 (*Services/FileService.cs*) に戻そうとします。</span><span class="sxs-lookup"><span data-stu-id="6668a-196">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="6668a-197">キャッシュ キーを使用してキャッシュされた内容が見つからない場合、次の操作が実行されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-197">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="6668a-198">`GetFileContent` を使用してファイルの内容が取得されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-198">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="6668a-199">変更トークンは、[IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) を使用してファイル プロバイダーから取得します。</span><span class="sxs-lookup"><span data-stu-id="6668a-199">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="6668a-200">ファイルが変更されたときに、トークンのコールバックがトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="6668a-200">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="6668a-201">ファイルの内容は、[スライド式有効期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)の期間キャシュされます。</span><span class="sxs-lookup"><span data-stu-id="6668a-201">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="6668a-202">ファイルがキャッシュされている間に変更された場合、キャッシュ エントリを削除するために、[MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) を使用して変更トークンがアタッチされます。</span><span class="sxs-lookup"><span data-stu-id="6668a-202">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="6668a-203">`FileService` がメモリ キャッシュ サービス (*Startup.cs*) と共にサービス コンテナーに登録されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-203">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="6668a-204">ページのモデルは、サービス (*Pages/Index.cshtml.cs*) を使用して、ファイルの内容を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="6668a-204">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="6668a-205">CompositeChangeToken クラス</span><span class="sxs-lookup"><span data-stu-id="6668a-205">CompositeChangeToken class</span></span>

<span data-ttu-id="6668a-206">1 つのオブジェクト内で 1 つまたは複数の `IChangeToken` インスタンスを表すために、[CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="6668a-206">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class.</span></span>

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

<span data-ttu-id="6668a-207">複合トークンの `HasChanged` は、いずれかの表現されているトークン `HasChanged` が `true` である場合に、`true` を報告します。</span><span class="sxs-lookup"><span data-stu-id="6668a-207">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="6668a-208">複合トークンの `ActiveChangeCallbacks` は、いずれかの表現されているトークン `ActiveChangeCallbacks` が `true` である場合に、`true` を報告します。</span><span class="sxs-lookup"><span data-stu-id="6668a-208">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="6668a-209">複数の同時変更イベントが発生すると、複合変更コールバックが正確に 1 回呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6668a-209">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="see-also"></a><span data-ttu-id="6668a-210">関連項目</span><span class="sxs-lookup"><span data-stu-id="6668a-210">See also</span></span>

* [<span data-ttu-id="6668a-211">メモリ内キャッシュ</span><span class="sxs-lookup"><span data-stu-id="6668a-211">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="6668a-212">分散キャッシュの使用</span><span class="sxs-lookup"><span data-stu-id="6668a-212">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="6668a-213">変更トークンを使用する変更の検出</span><span class="sxs-lookup"><span data-stu-id="6668a-213">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="6668a-214">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="6668a-214">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="6668a-215">応答キャッシュ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="6668a-215">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="6668a-216">キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="6668a-216">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="6668a-217">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="6668a-217">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
