---
title: ASP.NET Core でのログ記録
author: ardalis
description: ASP.NET Core でのログ記録フレームワークについて説明します。 組み込みのログ プロバイダーと、サードパーティ製の一般的なプロバイダーについて説明します。
ms.author: tdykstra
ms.date: 07/24/2018
uid: fundamentals/logging/index
ms.openlocfilehash: f629b062afb5c17cd05040a9ef0281aa7121aabc
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320753"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="add98-104">ASP.NET Core でのログ記録</span><span class="sxs-lookup"><span data-stu-id="add98-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="add98-105">執筆: [Steve Smith](https://ardalis.com/)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="add98-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="add98-106">ASP.NET Core は、さまざまなログ プロバイダーと連携するログ API をサポートします。</span><span class="sxs-lookup"><span data-stu-id="add98-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="add98-107">組み込みのプロバイダーを使用すると、1 つ以上の宛先にログを送信できます。また、サードパーティ製のログ記録フレームワークをプラグインとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="add98-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="add98-108">この記事では、組み込みのログ記録 API とプロバイダーをコードで使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="add98-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="add98-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="add98-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="add98-110">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="add98-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="add98-111">IIS でホストする場合の stdout ログについては、<xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="add98-111">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="add98-112">Azure App Service を使用した stdout ログについては、<xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="add98-112">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="add98-113">ログを作成する方法</span><span class="sxs-lookup"><span data-stu-id="add98-113">How to create logs</span></span>

<span data-ttu-id="add98-114">ログを作成するには、[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーから [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) オブジェクトを実装します。</span><span class="sxs-lookup"><span data-stu-id="add98-114">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="add98-115">次に、そのロガー オブジェクトに対してログ記録メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="add98-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="add98-116">この例では、*カテゴリ*として `TodoController` クラスを使用してログを作成しています。</span><span class="sxs-lookup"><span data-stu-id="add98-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="add98-117">カテゴリについては、[この記事で後ほど](#log-category)説明します。</span><span class="sxs-lookup"><span data-stu-id="add98-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="add98-118">ログ記録はすばやく行う必要があり、非同期を使用するコストに見合わないため、ASP.NET Core には非同期のロガー メソッドが用意されていません。</span><span class="sxs-lookup"><span data-stu-id="add98-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="add98-119">非同期のロガー メソッドが必要な場合は、ログ記録の方法を変更することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="add98-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="add98-120">データ ストアが低速な場合は、まずログ メッセージを高速なストアに書き込んでから、後で低速なストアに移動します。</span><span class="sxs-lookup"><span data-stu-id="add98-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="add98-121">たとえば、メッセージ キューにログを記録し、別のプロセスで読み取り、低速なストレージに保存します。</span><span class="sxs-lookup"><span data-stu-id="add98-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="add98-122">プロバイダーを追加する方法</span><span class="sxs-lookup"><span data-stu-id="add98-122">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="add98-123">`ILogger` オブジェクトで作成したメッセージはログ プロバイダーに受け取られ、表示または保存されます。</span><span class="sxs-lookup"><span data-stu-id="add98-123">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="add98-124">たとえば、Console プロバイダーでコンソールにメッセージが表示され、Azure App Service プロバイダーで Azure Blob Storage に保存されます。</span><span class="sxs-lookup"><span data-stu-id="add98-124">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="add98-125">プロバイダーを使用するには、*Program.cs* でプロバイダーの `Add<ProviderName>` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="add98-125">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="add98-126">既定のプロジェクト テンプレートでは、*Program.cs* 内の [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 拡張メソッドの呼び出しを使用して、コンソールとデバッグのログ プロバイダーが有効にされます。</span><span class="sxs-lookup"><span data-stu-id="add98-126">The default project template enables the Console and Debug logging providers with a call to the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="add98-127">`ILogger` オブジェクトで作成したメッセージはログ プロバイダーに受け取られ、表示または保存されます。</span><span class="sxs-lookup"><span data-stu-id="add98-127">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="add98-128">たとえば、Console プロバイダーでコンソールにメッセージが表示され、Azure App Service プロバイダーで Azure Blob Storage に保存されます。</span><span class="sxs-lookup"><span data-stu-id="add98-128">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="add98-129">プロバイダーを使用するには、NuGet パッケージをインストールし、次の例のように [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) のインスタンスに対してプロバイダーの拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="add98-129">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="add98-130">ASP.NET Core の[依存関係の挿入](xref:fundamentals/dependency-injection) (DI) には、`ILoggerFactory` インスタンスがあります。</span><span class="sxs-lookup"><span data-stu-id="add98-130">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="add98-131">`AddConsole` および `AddDebug` 拡張メソッドは、[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) パッケージと [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) パッケージで定義されています。</span><span class="sxs-lookup"><span data-stu-id="add98-131">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="add98-132">各拡張メソッドで `ILoggerFactory.AddProvider` メソッドを呼び出し、プロバイダーのインスタンスで渡します。</span><span class="sxs-lookup"><span data-stu-id="add98-132">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="add98-133">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample)では、`Startup.Configure` メソッドにログ プロバイダーを追加しています。</span><span class="sxs-lookup"><span data-stu-id="add98-133">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="add98-134">前の手順で実行したコードのログ出力を取得するには、`Startup` クラス コンストラクターにログ プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="add98-134">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="add98-135">この記事の後半では、[組み込みログ プロバイダー](#built-in-logging-providers)について説明し、[サードパーティ製ログ プロバイダー](#third-party-logging-providers)へのリンクを示します。</span><span class="sxs-lookup"><span data-stu-id="add98-135">Learn more about the [built-in logging providers](#built-in-logging-providers) and find links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="configuration"></a><span data-ttu-id="add98-136">構成</span><span class="sxs-lookup"><span data-stu-id="add98-136">Configuration</span></span>

<span data-ttu-id="add98-137">ログ プロバイダーの構成は、1 つまたは複数の構成プロバイダーによって提供されます。</span><span class="sxs-lookup"><span data-stu-id="add98-137">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="add98-138">ファイル形式 (INI、JSON、および XML)。</span><span class="sxs-lookup"><span data-stu-id="add98-138">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="add98-139">コマンド ライン引数。</span><span class="sxs-lookup"><span data-stu-id="add98-139">Command-line arguments.</span></span>
* <span data-ttu-id="add98-140">環境変数。</span><span class="sxs-lookup"><span data-stu-id="add98-140">Environment variables.</span></span>
* <span data-ttu-id="add98-141">メモリ内 .NET オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="add98-141">In-memory .NET objects.</span></span>
* <span data-ttu-id="add98-142">暗号化されていない[シークレット マネージャー](xref:security/app-secrets)の記憶域。</span><span class="sxs-lookup"><span data-stu-id="add98-142">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="add98-143">[Azure Key Vault](xref:security/key-vault-configuration) などの暗号化されたユーザー ストア。</span><span class="sxs-lookup"><span data-stu-id="add98-143">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="add98-144">カスタム プロバイダー (インストール済みまたは作成済み)。</span><span class="sxs-lookup"><span data-stu-id="add98-144">Custom providers (installed or created).</span></span>

<span data-ttu-id="add98-145">たとえば、一般的に、ログの構成はアプリ設定ファイルの `Logging` セクションで指定されます。</span><span class="sxs-lookup"><span data-stu-id="add98-145">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="add98-146">次の例は、一般的な *appsettings.Development.json* ファイルの内容を示しています。</span><span class="sxs-lookup"><span data-stu-id="add98-146">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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
      "IncludeScopes": "true"
    }
  }
}
```

<span data-ttu-id="add98-147">`LogLevel` キーは、ログ名を表します。</span><span class="sxs-lookup"><span data-stu-id="add98-147">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="add98-148">`Default` キーは明示的に表示されていないログに適用されます。</span><span class="sxs-lookup"><span data-stu-id="add98-148">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="add98-149">値は、指定されたログに適用された[ログ レベル](#log-level)を表します。</span><span class="sxs-lookup"><span data-stu-id="add98-149">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="add98-150">`IncludeScopes` を設定するログ キー (例では `Console`) は、示されたログに対して[ログのスコープ](#log-scopes)が有効になっているかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="add98-150">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="add98-151">`LogLevel` キーは、ログ名を表します。</span><span class="sxs-lookup"><span data-stu-id="add98-151">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="add98-152">`Default` キーは明示的に表示されていないログに適用されます。</span><span class="sxs-lookup"><span data-stu-id="add98-152">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="add98-153">値は、指定されたログに適用された[ログ レベル](#log-level)を表します。</span><span class="sxs-lookup"><span data-stu-id="add98-153">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="add98-154">構成プロバイダーの実装について詳しくは、<xref:fundamentals/configuration/index> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="add98-154">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="add98-155">サンプルのログ記録の出力</span><span class="sxs-lookup"><span data-stu-id="add98-155">Sample logging output</span></span>

<span data-ttu-id="add98-156">前のセクションで紹介したサンプル コードをコマンド ラインから実行すると、コンソールにログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="add98-156">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="add98-157">コンソールの出力例は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="add98-157">Here's an example of console output:</span></span>

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

<span data-ttu-id="add98-158">`http://localhost:5000/api/todo/0` にアクセスし、前のセクションで紹介した `ILogger` の呼び出しの実行が両方トリガーされることで、これらのログは作成されました。</span><span class="sxs-lookup"><span data-stu-id="add98-158">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="add98-159">Visual Studio でサンプル アプリケーションを実行すると [デバッグ] ウィンドウに表示されるログと同じログの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="add98-159">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="add98-160">前のセクションで紹介した `ILogger` の呼び出しで作成されるログは、"TodoApi.Controllers.TodoController" から始まります。</span><span class="sxs-lookup"><span data-stu-id="add98-160">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="add98-161">"Microsoft" カテゴリから始まるログは、ASP.NET Core のログです。</span><span class="sxs-lookup"><span data-stu-id="add98-161">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="add98-162">ASP.NET Core 自体とアプリケーション コードは、同じログ記録 API と同じログ プロバイダーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="add98-162">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="add98-163">以降、この記事では、ログ記録の詳細とオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="add98-163">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="add98-164">NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="add98-164">NuGet packages</span></span>

