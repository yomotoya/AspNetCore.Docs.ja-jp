---
title: "ASP.NET Core でのホスティング |Microsoft ドキュメント"
author: ardalis
description: "ASP.NET Core web ホストの概要です。"
keywords: "ASP.NET Core、web ホスト、IWebHost"
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a><span data-ttu-id="bc07e-104">ASP.NET Core でのホスティングの概要</span><span class="sxs-lookup"><span data-stu-id="bc07e-104">Introduction to hosting in ASP.NET Core</span></span>

<span data-ttu-id="bc07e-105">によって[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="bc07e-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="bc07e-106">ASP.NET Core アプリケーションを実行するには、構成および使用してホストを起動する必要があります`WebHostBuilder`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-106">To run an ASP.NET Core app, you need to configure and launch a host using `WebHostBuilder`.</span></span>

## <a name="what-is-a-host"></a><span data-ttu-id="bc07e-107">ホストとは何ですか。</span><span class="sxs-lookup"><span data-stu-id="bc07e-107">What is a Host?</span></span>

<span data-ttu-id="bc07e-108">ASP.NET Core アプリを必要とする*ホスト*を実行するためにします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-108">ASP.NET Core apps require a *host* in which to execute.</span></span> <span data-ttu-id="bc07e-109">ホストを実装する必要があります、`IWebHost`機能とサービスのコレクションを公開するインターフェイスと`Start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bc07e-109">A host must implement the `IWebHost` interface, which exposes collections of features and services, and a `Start` method.</span></span> <span data-ttu-id="bc07e-110">インスタンスを使用して、ホストが通常作成、 `WebHostBuilder`、ビルドを返す、`WebHost`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="bc07e-110">The host is typically created using an instance of a `WebHostBuilder`, which builds and returns a  `WebHost` instance.</span></span> <span data-ttu-id="bc07e-111">`WebHost`要求を処理するサーバーを参照します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-111">The `WebHost` references the server that will handle requests.</span></span> <span data-ttu-id="bc07e-112">詳細については[サーバー](servers/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-112">Learn more about [servers](servers/index.md).</span></span>

### <a name="what-is-the-difference-between-a-host-and-a-server"></a><span data-ttu-id="bc07e-113">ホストとサーバー間で違いは何ですか。</span><span class="sxs-lookup"><span data-stu-id="bc07e-113">What is the difference between a host and a server?</span></span>

<span data-ttu-id="bc07e-114">ホストは、アプリケーションの起動と有効期間の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-114">The host is responsible for application startup and lifetime management.</span></span> <span data-ttu-id="bc07e-115">サーバーが HTTP 要求の受け入れを担当します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-115">The server is responsible for accepting HTTP requests.</span></span> <span data-ttu-id="bc07e-116">ホストの役割の一部には、アプリケーションのサービスとサーバーは、使用可能で正しく構成されていることを確認が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bc07e-116">Part of the host's responsibility includes ensuring the application's services and the server are available and properly configured.</span></span> <span data-ttu-id="bc07e-117">サーバーをラップするラッパーとしてホストを検討することができます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-117">You can think of the host as being a wrapper around the server.</span></span> <span data-ttu-id="bc07e-118">ホストが、特定のサーバーを使用するよう構成します。サーバーでは、そのホストの認識しません。</span><span class="sxs-lookup"><span data-stu-id="bc07e-118">The host is configured to use a particular server; the server is unaware of its host.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="bc07e-119">ホストの設定</span><span class="sxs-lookup"><span data-stu-id="bc07e-119">Setting up a Host</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bc07e-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bc07e-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bc07e-121">インスタンスを使用してホストを作成する`WebHostBuilder`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-121">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="bc07e-122">これは通常、アプリのエントリ ポイントで実行: `public static void Main` (プロジェクト テンプレート内にある、 *Program.cs*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-122">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="bc07e-123">一般的な*Program.cs*ように、以下を使用する方法を示します、`WebHostBuilder`ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-123">A typical *Program.cs*, shown below, demonstrates how to use a `WebHostBuilder` to build a host.</span></span>

<span data-ttu-id="bc07e-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span><span class="sxs-lookup"><span data-stu-id="bc07e-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span></span>

<span data-ttu-id="bc07e-125">`WebHostBuilder`責任は、アプリのサーバーのブートス トラップは、ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-125">The `WebHostBuilder` is responsible for creating the host that will bootstrap the server for the app.</span></span> <span data-ttu-id="bc07e-126">`WebHostBuilder`実装するサーバーを指定する必要があります`IServer`(`UseKestrel`上記のコードで)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-126">`WebHostBuilder` requires you provide a server that implements `IServer` (`UseKestrel` in the code above).</span></span> <span data-ttu-id="bc07e-127">`UseKestrel`アプリで使用される、Kestrel サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-127">`UseKestrel` specifies the Kestrel server will be used by the app.</span></span>

<span data-ttu-id="bc07e-128">サーバーの*コンテンツのルート*MVC ビュー ファイルと同様に、コンテンツ ファイルの検索場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-128">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="bc07e-129">既定のコンテンツのルートは、アプリケーションが実行されるフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="bc07e-129">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="bc07e-130">指定する`Directory.GetCurrentDirectory`のコンテンツのルートは、このフォルダーから、アプリが開始したときに、アプリのコンテンツのルートとしての web プロジェクトのルート フォルダーが使用されます (たとえば、呼び出し`dotnet run`web プロジェクト フォルダーから)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-130">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="bc07e-131">これは Visual Studio で使用される既定値と`dotnet new`テンプレート。</span><span class="sxs-lookup"><span data-stu-id="bc07e-131">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="bc07e-132">リバース プロキシとして、IIS を使用するには、呼び出す`UseIISIntegration`ホストの作成の一部として。</span><span class="sxs-lookup"><span data-stu-id="bc07e-132">To use IIS as a reverse proxy, call `UseIISIntegration` as part of building the host.</span></span> 

<span data-ttu-id="bc07e-133">なお`UseIISIntegration`が構成されない、*サーバー*と同様、`UseKestrel`はします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-133">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="bc07e-134">ASP.NET Core で IIS を使用して、両方を指定する必要があります`UseKestrel`と`UseIISIntegration`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-134">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="bc07e-135">`UseKestrel`web サーバーを作成し、アプリをホストします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-135">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="bc07e-136">`UseIISIntegration`IIS または iis Express が使用する環境変数を検査し、リッスンするポートを使用するヘッダーなどの設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-136">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bc07e-137">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="bc07e-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bc07e-138">インスタンスを使用してホストを作成する`WebHostBuilder`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-138">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="bc07e-139">これは通常、アプリのエントリ ポイントで実行: `public static void Main` (プロジェクト テンプレート内にある、 *Program.cs*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-139">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="bc07e-140">一般的な*Program.cs*呼び出し次に示すように、`CreateDefaultbuilder`ホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-140">A typical *Program.cs*, shown below, calls `CreateDefaultbuilder` to build a host:</span></span>

<span data-ttu-id="bc07e-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="bc07e-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span></span>

<span data-ttu-id="bc07e-142">`CreateDefaultbuilder`インスタンスを作成`WebHostBuilder`をアプリのサーバーをブートス トラップするホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-142">`CreateDefaultbuilder` creates an instance of `WebHostBuilder` to build the host that bootstraps the server for the app.</span></span> <span data-ttu-id="bc07e-143">ホストが必要とする[IServer を実装しているサーバー](servers/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-143">The host requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="bc07e-144">組み込みのサーバーが[Kestrel](servers/kestrel.md)と[HTTP.sys](servers/httpsys.md)です。`CreateDefaultbuilder` Kestrel 既定で使用します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` use Kestrel by default.</span></span>

<span data-ttu-id="bc07e-145">`CreateDefaultbuilder`web サーバーとして Kestrel を構成するだけでなくのセットアップ タスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-145">`CreateDefaultbuilder` performs set-up tasks in addition to configuring Kestrel as the web server:</span></span>

* <span data-ttu-id="bc07e-146">コンテンツのルートに設定`Directory.GetCurrentDirectory`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-146">Sets the content root to `Directory.GetCurrentDirectory`.</span></span>
* <span data-ttu-id="bc07e-147">構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-147">Loads configuration from:</span></span>
  * <span data-ttu-id="bc07e-148">*される appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="bc07e-148">*appsettings.json*</span></span>
  * <span data-ttu-id="bc07e-149">*appsettings です。\<EnvironmentName > .json*です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-149">*appsettings.\<EnvironmentName>.json*.</span></span>
  * <span data-ttu-id="bc07e-150">開発環境でアプリを実行するときにユーザーの機密情報</span><span class="sxs-lookup"><span data-stu-id="bc07e-150">user secrets when the app runs in the Development environment</span></span>
  * <span data-ttu-id="bc07e-151">環境変数</span><span class="sxs-lookup"><span data-stu-id="bc07e-151">environment variables</span></span>
  * <span data-ttu-id="bc07e-152">指定されたコマンドライン引数</span><span class="sxs-lookup"><span data-stu-id="bc07e-152">supplied command line args</span></span>
* <span data-ttu-id="bc07e-153">[ログの構成] セクションで指定されたルールをフィルター処理にコンソールとデバッグの出力のログ記録を構成します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-153">Configures logging for console and debug output, with filtering rules specified in a Logging configuration section.</span></span>
* <span data-ttu-id="bc07e-154">IIS の統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-154">Enables IIS integration.</span></span>
* <span data-ttu-id="bc07e-155">開発環境でアプリを実行するときは、開発者の例外のページを追加します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-155">Adds the developer exception page when the app runs in the Development environment.</span></span>

<span data-ttu-id="bc07e-156">サーバーの*コンテンツのルート*MVC ビュー ファイルと同様に、コンテンツ ファイルの検索場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-156">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="bc07e-157">既定のコンテンツのルートは、アプリケーションが実行されるフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="bc07e-157">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="bc07e-158">指定する`Directory.GetCurrentDirectory`のコンテンツのルートは、このフォルダーから、アプリが開始したときに、アプリのコンテンツのルートとしての web プロジェクトのルート フォルダーが使用されます (たとえば、呼び出し`dotnet run`web プロジェクト フォルダーから)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-158">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="bc07e-159">これは Visual Studio で使用される既定値と`dotnet new`テンプレート。</span><span class="sxs-lookup"><span data-stu-id="bc07e-159">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="bc07e-160">ASP.NET Core を自動的に呼び出して、リバース プロキシとして IIS を使用するときに`UseIISIntegration`ホストの作成の一部として。</span><span class="sxs-lookup"><span data-stu-id="bc07e-160">When you use IIS as a reverse proxy, ASP.NET Core automatically calls `UseIISIntegration` as part of building the host.</span></span> <span data-ttu-id="bc07e-161">詳細については、次を参照してください。 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-161">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="bc07e-162">なお`UseIISIntegration`が構成されない、*サーバー*と同様、`UseKestrel`はします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-162">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="bc07e-163">`UseKestrel`web サーバーを作成し、アプリをホストします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-163">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="bc07e-164">`UseIISIntegration`IIS または iis Express が使用する環境変数を検査し、リッスンするポートを使用するヘッダーなどの設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-164">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

---

<span data-ttu-id="bc07e-165">ホスト (と ASP.NET Core アプリケーション) を構成する最小限の実装は、サーバーとアプリケーションの要求パイプラインの構成のみを含めます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-165">A minimal implementation of configuring a host (and an ASP.NET Core app) would include just a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> <span data-ttu-id="bc07e-166">ホストをセットアップするときに使用できる`Configure`と`ConfigureServices`メソッドを指定するだけでなく、`Startup`クラス (- これらのメソッドを定義することもあります。 を参照してください[アプリケーションの起動](startup.md))。</span><span class="sxs-lookup"><span data-stu-id="bc07e-166">When setting up a host, you can provide `Configure` and `ConfigureServices` methods, instead of or in addition to specifying a `Startup` class (which must also define these methods - see [Application Startup](startup.md)).</span></span> <span data-ttu-id="bc07e-167">複数回呼び出す`ConfigureServices`互いに追加されます。 呼び出しを`Configure`または`UseStartup`以前の設定を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-167">Multiple calls to `ConfigureServices` will append to one another; calls to `Configure` or `UseStartup` will replace previous settings.</span></span>

## <a name="configuring-a-host"></a><span data-ttu-id="bc07e-168">ホストの構成</span><span class="sxs-lookup"><span data-stu-id="bc07e-168">Configuring a Host</span></span>

<span data-ttu-id="bc07e-169">`WebHostBuilder`を使用して直接設定することもできるホストのほとんどの利用可能な構成値を設定するためのメソッドを提供`UseSetting`と関連するキー。</span><span class="sxs-lookup"><span data-stu-id="bc07e-169">The `WebHostBuilder` provides methods for setting most of the available configuration values for the host, which can also be set directly using `UseSetting` and associated key.</span></span>

### <a name="host-configuration-values"></a><span data-ttu-id="bc07e-170">ホストの構成値</span><span class="sxs-lookup"><span data-stu-id="bc07e-170">Host Configuration Values</span></span>

<span data-ttu-id="bc07e-171">**スタートアップ エラーがキャプチャされる**`bool`</span><span class="sxs-lookup"><span data-stu-id="bc07e-171">**Capture Startup Errors** `bool`</span></span>

<span data-ttu-id="bc07e-172">キー:`captureStartupErrors`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-172">Key: `captureStartupErrors`.</span></span> <span data-ttu-id="bc07e-173">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-173">Defaults to `false`.</span></span> <span data-ttu-id="bc07e-174">ときに`false`起動の結果、終了ホスト中のエラーです。</span><span class="sxs-lookup"><span data-stu-id="bc07e-174">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="bc07e-175">ときに`true`、ホストからすべての例外をキャプチャする、`Startup`クラスし、サーバーを起動しようとしています。</span><span class="sxs-lookup"><span data-stu-id="bc07e-175">When `true`, the host will capture any exceptions from the `Startup` class and attempt to start the server.</span></span> <span data-ttu-id="bc07e-176">エラー ページが表示されます (ジェネリック、または詳細、詳細なエラーの設定に基づいての下) の要求ごとにします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-176">It will display an error page (generic, or detailed, based on the Detailed Errors setting, below) for every request.</span></span> <span data-ttu-id="bc07e-177">使用して設定、`CaptureStartupErrors`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bc07e-177">Set using the `CaptureStartupErrors` method.</span></span>

<span data-ttu-id="bc07e-178">注: Kestrel および IIS で、アプリの実行時に既定の動作はスタートアップ エラーをキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-178">Note: When your app runs with Kestrel and IIS, the default behavior is to capture startup errors.</span></span> 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

<span data-ttu-id="bc07e-179">**コンテンツのルート**`string`</span><span class="sxs-lookup"><span data-stu-id="bc07e-179">**Content Root** `string`</span></span>

<span data-ttu-id="bc07e-180">キー:`contentRoot`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-180">Key: `contentRoot`.</span></span> <span data-ttu-id="bc07e-181">既定値は (Kestrel; 用アプリケーション アセンブリが存在するフォルダーIIS web プロジェクトのルート既定で使用されます)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-181">Defaults to the folder where the application assembly resides (for Kestrel; IIS will use the web project root by default).</span></span> <span data-ttu-id="bc07e-182">この設定は、ASP.NET Core の MVC ビューなどのコンテンツ ファイルの検索開始位置を決定します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-182">This setting determines where ASP.NET Core will begin searching for content files, such as MVC Views.</span></span> <span data-ttu-id="bc07e-183">ベース パスとしても使用、 [Web ルート設定](#web-root-setting)です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-183">Also used as the base path for the [Web Root Setting](#web-root-setting).</span></span> <span data-ttu-id="bc07e-184">使用して設定、`UseContentRoot`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bc07e-184">Set using the `UseContentRoot` method.</span></span> <span data-ttu-id="bc07e-185">パスが存在するか、ホストが起動しなくなります。</span><span class="sxs-lookup"><span data-stu-id="bc07e-185">Path must exist, or host will fail to start.</span></span>

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

<span data-ttu-id="bc07e-186">**詳細なエラー**`bool`</span><span class="sxs-lookup"><span data-stu-id="bc07e-186">**Detailed Errors** `bool`</span></span>

<span data-ttu-id="bc07e-187">キー:`detailedErrors`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-187">Key: `detailedErrors`.</span></span> <span data-ttu-id="bc07e-188">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-188">Defaults to `false`.</span></span> <span data-ttu-id="bc07e-189">ときに`true`(または"Development"に環境を設定すると)、アプリの一般的なエラー ページだけではなく、起動の例外の詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-189">When `true` (or when Environment is set to "Development"), the app will display details of startup exceptions, instead of just a generic error page.</span></span> <span data-ttu-id="bc07e-190">使用して設定`UseSetting`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-190">Set using `UseSetting`.</span></span>

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

<span data-ttu-id="bc07e-191">詳細なエラー情報を設定すると`false`起動エラーのキャプチャは`true`、サーバーに対するすべての要求に対する応答で汎用エラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-191">When Detailed Errors is set to `false` and Capture Startup Errors is `true`, a generic error page is displayed in response to every request to the server.</span></span>

![一般的なエラー ページ](hosting/_static/generic-error-page.png)

<span data-ttu-id="bc07e-193">詳細なエラー情報を設定すると`true`起動エラーのキャプチャは`true`、サーバーに対するすべての要求に対する応答の詳細なエラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-193">When Detailed Errors is set to `true` and Capture Startup Errors is `true`, a detailed error page is displayed in response to every request to the server.</span></span>

![詳細なエラー ページ](hosting/_static/detailed-error-page.png)

<span data-ttu-id="bc07e-195">**環境**`string`</span><span class="sxs-lookup"><span data-stu-id="bc07e-195">**Environment** `string`</span></span>

<span data-ttu-id="bc07e-196">キー:`environment`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-196">Key: `environment`.</span></span> <span data-ttu-id="bc07e-197">既定値は"Production"です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-197">Defaults to "Production".</span></span> <span data-ttu-id="bc07e-198">任意の値に設定することがあります。</span><span class="sxs-lookup"><span data-stu-id="bc07e-198">May be set to any value.</span></span> <span data-ttu-id="bc07e-199">フレームワークによって定義された値には、"Development"、「ステージング」および"Production"が含まれます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-199">Framework-defined values include "Development", "Staging", and "Production".</span></span> <span data-ttu-id="bc07e-200">値は、大文字小文字を区別はありません。</span><span class="sxs-lookup"><span data-stu-id="bc07e-200">Values are not case sensitive.</span></span> <span data-ttu-id="bc07e-201">参照してください[複数の環境で作業](environments.md)です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-201">See [Working with Multiple Environments](environments.md).</span></span> <span data-ttu-id="bc07e-202">使用して設定、`UseEnvironment`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bc07e-202">Set using the `UseEnvironment` method.</span></span>

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> <span data-ttu-id="bc07e-203">既定では、環境がから読み取られた、`ASPNETCORE_ENVIRONMENT`環境変数。</span><span class="sxs-lookup"><span data-stu-id="bc07e-203">By default, the environment is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="bc07e-204">環境変数を設定することがあります Visual Studio を使用する場合、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bc07e-204">When using Visual Studio, environment variables may be set in the *launchSettings.json* file.</span></span>

<a id="server-urls"></a>

<span data-ttu-id="bc07e-205">**サーバーの Url**`string`</span><span class="sxs-lookup"><span data-stu-id="bc07e-205">**Server URLs** `string`</span></span>

<span data-ttu-id="bc07e-206">キー:`urls`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-206">Key: `urls`.</span></span> <span data-ttu-id="bc07e-207">セミコロン (;) に設定区切り URL の一覧の前に、サーバーが応答する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bc07e-207">Set to a semicolon (;) separated list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="bc07e-208">たとえば、`http://localhost:123` のようにします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-208">For example, `http://localhost:123`.</span></span> <span data-ttu-id="bc07e-209">ドメインまたはホスト名が置き換えられます"\*"サーバーは任意の IP アドレスで要求をリッスンか、指定したポートとプロトコルを使用してホストを示すために (たとえば、`http://*:5000`または`https://*:5001`)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-209">The domain/host name can be replaced with "\*" to indicate the server should listen to requests on any IP address or host using the specified port and protocol (for example, `http://*:5000` or `https://*:5001`).</span></span> <span data-ttu-id="bc07e-210">プロトコル (`http://`または`https://`) 各 url を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="bc07e-210">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="bc07e-211">プレフィックスは、構成されたサーバーによって解釈されます。サポートされている形式は、サーバー間で異なります。</span><span class="sxs-lookup"><span data-stu-id="bc07e-211">The prefixes are interpreted by the configured server; supported formats will vary between servers.</span></span>

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="bc07e-212">ASP.NET Core 2.0 Kestrel が独自のエンドポイント構成 API ではサポートしていません`https://`で、`urls`文字列。</span><span class="sxs-lookup"><span data-stu-id="bc07e-212">In ASP.NET Core 2.0, Kestrel has its own endpoint configuration API and does not support `https://` in the `urls` string.</span></span> <span data-ttu-id="bc07e-213">詳細については、次を参照してください。 [Kestrel 概要](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-213">For more information, see [Introduction to Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

<span data-ttu-id="bc07e-214">**スタートアップ アセンブリ**`string`</span><span class="sxs-lookup"><span data-stu-id="bc07e-214">**Startup Assembly** `string`</span></span>

<span data-ttu-id="bc07e-215">キー:`startupAssembly`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-215">Key: `startupAssembly`.</span></span> <span data-ttu-id="bc07e-216">検索対象となるアセンブリの決定、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="bc07e-216">Determines the assembly to search for the `Startup` class.</span></span> <span data-ttu-id="bc07e-217">使用して設定、`UseStartup`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bc07e-217">Set using the `UseStartup` method.</span></span> <span data-ttu-id="bc07e-218">特定の種類を使用する代わりに参照`WebHostBuilder.UseStartup<StartupType>`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-218">May instead reference specific type using `WebHostBuilder.UseStartup<StartupType>`.</span></span> <span data-ttu-id="bc07e-219">複数`UseStartup`メソッドが呼び出されると、最後の 1 つが優先されます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-219">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

<span data-ttu-id="bc07e-220">**ルートの web**`string`</span><span class="sxs-lookup"><span data-stu-id="bc07e-220">**Web Root** `string`</span></span>

<span data-ttu-id="bc07e-221">キー:`webroot`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-221">Key: `webroot`.</span></span> <span data-ttu-id="bc07e-222">既定値は指定されていない場合`(Content Root Path)\wwwroot`存在する場合、します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-222">If not specified the default is `(Content Root Path)\wwwroot`, if it exists.</span></span> <span data-ttu-id="bc07e-223">このパスが存在しない、no-op ファイル プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-223">If this path doesn't exist, then a no-op file provider is used.</span></span> <span data-ttu-id="bc07e-224">使用して設定`UseWebRoot`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-224">Set using `UseWebRoot`.</span></span>

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a><span data-ttu-id="bc07e-225">構成をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-225">Overriding Configuration</span></span>

<span data-ttu-id="bc07e-226">使用して[構成](configuration.md)ホストによって使用される構成値を設定します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-226">Use [Configuration](configuration.md) to set configuration values to be used by the host.</span></span> <span data-ttu-id="bc07e-227">これらの値は、後でオーバーライドされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bc07e-227">These values may be subsequently overridden.</span></span> <span data-ttu-id="bc07e-228">使用してこれが指定されて`UseConfiguration`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-228">This is specified using `UseConfiguration`.</span></span>

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

<span data-ttu-id="bc07e-229">上記の例でコマンドライン引数可能性があるに渡された、ホストを構成するかに構成設定を指定できます必要に応じて、 *hosting.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bc07e-229">In the example above, command-line arguments may be passed in to configure the host, or configuration settings may optionally be specified in a *hosting.json* file.</span></span> <span data-ttu-id="bc07e-230">特定の URL で実行するホストを指定するには、コマンド プロンプトから目的の値で渡す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bc07e-230">To specify the host run on a particular URL, you could pass in the desired value from a command prompt:</span></span>

```console
dotnet run --urls "http://*:5000"
```

<span data-ttu-id="bc07e-231">`Run`メソッドは、web アプリが開始され、ホストがシャット ダウンするまで、呼び出し元のスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-231">The `Run` method starts the web app and blocks the calling thread until the host is shutdown.</span></span>

```csharp
host.Run();
```

<span data-ttu-id="bc07e-232">非ブロッキングの形で、ホストを実行するには呼び出すことによってその`Start`メソッド。</span><span class="sxs-lookup"><span data-stu-id="bc07e-232">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="bc07e-233">Url の一覧を渡す、`Start`メソッドと、指定された Url でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="bc07e-233">Pass a list of URLs to the `Start` method and it will listen on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="bc07e-234">ここでは有効な URL の形式は、使用しているサーバーに依存します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-234">The URL formats that are valid here depend on the server you're using.</span></span> <span data-ttu-id="bc07e-235">詳細については、次を参照してください。[サーバー Url](#server-urls)この記事で前述しました。</span><span class="sxs-lookup"><span data-stu-id="bc07e-235">For more information, see [Server URLs](#server-urls) earlier in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="bc07e-236">`UseConfiguration`拡張メソッドは現在によって返される構成セクションを解析できる`GetSection`(たとえば、`.UseConfiguration(Configuration.GetSection("section"))`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-236">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="bc07e-237">`GetSection`メソッド要求のセクションに構成キーをフィルター処理が残りますセクションの名前のキーに (たとえば、 `section:urls`、 `section:environment`)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-237">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="bc07e-238">`UseConfiguration`メソッドに一致するキーが必要ですが、`WebHostBuilder`キー (たとえば、 `urls`、 `environment`)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-238">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="bc07e-239">キーにセクション名の有無は、セクションの値が、ホストを構成することを防止します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-239">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="bc07e-240">この問題は、将来のリリースで対処される予定です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-240">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="bc07e-241">詳細と回避策については、次を参照してください。[フル キーを使用して WebHostBuilder.UseConfiguration に構成セクションを渡して](https://github.com/aspnet/Hosting/issues/839)です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-241">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="ordering-importance"></a><span data-ttu-id="bc07e-242">重要度の順序</span><span class="sxs-lookup"><span data-stu-id="bc07e-242">Ordering Importance</span></span>

<span data-ttu-id="bc07e-243">`WebHostBuilder`場合設定が特定の環境変数から読み取る最初に設定します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-243">`WebHostBuilder` settings are first read from certain environment variables, if set.</span></span> <span data-ttu-id="bc07e-244">これらの環境変数の形式を使用する必要があります`ASPNETCORE_{configurationKey}`ため、設定は既定では、サーバーがリッスン Url を設定する例については、`ASPNETCORE_URLS`です。</span><span class="sxs-lookup"><span data-stu-id="bc07e-244">These environment variables must use the format `ASPNETCORE_{configurationKey}`, so for example to set the URLs the server will listen on by default, you would set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="bc07e-245">これらの環境変数の値のいずれかの構成を指定することによってオーバーライドできます (を使用して`UseConfiguration`) または値を明示的に設定して (を使用して`UseUrls`のインスタンス)。</span><span class="sxs-lookup"><span data-stu-id="bc07e-245">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseUrls` for instance).</span></span> <span data-ttu-id="bc07e-246">どちらのオプション設定値が最後に、ホストが使用されます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-246">The host will use whichever option sets the value last.</span></span> <span data-ttu-id="bc07e-247">このため、`UseIISIntegration`後に表示する必要があります`UseUrls`IIS で提供される動的に 1 つを URL に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bc07e-247">For this reason, `UseIISIntegration` must appear after `UseUrls`, because it replaces the URL with one dynamically provided by IIS.</span></span> <span data-ttu-id="bc07e-248">プログラムで 1 つの値に設定されて、既定の URL が構成をオーバーライドすることを許可する場合は、次のようにホストを構成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bc07e-248">If you want to programmatically set the default URL to one value, but allow it to be overridden with configuration, you could configure the host as follows:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="bc07e-249">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="bc07e-249">Additional resources</span></span>

* [<span data-ttu-id="bc07e-250">IIS を使用して Windows への公開します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-250">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="bc07e-251">Nginx を使用して Linux への公開します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-251">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="bc07e-252">Apache を使用して Linux への公開します。</span><span class="sxs-lookup"><span data-stu-id="bc07e-252">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="bc07e-253">Windows サービスのホスト</span><span class="sxs-lookup"><span data-stu-id="bc07e-253">Host in a Windows Service</span></span>](xref:hosting/windows-service)

