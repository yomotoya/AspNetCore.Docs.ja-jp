---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f53c303dc63e092f08e933fea79eb805523cde9b
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861395"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="c54fd-103">Windows サービスでの ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="c54fd-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="c54fd-104">著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c54fd-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c54fd-105">ASP.NET Core アプリは、IIS を 使用せずに、[Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications)として Windows にホストできます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="c54fd-106">Windows サービスとしてホストされている場合、再起動後にアプリが自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="c54fd-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c54fd-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="c54fd-108">配置の種類</span><span class="sxs-lookup"><span data-stu-id="c54fd-108">Deployment type</span></span>

<span data-ttu-id="c54fd-109">フレームワーク依存型または自己完結型いずれかの Windows サービス展開を作成できます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="c54fd-110">展開のシナリオに関する情報および注意事項については、「[.NET Core アプリケーションの展開](/dotnet/core/deploying/)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="c54fd-111">フレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="c54fd-111">Framework-dependent deployment</span></span>

<span data-ttu-id="c54fd-112">フレームワーク依存型展開 (FDD) は、ターゲット システムに .NET Core のシステム全体の共有バージョンが存在することに依存します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="c54fd-113">FDD シナリオを ASP.NET Core Windows サービス アプリに対して使用すると、"*フレームワーク依存型実行可能ファイル*" と呼ばれる実行可能ファイル (*\*.exe*) が SDK によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="c54fd-114">自己完結型の展開</span><span class="sxs-lookup"><span data-stu-id="c54fd-114">Self-contained deployment</span></span>

<span data-ttu-id="c54fd-115">自己完結型の展開 (SCD) は、ターゲット システムに共有コンポーネントが存在することに依存しません。</span><span class="sxs-lookup"><span data-stu-id="c54fd-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="c54fd-116">ランタイムとアプリの依存関係が、アプリと共にホスティング システムに展開されます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="c54fd-117">プロジェクトを Windows サービスに変換する</span><span class="sxs-lookup"><span data-stu-id="c54fd-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="c54fd-118">既存の ASP.NET Core プロジェクトに次の変更を加え、アプリをサービスとして実行します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="c54fd-119">プロジェクト ファイルの更新</span><span class="sxs-lookup"><span data-stu-id="c54fd-119">Project file updates</span></span>

