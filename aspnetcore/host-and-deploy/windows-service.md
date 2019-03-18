---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/08/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ecc7f3a8cd813c2803d03294e38d726905eeb1b8
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841424"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="7cac2-103">Windows サービスでの ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="7cac2-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="7cac2-104">著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7cac2-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7cac2-105">ASP.NET Core アプリは、IIS を 使用せずに、[Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications)として Windows にホストできます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="7cac2-106">Windows サービスとしてホストされている場合、再起動後にアプリが自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="7cac2-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="7cac2-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7cac2-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="7cac2-108">Prerequisites</span></span>

* [<span data-ttu-id="7cac2-109">PowerShell 6</span><span class="sxs-lookup"><span data-stu-id="7cac2-109">PowerShell 6</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="deployment-type"></a><span data-ttu-id="7cac2-110">配置の種類</span><span class="sxs-lookup"><span data-stu-id="7cac2-110">Deployment type</span></span>

<span data-ttu-id="7cac2-111">フレームワーク依存型または自己完結型いずれかの Windows サービス展開を作成できます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-111">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="7cac2-112">展開のシナリオに関する情報および注意事項については、「[.NET Core アプリケーションの展開](/dotnet/core/deploying/)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-112">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="7cac2-113">フレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="7cac2-113">Framework-dependent deployment</span></span>

<span data-ttu-id="7cac2-114">フレームワーク依存型展開 (FDD) は、ターゲット システムに .NET Core のシステム全体の共有バージョンが存在することに依存します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-114">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="7cac2-115">FDD シナリオを ASP.NET Core Windows サービス アプリに対して使用すると、"*フレームワーク依存型実行可能ファイル*" と呼ばれる実行可能ファイル (*\*.exe*) が SDK によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-115">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="7cac2-116">自己完結型の展開</span><span class="sxs-lookup"><span data-stu-id="7cac2-116">Self-contained deployment</span></span>

<span data-ttu-id="7cac2-117">自己完結型の展開 (SCD) は、ターゲット システムに共有コンポーネントが存在することに依存しません。</span><span class="sxs-lookup"><span data-stu-id="7cac2-117">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="7cac2-118">ランタイムとアプリの依存関係が、アプリと共にホスティング システムに展開されます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-118">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="7cac2-119">プロジェクトを Windows サービスに変換する</span><span class="sxs-lookup"><span data-stu-id="7cac2-119">Convert a project into a Windows Service</span></span>

<span data-ttu-id="7cac2-120">既存の ASP.NET Core プロジェクトに次の変更を加え、アプリをサービスとして実行します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-120">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="7cac2-121">プロジェクト ファイルの更新</span><span class="sxs-lookup"><span data-stu-id="7cac2-121">Project file updates</span></span>

<span data-ttu-id="7cac2-122">どの[展開の種類](#deployment-type)を選択したかに基づいて、プロジェクト ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-122">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="7cac2-123">フレームワーク依存型展開 (FDD)</span><span class="sxs-lookup"><span data-stu-id="7cac2-123">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="7cac2-124">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) を、ターゲット フレームワークを含む `<PropertyGroup>` に追加します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-124">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="7cac2-125">次の例では、RID が `win7-x64` に設定されています。</span><span class="sxs-lookup"><span data-stu-id="7cac2-125">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="7cac2-126">`false` に設定した `<SelfContained>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-126">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="7cac2-127">これらのプロパティによって、Windows 用の実行可能ファイル (*.exe*) を生成するよう SDK に指示します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-127">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="7cac2-128">*web.config* ファイル (通常 ASP.NET Core アプリを発行する際に生成されます) は、Windows サービス アプリに対しては必要ありません。</span><span class="sxs-lookup"><span data-stu-id="7cac2-128">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="7cac2-129">*web.config* ファイルの作成を無効にするには、`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-129">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

::: moniker range=">= aspnetcore-2.2"

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

<span data-ttu-id="7cac2-130">`true` に設定した `<UseAppHost>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-130">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="7cac2-131">このプロパティによって、サービスに FDD のアクティブ化パス (実行可能ファイル、*.exe*) を指定します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-131">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="7cac2-132">自己完結型の展開 (SCD)</span><span class="sxs-lookup"><span data-stu-id="7cac2-132">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="7cac2-133">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) があることを確認するか、RID をターゲット フレームワークを含む `<PropertyGroup>` に追加します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-133">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="7cac2-134">`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加して、*web.config* ファイルの作成を無効にします。</span><span class="sxs-lookup"><span data-stu-id="7cac2-134">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="7cac2-135">複数の RID を発行するには、次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-135">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="7cac2-136">セミコロンで区切られたリストの形式で RID を指定します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-136">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="7cac2-137">プロパティ名 `<RuntimeIdentifiers>` (複数形) を使用します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-137">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="7cac2-138">詳細については、「[.NET Core の RID カタログ](/dotnet/core/rid-catalog)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-138">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="7cac2-139">[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-139">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="7cac2-140">Windows イベント ログのログ記録を有効にするには、[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-140">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="7cac2-141">詳しくは、「[イベントの開始と停止を扱う](#handle-starting-and-stopping-events)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-141">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="7cac2-142">Program.Main の更新</span><span class="sxs-lookup"><span data-stu-id="7cac2-142">Program.Main updates</span></span>

<span data-ttu-id="7cac2-143">`Program.Main` で次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="7cac2-143">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="7cac2-144">サービスの外部で実行しているときにテストとデバッグを行うには、アプリがサービスとして実行しているかコンソール アプリとして実行しているかを判別するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-144">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="7cac2-145">デバッガーがアタッチされているか、`--console` コマンドライン引数が存在するかを検査します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-145">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="7cac2-146">いずれかの条件が満たされる場合 (アプリがサービスとして実行していない場合)、Web ホストで <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-146">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="7cac2-147">条件が満たされない場合 (アプリがサービスとして実行している場合):</span><span class="sxs-lookup"><span data-stu-id="7cac2-147">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="7cac2-148"><xref:System.IO.Directory.SetCurrentDirectory*> を呼び出し、アプリの発行場所のパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-148">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="7cac2-149">パスを取得するために <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出さないでください。<xref:System.IO.Directory.GetCurrentDirectory*> が呼び出されると、Windows サービス アプリは *C:\\WINDOWS\\system32* フォルダーを戻すためです。</span><span class="sxs-lookup"><span data-stu-id="7cac2-149">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="7cac2-150">詳しくは、「[現在のディレクトリとコンテンツのルート](#current-directory-and-content-root)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-150">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="7cac2-151"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> を呼び出して、アプリをサービスとして実行します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-151">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="7cac2-152">[コマンドライン構成プロバイダー](xref:fundamentals/configuration/index#command-line-configuration-provider)では、コマンドライン引数に名前と値の組が必要であるため、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> が引数を受け取る前に `--console` スイッチは引数から削除されます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-152">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="7cac2-153">Windows イベント ログに書き込むには、EventLog プロバイダーを <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*> に追加します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-153">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="7cac2-154">*appsettings.Production.json* ファイルで `Logging:LogLevel:Default` キーを使用してログ レベルを設定します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-154">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="7cac2-155">デモとテストの目的のために、サンプル アプリの Production 設定ファイルでは、ログ レベルが `Information` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-155">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="7cac2-156">運用環境で、値は通常は `Error` に設定します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-156">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="7cac2-157">詳細については、「<xref:fundamentals/logging/index#windows-eventlog-provider>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-157">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="7cac2-158">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="7cac2-158">Publish the app</span></span>

<span data-ttu-id="7cac2-159">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish)、[Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)、または Visual Studio Code を使用してアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-159">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="7cac2-160">Visual Studio を使用する場合、**[発行]** ボタンを選択する前に、**[FolderProfile]** を選択して **[ターゲットの場所]** を構成します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-160">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="7cac2-161">コマンドライン インターフェイス (CLI) ツールを使用してサンプル アプリを発行するには、プロジェクト フォルダーからコマンド プロンプトで [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを実行し、その際にリリース構成を [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) オプションに渡します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-161">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="7cac2-162">アプリ外部のフォルダーに発行するには、[-o|--output](/dotnet/core/tools/dotnet-publish#options) オプションをパスと一緒に使用します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-162">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="7cac2-163">フレームワーク依存型展開 (FDD) を発行する</span><span class="sxs-lookup"><span data-stu-id="7cac2-163">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="7cac2-164">次の例では、アプリは *c:\\svc* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-164">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="7cac2-165">自己完結型展開 (SCD) を発行する</span><span class="sxs-lookup"><span data-stu-id="7cac2-165">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="7cac2-166">プロジェクト ファイルの `<RuntimeIdenfifier>` (または `<RuntimeIdentifiers>`) プロパティに RID を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7cac2-166">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="7cac2-167">ランタイムを `dotnet publish` コマンドの [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) オプションに指定します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-167">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="7cac2-168">次の例では、`win7-x64` ランタイムに関してアプリが *c:\\svc* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-168">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="7cac2-169">ユーザー アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="7cac2-169">Create a user account</span></span>

<span data-ttu-id="7cac2-170">PowerShell 6 の管理コマンド シェルから `net user` コマンドを使って、サービスのユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-170">Create a user account for the service using the `net user` command from an administrative PowerShell 6 command shell:</span></span>

```powershell
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="7cac2-171">既定のパスワードの有効期限は 6 週間です。</span><span class="sxs-lookup"><span data-stu-id="7cac2-171">The default password expiration is six weeks.</span></span>

<span data-ttu-id="7cac2-172">サンプル アプリでは、名前 `ServiceUser` とパスワードを持つユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-172">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="7cac2-173">次のコマンド内の `{PASSWORD}` を、[強力なパスワード](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-173">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```powershell
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="7cac2-174">ユーザーをグループに追加する必要がある場合は、`net localgroup` コマンドを使用します。`{GROUP}` にはグループの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-174">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```powershell
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="7cac2-175">詳細については、「[Service User Accounts](/windows/desktop/services/service-user-accounts)」(サービス ユーザー アカウント) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-175">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="7cac2-176">Active Directory を使う場合、ユーザーを管理するための別の方法は、マネージド サービス アカウントを使うことです。</span><span class="sxs-lookup"><span data-stu-id="7cac2-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="7cac2-177">詳細については、「[Group Managed Service Accounts Overview (グループ マネージド サービス アカウントの概要)](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="7cac2-178">アクセス許可の設定: サービスとしてログオンする</span><span class="sxs-lookup"><span data-stu-id="7cac2-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="7cac2-179">[icacls](/windows-server/administration/windows-commands/icacls) コマンドを使用して、アプリのフォルダーに書き込み/読み取り/実行アクセス許可を与えます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

```powershell
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="7cac2-180">`{PATH}` &ndash; アプリのフォルダーへのパス。</span><span class="sxs-lookup"><span data-stu-id="7cac2-180">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="7cac2-181">`{USER ACCOUNT}` &ndash; ユーザー アカウント (SID)。</span><span class="sxs-lookup"><span data-stu-id="7cac2-181">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="7cac2-182">`(OI)` &ndash; オブジェクトの継承フラグ。下位のファイルにアクセス許可を反映します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-182">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="7cac2-183">`(CI)` &ndash; コンテナーの継承フラグ。下位のフォルダーにアクセス許可を反映します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-183">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="7cac2-184">`{PERMISSION FLAGS}` &ndash; アプリのアクセス許可を設定します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-184">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="7cac2-185">書き込み (`W`)</span><span class="sxs-lookup"><span data-stu-id="7cac2-185">Write (`W`)</span></span>
  * <span data-ttu-id="7cac2-186">読み取り (`R`)</span><span class="sxs-lookup"><span data-stu-id="7cac2-186">Read (`R`)</span></span>
  * <span data-ttu-id="7cac2-187">実行 (`X`)</span><span class="sxs-lookup"><span data-stu-id="7cac2-187">Execute (`X`)</span></span>
  * <span data-ttu-id="7cac2-188">フル アクセス (`F`)</span><span class="sxs-lookup"><span data-stu-id="7cac2-188">Full (`F`)</span></span>
  * <span data-ttu-id="7cac2-189">変更 (`M`)</span><span class="sxs-lookup"><span data-stu-id="7cac2-189">Modify (`M`)</span></span>
* <span data-ttu-id="7cac2-190">`/t` &ndash; 存在する下位のフォルダーおよびファイルに再帰的に適用します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-190">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="7cac2-191">*c:\\svc* フォルダーに発行されるサンプル アプリと、書き込み/読み取り/実行アクセス許可を持つ `ServiceUser` アカウントに対して、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```powershell
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="7cac2-192">詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="7cac2-193">サービスを作成する</span><span class="sxs-lookup"><span data-stu-id="7cac2-193">Create the service</span></span>

<span data-ttu-id="7cac2-194">サービスを登録するには、[RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell スクリプトを使います。</span><span class="sxs-lookup"><span data-stu-id="7cac2-194">Use the [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="7cac2-195">PowerShell 6 の管理コマンド プロンプトから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-195">From an administrative PowerShell 6 command prompt, execute the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Path "{PATH}" 
    -Exe {ASSEMBLY}.exe 
    -User {DOMAIN\USER}
```

<span data-ttu-id="7cac2-196">サンプル アプリの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="7cac2-197">サービスは **MyService** という名前です。</span><span class="sxs-lookup"><span data-stu-id="7cac2-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="7cac2-198">発行されたサービスは、*c:\\svc* フォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="7cac2-199">アプリの実行可能ファイルの名前は *SampleApp.exe* です。</span><span class="sxs-lookup"><span data-stu-id="7cac2-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="7cac2-200">サービスは `ServiceUser` アカウントで実行されます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="7cac2-201">次の例では、ローカル コンピューターの名前は `Desktop-PC` です。</span><span class="sxs-lookup"><span data-stu-id="7cac2-201">In the following example, the local machine name is `Desktop-PC`.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Path "c:\svc" 
    -Exe SampleApp.exe 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="7cac2-202">サービスを管理する</span><span class="sxs-lookup"><span data-stu-id="7cac2-202">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="7cac2-203">サービスを開始する</span><span class="sxs-lookup"><span data-stu-id="7cac2-203">Start the service</span></span>

<span data-ttu-id="7cac2-204">PowerShell 6 の `Start-Service -Name {NAME}` コマンドでサービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-204">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="7cac2-205">サンプル アプリ サービスを開始するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-205">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="7cac2-206">このコマンドでサービスを開始するには数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="7cac2-206">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="7cac2-207">サービスの状態を確認する</span><span class="sxs-lookup"><span data-stu-id="7cac2-207">Determine the service status</span></span>

<span data-ttu-id="7cac2-208">サービスの状態を確認するには、PowerShell 6 の `Get-Service -Name {NAME}` コマンドを使います。</span><span class="sxs-lookup"><span data-stu-id="7cac2-208">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="7cac2-209">この状態は、次のいずれかの値として報告されます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-209">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="7cac2-210">次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-210">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="7cac2-211">Web アプリ サービスを参照する</span><span class="sxs-lookup"><span data-stu-id="7cac2-211">Browse a web app service</span></span>

<span data-ttu-id="7cac2-212">サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000` )。</span><span class="sxs-lookup"><span data-stu-id="7cac2-212">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="7cac2-213">サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-213">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="7cac2-214">サービスを停止して</span><span class="sxs-lookup"><span data-stu-id="7cac2-214">Stop the service</span></span>

<span data-ttu-id="7cac2-215">PowerShell 6 の `Stop-Service -Name {NAME}` コマンドでサービスを停止します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-215">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="7cac2-216">サンプル アプリ サービスは、次のコマンドで停止できます。</span><span class="sxs-lookup"><span data-stu-id="7cac2-216">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="7cac2-217">サービスを削除する</span><span class="sxs-lookup"><span data-stu-id="7cac2-217">Remove the service</span></span>

<span data-ttu-id="7cac2-218">サービスを停止した後少ししてから、Powershell 6 の `Remove-Service -Name {NAME}` コマンドを使ってサービスを削除します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-218">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="7cac2-219">サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-219">Check the status of the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="7cac2-220">イベントの開始と停止を扱う</span><span class="sxs-lookup"><span data-stu-id="7cac2-220">Handle starting and stopping events</span></span>

<span data-ttu-id="7cac2-221"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>、および <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> イベントを処理するには、次の追加の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="7cac2-221">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="7cac2-222">`OnStarting`、`OnStarted`、および `OnStopping` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-222">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="7cac2-223">`CustomWebHostService` を <xref:System.ServiceProcess.ServiceBase.Run*> に渡す <xref:Microsoft.AspNetCore.Hosting.IWebHost> の拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-223">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="7cac2-224">`Program.Main` で、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> ではなく、拡張メソッド `RunAsCustomService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-224">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="7cac2-225">`Program.Main` 内の <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> の場所を確認するには、「[プロジェクトを Windows サービスに変換する](#convert-a-project-into-a-windows-service)」セクションに示されているコード サンプルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-225">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="7cac2-226">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="7cac2-226">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="7cac2-227">インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="7cac2-227">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="7cac2-228">詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7cac2-228">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="7cac2-229">HTTPS の構成</span><span class="sxs-lookup"><span data-stu-id="7cac2-229">Configure HTTPS</span></span>

<span data-ttu-id="7cac2-230">セキュリティで保護されたエンドポイントを使用してサービスを構成するには:</span><span class="sxs-lookup"><span data-stu-id="7cac2-230">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="7cac2-231">プラットフォームの証明書の取得と展開のメカニズムを使用して、ホスト システム用に X.509 証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-231">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="7cac2-232">[Kestrel サーバーの HTTPS エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)を指定して、証明書を使用します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-232">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="7cac2-233">サービス エンドポイントをセキュリティで保護するために ASP.NET Core の HTTPS 開発証明書を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="7cac2-233">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="7cac2-234">現在のディレクトリとコンテンツのルート</span><span class="sxs-lookup"><span data-stu-id="7cac2-234">Current directory and content root</span></span>

<span data-ttu-id="7cac2-235">Windows サービスに対して <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出して返される現在の作業ディレクトリは *C:\\WINDOWS\\system32* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="7cac2-235">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="7cac2-236">*system32* フォルダーは、サービスのファイル (設定ファイルなど) を保存するために適した場所ではありません。</span><span class="sxs-lookup"><span data-stu-id="7cac2-236">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="7cac2-237">次のいずれかの方法を使用して、サービスのアセットと設定ファイルを管理し、アクセスします。</span><span class="sxs-lookup"><span data-stu-id="7cac2-237">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="7cac2-238">アプリのフォルダーにコンテンツ ルート パスを設定する</span><span class="sxs-lookup"><span data-stu-id="7cac2-238">Set the content root path to the app's folder</span></span>

<span data-ttu-id="7cac2-239"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> は、サービスの作成時に `binPath` 引数に指定されたものと同じパスです。</span><span class="sxs-lookup"><span data-stu-id="7cac2-239">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="7cac2-240">`GetCurrentDirectory` を呼び出して設定ファイルへのパスを作成する代わりに、アプリのコンテンツ ルートへのパスを指定して <xref:System.IO.Directory.SetCurrentDirectory*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-240">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="7cac2-241">`Program.Main` で、サービスの実行可能ファイルがあるフォルダーへのパスを判別し、そのパスを使用してアプリのコンテンツ ルートを確立します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-241">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="7cac2-242">ディスク上の適切な場所にサービスのファイルを格納します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-242">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="7cac2-243">ファイルを含むフォルダーに対して <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> を使用するときは、<xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> を使用して絶対パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="7cac2-243">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7cac2-244">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7cac2-244">Additional resources</span></span>

* <span data-ttu-id="7cac2-245">[Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)</span><span class="sxs-lookup"><span data-stu-id="7cac2-245">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
