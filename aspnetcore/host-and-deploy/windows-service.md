---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ec3a37fd859df7592fa0d6d9cc0109942a570e7a
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086984"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="220a0-103">Windows サービスでの ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="220a0-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="220a0-104">著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="220a0-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="220a0-105">ASP.NET Core アプリは、IIS を 使用せずに、[Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications)として Windows にホストできます。</span><span class="sxs-lookup"><span data-stu-id="220a0-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="220a0-106">Windows サービスとしてホストされている場合、再起動後にアプリが自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="220a0-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="220a0-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="220a0-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="220a0-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="220a0-108">Prerequisites</span></span>

* [<span data-ttu-id="220a0-109">PowerShell 6.2 以降</span><span class="sxs-lookup"><span data-stu-id="220a0-109">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> <span data-ttu-id="220a0-110">Windows 10 October 2018 Update (バージョン 1809/ビルド 10.0.17763) より前の Windows OS の場合、「[ユーザー アカウントを作成する](#create-a-user-account)」セクションで使用する [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) コマンドレットにアクセスするには、[WindowsCompatibility モジュール](https://github.com/PowerShell/WindowsCompatibility)と共に [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) モジュールをインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="220a0-110">For Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763), the [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) module must be imported with the [WindowsCompatibility module](https://github.com/PowerShell/WindowsCompatibility) to gain access to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet used in the [Create a user account](#create-a-user-account) section:</span></span>
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a><span data-ttu-id="220a0-111">配置の種類</span><span class="sxs-lookup"><span data-stu-id="220a0-111">Deployment type</span></span>

<span data-ttu-id="220a0-112">フレームワーク依存型または自己完結型いずれかの Windows サービス展開を作成できます。</span><span class="sxs-lookup"><span data-stu-id="220a0-112">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="220a0-113">展開のシナリオに関する情報および注意事項については、「[.NET Core アプリケーションの展開](/dotnet/core/deploying/)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="220a0-113">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="220a0-114">フレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="220a0-114">Framework-dependent deployment</span></span>

<span data-ttu-id="220a0-115">フレームワーク依存型展開 (FDD) は、ターゲット システムに .NET Core のシステム全体の共有バージョンが存在することに依存します。</span><span class="sxs-lookup"><span data-stu-id="220a0-115">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="220a0-116">FDD シナリオを ASP.NET Core Windows サービス アプリに対して使用すると、"*フレームワーク依存型実行可能ファイル*" と呼ばれる実行可能ファイル (*\*.exe*) が SDK によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="220a0-116">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="220a0-117">自己完結型の展開</span><span class="sxs-lookup"><span data-stu-id="220a0-117">Self-contained deployment</span></span>

<span data-ttu-id="220a0-118">自己完結型の展開 (SCD) は、ターゲット システムに共有コンポーネントが存在することに依存しません。</span><span class="sxs-lookup"><span data-stu-id="220a0-118">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="220a0-119">ランタイムとアプリの依存関係が、アプリと共にホスティング システムに展開されます。</span><span class="sxs-lookup"><span data-stu-id="220a0-119">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="220a0-120">プロジェクトを Windows サービスに変換する</span><span class="sxs-lookup"><span data-stu-id="220a0-120">Convert a project into a Windows Service</span></span>

<span data-ttu-id="220a0-121">既存の ASP.NET Core プロジェクトに次の変更を加え、アプリをサービスとして実行します。</span><span class="sxs-lookup"><span data-stu-id="220a0-121">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="220a0-122">プロジェクト ファイルの更新</span><span class="sxs-lookup"><span data-stu-id="220a0-122">Project file updates</span></span>

<span data-ttu-id="220a0-123">どの[展開の種類](#deployment-type)を選択したかに基づいて、プロジェクト ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="220a0-123">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="220a0-124">フレームワーク依存型展開 (FDD)</span><span class="sxs-lookup"><span data-stu-id="220a0-124">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="220a0-125">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) を、ターゲット フレームワークを含む `<PropertyGroup>` に追加します。</span><span class="sxs-lookup"><span data-stu-id="220a0-125">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="220a0-126">次の例では、RID が `win7-x64` に設定されています。</span><span class="sxs-lookup"><span data-stu-id="220a0-126">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="220a0-127">`false` に設定した `<SelfContained>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="220a0-127">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="220a0-128">これらのプロパティによって、Windows 用の実行可能ファイル (*.exe*) を生成するよう SDK に指示します。</span><span class="sxs-lookup"><span data-stu-id="220a0-128">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="220a0-129">*web.config* ファイル (通常 ASP.NET Core アプリを発行する際に生成されます) は、Windows サービス アプリに対しては必要ありません。</span><span class="sxs-lookup"><span data-stu-id="220a0-129">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="220a0-130">*web.config* ファイルの作成を無効にするには、`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="220a0-130">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="220a0-131">`true` に設定した `<UseAppHost>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="220a0-131">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="220a0-132">このプロパティによって、サービスに FDD のアクティブ化パス (実行可能ファイル、*.exe*) を指定します。</span><span class="sxs-lookup"><span data-stu-id="220a0-132">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="220a0-133">自己完結型の展開 (SCD)</span><span class="sxs-lookup"><span data-stu-id="220a0-133">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="220a0-134">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) があることを確認するか、RID をターゲット フレームワークを含む `<PropertyGroup>` に追加します。</span><span class="sxs-lookup"><span data-stu-id="220a0-134">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="220a0-135">`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加して、*web.config* ファイルの作成を無効にします。</span><span class="sxs-lookup"><span data-stu-id="220a0-135">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="220a0-136">複数の RID を発行するには、次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="220a0-136">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="220a0-137">セミコロンで区切られたリストの形式で RID を指定します。</span><span class="sxs-lookup"><span data-stu-id="220a0-137">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="220a0-138">プロパティ名 `<RuntimeIdentifiers>` (複数形) を使用します。</span><span class="sxs-lookup"><span data-stu-id="220a0-138">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="220a0-139">詳細については、「[.NET Core の RID カタログ](/dotnet/core/rid-catalog)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="220a0-139">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="220a0-140">[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="220a0-140">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="220a0-141">Windows イベント ログのログ記録を有効にするには、[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="220a0-141">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="220a0-142">詳しくは、「[イベントの開始と停止を扱う](#handle-starting-and-stopping-events)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="220a0-142">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="220a0-143">Program.Main の更新</span><span class="sxs-lookup"><span data-stu-id="220a0-143">Program.Main updates</span></span>

<span data-ttu-id="220a0-144">`Program.Main` で次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="220a0-144">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="220a0-145">サービスの外部で実行しているときにテストとデバッグを行うには、アプリがサービスとして実行しているかコンソール アプリとして実行しているかを判別するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="220a0-145">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="220a0-146">デバッガーがアタッチされているか、`--console` コマンドライン引数が存在するかを検査します。</span><span class="sxs-lookup"><span data-stu-id="220a0-146">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="220a0-147">いずれかの条件が満たされる場合 (アプリがサービスとして実行していない場合)、Web ホストで <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="220a0-147">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="220a0-148">条件が満たされない場合 (アプリがサービスとして実行している場合):</span><span class="sxs-lookup"><span data-stu-id="220a0-148">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="220a0-149"><xref:System.IO.Directory.SetCurrentDirectory*> を呼び出し、アプリの発行場所のパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="220a0-149">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="220a0-150">パスを取得するために <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出さないでください。<xref:System.IO.Directory.GetCurrentDirectory*> が呼び出されると、Windows サービス アプリは *C:\\WINDOWS\\system32* フォルダーを戻すためです。</span><span class="sxs-lookup"><span data-stu-id="220a0-150">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="220a0-151">詳しくは、「[現在のディレクトリとコンテンツのルート](#current-directory-and-content-root)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="220a0-151">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="220a0-152"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> を呼び出して、アプリをサービスとして実行します。</span><span class="sxs-lookup"><span data-stu-id="220a0-152">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="220a0-153">[コマンドライン構成プロバイダー](xref:fundamentals/configuration/index#command-line-configuration-provider)では、コマンドライン引数に名前と値の組が必要であるため、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> が引数を受け取る前に `--console` スイッチは引数から削除されます。</span><span class="sxs-lookup"><span data-stu-id="220a0-153">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="220a0-154">Windows イベント ログに書き込むには、EventLog プロバイダーを <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*> に追加します。</span><span class="sxs-lookup"><span data-stu-id="220a0-154">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="220a0-155">*appsettings.Production.json* ファイルで `Logging:LogLevel:Default` キーを使用してログ レベルを設定します。</span><span class="sxs-lookup"><span data-stu-id="220a0-155">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="220a0-156">デモとテストの目的のために、サンプル アプリの Production 設定ファイルでは、ログ レベルが `Information` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="220a0-156">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="220a0-157">運用環境で、値は通常は `Error` に設定します。</span><span class="sxs-lookup"><span data-stu-id="220a0-157">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="220a0-158">詳細については、「<xref:fundamentals/logging/index#windows-eventlog-provider>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="220a0-158">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="220a0-159">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="220a0-159">Publish the app</span></span>

<span data-ttu-id="220a0-160">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish)、[Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)、または Visual Studio Code を使用してアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="220a0-160">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="220a0-161">Visual Studio を使用する場合、**[発行]** ボタンを選択する前に、**[FolderProfile]** を選択して **[ターゲットの場所]** を構成します。</span><span class="sxs-lookup"><span data-stu-id="220a0-161">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="220a0-162">コマンドライン インターフェイス (CLI) ツールを使用してサンプル アプリを発行するには、Windows コマンド シェルでプロジェクト フォルダーから [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを実行し、[-c|--configuration](/dotnet/core/tools/dotnet-publish#options) オプションにリリース構成を渡します。</span><span class="sxs-lookup"><span data-stu-id="220a0-162">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command in a Windows command shell from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="220a0-163">アプリ外部のフォルダーに発行するには、[-o|--output](/dotnet/core/tools/dotnet-publish#options) オプションをパスと一緒に使用します。</span><span class="sxs-lookup"><span data-stu-id="220a0-163">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="220a0-164">フレームワーク依存型展開 (FDD) を発行する</span><span class="sxs-lookup"><span data-stu-id="220a0-164">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="220a0-165">次の例では、アプリは *c:\\svc* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="220a0-165">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="220a0-166">自己完結型展開 (SCD) を発行する</span><span class="sxs-lookup"><span data-stu-id="220a0-166">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="220a0-167">プロジェクト ファイルの `<RuntimeIdenfifier>` (または `<RuntimeIdentifiers>`) プロパティに RID を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="220a0-167">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="220a0-168">ランタイムを `dotnet publish` コマンドの [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) オプションに指定します。</span><span class="sxs-lookup"><span data-stu-id="220a0-168">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="220a0-169">次の例では、`win7-x64` ランタイムに関してアプリが *c:\\svc* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="220a0-169">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="220a0-170">ユーザー アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="220a0-170">Create a user account</span></span>

<span data-ttu-id="220a0-171">PowerShell 6 の管理コマンド シェルから [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) コマンドレットを使用して、サービス用のユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="220a0-171">Create a user account for the service using the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell:</span></span>

```powershell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="220a0-172">入力を求められたら、[強力なパスワード](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)を指定します。</span><span class="sxs-lookup"><span data-stu-id="220a0-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="220a0-173">サンプル アプリでは、`ServiceUser` という名前のユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="220a0-173">For the sample app, create a user account with the name `ServiceUser`.</span></span>

```powershell
New-LocalUser -Name ServiceUser
```

<span data-ttu-id="220a0-174">[New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) コマンドレットに <xref:System.DateTime> という有効期限で `-AccountExpires` パラメーターを指定しない場合、アカウントは期限切れになりません。</span><span class="sxs-lookup"><span data-stu-id="220a0-174">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="220a0-175">詳しくは、「[Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/)」および「[Service User Accounts (サービス ユーザー アカウント)](/windows/desktop/services/service-user-accounts)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="220a0-175">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="220a0-176">Active Directory を使う場合、ユーザーを管理するための別の方法は、マネージド サービス アカウントを使うことです。</span><span class="sxs-lookup"><span data-stu-id="220a0-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="220a0-177">詳細については、「[Group Managed Service Accounts Overview (グループ マネージド サービス アカウントの概要)](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="220a0-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="220a0-178">アクセス許可の設定: サービスとしてログオンする</span><span class="sxs-lookup"><span data-stu-id="220a0-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="220a0-179">PowerShell 6 の管理コマンド シェルから [icacls](/windows-server/administration/windows-commands/icacls) コマンドを使用して、アプリのフォルダーに書き込み/読み取り/実行アクセス許可を与えます。</span><span class="sxs-lookup"><span data-stu-id="220a0-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* <span data-ttu-id="220a0-180">`{PATH}` &ndash; アプリのフォルダーへのパス。</span><span class="sxs-lookup"><span data-stu-id="220a0-180">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="220a0-181">`{USER ACCOUNT}` &ndash; ユーザー アカウント (SID)。</span><span class="sxs-lookup"><span data-stu-id="220a0-181">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="220a0-182">`(OI)` &ndash; オブジェクトの継承フラグ。下位のファイルにアクセス許可を反映します。</span><span class="sxs-lookup"><span data-stu-id="220a0-182">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="220a0-183">`(CI)` &ndash; コンテナーの継承フラグ。下位のフォルダーにアクセス許可を反映します。</span><span class="sxs-lookup"><span data-stu-id="220a0-183">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="220a0-184">`{PERMISSION FLAGS}` &ndash; アプリのアクセス許可を設定します。</span><span class="sxs-lookup"><span data-stu-id="220a0-184">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="220a0-185">書き込み (`W`)</span><span class="sxs-lookup"><span data-stu-id="220a0-185">Write (`W`)</span></span>
  * <span data-ttu-id="220a0-186">読み取り (`R`)</span><span class="sxs-lookup"><span data-stu-id="220a0-186">Read (`R`)</span></span>
  * <span data-ttu-id="220a0-187">実行 (`X`)</span><span class="sxs-lookup"><span data-stu-id="220a0-187">Execute (`X`)</span></span>
  * <span data-ttu-id="220a0-188">フル アクセス (`F`)</span><span class="sxs-lookup"><span data-stu-id="220a0-188">Full (`F`)</span></span>
  * <span data-ttu-id="220a0-189">変更 (`M`)</span><span class="sxs-lookup"><span data-stu-id="220a0-189">Modify (`M`)</span></span>
* <span data-ttu-id="220a0-190">`/t` &ndash; 存在する下位のフォルダーおよびファイルに再帰的に適用します。</span><span class="sxs-lookup"><span data-stu-id="220a0-190">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="220a0-191">*c:\\svc* フォルダーに発行されるサンプル アプリと、書き込み/読み取り/実行アクセス許可を持つ `ServiceUser` アカウントに対して、PowerShell 6 の管理コマンド シェルで次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="220a0-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

<span data-ttu-id="220a0-192">詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="220a0-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="220a0-193">サービスを作成する</span><span class="sxs-lookup"><span data-stu-id="220a0-193">Create the service</span></span>

<span data-ttu-id="220a0-194">サービスを登録するには、[RegisterService.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell スクリプトを使います。</span><span class="sxs-lookup"><span data-stu-id="220a0-194">Use the [RegisterService.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="220a0-195">PowerShell 6 の管理コマンド シェルから、次のコマンドでスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="220a0-195">From an administrative PowerShell 6 command shell, execute the script with the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

<span data-ttu-id="220a0-196">サンプル アプリの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="220a0-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="220a0-197">サービスは **MyService** という名前です。</span><span class="sxs-lookup"><span data-stu-id="220a0-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="220a0-198">発行されたサービスは、*c:\\svc* フォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="220a0-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="220a0-199">アプリの実行可能ファイルの名前は *SampleApp.exe* です。</span><span class="sxs-lookup"><span data-stu-id="220a0-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="220a0-200">サービスは `ServiceUser` アカウントで実行されます。</span><span class="sxs-lookup"><span data-stu-id="220a0-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="220a0-201">次のコマンド例では、ローカル コンピューターの名前は `Desktop-PC` です。</span><span class="sxs-lookup"><span data-stu-id="220a0-201">In the following example command, the local machine name is `Desktop-PC`.</span></span> <span data-ttu-id="220a0-202">`Desktop-PC` は、自分のシステムのコンピューター名またはシステムに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="220a0-202">Replace `Desktop-PC` with the computer name or domain for your system.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="220a0-203">サービスを管理する</span><span class="sxs-lookup"><span data-stu-id="220a0-203">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="220a0-204">サービスを開始する</span><span class="sxs-lookup"><span data-stu-id="220a0-204">Start the service</span></span>

<span data-ttu-id="220a0-205">PowerShell 6 の `Start-Service -Name {NAME}` コマンドでサービスを開始します。</span><span class="sxs-lookup"><span data-stu-id="220a0-205">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="220a0-206">サンプル アプリ サービスを開始するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="220a0-206">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="220a0-207">このコマンドでサービスを開始するには数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="220a0-207">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="220a0-208">サービスの状態を確認する</span><span class="sxs-lookup"><span data-stu-id="220a0-208">Determine the service status</span></span>

<span data-ttu-id="220a0-209">サービスの状態を確認するには、PowerShell 6 の `Get-Service -Name {NAME}` コマンドを使います。</span><span class="sxs-lookup"><span data-stu-id="220a0-209">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="220a0-210">この状態は、次のいずれかの値として報告されます。</span><span class="sxs-lookup"><span data-stu-id="220a0-210">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="220a0-211">次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="220a0-211">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="220a0-212">Web アプリ サービスを参照する</span><span class="sxs-lookup"><span data-stu-id="220a0-212">Browse a web app service</span></span>

<span data-ttu-id="220a0-213">サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000` )。</span><span class="sxs-lookup"><span data-stu-id="220a0-213">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="220a0-214">サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。</span><span class="sxs-lookup"><span data-stu-id="220a0-214">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="220a0-215">サービスを停止して</span><span class="sxs-lookup"><span data-stu-id="220a0-215">Stop the service</span></span>

<span data-ttu-id="220a0-216">PowerShell 6 の `Stop-Service -Name {NAME}` コマンドでサービスを停止します。</span><span class="sxs-lookup"><span data-stu-id="220a0-216">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="220a0-217">サンプル アプリ サービスは、次のコマンドで停止できます。</span><span class="sxs-lookup"><span data-stu-id="220a0-217">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="220a0-218">サービスを削除する</span><span class="sxs-lookup"><span data-stu-id="220a0-218">Remove the service</span></span>

<span data-ttu-id="220a0-219">サービスを停止した後少ししてから、Powershell 6 の `Remove-Service -Name {NAME}` コマンドを使ってサービスを削除します。</span><span class="sxs-lookup"><span data-stu-id="220a0-219">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="220a0-220">次のコマンドでは、サンプル アプリ サービスを削除します。</span><span class="sxs-lookup"><span data-stu-id="220a0-220">The following command removes the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="220a0-221">イベントの開始と停止を扱う</span><span class="sxs-lookup"><span data-stu-id="220a0-221">Handle starting and stopping events</span></span>

<span data-ttu-id="220a0-222"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>、および <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> イベントを処理するには、次の追加の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="220a0-222">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="220a0-223">`OnStarting`、`OnStarted`、および `OnStopping` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="220a0-223">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="220a0-224">`CustomWebHostService` を <xref:System.ServiceProcess.ServiceBase.Run*> に渡す <xref:Microsoft.AspNetCore.Hosting.IWebHost> の拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="220a0-224">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="220a0-225">`Program.Main` で、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> ではなく、拡張メソッド `RunAsCustomService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="220a0-225">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="220a0-226">`Program.Main` 内の <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> の場所を確認するには、「[プロジェクトを Windows サービスに変換する](#convert-a-project-into-a-windows-service)」セクションに示されているコード サンプルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="220a0-226">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="220a0-227">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="220a0-227">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="220a0-228">インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="220a0-228">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="220a0-229">詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="220a0-229">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="220a0-230">HTTPS の構成</span><span class="sxs-lookup"><span data-stu-id="220a0-230">Configure HTTPS</span></span>

<span data-ttu-id="220a0-231">セキュリティで保護されたエンドポイントを使用してサービスを構成するには:</span><span class="sxs-lookup"><span data-stu-id="220a0-231">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="220a0-232">プラットフォームの証明書の取得と展開のメカニズムを使用して、ホスト システム用に X.509 証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="220a0-232">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="220a0-233">[Kestrel サーバーの HTTPS エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)を指定して、証明書を使用します。</span><span class="sxs-lookup"><span data-stu-id="220a0-233">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="220a0-234">サービス エンドポイントをセキュリティで保護するために ASP.NET Core の HTTPS 開発証明書を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="220a0-234">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="220a0-235">現在のディレクトリとコンテンツのルート</span><span class="sxs-lookup"><span data-stu-id="220a0-235">Current directory and content root</span></span>

<span data-ttu-id="220a0-236">Windows サービスに対して <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出して返される現在の作業ディレクトリは *C:\\WINDOWS\\system32* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="220a0-236">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="220a0-237">*system32* フォルダーは、サービスのファイル (設定ファイルなど) を保存するために適した場所ではありません。</span><span class="sxs-lookup"><span data-stu-id="220a0-237">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="220a0-238">次のいずれかの方法を使用して、サービスのアセットと設定ファイルを管理し、アクセスします。</span><span class="sxs-lookup"><span data-stu-id="220a0-238">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="220a0-239">アプリのフォルダーにコンテンツ ルート パスを設定する</span><span class="sxs-lookup"><span data-stu-id="220a0-239">Set the content root path to the app's folder</span></span>

<span data-ttu-id="220a0-240"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> は、サービスの作成時に `binPath` 引数に指定されたものと同じパスです。</span><span class="sxs-lookup"><span data-stu-id="220a0-240">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="220a0-241">`GetCurrentDirectory` を呼び出して設定ファイルへのパスを作成する代わりに、アプリのコンテンツ ルートへのパスを指定して <xref:System.IO.Directory.SetCurrentDirectory*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="220a0-241">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="220a0-242">`Program.Main` で、サービスの実行可能ファイルがあるフォルダーへのパスを判別し、そのパスを使用してアプリのコンテンツ ルートを確立します。</span><span class="sxs-lookup"><span data-stu-id="220a0-242">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="220a0-243">ディスク上の適切な場所にサービスのファイルを格納します。</span><span class="sxs-lookup"><span data-stu-id="220a0-243">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="220a0-244">ファイルを含むフォルダーに対して <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> を使用するときは、<xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> を使用して絶対パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="220a0-244">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="220a0-245">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="220a0-245">Additional resources</span></span>

* <span data-ttu-id="220a0-246">[Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)</span><span class="sxs-lookup"><span data-stu-id="220a0-246">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
