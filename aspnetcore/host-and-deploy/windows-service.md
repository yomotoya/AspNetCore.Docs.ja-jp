---
title: Windows サービスでの ASP.NET Core のホスト
author: guardrex
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758194"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="ff812-103">Windows サービスでの ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="ff812-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="ff812-104">著者: [Luke Latham](https://github.com/guardrex)、[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ff812-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ff812-105">ASP.NET Core アプリは、IIS を [Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications) として使用せずに、Windows にホストできます。</span><span class="sxs-lookup"><span data-stu-id="ff812-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="ff812-106">Windows サービスとしてホストされている場合、再起動後にアプリが自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="ff812-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="ff812-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ff812-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="ff812-108">プロジェクトを Windows サービスに変換する</span><span class="sxs-lookup"><span data-stu-id="ff812-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="ff812-109">サービスでとして実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ff812-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="ff812-110">プロジェクト ファイルで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="ff812-110">In the project file:</span></span>

   * <span data-ttu-id="ff812-111">Windows [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) があることを確認するか、それをターゲット フレームワークを含む `<PropertyGroup>` に追加します。</span><span class="sxs-lookup"><span data-stu-id="ff812-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="ff812-112">複数の RID を発行するには、次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="ff812-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="ff812-113">セミコロンで区切られたリストの形式で RID を指定します。</span><span class="sxs-lookup"><span data-stu-id="ff812-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="ff812-114">プロパティ名 `<RuntimeIdentifiers>` (複数形) を使用します。</span><span class="sxs-lookup"><span data-stu-id="ff812-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="ff812-115">詳細については、「[.NET Core の RID カタログ](/dotnet/core/rid-catalog)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ff812-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="ff812-116">[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) のパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="ff812-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="ff812-117">`Program.Main` で次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="ff812-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="ff812-118">`host.Run` ではなく、[host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ff812-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="ff812-119">[UseContentRoot](xref:fundamentals/host/web-host#content-root) を呼び出し、`Directory.GetCurrentDirectory()` の代わりにアプリの発行場所のパスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ff812-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="ff812-120">[dotnet publish](/dotnet/articles/core/tools/dotnet-publish)、[Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)、または Visual Studio Code を使用してアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="ff812-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="ff812-121">Visual Studio を使用する場合、**[発行]** ボタンを選択する前に、**[FolderProfile]** を選択して **[ターゲットの場所]** を構成します。</span><span class="sxs-lookup"><span data-stu-id="ff812-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="ff812-122">コマンド ライン インターフェイス (CLI) ツールを使用してサンプル アプリを発行するには、プロジェクト フォルダーからコマンド プロンプトで [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ff812-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="ff812-123">プロジェクト ファイルの `<RuntimeIdenfifier>` (または `<RuntimeIdentifiers>`) プロパティに RID を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ff812-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="ff812-124">次の例では、アプリが `win7-x64` ランタイムのリリース構成で、*c:\\svc* に作成されるフォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="ff812-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="ff812-125">`net user` コマンドを使用して、サービス用のユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ff812-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="ff812-126">サンプル アプリでは、名前 `ServiceUser` とパスワードを持つユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ff812-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="ff812-127">次のコマンド内の `{PASSWORD}` を、[強力なパスワード](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ff812-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="ff812-128">ユーザーをグループに追加する必要がある場合は、`net localgroup` コマンドを使用します。`{GROUP}` にはグループの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="ff812-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="ff812-129">詳細については、「[Service User Accounts](/windows/desktop/services/service-user-accounts)」(サービス ユーザー アカウント) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ff812-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="ff812-130">[icacls](/windows-server/administration/windows-commands/icacls) コマンドを使用して、アプリのフォルダーに書き込み/読み取り/実行アクセス許可を与えます。</span><span class="sxs-lookup"><span data-stu-id="ff812-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="ff812-131">`{PATH}` &ndash; アプリのフォルダーへのパス。</span><span class="sxs-lookup"><span data-stu-id="ff812-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="ff812-132">`{USER ACCOUNT}` &ndash; ユーザー アカウント (SID)。</span><span class="sxs-lookup"><span data-stu-id="ff812-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="ff812-133">`(OI)` &ndash; オブジェクトの継承フラグ。下位のファイルにアクセス許可を反映します。</span><span class="sxs-lookup"><span data-stu-id="ff812-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="ff812-134">`(CI)` &ndash; コンテナーの継承フラグ。下位のフォルダーにアクセス許可を反映します。</span><span class="sxs-lookup"><span data-stu-id="ff812-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="ff812-135">`{PERMISSION FLAGS}` &ndash; アプリのアクセス許可を設定します。</span><span class="sxs-lookup"><span data-stu-id="ff812-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="ff812-136">書き込み (`W`)</span><span class="sxs-lookup"><span data-stu-id="ff812-136">Write (`W`)</span></span>
     * <span data-ttu-id="ff812-137">読み取り (`R`)</span><span class="sxs-lookup"><span data-stu-id="ff812-137">Read (`R`)</span></span>
     * <span data-ttu-id="ff812-138">実行 (`X`)</span><span class="sxs-lookup"><span data-stu-id="ff812-138">Execute (`X`)</span></span>
     * <span data-ttu-id="ff812-139">フル アクセス (`F`)</span><span class="sxs-lookup"><span data-stu-id="ff812-139">Full (`F`)</span></span>
     * <span data-ttu-id="ff812-140">変更 (`M`)</span><span class="sxs-lookup"><span data-stu-id="ff812-140">Modify (`M`)</span></span>
   * <span data-ttu-id="ff812-141">`/t` &ndash; 存在する下位のフォルダーおよびファイルに再帰的に適用します。</span><span class="sxs-lookup"><span data-stu-id="ff812-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="ff812-142">*c:\\svc* フォルダーに発行されるサンプル アプリと、書き込み/読み取り/実行アクセス許可を持つ `ServiceUser` アカウントに対して、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="ff812-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="ff812-143">詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ff812-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="ff812-144">[sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用し、サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ff812-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="ff812-145">`binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ff812-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="ff812-146">**等号 (=) と、各パラメーターおよび値の引用符文字の間には、スペースが必要です。**</span><span class="sxs-lookup"><span data-stu-id="ff812-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="ff812-147">`{SERVICE NAME}` &ndash; [サービス コントロール マネージャー](/windows/desktop/services/service-control-manager)でサービスに割り当てる名前。</span><span class="sxs-lookup"><span data-stu-id="ff812-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="ff812-148">`{PATH}` &ndash; サービス実行可能ファイルへのパス。</span><span class="sxs-lookup"><span data-stu-id="ff812-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="ff812-149">`{DOMAIN}` (コンピューターがドメインに参加していない場合は、ローカル コンピューター名) および `{USER ACCOUNT}` &ndash; サービスを実行させるドメイン (またはローカル コンピューター名) とユーザー アカウント。</span><span class="sxs-lookup"><span data-stu-id="ff812-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="ff812-150">`obj` パラメーターを省略**しない**でください。</span><span class="sxs-lookup"><span data-stu-id="ff812-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="ff812-151">`obj` の既定値は、[LocalSystem アカウント](/windows/desktop/services/localsystem-account) アカウントです。</span><span class="sxs-lookup"><span data-stu-id="ff812-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="ff812-152">`LocalSystem` アカウントでサービスを実行すると、重大なセキュリティ リクスが生じます。</span><span class="sxs-lookup"><span data-stu-id="ff812-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="ff812-153">常に、サーバーに対する制限付きの特権を持つユーザー アカウントでサービスを実行します。</span><span class="sxs-lookup"><span data-stu-id="ff812-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="ff812-154">`{PASSWORD}` &ndash; ユーザー アカウントのパスワード。</span><span class="sxs-lookup"><span data-stu-id="ff812-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="ff812-155">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ff812-155">In the following example:</span></span>

   * <span data-ttu-id="ff812-156">サービスは **MyService** という名前です。</span><span class="sxs-lookup"><span data-stu-id="ff812-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="ff812-157">発行されたサービスは、*c:\\svc* フォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="ff812-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="ff812-158">*AspNetCoreService.exe* という名前のアプリの実行可能ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="ff812-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="ff812-159">`binPath` 値は二重引用符 (") で囲まれます。</span><span class="sxs-lookup"><span data-stu-id="ff812-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="ff812-160">サービスは `ServiceUser` アカウントで実行されます。</span><span class="sxs-lookup"><span data-stu-id="ff812-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="ff812-161">`{DOMAIN}` を、ユーザー アカウントのドメインまたはローカル コンピューター名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ff812-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="ff812-162">`obj` 値を二重引用符 (") 囲みます。</span><span class="sxs-lookup"><span data-stu-id="ff812-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="ff812-163">例: ホスト システムが `MairaPC` という名前のローカル コンピューターである場合は、`obj` を `"MairaPC\ServiceUser"` に設定にします。</span><span class="sxs-lookup"><span data-stu-id="ff812-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="ff812-164">`{PASSWORD}` をユーザー アカウントのパスワードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ff812-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="ff812-165">`password` 値は二重引用符 (") で囲まれます。</span><span class="sxs-lookup"><span data-stu-id="ff812-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="ff812-166">パラメーターの等号とパラメーターの値の間にスペースがあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ff812-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="ff812-167">サービスを `sc start {SERVICE NAME}` コマンドで開始します。</span><span class="sxs-lookup"><span data-stu-id="ff812-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="ff812-168">サンプル アプリ サービスを開始するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="ff812-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="ff812-169">このコマンドでサービスを開始するには数秒かかります。</span><span class="sxs-lookup"><span data-stu-id="ff812-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="ff812-170">サービスの状態を確認するには、`sc query {SERVICE NAME}` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="ff812-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="ff812-171">この状態は、次のいずれかの値として報告されます。</span><span class="sxs-lookup"><span data-stu-id="ff812-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="ff812-172">次のコマンドを使用し、サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="ff812-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="ff812-173">サービスの状態が `RUNNING` で、サービスが Web アプリである場合、そのアプリとそのパスを参照します (既定では、[HTTPS Redirection Middleware](xref:security/enforcing-ssl) の使用時に `https://localhost:5001` にリダイレクトされる `http://localhost:5000`)。</span><span class="sxs-lookup"><span data-stu-id="ff812-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="ff812-174">サンプル アプリ サービスの場合、アプリは `http://localhost:5000` で参照します。</span><span class="sxs-lookup"><span data-stu-id="ff812-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="ff812-175">`sc stop {SERVICE NAME}` コマンドを使用して、サービスを停止します。</span><span class="sxs-lookup"><span data-stu-id="ff812-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="ff812-176">サンプル アプリ サービスは、次のコマンドで停止できます。</span><span class="sxs-lookup"><span data-stu-id="ff812-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="ff812-177">サービスの停止の少し後に、`sc delete {SERVICE NAME}` コマンドを使用して、サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="ff812-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="ff812-178">サンプル アプリ サービスの状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="ff812-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="ff812-179">サンプル アプリ サービスの状態が `STOPPED` 状態の場合、次のコマンドを使用して、サンプル アプリ サービスをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="ff812-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="ff812-180">サービスの外部でアプリを実行する</span><span class="sxs-lookup"><span data-stu-id="ff812-180">Run the app outside of a service</span></span>

<span data-ttu-id="ff812-181">サービスの外部で実行する場合のほうがテストおよびデバッグは簡単です。このため、特定の条件下でのみ `RunAsService` を呼び出すコードを追加することが一般的です。</span><span class="sxs-lookup"><span data-stu-id="ff812-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="ff812-182">たとえば、`--console` コマンドライン引数を使用してアプリをコンソール アプリとしてアプリを実行できます。または、デバッガーがアタッチされている場合は、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="ff812-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="ff812-183">ASP.NET Core の構成では、コマンドライン引数に名前と値の組みが必要であるため、引数が [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) に渡される前に、`--console` スイッチは削除されます。</span><span class="sxs-lookup"><span data-stu-id="ff812-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="ff812-184">[統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。</span><span class="sxs-lookup"><span data-stu-id="ff812-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="ff812-185">停止および開始イベントの処理</span><span class="sxs-lookup"><span data-stu-id="ff812-185">Handle stopping and starting events</span></span>

<span data-ttu-id="ff812-186">[OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)、[OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) イベントを処理する場合、追加で次の変更も行います。</span><span class="sxs-lookup"><span data-stu-id="ff812-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="ff812-187">[WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) から派生するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ff812-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="ff812-188">カスタム `WebHostService` を [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) に渡す [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) の拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="ff812-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="ff812-189">`Program.Main` で、[RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) ではなく、新しい拡張メソッド `RunAsCustomService` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ff812-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="ff812-190">[統合テスト](xref:test/integration-tests)が正しく動作するには `CreateWebHostBuilder(string[])` に `CreateWebHostBuilder` の署名が必要なため、`isService` は `Main` から `CreateWebHostBuilder` に渡されません。</span><span class="sxs-lookup"><span data-stu-id="ff812-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="ff812-191">カスタム `WebHostService` コードに依存関係の挿入からのサービスが必要な場合は (ロガーなど)、[IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) プロパティからそれを取得します。</span><span class="sxs-lookup"><span data-stu-id="ff812-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ff812-192">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="ff812-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ff812-193">インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ff812-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="ff812-194">詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ff812-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="ff812-195">HTTPS の構成</span><span class="sxs-lookup"><span data-stu-id="ff812-195">Configure HTTPS</span></span>

<span data-ttu-id="ff812-196">セキュリティで保護されたエンドポイントを使用してサービスを構成するには:</span><span class="sxs-lookup"><span data-stu-id="ff812-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="ff812-197">プラットフォームの証明書の取得と展開のメカニズムを使用して、ホスト システム用に X.509 証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="ff812-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="ff812-198">[Kestrel サーバーの HTTPS エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)を指定して、証明書を使用します。</span><span class="sxs-lookup"><span data-stu-id="ff812-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="ff812-199">サービス エンドポイントをセキュリティで保護するために ASP.NET Core の HTTPS 開発証明書を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="ff812-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="ff812-200">現在のディレクトリとコンテンツのルート</span><span class="sxs-lookup"><span data-stu-id="ff812-200">Current directory and content root</span></span>

<span data-ttu-id="ff812-201">Windows サービスに対して `Directory.GetCurrentDirectory()` を呼び出して返される現在の作業ディレクトリは *C:\\WINDOWS\\system32* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ff812-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="ff812-202">*system32* フォルダーは、サービスのファイル (設定ファイルなど) を保存するために適した場所ではありません。</span><span class="sxs-lookup"><span data-stu-id="ff812-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="ff812-203">[IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) を使用する場合、次のいずれかの方法を使用して、サービスの資産と設定ファイルを [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) に格納し、アクセスします。</span><span class="sxs-lookup"><span data-stu-id="ff812-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="ff812-204">コンテンツのルート パスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ff812-204">Use the content root path.</span></span> <span data-ttu-id="ff812-205">`IHostingEnvironment.ContentRootPath` は、サービスの作成時に `binPath` 引数に指定されたものと同じパスです。</span><span class="sxs-lookup"><span data-stu-id="ff812-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="ff812-206">`Directory.GetCurrentDirectory()` を使用して設定ファイルのパスを作成する代わりに、コンテンツのルートパスを使用して、アプリのコンテンツ ルートにファイルを格納します。</span><span class="sxs-lookup"><span data-stu-id="ff812-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="ff812-207">ディスク上の適切な場所にファイルを格納します。</span><span class="sxs-lookup"><span data-stu-id="ff812-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="ff812-208">ファイルを含むフォルダーを示す絶対パスを `SetBasePath` で指定します。</span><span class="sxs-lookup"><span data-stu-id="ff812-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff812-209">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ff812-209">Additional resources</span></span>

* <span data-ttu-id="ff812-210">[Kestrel エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS 構成と SNI サポートを含む)</span><span class="sxs-lookup"><span data-stu-id="ff812-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
