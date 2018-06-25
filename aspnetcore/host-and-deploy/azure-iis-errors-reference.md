---
title: ASP.NET Core を使用した Azure App Service および IIS の一般的なエラーのリファレンス
author: guardrex
description: Azure Apps Service と IIS で ASP.NET Core アプリをホストする場合の一般的なエラーを区別します。
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 30b7f1d8e1cfdfd3d1db865ff428eb2094a84d32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277320"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="11ef2-103">ASP.NET Core を使用した Azure App Service および IIS の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="11ef2-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="11ef2-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="11ef2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="11ef2-105">次の一覧に含まれていないエラーもあります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="11ef2-106">ここにリストされていないエラーが発生した場合は、エラーを再現するための詳しい手順と共に[新しい問題を開いてください](https://github.com/aspnet/Docs/issues/new)。</span><span class="sxs-lookup"><span data-stu-id="11ef2-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

<span data-ttu-id="11ef2-107">次の情報を収集します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-107">Collect the following information:</span></span>

* <span data-ttu-id="11ef2-108">ブラウザーの動作</span><span class="sxs-lookup"><span data-stu-id="11ef2-108">Browser behavior</span></span>
* <span data-ttu-id="11ef2-109">アプリケーション イベント ログのエントリ</span><span class="sxs-lookup"><span data-stu-id="11ef2-109">Application Event Log entries</span></span>
* <span data-ttu-id="11ef2-110">ASP.NET Core モジュールの stdout ログのエントリ</span><span class="sxs-lookup"><span data-stu-id="11ef2-110">ASP.NET Core Module stdout log entries</span></span>

<span data-ttu-id="11ef2-111">情報を、次の一般的なエラーと比較します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-111">Compare the information to the following common errors.</span></span> <span data-ttu-id="11ef2-112">一致が見つかった場合は、トラブルシューティングのアドバイスに従います。</span><span class="sxs-lookup"><span data-stu-id="11ef2-112">If a match is found, follow the troubleshooting advice.</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="11ef2-113">インストーラーが VC++ 再頒布可能パッケージを取得できない</span><span class="sxs-lookup"><span data-stu-id="11ef2-113">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="11ef2-114">**インストーラー例外:** 0x80072efd または 0x80072f76 - 特定できないエラー</span><span class="sxs-lookup"><span data-stu-id="11ef2-114">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="11ef2-115">**インストーラー ログ例外&#8224;:** エラー 0x80072efd または 0x80072f76: Failed to execute EXE package (EXE パッケージを実行できませんでした)</span><span class="sxs-lookup"><span data-stu-id="11ef2-115">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="11ef2-116">&#8224;ログの場所は C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log です。</span><span class="sxs-lookup"><span data-stu-id="11ef2-116">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="11ef2-117">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-117">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-118">ホスティング バンドルのインストール時にインストーラーがインターネットにアクセスできない場合、インストーラーは *Microsoft Visual C++ 2015 再頒布可能パッケージ*を取得できず、この例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-118">If the system doesn't have Internet access while installing the Hosting Bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="11ef2-119">[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)からインストーラーを入手します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-119">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="11ef2-120">インストーラーが失敗する場合、フレームワークに依存する展開 (FDD) をホストするために必要な .NET Core ランタイムをサーバーが受け取っていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-120">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="11ef2-121">FDD をホストしている場合は、[プログラムと機能] でランタイムがインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-121">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="11ef2-122">ランタイム インストーラーが必要な場合は、[.NET の「All Downloads」](https://www.microsoft.com/net/download/all)から入手します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-122">If needed, obtain a runtime installer from [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span> <span data-ttu-id="11ef2-123">ランタイムのインストール後、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-123">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="11ef2-124">OS のアップグレードによって 32 ビット ASP.NET Core モジュールが削除された</span><span class="sxs-lookup"><span data-stu-id="11ef2-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="11ef2-125">**アプリケーション ログ:** モジュール DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** を読み込めませんでした。</span><span class="sxs-lookup"><span data-stu-id="11ef2-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="11ef2-126">このデータはエラーです。</span><span class="sxs-lookup"><span data-stu-id="11ef2-126">The data is the error.</span></span>

<span data-ttu-id="11ef2-127">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-127">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-128">**C:\Windows\SysWOW64\inetsrv** ディレクトリにある OS ファイルでないファイルは、OS アップグレード時に保持されません。</span><span class="sxs-lookup"><span data-stu-id="11ef2-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="11ef2-129">OS アップグレードより前に ASP.NET Core モジュールをインストールしていた場合、OS アップグレード後に 32 ビット モードで AppPool を実行しようとすると、この問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="11ef2-130">OS アップグレード後に ASP.NET Core モジュールを修復してください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="11ef2-131">「[.NET Core ホスティング バンドルのインストール](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-131">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="11ef2-132">インストーラーを実行するときに **[修復]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-132">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="11ef2-133">プラットフォームが RID と競合している</span><span class="sxs-lookup"><span data-stu-id="11ef2-133">Platform conflicts with RID</span></span>

* <span data-ttu-id="11ef2-134">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="11ef2-134">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="11ef2-135">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff. (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンドライン '"C:\{PATH}{assembly}.{exe|dll}" ' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : ff)</span><span class="sxs-lookup"><span data-stu-id="11ef2-135">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="11ef2-136">**ASP.NET Core モジュールのログ:** ハンドルされない例外: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'. (ファイルまたはアセンブリ '{assembly}.dll' が読み込めませんでした)</span><span class="sxs-lookup"><span data-stu-id="11ef2-136">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="11ef2-137">正しくない形式のプログラムを読み込もうとしました。</span><span class="sxs-lookup"><span data-stu-id="11ef2-137">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="11ef2-138">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-138">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-139">Kestrel でアプリをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-139">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="11ef2-140">プロセスのエラーは、アプリ内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-140">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="11ef2-141">詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-141">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="11ef2-142">*.csproj* 内の `<PlatformTarget>` が RID と競合していないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-142">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="11ef2-143">たとえば、`x86` の `<PlatformTarget>` を指定した場合は、*dotnet publish -c Release -r win10-x64* を使用するか、*.csproj* で `<RuntimeIdentifiers>` を `win10-x64` に設定するかを問わず、`win10-x64` の RID を使用して発行しないでください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-143">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="11ef2-144">プロジェクトは警告やエラーにならずに発行されますが、上記のログに記録されたシステムの例外によって失敗します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-144">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="11ef2-145">Azure Apps 展開で、アプリケーションをアップグレードして新しいアセンブリを展開しようとしたときにこの例外が発生した場合は、以前の展開からすべてのファイルを手動で削除してください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-145">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="11ef2-146">アップグレードしたアプリを展開するとき、互換性のないアセンブリが残っていると、`System.BadImageFormatException` 例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-146">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="11ef2-147">URI のエンドポイントが間違っているか、Web サイトが停止している</span><span class="sxs-lookup"><span data-stu-id="11ef2-147">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="11ef2-148">**ブラウザー:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="11ef2-148">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="11ef2-149">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="11ef2-149">**Application Log:** No entry</span></span>

* <span data-ttu-id="11ef2-150">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="11ef2-150">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="11ef2-151">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-151">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-152">アプリに対して正しい URI エンドポイントが使用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-152">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="11ef2-153">バインドを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-153">Check the bindings.</span></span>

* <span data-ttu-id="11ef2-154">IIS Web サイトが *[停止]* 状態でないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-154">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="11ef2-155">CoreWebEngine または W3SVC サーバー機能が無効</span><span class="sxs-lookup"><span data-stu-id="11ef2-155">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="11ef2-156">**OS の例外:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module. (ASP.NET Core モジュールを使用するには、IIS 7.0 CoreWebEngine および W3SVC の機能をインストールする必要があります。)</span><span class="sxs-lookup"><span data-stu-id="11ef2-156">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="11ef2-157">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-157">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-158">適切な役割と機能が有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-158">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="11ef2-159">「[IIS 構成](xref:host-and-deploy/iis/index#iis-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-159">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="11ef2-160">Web サイト物理パスが間違っているか、アプリが見つからない</span><span class="sxs-lookup"><span data-stu-id="11ef2-160">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="11ef2-161">**ブラウザー:** 403 許可されていません - アクセスが拒否されました **--または--** 403.14 許可されていません - Web サーバーは、このディレクトリの内容の一覧を表示しないように構成されています。</span><span class="sxs-lookup"><span data-stu-id="11ef2-161">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="11ef2-162">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="11ef2-162">**Application Log:** No entry</span></span>

* <span data-ttu-id="11ef2-163">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="11ef2-163">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="11ef2-164">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-164">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-165">IIS Web サイトの**基本設定**と物理アプリのフォルダーを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-165">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="11ef2-166">アプリが IIS Web サイトの**物理パス**にあるフォルダー内に配置されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-166">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="11ef2-167">役割が正しくない、モジュールがインストールされていない、または不適切なアクセス許可</span><span class="sxs-lookup"><span data-stu-id="11ef2-167">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="11ef2-168">**ブラウザー:** 500.19 内部サーバー エラー - ページに関連する構成データが無効であるため、要求されたページにアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="11ef2-168">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="11ef2-169">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="11ef2-169">**Application Log:** No entry</span></span>

* <span data-ttu-id="11ef2-170">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="11ef2-170">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="11ef2-171">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-171">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-172">適切な役割が有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-172">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="11ef2-173">「[IIS 構成](xref:host-and-deploy/iis/index#iis-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-173">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="11ef2-174">**[プログラムと機能]** をチェックし、**Microsoft ASP.NET Core モジュール**がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-174">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="11ef2-175">インストールされているプログラムの一覧に **Microsoft ASP.NET Core モジュール**が表示されない場合は、モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="11ef2-175">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="11ef2-176">「[.NET Core ホスティング バンドルのインストール](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-176">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="11ef2-177">**[アプリケーション プール]** > **[プロセス モデル]** > **[ID]** が **ApplicationPoolIdentity** に設定されていることを確認します。または、アプリの展開フォルダーにアクセスするための正しいアクセス許可がカスタム ID に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-177">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="11ef2-178">processPath の誤り、PATH 変数の欠如、ホスティング バンドルが未インストール、システムまたは IIS が再起動されていない、VC++ 再頒布可能パッケージが未インストール、dotnet.exe アクセス違反</span><span class="sxs-lookup"><span data-stu-id="11ef2-178">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="11ef2-179">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="11ef2-179">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="11ef2-180">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0. (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンドライン '".\{assembly}.exe" ' でプロセスを開始できませんでした。ErrorCode = '0x80070002 : 0)</span><span class="sxs-lookup"><span data-stu-id="11ef2-180">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="11ef2-181">**ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です</span><span class="sxs-lookup"><span data-stu-id="11ef2-181">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="11ef2-182">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-182">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-183">Kestrel でアプリをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-183">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="11ef2-184">プロセスのエラーは、アプリ内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-184">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="11ef2-185">詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-185">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="11ef2-186">*web.config* で `<aspNetCore>` 要素の *processPath* 属性を調べ、フレームワークに依存する展開 (FDD) を示す *dotnet*、または自己完結型の展開 (SCD) を示す *.\{assembly}.exe* になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-186">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="11ef2-187">FDD の場合、PATH 設定で *dotnet.exe* にアクセスできていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-187">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="11ef2-188">*C:\Program Files\dotnet\* がシステムの PATH 設定に含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-188">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="11ef2-189">FDD では、アプリケーション プールのユーザー ID で *dotnet.exe* にアクセスできていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-189">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="11ef2-190">AppPool ユーザー ID に、*C:\Program Files\dotnet* ディレクトリへのアクセス許可が設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-190">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="11ef2-191">*C:\Program Files\dotnet* とアプリのディレクトリに、AppPool ユーザー ID に対する拒否ルールが構成されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-191">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="11ef2-192">FDD を配置し、IIS を再起動せずに .NET Core をインストールした可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-192">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="11ef2-193">サーバーを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-193">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="11ef2-194">ホスト システムで、 .NET Core ランタイムをインストールせずに、FDD を配置した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-194">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="11ef2-195">.NET Core ランタイムがインストールされていない場合は、システムで **.NET Core ホスティング バンドルのインストーラー**を実行します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-195">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span> <span data-ttu-id="11ef2-196">「[.NET Core ホスティング バンドルのインストール](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-196">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="11ef2-197">インターネットに接続せずに、.NET Core ランタイムをシステムにインストールする場合は、[.NET の「All Downloads」](https://www.microsoft.com/net/download/all)のページからランタイムを入手し、ホスティング バンドル インストーラーを実行して ASP.NET Core モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="11ef2-197">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET All Downloads](https://www.microsoft.com/net/download/all) and run the Hosting Bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="11ef2-198">インストールを完了するために、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-198">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="11ef2-199">FDD を配置し、*Microsoft Visual C++ 2015 再頒布可能パッケージ (x64)* がシステムにインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-199">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="11ef2-200">[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)からインストーラーを入手します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-200">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="11ef2-201">\<aspNetCore\> 要素の引数の誤り</span><span class="sxs-lookup"><span data-stu-id="11ef2-201">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="11ef2-202">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="11ef2-202">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="11ef2-203">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081. (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンドライン '"dotnet" .\{assembly}.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)</span><span class="sxs-lookup"><span data-stu-id="11ef2-203">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="11ef2-204">**ASP.NET Core モジュールのログ:** The application to execute does not exist: 'PATH\{assembly}.dll' (実行するアプリケーションが存在しません: 'PATH\{assembly}.dll')</span><span class="sxs-lookup"><span data-stu-id="11ef2-204">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="11ef2-205">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-205">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-206">Kestrel でアプリをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-206">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="11ef2-207">プロセスのエラーは、アプリ内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-207">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="11ef2-208">詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-208">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="11ef2-209">*web.config* で `<aspNetCore>` 要素の *arguments* 属性を調べ、次のいずれかになっていることを確認します。(a) フレームワークに依存する展開 (FDD) の場合は *.\{assembly}.dll*、または (b) 自己完結型の展開 (SCD) の場合は、未指定の空の文字列 (*arguments=""*) か、アプリの引数のリスト (*arguments="arg1, arg2, ..."*)。</span><span class="sxs-lookup"><span data-stu-id="11ef2-209">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="11ef2-210">.NET Framework バージョンの欠落</span><span class="sxs-lookup"><span data-stu-id="11ef2-210">Missing .NET Framework version</span></span>

* <span data-ttu-id="11ef2-211">**ブラウザー:** 502.3 ゲートウェイが正しくありません - There was a connection error while trying to route the request. (要求のルーティング中に接続エラーが発生しました。)</span><span class="sxs-lookup"><span data-stu-id="11ef2-211">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="11ef2-212">**アプリケーション ログ:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081. (ErrorCode = 物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンドライン '"dotnet" .\{assembly}.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)</span><span class="sxs-lookup"><span data-stu-id="11ef2-212">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="11ef2-213">**ASP.NET Core モジュールのログ:** メソッド、ファイル、またはアセンブリの欠如の例外。</span><span class="sxs-lookup"><span data-stu-id="11ef2-213">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="11ef2-214">例外で特定されたメソッド、ファイル、またはアセンブリは、.NET Framework のメソッド、ファイル、またはアセンブリです。</span><span class="sxs-lookup"><span data-stu-id="11ef2-214">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="11ef2-215">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="11ef2-215">Troubleshooting:</span></span>

* <span data-ttu-id="11ef2-216">システムにない .NET Framework のバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="11ef2-216">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="11ef2-217">フレームワークに依存する展開 (FDD) では、正しいランタイムがシステムにインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-217">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="11ef2-218">プロジェクトを 1.1 から 2.0 にアップグレードして、ホスト システムに展開したときに、この例外が発生した場合は、ホスト システムに 2.0 Framework が存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-218">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="11ef2-219">アプリケーション プールの停止</span><span class="sxs-lookup"><span data-stu-id="11ef2-219">Stopped Application Pool</span></span>

* <span data-ttu-id="11ef2-220">**ブラウザー:** 503 サービスを利用できません</span><span class="sxs-lookup"><span data-stu-id="11ef2-220">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="11ef2-221">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="11ef2-221">**Application Log:** No entry</span></span>

* <span data-ttu-id="11ef2-222">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="11ef2-222">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="11ef2-223">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="11ef2-223">Troubleshooting</span></span>

* <span data-ttu-id="11ef2-224">アプリケーション プールが *[停止]* 状態でないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-224">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="11ef2-225">IIS 統合ミドルウェアが未実装</span><span class="sxs-lookup"><span data-stu-id="11ef2-225">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="11ef2-226">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="11ef2-226">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="11ef2-227">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンド ライン '"C:\{PATH}\{assembly}.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')</span><span class="sxs-lookup"><span data-stu-id="11ef2-227">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="11ef2-228">**ASP.NET Core モジュールのログ:** ログ ファイルが作成され、正常動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="11ef2-228">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="11ef2-229">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="11ef2-229">Troubleshooting</span></span>

* <span data-ttu-id="11ef2-230">Kestrel でアプリをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-230">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="11ef2-231">プロセスのエラーは、アプリ内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="11ef2-231">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="11ef2-232">詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-232">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="11ef2-233">次のいずれかを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-233">Confirm that either:</span></span>
  * <span data-ttu-id="11ef2-234">アプリの `WebHostBuilder` で `UseIISIntegration` メソッドを呼び出すことによって、IIS 統合ミドルウェアが参照される (ASP.NET Core 1.x)。</span><span class="sxs-lookup"><span data-stu-id="11ef2-234">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="11ef2-235">アプリが `CreateDefaultBuilder` メソッドを使用する (ASP.NET Core 2.x)。</span><span class="sxs-lookup"><span data-stu-id="11ef2-235">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="11ef2-236">詳細については、「[ASP.NET Core でのホスティング](xref:fundamentals/host/index)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-236">See [Host in ASP.NET Core](xref:fundamentals/host/index) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="11ef2-237">サブアプリケーションに \<handlers\> セクションが含まれている</span><span class="sxs-lookup"><span data-stu-id="11ef2-237">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="11ef2-238">**ブラウザー:** HTTP エラー 500.19 - 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="11ef2-238">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="11ef2-239">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="11ef2-239">**Application Log:** No entry</span></span>

* <span data-ttu-id="11ef2-240">**ASP.NET Core モジュールのログ:** ルート アプリに対してログ ファイルが作成され、正常動作を示します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-240">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="11ef2-241">サブアプリに対してはログ ファイルは作成されません。</span><span class="sxs-lookup"><span data-stu-id="11ef2-241">Log file not created for the sub-app.</span></span>

<span data-ttu-id="11ef2-242">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="11ef2-242">Troubleshooting</span></span>

* <span data-ttu-id="11ef2-243">サブアプリの *web.config* ファイルに `<handlers>` セクションが含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-243">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="11ef2-244">stdout ログのパスが正しくありません</span><span class="sxs-lookup"><span data-stu-id="11ef2-244">stdout log path incorrect</span></span>

* <span data-ttu-id="11ef2-245">**ブラウザー:** アプリは通常どおりに応答します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-245">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="11ef2-246">**アプリケーション ログ:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893. (警告: stdoutLogFile \?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log を作成できませんでした。ErrorCode = -2147024893.)</span><span class="sxs-lookup"><span data-stu-id="11ef2-246">**Application Log:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="11ef2-247">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="11ef2-247">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="11ef2-248">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="11ef2-248">Troubleshooting</span></span>

* <span data-ttu-id="11ef2-249">*web.config* の `<aspNetCore>` 要素で指定された `stdoutLogFile` パスが存在しません。</span><span class="sxs-lookup"><span data-stu-id="11ef2-249">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="11ef2-250">詳細については、ASP.NET Core モジュール構成の参照トピックの「[ログの作成とリダイレクト](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)」セクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-250">For more information, see the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) section of the ASP.NET Core Module configuration reference topic.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="11ef2-251">アプリケーション構成の一般的な問題</span><span class="sxs-lookup"><span data-stu-id="11ef2-251">Application configuration general issue</span></span>

* <span data-ttu-id="11ef2-252">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="11ef2-252">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="11ef2-253">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}' でのアプリケーション 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' は、コマンド ライン '"C:\{PATH}\{assembly}.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')</span><span class="sxs-lookup"><span data-stu-id="11ef2-253">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="11ef2-254">**ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です</span><span class="sxs-lookup"><span data-stu-id="11ef2-254">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="11ef2-255">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="11ef2-255">Troubleshooting</span></span>

* <span data-ttu-id="11ef2-256">この一般例外はプロセスを開始できなかったことを示し、ほとんどの場合はアプリケーション構成の問題が原因です。</span><span class="sxs-lookup"><span data-stu-id="11ef2-256">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="11ef2-257">[ディレクトリ構造](xref:host-and-deploy/directory-structure)に関する説明を参照して、アプリの展開済みファイルとフォルダーが適切であることと、アプリの構成ファイルが存在し、そのファイルにアプリと環境に対する正しい設定が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="11ef2-257">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="11ef2-258">詳細については、[トラブルシューティングのヒント](xref:host-and-deploy/iis/troubleshoot)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="11ef2-258">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