<span data-ttu-id="c54fd-120">どの[展開の種類](#deployment-type)を選択したかに基づいて、プロジェクト ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-120">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="c54fd-121">フレームワーク依存型展開 (FDD)</span><span class="sxs-lookup"><span data-stu-id="c54fd-121">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="c54fd-122">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) を、ターゲット フレームワークを含む `<PropertyGroup>` に追加します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-122">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="c54fd-123">`false` に設定した `<SelfContained>` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-123">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="c54fd-124">`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加して、*web.config* ファイルの作成を無効にします。</span><span class="sxs-lookup"><span data-stu-id="c54fd-124">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="c54fd-125">自己完結型の展開 (SCD)</span><span class="sxs-lookup"><span data-stu-id="c54fd-125">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="c54fd-126">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) があることを確認するか、RID をターゲット フレームワークを含む `<PropertyGroup>` に追加します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-126">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="c54fd-127">`true` に設定した `<IsTransformWebConfigDisabled>` プロパティを追加して、*web.config* ファイルの作成を無効にします。</span><span class="sxs-lookup"><span data-stu-id="c54fd-127">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="c54fd-128">複数の RID を発行するには、次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-128">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="c54fd-129">セミコロンで区切られたリストの形式で RID を指定します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-129">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="c54fd-130">プロパティ名 `<RuntimeIdentifiers>` (複数形) を使用します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-130">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="c54fd-131">詳細については、「[.NET Core の RID カタログ](/dotnet/core/rid-catalog)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-131">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="c54fd-132">[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-132">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="c54fd-133">Windows イベント ログのログ記録を有効にするには、[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-133">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="c54fd-134">詳しくは、「[イベントの開始と停止を扱う](#handle-starting-and-stopping-events)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-134">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="c54fd-135">Program.Main の更新</span><span class="sxs-lookup"><span data-stu-id="c54fd-135">Program.Main updates</span></span>

<span data-ttu-id="c54fd-136">`Program.Main` で次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="c54fd-136">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="c54fd-137">サービスの外部で実行しているときにテストとデバッグを行うには、アプリがサービスとして実行しているかコンソール アプリとして実行しているかを判別するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-137">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="c54fd-138">デバッガーがアタッチされているか、`--console` コマンドライン引数が存在するかを検査します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-138">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="c54fd-139">いずれかの条件が満たされる場合 (アプリがサービスとして実行していない場合)、Web ホストで <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-139">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="c54fd-140">条件が満たされない場合 (アプリがサービスとして実行している場合):</span><span class="sxs-lookup"><span data-stu-id="c54fd-140">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="c54fd-141"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> を呼び出し、アプリの発行場所のパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-141">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> and use a path to the app's published location.</span></span> <span data-ttu-id="c54fd-142">パスを取得するために <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出さないでください。`GetCurrentDirectory` が呼び出されると、Windows サービス アプリは *C:\\WINDOWS\\system32* フォルダーを戻すためです。</span><span class="sxs-lookup"><span data-stu-id="c54fd-142">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when `GetCurrentDirectory` is called.</span></span> <span data-ttu-id="c54fd-143">詳しくは、「[現在のディレクトリとコンテンツのルート](#current-directory-and-content-root)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-143">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="c54fd-144"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> を呼び出して、アプリをサービスとして実行します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-144">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="c54fd-145">[コマンドライン構成プロバイダー](xref:fundamentals/configuration/index#command-line-configuration-provider)では、コマンドライン引数に名前と値の組が必要であるため、<xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> が引数を受け取る前に `--console` スイッチは引数から削除されます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-145">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="c54fd-146">Windows イベント ログに書き込むには、EventLog プロバイダーを <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*> に追加します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-146">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="c54fd-147">*appsettings.Production.json* ファイルで `Logging:LogLevel:Default` キーを使用してログ レベルを設定します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-147">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="c54fd-148">デモとテストの目的のために、サンプル アプリの Production 設定ファイルでは、ログ レベルが `Information` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-148">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="c54fd-149">運用環境で、値は通常は `Error` に設定します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-149">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="c54fd-150">詳細については、「<xref:fundamentals/logging/index#windows-eventlog-provider>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-150">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a><span data-ttu-id="c54fd-151">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="c54fd-151">Publish the app</span></span>

<span data-ttu-id="c54fd-152">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish)、[Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)、または Visual Studio Code を使用してアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-152">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="c54fd-153">Visual Studio を使用する場合、**[発行]** ボタンを選択する前に、**[FolderProfile]** を選択して **[ターゲットの場所]** を構成します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-153">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="c54fd-154">コマンドライン インターフェイス (CLI) ツールを使用してサンプル アプリを発行するには、プロジェクト フォルダーからコマンド プロンプトで [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを実行し、その際にリリース構成を [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) オプションに渡します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-154">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="c54fd-155">アプリ外部のフォルダーに発行するには、[-o|--output](/dotnet/core/tools/dotnet-publish#options) オプションをパスと一緒に使用します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-155">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

#### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="c54fd-156">フレームワーク依存型展開 (FDD) を発行する</span><span class="sxs-lookup"><span data-stu-id="c54fd-156">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="c54fd-157">次の例では、アプリは *c:\\svc* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-157">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="c54fd-158">自己完結型展開 (SCD) を発行する</span><span class="sxs-lookup"><span data-stu-id="c54fd-158">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="c54fd-159">プロジェクト ファイルの `<RuntimeIdenfifier>` (または `<RuntimeIdentifiers>`) プロパティに RID を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c54fd-159">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="c54fd-160">ランタイムを `dotnet publish` コマンドの [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) オプションに指定します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-160">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="c54fd-161">次の例では、`win7-x64` ランタイムに関してアプリが *c:\\svc* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-161">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a><span data-ttu-id="c54fd-162">ユーザー アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="c54fd-162">Create a user account</span></span>

<span data-ttu-id="c54fd-163">`net user` コマンドを使用して、サービス用のユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-163">Create a user account for the service using the `net user` command:</span></span>

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="c54fd-164">サンプル アプリでは、名前 `ServiceUser` とパスワードを持つユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-164">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="c54fd-165">次のコマンド内の `{PASSWORD}` を、[強力なパスワード](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-165">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```console
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="c54fd-166">ユーザーをグループに追加する必要がある場合は、`net localgroup` コマンドを使用します。`{GROUP}` にはグループの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-166">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="c54fd-167">詳細については、「[Service User Accounts](/windows/desktop/services/service-user-accounts)」(サービス ユーザー アカウント) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-167">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

### <a name="set-permissions"></a><span data-ttu-id="c54fd-168">アクセス許可を設定する</span><span class="sxs-lookup"><span data-stu-id="c54fd-168">Set permissions</span></span>

<span data-ttu-id="c54fd-169">[icacls](/windows-server/administration/windows-commands/icacls) コマンドを使用して、アプリのフォルダーに書き込み/読み取り/実行アクセス許可を与えます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-169">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="c54fd-170">`{PATH}` &ndash; アプリのフォルダーへのパス。</span><span class="sxs-lookup"><span data-stu-id="c54fd-170">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="c54fd-171">`{USER ACCOUNT}` &ndash; ユーザー アカウント (SID)。</span><span class="sxs-lookup"><span data-stu-id="c54fd-171">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="c54fd-172">`(OI)` &ndash; オブジェクトの継承フラグ。下位のファイルにアクセス許可を反映します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-172">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="c54fd-173">`(CI)` &ndash; コンテナーの継承フラグ。下位のフォルダーにアクセス許可を反映します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-173">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="c54fd-174">`{PERMISSION FLAGS}` &ndash; アプリのアクセス許可を設定します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-174">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="c54fd-175">書き込み (`W`)</span><span class="sxs-lookup"><span data-stu-id="c54fd-175">Write (`W`)</span></span>
  * <span data-ttu-id="c54fd-176">読み取り (`R`)</span><span class="sxs-lookup"><span data-stu-id="c54fd-176">Read (`R`)</span></span>
  * <span data-ttu-id="c54fd-177">実行 (`X`)</span><span class="sxs-lookup"><span data-stu-id="c54fd-177">Execute (`X`)</span></span>
  * <span data-ttu-id="c54fd-178">フル アクセス (`F`)</span><span class="sxs-lookup"><span data-stu-id="c54fd-178">Full (`F`)</span></span>
  * <span data-ttu-id="c54fd-179">変更 (`M`)</span><span class="sxs-lookup"><span data-stu-id="c54fd-179">Modify (`M`)</span></span>
* <span data-ttu-id="c54fd-180">`/t` &ndash; 存在する下位のフォルダーおよびファイルに再帰的に適用します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-180">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="c54fd-181">*c:\\svc* フォルダーに発行されるサンプル アプリと、書き込み/読み取り/実行アクセス許可を持つ `ServiceUser` アカウントに対して、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-181">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="c54fd-182">詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-182">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="manage-the-service"></a><span data-ttu-id="c54fd-183">サービスを管理する</span><span class="sxs-lookup"><span data-stu-id="c54fd-183">Manage the service</span></span>

### <a name="create-the-service"></a><span data-ttu-id="c54fd-184">サービスを作成する</span><span class="sxs-lookup"><span data-stu-id="c54fd-184">Create the service</span></span>

<span data-ttu-id="c54fd-185">[sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用し、サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-185">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="c54fd-186">`binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-186">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="c54fd-187">**等号 (=) と、各パラメーターおよび値の引用符文字の間には、スペースが必要です。**</span><span class="sxs-lookup"><span data-stu-id="c54fd-187">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* <span data-ttu-id="c54fd-188">`{SERVICE NAME}` &ndash; [サービス コントロール マネージャー](/windows/desktop/services/service-control-manager)でサービスに割り当てる名前。</span><span class="sxs-lookup"><span data-stu-id="c54fd-188">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
* <span data-ttu-id="c54fd-189">`{PATH}` &ndash; サービス実行可能ファイルへのパス。</span><span class="sxs-lookup"><span data-stu-id="c54fd-189">`{PATH}` &ndash; The path to the service executable.</span></span>
* <span data-ttu-id="c54fd-190">`{DOMAIN}` &ndash; ドメイン参加済みマシンのドメイン。</span><span class="sxs-lookup"><span data-stu-id="c54fd-190">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="c54fd-191">マシンがドメインに参加していない場合は、ローカル マシン名。</span><span class="sxs-lookup"><span data-stu-id="c54fd-191">If the machine isn't domain-joined, the local machine name.</span></span>
* <span data-ttu-id="c54fd-192">`{USER ACCOUNT}` &ndash; サービスが実行されるユーザー アカウント。</span><span class="sxs-lookup"><span data-stu-id="c54fd-192">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
* <span data-ttu-id="c54fd-193">`{PASSWORD}` &ndash; ユーザー アカウントのパスワード。</span><span class="sxs-lookup"><span data-stu-id="c54fd-193">`{PASSWORD}` &ndash; The user account password.</span></span>

> [!WARNING]
> <span data-ttu-id="c54fd-194">`obj` パラメーターを省略**しない**でください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-194">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="c54fd-195">`obj` の既定値は、[LocalSystem アカウント](/windows/desktop/services/localsystem-account) アカウントです。</span><span class="sxs-lookup"><span data-stu-id="c54fd-195">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="c54fd-196">`LocalSystem` アカウントでサービスを実行すると、重大なセキュリティ リクスが生じます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-196">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="c54fd-197">常に、特権が制限されているユーザー アカウントでサービスを実行します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-197">Always run a service with a user account that has restricted privileges.</span></span>

<span data-ttu-id="c54fd-198">サンプル アプリの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-198">In the following example for the sample app:</span></span>

* <span data-ttu-id="c54fd-199">サービスは **MyService** という名前です。</span><span class="sxs-lookup"><span data-stu-id="c54fd-199">The service is named **MyService**.</span></span>
* <span data-ttu-id="c54fd-200">発行されたサービスは、*c:\\svc* フォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-200">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="c54fd-201">アプリの実行可能ファイルの名前は *SampleApp.exe* です。</span><span class="sxs-lookup"><span data-stu-id="c54fd-201">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="c54fd-202">`binPath` 値を二重引用符 (") で囲みます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-202">Enclose the `binPath` value in double quotation marks (").</span></span>
* <span data-ttu-id="c54fd-203">サービスは `ServiceUser` アカウントで実行されます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-203">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="c54fd-204">`{DOMAIN}` を、ユーザー アカウントのドメインまたはローカル コンピューター名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-204">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="c54fd-205">`obj` 値を二重引用符 (") で囲みます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-205">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="c54fd-206">例: ホスト システムが `MairaPC` という名前のローカル コンピューターである場合は、`obj` を `"MairaPC\ServiceUser"` に設定にします。</span><span class="sxs-lookup"><span data-stu-id="c54fd-206">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
* <span data-ttu-id="c54fd-207">`{PASSWORD}` をユーザー アカウントのパスワードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-207">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="c54fd-208">`password` 値を二重引用符 (") で囲みます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-208">Enclose the `password` value in double quotation marks (").</span></span>

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> <span data-ttu-id="c54fd-209">パラメーターの等号とパラメーターの値の間にスペースがあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-209">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

