---
title: "ASP.NET Core でのログ記録"
author: ardalis
description: "ASP.NET Core のログ記録フレームワークが導入されています。 いくつかの一般的なサード パーティ プロバイダーへのリンクと各組み込みのログ プロバイダーのセクションが含まれています。"
keywords: "ASP.NET Core、ログ、ログ プロバイダー、Microsoft.Extensions.Logging ILogger、iloggerfactory を提供、LogLevel、WithFilter、TraceSource、EventLog EventSource、スコープします。"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b9a4ae6e7d9b2fa998b91e643e63657239d4866b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="f52c3-105">ASP.NET Core でのログ記録の概要</span><span class="sxs-lookup"><span data-stu-id="f52c3-105">Introduction to Logging in ASP.NET Core</span></span>

<span data-ttu-id="f52c3-106">によって[Steve Smith](https://ardalis.com/)と[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f52c3-106">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f52c3-107">ASP.NET Core は、さまざまなログ プロバイダーと連携するログ記録 API をサポートします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="f52c3-108">組み込みのプロバイダーを使用する 1 つまたは複数の送信先にログを送信してとサード パーティ製のログ記録のフレームワークにプラグインできます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="f52c3-109">この記事では、コードで組み込みのログ記録 API とプロバイダーを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[<span data-ttu-id="f52c3-111">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="f52c3-111">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[<span data-ttu-id="f52c3-113">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="f52c3-113">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a><span data-ttu-id="f52c3-114">ログを作成する方法</span><span class="sxs-lookup"><span data-stu-id="f52c3-114">How to create logs</span></span>

<span data-ttu-id="f52c3-115">ログを作成するには、取得、`ILogger`オブジェクトから、[依存性の注入](dependency-injection.md)コンテナー。</span><span class="sxs-lookup"><span data-stu-id="f52c3-115">To create logs, get an `ILogger` object from the [dependency injection](dependency-injection.md) container:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="f52c3-116">その後そのロガー オブジェクト上ログ メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="f52c3-117">この例を使用してログを作成、`TodoController`クラスの*カテゴリ*です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-117">This example creates logs with the `TodoController` class as the *category*.</span></span>  <span data-ttu-id="f52c3-118">カテゴリが説明されている[この記事で後述](#log-category)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="f52c3-119">ASP.NET Core メソッドは提供しません async ロガーのログ記録は、非同期にかかるコストの価値があることができない高速に動作する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f52c3-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="f52c3-120">True でないそのような状況での場合は、ログに記録する方法の変更を検討してください。</span><span class="sxs-lookup"><span data-stu-id="f52c3-120">If you're in a situation where that's not true, consider changing the way you log.</span></span>  <span data-ttu-id="f52c3-121">データ ストアが低速の場合は、最初に、高速なストアに、ログ メッセージを記述し、後で、低速なストアに移動します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="f52c3-122">たとえば、読み取りおよび別のプロセスによって低速のストレージに保存されるメッセージ キューにログインします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="f52c3-123">プロバイダーを追加する方法</span><span class="sxs-lookup"><span data-stu-id="f52c3-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f52c3-125">ログ プロバイダーが使用して作成したメッセージを取得、`ILogger`オブジェクトで表示されるかに保存します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="f52c3-126">たとえば、コンソール プロバイダーでは、コンソールで、メッセージが表示され、Azure アプリのサービス プロバイダーが Azure blob ストレージに格納できます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="f52c3-127">プロバイダーを使用するには、プロバイダーを呼び出す`Add<ProviderName>`でも拡張メソッド*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f52c3-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="f52c3-128">既定のプロジェクト テンプレートが、前のコードに表示する方法のログ記録の設定が、`ConfigureLogging`呼び出しを行う、`CreateDefaultBuilder`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-128">The default project template sets up logging the way you see it in the preceding code, but the `ConfigureLogging` call is done by the `CreateDefaultBuilder` method.</span></span> <span data-ttu-id="f52c3-129">次のコードは*Program.cs*プロジェクト テンプレートによって作成します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-129">Here's the code in *Program.cs* that is created by project templates:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f52c3-131">ログ プロバイダーが使用して作成したメッセージを取得、`ILogger`オブジェクトで表示されるかに保存します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-131">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="f52c3-132">たとえば、コンソール プロバイダーでは、コンソールで、メッセージが表示され、Azure アプリのサービス プロバイダーが Azure blob ストレージに格納できます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-132">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="f52c3-133">プロバイダーを使用するその NuGet パッケージをインストールし、インスタンス上のプロバイダーの拡張メソッドを呼び出す`ILoggerFactory`の次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-133">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="f52c3-134">ASP.NET Core[依存性の注入](dependency-injection.md)(DI) の提供、`ILoggerFactory`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="f52c3-134">ASP.NET Core [dependency injection](dependency-injection.md) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="f52c3-135">`AddConsole`と`AddDebug`で拡張メソッドが定義されている、 [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)と[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-135">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="f52c3-136">各拡張メソッドを呼び出す、`ILoggerFactory.AddProvider`メソッド、プロバイダーのインスタンスに渡します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-136">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="f52c3-137">この記事のサンプル アプリケーションでのログ プロバイダーの追加、`Configure`のメソッド、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-137">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="f52c3-138">以前に実行されるコードからログ出力を取得する場合は、追加のログ プロバイダー、`Startup`クラス コンス トラクターを代わりにします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-138">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="f52c3-139">各に関する情報を参照する[組み込みのログ プロバイダー](#built-in-logging-providers)へのリンク[サード パーティ製のログ記録プロバイダー](#third-party-logging-providers)記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-139">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="f52c3-140">サンプルのログ出力</span><span class="sxs-lookup"><span data-stu-id="f52c3-140">Sample logging output</span></span>

<span data-ttu-id="f52c3-141">サンプル コードは、前のセクションに示すように、コマンドラインから実行すると、コンソールにログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-141">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="f52c3-142">コンソール出力の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-142">Here's an example of console output:</span></span>

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
 
<span data-ttu-id="f52c3-143">これらのログに移動して作成された`http://localhost:5000/api/todo/0`、両方の実行をトリガーする`ILogger`呼び出しの前のセクションに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-143">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="f52c3-144">Visual Studio でサンプル アプリケーションを実行すると、デバッグ ウィンドウに表示されるように、同じログの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-144">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="f52c3-145">によって作成されたログ、 `ILogger` "TodoApi.Controllers.TodoController"で、前のセクションで示した呼び出しを開始します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-145">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="f52c3-146">カテゴリの"Microsoft"で始まるログから ASP.NET Core されています。</span><span class="sxs-lookup"><span data-stu-id="f52c3-146">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="f52c3-147">ASP.NET Core 自体と、アプリケーション コードには、同じログ記録 API と同じログ プロバイダーが使用しています。</span><span class="sxs-lookup"><span data-stu-id="f52c3-147">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="f52c3-148">この記事の残りの部分は、いくつかの詳細とログ記録のオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-148">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="f52c3-149">NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="f52c3-149">NuGet packages</span></span>

<span data-ttu-id="f52c3-150">`ILogger`と`ILoggerFactory`インターフェイスが、 [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)、およびそれらの既定の実装を[Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-150">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="f52c3-151">ログのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="f52c3-151">Log category</span></span>

<span data-ttu-id="f52c3-152">A*カテゴリ*を作成する各ログに含まれています。</span><span class="sxs-lookup"><span data-stu-id="f52c3-152">A *category* is included with each log that you create.</span></span>  <span data-ttu-id="f52c3-153">作成するときに、カテゴリを指定する、`ILogger`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f52c3-153">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="f52c3-154">カテゴリできますが、任意の文字列は、規則は、ログの書き込み元となるクラスの完全修飾名を使用します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-154">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span>  <span data-ttu-id="f52c3-155">例:"TodoApi.Controllers.TodoController"です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-155">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="f52c3-156">文字列として指定したり、カテゴリの型から派生する拡張メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-156">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="f52c3-157">カテゴリを文字列として指定するには、呼び出す`CreateLogger`上、`ILoggerFactory`インスタンス、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-157">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="f52c3-158">ほとんどの場合、なりますを使いやすくする`ILogger<T>`、次の例のようにします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-158">Most of the time, it will be easier to use  `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="f52c3-159">これは、呼び出すことと同じ`CreateLogger`の完全修飾型名で`T`です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-159">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="f52c3-160">ログ レベル</span><span class="sxs-lookup"><span data-stu-id="f52c3-160">Log level</span></span>

<span data-ttu-id="f52c3-161">たびに、指定したログを記述すると、その[LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-161">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="f52c3-162">ログ レベルでは、重大度または重要性の度合いを示します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-162">The log level indicates the degree of severity or importance.</span></span>  <span data-ttu-id="f52c3-163">たとえば、記述する場合があります、`Information`メソッドは通常、終了時にログに記録、`Warning`メソッドから返された 404 リターン コード、および、ログに記録`Error`予期しない例外をキャッチするときにログに記録します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-163">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="f52c3-164">次のコード例で、メソッドの名前 (たとえば、 `LogWarning`) ログ レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-164">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span>  <span data-ttu-id="f52c3-165">最初のパラメーターは、[ログ イベント ID](#log-event-id) (この記事の後半で説明します)。</span><span class="sxs-lookup"><span data-stu-id="f52c3-165">The first parameter is the [Log event ID](#log-event-id) (explained later in this article).</span></span>  <span data-ttu-id="f52c3-166">残りのパラメーターは、ログのメッセージ文字列を構築します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-166">The remaining parameters construct a log message string.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="f52c3-167">メソッド名に、レベルが含まれるログのメソッドは[ILogger の拡張メソッドを](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="f52c3-168">背後では、これらのメソッドを呼び出す、`Log`を受け取るメソッド、`LogLevel`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="f52c3-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="f52c3-169">呼び出すことができます、`Log`これらの拡張メソッドが、構文のいずれかではなく、メソッドを直接は比較的複雑になります。</span><span class="sxs-lookup"><span data-stu-id="f52c3-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="f52c3-170">詳細については、次を参照してください。、 [ILogger インターフェイス](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger)と[ロガーの拡張機能のソース コード](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="f52c3-171">ASP.NET Core は、次を定義[ログ レベル](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel)、ここに最高の重大度が最もから注文します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="f52c3-172">トレース = 0</span><span class="sxs-lookup"><span data-stu-id="f52c3-172">Trace = 0</span></span>

  <span data-ttu-id="f52c3-173">開発者にのみ重要な情報の問題をデバッグします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="f52c3-174">これらのメッセージは機密性の高いアプリケーションのデータを含む可能性およびようにする必要がありますいないで有効にする、実稼働環境。</span><span class="sxs-lookup"><span data-stu-id="f52c3-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="f52c3-175">*既定では無効です。*</span><span class="sxs-lookup"><span data-stu-id="f52c3-175">*Disabled by default.*</span></span> <span data-ttu-id="f52c3-176">例 : `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="f52c3-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="f52c3-177">デバッグ = 1</span><span class="sxs-lookup"><span data-stu-id="f52c3-177">Debug = 1</span></span>

  <span data-ttu-id="f52c3-178">については、開発およびデバッグ中に短期的な実用性を持ちます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="f52c3-179">例:`Entering method Configure with flag set to true.`通常は有効にしない`Debug`大量のログのため、トラブルシューティングする場合を除き、実稼働環境でレベルをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="f52c3-180">情報 = 2</span><span class="sxs-lookup"><span data-stu-id="f52c3-180">Information = 2</span></span>

  <span data-ttu-id="f52c3-181">アプリケーションの一般的なフローを追跡します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="f52c3-182">これらのログは、通常、いくつかの長期的な値を持ちます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="f52c3-183">例 : `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="f52c3-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="f52c3-184">警告 3 を =</span><span class="sxs-lookup"><span data-stu-id="f52c3-184">Warning = 3</span></span>

  <span data-ttu-id="f52c3-185">アプリケーションのフローで異常なまたは予期しないイベントのします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="f52c3-186">エラーまたは停止するには、アプリケーションが発生しないことが調査する必要がありますが、その他の条件が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="f52c3-187">処理済みの例外は、共通の場所を使用する、`Warning`ログ レベルです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="f52c3-188">例 : `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="f52c3-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="f52c3-189">エラー = 4</span><span class="sxs-lookup"><span data-stu-id="f52c3-189">Error = 4</span></span>

  <span data-ttu-id="f52c3-190">エラーと例外を処理できません。</span><span class="sxs-lookup"><span data-stu-id="f52c3-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="f52c3-191">これらのメッセージは、現在のアクティビティまたは (現在の HTTP 要求) などの操作のエラー、アプリケーション全体にわたる障害ではありませんを示します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="f52c3-192">ログのメッセージの例:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="f52c3-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="f52c3-193">重要な 5 を =</span><span class="sxs-lookup"><span data-stu-id="f52c3-193">Critical = 5</span></span>

  <span data-ttu-id="f52c3-194">エラーは、早急に措置を必要とします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-194">For failures that require immediate attention.</span></span> <span data-ttu-id="f52c3-195">例: データ損失シナリオ、ディスク領域が不足しています。</span><span class="sxs-lookup"><span data-stu-id="f52c3-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="f52c3-196">ログ レベルを使用して、ログ出力の量が特定の記憶域メディアに書き込まれる制御またはウィンドウを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="f52c3-197">たとえば、実稼働環境でことができますのすべてのログ`Information`レベルとボリュームのデータ ストアとのすべてのログに移動して`Warning`レベルと値のデータ ストアに移動するより高い。</span><span class="sxs-lookup"><span data-stu-id="f52c3-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="f52c3-198">開発中はのログを送信することが通常`Warning`またはコンソールに重大度が高いです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="f52c3-199">追加することができますをトラブルシューティングする必要がある場合、`Debug`レベル。</span><span class="sxs-lookup"><span data-stu-id="f52c3-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="f52c3-200">[ログ フィルター](#log-filtering)この記事の後半で、プロバイダーを処理するログ レベルを制御する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="f52c3-201">ASP.NET Core framework 書き込みます`Debug`framework のイベント ログのレベルです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="f52c3-202">ログの例は、前の「除外以下のログ`Information`レベル、ため`Debug`レベル ログが表示されていました。</span><span class="sxs-lookup"><span data-stu-id="f52c3-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="f52c3-203">サンプル アプリケーションを表示するように構成を実行する場合に、コンソール ログの例を次に示します`Debug`とコンソールのプロバイダーの高いログ。</span><span class="sxs-lookup"><span data-stu-id="f52c3-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="f52c3-204">ログのイベント ID</span><span class="sxs-lookup"><span data-stu-id="f52c3-204">Log event ID</span></span>

<span data-ttu-id="f52c3-205">たびにログを記述することができますを指定する、*イベント ID*です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="f52c3-206">サンプル アプリはローカルに定義を使用することで`LoggingEvents`クラス。</span><span class="sxs-lookup"><span data-stu-id="f52c3-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="f52c3-207">イベント ID は、ログに記録されたイベントのセットを関連付ける互いに使用できる整数値です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="f52c3-208">たとえば、ショッピング カートにアイテムを追加するためのログはイベント ID 1000 と購入を完了するためのログはイベント ID 1001 である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f52c3-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="f52c3-209">ログ出力でイベント ID フィールドに格納されているまたはプロバイダーによって、テキスト メッセージに含まれます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span>  <span data-ttu-id="f52c3-210">デバッグ プロバイダーは、イベント Id を表示しませんが、コンソールのプロバイダーを示す角かっこでカテゴリ後。</span><span class="sxs-lookup"><span data-stu-id="f52c3-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a><span data-ttu-id="f52c3-211">ログ メッセージの書式指定文字列</span><span class="sxs-lookup"><span data-stu-id="f52c3-211">Log message format string</span></span>

<span data-ttu-id="f52c3-212">テキスト メッセージを提供するたびに、ログを記述します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-212">Each time you write a log, you provide a text message.</span></span> <span data-ttu-id="f52c3-213">メッセージ文字列は、どちらの引数に値を配置している次の例のように、名前付きプレース ホルダーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-213">The message string can contain named placeholders into which argument values are placed, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="f52c3-214">その名前のプレース ホルダーの順序は、どのパラメーターはそれらの使用を決定します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-214">The order of placeholders, not their names, determines which parameters are used for them.</span></span> <span data-ttu-id="f52c3-215">たとえば、次のようなコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-215">For example, if you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="f52c3-216">結果のログ メッセージは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f52c3-216">The resulting log message would look like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="f52c3-217">ログ フレームワークはメッセージのログ プロバイダーを実装するを可能にするには、この方法で書式設定[セマンティック ログ記録、構造化されたログ記録とも呼ばれる](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="f52c3-218">引数そのものが形式のメッセージ文字列だけでなく、ログ システムに渡されるために、ログ プロバイダーは、メッセージ文字列だけでなくフィールドとしてパラメーターの値を格納できます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-218">Because the arguments themselves are passed to the logging system, not just the formatted message string, logging providers can store the parameter values as fields in addition to the message string.</span></span> <span data-ttu-id="f52c3-219">たとえば、誘導している場合、ログが Azure テーブル ストレージに出力し、ロガー メソッドの呼び出しが次のよう。</span><span class="sxs-lookup"><span data-stu-id="f52c3-219">For example, if you are directing your log output to Azure Table Storage, and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="f52c3-220">各 Azure テーブル エンティティがある可能性があります`ID`と`RequestTime`プロパティで、ログ データにクエリが簡素化します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-220">Each Azure Table entity could have `ID` and `RequestTime` properties, which would simplify queries on log data.</span></span> <span data-ttu-id="f52c3-221">特定の内のすべてのログを検索できます`RequestTime`範囲、テキスト メッセージのタイムアウトを解析する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f52c3-221">You could find all logs within a particular `RequestTime` range, without having to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="f52c3-222">ログの例外</span><span class="sxs-lookup"><span data-stu-id="f52c3-222">Logging exceptions</span></span>

<span data-ttu-id="f52c3-223">ロガー メソッドでは、次の例のように、例外を渡すことのできるオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="f52c3-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="f52c3-224">異なるプロバイダーでは、さまざまな方法で、例外情報を処理します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="f52c3-225">上記のコードからのデバッグ、プロバイダーの出力の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="f52c3-226">ログのフィルター選択</span><span class="sxs-lookup"><span data-stu-id="f52c3-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f52c3-228">特定のプロバイダーとカテゴリまたはすべてのプロバイダーまたはすべてのカテゴリでは、最小のログ レベルを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span>  <span data-ttu-id="f52c3-229">最小レベルの下の任意のログはいない表示または保存されている取得しないように、そのプロバイダーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="f52c3-230">すべてのログを抑制するかどうかを指定できます`LogLevel.None`最小のログ レベルとします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="f52c3-231">整数値`LogLevel.None`6、これはより高い`LogLevel.Critical`(5)。</span><span class="sxs-lookup"><span data-stu-id="f52c3-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="f52c3-232">**構成でのフィルタの規則を作成します。**</span><span class="sxs-lookup"><span data-stu-id="f52c3-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="f52c3-233">呼び出すコードを作成するプロジェクト テンプレート`CreateDefaultBuilder`コンソールとデバッグのプロバイダーのログ記録を設定します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="f52c3-234">`CreateDefaultBuilder`メソッドもログ記録を構成で検索する設定、`Logging`セクションで、次のようにコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="f52c3-235">構成データは、プロバイダーと次の例のように、カテゴリによって最小のログ レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](logging/sample2/appsettings.json)]

<span data-ttu-id="f52c3-236">この JSON では、デバッグ プロバイダーの 1 つ、4 つのコンソール プロバイダーとすべてのプロバイダーに適用される 1 つ、6 つのフィルター規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="f52c3-237">これらの規則を 1 つだけかは、後ほどが各プロバイダーの選択が表示されるときに、`ILogger`オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="f52c3-238">**コードでのフィルタの規則**</span><span class="sxs-lookup"><span data-stu-id="f52c3-238">**Filter rules in code**</span></span>

<span data-ttu-id="f52c3-239">次の例で示すように、コードでは、フィルタの規則を登録できます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="f52c3-240">2 番目`AddFilter`型の名前を使用して、デバッグ プロバイダーを指定します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="f52c3-241">最初の`AddFilter`プロバイダーの種類を指定されていないために、すべてのプロバイダーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="f52c3-242">**フィルターの規則が適用されます。**</span><span class="sxs-lookup"><span data-stu-id="f52c3-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="f52c3-243">構成データと`AddFilter`前の例に示すコードは、次の表に示すようにルールを作成します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="f52c3-244">最初の 6 個が構成の例から取り出され、最後の 2 つのコード例に由来します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-244">The first six come from the configuration example and the last two come from the code example.</span></span>

<span data-ttu-id="f52c3-245">数値</span><span class="sxs-lookup"><span data-stu-id="f52c3-245">Number</span></span>|<span data-ttu-id="f52c3-246">プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-246">Provider</span></span>|<span data-ttu-id="f52c3-247">始まるカテゴリ</span><span class="sxs-lookup"><span data-stu-id="f52c3-247">Categories that begin with</span></span>|<span data-ttu-id="f52c3-248">最小のログ レベル</span><span class="sxs-lookup"><span data-stu-id="f52c3-248">Minimum log level</span></span>|
------|--------|--------------------------|-----------------|
<span data-ttu-id="f52c3-249">1</span><span class="sxs-lookup"><span data-stu-id="f52c3-249">1</span></span>|<span data-ttu-id="f52c3-250">デバッグ</span><span class="sxs-lookup"><span data-stu-id="f52c3-250">Debug</span></span>|<span data-ttu-id="f52c3-251">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="f52c3-251">All categories</span></span>|<span data-ttu-id="f52c3-252">情報</span><span class="sxs-lookup"><span data-stu-id="f52c3-252">Information</span></span>|
<span data-ttu-id="f52c3-253">2</span><span class="sxs-lookup"><span data-stu-id="f52c3-253">2</span></span>|<span data-ttu-id="f52c3-254">コンソール</span><span class="sxs-lookup"><span data-stu-id="f52c3-254">Console</span></span>|<span data-ttu-id="f52c3-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="f52c3-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span>|<span data-ttu-id="f52c3-256">警告</span><span class="sxs-lookup"><span data-stu-id="f52c3-256">Warning</span></span>|
<span data-ttu-id="f52c3-257">3</span><span class="sxs-lookup"><span data-stu-id="f52c3-257">3</span></span>|<span data-ttu-id="f52c3-258">コンソール</span><span class="sxs-lookup"><span data-stu-id="f52c3-258">Console</span></span>|<span data-ttu-id="f52c3-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="f52c3-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>|<span data-ttu-id="f52c3-260">デバッグ</span><span class="sxs-lookup"><span data-stu-id="f52c3-260">Debug</span></span>|
<span data-ttu-id="f52c3-261">4</span><span class="sxs-lookup"><span data-stu-id="f52c3-261">4</span></span>|<span data-ttu-id="f52c3-262">コンソール</span><span class="sxs-lookup"><span data-stu-id="f52c3-262">Console</span></span>|<span data-ttu-id="f52c3-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="f52c3-263">Microsoft.AspNetCore.Mvc.Razor</span></span>|<span data-ttu-id="f52c3-264">エラー</span><span class="sxs-lookup"><span data-stu-id="f52c3-264">Error</span></span>|
<span data-ttu-id="f52c3-265">5</span><span class="sxs-lookup"><span data-stu-id="f52c3-265">5</span></span>|<span data-ttu-id="f52c3-266">コンソール</span><span class="sxs-lookup"><span data-stu-id="f52c3-266">Console</span></span>|<span data-ttu-id="f52c3-267">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="f52c3-267">All categories</span></span>|<span data-ttu-id="f52c3-268">情報</span><span class="sxs-lookup"><span data-stu-id="f52c3-268">Information</span></span>|
<span data-ttu-id="f52c3-269">6</span><span class="sxs-lookup"><span data-stu-id="f52c3-269">6</span></span>|<span data-ttu-id="f52c3-270">すべてのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-270">All providers</span></span>|<span data-ttu-id="f52c3-271">すべてのカテゴリ</span><span class="sxs-lookup"><span data-stu-id="f52c3-271">All categories</span></span>|<span data-ttu-id="f52c3-272">デバッグ</span><span class="sxs-lookup"><span data-stu-id="f52c3-272">Debug</span></span>
<span data-ttu-id="f52c3-273">7</span><span class="sxs-lookup"><span data-stu-id="f52c3-273">7</span></span>|<span data-ttu-id="f52c3-274">すべてのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-274">All providers</span></span>|<span data-ttu-id="f52c3-275">システム</span><span class="sxs-lookup"><span data-stu-id="f52c3-275">System</span></span>|<span data-ttu-id="f52c3-276">デバッグ</span><span class="sxs-lookup"><span data-stu-id="f52c3-276">Debug</span></span>
<span data-ttu-id="f52c3-277">8</span><span class="sxs-lookup"><span data-stu-id="f52c3-277">8</span></span>|<span data-ttu-id="f52c3-278">デバッグ</span><span class="sxs-lookup"><span data-stu-id="f52c3-278">Debug</span></span>|<span data-ttu-id="f52c3-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="f52c3-279">Microsoft</span></span>|<span data-ttu-id="f52c3-280">トレース</span><span class="sxs-lookup"><span data-stu-id="f52c3-280">Trace</span></span>

<span data-ttu-id="f52c3-281">作成するときに、`ILogger`を使用してログに書き込むオブジェクト、`ILoggerFactory`オブジェクトはそのロガーに適用するプロバイダーごとに 1 つの規則を選択します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="f52c3-282">によって書き込まれたすべてのメッセージ`ILogger`オブジェクトは、選択したルールに基づいてフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="f52c3-283">プロバイダーとカテゴリのペアごとに可能な最も固有のルールは、利用可能な規則から選択されます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="f52c3-284">プロバイダーごとに、次のアルゴリズムが使用されるときに、`ILogger`が 1 つのカテゴリの作成します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="f52c3-285">プロバイダーまたはそのエイリアスと一致するすべてのルールを選択します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-285">Select all rules that match the provider or its alias.</span></span>  <span data-ttu-id="f52c3-286">、何も見つからない場合は、空のプロバイダーとすべてのルールを選択します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="f52c3-287">前の手順の結果からは、時間が最長を含む選択ルールは、カテゴリ プレフィックスを照合します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="f52c3-288">何も見つからない場合は、カテゴリを指定しないすべてのルールを選択します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="f52c3-289">複数の規則が選択されている場合、**最後**いずれか。</span><span class="sxs-lookup"><span data-stu-id="f52c3-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="f52c3-290">ルールが選択されていない場合に使用して`MinimumLevel`です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="f52c3-291">たとえば、上記の規則のリストがあり、作成する、`ILogger`カテゴリ"Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine"のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f52c3-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="f52c3-292">デバッグ プロバイダーの場合は、1、6、および 8 規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="f52c3-293">ルール 8 は、選択したものになるように最も固有です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="f52c3-294">コンソール プロバイダーの場合は、3、4、5、6 および規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="f52c3-295">規則 3 は、最も固有です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="f52c3-296">使用してログを作成する場合、 `ILogger` "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine"のカテゴリのログ`Trace`レベルし、上記のログとデバッグ プロバイダーに移動する`Debug`レベルし、以降は、コンソールのプロバイダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="f52c3-297">**プロバイダーのエイリアス**</span><span class="sxs-lookup"><span data-stu-id="f52c3-297">**Provider aliases**</span></span>

<span data-ttu-id="f52c3-298">構成では、プロバイダーを指定して、型名を使用することができますが、各プロバイダー定義、短い*エイリアス*を使いやすくすることができます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="f52c3-299">組み込みのプロバイダーでは、次のエイリアスを使用します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="f52c3-300">コンソール</span><span class="sxs-lookup"><span data-stu-id="f52c3-300">Console</span></span>
- <span data-ttu-id="f52c3-301">デバッグ</span><span class="sxs-lookup"><span data-stu-id="f52c3-301">Debug</span></span>
- <span data-ttu-id="f52c3-302">イベント ログ</span><span class="sxs-lookup"><span data-stu-id="f52c3-302">EventLog</span></span>
- <span data-ttu-id="f52c3-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="f52c3-303">AzureAppServices</span></span>
- <span data-ttu-id="f52c3-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="f52c3-304">TraceSource</span></span>
- <span data-ttu-id="f52c3-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="f52c3-305">EventSource</span></span>

<span data-ttu-id="f52c3-306">**既定の最小レベル**</span><span class="sxs-lookup"><span data-stu-id="f52c3-306">**Default minimum level**</span></span>

<span data-ttu-id="f52c3-307">指定されたプロバイダーおよびカテゴリの構成またはコードからのルールが適用されない場合にのみ有効になりますの最小レベルの設定があります。</span><span class="sxs-lookup"><span data-stu-id="f52c3-307">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="f52c3-308">次の例では、最低レベルを設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="f52c3-309">最小レベルを明示的に設定しない場合、既定値は`Information`、つまり`Trace`と`Debug`ログは無視されます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-309">IF you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="f52c3-310">**フィルター関数**</span><span class="sxs-lookup"><span data-stu-id="f52c3-310">**Filter functions**</span></span>

<span data-ttu-id="f52c3-311">フィルタ リング規則を適用するフィルター関数では、コードを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="f52c3-312">フィルター関数は、すべてのプロバイダーおよび構成またはコードによって、それらに割り当てられている規則がないカテゴリに対して呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-312">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="f52c3-313">関数のコードでは、メッセージをログに記録するかどうかを決定するには、プロバイダーの種類、カテゴリ、およびログ レベルへのアクセスを持っています。</span><span class="sxs-lookup"><span data-stu-id="f52c3-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="f52c3-314">例:</span><span class="sxs-lookup"><span data-stu-id="f52c3-314">For example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f52c3-316">一部のログ プロバイダーを指定できますログをストレージ メディアに書き込まれるまたは無視する場合のログ レベルとカテゴリに基づいています。</span><span class="sxs-lookup"><span data-stu-id="f52c3-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="f52c3-317">`AddConsole`と`AddDebug`拡張メソッドは、フィルター条件に渡すことが許可されるオーバー ロードを提供します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="f52c3-318">次のサンプル コードの原因を以下のログを無視するコンソール プロバイダー`Warning`レベル中、デバッグのプロバイダー フレームワークによって作成されるログは無視されます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="f52c3-319">`AddEventLog`メソッドを受け取るオーバー ロードには、`EventLogSettings`でフィルター処理関数を含むことができるインスタンスは、その`Filter`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="f52c3-320">ログ記録レベルとその他のパラメーターに基づいているため TraceSource プロバイダーがこれらのオーバー ロードのいずれかを指定できません、`SourceSwitch`と`TraceListener`を使用します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-320">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the  `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="f52c3-321">すべてのプロバイダーに登録されているフィルタ リング規則を設定することができます、`ILoggerFactory`インスタンスを使用して、`WithFilter`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="f52c3-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="f52c3-322">次の例は、デバッグ レベルでのアプリケーション ログをながら framework ログ (カテゴリは、"Microsoft"または"System"で始まる) の警告を制限します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="f52c3-323">フィルター処理をすべてのログが特定のカテゴリ用に記述されていることを防ぐために使用するかどうかを指定できます`LogLevel.None`そのカテゴリの最小のログ レベルとします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="f52c3-324">整数値`LogLevel.None`6、これはより高い`LogLevel.Critical`(5)。</span><span class="sxs-lookup"><span data-stu-id="f52c3-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="f52c3-325">`WithFilter`によって拡張メソッドが提供される、 [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="f52c3-326">メソッドは、新しい返します`ILoggerFactory`ことに登録されているすべてのログ プロバイダーに渡されたログ メッセージがフィルター処理するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="f52c3-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="f52c3-327">影響しません、他の`ILoggerFactory`元を含むインスタンス`ILoggerFactory`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="f52c3-327">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="f52c3-328">ログのスコープ</span><span class="sxs-lookup"><span data-stu-id="f52c3-328">Log scopes</span></span>

<span data-ttu-id="f52c3-329">内の論理操作のセットをグループ化することができます、*スコープ*そのセットの一部として作成される各ログに、同じデータをアタッチするためにします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span>  <span data-ttu-id="f52c3-330">たとえば、トランザクション ID は、トランザクションの処理の一部として作成されたすべてのログをする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f52c3-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="f52c3-331">スコープは、`IDisposable`によって返される型、`ILogger.BeginScope<TState>`メソッドおよびが破棄されるまで継続します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-331">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="f52c3-332">スコープを使用して、ロガーの呼び出しでラップして、`using`ブロック、次のように。</span><span class="sxs-lookup"><span data-stu-id="f52c3-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="f52c3-333">次のコードは、コンソールのプロバイダーのスコープを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f52c3-335">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f52c3-335">In *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-336">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-336">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f52c3-337">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f52c3-337">In *Startup.cs*:</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="f52c3-338">各ログ メッセージには、スコープ対象の情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f52c3-338">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="f52c3-339">組み込みのログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-339">Built-in logging providers</span></span>

<span data-ttu-id="f52c3-340">ASP.NET Core には、次のプロバイダーが付属します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-340">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="f52c3-341">コンソール</span><span class="sxs-lookup"><span data-stu-id="f52c3-341">Console</span></span>](#console)
* [<span data-ttu-id="f52c3-342">デバッグ</span><span class="sxs-lookup"><span data-stu-id="f52c3-342">Debug</span></span>](#debug)
* [<span data-ttu-id="f52c3-343">EventSource</span><span class="sxs-lookup"><span data-stu-id="f52c3-343">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="f52c3-344">EventLog</span><span class="sxs-lookup"><span data-stu-id="f52c3-344">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="f52c3-345">TraceSource</span><span class="sxs-lookup"><span data-stu-id="f52c3-345">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="f52c3-346">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="f52c3-346">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="f52c3-347">コンソール プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-347">The console provider</span></span>

<span data-ttu-id="f52c3-348">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console)プロバイダーのパッケージは、コンソールにログ出力を送信します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-348">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-350">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="f52c3-351">[AddConsole オーバー ロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions)を渡すことができます、最小のログ レベル、フィルター関数の場合、およびスコープがサポートされているかどうかを示すブール値。</span><span class="sxs-lookup"><span data-stu-id="f52c3-351">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span>  <span data-ttu-id="f52c3-352">渡すこともできます、`IConfiguration`オブジェクトで、スコープのサポートとログ記録レベルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-352">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="f52c3-353">実稼働環境で使用するためのコンソール プロバイダーを検討している場合のパフォーマンスに大きな影響があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f52c3-353">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="f52c3-354">Visual Studio で、新しいプロジェクトを作成するときに、`AddConsole`メソッドは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f52c3-354">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="f52c3-355">このコードを参照して、`Logging`のセクションで、*される appSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f52c3-355">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](logging/sample//appsettings.json)]

<span data-ttu-id="f52c3-356">説明するようデバッグ レベルでログに記録する一方で、アプリ制限 framework ログ警告が表示設定、[ログ フィルター](#log-filtering)セクションです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-356">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="f52c3-357">詳細については、[構成](configuration.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f52c3-357">For more information, see [Configuration](configuration.md).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="f52c3-358">デバッグ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-358">The Debug provider</span></span>

<span data-ttu-id="f52c3-359">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug)プロバイダーのパッケージを使用してログの出力を書き込みます、 [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug)クラス (`Debug.WriteLine`メソッドの呼び出し)。</span><span class="sxs-lookup"><span data-stu-id="f52c3-359">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="f52c3-360">Linux では、このプロバイダーはログを書き込む*/var/log/message*です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-360">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="f52c3-363">[AddDebug オーバー ロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions)最小のログ レベル、またはフィルター関数に渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-363">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="f52c3-364">EventSource プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-364">The EventSource provider</span></span>

<span data-ttu-id="f52c3-365">ASP.NET Core 1.1.0 を対象とするアプリの高い、または、 [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource)プロバイダーのパッケージは、イベントのトレースを実装できます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-365">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="f52c3-366">Windows では、使用して[ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-366">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="f52c3-367">プロバイダーは、クロスプラット フォームが存在しないイベント for Linux または macOS まだ収集および表示するツールです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-367">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-368">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-368">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-369">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-369">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="f52c3-370">収集し、ログを表示することをお勧めは使用する、 [PerfView ユーティリティ](https://www.microsoft.com/download/details.aspx?id=28567)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-370">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="f52c3-371">ETW ログを表示するには、その他のツールがありますが、PerfView は、ASP.NET によって出力される ETW イベントを操作するための最適なエクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-371">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="f52c3-372">このプロバイダーによって記録されたイベントを収集するため PerfView を構成するには、文字列を追加`*Microsoft-Extensions-Logging`を**追加のプロバイダー**  ボックスの一覧です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-372">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="f52c3-373">(見逃さない文字列の先頭にアスタリスク。)</span><span class="sxs-lookup"><span data-stu-id="f52c3-373">(Don't miss the asterisk at the start of the string.)</span></span>

![Perfview プロバイダーの追加](logging/_static/perfview-additional-providers.png)

<span data-ttu-id="f52c3-375">Nano Server でのイベントのキャプチャには、いくつか追加の設定が必要です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-375">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="f52c3-376">PowerShell リモート処理を Nano Server に接続します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-376">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="f52c3-377">ETW セッションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-377">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="f52c3-378">ETW プロバイダーを追加[CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers)、ASP.NET Core および必要に応じて他のユーザーです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-378">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="f52c3-379">GUID は、ASP.NET Core プロバイダー`3ac73b97-af73-50e9-0822-5da4367920d0`です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-379">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="f52c3-380">サイトを実行しのトレース情報が必要な任意のアクションを実行します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-380">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="f52c3-381">完了したらトレース セッションを停止します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-381">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="f52c3-382">その結果、 *C:\trace.etl*ファイルは、Windows の他のエディションで、PerfView で分析できます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-382">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="f52c3-383">Windows イベント ログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-383">The Windows EventLog provider</span></span>

<span data-ttu-id="f52c3-384">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog)プロバイダーのパッケージは、Windows イベント ログにログ出力を送信します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-384">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-385">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-385">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-386">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-386">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="f52c3-387">[AddEventLog オーバー ロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions)を渡すように`EventLogSettings`または最小のログ レベルです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-387">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="f52c3-388">TraceSource プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-388">The TraceSource provider</span></span>

<span data-ttu-id="f52c3-389">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource)プロバイダーのパッケージを使用して、 [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource)ライブラリとプロバイダー。</span><span class="sxs-lookup"><span data-stu-id="f52c3-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-390">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-390">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-391">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-391">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="f52c3-392">[AddTraceSource オーバー ロード](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions)ソース スイッチと、トレース リスナーを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-392">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="f52c3-393">このプロバイダーを使用するのには、アプリケーションは、.NET Framework (ではなく .NET Core) で実行するがします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-393">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="f52c3-394">このプロバイダーでのさまざまなにメッセージをルーティングする[リスナー](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners)など、 [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr)サンプル アプリケーションで使用します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-394">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="f52c3-395">次の例では、構成、`TraceSource`ログに記録するプロバイダー`Warning`とコンソール ウィンドウに高いメッセージです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-395">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="f52c3-396">Azure App Service provider</span><span class="sxs-lookup"><span data-stu-id="f52c3-396">The Azure App Service provider</span></span>

<span data-ttu-id="f52c3-397">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices)プロバイダーのパッケージは、Azure App Service アプリのファイル システム内とテキスト ファイルにログを書き込みます[blob ストレージ](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage)Azure ストレージ アカウントにします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-397">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="f52c3-398">プロバイダーは、ASP.NET Core 1.1.0 を対象とするアプリに対してのみ使用可能な以上です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-398">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f52c3-399">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-399">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

> [!NOTE]
> <span data-ttu-id="f52c3-400">ASP.NET Core 2.0 はプレビュー段階です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-400">ASP.NET Core 2.0 is in preview.</span></span>  <span data-ttu-id="f52c3-401">Azure App Service に配置されたときに、最新のプレビュー リリースで作成されたアプリは実行されない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f52c3-401">Apps created with the latest preview release may not run when deployed to Azure App Service.</span></span> <span data-ttu-id="f52c3-402">Azure App Service が 2.0 を実行する ASP.NET Core 2.0 が解放されると、アプリ、および Azure App Service をここに記載されているプロバイダーは機能します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-402">When ASP.NET Core 2.0 is released, Azure App Service will run 2.0 apps, and the Azure App Service provider will work as indicated here.</span></span>

<span data-ttu-id="f52c3-403">プロバイダーのパッケージまたは呼び出しをインストールする必要はありません、`AddAzureWebAppDiagnostics`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="f52c3-403">You don't have to install the provider package or call the `AddAzureWebAppDiagnostics` extension method.</span></span>  <span data-ttu-id="f52c3-404">プロバイダーは、Azure App Service にアプリを展開するとき、アプリを自動的に利用可能です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f52c3-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f52c3-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="f52c3-406">`AddAzureWebAppDiagnostics`を渡すことができます、オーバー ロードの[AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)、使用するには、ログ出力のテンプレート、blob の名前、およびファイル サイズの制限などの既定の設定をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="f52c3-407">(*出力テンプレート*メッセージの書式指定文字列を呼び出すときに提供する 1 つ上のすべてのログに適用されるは、`ILogger`メソッドです)。</span><span class="sxs-lookup"><span data-stu-id="f52c3-407">(*Output template* is a message format string that is applied to all logs, on top of the one that you provide when you call an `ILogger` method.)</span></span>  

---

<span data-ttu-id="f52c3-408">アプリケーションでの設定は、App Service アプリを展開するときに、[診断ログ](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag)のセクションで、 **App Service** Azure ポータルのページです。</span><span class="sxs-lookup"><span data-stu-id="f52c3-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="f52c3-409">これらの設定を変更するときに変更を有効すぐにアプリを再起動するか、コードを再展開を必要とせずします。</span><span class="sxs-lookup"><span data-stu-id="f52c3-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Azure のログ記録の設定](logging/_static/azure-logging-settings.png)

<span data-ttu-id="f52c3-411">ログ ファイルの既定の場所は、 *d:\\ホーム\\LogFiles\\アプリケーション*フォルダー、および既定のファイル名は*診断 yyyymmdd.txt*です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="f52c3-412">既定のファイル サイズの制限は 10 MB であり、保持ファイルの既定の最大数は 2 です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="f52c3-413">既定の blob 名は*{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="f52c3-414">既定の動作の詳細については、次を参照してください。 [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="f52c3-415">プロバイダーは、プロジェクトは、Azure 環境で実行するときにのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-415">The provider only works when your project runs in the Azure environment.</span></span>  <span data-ttu-id="f52c3-416">影響を与えませんローカルで実行するときに&mdash;ローカル ファイルまたは blob のローカル開発ストレージに書き込まれません。</span><span class="sxs-lookup"><span data-stu-id="f52c3-416">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="f52c3-417">サード パーティ製のログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-417">Third-party logging providers</span></span>

<span data-ttu-id="f52c3-418">ASP.NET Core で動作する一部のサード パーティ製のログ記録フレームワークを次に示します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="f52c3-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -Elmah.Io サービス プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="f52c3-420">[JSNLog](http://jsnlog.com) -、サーバー側のログでの JavaScript 例外およびその他のクライアント側のイベント ログに記録します。</span><span class="sxs-lookup"><span data-stu-id="f52c3-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="f52c3-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -Loggr サービス プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="f52c3-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) -NLog ライブラリのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="f52c3-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) -Serilog ライブラリのプロバイダー</span><span class="sxs-lookup"><span data-stu-id="f52c3-423">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="f52c3-424">一部のサード パーティ製のフレームワークが実行できる[セマンティック ログ記録、構造化されたログ記録とも呼ばれる](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-424">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="f52c3-425">組み込みのプロバイダーのいずれかを使用してに似ていますが、サード パーティ製のフレームワークを使用します。 NuGet パッケージをプロジェクトに追加し、上の拡張メソッドを呼び出す`ILoggerFactory`です。</span><span class="sxs-lookup"><span data-stu-id="f52c3-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="f52c3-426">詳細については、各 framework のドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f52c3-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="f52c3-427">独自のカスタム プロバイダーを同様は、他のログ記録のフレームワーク、または独自のログ記録の要件をサポートするために作成できます。</span><span class="sxs-lookup"><span data-stu-id="f52c3-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>
