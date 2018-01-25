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
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="6caa3-103">ASP.NET Core の変更のトークンを使用して変更を検出します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="6caa3-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6caa3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6caa3-105">A*トークンを変更*変更の追跡に使用される汎用の低レベルの構成要素です。</span><span class="sxs-lookup"><span data-stu-id="6caa3-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="6caa3-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="6caa3-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="6caa3-107">IChangeToken インターフェイス</span><span class="sxs-lookup"><span data-stu-id="6caa3-107">IChangeToken interface</span></span>

<span data-ttu-id="6caa3-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)変更が発生したことの通知を伝達します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="6caa3-109">`IChangeToken`存在する、 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives)名前空間。</span><span class="sxs-lookup"><span data-stu-id="6caa3-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="6caa3-110">使用していないアプリ、 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage、参照、 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)プロジェクト ファイルでの NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="6caa3-110">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="6caa3-111">`IChangeToken`2 つのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="6caa3-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="6caa3-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks)トークン プロアクティブにコールバックを発生させるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="6caa3-113">場合`ActiveChangedCallbacks`に設定されている`false`コールバックが呼び出されないと、アプリをポーリングする必要があります、`HasChanged`の変更。</span><span class="sxs-lookup"><span data-stu-id="6caa3-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="6caa3-114">変更は行われません、基になる変更リスナーが破棄または無効になっている場合は、キャンセルすることはありませんトークンのこともできます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="6caa3-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged)を変更が発生したかどうかを示す値を取得します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="6caa3-116">このインターフェイスに 1 つのメソッドでは、 [RegisterChangeCallback (アクション&lt;オブジェクト&gt;、オブジェクト)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)トークンが変更されたときに呼び出されるコールバックを登録します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="6caa3-117">`HasChanged`コールバックが呼び出される前に設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6caa3-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="6caa3-118">ChangeToken クラス</span><span class="sxs-lookup"><span data-stu-id="6caa3-118">ChangeToken class</span></span>

