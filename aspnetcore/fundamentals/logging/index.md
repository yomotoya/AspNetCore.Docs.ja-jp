---
title: ASP.NET Core でのログ記録
author: ardalis
description: ASP.NET Core でのログ記録フレームワークについて説明します。 組み込みのログ プロバイダーと、サードパーティ製の一般的なプロバイダーについて説明します。
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: 4ceb7886cc9410c3b39beec68c2b11ea3578d851
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077778"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="096db-104">ASP.NET Core でのログ記録</span><span class="sxs-lookup"><span data-stu-id="096db-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="096db-105">執筆: [Steve Smith](https://ardalis.com/)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="096db-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="096db-106">ASP.NET Core は、さまざまなログ プロバイダーと連携するログ API をサポートします。</span><span class="sxs-lookup"><span data-stu-id="096db-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="096db-107">組み込みのプロバイダーを使用すると、1 つ以上の宛先にログを送信できます。また、サードパーティ製のログ記録フレームワークをプラグインとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="096db-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="096db-108">この記事では、組み込みのログ記録 API とプロバイダーをコードで使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="096db-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="096db-110">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="096db-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="096db-112">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="096db-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="096db-113">ログを作成する方法</span><span class="sxs-lookup"><span data-stu-id="096db-113">How to create logs</span></span>

<span data-ttu-id="096db-114">ログを作成するには、[依存関係の挿入](xref:fundamentals/dependency-injection)コンテナーから [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) オブジェクトを実装します。</span><span class="sxs-lookup"><span data-stu-id="096db-114">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="096db-115">次に、そのロガー オブジェクトに対してログ記録メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="096db-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="096db-116">この例では、*カテゴリ*として `TodoController` クラスを使用してログを作成しています。</span><span class="sxs-lookup"><span data-stu-id="096db-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="096db-117">カテゴリについては、[この記事で後ほど](#log-category)説明します。</span><span class="sxs-lookup"><span data-stu-id="096db-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="096db-118">ログ記録はすばやく行う必要があり、非同期を使用するコストに見合わないため、ASP.NET Core には非同期のロガー メソッドが用意されていません。</span><span class="sxs-lookup"><span data-stu-id="096db-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="096db-119">非同期のロガー メソッドが必要な場合は、ログ記録の方法を変更することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="096db-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="096db-120">データ ストアが低速な場合は、まずログ メッセージを高速なストアに書き込んでから、後で低速なストアに移動します。</span><span class="sxs-lookup"><span data-stu-id="096db-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="096db-121">たとえば、メッセージ キューにログを記録し、別のプロセスで読み取り、低速なストレージに保存します。</span><span class="sxs-lookup"><span data-stu-id="096db-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="096db-122">プロバイダーを追加する方法</span><span class="sxs-lookup"><span data-stu-id="096db-122">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="096db-124">`ILogger` オブジェクトで作成したメッセージはログ プロバイダーに受け取られ、表示または保存されます。</span><span class="sxs-lookup"><span data-stu-id="096db-124">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="096db-125">たとえば、Console プロバイダーでコンソールにメッセージが表示され、Azure App Service プロバイダーで Azure Blob Storage に保存されます。</span><span class="sxs-lookup"><span data-stu-id="096db-125">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="096db-126">プロバイダーを使用するには、*Program.cs* でプロバイダーの `Add<ProviderName>` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="096db-126">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="096db-127">既定のプロジェクト テンプレートを使用すると、[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) メソッドを使用したログ記録が有効になります。</span><span class="sxs-lookup"><span data-stu-id="096db-127">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="096db-129">`ILogger` オブジェクトで作成したメッセージはログ プロバイダーに受け取られ、表示または保存されます。</span><span class="sxs-lookup"><span data-stu-id="096db-129">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="096db-130">たとえば、Console プロバイダーでコンソールにメッセージが表示され、Azure App Service プロバイダーで Azure Blob Storage に保存されます。</span><span class="sxs-lookup"><span data-stu-id="096db-130">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="096db-131">プロバイダーを使用するには、NuGet パッケージをインストールし、次の例のように [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) のインスタンスに対してプロバイダーの拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="096db-131">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="096db-132">ASP.NET Core の[依存関係の挿入](xref:fundamentals/dependency-injection) (DI) には、`ILoggerFactory` インスタンスがあります。</span><span class="sxs-lookup"><span data-stu-id="096db-132">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="096db-133">`AddConsole` および `AddDebug` 拡張メソッドは、[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) パッケージと [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) パッケージで定義されています。</span><span class="sxs-lookup"><span data-stu-id="096db-133">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="096db-134">各拡張メソッドで `ILoggerFactory.AddProvider` メソッドを呼び出し、プロバイダーのインスタンスで渡します。</span><span class="sxs-lookup"><span data-stu-id="096db-134">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="096db-135">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample)では、`Startup.Configure` メソッドにログ プロバイダーを追加しています。</span><span class="sxs-lookup"><span data-stu-id="096db-135">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="096db-136">前の手順で実行したコードのログ出力を取得するには、`Startup` クラス コンストラクターにログ プロバイダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="096db-136">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

---

<span data-ttu-id="096db-137">各[組み込みログ プロバイダー](#built-in-logging-providers)と[サードパーティ製ログ プロバイダー](#third-party-logging-providers)のリンクについては、この記事で後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="096db-137">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="096db-138">サンプルのログ記録の出力</span><span class="sxs-lookup"><span data-stu-id="096db-138">Sample logging output</span></span>

<span data-ttu-id="096db-139">前のセクションで紹介したサンプル コードをコマンド ラインから実行すると、コンソールにログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="096db-139">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="096db-140">コンソールの出力例は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="096db-140">Here's an example of console output:</span></span>

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

<span data-ttu-id="096db-141">`http://localhost:5000/api/todo/0` にアクセスし、前のセクションで紹介した `ILogger` の呼び出しの実行が両方トリガーされることで、これらのログは作成されました。</span><span class="sxs-lookup"><span data-stu-id="096db-141">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="096db-142">Visual Studio でサンプル アプリケーションを実行すると [デバッグ] ウィンドウに表示されるログと同じログの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="096db-142">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="096db-143">前のセクションで紹介した `ILogger` の呼び出しで作成されるログは、"TodoApi.Controllers.TodoController" から始まります。</span><span class="sxs-lookup"><span data-stu-id="096db-143">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="096db-144">"Microsoft" カテゴリから始まるログは、ASP.NET Core のログです。</span><span class="sxs-lookup"><span data-stu-id="096db-144">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="096db-145">ASP.NET Core 自体とアプリケーション コードは、同じログ記録 API と同じログ プロバイダーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="096db-145">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="096db-146">以降、この記事では、ログ記録の詳細とオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="096db-146">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="096db-147">NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="096db-147">NuGet packages</span></span>

<span data-ttu-id="096db-148">`ILogger` および `ILoggerFactory` インターフェイスは、[Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) 内にあり、それらの既定の実装は [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) 内にあります。</span><span class="sxs-lookup"><span data-stu-id="096db-148">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="096db-149">ログのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="096db-149">Log category</span></span>

<span data-ttu-id="096db-150">作成する各ログには、*カテゴリ*が含まれています。</span><span class="sxs-lookup"><span data-stu-id="096db-150">A *category* is included with each log that you create.</span></span> <span data-ttu-id="096db-151">`ILogger` オブジェクトを作成するときにカテゴリを指定します。</span><span class="sxs-lookup"><span data-stu-id="096db-151">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="096db-152">カテゴリには任意の文字列を指定できますが、ログを書き込むクラスの完全修飾名を使用することが規則です。</span><span class="sxs-lookup"><span data-stu-id="096db-152">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="096db-153">たとえば、"TodoApi.Controllers.TodoController" と指定します。</span><span class="sxs-lookup"><span data-stu-id="096db-153">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="096db-154">文字列としてカテゴリを指定するか、型からカテゴリを派生させる拡張メソッドを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="096db-154">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="096db-155">文字列としてカテゴリを指定するには、次のように `ILoggerFactory` インスタンスに対して `CreateLogger` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="096db-155">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="096db-156">ほとんどの場合、次の例のように `ILogger<T>` を使用する方が簡単です。</span><span class="sxs-lookup"><span data-stu-id="096db-156">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="096db-157">これは、`T` の完全修飾型名を使用した `CreateLogger` の呼び出しと同じです。</span><span class="sxs-lookup"><span data-stu-id="096db-157">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="096db-158">ログ レベル</span><span class="sxs-lookup"><span data-stu-id="096db-158">Log level</span></span>

<span data-ttu-id="096db-159">ログを書き込むたびに、[LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel) を指定します。</span><span class="sxs-lookup"><span data-stu-id="096db-159">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="096db-160">ログ レベルは、重大度または重要度を示します。</span><span class="sxs-lookup"><span data-stu-id="096db-160">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="096db-161">たとえば、メソッドが通常どおりに終了したときには `Information` ログ、メソッドから 404 リターン コードが返されたときには `Warning` ログ、予期しない例外をキャッチしたときには `Error` ログを書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="096db-161">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="096db-162">次のコード例では、メソッドの名前 (たとえば `LogWarning`) でログ レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="096db-162">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="096db-163">最初のパラメーターは[ログ イベント ID](#log-event-id) です。</span><span class="sxs-lookup"><span data-stu-id="096db-163">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="096db-164">2 つ目のパラメーターは、他のメソッド パラメーターに提供される引数値のプレースホルダーを含む[メッセージ テンプレート](#log-message-template)です。</span><span class="sxs-lookup"><span data-stu-id="096db-164">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="096db-165">メソッド パラメーターの詳細については、この記事で後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="096db-165">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="096db-166">メソッド名にレベルを含むログ メソッドは、[ILogger の拡張メソッド](/dotnet/api/microsoft.extensions.logging.loggerextensions)です。</span><span class="sxs-lookup"><span data-stu-id="096db-166">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="096db-167">これらのメソッドは、背後で `LogLevel` パラメーターを受け取る `Log` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="096db-167">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="096db-168">これらの拡張メソッドのいずれかではなく、`Log` メソッドを直接呼び出すことができますが、構文は比較的複雑です。</span><span class="sxs-lookup"><span data-stu-id="096db-168">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="096db-169">詳細については、[ILogger インターフェイス](/dotnet/api/microsoft.extensions.logging.ilogger)と[ロガー拡張ソース コード](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="096db-169">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="096db-170">ASP.NET Core には、次の[ログ レベル](/dotnet/api/microsoft.extensions.logging.loglevel)が定義されています (重大度の低いレベルから高いレベルの順)。</span><span class="sxs-lookup"><span data-stu-id="096db-170">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="096db-171">Trace = 0</span><span class="sxs-lookup"><span data-stu-id="096db-171">Trace = 0</span></span>

  <span data-ttu-id="096db-172">問題をデバッグする開発者にのみ重要な情報の場合。</span><span class="sxs-lookup"><span data-stu-id="096db-172">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="096db-173">これらのメッセージには機密性の高いアプリケーション データが含まれる可能性があるため、運用環境は有効にしないことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="096db-173">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="096db-174">*既定で無効です。*</span><span class="sxs-lookup"><span data-stu-id="096db-174">*Disabled by default.*</span></span> <span data-ttu-id="096db-175">例 : `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="096db-175">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="096db-176">Debug = 1</span><span class="sxs-lookup"><span data-stu-id="096db-176">Debug = 1</span></span>

  <span data-ttu-id="096db-177">開発およびデバッグ中に短期的に実用性がある情報の場合。</span><span class="sxs-lookup"><span data-stu-id="096db-177">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="096db-178">例: `Entering method Configure with flag set to true.` `Debug` レベルのログはログのサイズが大きくなるため、通常、トラブルシューティングのためではない限り、運用環境では有効にしません。</span><span class="sxs-lookup"><span data-stu-id="096db-178">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="096db-179">Information = 2</span><span class="sxs-lookup"><span data-stu-id="096db-179">Information = 2</span></span>

  <span data-ttu-id="096db-180">アプリケーションの一般的なフローを追跡する場合。</span><span class="sxs-lookup"><span data-stu-id="096db-180">For tracking the general flow of the application.</span></span> <span data-ttu-id="096db-181">通常、これらのログには、長期的な値があります。</span><span class="sxs-lookup"><span data-stu-id="096db-181">These logs typically have some long-term value.</span></span> <span data-ttu-id="096db-182">例 : `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="096db-182">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="096db-183">Warning = 3</span><span class="sxs-lookup"><span data-stu-id="096db-183">Warning = 3</span></span>

  <span data-ttu-id="096db-184">アプリケーション フローの異常なイベントまたは予期しないイベントの場合。</span><span class="sxs-lookup"><span data-stu-id="096db-184">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="096db-185">たとえば、アプリケーションは停止しなくても、調査が必要な可能性があるエラーや他の条件が含まれます。</span><span class="sxs-lookup"><span data-stu-id="096db-185">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="096db-186">`Warning` ログ レベルが使用される一般的な場所として、例外の処理があります。</span><span class="sxs-lookup"><span data-stu-id="096db-186">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="096db-187">例 : `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="096db-187">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="096db-188">Error = 4</span><span class="sxs-lookup"><span data-stu-id="096db-188">Error = 4</span></span>

  <span data-ttu-id="096db-189">処理できないエラーと例外の場合。</span><span class="sxs-lookup"><span data-stu-id="096db-189">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="096db-190">これらのメッセージは、アプリケーション全体のエラーではなく、現在のアクティビティまたは操作 (現在の HTTP 要求など) のエラーを示します。</span><span class="sxs-lookup"><span data-stu-id="096db-190">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="096db-191">ログ メッセージの例: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="096db-191">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="096db-192">Critical = 5</span><span class="sxs-lookup"><span data-stu-id="096db-192">Critical = 5</span></span>

  <span data-ttu-id="096db-193">即時の注意が必要なエラーの場合。</span><span class="sxs-lookup"><span data-stu-id="096db-193">For failures that require immediate attention.</span></span> <span data-ttu-id="096db-194">例: データ損失のシナリオ、ディスク領域不足。</span><span class="sxs-lookup"><span data-stu-id="096db-194">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="096db-195">ログ レベルを使用して、特定のストレージ メディアまたは表示ウィンドウに書き込むログの出力量を制御できます。</span><span class="sxs-lookup"><span data-stu-id="096db-195">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="096db-196">たとえば、運用環境で、`Information` レベル以下のすべてのログをボリューム データ ストアに、`Warning` 以上のすべてのログを値のデータ ストアに書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="096db-196">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="096db-197">開発中は、通常、`Warning` 以上の重大度のログをコンソールに送信することがあります。</span><span class="sxs-lookup"><span data-stu-id="096db-197">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="096db-198">トラブルシューティングが必要になったら、`Debug` レベルを追加できます。</span><span class="sxs-lookup"><span data-stu-id="096db-198">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="096db-199">この記事で後述する「[ログのフィルター処理](#log-filtering)」セクションでは、プロバイダーで処理するログ レベルの制御方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="096db-199">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="096db-200">ASP.NET Core フレームワークは、フレームワーク イベントに対して `Debug` ログ レベルを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="096db-200">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="096db-201">この記事で前述したログの例では、`Information` レベル以下のログを除外したため、`Debug` ログ レベルは表示されていません。</span><span class="sxs-lookup"><span data-stu-id="096db-201">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="096db-202">Console プロバイダーに関する `Debug` 以上のログを表示するように構成されたサンプル アプリケーションを実行した場合のコンソール ログ例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="096db-202">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="096db-203">ログ イベント ID</span><span class="sxs-lookup"><span data-stu-id="096db-203">Log event ID</span></span>

<span data-ttu-id="096db-204">ログを書き込むたびに、*イベント ID* を指定します。</span><span class="sxs-lookup"><span data-stu-id="096db-204">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="096db-205">サンプル アプリでは、この処理にローカルで定義された `LoggingEvents` クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="096db-205">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="096db-206">イベント ID は、ログに記録された一連のイベントを関連付けるために使用できる整数値です。</span><span class="sxs-lookup"><span data-stu-id="096db-206">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="096db-207">たとえば、項目をショッピング カートに追加する処理のログをイベント ID 1000 にしたり、購入を完了する処理のログをイベント ID 1001 にしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="096db-207">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="096db-208">ログ記録の出力のイベント ID は、プロバイダーに応じてフィールドに保存されるか、テキスト メッセージに含まれることがあります。</span><span class="sxs-lookup"><span data-stu-id="096db-208">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="096db-209">Debug プロバイダーではイベント ID が表示されませんが、Console プロバイダーではカテゴリの後に角かっこに囲まれたイベント ID が表示されます。</span><span class="sxs-lookup"><span data-stu-id="096db-209">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="096db-210">ログ メッセージ テンプレート</span><span class="sxs-lookup"><span data-stu-id="096db-210">Log message template</span></span>

<span data-ttu-id="096db-211">ログ メッセージを書き込むたびに、メッセージ テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="096db-211">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="096db-212">メッセージ テンプレートは文字列にするか、引数値を配置する名前付きプレースホルダーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="096db-212">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="096db-213">テンプレートは書式設定文字列ではありません。プレースホルダーは番号ではなく名前付きにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="096db-213">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="096db-214">プレースホルダーの名前ではなく、プレースホルダーの順序によって、値の指定に使用されるパラメーターが決まります。</span><span class="sxs-lookup"><span data-stu-id="096db-214">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="096db-215">次のようなコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="096db-215">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="096db-216">結果のログ メッセージは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="096db-216">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="096db-217">ログ記録フレームワークは、ログ プロバイダーが [セマンティック ログ記録 (構造化ログ記録とも呼ばれます)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) を実装できるような方法でメッセージの書式設定を実行します。</span><span class="sxs-lookup"><span data-stu-id="096db-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="096db-218">書式設定されたメッセージ テンプレートだけでなく、引数自体がログ記録システムに渡されるので、ログ プロバイダーはメッセージ テンプレートに加えてフィールドとしてもパラメーター値を保存できます。</span><span class="sxs-lookup"><span data-stu-id="096db-218">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="096db-219">ログの出力先を Azure Table Storage にすると、ロガー メソッドの呼び出しは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="096db-219">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="096db-220">各 Azure Table エンティティに `ID` および `RequestTime` プロパティを指定し、ログ データのクエリを簡略化することができます。</span><span class="sxs-lookup"><span data-stu-id="096db-220">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="096db-221">指定した `RequestTime` の範囲内のすべてのログを検索できます。テキスト メッセージから時間を解析する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="096db-221">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="096db-222">ログ記録の例外</span><span class="sxs-lookup"><span data-stu-id="096db-222">Logging exceptions</span></span>

<span data-ttu-id="096db-223">ロガー メソッドには、次の例のように、例外で渡すことができるオーバーロードがあります。</span><span class="sxs-lookup"><span data-stu-id="096db-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="096db-224">プロバイダーごとに、例外情報の処理方法は異なります。</span><span class="sxs-lookup"><span data-stu-id="096db-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="096db-225">前述のコードの Debug プロバイダーの出力の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="096db-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="096db-226">ログのフィルター処理</span><span class="sxs-lookup"><span data-stu-id="096db-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="096db-228">特定のプロバイダーとカテゴリ、またはすべてのプロバイダーまたはすべてのカテゴリに最小ログ レベルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="096db-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="096db-229">最小レベルを下回るログは、そのプロバイダーに渡されないので、表示または保存されません。</span><span class="sxs-lookup"><span data-stu-id="096db-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="096db-230">すべてのログを抑制する場合は、最小ログ レベルに `LogLevel.None` を指定できます。</span><span class="sxs-lookup"><span data-stu-id="096db-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="096db-231">`LogLevel.None` の整数値は 6 であり、`LogLevel.Critical` (5) を超えます。</span><span class="sxs-lookup"><span data-stu-id="096db-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="096db-232">**構成にフィルター規則を作成する**</span><span class="sxs-lookup"><span data-stu-id="096db-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="096db-233">プロジェクト テンプレートは、`CreateDefaultBuilder` を呼び出して、Console プロバイダーと Debug プロバイダーのログ記録を設定するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="096db-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="096db-234">`CreateDefaultBuilder` メソッドは、次のようなコードを使用して、`Logging` セクションの構成を検索するログ記録も設定します。</span><span class="sxs-lookup"><span data-stu-id="096db-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="096db-235">次の例のように、構成データでは、プロバイダーとカテゴリごとに最小ログ レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="096db-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="096db-236">この JSON では、6 個のフィルター規則を作成します。1 つは Debug プロバイダー用、4 つは Console プロバイダー用、1 つはすべてのプロバイダーに適用されるフィルターです。</span><span class="sxs-lookup"><span data-stu-id="096db-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="096db-237">`ILogger` オブジェクトの作成時に、各プロバイダーにこれらの規則のうち 1 つのみを選択する方法を後で説明します。</span><span class="sxs-lookup"><span data-stu-id="096db-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="096db-238">**コードのフィルター規則**</span><span class="sxs-lookup"><span data-stu-id="096db-238">**Filter rules in code**</span></span>

<span data-ttu-id="096db-239">次の例のように、コードでフィルター規則を登録できます。</span><span class="sxs-lookup"><span data-stu-id="096db-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="096db-240">2 つ目の `AddFilter` では、プロバイダーの種類名を使用して Debug プロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="096db-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="096db-241">1 つ目の `AddFilter` は、プロバイダーの種類を指定していないため、すべてのプロバイダーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="096db-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="096db-242">**フィルター規則を適用する方法**</span><span class="sxs-lookup"><span data-stu-id="096db-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="096db-243">前の例の構成データと `AddFilter` コードでは、次の表に示す規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="096db-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="096db-244">最初の 6 つは構成例、最後の 2 つはコード例のものです。</span><span class="sxs-lookup"><span data-stu-id="096db-244">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="096db-245">数値</span><span class="sxs-lookup"><span data-stu-id="096db-245">Number</span></span> | <span data-ttu-id="096db-246">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-246">Provider</span></span>      | <span data-ttu-id="096db-247">以下から始まるカテゴリ</span><span class="sxs-lookup"><span data-stu-id="096db-247">Categories that begin with ...</span></span>          | <span data-ttu-id="096db-248">最小ログ レベル</span><span class="sxs-lookup"><span data-stu-id="096db-248">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="096db-249">1</span><span class="sxs-lookup"><span data-stu-id="096db-249">1</span></span>      | <span data-ttu-id="096db-250">デバッグ</span><span class="sxs-lookup"><span data-stu-id="096db-250">Debug</span></span>         | <span data-ttu-id="096db-251">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="096db-251">All categories</span></span>                          | <span data-ttu-id="096db-252">情報</span><span class="sxs-lookup"><span data-stu-id="096db-252">Information</span></span>       |
| <span data-ttu-id="096db-253">2</span><span class="sxs-lookup"><span data-stu-id="096db-253">2</span></span>      | <span data-ttu-id="096db-254">コンソール</span><span class="sxs-lookup"><span data-stu-id="096db-254">Console</span></span>       | <span data-ttu-id="096db-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="096db-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="096db-256">警告</span><span class="sxs-lookup"><span data-stu-id="096db-256">Warning</span></span>           |
| <span data-ttu-id="096db-257">3</span><span class="sxs-lookup"><span data-stu-id="096db-257">3</span></span>      | <span data-ttu-id="096db-258">コンソール</span><span class="sxs-lookup"><span data-stu-id="096db-258">Console</span></span>       | <span data-ttu-id="096db-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="096db-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="096db-260">デバッグ</span><span class="sxs-lookup"><span data-stu-id="096db-260">Debug</span></span>             |
| <span data-ttu-id="096db-261">4</span><span class="sxs-lookup"><span data-stu-id="096db-261">4</span></span>      | <span data-ttu-id="096db-262">コンソール</span><span class="sxs-lookup"><span data-stu-id="096db-262">Console</span></span>       | <span data-ttu-id="096db-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="096db-263">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="096db-264">Error</span><span class="sxs-lookup"><span data-stu-id="096db-264">Error</span></span>             |
| <span data-ttu-id="096db-265">5</span><span class="sxs-lookup"><span data-stu-id="096db-265">5</span></span>      | <span data-ttu-id="096db-266">コンソール</span><span class="sxs-lookup"><span data-stu-id="096db-266">Console</span></span>       | <span data-ttu-id="096db-267">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="096db-267">All categories</span></span>                          | <span data-ttu-id="096db-268">情報</span><span class="sxs-lookup"><span data-stu-id="096db-268">Information</span></span>       |
| <span data-ttu-id="096db-269">6</span><span class="sxs-lookup"><span data-stu-id="096db-269">6</span></span>      | <span data-ttu-id="096db-270">すべてのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-270">All providers</span></span> | <span data-ttu-id="096db-271">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="096db-271">All categories</span></span>                          | <span data-ttu-id="096db-272">デバッグ</span><span class="sxs-lookup"><span data-stu-id="096db-272">Debug</span></span>             |
| <span data-ttu-id="096db-273">7</span><span class="sxs-lookup"><span data-stu-id="096db-273">7</span></span>      | <span data-ttu-id="096db-274">すべてのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-274">All providers</span></span> | <span data-ttu-id="096db-275">システム</span><span class="sxs-lookup"><span data-stu-id="096db-275">System</span></span>                                  | <span data-ttu-id="096db-276">デバッグ</span><span class="sxs-lookup"><span data-stu-id="096db-276">Debug</span></span>             |
| <span data-ttu-id="096db-277">8</span><span class="sxs-lookup"><span data-stu-id="096db-277">8</span></span>      | <span data-ttu-id="096db-278">デバッグ</span><span class="sxs-lookup"><span data-stu-id="096db-278">Debug</span></span>         | <span data-ttu-id="096db-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="096db-279">Microsoft</span></span>                               | <span data-ttu-id="096db-280">トレース</span><span class="sxs-lookup"><span data-stu-id="096db-280">Trace</span></span>             |

<span data-ttu-id="096db-281">ログの書き込みに使用する `ILogger` オブジェクトを作成すると、`ILoggerFactory` オブジェクトでそのロガーに適用されるプロバイダーごとの 1 つの規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="096db-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="096db-282">その `ILogger` オブジェクトで書き込まれるすべてのメッセージは、選択した規則に基づいてフィルター処理されます。</span><span class="sxs-lookup"><span data-stu-id="096db-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="096db-283">使用できる規則から、各プロバイダーとカテゴリのペアごとに該当する最も限定的な規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="096db-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="096db-284">特定のカテゴリに `ILogger` が作成されるときに、各プロバイダーに次のアルゴリズムが使用されます。</span><span class="sxs-lookup"><span data-stu-id="096db-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="096db-285">プロバイダーとそのエイリアスと一致するすべての規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="096db-285">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="096db-286">何も見つからない場合は、空のプロバイダーですべての規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="096db-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="096db-287">前の手順の結果、最も長いカテゴリのプレフィックスが一致する規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="096db-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="096db-288">何も見つからない場合は、カテゴリを指定しないすべての規則が選択されます。</span><span class="sxs-lookup"><span data-stu-id="096db-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="096db-289">複数の規則が選択されている場合は、**最後**の 1 つが使用されます。</span><span class="sxs-lookup"><span data-stu-id="096db-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="096db-290">規則が選択されていない場合は、`MinimumLevel` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="096db-290">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="096db-291">たとえば、前の規則一覧があり、カテゴリ "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" に `ILogger` オブジェクトを作成するとします。</span><span class="sxs-lookup"><span data-stu-id="096db-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="096db-292">Debug プロバイダーの場合、規則 1、6、8 が適用されます。</span><span class="sxs-lookup"><span data-stu-id="096db-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="096db-293">規則 8 が最も限定的なので、規則 8 が選択されます。</span><span class="sxs-lookup"><span data-stu-id="096db-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="096db-294">Console プロバイダーの場合、規則 3、4、5、6 が適用されます。</span><span class="sxs-lookup"><span data-stu-id="096db-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="096db-295">規則 3 が最も限定的です。</span><span class="sxs-lookup"><span data-stu-id="096db-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="096db-296">カテゴリ "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" の `ILogger` を含むログを作成すると、`Trace` レベル以上のログは Debug プロバイダーに送信され、`Debug` レベル以上のログは Console プロバイダーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="096db-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="096db-297">**プロバイダーのエイリアス**</span><span class="sxs-lookup"><span data-stu-id="096db-297">**Provider aliases**</span></span>

<span data-ttu-id="096db-298">構成では種類の名前を使用してプロバイダーを指定できますが、各プロバイダーには、使いやすい短い*エイリアス*が定義されています。</span><span class="sxs-lookup"><span data-stu-id="096db-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="096db-299">組み込みのプロバイダーの場合は、次のエイリアスを使用してください。</span><span class="sxs-lookup"><span data-stu-id="096db-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="096db-300">コンソール</span><span class="sxs-lookup"><span data-stu-id="096db-300">Console</span></span>
- <span data-ttu-id="096db-301">デバッグ</span><span class="sxs-lookup"><span data-stu-id="096db-301">Debug</span></span>
- <span data-ttu-id="096db-302">EventLog</span><span class="sxs-lookup"><span data-stu-id="096db-302">EventLog</span></span>
- <span data-ttu-id="096db-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="096db-303">AzureAppServices</span></span>
- <span data-ttu-id="096db-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="096db-304">TraceSource</span></span>
- <span data-ttu-id="096db-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="096db-305">EventSource</span></span>

<span data-ttu-id="096db-306">**既定の最小レベル**</span><span class="sxs-lookup"><span data-stu-id="096db-306">**Default minimum level**</span></span>

<span data-ttu-id="096db-307">指定したプロバイダーとカテゴリに適用される構成またはコードの規則がない場合にのみ反映される最小レベルの設定があります。</span><span class="sxs-lookup"><span data-stu-id="096db-307">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="096db-308">最小レベルを設定する方法を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="096db-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="096db-309">最小レベルを明示的に設定しない場合、既定値は `Information` です。これは、`Trace` および `Debug` ログが無視されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="096db-309">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="096db-310">**フィルター関数**</span><span class="sxs-lookup"><span data-stu-id="096db-310">**Filter functions**</span></span>

<span data-ttu-id="096db-311">フィルター関数には、フィルター規則を適用するコードを記述できます。</span><span class="sxs-lookup"><span data-stu-id="096db-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="096db-312">フィルター関数は、構成またはコードによって規則が割り当てられていないすべてのプロバイダーとカテゴリについて呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="096db-312">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="096db-313">この関数内のコードは、プロバイダーの種類、カテゴリ、およびログ レベルにアクセスし、メッセージをログに記録するかどうかを決定することができます。</span><span class="sxs-lookup"><span data-stu-id="096db-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="096db-314">例:</span><span class="sxs-lookup"><span data-stu-id="096db-314">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="096db-316">一部のログ プロバイダーでは、ログのレベルとカテゴリに基づいてログをストレージ メディアに書き込む場合や無視する場合を指定できます。</span><span class="sxs-lookup"><span data-stu-id="096db-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="096db-317">`AddConsole` および `AddDebug` 拡張メソッドには、フィルター条件で渡すことができるオーバーロードが用意されています。</span><span class="sxs-lookup"><span data-stu-id="096db-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="096db-318">次のサンプル コードを実行すると、Console プロバイダーは `Warning` レベル未満のログを無視し、Debug プロバイダーはフレームワークで作成されたログを無視します。</span><span class="sxs-lookup"><span data-stu-id="096db-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="096db-319">`AddEventLog` メソッドには、`EventLogSettings` インスタンスを受け取るオーバーロードがあります。このインスタンスの `Filter` プロパティにはフィルター関数を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="096db-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="096db-320">TraceSource プロバイダーには、これらのオーバーロードのいずれも用意されていません。これは、ログ レベルと他のパラメーターが、使用される `SourceSwitch` と `TraceListener` に基づいているためです。</span><span class="sxs-lookup"><span data-stu-id="096db-320">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="096db-321">`WithFilter` 拡張メソッドを使用して、`ILoggerFactory` インスタンスに登録されているすべてのプロバイダーにフィルター規則を設定できます。</span><span class="sxs-lookup"><span data-stu-id="096db-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="096db-322">以下の例は、フレームワーク ログを警告に制限しますが (カテゴリは "Microsoft" または "System" で始まります)、アプリではデバッグ レベルでログを記録できます。</span><span class="sxs-lookup"><span data-stu-id="096db-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="096db-323">フィルター処理を使用して、特定のカテゴリに関するすべてのログが書き込まれないようにするには、そのカテゴリの最小ログ レベルとして `LogLevel.None` を指定できます。</span><span class="sxs-lookup"><span data-stu-id="096db-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="096db-324">`LogLevel.None` の整数値は 6 であり、`LogLevel.Critical` (5) を超えます。</span><span class="sxs-lookup"><span data-stu-id="096db-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="096db-325">`WithFilter` 拡張メソッドは [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet パッケージで提供されます。</span><span class="sxs-lookup"><span data-stu-id="096db-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="096db-326">このメソッドは、登録されているすべてのログ プロバイダーに渡されたログ メッセージをフィルター処理する新しい `ILoggerFactory` インスタンスを返します。</span><span class="sxs-lookup"><span data-stu-id="096db-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="096db-327">これは、元の `ILoggerFactory` インスタンスを含め、他の `ILoggerFactory` インスタンスには影響がありません。</span><span class="sxs-lookup"><span data-stu-id="096db-327">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="096db-328">ログのスコープ</span><span class="sxs-lookup"><span data-stu-id="096db-328">Log scopes</span></span>

<span data-ttu-id="096db-329">論理操作セットの一部として作成される各ログに同じデータをアタッチするために、*スコープ*内の論理操作セットをグループ化することができます。</span><span class="sxs-lookup"><span data-stu-id="096db-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="096db-330">たとえば、状況によっては、トランザクションの処理の一部として作成されるすべてのログにトランザクション ID を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="096db-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="096db-331">スコープは [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) メソッドから返される `IDisposable` の種類であり、破棄されるまで継続します。</span><span class="sxs-lookup"><span data-stu-id="096db-331">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="096db-332">スコープを使用するには、次のようにロガーの呼び出しを `using` ブロックでラップします。</span><span class="sxs-lookup"><span data-stu-id="096db-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="096db-333">次のコードでは、Console プロバイダーのスコープを有効にしています。</span><span class="sxs-lookup"><span data-stu-id="096db-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="096db-335">*Program.cs* の場合:</span><span class="sxs-lookup"><span data-stu-id="096db-335">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="096db-336">スコープベースのログ記録を有効にするには、`IncludeScopes` コンソールのロガー オプションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="096db-336">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="096db-337">*appsettings* 構成ファイルを使用する `IncludeScopes` の構成は、ASP.NET Core 2.1 のリリースで使用できます。</span><span class="sxs-lookup"><span data-stu-id="096db-337">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-338">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-338">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="096db-339">*Startup.cs* の場合:</span><span class="sxs-lookup"><span data-stu-id="096db-339">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="096db-340">各ログ メッセージには、スコープ内の情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="096db-340">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="096db-341">組み込みのログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-341">Built-in logging providers</span></span>

<span data-ttu-id="096db-342">ASP.NET Core には次のプロバイダーが付属しています。</span><span class="sxs-lookup"><span data-stu-id="096db-342">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="096db-343">コンソール</span><span class="sxs-lookup"><span data-stu-id="096db-343">Console</span></span>](#console-provider)
* [<span data-ttu-id="096db-344">デバッグ</span><span class="sxs-lookup"><span data-stu-id="096db-344">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="096db-345">EventSource</span><span class="sxs-lookup"><span data-stu-id="096db-345">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="096db-346">EventLog</span><span class="sxs-lookup"><span data-stu-id="096db-346">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="096db-347">TraceSource</span><span class="sxs-lookup"><span data-stu-id="096db-347">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="096db-348">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="096db-348">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="096db-349">Console プロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-349">Console provider</span></span>

<span data-ttu-id="096db-350">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) プロバイダー パッケージは、ログ出力をコンソールに送信します。</span><span class="sxs-lookup"><span data-stu-id="096db-350">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-351">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-351">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="096db-353">[AddConsole オーバーロード](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions)を使用すると、最小ログ レベル、フィルター関数、スコープがサポートされているかどうかを示すブール値を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="096db-353">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="096db-354">もう 1 つの選択肢として、`IConfiguration` オブジェクトを渡す方法があります。この場合、スコープのサポートとログ レベルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="096db-354">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="096db-355">運用環境で使用する Console プロバイダーを検討している場合は、パフォーマンスに重大な影響がある点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="096db-355">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="096db-356">Visual Studio で新しいプロジェクトを作成する場合、`AddConsole` メソッドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="096db-356">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="096db-357">このコードは、*appSettings.json* ファイルの `Logging` セクションを参照しています。</span><span class="sxs-lookup"><span data-stu-id="096db-357">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="096db-358">この設定ではフレームワークのログを警告に制限していますが、「[ログのフィルター処理](#log-filtering)」セクションで説明したように、アプリではデバッグ レベルでログに記録することができます。</span><span class="sxs-lookup"><span data-stu-id="096db-358">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="096db-359">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="096db-359">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

### <a name="debug-provider"></a><span data-ttu-id="096db-360">Debug プロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-360">Debug provider</span></span>

<span data-ttu-id="096db-361">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) プロバイダー パッケージは、[System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) クラス (`Debug.WriteLine` メソッドの呼び出し) を使用してログの出力を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="096db-361">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="096db-362">Linux では、このプロバイダーから */var/log/message* にログが書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="096db-362">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="096db-365">[AddDebug オーバーロード](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions)を使用すると、最小ログ レベルまたはフィルター関数を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="096db-365">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

### <a name="eventsource-provider"></a><span data-ttu-id="096db-366">EventSource プロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-366">EventSource provider</span></span>

<span data-ttu-id="096db-367">ASP.NET Core 1.1.0 以降をターゲットとするアプリの場合、[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) プロバイダー パッケージは、イベントのトレースを実装できます。</span><span class="sxs-lookup"><span data-stu-id="096db-367">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="096db-368">Windows では、[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803) を使用します。</span><span class="sxs-lookup"><span data-stu-id="096db-368">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="096db-369">プロバイダーはクロスプラットフォームですが、Linux または macOS 用のイベント コレクションはまだ存在せず、ツールは表示されません。</span><span class="sxs-lookup"><span data-stu-id="096db-369">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="096db-372">ログの収集と表示には、[PerfView utility](https://github.com/Microsoft/perfview) を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="096db-372">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="096db-373">ETW ログを表示できる他のツールはありますが、ASP.NET から出力される ETW イベントを操作する場合、PerfView は最適なエクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="096db-373">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="096db-374">このプロバイダーでログに記録されるイベントを収集するように PerfView を構成するには、**[追加プロバイダー]** の一覧に文字列 `*Microsoft-Extensions-Logging` を追加します </span><span class="sxs-lookup"><span data-stu-id="096db-374">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="096db-375">(文字列の先頭に忘れずにアスタリスクを付けてください)。</span><span class="sxs-lookup"><span data-stu-id="096db-375">(Don't miss the asterisk at the start of the string.)</span></span>

![Perfview の追加プロバイダー](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="096db-377">Windows EventLog プロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-377">Windows EventLog provider</span></span>

<span data-ttu-id="096db-378">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) プロバイダー パッケージは、ログ出力を Windows イベント ログに送信します。</span><span class="sxs-lookup"><span data-stu-id="096db-378">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-379">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-379">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-380">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-380">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="096db-381">[AddEventLog オーバーロード](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions)を使用すると、`EventLogSettings` または最小ログ レベルを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="096db-381">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

### <a name="tracesource-provider"></a><span data-ttu-id="096db-382">TraceSource プロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-382">TraceSource provider</span></span>

<span data-ttu-id="096db-383">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) プロバイダー パッケージは、[System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) のライブラリとプロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="096db-383">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-384">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-384">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-385">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-385">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="096db-386">[AddTraceSource オーバーロード](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions)を使用すると、ソース スイッチとトレース リスナーを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="096db-386">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="096db-387">このプロバイダーを使用するには、アプリケーションを (.NET Core ではなく) .NET Framework 上で実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="096db-387">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="096db-388">このプロバイダーを使用すると、サンプル アプリケーションで使用されている [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) など、多様な[リスナー](/dotnet/framework/debug-trace-profile/trace-listeners)にメッセージをルーティングすることができます。</span><span class="sxs-lookup"><span data-stu-id="096db-388">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="096db-389">次の例では、`Warning` 以上のメッセージのログをコンソール ウィンドウに出力する `TraceSource` プロバイダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="096db-389">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="096db-390">Azure App Service プロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-390">Azure App Service provider</span></span>

<span data-ttu-id="096db-391">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) プロバイダー パッケージは、Azure App Service アプリのファイル システムのテキスト ファイルと、Azure Storage アカウントの [BLOB ストレージ](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)にログを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="096db-391">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="096db-392">このプロバイダーは、ASP.NET Core 1.1 以降をターゲットとするアプリでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="096db-392">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="096db-393">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="096db-393">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="096db-394">.NET Core をターゲットとする場合は、プロバイダー パッケージをインストールしたり、[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) を明示的に呼び出したりしないでください。</span><span class="sxs-lookup"><span data-stu-id="096db-394">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="096db-395">アプリを Azure App Service にデプロイすると、プロバイダーはアプリで自動的に使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="096db-395">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="096db-396">.NET Framework ターゲットとする場合は、プロバイダー パッケージをプロジェクトに追加して、`AddAzureWebAppDiagnostics` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="096db-396">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="096db-397">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="096db-397">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="096db-398">[AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) オーバーロードを使用すると、ログ記録出力テンプレート、BLOB 名、ファイル サイズの制限など、既定の設定をオーバーライドできる [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) を渡すことができます </span><span class="sxs-lookup"><span data-stu-id="096db-398">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="096db-399">(*出力テンプレート*は、`ILogger` メソッドを呼び出すときに指定するテンプレートに加え、すべてのログに適用されるメッセージ テンプレートです)。</span><span class="sxs-lookup"><span data-stu-id="096db-399">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="096db-400">App Service アプリにデプロイすると、アプリは Azure Portal の **[App Service]** ページの [[診断ログ]](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) セクションに指定された設定を適用します。</span><span class="sxs-lookup"><span data-stu-id="096db-400">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="096db-401">これらの設定が更新されると、アプリの再起動や再デプロイを必要とせずに、変更がすぐに有効になります。</span><span class="sxs-lookup"><span data-stu-id="096db-401">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Azure のログ設定](index/_static/azure-logging-settings.png)

<span data-ttu-id="096db-403">ログ ファイルの既定の場所は、*D:\\home\\LogFiles\\Application* です。既定のファイル名は *diagnostics-yyyymmdd.txt* です。</span><span class="sxs-lookup"><span data-stu-id="096db-403">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="096db-404">既定のファイル サイズ制限は 10 MB です。保持されるファイルの既定の最大数は 2 です。</span><span class="sxs-lookup"><span data-stu-id="096db-404">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="096db-405">既定の BLOB 名は *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt* です。</span><span class="sxs-lookup"><span data-stu-id="096db-405">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="096db-406">既定の動作の詳細については、[AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="096db-406">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="096db-407">このプロバイダーは、プロジェクトが Azure 環境で実行される場合にのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="096db-407">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="096db-408">プロジェクトをローカルで実行しても、効果はありません&mdash;BLOB のローカル ファイルまたはローカル開発ストレージへの書き込みは行われません。</span><span class="sxs-lookup"><span data-stu-id="096db-408">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="096db-409">サードパーティ製のログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="096db-409">Third-party logging providers</span></span>

<span data-ttu-id="096db-410">ASP.NET Core で使用できるサードパーティ製のログ記録フレームワークをいくつか紹介します。</span><span class="sxs-lookup"><span data-stu-id="096db-410">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="096db-411">[elmah.io](https://elmah.io/) ([GitHub リポジトリ](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="096db-411">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="096db-412">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub リポジトリ](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="096db-412">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="096db-413">[JSNLog](http://jsnlog.com/) ([GitHub リポジトリ](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="096db-413">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="096db-414">[Loggr](http://loggr.net/) ([GitHub リポジトリ](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="096db-414">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="096db-415">[NLog](http://nlog-project.org/) ([GitHub リポジトリ](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="096db-415">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="096db-416">[Serilog](https://serilog.net/) ([GitHub リポジトリ](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="096db-416">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="096db-417">一部のサードパーティ製フレームワークは、[セマンティック ログ記録 (構造化ログ記録とも呼ばれます)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) を実行できます。</span><span class="sxs-lookup"><span data-stu-id="096db-417">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="096db-418">サード パーティ製フレームワークを使用することは、組み込みのプロバイダーのいずれかを使用することと似ています。</span><span class="sxs-lookup"><span data-stu-id="096db-418">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="096db-419">プロジェクトに NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="096db-419">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="096db-420">`ILoggerFactory` で拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="096db-420">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="096db-421">詳細については、各フレームワークのドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="096db-421">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="096db-422">Azure ログのストリーミング</span><span class="sxs-lookup"><span data-stu-id="096db-422">Azure log streaming</span></span>

<span data-ttu-id="096db-423">Azure ログのストリーミングを使用すると、リアル タイムで以下のログ アクティビティを確認できます。</span><span class="sxs-lookup"><span data-stu-id="096db-423">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="096db-424">アプリケーション サーバー</span><span class="sxs-lookup"><span data-stu-id="096db-424">The application server</span></span>
* <span data-ttu-id="096db-425">Web サーバー</span><span class="sxs-lookup"><span data-stu-id="096db-425">The web server</span></span>
* <span data-ttu-id="096db-426">失敗した要求のトレース</span><span class="sxs-lookup"><span data-stu-id="096db-426">Failed request tracing</span></span>

<span data-ttu-id="096db-427">Azure ログのストリーミングを構成するには</span><span class="sxs-lookup"><span data-stu-id="096db-427">To configure Azure log streaming:</span></span>

* <span data-ttu-id="096db-428">アプリケーションのポータル ページから **[診断ログ]** ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="096db-428">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="096db-429">**[アプリケーション ログ (ファイル システム)]** を [オン] に設定します。</span><span class="sxs-lookup"><span data-stu-id="096db-429">Set **Application Logging (Filesystem)** to on.</span></span>

![Azure Portal の [診断ログ] ページ](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="096db-431">**[ログ ストリーミング]** ページに移動して、アプリケーション メッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="096db-431">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="096db-432">これらのメッセージは、アプリケーションで `ILogger` インターフェイスを介してログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="096db-432">They're logged by application through the `ILogger` interface.</span></span>

![Azure Portal のアプリケーション ログのストリーミング](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="096db-434">Azure Application Insights のトレース ログ</span><span class="sxs-lookup"><span data-stu-id="096db-434">Azure Application Insights trace logging</span></span>

<span data-ttu-id="096db-435">[Application Insights](https://azure.microsoft.com/services/application-insights/) SDK では、ASP.NET Core ログ インフラストラクチャを介して生成されるログからトレース テレメトリを収集することができます。</span><span class="sxs-lookup"><span data-stu-id="096db-435">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="096db-436">詳細については、[Microsoft/ApplicationInsights-aspnetcore の Wiki のログ](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="096db-436">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="096db-437">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="096db-437">Additional resources</span></span>

[<span data-ttu-id="096db-438">LoggerMessage による高パフォーマンスのログ記録</span><span class="sxs-lookup"><span data-stu-id="096db-438">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