### <a name="start-the-service"></a><span data-ttu-id="c54fd-210">サービスを開始する</span><span class="sxs-lookup"><span data-stu-id="c54fd-210">Start the service</span></span>

<span data-ttu-id="c54fd-211">サービスを `sc start {SERVICE NAME}` コマンドで開始します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-211">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

<span data-ttu-id="c54fd-212">サンプル アプリ サービスを開始するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-212">To start the sample app service, use the following command:</span></span>

```console
sc start MyService
```

<span data-ttu-id="c54fd-213">このコマンドでサービスを開始するには数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="c54fd-213">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="c54fd-214">サービスの状態を確認する</span><span class="sxs-lookup"><span data-stu-id="c54fd-214">Determine the service status</span></span>

<span data-ttu-id="c54fd-215">サービスの状態を確認するには、`sc query {SERVICE NAME}` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-215">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="c54fd-216">この状態は、次のいずれかの値として報告されます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-216">The status is reported as one of the following values:</span></span>

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

<span data-ttu-id="c54fd-217">次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-217">Use the following command to check the status of the sample app service:</span></span>

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="c54fd-218">Web アプリ サービスを参照する</span><span class="sxs-lookup"><span data-stu-id="c54fd-218">Browse a web app service</span></span>

<span data-ttu-id="c54fd-219">サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000` )。</span><span class="sxs-lookup"><span data-stu-id="c54fd-219">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="c54fd-220">サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-220">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="c54fd-221">サービスを停止して</span><span class="sxs-lookup"><span data-stu-id="c54fd-221">Stop the service</span></span>

<span data-ttu-id="c54fd-222">`sc stop {SERVICE NAME}` コマンドを使用して、サービスを停止します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-222">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

<span data-ttu-id="c54fd-223">サンプル アプリ サービスは、次のコマンドで停止できます。</span><span class="sxs-lookup"><span data-stu-id="c54fd-223">The following command stops the sample app service:</span></span>

```console
sc stop MyService
```

### <a name="delete-the-service"></a><span data-ttu-id="c54fd-224">サービスを削除する</span><span class="sxs-lookup"><span data-stu-id="c54fd-224">Delete the service</span></span>

<span data-ttu-id="c54fd-225">サービスの停止の少し後に、`sc delete {SERVICE NAME}` コマンドを使用して、サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="c54fd-225">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

<span data-ttu-id="c54fd-226">サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-226">Check the status of the sample app service:</span></span>

```console
sc query MyService
```

<span data-ttu-id="c54fd-227">サンプル アプリ サービスの状態が `STOPPED` 状態の場合、次のコマンドを使用して、サンプル アプリ サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="c54fd-227">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="c54fd-228">イベントの開始と停止を扱う</span><span class="sxs-lookup"><span data-stu-id="c54fd-228">Handle starting and stopping events</span></span>

<span data-ttu-id="c54fd-229"><xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>、および <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> イベントを処理するには、次の追加の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="c54fd-229">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="c54fd-230">`OnStarting`、`OnStarted`、および `OnStopping` メソッドを使用して、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-230">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="c54fd-231">`CustomWebHostService` を <xref:System.ServiceProcess.ServiceBase.Run*> に渡す <xref:Microsoft.AspNetCore.Hosting.IWebHost> の拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-231">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="c54fd-232">`Program.Main` で、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> ではなく、拡張メソッド `RunAsCustomService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-232">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="c54fd-233">`Program.Main` 内の <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> の場所を確認するには、「[プロジェクトを Windows サービスに変換する](#convert-a-project-into-a-windows-service)」セクションに示されているコード サンプルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-233">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="c54fd-234">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="c54fd-234">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="c54fd-235">インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="c54fd-235">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="c54fd-236">詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c54fd-236">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="c54fd-237">HTTPS の構成</span><span class="sxs-lookup"><span data-stu-id="c54fd-237">Configure HTTPS</span></span>

