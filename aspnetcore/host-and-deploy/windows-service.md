---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/21/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ab36bc1b2827c80bb1e7b9e8cee558b346a991f8
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251428"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="ba64f-103">Windows サービスでの ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="ba64f-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="ba64f-104">著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ba64f-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ba64f-105">ASP.NET Core アプリは、IIS を 使用せずに、[Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications)として Windows にホストできます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="ba64f-106">Windows サービスとしてホストされている場合、サーバーの再起動後にアプリが自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="ba64f-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ba64f-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba64f-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="ba64f-108">Prerequisites</span></span>

* [<span data-ttu-id="ba64f-109">ASP.NET Core SDK 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="ba64f-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="ba64f-110">PowerShell 6.2 以降</span><span class="sxs-lookup"><span data-stu-id="ba64f-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="ba64f-111">アプリの構成</span><span class="sxs-lookup"><span data-stu-id="ba64f-111">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ba64f-112">[Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) パッケージによって提供される `IHostBuilder.UseWindowsService` は、ホストのビルド時に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-112">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="ba64f-113">アプリが Windows サービスとして実行している場合、メソッドは</span><span class="sxs-lookup"><span data-stu-id="ba64f-113">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="ba64f-114">ホストの有効期間を `WindowsServiceLifetime` に設定します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-114">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="ba64f-115">コンテンツのルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-115">Sets the content root.</span></span>
* <span data-ttu-id="ba64f-116">既定のソース名として、アプリケーション名によるイベント ログへの記録を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ba64f-116">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="ba64f-117">*appsettings.Production.json* ファイルで `Logging:LogLevel:Default` キーを使用してログ レベルを構成できます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-117">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="ba64f-118">管理者のみが新しいイベント ソースを作成できます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-118">Only administrators can create new event sources.</span></span> <span data-ttu-id="ba64f-119">アプリケーション名を使用して、イベント ソースを作成できない場合、警告が*アプリケーション* ソースに記録され、イベント ログが無効になります。</span><span class="sxs-lookup"><span data-stu-id="ba64f-119">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ba64f-120">アプリでは、[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) と [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) へのパッケージ参照が必要です。</span><span class="sxs-lookup"><span data-stu-id="ba64f-120">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="ba64f-121">サービスの外部で実行しているときにテストとデバッグを行うには、アプリがサービスとして実行しているかコンソール アプリとして実行しているかを判別するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-121">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="ba64f-122">デバッガーがアタッチされているか、`--console` スイッチが存在するかを検査します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-122">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="ba64f-123">いずれかの条件が満たされる場合 (アプリがサービスとして実行していない場合)、<xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-123">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="ba64f-124">条件が満たされない場合 (アプリがサービスとして実行している場合):</span><span class="sxs-lookup"><span data-stu-id="ba64f-124">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="ba64f-125"><xref:System.IO.Directory.SetCurrentDirectory*> を呼び出し、アプリの発行場所のパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-125">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="ba64f-126">パスを取得するために <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出さないでください。<xref:System.IO.Directory.GetCurrentDirectory*> が呼び出されると、Windows サービス アプリは *C:\\WINDOWS\\system32* フォルダーを戻すためです。</span><span class="sxs-lookup"><span data-stu-id="ba64f-126">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="ba64f-127">詳しくは、「[現在のディレクトリとコンテンツのルート](#current-directory-and-content-root)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ba64f-127">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="ba64f-128">`CreateWebHostBuilder` でアプリを構成する前に、この手順に従います。</span><span class="sxs-lookup"><span data-stu-id="ba64f-128">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="ba64f-129"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> を呼び出して、アプリをサービスとして実行します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-129">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="ba64f-130">[コマンドライン構成プロバイダー](xref:fundamentals/configuration/index#command-line-configuration-provider)では、コマンドライン引数に名前と値の組が必要であるため、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> が引数を受け取る前に `--console` スイッチは引数から削除されます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-130">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="ba64f-131">Windows イベント ログに書き込むには、EventLog プロバイダーを <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*> に追加します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-131">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="ba64f-132">*appsettings.Production.json* ファイルで `Logging:LogLevel:Default` キーを使用してログ レベルを設定します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-132">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="ba64f-133">サンプル アプリの次の例では、アプリ内で有効期間イベントを処理するために、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> の代わりに `RunAsCustomService` を呼び出しています。</span><span class="sxs-lookup"><span data-stu-id="ba64f-133">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="ba64f-134">詳しくは、「[イベントの開始と停止を扱う](#handle-starting-and-stopping-events)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ba64f-134">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="ba64f-135">配置の種類</span><span class="sxs-lookup"><span data-stu-id="ba64f-135">Deployment type</span></span>

<span data-ttu-id="ba64f-136">展開のシナリオに関する情報および注意事項については、「[.NET Core アプリケーションの展開](/dotnet/core/deploying/)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ba64f-136">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="ba64f-137">フレームワーク依存型展開 (FDD)</span><span class="sxs-lookup"><span data-stu-id="ba64f-137">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="ba64f-138">フレームワーク依存型展開 (FDD) は、ターゲット システムに .NET Core のシステム全体の共有バージョンが存在することに依存します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-138">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="ba64f-139">この記事のガイダンスに従って、FDD シナリオを採用すると、*フレームワーク依存型実行可能ファイル* と呼ばれる実行可能ファイル ( *.exe*) が SDK によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-139">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ba64f-140">プロジェクト ファイルに次のプロパティ要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-140">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="ba64f-141">`<OutputType>` &ndash; アプリの出力の種類 (実行可能ファイルの場合 `Exe`)。</span><span class="sxs-lookup"><span data-stu-id="ba64f-141">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="ba64f-142">`<LangVersion>` &ndash; C# 言語バージョン (`latest` または `preview`)。</span><span class="sxs-lookup"><span data-stu-id="ba64f-142">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="ba64f-143">*web.config* ファイル (通常 ASP.NET Core アプリを発行する際に生成されます) は、Windows サービス アプリに対しては必要ありません。</span><span class="sxs-lookup"><span data-stu-id="ba64f-143">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ba64f-144">*web.config* ファイルの作成を無効にするには、`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-144">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ba64f-145">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) にはターゲット フレームワークが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-145">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="ba64f-146">次の例では、RID が `win7-x64` に設定されています。</span><span class="sxs-lookup"><span data-stu-id="ba64f-146">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="ba64f-147">`<SelfContained>` プロパティが `false` に設定されている。</span><span class="sxs-lookup"><span data-stu-id="ba64f-147">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="ba64f-148">これらのプロパティによって、Windows 用の実行可能ファイル ( *.exe*) と共有 .NET Core フレームワークに依存するアプリを生成するよう SDK に指示します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-148">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="ba64f-149">*web.config* ファイル (通常 ASP.NET Core アプリを発行する際に生成されます) は、Windows サービス アプリに対しては必要ありません。</span><span class="sxs-lookup"><span data-stu-id="ba64f-149">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ba64f-150">*web.config* ファイルの作成を無効にするには、`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-150">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="ba64f-151">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) にはターゲット フレームワークが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-151">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="ba64f-152">次の例では、RID が `win7-x64` に設定されています。</span><span class="sxs-lookup"><span data-stu-id="ba64f-152">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="ba64f-153">`<SelfContained>` プロパティが `false` に設定されている。</span><span class="sxs-lookup"><span data-stu-id="ba64f-153">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="ba64f-154">これらのプロパティによって、Windows 用の実行可能ファイル ( *.exe*) と共有 .NET Core フレームワークに依存するアプリを生成するよう SDK に指示します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-154">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="ba64f-155">`<UseAppHost>` プロパティが `true` に設定されている。</span><span class="sxs-lookup"><span data-stu-id="ba64f-155">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="ba64f-156">このプロパティによって、サービスに FDD のアクティブ化パス (実行可能ファイル、 *.exe*) を指定します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-156">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="ba64f-157">*web.config* ファイル (通常 ASP.NET Core アプリを発行する際に生成されます) は、Windows サービス アプリに対しては必要ありません。</span><span class="sxs-lookup"><span data-stu-id="ba64f-157">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="ba64f-158">*web.config* ファイルの作成を無効にするには、`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-158">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="ba64f-159">自己完結型の展開 (SCD)</span><span class="sxs-lookup"><span data-stu-id="ba64f-159">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="ba64f-160">自己完結型の展開 (SCD) は、ホスト システムに共有フレームワークが存在することに依存しません。</span><span class="sxs-lookup"><span data-stu-id="ba64f-160">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="ba64f-161">ランタイムとアプリの依存関係が、アプリと共に展開されます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-161">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="ba64f-162">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) は、ターゲット フレームワークを格納する `<PropertyGroup>` に含まれます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-162">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="ba64f-163">複数の RID を発行するには、次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-163">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="ba64f-164">セミコロンで区切られたリストの形式で RID を指定します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-164">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="ba64f-165">プロパティ名 [ \<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (複数) を使用します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-165">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="ba64f-166">詳細については、「[.NET Core の RID カタログ](/dotnet/core/rid-catalog)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ba64f-166">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ba64f-167">`<SelfContained>` プロパティが `true` に設定されています。</span><span class="sxs-lookup"><span data-stu-id="ba64f-167">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="ba64f-168">サービス ユーザー アカウント</span><span class="sxs-lookup"><span data-stu-id="ba64f-168">Service user account</span></span>

<span data-ttu-id="ba64f-169">サービス用のユーザー アカウントを作成するには、PowerShell 6 の管理コマンド シェルから [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-169">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="ba64f-170">Windows 10 October 2018 Update (バージョン 1809/ビルド 10.0.17763) 以降:</span><span class="sxs-lookup"><span data-stu-id="ba64f-170">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="ba64f-171">Windows 10 October 2018 Update (バージョン 1809/ビルド 10.0.17763) 以前の Windows OS:</span><span class="sxs-lookup"><span data-stu-id="ba64f-171">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="ba64f-172">入力を求められたら、[強力なパスワード](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)を指定します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="ba64f-173">[New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) コマンドレットに <xref:System.DateTime> という有効期限で `-AccountExpires` パラメーターを指定しない場合、アカウントは期限切れになりません。</span><span class="sxs-lookup"><span data-stu-id="ba64f-173">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="ba64f-174">詳しくは、「[Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/)」および「[Service User Accounts (サービス ユーザー アカウント)](/windows/desktop/services/service-user-accounts)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ba64f-174">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="ba64f-175">Active Directory を使う場合、ユーザーを管理するための別の方法は、マネージド サービス アカウントを使うことです。</span><span class="sxs-lookup"><span data-stu-id="ba64f-175">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="ba64f-176">詳細については、「[Group Managed Service Accounts Overview (グループ マネージド サービス アカウントの概要)](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ba64f-176">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="ba64f-177">サービスとしてログオン権利</span><span class="sxs-lookup"><span data-stu-id="ba64f-177">Log on as a service rights</span></span>

<span data-ttu-id="ba64f-178">サービス ユーザー アカウントに*サービスとしてログオン*権利を確立するには、次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-178">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="ba64f-179">*secpool.msc* を実行して、ローカル セキュリティ ポリシー エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-179">Open the Local Security Policy editor by running *secpool.msc*.</span></span>
1. <span data-ttu-id="ba64f-180">**[ローカル ポリシー]** ノードを展開し、 **[ユーザー権利の割り当て]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-180">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="ba64f-181">**[サービスとしてログオン]** ポリシーを開きます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-181">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="ba64f-182">**[ユーザーまたはグループの追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-182">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="ba64f-183">次のいずれかの方法を使用して、オブジェクト名 (ユーザー アカウント) を指定します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-183">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="ba64f-184">オブジェクト名フィールドにユーザー アカウント (`{DOMAIN OR COMPUTER NAME\USER}`) を入力し、 **[OK]** を選択して、ポリシーにユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-184">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="ba64f-185">**[詳細]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-185">Select **Advanced**.</span></span> <span data-ttu-id="ba64f-186">**[検索開始]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-186">Select **Find Now**.</span></span> <span data-ttu-id="ba64f-187">一覧からユーザー アカウントを選択します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-187">Select the user account from the list.</span></span> <span data-ttu-id="ba64f-188">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-188">Select **OK**.</span></span> <span data-ttu-id="ba64f-189">再度 **[OK]** を選択して、ポリシーにユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-189">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="ba64f-190">**[OK]** または **[適用]** を選択して、変更を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-190">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="ba64f-191">Windows サービスを作成して管理する</span><span class="sxs-lookup"><span data-stu-id="ba64f-191">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="ba64f-192">サービスを作成する</span><span class="sxs-lookup"><span data-stu-id="ba64f-192">Create a service</span></span>

<span data-ttu-id="ba64f-193">PowerShell コマンドを使用して、サービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-193">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="ba64f-194">PowerShell 6 の管理コマンド シェルから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-194">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="ba64f-195">`{EXE PATH}` &ndash; ホスト上のアプリのフォルダーへのパス (`d:\myservice` など)。</span><span class="sxs-lookup"><span data-stu-id="ba64f-195">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="ba64f-196">このパスに、アプリの実行可能ファイルは含めないでください。</span><span class="sxs-lookup"><span data-stu-id="ba64f-196">Don't include the app's executable in the path.</span></span> <span data-ttu-id="ba64f-197">末尾のスラッシュは、必要ありません。</span><span class="sxs-lookup"><span data-stu-id="ba64f-197">A trailing slash isn't required.</span></span>
* <span data-ttu-id="ba64f-198">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; サービスのユーザー アカウント (`Contoso\ServiceUser` など)。</span><span class="sxs-lookup"><span data-stu-id="ba64f-198">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="ba64f-199">`{NAME}` &ndash; サービス名 (`MyService` など)。</span><span class="sxs-lookup"><span data-stu-id="ba64f-199">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="ba64f-200">`{EXE FILE PATH}` &ndash; アプリの実行可能ファイルのパス (`d:\myservice\myservice.exe` など)。</span><span class="sxs-lookup"><span data-stu-id="ba64f-200">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="ba64f-201">拡張子付きの実行可能ファイルのファイル名を含めます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-201">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="ba64f-202">`{DESCRIPTION}` &ndash; サービスの説明 (`My sample service` など)。</span><span class="sxs-lookup"><span data-stu-id="ba64f-202">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="ba64f-203">`{DISPLAY NAME}` &ndash; サービスの表示名 (`My Service` など)。</span><span class="sxs-lookup"><span data-stu-id="ba64f-203">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="ba64f-204">サービスを開始する</span><span class="sxs-lookup"><span data-stu-id="ba64f-204">Start a service</span></span>

<span data-ttu-id="ba64f-205">次の PowerShell 6 コマンドでサービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-205">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="ba64f-206">このコマンドでサービスを開始するには数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="ba64f-206">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="ba64f-207">サービスの状態を確認する</span><span class="sxs-lookup"><span data-stu-id="ba64f-207">Determine a service's status</span></span>

<span data-ttu-id="ba64f-208">サービスの状態を確認するには、次の PowerShell 6 コマンドを使います。</span><span class="sxs-lookup"><span data-stu-id="ba64f-208">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="ba64f-209">この状態は、次のいずれかの値として報告されます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-209">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="ba64f-210">サービスを停止する</span><span class="sxs-lookup"><span data-stu-id="ba64f-210">Stop a service</span></span>

<span data-ttu-id="ba64f-211">次の PowerShell 6 コマンドでサービスを停止します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-211">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="ba64f-212">サービスを削除する</span><span class="sxs-lookup"><span data-stu-id="ba64f-212">Remove a service</span></span>

<span data-ttu-id="ba64f-213">サービスを停止した後、少ししてから、次の Powershell 6 コマンドを使ってサービスを削除します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-213">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="ba64f-214">イベントの開始と停止を扱う</span><span class="sxs-lookup"><span data-stu-id="ba64f-214">Handle starting and stopping events</span></span>

<span data-ttu-id="ba64f-215"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>、および <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> イベントを処理するには、次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-215">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="ba64f-216">`OnStarting`、`OnStarted`、および `OnStopping` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-216">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="ba64f-217">`CustomWebHostService` を <xref:System.ServiceProcess.ServiceBase.Run*> に渡す <xref:Microsoft.AspNetCore.Hosting.IWebHost> の拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-217">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="ba64f-218">`Program.Main` で、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> ではなく、拡張メソッド `RunAsCustomService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-218">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="ba64f-219">`Program.Main` 内の <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> の場所を確認するには、「[展開の種類](#deployment-type)」セクションに示されているコード サンプルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ba64f-219">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ba64f-220">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="ba64f-220">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ba64f-221">インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ba64f-221">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="ba64f-222">詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ba64f-222">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="ba64f-223">HTTPS の構成</span><span class="sxs-lookup"><span data-stu-id="ba64f-223">Configure HTTPS</span></span>

<span data-ttu-id="ba64f-224">セキュリティで保護されたエンドポイントを使用してサービスを構成するには、次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-224">To configure a service with a secure endpoint:</span></span>

1. <span data-ttu-id="ba64f-225">プラットフォームの証明書の取得と展開のメカニズムを使用して、ホスト システム用に X.509 証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-225">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="ba64f-226">[Kestrel サーバーの HTTPS エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)を指定して、証明書を使用します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-226">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="ba64f-227">サービス エンドポイントをセキュリティで保護するために ASP.NET Core の HTTPS 開発証明書を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ba64f-227">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="ba64f-228">現在のディレクトリとコンテンツのルート</span><span class="sxs-lookup"><span data-stu-id="ba64f-228">Current directory and content root</span></span>

<span data-ttu-id="ba64f-229">Windows サービスに対して <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出して返される現在の作業ディレクトリは *C:\\WINDOWS\\system32* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ba64f-229">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="ba64f-230">*system32* フォルダーは、サービスのファイル (設定ファイルなど) を保存するために適した場所ではありません。</span><span class="sxs-lookup"><span data-stu-id="ba64f-230">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="ba64f-231">次のいずれかの方法を使用して、サービスのアセットと設定ファイルを管理し、アクセスします。</span><span class="sxs-lookup"><span data-stu-id="ba64f-231">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="ba64f-232">ContentRootPath または ContentRootFileProvider を使用する</span><span class="sxs-lookup"><span data-stu-id="ba64f-232">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="ba64f-233">[IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) または <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> を使用して、アプリのリソースを見つけます。</span><span class="sxs-lookup"><span data-stu-id="ba64f-233">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="ba64f-234">アプリのフォルダーにコンテンツ ルート パスを設定する</span><span class="sxs-lookup"><span data-stu-id="ba64f-234">Set the content root path to the app's folder</span></span>

<span data-ttu-id="ba64f-235"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> は、サービスの作成時に `binPath` 引数に指定されたものと同じパスです。</span><span class="sxs-lookup"><span data-stu-id="ba64f-235">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="ba64f-236">`GetCurrentDirectory` を呼び出して設定ファイルへのパスを作成する代わりに、アプリのコンテンツ ルートへのパスを指定して <xref:System.IO.Directory.SetCurrentDirectory*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-236">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="ba64f-237">`Program.Main` で、サービスの実行可能ファイルがあるフォルダーへのパスを判別し、そのパスを使用してアプリのコンテンツ ルートを確立します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-237">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="ba64f-238">ディスク上の適切な場所にサービスのファイルを格納する</span><span class="sxs-lookup"><span data-stu-id="ba64f-238">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="ba64f-239">ファイルを含むフォルダーに対して <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> を使用するときは、<xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> を使用して絶対パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="ba64f-239">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ba64f-240">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ba64f-240">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="ba64f-241">[Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)</span><span class="sxs-lookup"><span data-stu-id="ba64f-241">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="ba64f-242">[Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)</span><span class="sxs-lookup"><span data-stu-id="ba64f-242">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
