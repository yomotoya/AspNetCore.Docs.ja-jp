---
title: "Azure App Service と ASP.NET Core を使用した IIS の一般的なエラーのリファレンス"
author: guardrex
description: "Azure アプリのサービスと IIS の ASP.NET Core アプリケーションをホストしているときに、一般的なエラーを識別します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 58c5ec6bb2603499332698fd4225e2fe636256e4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="e3e09-103">Azure App Service と ASP.NET Core を使用した IIS の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="e3e09-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="e3e09-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e3e09-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e3e09-105">次の一覧に含まれていないエラーもあります。</span><span class="sxs-lookup"><span data-stu-id="e3e09-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="e3e09-106">ここでは、表示されていないエラーが発生した場合[新しい懸案事項を開く](https://github.com/aspnet/Docs/issues/new)エラーを再現する詳細な指示を伴うです。</span><span class="sxs-lookup"><span data-stu-id="e3e09-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="e3e09-107">インストーラーが VC++ 再頒布可能パッケージを取得できない</span><span class="sxs-lookup"><span data-stu-id="e3e09-107">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="e3e09-108">**インストーラー例外:** 0x80072efd または 0x80072f76 - 特定できないエラー</span><span class="sxs-lookup"><span data-stu-id="e3e09-108">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="e3e09-109">**インストーラー ログ例外&#8224;:** エラー 0x80072efd または 0x80072f76: Failed to execute EXE package (EXE パッケージを実行できませんでした)</span><span class="sxs-lookup"><span data-stu-id="e3e09-109">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="e3e09-110">&#8224;ログの場所は C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-110">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="e3e09-111">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-111">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-112">サーバー ホスティング バンドルのインストール時にインストーラーがインターネットにアクセスできない場合、インストーラーは *Microsoft Visual C++ 2015 再頒布可能パッケージ*を取得できず、この例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-112">If the system doesn't have Internet access while installing the server hosting bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="e3e09-113">インストーラーを取得、 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-113">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="e3e09-114">インストーラーが失敗した場合、サーバーがフレームワークに依存する展開 (FDD) をホストするために、.NET Core ランタイムを受信しません。</span><span class="sxs-lookup"><span data-stu-id="e3e09-114">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="e3e09-115">場合は、FDD をホストするには、プログラムで、ランタイムがインストールされていることを確認&amp;機能します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-115">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="e3e09-116">必要な場合からランタイム インストーラーを入手[.NET ダウンロード](https://www.microsoft.com/net/download/core)です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-116">If needed, obtain a runtime installer from [.NET Downloads](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="e3e09-117">ランタイムのインストール後、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-117">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="e3e09-118">OS のアップグレードによって 32 ビット ASP.NET Core モジュールが削除された</span><span class="sxs-lookup"><span data-stu-id="e3e09-118">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="e3e09-119">**アプリケーション ログ:** モジュール DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** を読み込めませんでした。</span><span class="sxs-lookup"><span data-stu-id="e3e09-119">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="e3e09-120">このデータはエラーです。</span><span class="sxs-lookup"><span data-stu-id="e3e09-120">The data is the error.</span></span>