<span data-ttu-id="6caa3-119">`ChangeToken`変更が発生したことの通知を伝達するために使用する静的クラスです。</span><span class="sxs-lookup"><span data-stu-id="6caa3-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="6caa3-120">`ChangeToken`存在する、 [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives)名前空間。</span><span class="sxs-lookup"><span data-stu-id="6caa3-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="6caa3-121">使用していないアプリ、 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage、参照、 [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/)プロジェクト ファイルでの NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="6caa3-121">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="6caa3-122">`ChangeToken` [OnChange (Func&lt;IChangeToken&gt;、アクション)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_)メソッド レジスタ、`Action`を呼び出して、トークンが変更されるたびに。</span><span class="sxs-lookup"><span data-stu-id="6caa3-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>
* <span data-ttu-id="6caa3-123">`Func<IChangeToken>`トークンを生成します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="6caa3-124">`Action`トークンが変更されたときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="6caa3-125">`ChangeToken`[OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;、アクション&lt;TState&gt;、TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_)オーバー ロードを追加`TState`トークンのコンシューマーに渡されるパラメーター`Action`です。</span><span class="sxs-lookup"><span data-stu-id="6caa3-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="6caa3-126">`OnChange`返します、 [IDisposable](/dotnet/api/system.idisposable)です。</span><span class="sxs-lookup"><span data-stu-id="6caa3-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="6caa3-127">呼び出す[Dispose](/dotnet/api/system.idisposable.dispose)さらに変更をリッスンしてから、トークンを停止し、トークンのリソースを解放します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="6caa3-128">ASP.NET Core での変更のトークンの使用例</span><span class="sxs-lookup"><span data-stu-id="6caa3-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="6caa3-129">変更のトークンは、オブジェクトへの変更を監視する ASP.NET Core の目立つ領域で使用されます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="6caa3-130">ファイルへの変更の監視[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)の[ウォッチ](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch)メソッドを作成、`IChangeToken`指定したファイルまたはフォルダーを監視します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="6caa3-131">`IChangeToken`トークンは、変更のキャッシュ立ち退きをトリガーするキャッシュ エントリに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="6caa3-132">`TOptions` 、既定値を変更します[OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1)の実装[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) 1 つまたは複数を受け入れるオーバー ロードを持つ[IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)。インスタンス。</span><span class="sxs-lookup"><span data-stu-id="6caa3-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="6caa3-133">各インスタンスを返します、`IChangeToken`オプションの変更の追跡、変更通知のコールバックを登録します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="6caa3-134">構成の変更の監視</span><span class="sxs-lookup"><span data-stu-id="6caa3-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="6caa3-135">既定では、ASP.NET Core テンプレートを使用して[JSON 構成ファイル](xref:fundamentals/configuration/index#json-configuration)(*される appsettings.json*、 *appsettings です。Development.json*、および*appsettings です。Production.json*) アプリの構成設定を読み込めません。</span><span class="sxs-lookup"><span data-stu-id="6caa3-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="6caa3-136">使用してこれらのファイルは、構成、 [AddJsonFile (IConfigurationBuilder、String、Boolean、ブール値)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_)拡張メソッドを[ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder)を受け入れる、`reloadOnChange`パラメーター (ASP.NETコア 1.1 以降)。</span><span class="sxs-lookup"><span data-stu-id="6caa3-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="6caa3-137">`reloadOnChange`ファイルの変更の構成を再読み込みするかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="6caa3-138">この設定を参照してください、 [WebHost](/dotnet/api/microsoft.aspnetcore.webhost)便利なメソッド[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([参照ソース](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193))。</span><span class="sxs-lookup"><span data-stu-id="6caa3-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([reference source](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="6caa3-139">ファイル ベースの構成がによって表される[FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource)です。</span><span class="sxs-lookup"><span data-stu-id="6caa3-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="6caa3-140">`FileConfigurationSource`使用して[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([参照ソース](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) ファイルを監視します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([reference source](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) to monitor files.</span></span>

<span data-ttu-id="6caa3-141">既定では、`IFileMonitor`によって提供される、 [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([参照ソース](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)) を使用する[FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher)構成ファイルを監視するには変更します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([reference source](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="6caa3-142">サンプル アプリでは、構成の変更を監視するための 2 つの実装を示します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="6caa3-143">どちらの場合、*される appsettings.json*ファイルまたは環境のバージョンのファイルが変更された、各実装は、カスタム コードを実行します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="6caa3-144">サンプル アプリは、コンソールにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="6caa3-145">構成ファイルの`FileSystemWatcher`1 つの構成ファイルの変更を複数のトークンのコールバックをトリガーできます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="6caa3-146">サンプルの実装は、構成ファイルのファイル ハッシュをチェックして、この問題を防ぐ。</span><span class="sxs-lookup"><span data-stu-id="6caa3-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="6caa3-147">カスタム コードを実行する前に、構成ファイルの少なくとも 1 つが変更されたことにより、ファイル ハッシュを確認しています。</span><span class="sxs-lookup"><span data-stu-id="6caa3-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="6caa3-148">このサンプルでは、SHA1 ファイルのハッシュでは (*Utilities/Utilities.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6caa3-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="6caa3-149">再試行は、指数バックオフで実装されます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="6caa3-150">再実行は、ファイルのロックが発生する一時的に妨げるファイルのいずれかで新しいハッシュを計算するために存在します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="6caa3-151">シンプルなスタートアップ変更トークン</span><span class="sxs-lookup"><span data-stu-id="6caa3-151">Simple startup change token</span></span>

<span data-ttu-id="6caa3-152">トークンのコンシューマーを登録`Action`構成の再読み込みトークンへのコールバック変更通知 (*Startup.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6caa3-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="6caa3-153">`config.GetReloadToken()`トークンを提供します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="6caa3-154">コールバックを`InvokeChanged`メソッド。</span><span class="sxs-lookup"><span data-stu-id="6caa3-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="6caa3-155">`state`に渡すコールバックに使用される、`IHostingEnvironment`です。</span><span class="sxs-lookup"><span data-stu-id="6caa3-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="6caa3-156">これは、正しいを調べるのに役立ちます*appsettings*構成 JSON ファイルを監視する*appsettings&lt; 。環境&gt;.json*です。</span><span class="sxs-lookup"><span data-stu-id="6caa3-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="6caa3-157">ファイルのハッシュを防ぐために使用される、`WriteConsole`ステートメントの構成ファイルが 1 回のみ変更されたときの複数のトークン コールバックにより複数回実行します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="6caa3-158">このシステムを実行する限り、アプリが実行されていると、ユーザーを無効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="6caa3-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="6caa3-159">サービスとしての構成の変更の監視</span><span class="sxs-lookup"><span data-stu-id="6caa3-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="6caa3-160">このサンプルを実装します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-160">The sample implements:</span></span>

* <span data-ttu-id="6caa3-161">基本的なスタートアップ トークンを監視します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="6caa3-162">サービスを監視します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-162">Monitoring as a service.</span></span>
* <span data-ttu-id="6caa3-163">有効にして、監視を無効にするメカニズム。</span><span class="sxs-lookup"><span data-stu-id="6caa3-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="6caa3-164">このサンプルの確立、`IConfigurationMonitor`インターフェイス (*Extensions/ConfigurationMonitor.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6caa3-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="6caa3-165">実装済みのクラスのコンス トラクター `ConfigurationMonitor`、変更通知のコールバックを登録します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="6caa3-166">`config.GetReloadToken()`トークンを提供します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="6caa3-167">`InvokeChanged`コールバック メソッドです。</span><span class="sxs-lookup"><span data-stu-id="6caa3-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="6caa3-168">`state`がこのインスタンスでは、監視の状態を説明する文字列。</span><span class="sxs-lookup"><span data-stu-id="6caa3-168">The `state` in this instance is a string that describes the monitoring state.</span></span> <span data-ttu-id="6caa3-169">2 つのプロパティが使用されます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-169">Two properties are used:</span></span>

* <span data-ttu-id="6caa3-170">`MonitoringEnabled`コールバックが、カスタム コードを実行する必要があるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="6caa3-171">`CurrentState`UI で使用するための現在の監視状態を説明します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="6caa3-172">`InvokeChanged`メソッドは点を除けば、前の方法に似ています。</span><span class="sxs-lookup"><span data-stu-id="6caa3-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="6caa3-173">しない限り、そのコードを実行しない`MonitoringEnabled`は`true`します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="6caa3-174">セット、`CurrentState`コードを実行した時刻を記録する説明メッセージにプロパティの文字列。</span><span class="sxs-lookup"><span data-stu-id="6caa3-174">Sets the `CurrentState` property string to a descriptive message that records the time that the code ran.</span></span>
* <span data-ttu-id="6caa3-175">現在のノート`state`でその`WriteConsole`出力します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-175">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="6caa3-176">インスタンス`ConfigurationMonitor`でサービスとして登録されて`ConfigureServices`の*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6caa3-176">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="6caa3-177">インデックス ページは、ユーザーを制御する構成の監視を提供します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-177">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="6caa3-178">インスタンス`IConfigurationMonitor`に挿入された、 `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="6caa3-178">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="6caa3-179">ボタンを有効にし、監視を無効に。</span><span class="sxs-lookup"><span data-stu-id="6caa3-179">A button enables and disables monitoring:</span></span>

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="6caa3-180">ときに`OnPostStartMonitoring`がトリガーされると、監視が有効になっているし、現在の状態がオフになっています。</span><span class="sxs-lookup"><span data-stu-id="6caa3-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="6caa3-181">ときに`OnPostStopMonitoring`がトリガーされると、監視は無効になり、状態が監視されていない行われていることを反映するように設定します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="6caa3-182">キャッシュされたファイルの変更の監視</span><span class="sxs-lookup"><span data-stu-id="6caa3-182">Monitoring cached file changes</span></span>

<span data-ttu-id="6caa3-183">ファイルの内容がメモリ内のキャッシュを使用することができます[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)です。</span><span class="sxs-lookup"><span data-stu-id="6caa3-183">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="6caa3-184">メモリ内キャッシュに記載されて、[のメモリ内キャッシュ](xref:performance/caching/memory)トピックです。</span><span class="sxs-lookup"><span data-stu-id="6caa3-184">In-memory caching is described in the [In-memory caching](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="6caa3-185">以下に示す実装など、追加の手順を考慮せず*古い*ソース データが変更された場合 (古い) データがキャッシュから返されます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-185">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="6caa3-186">いないを考慮して、キャッシュされたソース ファイルの状態を更新するときに、[スライディング期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間は、古いデータをキャッシュにつながります。</span><span class="sxs-lookup"><span data-stu-id="6caa3-186">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="6caa3-187">データの各要求が、スライド式有効期限を更新しますが、ファイルがキャッシュに再読み込みされません。</span><span class="sxs-lookup"><span data-stu-id="6caa3-187">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="6caa3-188">ファイルのキャッシュされたコンテンツを使用するアプリの機能は、可能性のある古いコンテンツが受信されるとします。</span><span class="sxs-lookup"><span data-stu-id="6caa3-188">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="6caa3-189">シナリオをキャッシュ ファイルの変更のトークンを使用して、キャッシュの内容を古いファイルができなくなります。</span><span class="sxs-lookup"><span data-stu-id="6caa3-189">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="6caa3-190">サンプル アプリでは、アプローチの実装を示します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-190">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="6caa3-191">このサンプルでは`GetFileContent`に。</span><span class="sxs-lookup"><span data-stu-id="6caa3-191">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="6caa3-192">ファイルの内容を返します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-192">Return file content.</span></span>
* <span data-ttu-id="6caa3-193">カバーの場合、ファイルのロックが一時的に妨げられてファイル読み取り中に指数バックオフでの再試行アルゴリズムを実装します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-193">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="6caa3-194">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="6caa3-194">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="6caa3-195">A`FileService`がキャッシュされたファイル参照の処理を作成します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-195">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="6caa3-196">`GetFileContent`メモリ内キャッシュからファイル コンテンツを取得して、呼び出し元に戻すしようとサービスのメソッドの呼び出し (*Services/FileService.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6caa3-196">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="6caa3-197">キャッシュされたコンテンツが見つからない場合、キャッシュ キーを使用して、次の操作は取得されます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-197">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="6caa3-198">使用して、ファイルの内容を取得`GetFileContent`です。</span><span class="sxs-lookup"><span data-stu-id="6caa3-198">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="6caa3-199">変更トークンは、プロバイダーから取得した、ファイルと[IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch)です。</span><span class="sxs-lookup"><span data-stu-id="6caa3-199">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="6caa3-200">ファイルが変更されたときに、トークンのコールバックがトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-200">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="6caa3-201">ファイルの内容はと共にキャッシュされる、[スライディング期限](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration)期間。</span><span class="sxs-lookup"><span data-stu-id="6caa3-201">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="6caa3-202">変更のトークンが割り当てられて[MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken)がキャッシュされているときに、ファイルが変更された場合、キャッシュ エントリを削除します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-202">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="6caa3-203">`FileService`がメモリのキャッシュ サービスと共にサービス コンテナーに登録 (*Startup.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6caa3-203">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="6caa3-204">ページのモデルは、サービスを使用して、ファイルの内容を読み込みます (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="6caa3-204">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="6caa3-205">CompositeChangeToken クラス</span><span class="sxs-lookup"><span data-stu-id="6caa3-205">CompositeChangeToken class</span></span>

<span data-ttu-id="6caa3-206">表す 1 つまたは複数の`IChangeToken`1 つのオブジェクトのインスタンスを使用して、 [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken)クラス ([参照ソース](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs))。</span><span class="sxs-lookup"><span data-stu-id="6caa3-206">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class ([reference source](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).</span></span>

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

<span data-ttu-id="6caa3-207">`HasChanged`複合トークン レポートに対して`true`トークンを表すいずれかの場合は`HasChanged`は`true`します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-207">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="6caa3-208">`ActiveChangeCallbacks`複合トークン レポートに対して`true`トークンを表すいずれかの場合は`ActiveChangeCallbacks`は`true`します。</span><span class="sxs-lookup"><span data-stu-id="6caa3-208">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="6caa3-209">複数の同時変更イベントが発生すると、正確に 1 回、複合変更コールバックが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6caa3-209">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="see-also"></a><span data-ttu-id="6caa3-210">関連項目</span><span class="sxs-lookup"><span data-stu-id="6caa3-210">See also</span></span>

* [<span data-ttu-id="6caa3-211">メモリ内キャッシュ</span><span class="sxs-lookup"><span data-stu-id="6caa3-211">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="6caa3-212">分散キャッシュの使用</span><span class="sxs-lookup"><span data-stu-id="6caa3-212">Working with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="6caa3-213">変更トークンを使用する変更の検出</span><span class="sxs-lookup"><span data-stu-id="6caa3-213">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="6caa3-214">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="6caa3-214">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="6caa3-215">応答キャッシュ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="6caa3-215">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="6caa3-216">キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="6caa3-216">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="6caa3-217">分散キャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="6caa3-217">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
