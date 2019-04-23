---
title: ASP.NET Core でのログ記録
author: tdykstra
description: ASP.NET Core でのログ記録フレームワークについて説明します。 組み込みのログ プロバイダーと、サードパーティ製の一般的なプロバイダーについて説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: 8a2e310b47e32e9015b0c127ed79d8f6bdf2e44d
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982854"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="baea0-104">ASP.NET Core でのログ記録</span><span class="sxs-lookup"><span data-stu-id="baea0-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="baea0-105">執筆: [Steve Smith](https://ardalis.com/)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="baea0-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="baea0-106">ASP.NET Core では、組み込みやサード パーティ製のさまざまなログ プロバイダーと連携するログ API がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="baea0-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="baea0-107">この記事では、組み込みプロバイダーと共にログ API を使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="baea0-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="baea0-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="baea0-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="baea0-109">プロバイダーを追加する</span><span class="sxs-lookup"><span data-stu-id="baea0-109">Add providers</span></span>

<span data-ttu-id="baea0-110">ログ プロバイダーによってログが表示または格納されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="baea0-111">たとえば、Console プロバイダーによってコンソール上にログが表示され、Azure Application Insights プロバイダーによってそれらが Azure Application Insights に格納されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="baea0-112">複数のプロバイダーを追加することで、複数の宛先にログを送信することができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="baea0-113">プロバイダーを追加するには、*Program.cs* でプロバイダーの `Add{provider name}` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="baea0-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

<span data-ttu-id="baea0-114">既定のプロジェクト テンプレートでは、次のログ プロバイダーを追加する <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-114">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="baea0-115">コンソール</span><span class="sxs-lookup"><span data-stu-id="baea0-115">Console</span></span>
* <span data-ttu-id="baea0-116">デバッグ</span><span class="sxs-lookup"><span data-stu-id="baea0-116">Debug</span></span>
* <span data-ttu-id="baea0-117">EventSource (ASP.NET Core 2.2 以降)</span><span class="sxs-lookup"><span data-stu-id="baea0-117">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="baea0-118">`CreateDefaultBuilder` を使用する場合は、既定のプロバイダーを自分で選択したものと置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-118">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="baea0-119"><xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> を呼び出し、目的のプロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="baea0-119">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="baea0-120">プロバイダーを使用するには、その NuGet パッケージをインストールし、<xref:Microsoft.Extensions.Logging.ILoggerFactory> のインスタンスに対してプロバイダーの拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="baea0-120">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="baea0-121">ASP.NET Core の[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) には、`ILoggerFactory` インスタンスが用意されています。</span><span class="sxs-lookup"><span data-stu-id="baea0-121">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="baea0-122">`AddConsole` および `AddDebug` 拡張メソッドは、[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) パッケージと [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) パッケージで定義されています。</span><span class="sxs-lookup"><span data-stu-id="baea0-122">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="baea0-123">各拡張メソッドで `ILoggerFactory.AddProvider` メソッドを呼び出し、プロバイダーのインスタンスで渡します。</span><span class="sxs-lookup"><span data-stu-id="baea0-123">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="baea0-124">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x)では、`Startup.Configure` メソッドにログ プロバイダーを追加しています。</span><span class="sxs-lookup"><span data-stu-id="baea0-124">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="baea0-125">前の手順で実行したコードのログ出力を取得するには、`Startup` クラス コンストラクターにログ プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="baea0-125">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="baea0-126">この記事の後半では、[組み込みログ プロバイダー](#built-in-logging-providers)と[サードパーティ製ログ プロバイダー](#third-party-logging-providers)について説明します。</span><span class="sxs-lookup"><span data-stu-id="baea0-126">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="baea0-127">ログを作成する</span><span class="sxs-lookup"><span data-stu-id="baea0-127">Create logs</span></span>

<span data-ttu-id="baea0-128">DI から <xref:Microsoft.Extensions.Logging.ILogger%601> オブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="baea0-128">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="baea0-129">次のコントローラーの例では、`Information` と `Warning` ログを作成します。</span><span class="sxs-lookup"><span data-stu-id="baea0-129">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="baea0-130">"*カテゴリ*" は `TodoApiSample.Controllers.TodoController` (サンプル アプリの `TodoController` の完全修飾クラス名) です。</span><span class="sxs-lookup"><span data-stu-id="baea0-130">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="baea0-131">次の Razor Pages の例では、`Information` を "*レベル*" に、`TodoApiSample.Pages.AboutModel` を "*カテゴリ*" に設定してログを作成します。</span><span class="sxs-lookup"><span data-stu-id="baea0-131">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="baea0-132">前の例では、`Information` と `Warning` を "*レベル*" に、`TodoController` クラスを "*カテゴリ*" に設定してログを作成しています。</span><span class="sxs-lookup"><span data-stu-id="baea0-132">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="baea0-133">ログの "*レベル*" は、ログに記録されるイベントの重大度を示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-133">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="baea0-134">ログの "*カテゴリ*" は、各ログに関連付けられている文字列です。</span><span class="sxs-lookup"><span data-stu-id="baea0-134">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="baea0-135">`ILogger<T>` インスタンスでは、カテゴリとして型 `T` の完全修飾名を持つログが作成されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-135">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="baea0-136">[レベル](#log-level)と[カテゴリ](#log-category)の詳細については、この記事で後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="baea0-136">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="baea0-137">Startup でログを作成する</span><span class="sxs-lookup"><span data-stu-id="baea0-137">Create logs in Startup</span></span>

<span data-ttu-id="baea0-138">`Startup` クラスでログを作成するには、コンストラクター シグネチャに `ILogger` パラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="baea0-138">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="baea0-139">プログラムでログを作成する</span><span class="sxs-lookup"><span data-stu-id="baea0-139">Create logs in Program</span></span>

<span data-ttu-id="baea0-140">`Program` クラスでログを作成するには、DI から `ILogger` インスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="baea0-140">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="baea0-141">非同期でないロガー メソッド</span><span class="sxs-lookup"><span data-stu-id="baea0-141">No asynchronous logger methods</span></span>

<span data-ttu-id="baea0-142">ログ記録は高速に実行され、非同期コードのパフォーマンス コストを下回る必要があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-142">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="baea0-143">ログ記録のデータ ストアが低速の場合は、そこへ直接書き込むべきではありません。</span><span class="sxs-lookup"><span data-stu-id="baea0-143">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="baea0-144">まずログ メッセージを高速なストアに書き込んでから、後で低速なストアに移動する方法を検討してください。</span><span class="sxs-lookup"><span data-stu-id="baea0-144">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="baea0-145">たとえば、SQL Server にログインしている場合、それを `Log` メソッドで直接実行したくはないでしょう。`Log` が同期メソッドであるためです。</span><span class="sxs-lookup"><span data-stu-id="baea0-145">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="baea0-146">代わりに、ログ メッセージをインメモリ キューに同期的に追加し、バックグラウンド ワーカーにキューからメッセージをプルさせて、SQL Server にデータをプッシュする非同期処理を実行させます。</span><span class="sxs-lookup"><span data-stu-id="baea0-146">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="baea0-147">構成</span><span class="sxs-lookup"><span data-stu-id="baea0-147">Configuration</span></span>

<span data-ttu-id="baea0-148">ログ プロバイダーの構成は、1 つまたは複数の構成プロバイダーによって提供されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-148">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="baea0-149">ファイル形式 (INI、JSON、および XML)。</span><span class="sxs-lookup"><span data-stu-id="baea0-149">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="baea0-150">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="baea0-150">Command-line arguments.</span></span>
* <span data-ttu-id="baea0-151">環境変数。</span><span class="sxs-lookup"><span data-stu-id="baea0-151">Environment variables.</span></span>
* <span data-ttu-id="baea0-152">メモリ内 .NET オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="baea0-152">In-memory .NET objects.</span></span>
* <span data-ttu-id="baea0-153">暗号化されていない[シークレット マネージャー](xref:security/app-secrets)の記憶域。</span><span class="sxs-lookup"><span data-stu-id="baea0-153">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="baea0-154">[Azure Key Vault](xref:security/key-vault-configuration) などの暗号化されたユーザー ストア。</span><span class="sxs-lookup"><span data-stu-id="baea0-154">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="baea0-155">カスタム プロバイダー (インストール済みまたは作成済み)。</span><span class="sxs-lookup"><span data-stu-id="baea0-155">Custom providers (installed or created).</span></span>

<span data-ttu-id="baea0-156">たとえば、一般的に、ログの構成はアプリ設定ファイルの `Logging` セクションで指定されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-156">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="baea0-157">次の例は、一般的な *appsettings.Development.json* ファイルの内容を示しています。</span><span class="sxs-lookup"><span data-stu-id="baea0-157">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

<span data-ttu-id="baea0-158">`Logging` プロパティには `LogLevel` およびログ プロバイダーのプロパティ (Console が示されています) を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-158">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="baea0-159">`Logging` の下の `LogLevel` プロパティでは、選択したカテゴリに対するログの最小の[レベル](#log-level)が指定されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-159">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="baea0-160">この例では、`System` と `Microsoft` カテゴリが `Information` レベルで、その他はすべて `Debug` レベルでログに記録します。</span><span class="sxs-lookup"><span data-stu-id="baea0-160">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="baea0-161">`Logging` の下のその他のプロパティではログ プロバイダーが指定されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-161">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="baea0-162">この例では、Console プロバイダーです。</span><span class="sxs-lookup"><span data-stu-id="baea0-162">The example is for the Console provider.</span></span> <span data-ttu-id="baea0-163">プロバイダーで[ログのスコープ](#log-scopes)がサポートされている場合、`IncludeScopes` によってそれを有効にするかどうかが指定されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-163">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="baea0-164">プロバイダーのプロパティ (例の `Console` など) では、`LogLevel` プロパティが指定される場合があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-164">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="baea0-165">プロバイダーの下の `LogLevel` では、そのプロバイダーのログのレベルが指定されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-165">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="baea0-166">`Logging.{providername}.LogLevel` でレベルが指定される場合、それによって `Logging.LogLevel` で設定されたものはすべてオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="baea0-166">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

<span data-ttu-id="baea0-167">`LogLevel` キーは、ログ名を表します。</span><span class="sxs-lookup"><span data-stu-id="baea0-167">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="baea0-168">`Default` キーは明示的に表示されていないログに適用されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-168">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="baea0-169">値は、指定されたログに適用された[ログ レベル](#log-level)を表します。</span><span class="sxs-lookup"><span data-stu-id="baea0-169">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="baea0-170">構成プロバイダーの実装について詳しくは、<xref:fundamentals/configuration/index> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="baea0-170">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="baea0-171">サンプルのログ記録の出力</span><span class="sxs-lookup"><span data-stu-id="baea0-171">Sample logging output</span></span>

<span data-ttu-id="baea0-172">前のセクションで紹介したサンプル コードでは、コマンド ラインからアプリを実行するとコンソールにログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-172">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="baea0-173">コンソールの出力例は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="baea0-173">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

<span data-ttu-id="baea0-174">前のログは、`http://localhost:5000/api/todo/0` のサンプル アプリに向けて HTTP Get 要求を作成することで生成されました。</span><span class="sxs-lookup"><span data-stu-id="baea0-174">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="baea0-175">Visual Studio でサンプル アプリを実行したときに [デバッグ] ウィンドウに表示されるログと同じログの例を、次に示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-175">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="baea0-176">前のセクションで紹介した `ILogger` の呼び出しで作成されるログは、"TodoApi.Controllers.TodoController" から始まります。</span><span class="sxs-lookup"><span data-stu-id="baea0-176">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="baea0-177">"Microsoft" カテゴリから始まるログは、ASP.NET Core のフレームワークのコードからのログです。</span><span class="sxs-lookup"><span data-stu-id="baea0-177">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="baea0-178">ASP.NET Core とアプリケーション コードでは、同じログ API とログ プロバイダーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="baea0-178">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="baea0-179">以降、この記事では、ログ記録の詳細とオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="baea0-179">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="baea0-180">NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="baea0-180">NuGet packages</span></span>

<span data-ttu-id="baea0-181">`ILogger` および `ILoggerFactory` インターフェイスは、[Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 内にあり、それらの既定の実装は [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 内にあります。</span><span class="sxs-lookup"><span data-stu-id="baea0-181">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="baea0-182">ログのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="baea0-182">Log category</span></span>

<span data-ttu-id="baea0-183">`ILogger` オブジェクトが作成されるときに、その "*カテゴリ*" が指定されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-183">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="baea0-184">このカテゴリは、その `ILogger` のインスタンスによって作成される各ログ メッセージと共に含められます。</span><span class="sxs-lookup"><span data-stu-id="baea0-184">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="baea0-185">カテゴリには任意の文字列を指定できますが、"TodoApi.Controllers.TodoController" などのクラス名を使用するが慣例です。</span><span class="sxs-lookup"><span data-stu-id="baea0-185">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="baea0-186">`ILogger<T>` を使用して、カテゴリとして `T` の完全修飾型名が使用される `ILogger` インスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="baea0-186">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="baea0-187">カテゴリを明示的に指定するには、`ILoggerFactory.CreateLogger` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="baea0-187">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="baea0-188">`ILogger<T>` は、`T` の完全修飾型名を使用した `CreateLogger` の呼び出しと同じです。</span><span class="sxs-lookup"><span data-stu-id="baea0-188">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="baea0-189">ログ レベル</span><span class="sxs-lookup"><span data-stu-id="baea0-189">Log level</span></span>

<span data-ttu-id="baea0-190">すべてのログで <xref:Microsoft.Extensions.Logging.LogLevel> 値が指定されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-190">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="baea0-191">ログ レベルは、重大度または重要度を示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-191">The log level indicates the severity or importance.</span></span> <span data-ttu-id="baea0-192">たとえば、メソッドが正常に終了した場合は `Information` ログを、メソッドが *404 Not Found* 状態コードを返した場合は `Warning` ログを書き込む場合があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-192">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="baea0-193">`Information` および `Warning` ログを作成するコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-193">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="baea0-194">前のコードでは、最初のパラメーターは[ログ イベント ID](#log-event-id) です。</span><span class="sxs-lookup"><span data-stu-id="baea0-194">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="baea0-195">2 つ目のパラメーターは、他のメソッド パラメーターによって提供される引数値のプレースホルダーを含むメッセージ テンプレートです。</span><span class="sxs-lookup"><span data-stu-id="baea0-195">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="baea0-196">メソッド パラメーターについては、この記事の後半の[メッセージ テンプレートのセクション](#log-message-template)で説明します。</span><span class="sxs-lookup"><span data-stu-id="baea0-196">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="baea0-197">メソッド名にレベルを含むログ メソッド (たとえば `LogInformation` や `LogWarning`) は、[ILogger の拡張メソッド](xref:Microsoft.Extensions.Logging.LoggerExtensions)です。</span><span class="sxs-lookup"><span data-stu-id="baea0-197">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="baea0-198">これらのメソッドでは、`LogLevel` パラメーターを受け取る `Log` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-198">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="baea0-199">これらの拡張メソッドのいずれかではなく、`Log` メソッドを直接呼び出すことができますが、構文は比較的複雑です。</span><span class="sxs-lookup"><span data-stu-id="baea0-199">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="baea0-200">詳細については、<xref:Microsoft.Extensions.Logging.ILogger> および[ロガー拡張ソース コード](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="baea0-200">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="baea0-201">ASP.NET Core には、次のログ レベルが定義されています (重大度の低いものから高い順)。</span><span class="sxs-lookup"><span data-stu-id="baea0-201">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="baea0-202">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="baea0-202">Trace = 0</span></span>

  <span data-ttu-id="baea0-203">通常はデバッグでのみ役立つ情報の場合。</span><span class="sxs-lookup"><span data-stu-id="baea0-203">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="baea0-204">これらのメッセージには機密性の高いアプリケーション データが含まれる可能性があるため、運用環境は有効にしないことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="baea0-204">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="baea0-205">*既定で無効です。*</span><span class="sxs-lookup"><span data-stu-id="baea0-205">*Disabled by default.*</span></span>

* <span data-ttu-id="baea0-206">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="baea0-206">Debug = 1</span></span>

  <span data-ttu-id="baea0-207">開発とデバッグで役立つ可能性がある情報の場合。</span><span class="sxs-lookup"><span data-stu-id="baea0-207">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="baea0-208">例:`Entering method Configure with flag set to true.` `Debug` レベルのログは、ログのサイズが大きくなるため、トラブルシューティングの場合を除き運用環境では有効にしません。</span><span class="sxs-lookup"><span data-stu-id="baea0-208">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="baea0-209">Information = 2</span><span class="sxs-lookup"><span data-stu-id="baea0-209">Information = 2</span></span>

  <span data-ttu-id="baea0-210">アプリの一般的なフローを追跡する場合。</span><span class="sxs-lookup"><span data-stu-id="baea0-210">For tracking the general flow of the app.</span></span> <span data-ttu-id="baea0-211">通常、これらのログには、長期的な値があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-211">These logs typically have some long-term value.</span></span> <span data-ttu-id="baea0-212">例 : `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="baea0-212">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="baea0-213">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="baea0-213">Warning = 3</span></span>

  <span data-ttu-id="baea0-214">アプリのフローで異常なイベントや予期しないイベントが発生した場合。</span><span class="sxs-lookup"><span data-stu-id="baea0-214">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="baea0-215">アプリの停止の原因にはならないが、調査する必要があるエラーやその他の状態がここに含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-215">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="baea0-216">`Warning` ログ レベルが使用される一般的な場所として、例外の処理があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-216">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="baea0-217">例 : `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="baea0-217">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="baea0-218">Error = 4</span><span class="sxs-lookup"><span data-stu-id="baea0-218">Error = 4</span></span>

  <span data-ttu-id="baea0-219">処理できないエラーと例外の場合。</span><span class="sxs-lookup"><span data-stu-id="baea0-219">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="baea0-220">これらのメッセージは、アプリ全体のエラーではなく、現在のアクティビティまたは操作 (現在の HTTP 要求など) におけるエラーを示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-220">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="baea0-221">ログ メッセージの例: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="baea0-221">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="baea0-222">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="baea0-222">Critical = 5</span></span>

  <span data-ttu-id="baea0-223">即時の注意が必要なエラーの場合。</span><span class="sxs-lookup"><span data-stu-id="baea0-223">For failures that require immediate attention.</span></span> <span data-ttu-id="baea0-224">例: データ損失のシナリオ、ディスク領域不足。</span><span class="sxs-lookup"><span data-stu-id="baea0-224">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="baea0-225">ログ レベルを使用して、特定のストレージ メディアまたは表示ウィンドウに書き込むログの出力量を制御します。</span><span class="sxs-lookup"><span data-stu-id="baea0-225">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="baea0-226">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-226">For example:</span></span>

* <span data-ttu-id="baea0-227">運用環境では、`Information` レベルを使用してボリューム データ ストアに `Trace` を送信します。</span><span class="sxs-lookup"><span data-stu-id="baea0-227">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="baea0-228">`Critical` を使用して値のデータ ストアに `Warning` を送信します。</span><span class="sxs-lookup"><span data-stu-id="baea0-228">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="baea0-229">開発中は `Critical` を使用してコンソールに `Warning` を送信し、トラブルシューティングの際は `Information` を使用して `Trace` を追加します。</span><span class="sxs-lookup"><span data-stu-id="baea0-229">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="baea0-230">この記事で後述する「[ログのフィルター処理](#log-filtering)」セクションでは、プロバイダーで処理するログ レベルの制御方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="baea0-230">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="baea0-231">ASP.NET Core では、フレームワーク イベントのログが書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="baea0-231">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="baea0-232">この記事の前のログの例では、`Information` レベル以下のログを除外したため、`Debug` または `Trace` レベルのログは作成されませんでした。</span><span class="sxs-lookup"><span data-stu-id="baea0-232">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="baea0-233">`Debug` ログを示すために構成したサンプル アプリを実行することで生成されるコンソールのログの例を、次に示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-233">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="baea0-234">ログ イベント ID</span><span class="sxs-lookup"><span data-stu-id="baea0-234">Log event ID</span></span>

<span data-ttu-id="baea0-235">各ログで "*イベント ID*" を指定できます。</span><span class="sxs-lookup"><span data-stu-id="baea0-235">Each log can specify an *event ID*.</span></span> <span data-ttu-id="baea0-236">サンプル アプリでは、この処理にローカルで定義された `LoggingEvents` クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="baea0-236">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="baea0-237">イベント ID によって一連のイベントが関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="baea0-237">An event ID associates a set of events.</span></span> <span data-ttu-id="baea0-238">たとえば、ページ上に項目の一覧を表示する機能に関連するすべてのログを 1001 に設定します。</span><span class="sxs-lookup"><span data-stu-id="baea0-238">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="baea0-239">ログ プロバイダーでは、ID フィールドやログ メッセージにイベント ID が格納されたり、またはまったく格納されなかったりする場合があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-239">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="baea0-240">Debug プロバイダーでイベント ID が表示されることはありません。</span><span class="sxs-lookup"><span data-stu-id="baea0-240">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="baea0-241">Console プロバイダーでは、カテゴリの後のブラケット内にイベント ID が表示されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-241">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="baea0-242">ログ メッセージ テンプレート</span><span class="sxs-lookup"><span data-stu-id="baea0-242">Log message template</span></span>

<span data-ttu-id="baea0-243">各ログでメッセージ テンプレートが指定されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-243">Each log specifies a message template.</span></span> <span data-ttu-id="baea0-244">メッセージ テンプレートには、指定される引数のためのプレースホルダーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-244">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="baea0-245">プレースホルダーには、数値ではなく名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="baea0-245">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="baea0-246">プレースホルダーの名前ではなく、プレースホルダーの順序によって、値の指定に使用されるパラメーターが決まります。</span><span class="sxs-lookup"><span data-stu-id="baea0-246">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="baea0-247">次のコードでは、パラメーター名がメッセージ テンプレート内のシーケンスの外にあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="baea0-247">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="baea0-248">このコードでは、シーケンス内のパラメーター値を含むログ メッセージが作成されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-248">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="baea0-249">ログ プロバイダーが[セマンティック ログ記録 (または構造化ログ記録)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)を実装できるようにするために、ログ フレームワークはこのように動作します。</span><span class="sxs-lookup"><span data-stu-id="baea0-249">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="baea0-250">書式設定されたメッセージ テンプレートだけでなく、引数自体がログ システムに渡されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-250">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="baea0-251">この情報により、ログ プロバイダーはフィールドとしてパラメーター値を格納することができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-251">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="baea0-252">たとえば、つぎのようなロガー メソッドの呼び出しがあるとします。</span><span class="sxs-lookup"><span data-stu-id="baea0-252">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="baea0-253">Azure Table Storage にログを送信する場合、各 Azure Table エンティティに `ID` および `RequestTime` プロパティを指定し、ログ データのクエリを簡略化することができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-253">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="baea0-254">クエリによって指定した `RequestTime` の範囲内のすべてのログを検索できます。テキスト メッセージから時間を解析する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="baea0-254">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="baea0-255">ログ記録の例外</span><span class="sxs-lookup"><span data-stu-id="baea0-255">Logging exceptions</span></span>

<span data-ttu-id="baea0-256">ロガー メソッドには、次の例のように、例外で渡すことができるオーバーロードがあります。</span><span class="sxs-lookup"><span data-stu-id="baea0-256">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="baea0-257">プロバイダーごとに、例外情報の処理方法は異なります。</span><span class="sxs-lookup"><span data-stu-id="baea0-257">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="baea0-258">前述のコードの Debug プロバイダーの出力の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-258">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="baea0-259">ログのフィルター処理</span><span class="sxs-lookup"><span data-stu-id="baea0-259">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="baea0-260">特定のプロバイダーとカテゴリ、またはすべてのプロバイダーまたはすべてのカテゴリに最小ログ レベルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="baea0-260">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="baea0-261">最小レベルを下回るログは、そのプロバイダーに渡されないので、表示または保存されません。</span><span class="sxs-lookup"><span data-stu-id="baea0-261">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="baea0-262">すべてのログを抑制するには、最小ログ レベルに `LogLevel.None` を指定します。</span><span class="sxs-lookup"><span data-stu-id="baea0-262">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="baea0-263">`LogLevel.None` の整数値は 6 であり、`LogLevel.Critical` (5) を超えます。</span><span class="sxs-lookup"><span data-stu-id="baea0-263">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="baea0-264">構成にフィルター規則を作成する</span><span class="sxs-lookup"><span data-stu-id="baea0-264">Create filter rules in configuration</span></span>

<span data-ttu-id="baea0-265">プロジェクト テンプレートのコードでは `CreateDefaultBuilder` が呼び出され、Console プロバイダーと Debug プロバイダーのログ記録が設定されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-265">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="baea0-266">`CreateDefaultBuilder` メソッドは、次のようなコードを使用して、`Logging` セクションの構成を検索するログ記録も設定します。</span><span class="sxs-lookup"><span data-stu-id="baea0-266">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

<span data-ttu-id="baea0-267">次の例のように、構成データでは、プロバイダーとカテゴリごとに最小ログ レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="baea0-267">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="baea0-268">この JSON では、6 個のフィルター規則を作成します。1 つは Debug プロバイダー用、4 つは Console プロバイダー用、1 つはすべてのプロバイダー用です。</span><span class="sxs-lookup"><span data-stu-id="baea0-268">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="baea0-269">`ILogger` オブジェクトが作成されると、各プロバイダーに対して 1 つの規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-269">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="baea0-270">コードのフィルター規則</span><span class="sxs-lookup"><span data-stu-id="baea0-270">Filter rules in code</span></span>

<span data-ttu-id="baea0-271">コードにフィルター規則を登録する方法を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-271">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="baea0-272">2 つ目の `AddFilter` では、プロバイダーの種類名を使用して Debug プロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="baea0-272">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="baea0-273">1 つ目の `AddFilter` は、プロバイダーの種類を指定していないため、すべてのプロバイダーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-273">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="baea0-274">フィルター規則を適用する方法</span><span class="sxs-lookup"><span data-stu-id="baea0-274">How filtering rules are applied</span></span>

<span data-ttu-id="baea0-275">前の例の構成データと `AddFilter` コードでは、次の表に示す規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="baea0-275">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="baea0-276">最初の 6 つは構成例、最後の 2 つはコード例のものです。</span><span class="sxs-lookup"><span data-stu-id="baea0-276">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="baea0-277">数値</span><span class="sxs-lookup"><span data-stu-id="baea0-277">Number</span></span> | <span data-ttu-id="baea0-278">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-278">Provider</span></span>      | <span data-ttu-id="baea0-279">以下から始まるカテゴリ</span><span class="sxs-lookup"><span data-stu-id="baea0-279">Categories that begin with ...</span></span>          | <span data-ttu-id="baea0-280">最小ログ レベル</span><span class="sxs-lookup"><span data-stu-id="baea0-280">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="baea0-281">1</span><span class="sxs-lookup"><span data-stu-id="baea0-281">1</span></span>      | <span data-ttu-id="baea0-282">デバッグ</span><span class="sxs-lookup"><span data-stu-id="baea0-282">Debug</span></span>         | <span data-ttu-id="baea0-283">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="baea0-283">All categories</span></span>                          | <span data-ttu-id="baea0-284">情報</span><span class="sxs-lookup"><span data-stu-id="baea0-284">Information</span></span>       |
| <span data-ttu-id="baea0-285">2</span><span class="sxs-lookup"><span data-stu-id="baea0-285">2</span></span>      | <span data-ttu-id="baea0-286">コンソール</span><span class="sxs-lookup"><span data-stu-id="baea0-286">Console</span></span>       | <span data-ttu-id="baea0-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="baea0-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="baea0-288">警告</span><span class="sxs-lookup"><span data-stu-id="baea0-288">Warning</span></span>           |
| <span data-ttu-id="baea0-289">3</span><span class="sxs-lookup"><span data-stu-id="baea0-289">3</span></span>      | <span data-ttu-id="baea0-290">コンソール</span><span class="sxs-lookup"><span data-stu-id="baea0-290">Console</span></span>       | <span data-ttu-id="baea0-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="baea0-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="baea0-292">デバッグ</span><span class="sxs-lookup"><span data-stu-id="baea0-292">Debug</span></span>             |
| <span data-ttu-id="baea0-293">4</span><span class="sxs-lookup"><span data-stu-id="baea0-293">4</span></span>      | <span data-ttu-id="baea0-294">コンソール</span><span class="sxs-lookup"><span data-stu-id="baea0-294">Console</span></span>       | <span data-ttu-id="baea0-295">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="baea0-295">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="baea0-296">Error</span><span class="sxs-lookup"><span data-stu-id="baea0-296">Error</span></span>             |
| <span data-ttu-id="baea0-297">5</span><span class="sxs-lookup"><span data-stu-id="baea0-297">5</span></span>      | <span data-ttu-id="baea0-298">コンソール</span><span class="sxs-lookup"><span data-stu-id="baea0-298">Console</span></span>       | <span data-ttu-id="baea0-299">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="baea0-299">All categories</span></span>                          | <span data-ttu-id="baea0-300">情報</span><span class="sxs-lookup"><span data-stu-id="baea0-300">Information</span></span>       |
| <span data-ttu-id="baea0-301">6</span><span class="sxs-lookup"><span data-stu-id="baea0-301">6</span></span>      | <span data-ttu-id="baea0-302">すべてのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-302">All providers</span></span> | <span data-ttu-id="baea0-303">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="baea0-303">All categories</span></span>                          | <span data-ttu-id="baea0-304">デバッグ</span><span class="sxs-lookup"><span data-stu-id="baea0-304">Debug</span></span>             |
| <span data-ttu-id="baea0-305">7</span><span class="sxs-lookup"><span data-stu-id="baea0-305">7</span></span>      | <span data-ttu-id="baea0-306">すべてのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-306">All providers</span></span> | <span data-ttu-id="baea0-307">システム</span><span class="sxs-lookup"><span data-stu-id="baea0-307">System</span></span>                                  | <span data-ttu-id="baea0-308">デバッグ</span><span class="sxs-lookup"><span data-stu-id="baea0-308">Debug</span></span>             |
| <span data-ttu-id="baea0-309">8</span><span class="sxs-lookup"><span data-stu-id="baea0-309">8</span></span>      | <span data-ttu-id="baea0-310">デバッグ</span><span class="sxs-lookup"><span data-stu-id="baea0-310">Debug</span></span>         | <span data-ttu-id="baea0-311">Microsoft</span><span class="sxs-lookup"><span data-stu-id="baea0-311">Microsoft</span></span>                               | <span data-ttu-id="baea0-312">トレース</span><span class="sxs-lookup"><span data-stu-id="baea0-312">Trace</span></span>             |

<span data-ttu-id="baea0-313">`ILogger` オブジェクトを作成すると、`ILoggerFactory` オブジェクトによって、そのロガーに適用するプロバイダーごとに 1 つの規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-313">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="baea0-314">`ILogger` インスタンスによって書き込まれるすべてのメッセージは、選択した規則に基づいてフィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-314">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="baea0-315">使用できる規則から、各プロバイダーとカテゴリのペアごとに該当する最も限定的な規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-315">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="baea0-316">特定のカテゴリに `ILogger` が作成されるときに、各プロバイダーに次のアルゴリズムが使用されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-316">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="baea0-317">プロバイダーとそのエイリアスと一致するすべての規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-317">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="baea0-318">一致が見つからない場合は、空のプロバイダーですべての規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-318">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="baea0-319">前の手順の結果、最も長いカテゴリのプレフィックスが一致する規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-319">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="baea0-320">一致が見つからない場合は、カテゴリを指定しないすべての規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-320">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="baea0-321">複数の規則が選択されている場合は、**最後**の 1 つが使用されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-321">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="baea0-322">規則が選択されていない場合は、`MinimumLevel` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-322">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="baea0-323">前の規則一覧を使用して、カテゴリ "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" に `ILogger` オブジェクトを作成するとします。</span><span class="sxs-lookup"><span data-stu-id="baea0-323">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="baea0-324">Debug プロバイダーの場合、規則 1、6、8 が適用されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-324">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="baea0-325">規則 8 が最も限定的なので、規則 8 が選択されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-325">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="baea0-326">Console プロバイダーの場合、規則 3、4、5、6 が適用されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-326">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="baea0-327">規則 3 が最も限定的です。</span><span class="sxs-lookup"><span data-stu-id="baea0-327">Rule 3 is most specific.</span></span>

<span data-ttu-id="baea0-328">作成される `ILogger` インスタンスでは、Debug プロバイダーに `Trace` レベル以上のログが送信されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-328">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="baea0-329">`Debug` レベル以上のログが Console プロバイダーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-329">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="baea0-330">プロバイダーのエイリアス</span><span class="sxs-lookup"><span data-stu-id="baea0-330">Provider aliases</span></span>

<span data-ttu-id="baea0-331">各プロバイダーでは "*エイリアス*" が定義されます。これは構成で完全修飾型名の代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="baea0-331">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="baea0-332">組み込みのプロバイダーの場合は、次のエイリアスを使用してください。</span><span class="sxs-lookup"><span data-stu-id="baea0-332">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="baea0-333">コンソール</span><span class="sxs-lookup"><span data-stu-id="baea0-333">Console</span></span>
* <span data-ttu-id="baea0-334">デバッグ</span><span class="sxs-lookup"><span data-stu-id="baea0-334">Debug</span></span>
* <span data-ttu-id="baea0-335">EventLog</span><span class="sxs-lookup"><span data-stu-id="baea0-335">EventLog</span></span>
* <span data-ttu-id="baea0-336">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="baea0-336">AzureAppServicesFile</span></span>
* <span data-ttu-id="baea0-337">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="baea0-337">AzureAppServicesBlob</span></span>
* <span data-ttu-id="baea0-338">TraceSource</span><span class="sxs-lookup"><span data-stu-id="baea0-338">TraceSource</span></span>
* <span data-ttu-id="baea0-339">EventSource</span><span class="sxs-lookup"><span data-stu-id="baea0-339">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="baea0-340">既定の最小レベル</span><span class="sxs-lookup"><span data-stu-id="baea0-340">Default minimum level</span></span>

<span data-ttu-id="baea0-341">指定したプロバイダーとカテゴリに適用される構成またはコードの規則がない場合にのみ反映される最小レベルの設定があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-341">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="baea0-342">最小レベルを設定する方法を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-342">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="baea0-343">最小レベルを明示的に設定しない場合、既定値は `Information` です。これは、`Trace` および `Debug` ログが無視されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="baea0-343">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="baea0-344">フィルター関数</span><span class="sxs-lookup"><span data-stu-id="baea0-344">Filter functions</span></span>

<span data-ttu-id="baea0-345">フィルター関数は、構成またはコードによって規則が割り当てられていないすべてのプロバイダーとカテゴリについて呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-345">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="baea0-346">関数内のコードから、プロバイダーの種類、カテゴリ、ログ レベルにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-346">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="baea0-347">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-347">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="baea0-348">一部のログ プロバイダーでは、ログのレベルとカテゴリに基づいてログをストレージ メディアに書き込む場合や無視する場合を指定できます。</span><span class="sxs-lookup"><span data-stu-id="baea0-348">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="baea0-349">`AddConsole` および `AddDebug` 拡張メソッドには、フィルター条件を指定できるオーバーロードが用意されています。</span><span class="sxs-lookup"><span data-stu-id="baea0-349">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="baea0-350">次のサンプル コードを実行すると、Console プロバイダーは `Warning` レベル未満のログを無視し、Debug プロバイダーはフレームワークで作成されたログを無視します。</span><span class="sxs-lookup"><span data-stu-id="baea0-350">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="baea0-351">`AddEventLog` メソッドには、`EventLogSettings` インスタンスを受け取るオーバーロードがあります。このインスタンスの `Filter` プロパティにはフィルター関数を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-351">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="baea0-352">TraceSource プロバイダーには、これらのオーバーロードのいずれも用意されていません。これは、ログ レベルと他のパラメーターが、使用される `SourceSwitch` と `TraceListener` に基づいているためです。</span><span class="sxs-lookup"><span data-stu-id="baea0-352">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="baea0-353">`ILoggerFactory` インスタンスに登録されているすべてのプロバイダーにフィルター規則を設定するには、`WithFilter` 拡張メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="baea0-353">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="baea0-354">以下の例では、フレームワーク ログ ("Microsoft" または "System" で始まるカテゴリ) が警告に制限されますが、アプリケーション コードによって作成されたログはデバッグ レベルで記録されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-354">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="baea0-355">あらゆるログの書き込みを防ぐには、最小のログ レベルとして `LogLevel.None` を指定します。</span><span class="sxs-lookup"><span data-stu-id="baea0-355">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="baea0-356">`LogLevel.None` の整数値は 6 であり、`LogLevel.Critical` (5) を超えます。</span><span class="sxs-lookup"><span data-stu-id="baea0-356">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="baea0-357">`WithFilter` 拡張メソッドは [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet パッケージで提供されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-357">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="baea0-358">このメソッドは、登録されているすべてのログ プロバイダーに渡されたログ メッセージをフィルター処理する新しい `ILoggerFactory` インスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="baea0-358">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="baea0-359">これは、元の `ILoggerFactory` インスタンスを含め、他の `ILoggerFactory` インスタンスには影響がありません。</span><span class="sxs-lookup"><span data-stu-id="baea0-359">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="baea0-360">システムのカテゴリとレベル</span><span class="sxs-lookup"><span data-stu-id="baea0-360">System categories and levels</span></span>

<span data-ttu-id="baea0-361">ASP.NET Core と Entity Framework Core によって使用されるいくつかのカテゴリと、それらから想定されるログの種類に関するメモを、次に示します。</span><span class="sxs-lookup"><span data-stu-id="baea0-361">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="baea0-362">カテゴリ</span><span class="sxs-lookup"><span data-stu-id="baea0-362">Category</span></span>                            | <span data-ttu-id="baea0-363">メモ</span><span class="sxs-lookup"><span data-stu-id="baea0-363">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="baea0-364">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="baea0-364">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="baea0-365">ASP.NET Core の一般的な診断。</span><span class="sxs-lookup"><span data-stu-id="baea0-365">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="baea0-366">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="baea0-366">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="baea0-367">どのキーが検討、検索、および使用されたか。</span><span class="sxs-lookup"><span data-stu-id="baea0-367">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="baea0-368">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="baea0-368">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="baea0-369">許可されるホスト。</span><span class="sxs-lookup"><span data-stu-id="baea0-369">Hosts allowed.</span></span> |
| <span data-ttu-id="baea0-370">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="baea0-370">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="baea0-371">HTTP 要求が完了するまでにかかった時間と、それらの開始時刻。</span><span class="sxs-lookup"><span data-stu-id="baea0-371">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="baea0-372">どのホスティング スタートアップ アセンブリが読み込まれたか。</span><span class="sxs-lookup"><span data-stu-id="baea0-372">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="baea0-373">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="baea0-373">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="baea0-374">MVC と Razor の診断。</span><span class="sxs-lookup"><span data-stu-id="baea0-374">MVC and Razor diagnostics.</span></span> <span data-ttu-id="baea0-375">モデルの構築、フィルター処理の実行、ビューのコンパイル、アクションの選択。</span><span class="sxs-lookup"><span data-stu-id="baea0-375">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="baea0-376">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="baea0-376">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="baea0-377">一致する情報をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="baea0-377">Route matching information.</span></span> |
| <span data-ttu-id="baea0-378">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="baea0-378">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="baea0-379">接続の開始、停止、キープ アライブ応答。</span><span class="sxs-lookup"><span data-stu-id="baea0-379">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="baea0-380">HTTPS 証明書情報。</span><span class="sxs-lookup"><span data-stu-id="baea0-380">HTTPS certificate information.</span></span> |
| <span data-ttu-id="baea0-381">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="baea0-381">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="baea0-382">提供されるファイル。</span><span class="sxs-lookup"><span data-stu-id="baea0-382">Files served.</span></span> |
| <span data-ttu-id="baea0-383">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="baea0-383">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="baea0-384">Entity Framework Core の一般的な診断。</span><span class="sxs-lookup"><span data-stu-id="baea0-384">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="baea0-385">データベースのアクティビティと構成、変更の検出、移行。</span><span class="sxs-lookup"><span data-stu-id="baea0-385">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="baea0-386">ログのスコープ</span><span class="sxs-lookup"><span data-stu-id="baea0-386">Log scopes</span></span>

 <span data-ttu-id="baea0-387">"*スコープ*" では、論理操作のセットをグループ化できます。</span><span class="sxs-lookup"><span data-stu-id="baea0-387">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="baea0-388">このグループ化を使用して、セットの一部として作成される各ログに同じデータをアタッチすることができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-388">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="baea0-389">たとえば、トランザクション処理の一部として作成されるすべてのログに、トランザクション ID を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-389">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="baea0-390">スコープは <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> メソッドから返される `IDisposable` の種類であり、破棄されるまで継続します。</span><span class="sxs-lookup"><span data-stu-id="baea0-390">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="baea0-391">ロガーの呼び出しを `using` ブロックでラップすることによって、スコープを使用します。</span><span class="sxs-lookup"><span data-stu-id="baea0-391">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="baea0-392">次のコードでは、Console プロバイダーのスコープを有効にしています。</span><span class="sxs-lookup"><span data-stu-id="baea0-392">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="baea0-393">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="baea0-393">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="baea0-394">スコープベースのログ記録を有効にするには、`IncludeScopes` コンソールのロガー オプションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-394">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="baea0-395">構成について詳しくは、「[構成](#configuration)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="baea0-395">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="baea0-396">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="baea0-396">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="baea0-397">スコープベースのログ記録を有効にするには、`IncludeScopes` コンソールのロガー オプションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-397">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="baea0-398">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="baea0-398">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="baea0-399">各ログ メッセージには、スコープ内の情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="baea0-399">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="baea0-400">組み込みのログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-400">Built-in logging providers</span></span>

<span data-ttu-id="baea0-401">ASP.NET Core には次のプロバイダーが付属しています。</span><span class="sxs-lookup"><span data-stu-id="baea0-401">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="baea0-402">コンソール</span><span class="sxs-lookup"><span data-stu-id="baea0-402">Console</span></span>](#console-provider)
* [<span data-ttu-id="baea0-403">デバッグ</span><span class="sxs-lookup"><span data-stu-id="baea0-403">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="baea0-404">EventSource</span><span class="sxs-lookup"><span data-stu-id="baea0-404">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="baea0-405">EventLog</span><span class="sxs-lookup"><span data-stu-id="baea0-405">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="baea0-406">TraceSource</span><span class="sxs-lookup"><span data-stu-id="baea0-406">TraceSource</span></span>](#tracesource-provider)

<span data-ttu-id="baea0-407">[Azure でのログ記録](#logging-in-azure)用のオプションについては、この記事で後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="baea0-407">Options for [Logging in Azure](#logging-in-azure) are covered later in this article.</span></span>

<span data-ttu-id="baea0-408">stdout ログについては、<xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> と <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="baea0-408">For information about stdout logging, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> and <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="baea0-409">Console プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-409">Console provider</span></span>

<span data-ttu-id="baea0-410">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) プロバイダー パッケージは、ログ出力をコンソールに送信します。</span><span class="sxs-lookup"><span data-stu-id="baea0-410">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="baea0-411">[AddConsole オーバーロード](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions)を使用すると、最小ログ レベル、フィルター関数、スコープがサポートされているかどうかを示すブール値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-411">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="baea0-412">もう 1 つの選択肢として、`IConfiguration` オブジェクトを渡す方法があります。この場合、スコープのサポートとログ レベルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="baea0-412">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="baea0-413">Console プロバイダーはパフォーマンスに大きな影響を及ぼすため、一般的に運用環境での使用には適していません。</span><span class="sxs-lookup"><span data-stu-id="baea0-413">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="baea0-414">Visual Studio で新しいプロジェクトを作成する場合、`AddConsole` メソッドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="baea0-414">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="baea0-415">このコードは、*appSettings.json* ファイルの `Logging` セクションを参照しています。</span><span class="sxs-lookup"><span data-stu-id="baea0-415">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="baea0-416">この設定ではフレームワークのログを警告に制限していますが、「[ログのフィルター処理](#log-filtering)」セクションで説明したように、アプリではデバッグ レベルでログに記録することができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-416">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="baea0-417">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="baea0-417">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="baea0-418">コンソールのログ出力を確認するには、プロジェクト フォルダーでコマンド プロンプトを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="baea0-418">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="baea0-419">Debug プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-419">Debug provider</span></span>

<span data-ttu-id="baea0-420">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) プロバイダー パッケージは、[System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) クラス (`Debug.WriteLine` メソッドの呼び出し) を使用してログの出力を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="baea0-420">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="baea0-421">Linux では、このプロバイダーから */var/log/message* にログが書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="baea0-421">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="baea0-422">[AddDebug オーバーロード](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions)を使用すると、最小ログ レベルまたはフィルター関数を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-422">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="baea0-423">EventSource プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-423">EventSource provider</span></span>

<span data-ttu-id="baea0-424">ASP.NET Core 1.1.0 以降をターゲットとするアプリの場合、[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) プロバイダー パッケージは、イベントのトレースを実装できます。</span><span class="sxs-lookup"><span data-stu-id="baea0-424">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="baea0-425">Windows では、[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803) を使用します。</span><span class="sxs-lookup"><span data-stu-id="baea0-425">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="baea0-426">プロバイダーはクロスプラットフォームですが、Linux または macOS 用のイベント コレクションはまだ存在せず、ツールは表示されません。</span><span class="sxs-lookup"><span data-stu-id="baea0-426">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

<span data-ttu-id="baea0-427">ログの収集と表示には、[PerfView utility](https://github.com/Microsoft/perfview) を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="baea0-427">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="baea0-428">ETW ログを表示できる他のツールはありますが、ASP.NET から出力される ETW イベントを操作する場合、PerfView は最適なエクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="baea0-428">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="baea0-429">このプロバイダーでログに記録されるイベントを収集するように PerfView を構成するには、**[追加プロバイダー]** の一覧に文字列 `*Microsoft-Extensions-Logging` を追加します </span><span class="sxs-lookup"><span data-stu-id="baea0-429">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="baea0-430">(文字列の先頭に忘れずにアスタリスクを付けてください)。</span><span class="sxs-lookup"><span data-stu-id="baea0-430">(Don't miss the asterisk at the start of the string.)</span></span>

![Perfview の追加プロバイダー](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="baea0-432">Windows EventLog プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-432">Windows EventLog provider</span></span>

<span data-ttu-id="baea0-433">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) プロバイダー パッケージは、ログ出力を Windows イベント ログに送信します。</span><span class="sxs-lookup"><span data-stu-id="baea0-433">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="baea0-434">[AddEventLog オーバーロード](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)を使用すると、`EventLogSettings` または最小ログ レベルを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-434">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="baea0-435">TraceSource プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-435">TraceSource provider</span></span>

<span data-ttu-id="baea0-436">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) プロバイダー パッケージでは、<xref:System.Diagnostics.TraceSource> ライブラリとプロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-436">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

<span data-ttu-id="baea0-437">[AddTraceSource オーバーロード](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions)を使用すると、ソース スイッチとトレース リスナーを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-437">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="baea0-438">このプロバイダーを使用するには、アプリを (.NET Core ではなく) .NET Framework 上で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="baea0-438">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="baea0-439">このプロバイダーでは、サンプル アプリで使用されている <xref:System.Diagnostics.TextWriterTraceListener> など、さまざまな[リスナー](/dotnet/framework/debug-trace-profile/trace-listeners)にメッセージをルーティングさせることができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-439">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="baea0-440">次の例では、`Warning` 以上のメッセージのログをコンソール ウィンドウに出力する `TraceSource` プロバイダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="baea0-440">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a><span data-ttu-id="baea0-441">Azure でのログ記録</span><span class="sxs-lookup"><span data-stu-id="baea0-441">Logging in Azure</span></span>

<span data-ttu-id="baea0-442">Azure でのログ記録については、次のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="baea0-442">For information about logging in Azure, see the following sections:</span></span>

* [<span data-ttu-id="baea0-443">Azure App Service プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-443">Azure App Service provider</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="baea0-444">Azure ログのストリーミング</span><span class="sxs-lookup"><span data-stu-id="baea0-444">Azure log streaming</span></span>](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [<span data-ttu-id="baea0-445">Azure Application Insights のトレース ログ</span><span class="sxs-lookup"><span data-stu-id="baea0-445">Azure Application Insights trace logging</span></span>](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="baea0-446">Azure App Service プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-446">Azure App Service provider</span></span>

<span data-ttu-id="baea0-447">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) プロバイダー パッケージは、Azure App Service アプリのファイル システムのテキスト ファイルと、Azure Storage アカウントの [BLOB ストレージ](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)にログを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="baea0-447">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="baea0-448">プロバイダー パッケージは、.NET Core 1.1 以降を対象とするアプリで使用可能です。</span><span class="sxs-lookup"><span data-stu-id="baea0-448">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="baea0-449">.NET Core を対象とする場合は、次の点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="baea0-449">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="baea0-450">プロバイダー パッケージは、ASP.NET Core の [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="baea0-450">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="baea0-451">プロバイダー パッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)には含まれません。</span><span class="sxs-lookup"><span data-stu-id="baea0-451">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="baea0-452">プロバイダーを使用するには、パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="baea0-452">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="baea0-453">.NET Framework をターゲットとする場合、または `Microsoft.AspNetCore.App` を参照している場合は、プロバイダー パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="baea0-453">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="baea0-454">`AddAzureWebAppDiagnostics` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="baea0-454">Invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="baea0-455"><xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> のオーバーロードによって <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings> を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-455">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="baea0-456">設定オブジェクトで既定の設定をオーバーライドできます。たとえば、ログの出力テンプレート、BLOB 名、ファイル サイズの制限などです。</span><span class="sxs-lookup"><span data-stu-id="baea0-456">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="baea0-457">("*出力テンプレート*" は、`ILogger` メソッドの呼び出しと共に指定されるものに加えて、すべてのログに適用されるメッセージ テンプレートです。)</span><span class="sxs-lookup"><span data-stu-id="baea0-457">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="baea0-458">プロバイダーの設定を構成するには、次の例のように <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> と <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions> を使用します。</span><span class="sxs-lookup"><span data-stu-id="baea0-458">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="baea0-459">アプリケーションを App Service アプリにデプロイすると、Azure portal の **[App Service]** ページの [[診断ログ]](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) セクションで指定された設定が適用されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-459">When you deploy to an App Service app, the application honors the settings in the [Diagnostic Logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="baea0-460">これらの設定が更新されると、アプリの再起動や再デプロイを必要とせずに、変更がすぐに有効になります。</span><span class="sxs-lookup"><span data-stu-id="baea0-460">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Azure のログ設定](index/_static/azure-logging-settings.png)

<span data-ttu-id="baea0-462">ログ ファイルの既定の場所は、*D:\\home\\LogFiles\\Application* です。既定のファイル名は *diagnostics-yyyymmdd.txt* です。</span><span class="sxs-lookup"><span data-stu-id="baea0-462">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="baea0-463">既定のファイル サイズ制限は 10 MB です。保持されるファイルの既定の最大数は 2 です。</span><span class="sxs-lookup"><span data-stu-id="baea0-463">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="baea0-464">既定の BLOB 名は *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt* です。</span><span class="sxs-lookup"><span data-stu-id="baea0-464">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="baea0-465">このプロバイダーは、プロジェクトが Azure 環境で実行される場合にのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="baea0-465">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="baea0-466">プロジェクトをローカルで実行しても、効果はありません&mdash;BLOB のローカル ファイルまたはローカル開発ストレージへの書き込みは行われません。</span><span class="sxs-lookup"><span data-stu-id="baea0-466">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

### <a name="azure-log-streaming"></a><span data-ttu-id="baea0-467">Azure ログのストリーミング</span><span class="sxs-lookup"><span data-stu-id="baea0-467">Azure log streaming</span></span>

<span data-ttu-id="baea0-468">Azure ログのストリーミングを使用すると、以下からリアル タイムでログ アクティビティを確認できます。</span><span class="sxs-lookup"><span data-stu-id="baea0-468">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="baea0-469">アプリ サーバー</span><span class="sxs-lookup"><span data-stu-id="baea0-469">The app server</span></span>
* <span data-ttu-id="baea0-470">Web サーバー</span><span class="sxs-lookup"><span data-stu-id="baea0-470">The web server</span></span>
* <span data-ttu-id="baea0-471">失敗した要求のトレース</span><span class="sxs-lookup"><span data-stu-id="baea0-471">Failed request tracing</span></span>

<span data-ttu-id="baea0-472">Azure ログのストリーミングを構成するには</span><span class="sxs-lookup"><span data-stu-id="baea0-472">To configure Azure log streaming:</span></span>

* <span data-ttu-id="baea0-473">アプリのポータル ページから **[診断ログ]** ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="baea0-473">Navigate to the **Diagnostics Logs** page from your app's portal page.</span></span>
* <span data-ttu-id="baea0-474">**[アプリケーション ログ (ファイル システム)]** を **[オン]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="baea0-474">Set **Application Logging (Filesystem)** to **On**.</span></span>

![Azure Portal の [診断ログ] ページ](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="baea0-476">**[ログ ストリーミング]** ページに移動して、アプリのメッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="baea0-476">Navigate to the **Log Streaming** page to view app messages.</span></span> <span data-ttu-id="baea0-477">これらはアプリによって、`ILogger` インターフェイスを介してログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="baea0-477">They're logged by the app through the `ILogger` interface.</span></span>

![Azure Portal のアプリケーション ログのストリーミング](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="baea0-479">Azure Application Insights のトレース ログ</span><span class="sxs-lookup"><span data-stu-id="baea0-479">Azure Application Insights trace logging</span></span>

<span data-ttu-id="baea0-480">Application Insights SDK では、ASP.NET Core のログ インフラストラクチャによって生成されるログを収集およびレポートすることができます。</span><span class="sxs-lookup"><span data-stu-id="baea0-480">The Application Insights SDK can collect and report logs generated by the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="baea0-481">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="baea0-481">For more information, see the following resources:</span></span>

* [<span data-ttu-id="baea0-482">Application Insights の概要</span><span class="sxs-lookup"><span data-stu-id="baea0-482">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* [<span data-ttu-id="baea0-483">ASP.NET Core 用 Application Insights</span><span class="sxs-lookup"><span data-stu-id="baea0-483">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* <span data-ttu-id="baea0-484">[Application Insights のログ アダプター](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md)。</span><span class="sxs-lookup"><span data-stu-id="baea0-484">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* [<span data-ttu-id="baea0-485">Application Insights ILogger の実装のサンプル</span><span class="sxs-lookup"><span data-stu-id="baea0-485">Application Insights ILogger implementation samples</span></span>](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a><span data-ttu-id="baea0-486">サードパーティ製のログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="baea0-486">Third-party logging providers</span></span>

<span data-ttu-id="baea0-487">ASP.NET Core で使用できるサードパーティ製のログ記録フレームワークをいくつか紹介します。</span><span class="sxs-lookup"><span data-stu-id="baea0-487">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="baea0-488">[elmah.io](https://elmah.io/) ([GitHub リポジトリ](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="baea0-488">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="baea0-489">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub リポジトリ](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="baea0-489">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="baea0-490">[JSNLog](http://jsnlog.com/) ([GitHub リポジトリ](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="baea0-490">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="baea0-491">[KissLog.net](https://kisslog.net/) ([GitHub リポジトリ](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="baea0-491">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="baea0-492">[Loggr](http://loggr.net/) ([GitHub リポジトリ](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="baea0-492">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="baea0-493">[NLog](http://nlog-project.org/) ([GitHub リポジトリ](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="baea0-493">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="baea0-494">[Sentry](https://sentry.io/welcome/) ([GitHub リポジトリ](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="baea0-494">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="baea0-495">[Serilog](https://serilog.net/) ([GitHub リポジトリ](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="baea0-495">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>
* <span data-ttu-id="baea0-496">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github リポジトリ](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="baea0-496">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="baea0-497">一部のサードパーティ製フレームワークは、[セマンティック ログ記録 (構造化ログ記録とも呼ばれます)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) を実行できます。</span><span class="sxs-lookup"><span data-stu-id="baea0-497">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="baea0-498">サード パーティ製フレームワークを使用することは、組み込みのプロバイダーのいずれかを使用することと似ています。</span><span class="sxs-lookup"><span data-stu-id="baea0-498">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="baea0-499">プロジェクトに NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="baea0-499">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="baea0-500">`ILoggerFactory` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="baea0-500">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="baea0-501">詳細については、各プロバイダーのドキュメントをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="baea0-501">For more information, see each provider's documentation.</span></span> <span data-ttu-id="baea0-502">サード パーティ製のログ プロバイダーは、Microsoft ではサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="baea0-502">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="baea0-503">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="baea0-503">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