<span data-ttu-id="e3e09-121">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-121">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-122">**C:\Windows\SysWOW64\inetsrv** ディレクトリにある OS ファイルでないファイルは、OS アップグレード時に保持されません。</span><span class="sxs-lookup"><span data-stu-id="e3e09-122">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="e3e09-123">前に、ASP.NET Core モジュールがインストールされている場合、OS のアップグレードと任意の AppPool し、実行 32 ビット モードで OS のアップグレード後に、この問題が発生しました。</span><span class="sxs-lookup"><span data-stu-id="e3e09-123">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="e3e09-124">OS アップグレード後に ASP.NET Core モジュールを修復してください。</span><span class="sxs-lookup"><span data-stu-id="e3e09-124">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="e3e09-125">「[.NET Core Windows Server ホスティング バンドルのインストール](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e3e09-125">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="e3e09-126">選択**修復**インストーラーを実行するとします。</span><span class="sxs-lookup"><span data-stu-id="e3e09-126">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="e3e09-127">プラットフォームが RID と競合している</span><span class="sxs-lookup"><span data-stu-id="e3e09-127">Platform conflicts with RID</span></span>

* <span data-ttu-id="e3e09-128">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e3e09-128">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e3e09-129">**アプリケーション ログ:**アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\{パス}\' commandline でプロセスを開始できませんでした '"c:\\{パス} {アセンブリ} です {。exe | dll}"'、エラー コード = ' 0x80004005: ff します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-129">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="e3e09-130">**ASP.NET Core モジュールのログ:**未処理の例外: System.BadImageFormatException: を読み込めませんでしたファイルまたはアセンブリ '{アセンブリ} .dll' です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-130">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="e3e09-131">正しくない形式のプログラムを読み込もうとしました。</span><span class="sxs-lookup"><span data-stu-id="e3e09-131">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="e3e09-132">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-132">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-133">Kestrel でアプリをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-133">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e3e09-134">プロセスのエラーは、アプリ内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e3e09-134">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e3e09-135">詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-135">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e3e09-136">いることを確認、`<PlatformTarget>`で、 *.csproj* RID には抵触しません。</span><span class="sxs-lookup"><span data-stu-id="e3e09-136">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="e3e09-137">などを指定しない、`<PlatformTarget>`の`x86`と、RID のパブリッシュ`win10-x64`、いずれかを使用して*dotnet 発行-c リリース-r win10 x64*かを設定して、`<RuntimeIdentifiers>`で、 *.csproj*に`win10-x64`です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-137">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="e3e09-138">プロジェクトは警告やエラーにならずに発行されますが、上記のログに記録されたシステムの例外によって失敗します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-138">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="e3e09-139">この例外は、アプリをアップグレードするときに Azure のアプリ展開の場合に発生し、以前の展開からすべてのファイルを手動で削除、新しいアセンブリを展開する場合。</span><span class="sxs-lookup"><span data-stu-id="e3e09-139">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="e3e09-140">アップグレードしたアプリを展開するとき、互換性のないアセンブリが残っていると、`System.BadImageFormatException` 例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-140">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="e3e09-141">URI のエンドポイントが間違っているか、Web サイトが停止している</span><span class="sxs-lookup"><span data-stu-id="e3e09-141">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="e3e09-142">**ブラウザー:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="e3e09-142">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="e3e09-143">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e3e09-143">**Application Log:** No entry</span></span>

* <span data-ttu-id="e3e09-144">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="e3e09-144">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e3e09-145">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-145">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-146">アプリ用の正しい URI エンドポイントが使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-146">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="e3e09-147">バインドを確認してください。</span><span class="sxs-lookup"><span data-stu-id="e3e09-147">Check the bindings.</span></span>

* <span data-ttu-id="e3e09-148">IIS web サイトがないことを確認、 *Stopped*状態です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-148">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="e3e09-149">CoreWebEngine または W3SVC サーバー機能が無効</span><span class="sxs-lookup"><span data-stu-id="e3e09-149">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="e3e09-150">**OS の例外:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module. (ASP.NET Core モジュールを使用するには、IIS 7.0 CoreWebEngine および W3SVC の機能をインストールする必要があります。)</span><span class="sxs-lookup"><span data-stu-id="e3e09-150">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="e3e09-151">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-151">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-152">適切な役割と機能が有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-152">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="e3e09-153">「[IIS 構成](xref:host-and-deploy/iis/index#iis-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e3e09-153">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="e3e09-154">不適切な web サイトの物理パス、またはアプリがありません。</span><span class="sxs-lookup"><span data-stu-id="e3e09-154">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="e3e09-155">**ブラウザー:** 403 許可されていません - アクセスが拒否されました **--または--** 403.14 許可されていません - Web サーバーは、このディレクトリの内容の一覧を表示しないように構成されています。</span><span class="sxs-lookup"><span data-stu-id="e3e09-155">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="e3e09-156">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e3e09-156">**Application Log:** No entry</span></span>

* <span data-ttu-id="e3e09-157">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="e3e09-157">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e3e09-158">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-158">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-159">IIS web サイトを確認**基本設定**と物理アプリ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e3e09-159">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="e3e09-160">アプリが、IIS web サイトにあるフォルダーにあることを確認**物理パス**です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-160">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="e3e09-161">役割が正しくない、モジュールがインストールされていない、または不適切なアクセス許可</span><span class="sxs-lookup"><span data-stu-id="e3e09-161">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="e3e09-162">**ブラウザー:** 500.19 内部サーバー エラー - ページに関連する構成データが無効であるため、要求されたページにアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="e3e09-162">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="e3e09-163">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e3e09-163">**Application Log:** No entry</span></span>

* <span data-ttu-id="e3e09-164">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="e3e09-164">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e3e09-165">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-165">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-166">適切なロールが有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-166">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="e3e09-167">「[IIS 構成](xref:host-and-deploy/iis/index#iis-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e3e09-167">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="e3e09-168">**[プログラムと機能]** をチェックし、**Microsoft ASP.NET Core モジュール**がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-168">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="e3e09-169">インストールされているプログラムの一覧に **Microsoft ASP.NET Core モジュール**が表示されない場合は、モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e3e09-169">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="e3e09-170">「[.NET Core Windows Server ホスティング バンドルのインストール](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e3e09-170">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="e3e09-171">確認して、**アプリケーション プール** > **プロセス モデル** > **Identity**に設定されている**ApplicationPoolIdentity**またはカスタム id は、アプリの配置フォルダーにアクセスする適切なアクセス許可を持っています。</span><span class="sxs-lookup"><span data-stu-id="e3e09-171">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="e3e09-172">processPath の誤り、PATH 変数の欠如、ホスティング バンドルが未インストール、システムまたは IIS が再起動されていない、VC++ 再頒布可能パッケージが未インストール、dotnet.exe アクセス違反</span><span class="sxs-lookup"><span data-stu-id="e3e09-172">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="e3e09-173">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e3e09-173">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e3e09-174">**アプリケーションのログ:**アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを開始できませんでした '".\{アセンブリ} .exe"'、エラー コード = ' 0x80070002: 0。</span><span class="sxs-lookup"><span data-stu-id="e3e09-174">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="e3e09-175">**ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です</span><span class="sxs-lookup"><span data-stu-id="e3e09-175">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="e3e09-176">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-176">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-177">Kestrel でアプリをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-177">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e3e09-178">プロセスのエラーは、アプリ内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e3e09-178">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e3e09-179">詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-179">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e3e09-180">チェック、 *processPath*属性を`<aspNetCore>`内の要素*web.config*であることを確認する*dotnet*フレームワークに依存する展開 (FDD) または*.\{アセンブリ} .exe*自己完結型の展開 (SCD)。</span><span class="sxs-lookup"><span data-stu-id="e3e09-180">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="e3e09-181">FDD の場合、PATH 設定で *dotnet.exe* にアクセスできていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e3e09-181">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="e3e09-182">*C:\Program Files\dotnet\* がシステムの PATH 設定に含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-182">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="e3e09-183">FDD では、アプリケーション プールのユーザー ID で *dotnet.exe* にアクセスできていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e3e09-183">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="e3e09-184">AppPool ユーザー ID に、*C:\Program Files\dotnet* ディレクトリへのアクセス許可が設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-184">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="e3e09-185">上の AppPool ユーザー id に対して構成された拒否ルールがないことを確認、 *C:\Program Files\dotnet*とアプリのディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="e3e09-185">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="e3e09-186">FDD が配置されているし、IIS を再起動しなくても .NET Core がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="e3e09-186">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="e3e09-187">サーバーを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-187">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="e3e09-188">ホスト システムに、.NET Core ランタイムをインストールしなくても、FDD が展開された可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e3e09-188">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="e3e09-189">.NET Core ランタイムがインストールされていない場合は、実行、 **.NET コア Windows Server をホストしているバンドル インストーラー**システムにします。</span><span class="sxs-lookup"><span data-stu-id="e3e09-189">If the .NET Core runtime hasn't been installed, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="e3e09-190">「[.NET Core Windows Server ホスティング バンドルのインストール](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e3e09-190">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="e3e09-191">場合は、インターネット接続のないシステムで、.NET Core ランタイムをインストールしようとすると、取得、ランタイムから[.NET ダウンロード](https://www.microsoft.com/net/download/core)ASP.NET Core モジュールをインストールするホスティング バンドル インストーラーを実行しています。</span><span class="sxs-lookup"><span data-stu-id="e3e09-191">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET Downloads](https://www.microsoft.com/net/download/core) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="e3e09-192">インストールを完了するために、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-192">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="e3e09-193">FDD が配置されていると、 *Microsoft Visual C 2015 Redistributable (x64)*システムがインストールされていません。</span><span class="sxs-lookup"><span data-stu-id="e3e09-193">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="e3e09-194">インストーラーを取得、 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-194">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="e3e09-195">\<aspNetCore\> 要素の引数の誤り</span><span class="sxs-lookup"><span data-stu-id="e3e09-195">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="e3e09-196">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e3e09-196">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e3e09-197">**アプリケーションのログ:**アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを開始できませんでした '"dotnet".\{アセンブリ} .dll '、エラー コード = ' 0x80004005: 80008081 です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-197">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="e3e09-198">**ASP.NET Core モジュールのログ:**を実行するアプリケーションは存在しません: ' パス\{アセンブリ} .dll '</span><span class="sxs-lookup"><span data-stu-id="e3e09-198">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="e3e09-199">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-199">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-200">Kestrel でアプリをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-200">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e3e09-201">プロセスのエラーは、アプリ内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e3e09-201">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e3e09-202">詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-202">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e3e09-203">確認、*引数*属性を`<aspNetCore>`内の要素*web.config*したことを確認するかを (a) *.\{アセンブリ} .dll*のフレームワークに依存する展開 (FDD); か、(b) ではない、空の文字列 (*引数 =""*)、またはアプリの引数の一覧 (*引数"arg1 arg2,..."を =*)自己完結型の展開 (SCD)。</span><span class="sxs-lookup"><span data-stu-id="e3e09-203">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="e3e09-204">.NET Framework バージョンの欠落</span><span class="sxs-lookup"><span data-stu-id="e3e09-204">Missing .NET Framework version</span></span>

* <span data-ttu-id="e3e09-205">**ブラウザー:** 502.3 ゲートウェイが正しくありません - There was a connection error while trying to route the request. (要求のルーティング中に接続エラーが発生しました。)</span><span class="sxs-lookup"><span data-stu-id="e3e09-205">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="e3e09-206">**アプリケーションのログ:** ErrorCode アプリケーション = 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを開始できませんでした '"dotnet".\{アセンブリ} .dll '、エラー コード = ' 0x80004005: 80008081 です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-206">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="e3e09-207">**ASP.NET Core モジュールのログ:** メソッド、ファイル、またはアセンブリの欠如の例外。</span><span class="sxs-lookup"><span data-stu-id="e3e09-207">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="e3e09-208">例外で特定されたメソッド、ファイル、またはアセンブリは、.NET Framework のメソッド、ファイル、またはアセンブリです。</span><span class="sxs-lookup"><span data-stu-id="e3e09-208">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="e3e09-209">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e3e09-209">Troubleshooting:</span></span>

* <span data-ttu-id="e3e09-210">システムにない .NET Framework のバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e3e09-210">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="e3e09-211">フレームワークに依存する展開 (FDD) は、適切なランタイムがシステムにインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-211">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="e3e09-212">2.0 では、ホストのシステムに配置する 1.1 からプロジェクトをアップグレードすると、この例外により、2.0 framework がホスト システム上にあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-212">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="e3e09-213">アプリケーション プールの停止</span><span class="sxs-lookup"><span data-stu-id="e3e09-213">Stopped Application Pool</span></span>

* <span data-ttu-id="e3e09-214">**ブラウザー:** 503 サービスを利用できません</span><span class="sxs-lookup"><span data-stu-id="e3e09-214">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="e3e09-215">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e3e09-215">**Application Log:** No entry</span></span>

* <span data-ttu-id="e3e09-216">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="e3e09-216">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e3e09-217">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e3e09-217">Troubleshooting</span></span>

* <span data-ttu-id="e3e09-218">アプリケーション プールされていないことを確認、 *Stopped*状態です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-218">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="e3e09-219">IIS 統合ミドルウェアが未実装</span><span class="sxs-lookup"><span data-stu-id="e3e09-219">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="e3e09-220">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e3e09-220">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e3e09-221">**アプリケーションのログ:**アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを作成 '"c:\\{PATH}\{アセンブリ} です {。exe | dll}"' がクラッシュまたは応答がありませんでしたまたは指定されたポート {ポート}、ErrorCode でリッスンできませんでした '0x800705b4' を =</span><span class="sxs-lookup"><span data-stu-id="e3e09-221">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="e3e09-222">**ASP.NET Core モジュールのログ:** ログ ファイルが作成され、正常動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="e3e09-222">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="e3e09-223">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e3e09-223">Troubleshooting</span></span>

* <span data-ttu-id="e3e09-224">Kestrel でアプリをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-224">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e3e09-225">プロセスのエラーは、アプリ内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e3e09-225">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e3e09-226">詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-226">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e3e09-227">か、いることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-227">Confirm that either:</span></span>
  * <span data-ttu-id="e3e09-228">IIS 統合ミドルウェアが参照する呼び出し、`UseIISIntegration`メソッドをアプリの`WebHostBuilder`(ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="e3e09-228">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="e3e09-229">アプリの使用、`CreateDefaultBuilder`メソッド (ASP.NET Core 2.x)。</span><span class="sxs-lookup"><span data-stu-id="e3e09-229">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="e3e09-230">詳細については、「[ASP.NET Core でのホスティング](xref:fundamentals/hosting)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e3e09-230">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="e3e09-231">サブアプリケーションに \<handlers\> セクションが含まれている</span><span class="sxs-lookup"><span data-stu-id="e3e09-231">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="e3e09-232">**ブラウザー:** HTTP エラー 500.19 - 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="e3e09-232">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="e3e09-233">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e3e09-233">**Application Log:** No entry</span></span>

* <span data-ttu-id="e3e09-234">**ASP.NET Core モジュールのログ:**ログ ファイルが作成され、ルート アプリケーションの通常の操作を示します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-234">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="e3e09-235">ログ ファイルが sub アプリ用に作成されません。</span><span class="sxs-lookup"><span data-stu-id="e3e09-235">Log file not created for the sub-app.</span></span>

<span data-ttu-id="e3e09-236">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e3e09-236">Troubleshooting</span></span>

* <span data-ttu-id="e3e09-237">サブアプリの *web.config* ファイルに `<handlers>` セクションが含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-237">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="e3e09-238">アプリケーション構成の一般的な問題</span><span class="sxs-lookup"><span data-stu-id="e3e09-238">Application configuration general issue</span></span>

* <span data-ttu-id="e3e09-239">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e3e09-239">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e3e09-240">**アプリケーションのログ:**アプリケーション 'APPHOST WEBROOT/コンピューター//{アセンブリ}' と物理ルート' c:\\{PATH}\' commandline でプロセスを作成 '"c:\\{PATH}\{アセンブリ} です {。exe | dll}"' がクラッシュまたは応答がありませんでしたまたは指定されたポート {ポート}、ErrorCode でリッスンできませんでした '0x800705b4' を =</span><span class="sxs-lookup"><span data-stu-id="e3e09-240">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="e3e09-241">**ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です</span><span class="sxs-lookup"><span data-stu-id="e3e09-241">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="e3e09-242">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e3e09-242">Troubleshooting</span></span>

* <span data-ttu-id="e3e09-243">この一般的な例外は、開始するほとんどの場合、アプリの構成の問題のために、処理が失敗したことを示します。</span><span class="sxs-lookup"><span data-stu-id="e3e09-243">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="e3e09-244">参照する[ディレクトリ構造](xref:host-and-deploy/directory-structure)アプリの構成ファイルが存在して、アプリの配置ファイルとフォルダーが適切であることを確認し、アプリおよび環境の正しい設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e3e09-244">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="e3e09-245">詳細については、次を参照してください。[トラブルシューティング](xref:host-and-deploy/iis/troubleshoot)です。</span><span class="sxs-lookup"><span data-stu-id="e3e09-245">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
