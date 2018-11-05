---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Visual Studio を使用して ASP.NET Web 展開: のトラブルシューティング |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 65cd5cd9f7d1f9c5fdaea9b0d16bdfd84259efdd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837123"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a><span data-ttu-id="4eef1-103">Visual Studio を使用して ASP.NET Web 展開: のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="4eef1-103">ASP.NET Web Deployment using Visual Studio: Troubleshooting</span></span>
====================
<span data-ttu-id="4eef1-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4eef1-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="4eef1-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="4eef1-106">このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="4eef1-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="4eef1-107">系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


<span data-ttu-id="4eef1-108">このページには、Visual Studio を使用して ASP.NET web アプリケーションを展開するときに発生する可能性がある一般的な問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-108">This page describes some common problems that may arise when you deploy an ASP.NET web application by using Visual Studio.</span></span> <span data-ttu-id="4eef1-109">1 つの 1 つ以上の考えられる原因と対応するソリューションは提供されます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-109">For each one, one or more possible causes and corresponding solutions are provided.</span></span>

<span data-ttu-id="4eef1-110">表示されるシナリオは、Azure とサード パーティのホスティング プロバイダーの両方に適用されます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-110">The scenarios shown apply to both Azure and third-party hosting providers.</span></span> <span data-ttu-id="4eef1-111">Azure App Service で web アプリのトラブルシューティングの詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-111">For more information about troubleshooting web apps in Azure App Service, see the following resources:</span></span>