<span data-ttu-id="c54fd-238">セキュリティで保護されたエンドポイントを使用してサービスを構成するには:</span><span class="sxs-lookup"><span data-stu-id="c54fd-238">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="c54fd-239">プラットフォームの証明書の取得と展開のメカニズムを使用して、ホスト システム用に X.509 証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-239">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="c54fd-240">[Kestrel サーバーの HTTPS エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)を指定して、証明書を使用します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-240">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="c54fd-241">サービス エンドポイントをセキュリティで保護するために ASP.NET Core の HTTPS 開発証明書を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="c54fd-241">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="c54fd-242">現在のディレクトリとコンテンツのルート</span><span class="sxs-lookup"><span data-stu-id="c54fd-242">Current directory and content root</span></span>

<span data-ttu-id="c54fd-243">Windows サービスに対して <xref:System.IO.Directory.GetCurrentDirectory*> を呼び出して返される現在の作業ディレクトリは *C:\\WINDOWS\\system32* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="c54fd-243">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="c54fd-244">*system32* フォルダーは、サービスのファイル (設定ファイルなど) を保存するために適した場所ではありません。</span><span class="sxs-lookup"><span data-stu-id="c54fd-244">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="c54fd-245">次のいずれかの方法を使用して、サービスのアセットと設定ファイルを管理し、アクセスします。</span><span class="sxs-lookup"><span data-stu-id="c54fd-245">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="c54fd-246">アプリのフォルダーにコンテンツ ルート パスを設定する</span><span class="sxs-lookup"><span data-stu-id="c54fd-246">Set the content root path to the app's folder</span></span>

<span data-ttu-id="c54fd-247"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> は、サービスの作成時に `binPath` 引数に指定されたものと同じパスです。</span><span class="sxs-lookup"><span data-stu-id="c54fd-247">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="c54fd-248">`GetCurrentDirectory` を呼び出して設定ファイルへのパスを作成する代わりに、アプリのコンテンツ ルートへのパスを指定して <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-248">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> with the path to the app's content root.</span></span>

<span data-ttu-id="c54fd-249">`Program.Main` で、サービスの実行可能ファイルがあるフォルダーへのパスを判別し、そのパスを使用してアプリのコンテンツ ルートを確立します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-249">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="c54fd-250">ディスク上の適切な場所にサービスのファイルを格納します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-250">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="c54fd-251">ファイルを含むフォルダーに対して <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> を使用するときは、<xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> を使用して絶対パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="c54fd-251">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c54fd-252">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c54fd-252">Additional resources</span></span>

* <span data-ttu-id="c54fd-253">[Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)</span><span class="sxs-lookup"><span data-stu-id="c54fd-253">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