<span data-ttu-id="add98-165">`ILogger` および `ILoggerFactory` インターフェイスは、[Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 内にあり、それらの既定の実装は [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 内にあります。</span><span class="sxs-lookup"><span data-stu-id="add98-165">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="add98-166">ログのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="add98-166">Log category</span></span>

<span data-ttu-id="add98-167">作成する各ログには、*カテゴリ*が含まれています。</span><span class="sxs-lookup"><span data-stu-id="add98-167">A *category* is included with each log that you create.</span></span> <span data-ttu-id="add98-168">`ILogger` オブジェクトを作成するときにカテゴリを指定します。</span><span class="sxs-lookup"><span data-stu-id="add98-168">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="add98-169">カテゴリには任意の文字列を指定できますが、ログを書き込むクラスの完全修飾名を使用することが規則です。</span><span class="sxs-lookup"><span data-stu-id="add98-169">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="add98-170">たとえば、"TodoApi.Controllers.TodoController" と指定します。</span><span class="sxs-lookup"><span data-stu-id="add98-170">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="add98-171">文字列としてカテゴリを指定するか、型からカテゴリを派生させる拡張メソッドを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="add98-171">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="add98-172">文字列としてカテゴリを指定するには、次のように `ILoggerFactory` インスタンスに対して `CreateLogger` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="add98-172">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="add98-173">ほとんどの場合、次の例のように `ILogger<T>` を使用する方が簡単です。</span><span class="sxs-lookup"><span data-stu-id="add98-173">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="add98-174">これは、`T` の完全修飾型名を使用した `CreateLogger` の呼び出しと同じです。</span><span class="sxs-lookup"><span data-stu-id="add98-174">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="add98-175">ログ レベル</span><span class="sxs-lookup"><span data-stu-id="add98-175">Log level</span></span>

<span data-ttu-id="add98-176">ログを書き込むたびに、[LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel) を指定します。</span><span class="sxs-lookup"><span data-stu-id="add98-176">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="add98-177">ログ レベルは、重大度または重要度を示します。</span><span class="sxs-lookup"><span data-stu-id="add98-177">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="add98-178">たとえば、メソッドが通常どおりに終了したときには `Information` ログ、メソッドから 404 リターン コードが返されたときには `Warning` ログ、予期しない例外をキャッチしたときには `Error` ログを書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="add98-178">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="add98-179">次のコード例では、メソッドの名前 (たとえば `LogWarning`) でログ レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="add98-179">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="add98-180">最初のパラメーターは[ログ イベント ID](#log-event-id) です。</span><span class="sxs-lookup"><span data-stu-id="add98-180">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="add98-181">2 つ目のパラメーターは、他のメソッド パラメーターに提供される引数値のプレースホルダーを含む[メッセージ テンプレート](#log-message-template)です。</span><span class="sxs-lookup"><span data-stu-id="add98-181">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="add98-182">メソッド パラメーターの詳細については、この記事で後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="add98-182">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="add98-183">メソッド名にレベルを含むログ メソッドは、[ILogger の拡張メソッド](/dotnet/api/microsoft.extensions.logging.loggerextensions)です。</span><span class="sxs-lookup"><span data-stu-id="add98-183">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="add98-184">これらのメソッドは、背後で `LogLevel` パラメーターを受け取る `Log` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="add98-184">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="add98-185">これらの拡張メソッドのいずれかではなく、`Log` メソッドを直接呼び出すことができますが、構文は比較的複雑です。</span><span class="sxs-lookup"><span data-stu-id="add98-185">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="add98-186">詳細については、[ILogger インターフェイス](/dotnet/api/microsoft.extensions.logging.ilogger)と[ロガー拡張ソース コード](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="add98-186">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="add98-187">ASP.NET Core には、次の[ログ レベル](/dotnet/api/microsoft.extensions.logging.loglevel)が定義されています (重大度の低いレベルから高いレベルの順)。</span><span class="sxs-lookup"><span data-stu-id="add98-187">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="add98-188">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="add98-188">Trace = 0</span></span>

  <span data-ttu-id="add98-189">問題をデバッグする開発者にのみ重要な情報の場合。</span><span class="sxs-lookup"><span data-stu-id="add98-189">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="add98-190">これらのメッセージには機密性の高いアプリケーション データが含まれる可能性があるため、運用環境は有効にしないことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="add98-190">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="add98-191">*既定で無効です。*</span><span class="sxs-lookup"><span data-stu-id="add98-191">*Disabled by default.*</span></span> <span data-ttu-id="add98-192">例 : `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="add98-192">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="add98-193">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="add98-193">Debug = 1</span></span>

  <span data-ttu-id="add98-194">開発およびデバッグ中に短期的に実用性がある情報の場合。</span><span class="sxs-lookup"><span data-stu-id="add98-194">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="add98-195">例: `Entering method Configure with flag set to true.` `Debug` レベルのログはログのサイズが大きくなるため、通常、トラブルシューティングのためではない限り、運用環境では有効にしません。</span><span class="sxs-lookup"><span data-stu-id="add98-195">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="add98-196">Information = 2</span><span class="sxs-lookup"><span data-stu-id="add98-196">Information = 2</span></span>

  <span data-ttu-id="add98-197">アプリケーションの一般的なフローを追跡する場合。</span><span class="sxs-lookup"><span data-stu-id="add98-197">For tracking the general flow of the application.</span></span> <span data-ttu-id="add98-198">通常、これらのログには、長期的な値があります。</span><span class="sxs-lookup"><span data-stu-id="add98-198">These logs typically have some long-term value.</span></span> <span data-ttu-id="add98-199">例 : `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="add98-199">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="add98-200">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="add98-200">Warning = 3</span></span>

  <span data-ttu-id="add98-201">アプリケーション フローの異常なイベントまたは予期しないイベントの場合。</span><span class="sxs-lookup"><span data-stu-id="add98-201">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="add98-202">たとえば、アプリケーションは停止しなくても、調査が必要な可能性があるエラーや他の条件が含まれます。</span><span class="sxs-lookup"><span data-stu-id="add98-202">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="add98-203">`Warning` ログ レベルが使用される一般的な場所として、例外の処理があります。</span><span class="sxs-lookup"><span data-stu-id="add98-203">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="add98-204">例 : `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="add98-204">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="add98-205">Error = 4</span><span class="sxs-lookup"><span data-stu-id="add98-205">Error = 4</span></span>

  <span data-ttu-id="add98-206">処理できないエラーと例外の場合。</span><span class="sxs-lookup"><span data-stu-id="add98-206">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="add98-207">これらのメッセージは、アプリケーション全体のエラーではなく、現在のアクティビティまたは操作 (現在の HTTP 要求など) のエラーを示します。</span><span class="sxs-lookup"><span data-stu-id="add98-207">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="add98-208">ログ メッセージの例: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="add98-208">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="add98-209">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="add98-209">Critical = 5</span></span>

  <span data-ttu-id="add98-210">即時の注意が必要なエラーの場合。</span><span class="sxs-lookup"><span data-stu-id="add98-210">For failures that require immediate attention.</span></span> <span data-ttu-id="add98-211">例: データ損失のシナリオ、ディスク領域不足。</span><span class="sxs-lookup"><span data-stu-id="add98-211">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="add98-212">ログ レベルを使用して、特定のストレージ メディアまたは表示ウィンドウに書き込むログの出力量を制御できます。</span><span class="sxs-lookup"><span data-stu-id="add98-212">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="add98-213">たとえば、運用環境で、`Information` レベル以下のすべてのログをボリューム データ ストアに、`Warning` 以上のすべてのログを値のデータ ストアに書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="add98-213">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="add98-214">開発中は、通常、`Warning` 以上の重大度のログをコンソールに送信することがあります。</span><span class="sxs-lookup"><span data-stu-id="add98-214">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="add98-215">トラブルシューティングが必要になったら、`Debug` レベルを追加できます。</span><span class="sxs-lookup"><span data-stu-id="add98-215">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="add98-216">この記事で後述する「[ログのフィルター処理](#log-filtering)」セクションでは、プロバイダーで処理するログ レベルの制御方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="add98-216">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="add98-217">ASP.NET Core フレームワークは、フレームワーク イベントに対して `Debug` ログ レベルを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="add98-217">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="add98-218">この記事で前述したログの例では、`Information` レベル以下のログを除外したため、`Debug` ログ レベルは表示されていません。</span><span class="sxs-lookup"><span data-stu-id="add98-218">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="add98-219">Console プロバイダーに関する `Debug` 以上のログを表示するように構成されたサンプル アプリケーションを実行した場合のコンソール ログ例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="add98-219">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="add98-220">ログ イベント ID</span><span class="sxs-lookup"><span data-stu-id="add98-220">Log event ID</span></span>

<span data-ttu-id="add98-221">ログを書き込むたびに、*イベント ID* を指定します。</span><span class="sxs-lookup"><span data-stu-id="add98-221">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="add98-222">サンプル アプリでは、この処理にローカルで定義された `LoggingEvents` クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="add98-222">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="add98-223">イベント ID は、ログに記録された一連のイベントを関連付けるために使用できる整数値です。</span><span class="sxs-lookup"><span data-stu-id="add98-223">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="add98-224">たとえば、項目をショッピング カートに追加する処理のログをイベント ID 1000 にしたり、購入を完了する処理のログをイベント ID 1001 にしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="add98-224">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="add98-225">ログ記録の出力のイベント ID は、プロバイダーに応じてフィールドに保存されるか、テキスト メッセージに含まれることがあります。</span><span class="sxs-lookup"><span data-stu-id="add98-225">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="add98-226">Debug プロバイダーではイベント ID が表示されませんが、Console プロバイダーではカテゴリの後に角かっこに囲まれたイベント ID が表示されます。</span><span class="sxs-lookup"><span data-stu-id="add98-226">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="add98-227">ログ メッセージ テンプレート</span><span class="sxs-lookup"><span data-stu-id="add98-227">Log message template</span></span>

<span data-ttu-id="add98-228">ログ メッセージを書き込むたびに、メッセージ テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="add98-228">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="add98-229">メッセージ テンプレートは文字列にするか、引数値を配置する名前付きプレースホルダーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="add98-229">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="add98-230">テンプレートは書式設定文字列ではありません。プレースホルダーは番号ではなく名前付きにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="add98-230">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="add98-231">プレースホルダーの名前ではなく、プレースホルダーの順序によって、値の指定に使用されるパラメーターが決まります。</span><span class="sxs-lookup"><span data-stu-id="add98-231">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="add98-232">次のようなコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="add98-232">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="add98-233">結果のログ メッセージは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="add98-233">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="add98-234">ログ記録フレームワークは、ログ プロバイダーが [セマンティック ログ記録 (構造化ログ記録とも呼ばれます)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) を実装できるような方法でメッセージの書式設定を実行します。</span><span class="sxs-lookup"><span data-stu-id="add98-234">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="add98-235">書式設定されたメッセージ テンプレートだけでなく、引数自体がログ記録システムに渡されるので、ログ プロバイダーはメッセージ テンプレートに加えてフィールドとしてもパラメーター値を保存できます。</span><span class="sxs-lookup"><span data-stu-id="add98-235">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="add98-236">ログの出力先を Azure Table Storage にすると、ロガー メソッドの呼び出しは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="add98-236">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="add98-237">各 Azure Table エンティティに `ID` および `RequestTime` プロパティを指定し、ログ データのクエリを簡略化することができます。</span><span class="sxs-lookup"><span data-stu-id="add98-237">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="add98-238">指定した `RequestTime` の範囲内のすべてのログを検索できます。テキスト メッセージから時間を解析する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="add98-238">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="add98-239">ログ記録の例外</span><span class="sxs-lookup"><span data-stu-id="add98-239">Logging exceptions</span></span>

<span data-ttu-id="add98-240">ロガー メソッドには、次の例のように、例外で渡すことができるオーバーロードがあります。</span><span class="sxs-lookup"><span data-stu-id="add98-240">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="add98-241">プロバイダーごとに、例外情報の処理方法は異なります。</span><span class="sxs-lookup"><span data-stu-id="add98-241">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="add98-242">前述のコードの Debug プロバイダーの出力の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="add98-242">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="add98-243">ログのフィルター処理</span><span class="sxs-lookup"><span data-stu-id="add98-243">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="add98-244">特定のプロバイダーとカテゴリ、またはすべてのプロバイダーまたはすべてのカテゴリに最小ログ レベルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="add98-244">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="add98-245">最小レベルを下回るログは、そのプロバイダーに渡されないので、表示または保存されません。</span><span class="sxs-lookup"><span data-stu-id="add98-245">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="add98-246">すべてのログを抑制する場合は、最小ログ レベルに `LogLevel.None` を指定できます。</span><span class="sxs-lookup"><span data-stu-id="add98-246">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="add98-247">`LogLevel.None` の整数値は 6 であり、`LogLevel.Critical` (5) を超えます。</span><span class="sxs-lookup"><span data-stu-id="add98-247">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="add98-248">構成にフィルター規則を作成する</span><span class="sxs-lookup"><span data-stu-id="add98-248">Create filter rules in configuration</span></span>

<span data-ttu-id="add98-249">プロジェクト テンプレートは、`CreateDefaultBuilder` を呼び出して、Console プロバイダーと Debug プロバイダーのログ記録を設定するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="add98-249">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="add98-250">`CreateDefaultBuilder` メソッドは、次のようなコードを使用して、`Logging` セクションの構成を検索するログ記録も設定します。</span><span class="sxs-lookup"><span data-stu-id="add98-250">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="add98-251">次の例のように、構成データでは、プロバイダーとカテゴリごとに最小ログ レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="add98-251">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="add98-252">この JSON では、6 個のフィルター規則を作成します。1 つは Debug プロバイダー用、4 つは Console プロバイダー用、1 つはすべてのプロバイダーに適用されるフィルターです。</span><span class="sxs-lookup"><span data-stu-id="add98-252">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="add98-253">`ILogger` オブジェクトの作成時に、各プロバイダーにこれらの規則のうち 1 つのみを選択する方法を後で説明します。</span><span class="sxs-lookup"><span data-stu-id="add98-253">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="add98-254">コードのフィルター規則</span><span class="sxs-lookup"><span data-stu-id="add98-254">Filter rules in code</span></span>

<span data-ttu-id="add98-255">次の例のように、コードでフィルター規則を登録できます。</span><span class="sxs-lookup"><span data-stu-id="add98-255">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="add98-256">2 つ目の `AddFilter` では、プロバイダーの種類名を使用して Debug プロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="add98-256">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="add98-257">1 つ目の `AddFilter` は、プロバイダーの種類を指定していないため、すべてのプロバイダーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="add98-257">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="add98-258">フィルター規則を適用する方法</span><span class="sxs-lookup"><span data-stu-id="add98-258">How filtering rules are applied</span></span>

<span data-ttu-id="add98-259">前の例の構成データと `AddFilter` コードでは、次の表に示す規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="add98-259">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="add98-260">最初の 6 つは構成例、最後の 2 つはコード例のものです。</span><span class="sxs-lookup"><span data-stu-id="add98-260">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="add98-261">数値</span><span class="sxs-lookup"><span data-stu-id="add98-261">Number</span></span> | <span data-ttu-id="add98-262">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-262">Provider</span></span>      | <span data-ttu-id="add98-263">以下から始まるカテゴリ</span><span class="sxs-lookup"><span data-stu-id="add98-263">Categories that begin with ...</span></span>          | <span data-ttu-id="add98-264">最小ログ レベル</span><span class="sxs-lookup"><span data-stu-id="add98-264">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="add98-265">1</span><span class="sxs-lookup"><span data-stu-id="add98-265">1</span></span>      | <span data-ttu-id="add98-266">デバッグ</span><span class="sxs-lookup"><span data-stu-id="add98-266">Debug</span></span>         | <span data-ttu-id="add98-267">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="add98-267">All categories</span></span>                          | <span data-ttu-id="add98-268">情報</span><span class="sxs-lookup"><span data-stu-id="add98-268">Information</span></span>       |
| <span data-ttu-id="add98-269">2</span><span class="sxs-lookup"><span data-stu-id="add98-269">2</span></span>      | <span data-ttu-id="add98-270">コンソール</span><span class="sxs-lookup"><span data-stu-id="add98-270">Console</span></span>       | <span data-ttu-id="add98-271">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="add98-271">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="add98-272">警告</span><span class="sxs-lookup"><span data-stu-id="add98-272">Warning</span></span>           |
| <span data-ttu-id="add98-273">3</span><span class="sxs-lookup"><span data-stu-id="add98-273">3</span></span>      | <span data-ttu-id="add98-274">コンソール</span><span class="sxs-lookup"><span data-stu-id="add98-274">Console</span></span>       | <span data-ttu-id="add98-275">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="add98-275">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="add98-276">デバッグ</span><span class="sxs-lookup"><span data-stu-id="add98-276">Debug</span></span>             |
| <span data-ttu-id="add98-277">4</span><span class="sxs-lookup"><span data-stu-id="add98-277">4</span></span>      | <span data-ttu-id="add98-278">コンソール</span><span class="sxs-lookup"><span data-stu-id="add98-278">Console</span></span>       | <span data-ttu-id="add98-279">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="add98-279">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="add98-280">Error</span><span class="sxs-lookup"><span data-stu-id="add98-280">Error</span></span>             |
| <span data-ttu-id="add98-281">5</span><span class="sxs-lookup"><span data-stu-id="add98-281">5</span></span>      | <span data-ttu-id="add98-282">コンソール</span><span class="sxs-lookup"><span data-stu-id="add98-282">Console</span></span>       | <span data-ttu-id="add98-283">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="add98-283">All categories</span></span>                          | <span data-ttu-id="add98-284">情報</span><span class="sxs-lookup"><span data-stu-id="add98-284">Information</span></span>       |
| <span data-ttu-id="add98-285">6</span><span class="sxs-lookup"><span data-stu-id="add98-285">6</span></span>      | <span data-ttu-id="add98-286">すべてのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-286">All providers</span></span> | <span data-ttu-id="add98-287">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="add98-287">All categories</span></span>                          | <span data-ttu-id="add98-288">デバッグ</span><span class="sxs-lookup"><span data-stu-id="add98-288">Debug</span></span>             |
| <span data-ttu-id="add98-289">7</span><span class="sxs-lookup"><span data-stu-id="add98-289">7</span></span>      | <span data-ttu-id="add98-290">すべてのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-290">All providers</span></span> | <span data-ttu-id="add98-291">システム</span><span class="sxs-lookup"><span data-stu-id="add98-291">System</span></span>                                  | <span data-ttu-id="add98-292">デバッグ</span><span class="sxs-lookup"><span data-stu-id="add98-292">Debug</span></span>             |
| <span data-ttu-id="add98-293">8</span><span class="sxs-lookup"><span data-stu-id="add98-293">8</span></span>      | <span data-ttu-id="add98-294">デバッグ</span><span class="sxs-lookup"><span data-stu-id="add98-294">Debug</span></span>         | <span data-ttu-id="add98-295">Microsoft</span><span class="sxs-lookup"><span data-stu-id="add98-295">Microsoft</span></span>                               | <span data-ttu-id="add98-296">トレース</span><span class="sxs-lookup"><span data-stu-id="add98-296">Trace</span></span>             |

<span data-ttu-id="add98-297">ログの書き込みに使用する `ILogger` オブジェクトを作成すると、`ILoggerFactory` オブジェクトでそのロガーに適用されるプロバイダーごとの 1 つの規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="add98-297">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="add98-298">その `ILogger` オブジェクトで書き込まれるすべてのメッセージは、選択した規則に基づいてフィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="add98-298">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="add98-299">使用できる規則から、各プロバイダーとカテゴリのペアごとに該当する最も限定的な規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="add98-299">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="add98-300">特定のカテゴリに `ILogger` が作成されるときに、各プロバイダーに次のアルゴリズムが使用されます。</span><span class="sxs-lookup"><span data-stu-id="add98-300">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="add98-301">プロバイダーとそのエイリアスと一致するすべての規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="add98-301">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="add98-302">何も見つからない場合は、空のプロバイダーですべての規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="add98-302">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="add98-303">前の手順の結果、最も長いカテゴリのプレフィックスが一致する規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="add98-303">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="add98-304">何も見つからない場合は、カテゴリを指定しないすべての規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="add98-304">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="add98-305">複数の規則が選択されている場合は、**最後**の 1 つが使用されます。</span><span class="sxs-lookup"><span data-stu-id="add98-305">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="add98-306">規則が選択されていない場合は、`MinimumLevel` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="add98-306">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="add98-307">たとえば、前の規則一覧があり、カテゴリ "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" に `ILogger` オブジェクトを作成するとします。</span><span class="sxs-lookup"><span data-stu-id="add98-307">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="add98-308">Debug プロバイダーの場合、規則 1、6、8 が適用されます。</span><span class="sxs-lookup"><span data-stu-id="add98-308">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="add98-309">規則 8 が最も限定的なので、規則 8 が選択されます。</span><span class="sxs-lookup"><span data-stu-id="add98-309">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="add98-310">Console プロバイダーの場合、規則 3、4、5、6 が適用されます。</span><span class="sxs-lookup"><span data-stu-id="add98-310">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="add98-311">規則 3 が最も限定的です。</span><span class="sxs-lookup"><span data-stu-id="add98-311">Rule 3 is most specific.</span></span>

<span data-ttu-id="add98-312">カテゴリ "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" の `ILogger` を含むログを作成すると、`Trace` レベル以上のログは Debug プロバイダーに送信され、`Debug` レベル以上のログは Console プロバイダーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="add98-312">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="add98-313">プロバイダーのエイリアス</span><span class="sxs-lookup"><span data-stu-id="add98-313">Provider aliases</span></span>

<span data-ttu-id="add98-314">構成では種類の名前を使用してプロバイダーを指定できますが、各プロバイダーには、使いやすい短い*エイリアス*が定義されています。</span><span class="sxs-lookup"><span data-stu-id="add98-314">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="add98-315">組み込みのプロバイダーの場合は、次のエイリアスを使用してください。</span><span class="sxs-lookup"><span data-stu-id="add98-315">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="add98-316">コンソール</span><span class="sxs-lookup"><span data-stu-id="add98-316">Console</span></span>
* <span data-ttu-id="add98-317">デバッグ</span><span class="sxs-lookup"><span data-stu-id="add98-317">Debug</span></span>
* <span data-ttu-id="add98-318">EventLog</span><span class="sxs-lookup"><span data-stu-id="add98-318">EventLog</span></span>
* <span data-ttu-id="add98-319">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="add98-319">AzureAppServices</span></span>
* <span data-ttu-id="add98-320">TraceSource</span><span class="sxs-lookup"><span data-stu-id="add98-320">TraceSource</span></span>
* <span data-ttu-id="add98-321">EventSource</span><span class="sxs-lookup"><span data-stu-id="add98-321">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="add98-322">既定の最小レベル</span><span class="sxs-lookup"><span data-stu-id="add98-322">Default minimum level</span></span>

<span data-ttu-id="add98-323">指定したプロバイダーとカテゴリに適用される構成またはコードの規則がない場合にのみ反映される最小レベルの設定があります。</span><span class="sxs-lookup"><span data-stu-id="add98-323">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="add98-324">最小レベルを設定する方法を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="add98-324">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="add98-325">最小レベルを明示的に設定しない場合、既定値は `Information` です。これは、`Trace` および `Debug` ログが無視されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="add98-325">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="add98-326">フィルター関数</span><span class="sxs-lookup"><span data-stu-id="add98-326">Filter functions</span></span>

<span data-ttu-id="add98-327">フィルター関数には、フィルター規則を適用するコードを記述できます。</span><span class="sxs-lookup"><span data-stu-id="add98-327">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="add98-328">フィルター関数は、構成またはコードによって規則が割り当てられていないすべてのプロバイダーとカテゴリについて呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="add98-328">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="add98-329">この関数内のコードは、プロバイダーの種類、カテゴリ、およびログ レベルにアクセスし、メッセージをログに記録するかどうかを決定することができます。</span><span class="sxs-lookup"><span data-stu-id="add98-329">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="add98-330">例:</span><span class="sxs-lookup"><span data-stu-id="add98-330">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="add98-331">一部のログ プロバイダーでは、ログのレベルとカテゴリに基づいてログをストレージ メディアに書き込む場合や無視する場合を指定できます。</span><span class="sxs-lookup"><span data-stu-id="add98-331">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="add98-332">`AddConsole` および `AddDebug` 拡張メソッドには、フィルター条件で渡すことができるオーバーロードが用意されています。</span><span class="sxs-lookup"><span data-stu-id="add98-332">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="add98-333">次のサンプル コードを実行すると、Console プロバイダーは `Warning` レベル未満のログを無視し、Debug プロバイダーはフレームワークで作成されたログを無視します。</span><span class="sxs-lookup"><span data-stu-id="add98-333">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="add98-334">`AddEventLog` メソッドには、`EventLogSettings` インスタンスを受け取るオーバーロードがあります。このインスタンスの `Filter` プロパティにはフィルター関数を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="add98-334">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="add98-335">TraceSource プロバイダーには、これらのオーバーロードのいずれも用意されていません。これは、ログ レベルと他のパラメーターが、使用される `SourceSwitch` と `TraceListener` に基づいているためです。</span><span class="sxs-lookup"><span data-stu-id="add98-335">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="add98-336">`WithFilter` 拡張メソッドを使用して、`ILoggerFactory` インスタンスに登録されているすべてのプロバイダーにフィルター規則を設定できます。</span><span class="sxs-lookup"><span data-stu-id="add98-336">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="add98-337">以下の例は、フレームワーク ログを警告に制限しますが (カテゴリは "Microsoft" または "System" で始まります)、アプリではデバッグ レベルでログを記録できます。</span><span class="sxs-lookup"><span data-stu-id="add98-337">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="add98-338">フィルター処理を使用して、特定のカテゴリに関するすべてのログが書き込まれないようにするには、そのカテゴリの最小ログ レベルとして `LogLevel.None` を指定できます。</span><span class="sxs-lookup"><span data-stu-id="add98-338">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="add98-339">`LogLevel.None` の整数値は 6 であり、`LogLevel.Critical` (5) を超えます。</span><span class="sxs-lookup"><span data-stu-id="add98-339">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="add98-340">`WithFilter` 拡張メソッドは [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet パッケージで提供されます。</span><span class="sxs-lookup"><span data-stu-id="add98-340">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="add98-341">このメソッドは、登録されているすべてのログ プロバイダーに渡されたログ メッセージをフィルター処理する新しい `ILoggerFactory` インスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="add98-341">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="add98-342">これは、元の `ILoggerFactory` インスタンスを含め、他の `ILoggerFactory` インスタンスには影響がありません。</span><span class="sxs-lookup"><span data-stu-id="add98-342">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="add98-343">ログのスコープ</span><span class="sxs-lookup"><span data-stu-id="add98-343">Log scopes</span></span>

<span data-ttu-id="add98-344">論理操作セットの一部として作成される各ログに同じデータをアタッチするために、*スコープ*内の論理操作セットをグループ化することができます。</span><span class="sxs-lookup"><span data-stu-id="add98-344">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="add98-345">たとえば、状況によっては、トランザクションの処理の一部として作成されるすべてのログにトランザクション ID を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="add98-345">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="add98-346">スコープは [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) メソッドから返される `IDisposable` の種類であり、破棄されるまで継続します。</span><span class="sxs-lookup"><span data-stu-id="add98-346">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="add98-347">スコープを使用するには、次のようにロガーの呼び出しを `using` ブロックでラップします。</span><span class="sxs-lookup"><span data-stu-id="add98-347">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="add98-348">次のコードでは、Console プロバイダーのスコープを有効にしています。</span><span class="sxs-lookup"><span data-stu-id="add98-348">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="add98-349">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="add98-349">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="add98-350">スコープベースのログ記録を有効にするには、`IncludeScopes` コンソールのロガー オプションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="add98-350">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="add98-351">構成について詳しくは、「[構成](#Configuration)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="add98-351">For information on configuration, see the [Configuration](#Configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="add98-352">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="add98-352">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="add98-353">スコープベースのログ記録を有効にするには、`IncludeScopes` コンソールのロガー オプションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="add98-353">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="add98-354">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="add98-354">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="add98-355">各ログ メッセージには、スコープ内の情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="add98-355">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="add98-356">組み込みのログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-356">Built-in logging providers</span></span>

<span data-ttu-id="add98-357">ASP.NET Core には次のプロバイダーが付属しています。</span><span class="sxs-lookup"><span data-stu-id="add98-357">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="add98-358">コンソール</span><span class="sxs-lookup"><span data-stu-id="add98-358">Console</span></span>](#console-provider)
* [<span data-ttu-id="add98-359">デバッグ</span><span class="sxs-lookup"><span data-stu-id="add98-359">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="add98-360">EventSource</span><span class="sxs-lookup"><span data-stu-id="add98-360">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="add98-361">EventLog</span><span class="sxs-lookup"><span data-stu-id="add98-361">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="add98-362">TraceSource</span><span class="sxs-lookup"><span data-stu-id="add98-362">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="add98-363">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="add98-363">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="add98-364">Console プロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-364">Console provider</span></span>

<span data-ttu-id="add98-365">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) プロバイダー パッケージは、ログ出力をコンソールに送信します。</span><span class="sxs-lookup"><span data-stu-id="add98-365">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="add98-366">[AddConsole オーバーロード](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions)を使用すると、最小ログ レベル、フィルター関数、スコープがサポートされているかどうかを示すブール値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="add98-366">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="add98-367">もう 1 つの選択肢として、`IConfiguration` オブジェクトを渡す方法があります。この場合、スコープのサポートとログ レベルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="add98-367">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="add98-368">運用環境で使用する Console プロバイダーを検討している場合は、パフォーマンスに重大な影響がある点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="add98-368">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="add98-369">Visual Studio で新しいプロジェクトを作成する場合、`AddConsole` メソッドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="add98-369">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="add98-370">このコードは、*appSettings.json* ファイルの `Logging` セクションを参照しています。</span><span class="sxs-lookup"><span data-stu-id="add98-370">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="add98-371">この設定ではフレームワークのログを警告に制限していますが、「[ログのフィルター処理](#log-filtering)」セクションで説明したように、アプリではデバッグ レベルでログに記録することができます。</span><span class="sxs-lookup"><span data-stu-id="add98-371">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="add98-372">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="add98-372">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="add98-373">Debug プロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-373">Debug provider</span></span>

<span data-ttu-id="add98-374">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) プロバイダー パッケージは、[System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) クラス (`Debug.WriteLine` メソッドの呼び出し) を使用してログの出力を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="add98-374">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="add98-375">Linux では、このプロバイダーから */var/log/message* にログが書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="add98-375">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="add98-376">[AddDebug オーバーロード](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions)を使用すると、最小ログ レベルまたはフィルター関数を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="add98-376">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="add98-377">EventSource プロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-377">EventSource provider</span></span>

<span data-ttu-id="add98-378">ASP.NET Core 1.1.0 以降をターゲットとするアプリの場合、[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) プロバイダー パッケージは、イベントのトレースを実装できます。</span><span class="sxs-lookup"><span data-stu-id="add98-378">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="add98-379">Windows では、[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803) を使用します。</span><span class="sxs-lookup"><span data-stu-id="add98-379">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="add98-380">プロバイダーはクロスプラットフォームですが、Linux または macOS 用のイベント コレクションはまだ存在せず、ツールは表示されません。</span><span class="sxs-lookup"><span data-stu-id="add98-380">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="add98-381">ログの収集と表示には、[PerfView utility](https://github.com/Microsoft/perfview) を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="add98-381">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="add98-382">ETW ログを表示できる他のツールはありますが、ASP.NET から出力される ETW イベントを操作する場合、PerfView は最適なエクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="add98-382">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="add98-383">このプロバイダーでログに記録されるイベントを収集するように PerfView を構成するには、**[追加プロバイダー]** の一覧に文字列 `*Microsoft-Extensions-Logging` を追加します </span><span class="sxs-lookup"><span data-stu-id="add98-383">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="add98-384">(文字列の先頭に忘れずにアスタリスクを付けてください)。</span><span class="sxs-lookup"><span data-stu-id="add98-384">(Don't miss the asterisk at the start of the string.)</span></span>

![Perfview の追加プロバイダー](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="add98-386">Windows EventLog プロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-386">Windows EventLog provider</span></span>

<span data-ttu-id="add98-387">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) プロバイダー パッケージは、ログ出力を Windows イベント ログに送信します。</span><span class="sxs-lookup"><span data-stu-id="add98-387">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="add98-388">[AddEventLog オーバーロード](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions)を使用すると、`EventLogSettings` または最小ログ レベルを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="add98-388">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="add98-389">TraceSource プロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-389">TraceSource provider</span></span>

<span data-ttu-id="add98-390">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) プロバイダー パッケージは、[System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) のライブラリとプロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="add98-390">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="add98-391">[AddTraceSource オーバーロード](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions)を使用すると、ソース スイッチとトレース リスナーを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="add98-391">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="add98-392">このプロバイダーを使用するには、アプリケーションを (.NET Core ではなく) .NET Framework 上で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="add98-392">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="add98-393">このプロバイダーを使用すると、サンプル アプリケーションで使用されている [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) など、多様な[リスナー](/dotnet/framework/debug-trace-profile/trace-listeners)にメッセージをルーティングすることができます。</span><span class="sxs-lookup"><span data-stu-id="add98-393">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="add98-394">次の例では、`Warning` 以上のメッセージのログをコンソール ウィンドウに出力する `TraceSource` プロバイダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="add98-394">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="add98-395">Azure App Service プロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-395">Azure App Service provider</span></span>

<span data-ttu-id="add98-396">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) プロバイダー パッケージは、Azure App Service アプリのファイル システムのテキスト ファイルと、Azure Storage アカウントの [BLOB ストレージ](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)にログを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="add98-396">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="add98-397">プロバイダー パッケージは、.NET Core 1.1 以降を対象とするアプリで使用可能です。</span><span class="sxs-lookup"><span data-stu-id="add98-397">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="add98-398">.NET Core を対象とする場合は、次の点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="add98-398">If targeting .NET Core, note the following points:</span></span>

* <span data-ttu-id="add98-399">プロバイダー パッケージは ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) に含まれ、[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) には含まれません。</span><span class="sxs-lookup"><span data-stu-id="add98-399">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) but not in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="add98-400">[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) を明示的に呼び出さないでください。</span><span class="sxs-lookup"><span data-stu-id="add98-400">Don't explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="add98-401">アプリを Azure App Service にデプロイすると、プロバイダーはアプリで自動的に使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="add98-401">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="add98-402">.NET Framework をターゲットとする場合、または `Microsoft.AspNetCore.App` を参照している場合は、プロバイダー パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="add98-402">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="add98-403">`AddAzureWebAppDiagnostics` を [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) インスタンス上で呼び出します。</span><span class="sxs-lookup"><span data-stu-id="add98-403">Invoke `AddAzureWebAppDiagnostics` on an [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) instance:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

<span data-ttu-id="add98-404">[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) オーバーロードを使用すると、ログ記録出力テンプレート、BLOB 名、ファイル サイズの制限など、既定の設定をオーバーライドできる [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="add98-404">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="add98-405">(*出力テンプレート*は、`ILogger` メソッドを呼び出すときに指定するテンプレートに加え、すべてのログに適用されるメッセージ テンプレートです)。</span><span class="sxs-lookup"><span data-stu-id="add98-405">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

<span data-ttu-id="add98-406">App Service アプリにデプロイすると、アプリは Azure Portal の **[App Service]** ページの [[診断ログ]](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) セクションに指定された設定を適用します。</span><span class="sxs-lookup"><span data-stu-id="add98-406">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="add98-407">これらの設定が更新されると、アプリの再起動や再デプロイを必要とせずに、変更がすぐに有効になります。</span><span class="sxs-lookup"><span data-stu-id="add98-407">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Azure のログ設定](index/_static/azure-logging-settings.png)

<span data-ttu-id="add98-409">ログ ファイルの既定の場所は、*D:\\home\\LogFiles\\Application* です。既定のファイル名は *diagnostics-yyyymmdd.txt* です。</span><span class="sxs-lookup"><span data-stu-id="add98-409">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="add98-410">既定のファイル サイズ制限は 10 MB です。保持されるファイルの既定の最大数は 2 です。</span><span class="sxs-lookup"><span data-stu-id="add98-410">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="add98-411">既定の BLOB 名は *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt* です。</span><span class="sxs-lookup"><span data-stu-id="add98-411">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="add98-412">既定の動作の詳細については、[AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="add98-412">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="add98-413">このプロバイダーは、プロジェクトが Azure 環境で実行される場合にのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="add98-413">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="add98-414">プロジェクトをローカルで実行しても、効果はありません&mdash;BLOB のローカル ファイルまたはローカル開発ストレージへの書き込みは行われません。</span><span class="sxs-lookup"><span data-stu-id="add98-414">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="add98-415">サードパーティ製のログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="add98-415">Third-party logging providers</span></span>

<span data-ttu-id="add98-416">ASP.NET Core で使用できるサードパーティ製のログ記録フレームワークをいくつか紹介します。</span><span class="sxs-lookup"><span data-stu-id="add98-416">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="add98-417">[elmah.io](https://elmah.io/) ([GitHub リポジトリ](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="add98-417">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="add98-418">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub リポジトリ](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="add98-418">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="add98-419">[JSNLog](http://jsnlog.com/) ([GitHub リポジトリ](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="add98-419">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="add98-420">[Loggr](http://loggr.net/) ([GitHub リポジトリ](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="add98-420">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="add98-421">[NLog](http://nlog-project.org/) ([GitHub リポジトリ](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="add98-421">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="add98-422">[Serilog](https://serilog.net/) ([GitHub リポジトリ](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="add98-422">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="add98-423">一部のサードパーティ製フレームワークは、[セマンティック ログ記録 (構造化ログ記録とも呼ばれます)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) を実行できます。</span><span class="sxs-lookup"><span data-stu-id="add98-423">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="add98-424">サード パーティ製フレームワークを使用することは、組み込みのプロバイダーのいずれかを使用することと似ています。</span><span class="sxs-lookup"><span data-stu-id="add98-424">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="add98-425">プロジェクトに NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="add98-425">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="add98-426">`ILoggerFactory` で拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="add98-426">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="add98-427">詳細については、各フレームワークのドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="add98-427">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="add98-428">Azure ログのストリーミング</span><span class="sxs-lookup"><span data-stu-id="add98-428">Azure log streaming</span></span>

<span data-ttu-id="add98-429">Azure ログのストリーミングを使用すると、リアル タイムで以下のログ アクティビティを確認できます。</span><span class="sxs-lookup"><span data-stu-id="add98-429">Azure log streaming enables you to view log activity in real time from:</span></span>

* <span data-ttu-id="add98-430">アプリケーション サーバー</span><span class="sxs-lookup"><span data-stu-id="add98-430">The application server</span></span>
* <span data-ttu-id="add98-431">Web サーバー</span><span class="sxs-lookup"><span data-stu-id="add98-431">The web server</span></span>
* <span data-ttu-id="add98-432">失敗した要求のトレース</span><span class="sxs-lookup"><span data-stu-id="add98-432">Failed request tracing</span></span>

<span data-ttu-id="add98-433">Azure ログのストリーミングを構成するには</span><span class="sxs-lookup"><span data-stu-id="add98-433">To configure Azure log streaming:</span></span>

* <span data-ttu-id="add98-434">アプリケーションのポータル ページから **[診断ログ]** ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="add98-434">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="add98-435">**[アプリケーション ログ (ファイル システム)]** を [オン] に設定します。</span><span class="sxs-lookup"><span data-stu-id="add98-435">Set **Application Logging (Filesystem)** to on.</span></span>

![Azure Portal の [診断ログ] ページ](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="add98-437">**[ログ ストリーミング]** ページに移動して、アプリケーション メッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="add98-437">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="add98-438">これらのメッセージは、アプリケーションで `ILogger` インターフェイスを介してログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="add98-438">They're logged by application through the `ILogger` interface.</span></span>

![Azure Portal のアプリケーション ログのストリーミング](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="add98-440">Azure Application Insights のトレース ログ</span><span class="sxs-lookup"><span data-stu-id="add98-440">Azure Application Insights trace logging</span></span>

<span data-ttu-id="add98-441">[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK では、ASP.NET Core ログ インフラストラクチャを介して生成されるログからトレース テレメトリを収集することができます。</span><span class="sxs-lookup"><span data-stu-id="add98-441">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="add98-442">詳細については、[Microsoft/ApplicationInsights-aspnetcore の Wiki のログ](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="add98-442">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="add98-443">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="add98-443">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