- [<span data-ttu-id="4eef1-112">Visual Studio を使用した Azure App Service での Web アプリのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="4eef1-112">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [<span data-ttu-id="4eef1-113">Azure App Service で Web アプリの監視</span><span class="sxs-lookup"><span data-stu-id="4eef1-113">Monitor Web Apps in Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- <span data-ttu-id="4eef1-114">[.NET 用の Windows Azure SDK 2.0 のリリースの発表](http:// https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net)(ScottGu のブログでは、Visual Studio で診断ログを取得する方法を示します)</span><span class="sxs-lookup"><span data-stu-id="4eef1-114">[Announcing the release of Windows Azure SDK 2.0 for .NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (ScottGu's blog, shows how to get diagnostic logs in Visual Studio)</span></span>

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a><span data-ttu-id="4eef1-115">サーバー エラーからリモートで表示されている現在のカスタム エラーの設定が、エラーの詳細を防ぐため '/' アプリケーション - で</span><span class="sxs-lookup"><span data-stu-id="4eef1-115">Server Error in '/' Application - Current Custom Error Settings Prevent Details of the Error from Being Viewed Remotely</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-116">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-116">Scenario</span></span>

<span data-ttu-id="4eef1-117">リモート ホストに、サイトを展開した後は、メンションの Web.config ファイルの customErrors 設定が、実際のエラーの原因が示されていませんが、エラー メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-117">After deploying a site to a remote host, you get an error message that mentions the customErrors setting in the Web.config file but doesn't indicate what the actual cause of the error was:</span></span>

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-118">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-118">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-119">既定では、ASP.NET では、web アプリケーションがローカル コンピューターで実行されている場合にのみに詳細なエラー情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-119">By default, ASP.NET shows detailed error information only when your web application is running on the local computer.</span></span> <span data-ttu-id="4eef1-120">一般に、web アプリケーションが、ハッカーがこの情報を使用して、アプリケーションの脆弱性を検索することができますので、インターネット経由で公開されている使用可能なときに、詳細なエラー情報を表示したくないです。</span><span class="sxs-lookup"><span data-stu-id="4eef1-120">Generally you don't want to display detailed error information when your web application is publicly available over the Internet, because hackers may be able to use this information to find vulnerabilities in the application.</span></span> <span data-ttu-id="4eef1-121">ただし、サイト、サイトまたは更新プログラムを展開する場合に場合があります何かが悪化して実際のエラー メッセージを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-121">However, when you are deploying a site or updates to a site, sometimes something will go wrong and you need to get the actual error message.</span></span>

<span data-ttu-id="4eef1-122">リモート ホストで実行されるときに、詳細なエラー メッセージを表示するアプリケーションを有効にするには、customErrors モードを off に設定、アプリケーションを再デプロイ、およびアプリケーションを再度実行するには、Web.config ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-122">To enable the application to display detailed error messages when it runs on the remote host, edit the Web.config file to set customErrors mode off, redeploy the application, and run the application again:</span></span>

1. <span data-ttu-id="4eef1-123">アプリケーションの Web.config ファイルでは、thesystem.web 要素に acustomErrors 要素が含まれる場合は、"off"に themode 属性を変更します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-123">If the application Web.config file has acustomErrors element in thesystem.web element, change themode attribute to "off".</span></span> <span data-ttu-id="4eef1-124">それ以外の場合要素に追加 acustomErrors 要素 thesystem.web themode 属性が"off"に設定された次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-124">Otherwise add acustomErrors element in thesystem.web element with themode attribute set to "off", as shown in the following example:</span></span> 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. <span data-ttu-id="4eef1-125">アプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-125">Deploy the application.</span></span>
3. <span data-ttu-id="4eef1-126">アプリケーションを実行し、どの前に行ったエラーが発生する原因となったを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-126">Run the application and repeat whatever you did earlier that caused the error to occur.</span></span> <span data-ttu-id="4eef1-127">ここでは、実際のエラー メッセージを確認できます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-127">Now you can see what the actual error message is.</span></span>
4. <span data-ttu-id="4eef1-128">エラーを解決するときに、customErrors の元の設定を復元し、アプリケーションを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-128">When you have resolved the error, restore the original customErrors setting and redeploy the application.</span></span>

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a><span data-ttu-id="4eef1-129">できませんを作成/シャドウ コピー 'ContosoUniversity' そのファイルが既に存在する場合。</span><span class="sxs-lookup"><span data-stu-id="4eef1-129">Cannot create/shadow copy 'ContosoUniversity' when that file already exists.</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-130">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-130">Scenario</span></span>

<span data-ttu-id="4eef1-131">Visual Studio でプロジェクトを実行するときは、次の例のようなメッセージを示すエラー ページを取得します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-131">When you try to run a project in Visual Studio you get an error page with a message like the following example:</span></span>

<span data-ttu-id="4eef1-132">'/' アプリケーションにサーバー エラーがあります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-132">Server Error in '/' Application.</span></span> <span data-ttu-id="4eef1-133">できませんを作成/シャドウ コピー 'ContosoUniversity' そのファイルが既に存在する場合。</span><span class="sxs-lookup"><span data-stu-id="4eef1-133">Cannot create/shadow copy 'ContosoUniversity' when that file already exists.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-134">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-134">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-135">ちょっと待ってくださいとブラウザーを更新するまたはサイトを再コンパイルし、もう一度実行していることをお試しください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-135">Wait a minute and refresh the browser, or recompile the site and try running it again.</span></span>

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a><span data-ttu-id="4eef1-136">SQL Server の使用を最適化することで、Web ページ アクセスが拒否されました</span><span class="sxs-lookup"><span data-stu-id="4eef1-136">Access is Denied in a Web Page that Uses SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-137">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-137">Scenario</span></span>

<span data-ttu-id="4eef1-138">SQL Server Compact を使用するサイトを展開すると、データベースにアクセスするデプロイされたサイトのページを実行する、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-138">When you deploy a site that uses SQL Server Compact and you run a page in the deployed site that accesses the database, you see the following error message:</span></span>

<span data-ttu-id="4eef1-139">アクセスが拒否されました。</span><span class="sxs-lookup"><span data-stu-id="4eef1-139">Access is denied.</span></span> <span data-ttu-id="4eef1-140">(HRESULT からの例外: 0x80070005 (E\_ACCESSDENIED))</span><span class="sxs-lookup"><span data-stu-id="4eef1-140">(Exception from HRESULT: 0x80070005 (E\_ACCESSDENIED))</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-141">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-141">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-142">サーバー上のネットワーク サービス アカウントがされている SQL Service Compact のネイティブ バイナリを読めるようにする必要があります、 *bin\amd64*または*bin\x86*フォルダーでは、これは読み取り権限がないそれらのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="4eef1-142">The NETWORK SERVICE account on the server needs to be able to read SQL Service Compact native binaries that are in the *bin\amd64* or *bin\x86* folder, but it does not have read permissions for those folders.</span></span> <span data-ttu-id="4eef1-143">ネットワーク サービスのアクセス許可の読み取りの設定、 *bin*フォルダー、サブフォルダーへのアクセス許可を拡張することを確認します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-143">Set read permission for NETWORK SERVICE on the *bin* folder, making sure to extend the permissions to subfolders.</span></span>

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a><span data-ttu-id="4eef1-144">十分なアクセス許可により、構成ファイルを読み取ることができません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-144">Cannot Read Configuration File Due to Insufficient Permissions</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-145">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-145">Scenario</span></span>

<span data-ttu-id="4eef1-146">クリックすると、Visual Studio の発行、ローカル コンピューター上の IIS にアプリケーションを配置する発行が失敗して、**出力**ウィンドウは次のようなエラー メッセージを表示。</span><span class="sxs-lookup"><span data-stu-id="4eef1-146">When you click the Visual Studio publish button to deploy an application to IIS on your local machine, publishing fails and the **Output** window shows an error message similar to this:</span></span>

<span data-ttu-id="4eef1-147">IIS 構成ファイル 'マシン/リダイレクト' を読み込むときにエラーが発生しました。</span><span class="sxs-lookup"><span data-stu-id="4eef1-147">An error occurred when reading the IIS Configuration File 'MACHINE/REDIRECTION'.</span></span> <span data-ttu-id="4eef1-148">この操作を実行する id がしています.エラー: 十分なアクセス許可により、構成ファイルを読み取ることができません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-148">The identity performing this operation was ... Error: Cannot read configuration file due to insufficient permissions.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-149">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-149">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-150">1 回のクリックを使用するローカル コンピューター上の IIS に発行、管理者のアクセス許可を持つ Visual Studio を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-150">To use one-click publish to IIS on your local machine, you must be running Visual Studio with administrator permissions.</span></span> <span data-ttu-id="4eef1-151">Visual Studio を閉じてから、管理者権限で再起動します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-151">Close Visual Studio and restart it with administrator permissions.</span></span>

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a><span data-ttu-id="4eef1-152">対象のコンピューターに接続できませんでした.指定されたプロセスを使用します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-152">Could Not Connect to the Destination Computer ... Using the Specified Process</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-153">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-153">Scenario</span></span>

<span data-ttu-id="4eef1-154">クリックすると、Visual Studio の発行、アプリケーションの展開ボタン発行が失敗して、**出力**ウィンドウは次のようなエラー メッセージを表示。</span><span class="sxs-lookup"><span data-stu-id="4eef1-154">When you click the Visual Studio publish button to deploy an application, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-155">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-155">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-156">プロキシ サーバーは、移行先サーバーとの通信を中断することです。</span><span class="sxs-lookup"><span data-stu-id="4eef1-156">A proxy server is interrupting communication with the destination server.</span></span> <span data-ttu-id="4eef1-157">Windows コントロール パネルから、または Internet Explorer で、選択**インターネット オプション**を選択し、**接続**タブ。**インターネット プロパティ**ダイアログ ボックスで、をクリックして**LAN の設定**します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-157">From the Windows Control Panel or in Internet Explorer, select **Internet Options** and select the **Connections** tab. In the **Internet Properties** dialog box, click **LAN Settings**.</span></span> <span data-ttu-id="4eef1-158">**ローカル エリア ネットワーク (LAN) 設定** ダイアログ ボックスで、クリア、**設定を自動的に検出**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-158">In the **Local Area Network (LAN) Settings** dialog box, clear the **Automatically detect settings** checkbox.</span></span> <span data-ttu-id="4eef1-159">[発行] ボタンを再度クリックします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-159">Then click the publish button again.</span></span>

<span data-ttu-id="4eef1-160">問題が解決しない場合は、プロキシまたはファイアウォールの設定で何ができる、システム管理者に問い合わせてください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-160">If the problem persists, contact your system administrator to determine what can be done with proxy or firewall settings.</span></span> <span data-ttu-id="4eef1-161">Web Deploy Web 管理サービスの展開 (8172); の非標準ポートを使用するために、問題が発生します。他の接続では、Web Deploy はポート 80 を使用します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-161">The problem happens because Web Deploy uses a non-standard port for Web Management Service deployment (8172); for other connections, Web Deploy uses port 80.</span></span> <span data-ttu-id="4eef1-162">サード パーティのホスティング プロバイダーに展開する場合に通常 Web 管理サービスを使用しています。</span><span class="sxs-lookup"><span data-stu-id="4eef1-162">When you are deploying to a third-party hosting provider, you are typically using the Web Management Service.</span></span>

## <a name="default-net-40-application-pool-does-not-exist"></a><span data-ttu-id="4eef1-163">既定の .NET 4.0 のアプリケーション プールが存在しません</span><span class="sxs-lookup"><span data-stu-id="4eef1-163">Default .NET 4.0 Application Pool Does Not Exist</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-164">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-164">Scenario</span></span>

<span data-ttu-id="4eef1-165">.NET Framework 4 が必要なアプリケーションを展開するときに、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-165">When you deploy an application that requires the .NET Framework 4, you see the following error message:</span></span>

<span data-ttu-id="4eef1-166">既定の .NET 4.0 アプリケーション プールが存在しないか、アプリケーションを追加できませんでした。</span><span class="sxs-lookup"><span data-stu-id="4eef1-166">The default .NET 4.0 application pool does not exist or the application could not be added.</span></span> <span data-ttu-id="4eef1-167">このコンピューターで ASP.NET 4.0 がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-167">Please verify that ASP.NET 4.0 is installed on this machine.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-168">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-168">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-169">IIS では、ASP.NET 4 がインストールされていません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-169">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="4eef1-170">配置するサーバーは、開発用コンピューターに Visual Studio 2010 をインストールする場合は、ASP.NET 4 がコンピューターにインストールされますが、IIS にインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-170">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="4eef1-171">展開して、サーバーでコマンド プロンプトを開くし、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-171">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

<span data-ttu-id="4eef1-172">また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-172">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="4eef1-173">詳細については、このシリーズではテスト環境のチュートリアルとして IIS に展開するを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-173">For more information, see the Deploying to IIS as a Test Environment tutorial in this series.</span></span>

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a><span data-ttu-id="4eef1-174">初期化文字列の形式は、インデックス 0 から始まる位置の仕様に準拠していません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-174">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-175">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-175">Scenario</span></span>

<span data-ttu-id="4eef1-176">1 回のクリックを使用してアプリケーションを展開した後の発行、次のエラー メッセージを取得したデータベースにアクセスするページを実行するとき。</span><span class="sxs-lookup"><span data-stu-id="4eef1-176">After you deploy an application using one-click publish, when you run a page that accesses the database you get the following error message:</span></span>

<span data-ttu-id="4eef1-177">初期化文字列の形式は、インデックス 0 から始まる位置の仕様に準拠していません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-177">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-178">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-178">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-179">開く、 *Web.config*ファイルで、展開されたサイトと接続文字列の値が $ で始まるかどうかを確認 (ReplacableToken\_、次の例。</span><span class="sxs-lookup"><span data-stu-id="4eef1-179">Open the *Web.config* file in the deployed site and check to see whether the connection string values begin with $(ReplacableToken\_, as in the following example:</span></span>

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

<span data-ttu-id="4eef1-180">接続文字列は、この例のように、プロジェクト ファイルを編集し、次のプロパティをすべてのビルド構成である PropertyGroup 要素に追加。</span><span class="sxs-lookup"><span data-stu-id="4eef1-180">If the connection strings look like this example, edit the project file and add the following property to the PropertyGroup element that is for all build configurations:</span></span>

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

<span data-ttu-id="4eef1-181">アプリケーションを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-181">Then redeploy the application.</span></span>

## <a name="http-500-internal-server-error"></a><span data-ttu-id="4eef1-182">HTTP 500 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="4eef1-182">HTTP 500 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-183">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-183">Scenario</span></span>

<span data-ttu-id="4eef1-184">デプロイされたサイトを実行するとは、エラーの原因を示す特定の情報がない場合、次のエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-184">When you run the deployed site, you see the following error message without specific information indicating the cause of the error:</span></span>

<span data-ttu-id="4eef1-185">HTTP エラー 500 - 内部サーバー エラー。</span><span class="sxs-lookup"><span data-stu-id="4eef1-185">HTTP Error 500 - Internal Server Error.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-186">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-186">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-187">500 のエラーの原因の多くがありますが、これらのチュートリアルをフォローしている場合は、考えられる原因の 1 つは XML 要素を Web.config 変換ファイルのいずれかで間違った場所に配置すること。</span><span class="sxs-lookup"><span data-stu-id="4eef1-187">There are many causes of 500 errors, but one possible cause if you are following these tutorials is that you put an XML element in the wrong place in one of the Web.config transformation files.</span></span> <span data-ttu-id="4eef1-188">挿入する変換を配置する場合にこのエラーが得られるなど、&lt;場所&gt;要素  &lt;system.web&gt;の代わりの直下にある&lt;構成&gt;します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-188">For example, you would get this error if you put the transformation that inserts a &lt;location&gt; element under &lt;system.web&gt; instead of directly under &lt;configuration&gt;.</span></span> <span data-ttu-id="4eef1-189">Web.config 変換のプレビュー機能を使用すると、変換が意図したとおりに動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-189">You can use the Web.config transform preview feature to verify that transformations are working as intended.</span></span> <span data-ttu-id="4eef1-190">コーディングがある変換を検索する場合は、変換ファイルを修正して再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-190">The solution if you find a transform that was coded incorrectly is to correct the transformation file and redeploy.</span></span> <span data-ttu-id="4eef1-191">エラーが明らかな場合は、変換をコメント アウトし、500 エラーの原因を確認する 1 つの再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-191">If an error isn't obvious, try commenting out transforms and redeploying to see which one is causing the 500 error.</span></span>

## <a name="http-50021-internal-server-error"></a><span data-ttu-id="4eef1-192">HTTP 500.21 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="4eef1-192">HTTP 500.21 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-193">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-193">Scenario</span></span>

<span data-ttu-id="4eef1-194">デプロイされたサイトを実行すると、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-194">When you run the deployed site, you see the following error message:</span></span>

<span data-ttu-id="4eef1-195">HTTP エラー 500.21 - 内部サーバー エラーです。</span><span class="sxs-lookup"><span data-stu-id="4eef1-195">HTTP Error 500.21 - Internal Server Error.</span></span> <span data-ttu-id="4eef1-196">「PageHandlerFactory 統合」ハンドラーが、そのモジュールの一覧で無効なモジュール"ManagedPipelineHandler"。</span><span class="sxs-lookup"><span data-stu-id="4eef1-196">Handler "PageHandlerFactory-Integrated" has a bad module "ManagedPipelineHandler" in its module list.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-197">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-197">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-198">サイトが、ASP.NET 4、ASP.NET 4 に登録されていない IIS サーバー上のターゲットを導入しています。</span><span class="sxs-lookup"><span data-stu-id="4eef1-198">The site you have deployed targets ASP.NET 4, but ASP.NET 4 is not registered in IIS on the server.</span></span> <span data-ttu-id="4eef1-199">サーバーで管理者特権のコマンド プロンプトを開き、次のコマンドを実行して、ASP.NET 4 を登録します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-199">On the server open an elevated command prompt and register ASP.NET 4 by running the following commands:</span></span>

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

<span data-ttu-id="4eef1-200">また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-200">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="4eef1-201">詳細については、このシリーズではテスト環境のチュートリアルとして IIS に展開するを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-201">For more information, see the Deploying to IIS as a Test Environment tutorial in this series.</span></span>

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a><span data-ttu-id="4eef1-202">アプリには、SQL Server Express のデータベースを開くはログインできませんでした\_データ</span><span class="sxs-lookup"><span data-stu-id="4eef1-202">Login Failed Opening SQL Server Express Database in App\_Data</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-203">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-203">Scenario</span></span>

<span data-ttu-id="4eef1-204">更新する、 *Web.config*ファイルとしての SQL Server Express データベースを指す接続文字列、 *.mdf*ファイル、*アプリ\_データ*フォルダー、および最初次のエラー メッセージが表示アプリケーションを実行する時刻:</span><span class="sxs-lookup"><span data-stu-id="4eef1-204">You updated the *Web.config* file connection string to point to a SQL Server Express database as an *.mdf* file in your *App\_Data* folder, and the first time you run the application you see the following error message:</span></span>

<span data-ttu-id="4eef1-205">System.data.sqlclient.sqlexception: には、データベース"DatabaseName"ログインで要求を開くことができません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-205">System.Data.SqlClient.SqlException: Cannot open database "DatabaseName" requested by the login.</span></span> <span data-ttu-id="4eef1-206">The login failed.\(ログインに失敗しました。\)</span><span class="sxs-lookup"><span data-stu-id="4eef1-206">The login failed.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-207">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-207">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-208">名前、 *.mdf*削除した場合でも、ファイルがコンピューターには、これまで存在していた SQL Server Express のデータベースの名前を一致ことはできません、 *.mdf*既存のデータベースのファイル。</span><span class="sxs-lookup"><span data-stu-id="4eef1-208">The name of the *.mdf* file cannot match the name of any SQL Server Express database that has ever existed on your computer, even if you deleted the *.mdf* file of the previously existing database.</span></span> <span data-ttu-id="4eef1-209">名前を変更、 *.mdf*ファイル変更とデータベースの名前として使用されていない名前を*Web.config*ファイルを新しい名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-209">Change the name of the *.mdf* file to a name that has never been used as a database name and change the *Web.config* file to use the new name.</span></span> <span data-ttu-id="4eef1-210">代わりに、使用することができます[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)データベースを以前の既存の SQL Server Express を削除します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-210">As an alternative, you can use [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete previously existing SQL Server Express databases.</span></span>

## <a name="model-compatibility-cannot-be-checked"></a><span data-ttu-id="4eef1-211">チェックするモデルの互換性ことはできません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-211">Model Compatibility Cannot be Checked</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-212">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-212">Scenario</span></span>

<span data-ttu-id="4eef1-213">更新する、 *Web.config*ファイルを新しい SQL Server Express データベースを指す接続文字列と、初めてアプリケーションを実行する次のエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-213">You updated the *Web.config* file connection string to point to a new SQL Server Express database, and the first time you run the application you see the following error message:</span></span>

<span data-ttu-id="4eef1-214">データベースにモデルのメタデータが含まれていないために、モデルの互換性をチェックできません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-214">Model compatibility cannot be checked because the database does not contain model metadata.</span></span> <span data-ttu-id="4eef1-215">IncludeMetadataConvention が DbModelBuilder 規則に追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-215">Ensure that IncludeMetadataConvention has been added to the DbModelBuilder conventions.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-216">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-216">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-217">場合に、Web.config ファイルを配置するデータベース名がコンピューターには、データベースが既に存在するいくつかのテーブルを含む前にこれまで使用されます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-217">If the database name you put in the Web.config file was ever used before on your computer, a database might already exist with some tables in it.</span></span> <span data-ttu-id="4eef1-218">前に、のコンピューターと変更に使用されていない新しい名前を選択、 *Web.config*ファイルをポイントして、この新しいデータベース名を使用します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-218">Select a new name that has not been used on your computer before and change the *Web.config* file to point to use this new database name.</span></span> <span data-ttu-id="4eef1-219">代わりに、使用することができます[SQL Server Express ユーティリティ](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)または[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)を既存のデータベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-219">As an alternative, you can use [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) or [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete the existing database.</span></span>

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a><span data-ttu-id="4eef1-220">スクリプトがユーザーまたはロールを作成しようとしたときに、SQL エラー</span><span class="sxs-lookup"><span data-stu-id="4eef1-220">SQL Error When a Script Attempts to Create Users or Roles</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-221">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-221">Scenario</span></span>

<span data-ttu-id="4eef1-222">構成されているデータベースの配置を使用している、**パッケージ化/発行 SQL**  タブで、デプロイ時に実行される SQL スクリプトなどのユーザーの作成またはロールの作成のコマンドをスクリプトの実行が失敗したこれらのコマンドを実行するとします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-222">You are using database deployment configured on the **Package/Publish SQL** tab, SQL scripts that run during deployment include Create User or Create Role commands, and script execution fails when those commands are executed.</span></span> <span data-ttu-id="4eef1-223">次のように、メッセージの詳細を参照して可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-223">You might see more detailed messages, such as the following:</span></span>

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

<span data-ttu-id="4eef1-224">データベースの配置を構成したときにこのエラーが発生、 **Web の発行**ウィザードではなく、**パッケージ化/発行 SQL**  タブでスレッドを作成、[構成し、展開](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)フォーラム、およびソリューションは、このトラブルシューティングのページに追加されます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-224">If this error occurs when you have configured database deployment in the **Publish Web** wizard rather than the **Package/Publish SQL** tab, create a thread in the [Configuration and Deployment](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum, and the solution will be added to this troubleshooting page.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-225">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-225">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-226">展開の実行を使用しているユーザー アカウントには、ユーザーまたはロールを作成するアクセス許可がありません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-226">The user account you are using to perform deployment does not have permission to create users or roles.</span></span> <span data-ttu-id="4eef1-227">たとえば、ホスティング企業が、db を割り当てることができます\_datareader、db\_、datawriter の各と db\_ddladmin ロールを設定するユーザー アカウントにします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-227">For example, the hosting company might assign the db\_datareader, db\_datawriter, and db\_ddladmin roles to the user account that it sets up for you.</span></span> <span data-ttu-id="4eef1-228">これらは、十分なユーザーまたはロールの作成ではなく、ほとんどのデータベース オブジェクトを作成するためです。</span><span class="sxs-lookup"><span data-stu-id="4eef1-228">These are sufficient for creating most database objects, but not for creating users or roles.</span></span> <span data-ttu-id="4eef1-229">エラーを回避する方法の 1 つは、データベースの配置からのユーザーとロールを除外することです。</span><span class="sxs-lookup"><span data-stu-id="4eef1-229">One way to avoid the error is by excluding users and roles from database deployment.</span></span> <span data-ttu-id="4eef1-230">次の属性が含まれるようにデータベースの自動生成されたスクリプトの PreSource 要素を編集して、これを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-230">You can do this by editing the PreSource element for the database's automatically generated script so that it includes the following attributes:</span></span>

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

<span data-ttu-id="4eef1-231">プロジェクト ファイルで PreSource 要素を編集する方法については、次を参照してください。[方法: プロジェクト ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-231">For information about how to edit the PreSource element in the project file, see [How to: Edit Deployment Settings in the Project File](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx).</span></span> <span data-ttu-id="4eef1-232">ユーザーまたはロールは、開発用データベースでは、移行先データベースに存在する必要がある場合、は、ホスティング プロバイダーに問い合わせてしてください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-232">If the users or roles in your development database need to be in the destination database, contact your hosting provider for assistance.</span></span>

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a><span data-ttu-id="4eef1-233">デプロイ時にカスタム スクリプトを実行するときに、SQL Server のタイムアウト エラー</span><span class="sxs-lookup"><span data-stu-id="4eef1-233">SQL Server Timeout Error When Running Custom Scripts During Deployment</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-234">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-234">Scenario</span></span>

<span data-ttu-id="4eef1-235">デプロイ時に、実行するカスタムの SQL スクリプトを指定したし、タイムアウトと Web 配置を実行して、します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-235">You have specified custom SQL scripts to run during deployment, and when Web Deploy runs them, they time out.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-236">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-236">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-237">別のトランザクション モードを持つ複数のスクリプトを実行すると、タイムアウト エラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-237">Running multiple scripts that have different transaction modes can cause time-out errors.</span></span> <span data-ttu-id="4eef1-238">既定では、自動的に生成されたスクリプトは、トランザクションで実行しますが、カスタム スクリプトはありません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-238">By default, automatically generated scripts run in a transaction, but custom scripts do not.</span></span> <span data-ttu-id="4eef1-239">選択した場合、**データや既存のデータベースからスキーマをプル**オプション、**パッケージ化/発行 SQL**  タブで、カスタム SQL スクリプトを追加する場合は、いくつかのスクリプトでのトランザクション設定を変更する必要がありますようにすべてのスクリプトでは、同じトランザクションの設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-239">If you select the **Pull data and/or schema from an existing database** option on the **Package/Publish SQL** tab, and if you add a custom SQL script, you must change transaction settings on some scripts so that all scripts use the same transaction settings.</span></span> <span data-ttu-id="4eef1-240">詳細については、次を参照してください。[方法: Web アプリケーション プロジェクトでのデータベース配置](https://msdn.microsoft.com/library/dd465343.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-240">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343.aspx).</span></span>

<span data-ttu-id="4eef1-241">すべてが同じになるように、トランザクションの設定を構成したが、このエラーが引き続き発生すると場合、考えられる回避策とは別にスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-241">If you have configured transaction settings so that all are the same but still get this error, a possible workaround is to run the scripts separately.</span></span> <span data-ttu-id="4eef1-242">**データベース スクリプト**グリッドで、**パッケージ化/発行**SQL タブで、、 **Include**タイムアウト エラーが発生するスクリプトのチェック ボックスは、プロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-242">In the **Database Scripts** grid in the **Package/Publish** SQL tab, clear the **Include** check box for the script that causes the timeout error, then publish the project.</span></span> <span data-ttu-id="4eef1-243">移動し、**データベース スクリプト**グリッド、スクリプトを選択します**Include**チェック ボックスをオンし、オフ、 **Include**他のスクリプトのチェック ボックスを。</span><span class="sxs-lookup"><span data-stu-id="4eef1-243">Then go back into the **Database Scripts** grid, select that script's **Include** check box, and clear the **Include** check boxes for the other scripts.</span></span> <span data-ttu-id="4eef1-244">プロジェクトをもう一度発行します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-244">Then publish the project again.</span></span> <span data-ttu-id="4eef1-245">この時間を発行すると、選択したカスタム スクリプトのみが実行されます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-245">This time when you publish, only the selected custom script runs.</span></span>

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a><span data-ttu-id="4eef1-246">サイトのマニフェストの Stream データはまだ使用できません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-246">Stream Data of Site Manifest Is Not Yet Available</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-247">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-247">Scenario</span></span>

<span data-ttu-id="4eef1-248">使用してパッケージをインストールする場合、 *deploy.cmd*ファイルを t (テスト) オプションでは、次のエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-248">When you are installing a package using the *deploy.cmd* file with the t (test) option, you see the following error message:</span></span>

<span data-ttu-id="4eef1-249">エラー: のストリーム データが、' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' はまだ使用できません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-249">Error: The stream data of 'sitemanifest/dbFullSql[@path='C:\TEMP\AdventureWorksGrant.sql']/sqlScript' is not yet available.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-250">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-250">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-251">エラー メッセージでは、コマンドがテスト レポートを生成できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-251">The error message means that the command cannot produce a test report.</span></span> <span data-ttu-id="4eef1-252">ただし、y (実際のインストール) オプションを使用する場合、コマンドは実行可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-252">However, the command might run if you use the y (actual installation) option.</span></span> <span data-ttu-id="4eef1-253">メッセージは、テスト モードでコマンドを実行に問題があることにのみを示します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-253">The message indicates only that there is a problem with running the command in test mode.</span></span>

## <a name="this-application-requires-managedruntimeversion-v40"></a><span data-ttu-id="4eef1-254">このアプリケーションに ManagedRuntimeVersion v4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="4eef1-254">This Application Requires ManagedRuntimeVersion v4.0</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-255">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-255">Scenario</span></span>

<span data-ttu-id="4eef1-256">デプロイしようとすると、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-256">When you attempt to deploy, you see the following error message:</span></span>

<span data-ttu-id="4eef1-257">使用しようとしているアプリケーション プールが、'managedRuntimeVersion' プロパティが 'v2.0' に設定します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-257">The application pool that you are trying to use has the 'managedRuntimeVersion' property set to 'v2.0'.</span></span> <span data-ttu-id="4eef1-258">このアプリケーションでは、'v4.0' が必要です。</span><span class="sxs-lookup"><span data-stu-id="4eef1-258">This application requires 'v4.0'.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-259">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-259">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-260">IIS では、ASP.NET 4 がインストールされていません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-260">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="4eef1-261">配置するサーバーは、開発用コンピューターに Visual Studio 2010 をインストールする場合は、ASP.NET 4 がコンピューターにインストールされますが、IIS にインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-261">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="4eef1-262">展開して、サーバーでコマンド プロンプトを開くし、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-262">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a><span data-ttu-id="4eef1-263">Microsoft.Web.Deployment.DeploymentProviderOptions をキャストできません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-263">Unable to cast Microsoft.Web.Deployment.DeploymentProviderOptions</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-264">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-264">Scenario</span></span>

<span data-ttu-id="4eef1-265">パッケージを展開する場合に、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-265">When you are deploying a package, you see the following error message:</span></span>

<span data-ttu-id="4eef1-266">型 'Microsoft.Web.Deployment.DeploymentProviderOptions' を ' Microsoft.Web.Deployment.DeploymentProviderOptions' のオブジェクトをキャストできません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-266">Unable to cast object of type 'Microsoft.Web.Deployment.DeploymentProviderOptions' to 'Microsoft.Web.Deployment.DeploymentProviderOptions'.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-267">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-267">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-268">Web Deploy 2.0 がインストールされているサーバーに Web デプロイ 1.1 UI を使用して IIS マネージャーからのデプロイしようとしました。</span><span class="sxs-lookup"><span data-stu-id="4eef1-268">You are trying to deploy from IIS Manager using the Web Deploy 1.1 UI to a server that has Web Deploy 2.0 installed.</span></span> <span data-ttu-id="4eef1-269">チェック、パッケージをインポートして展開する IIS のリモート管理ツールを使用している場合、**機能利用可能な新しい** ダイアログ ボックス、接続を確立するときにします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-269">If you are using the IIS Remote Administration Tool to deploy by importing a package, check the **New Features Available** dialog box when you establish the connection.</span></span> <span data-ttu-id="4eef1-270">(このダイアログ ボックスがありますのみ表示されます 1 回、接続が最初に確立されたときに。</span><span class="sxs-lookup"><span data-stu-id="4eef1-270">(This dialog box might only be shown once when the connection is first established.</span></span> <span data-ttu-id="4eef1-271">接続を消去し、最初からやり直す、IIS マネージャーを閉じますと inetmgr を入力して、もう一度開始が/コマンド プロンプトでリセットします。)機能のいずれかが表示されている場合は、 **Web デプロイ UI**、8 よりも低いバージョン番号があるを展開して、サーバーは 1.1 と 2.0 の両方のバージョンの Web 配置がインストールされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-271">To clear the connection and start over, close IIS Manager and start it up again by entering inetmgr /reset at the command prompt.) If one of the features listed is **Web Deploy UI**, and it has a version number lower than 8, the server you are deploying to might have both 1.1 and 2.0 versions of Web Deploy installed.</span></span> <span data-ttu-id="4eef1-272">2.0 をインストールしているクライアントからのデプロイ、サーバーにのみ Web Deploy 2.0 をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-272">To deploy from a client that has 2.0 installed, the server must have only Web Deploy 2.0 installed.</span></span> <span data-ttu-id="4eef1-273">この問題を解決するのには、ホスティング プロバイダーに連絡する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-273">You will have to contact your hosting provider to resolve this problem.</span></span>

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a><span data-ttu-id="4eef1-274">SQL Server Compact のネイティブ コンポーネントを読み込めませんでした。</span><span class="sxs-lookup"><span data-stu-id="4eef1-274">Unable to load the native components of SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-275">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-275">Scenario</span></span>

<span data-ttu-id="4eef1-276">デプロイされたサイトを実行すると、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-276">When you run the deployed site, you see the following error message:</span></span>

<span data-ttu-id="4eef1-277">SQL Server Compact の 8482 のバージョンの ADO.NET プロバイダーに対応のネイティブ コンポーネントを読み込めません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-277">Unable to load the native components of SQL Server Compact corresponding to the ADO.NET provider of version 8482.</span></span> <span data-ttu-id="4eef1-278">SQL Server Compact の正しいバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-278">Install the correct version of SQL Server Compact.</span></span> <span data-ttu-id="4eef1-279">詳細については、KB 記事 974247 を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-279">Refer to KB article 974247 for more details.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-280">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-280">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-281">展開されたサイトがない*amd64*と*x86*アプリケーションの下にネイティブ アセンブリを含むサブフォルダー *bin*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="4eef1-281">The deployed site does not have *amd64* and *x86* subfolders with the native assemblies in them under the application's *bin* folder.</span></span> <span data-ttu-id="4eef1-282">SQL Server Compact がインストールされているコンピューターで、ネイティブ アセンブリ内にある*C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private*します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-282">On a computer that has SQL Server Compact installed, the native assemblies are located in *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*.</span></span> <span data-ttu-id="4eef1-283">Visual Studio プロジェクトに適切なフォルダーに正しいファイルを取得する最善の方法では、NuGet SqlServerCompact パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-283">The best way to get the correct files into the correct folders in a Visual Studio project is to install the NuGet SqlServerCompact package.</span></span> <span data-ttu-id="4eef1-284">パッケージのインストールにネイティブ アセンブリをコピーするビルド後のスクリプトを追加します*amd64*と*x86*します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-284">Package installation adds a post-build script to copy the native assemblies into *amd64* and *x86*.</span></span> <span data-ttu-id="4eef1-285">これらを展開するためには、ただし、手動で、プロジェクトに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-285">In order for these to be deployed, however, you have to manually include them in the project.</span></span> <span data-ttu-id="4eef1-286">詳細については、次を参照してください。、[を展開する SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="4eef1-286">For more information, see the [Deploying SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a><span data-ttu-id="4eef1-287">Entity Framework Code First アプリケーションのデプロイ後にエラーが「パスが無効です」</span><span class="sxs-lookup"><span data-stu-id="4eef1-287">"Path is not valid" error after deploying an Entity Framework Code First application</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-288">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-288">Scenario</span></span>

<span data-ttu-id="4eef1-289">SQL Server Compact、アプリ内のファイルにそのデータベースを格納するなど、Entity Framework Code First Migrations と DBMS を使用するアプリケーションを展開する\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="4eef1-289">You deploy an application that uses Entity Framework Code First Migrations and a DBMS such as SQL Server Compact which stores its database in a file in the App\_Data folder.</span></span> <span data-ttu-id="4eef1-290">Code First Migrations の最初のデプロイ後にデータベースを作成するように構成があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-290">You have Code First Migrations configured to create the database after your first deployment.</span></span> <span data-ttu-id="4eef1-291">アプリケーションを実行するときに、次の例のようなエラー メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-291">When you run the application you get an error message like the following example:</span></span>

<span data-ttu-id="4eef1-292">パスが無効です。</span><span class="sxs-lookup"><span data-stu-id="4eef1-292">The path is not valid.</span></span> <span data-ttu-id="4eef1-293">データベースのディレクトリを確認します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-293">Check the directory for the database.</span></span> <span data-ttu-id="4eef1-294">[Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]</span><span class="sxs-lookup"><span data-stu-id="4eef1-294">[Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-295">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-295">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-296">コードは、データベースが、アプリを作成する試行が最初\_データ フォルダーが存在しません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-296">Code First is attempting to create the database but the App\_Data folder does not exist.</span></span> <span data-ttu-id="4eef1-297">ファイルがあるいないか、*アプリ\_データ*フォルダーを展開したか、選択したときに**を除外するアプリ\_データ**上、**パッケージ化/発行 Web**プロジェクトのプロパティ ウィンドウのタブ。</span><span class="sxs-lookup"><span data-stu-id="4eef1-297">Either you didn't have any files in the *App\_Data* folder when you deployed, or you selected **Exclude App\_Data** on the **Package/Publish Web** tab of the Project Properties window.</span></span> <span data-ttu-id="4eef1-298">サーバーにコピーするフォルダーにファイルがない場合、展開プロセス サーバーのフォルダーは作成されません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-298">The deployment process won't create a folder on the server if there are no files in the folder to be copied to the server.</span></span> <span data-ttu-id="4eef1-299">展開プロセスがファイルを削除、サイトの設定、データベースがある場合、*アプリ\_データ*フォルダー自体を選択した場合**転送先に追加のファイルを削除**で発行プロファイル。</span><span class="sxs-lookup"><span data-stu-id="4eef1-299">If you already had the database set up in the site, the deployment process will delete the files and the *App\_Data* folder itself if you selected **Remove additional files at destination** in the publish profile.</span></span> <span data-ttu-id="4eef1-300">問題を解決するには、内の .txt ファイルなどのプレース ホルダー ファイルを配置、*アプリ\_データ*フォルダーがないことを確認**を除外するアプリ\_データ**を選択し、再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-300">To solve the problem, put a placeholder file such as a .txt file in the *App\_Data* folder, make sure you do not have **Exclude App\_Data** selected, and redeploy.</span></span>

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a><span data-ttu-id="4eef1-301">「基になる RCW から分割された COM オブジェクト使用できません。」</span><span class="sxs-lookup"><span data-stu-id="4eef1-301">"COM object that has been separated from its underlying RCW cannot be used."</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-302">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-302">Scenario</span></span>

<span data-ttu-id="4eef1-303">正常にされている必要が 1 回のクリックを使用してアプリケーションをデプロイする発行開始してこのエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-303">You have been successfully using one-click publish to deploy your application and then you start getting this error:</span></span>

<span data-ttu-id="4eef1-304">Web deploy タスクに失敗しました。</span><span class="sxs-lookup"><span data-stu-id="4eef1-304">Web deployment task failed.</span></span> <span data-ttu-id="4eef1-305">(リモート エージェントの URL に要求を完了できませんでした '<https://serverurl.com/msdeploy.axd?site=sitename>' です)。</span><span class="sxs-lookup"><span data-stu-id="4eef1-305">(Could not complete the request to remote agent URL '<https://serverurl.com/msdeploy.axd?site=sitename>'.)</span></span>  
 <span data-ttu-id="4eef1-306">リモート エージェントの URL に要求を完了できませんでした '<https://url/msdeploy.axd?site=sitename>'。</span><span class="sxs-lookup"><span data-stu-id="4eef1-306">Could not complete the request to remote agent URL '<https://url/msdeploy.axd?site=sitename>'.</span></span>  
<span data-ttu-id="4eef1-307">要求が中止されました: 要求が取り消されました。</span><span class="sxs-lookup"><span data-stu-id="4eef1-307">The request was aborted: The request was canceled.</span></span>  
<span data-ttu-id="4eef1-308">基になる RCW から分割された COM オブジェクトを使用できません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-308">COM object that has been separated from its underlying RCW cannot be used.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-309">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-309">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-310">閉じると、Visual Studio を再起動して、通常このエラーを解決するために必要な。</span><span class="sxs-lookup"><span data-stu-id="4eef1-310">Closing and restarting Visual Studio is usually all that is required to resolve this error.</span></span>

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a><span data-ttu-id="4eef1-311">デプロイが失敗したため、ユーザー資格情報の使用がない公開 setACL 機関</span><span class="sxs-lookup"><span data-stu-id="4eef1-311">Deployment Fails Because User Credentials Used for Publishing Don't Have setACL Authority</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-312">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-312">Scenario</span></span>

<span data-ttu-id="4eef1-313">発行が失敗することを示すエラーは、(ユーザー アカウントを使用している必要はありません setACL 機関) フォルダーのアクセス許可を設定する権限を必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-313">Publishing fails with an error that indicates you don't have authority to set folder permissions (the user account you are using doesn't have setACL authority).</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-314">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-314">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-315">既定では、Visual Studio の設定、サイトのルート フォルダーに対する読み取り権限と書き込みアクセス許可をアプリで\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="4eef1-315">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="4eef1-316">追加することで、この動作を無効にするサイトのフォルダーの既定のアクセス許可が正しいことと、設定する必要はありませんがわかっている場合**&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** (1 つのプロファイルに影響を与える) に発行プロファイル ファイルをまたは (にすべてのプロファイルの影響を与える) wpp.targets ファイル。</span><span class="sxs-lookup"><span data-stu-id="4eef1-316">If you know that the default permissions on site folders are correct and do not need to be set, you disable this behavior by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="4eef1-317">これらのファイルを編集する方法については、次を参照してください。[方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-317">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span>

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a><span data-ttu-id="4eef1-318">アプリケーションがアプリケーション フォルダーへの書き込みを試みると、アクセス拒否エラー</span><span class="sxs-lookup"><span data-stu-id="4eef1-318">Access Denied Errors when the Application Tries to Write to an Application Folder</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-319">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-319">Scenario</span></span>

<span data-ttu-id="4eef1-320">アプリケーション エラーそのフォルダーに対する書き込み権限があるないため、作成、またはアプリケーションのフォルダーのいずれかのファイルを編集しようとします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-320">Your application errors when it tries to create or edit a file in one of the application folders, because it does not have write authority for that folder.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-321">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-321">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-322">既定では、Visual Studio の設定、サイトのルート フォルダーに対する読み取り権限と書き込みアクセス許可をアプリで\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="4eef1-322">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="4eef1-323">アプリケーションでは、サブ フォルダーへの書き込みアクセスを必要とする場合は、このシリーズでは、運用環境のチュートリアルをフォルダーのアクセス許可の設定と展開で示すようにそのフォルダーのアクセス許可を設定できます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-323">If your application needs write access to a sub-folder, you can set permissions for that folder as shown in the Setting Folder Permissions and Deploying to the Production Environment tutorials in this series.</span></span> <span data-ttu-id="4eef1-324">ルート フォルダーに追加することで読み取り専用アクセスを設定することを防ぐことがある場合は、アプリケーションでは、サイトのルート フォルダーへの書き込みアクセスが必要な**&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** (1 つのプロファイルに影響を与える) に発行プロファイル ファイルをまたは (にすべてのプロファイルの影響を与える) wpp.targets ファイル。</span><span class="sxs-lookup"><span data-stu-id="4eef1-324">If your application needs write access to the root folder of the site, you have to prevent it from setting read-only access on the root folder by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="4eef1-325">これらのファイルを編集する方法については、次を参照してください。[方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-325">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span>

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a><span data-ttu-id="4eef1-326">構成エラー - targetFramework 属性は、.NET Framework のインストールされているバージョンより後のバージョンを参照します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-326">Configuration Error - targetFramework attribute references a version that is later than the installed version of the .NET Framework</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-327">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-327">Scenario</span></span>

<span data-ttu-id="4eef1-328">ASP.NET 4.5 を対象とする web プロジェクトを正常に発行したが、次のエラーが発生した (customErrors モードを"off"に、web.config ファイルに設定) をアプリケーションを実行する場合。</span><span class="sxs-lookup"><span data-stu-id="4eef1-328">You successfully published a web project that targets ASP.NET 4.5, but when you run the application (with the customErrors mode set to "off" in the Web.config file) you get the following error:</span></span>

<span data-ttu-id="4eef1-329">'TargetFramework' 属性、&lt;コンパイル&gt;のみをターゲット バージョン 4.0 と .NET Framework の以降の Web.config ファイルの要素が使用されます (たとえば、'&lt;コンパイル targetFramework =「4.0」&gt;').</span><span class="sxs-lookup"><span data-stu-id="4eef1-329">The 'targetFramework' attribute in the &lt;compilation&gt; element of the Web.config file is used only to target version 4.0 and later of the .NET Framework (for example, '&lt;compilation targetFramework="4.0"&gt;').</span></span> <span data-ttu-id="4eef1-330">現在、'targetFramework' 属性は、.NET Framework のインストールされているバージョンより後のバージョンを参照します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-330">The 'targetFramework' attribute currently references a version that is later than the installed version of the .NET Framework.</span></span> <span data-ttu-id="4eef1-331">有効な対象とする .NET Framework のバージョンを指定するか、必要な .NET Framework のバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-331">Specify a valid target version of the .NET Framework, or install the required version of the .NET Framework.</span></span>

<span data-ttu-id="4eef1-332">エラー ページの [ソースのエラー] ボックスは、エラーの原因として、web.config ファイルから次の行を示しています。</span><span class="sxs-lookup"><span data-stu-id="4eef1-332">The Source Error box of the error page highlights the following line from Web.config as the cause of the error:</span></span>

<span data-ttu-id="4eef1-333">&lt;compilation targetFramework="4.5" /&gt;</span><span class="sxs-lookup"><span data-stu-id="4eef1-333">&lt;compilation targetFramework="4.5" /&gt;</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-334">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-334">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-335">サーバーは、ASP.NET 4.5 をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-335">The server does not support ASP.NET 4.5.</span></span> <span data-ttu-id="4eef1-336">ASP.NET 4.5 用のサポートを追加できるかどうかを判断するホスティング プロバイダーにお問い合わせください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-336">Contact the hosting provider to determine when and if support for ASP.NET 4.5 can be added.</span></span> <span data-ttu-id="4eef1-337">ASP.NET 4 またはそれ以前を対象とする web プロジェクトを配置する必要がある場合は、サーバーをアップグレードすることは、オプションではありません、代わりにします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-337">If upgrading the server is not an option, you have to deploy a web project that targets ASP.NET 4 or earlier instead.</span></span>

<span data-ttu-id="4eef1-338">同じ宛先へ、ASP.NET 4 またはそれ以前の web プロジェクトを展開する場合は、選択、**転送先に追加のファイルを削除**チェック ボックスをオン、**設定**のタブ、 **Web の発行**ウィザード。</span><span class="sxs-lookup"><span data-stu-id="4eef1-338">If you deploy an ASP.NET 4 or earlier web project to the same destination, select the **Remove additional files at destination** check box on the **Settings** tab of the **Publish Web** wizard.</span></span> <span data-ttu-id="4eef1-339">選択しない場合**転送先に追加のファイルを削除**、構成エラー ページを取得する続行されます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-339">If you don't select **Remove additional files at destination**, you will continue to get the Configuration Error page.</span></span>

<span data-ttu-id="4eef1-340">プロジェクト**プロパティ**windows には、ターゲット フレームワークのドロップダウン リストが含まれていますが、だけを変更することでこの問題を解決できない **.NET Framework 4.5**に **.NET Framework 4**.</span><span class="sxs-lookup"><span data-stu-id="4eef1-340">The project **Properties** windows includes a Target framework drop-down list, but you can't resolve this problem by just changing that from **.NET Framework 4.5** to **.NET Framework 4**.</span></span> <span data-ttu-id="4eef1-341">以前の framework バージョンをターゲット フレームワークを変更した場合、プロジェクトは framework の以降のバージョンのアセンブリへの参照が残っているし、は実行されません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-341">If you change the target framework to an earlier framework version, the project will still have references to the later framework version's assemblies and will not run.</span></span> <span data-ttu-id="4eef1-342">手動でこれらの参照を変更または .NET Framework 4 以前を対象とする新しいプロジェクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-342">You have to manually change those references or create a new project that targets .NET Framework 4 or earlier.</span></span> <span data-ttu-id="4eef1-343">詳細については、次を参照してください。 [Web サイトの .NET Framework Targeting](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-343">For more information, see [.NET Framework Targeting for Web Sites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).</span></span>

## <a name="medium-trust-errors"></a><span data-ttu-id="4eef1-344">中程度の信頼のエラー</span><span class="sxs-lookup"><span data-stu-id="4eef1-344">Medium Trust Errors</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-345">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-345">Scenario</span></span>

<span data-ttu-id="4eef1-346">運用環境でアプリケーションを実行する場合は、中程度の信頼に関連したエラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-346">When you run your application in production, it gets an error related to medium trust.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-347">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-347">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-348">多くのサード パーティのホスティング プロバイダーは、中程度の信頼があり、行うことが許可されていないものがあることを意味するで、web サイトを実行します。</span><span class="sxs-lookup"><span data-stu-id="4eef1-348">Many third-party hosting providers run your web site in medium trust, which means that there are some things it isn't allowed to do.</span></span> <span data-ttu-id="4eef1-349">たとえば、アプリケーション コードことはできません、Windows レジストリへのアクセスしことはできません読み取りまたは書き込みフォルダー階層のアプリケーションの外部にあるファイル。</span><span class="sxs-lookup"><span data-stu-id="4eef1-349">For example, application code can't access the Windows registry and can't read or write files that are outside of your application's folder hierarchy.</span></span> <span data-ttu-id="4eef1-350">既定では、アプリケーションの実行*完全信頼*ローカル コンピューターにつまりが運用環境にデプロイするときに失敗するための作業を行うことが、アプリケーションである可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-350">By default your application runs in *full trust* on your local computer, which means that the application might be able to do things that would fail when you deploy it to production.</span></span>

<span data-ttu-id="4eef1-351">トラブルシューティングのために、ローカルの IIS 環境での中程度の信頼で実行するアプリケーションを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-351">You can configure the application to run in medium trust in the local IIS environment in order to troubleshoot.</span></span> <span data-ttu-id="4eef1-352">そのためには、アプリケーションを開きます*Web.config*ファイルを開き、追加、**信頼**内の要素、 **system.web**要素は、この例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-352">To do that, open the application *Web.config* file, and add a **trust** element in the **system.web** element, as shown in this example.</span></span>

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

<span data-ttu-id="4eef1-353">アプリケーションは、ローカル コンピューター上でも今すぐ IIS での中程度の信頼で実行されます。</span><span class="sxs-lookup"><span data-stu-id="4eef1-353">The application will now run in medium trust in IIS even on your local computer.</span></span>

<span data-ttu-id="4eef1-354">この場合は Azure が中程度の信頼を要求していないため、Azure App Service にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="4eef1-354">Don't do this if you are deploying to Azure App Service, because Azure does not require medium trust.</span></span> <span data-ttu-id="4eef1-355">このチュートリアルは 2012 年 2 月で記述されている時にアプリケーションを中程度の信頼で実行させるのにはこのメソッドを使用して、エラーが発生 azure。</span><span class="sxs-lookup"><span data-stu-id="4eef1-355">At the time this tutorial is being written in February, 2012, using this method to make your application run in medium trust will cause an error in Azure.</span></span>

<span data-ttu-id="4eef1-356">Entity Framework Code First Migrations を使用しているアプリケーションを中程度の信頼で実行されているホスティング プロバイダーにデプロイする場合は、バージョン 5.0 を行ったことを確認または後でインストールされていること。</span><span class="sxs-lookup"><span data-stu-id="4eef1-356">If you are using Entity Framework Code First Migrations and you are deploying to a hosting provider that runs your application in medium trust, make sure that you have version 5.0 or later installed.</span></span> <span data-ttu-id="4eef1-357">Entity Framework バージョン 4.3 での移行には、データベース スキーマを更新するには完全な信頼が必要です。</span><span class="sxs-lookup"><span data-stu-id="4eef1-357">In Entity Framework version 4.3, Migrations requires full trust in order to update the database schema.</span></span>

## <a name="http-40417-not-found-error"></a><span data-ttu-id="4eef1-358">HTTP 404.17 が見つからないというエラー</span><span class="sxs-lookup"><span data-stu-id="4eef1-358">HTTP 404.17 Not Found Error</span></span>

### <a name="scenario"></a><span data-ttu-id="4eef1-359">シナリオ</span><span class="sxs-lookup"><span data-stu-id="4eef1-359">Scenario</span></span>

<span data-ttu-id="4eef1-360">IIS で開発用コンピューターに展開されたサイトを実行すると、サーバーで Default.aspx を処理できないことを報告する次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-360">When you run the deployed site on your development computer in IIS, you see the following error message reporting that the server can't process Default.aspx:</span></span>

<span data-ttu-id="4eef1-361">HTTP Error 404.17 - が見つかりませんでした。</span><span class="sxs-lookup"><span data-stu-id="4eef1-361">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="4eef1-362">要求されたコンテンツにはスクリプトが表示され、静的ファイル ハンドラーは発生しません。</span><span class="sxs-lookup"><span data-stu-id="4eef1-362">The requested content appears to be script and will not be served by the static file handler.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="4eef1-363">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="4eef1-363">Possible Cause and Solution</span></span>

<span data-ttu-id="4eef1-364">ASP.NET 4.5 をコンピューターにインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4eef1-364">ASP.NET 4.5 might not be installed on your computer.</span></span> <span data-ttu-id="4eef1-365">ASP.NET 4.5 をインストールする方法を説明するこのシリーズではテスト環境のチュートリアルとしては、IIS に展開の手順を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4eef1-365">See the steps in the Deploying to IIS as a Test Environment tutorial in this series that explain how to install ASP.NET 4.5.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4eef1-366">前へ</span><span class="sxs-lookup"><span data-stu-id="4eef1-366">Previous</span></span>](deploying-extra-files.md)
