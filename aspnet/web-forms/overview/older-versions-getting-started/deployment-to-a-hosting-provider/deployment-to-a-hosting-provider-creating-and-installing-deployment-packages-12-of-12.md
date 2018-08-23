---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: (12 は 12) のトラブルシューティング |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2923501289f31243a7341848ed3f7c2142c98e75
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837102"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a><span data-ttu-id="efd0c-103">SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: (12 は 12) のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="efd0c-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Troubleshooting (12 of 12)</span></span>
====================
<span data-ttu-id="efd0c-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="efd0c-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="efd0c-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="efd0c-106">この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="efd0c-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="efd0c-107">Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="efd0c-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="efd0c-108">シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="efd0c-109">Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Windows Azure Web サイトをデプロイする方法を示しますチュートリアルでは、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


<span data-ttu-id="efd0c-110">このページには、Visual Studio を使用して ASP.NET web アプリケーションを展開するときに発生する可能性がある一般的な問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-110">This page describes some common problems that may arise when you deploy an ASP.NET web application by using Visual Studio.</span></span> <span data-ttu-id="efd0c-111">1 つの 1 つ以上の考えられる原因と対応するソリューションは提供されます。</span><span class="sxs-lookup"><span data-stu-id="efd0c-111">For each one, one or more possible causes and corresponding solutions are provided.</span></span>

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a><span data-ttu-id="efd0c-112">サーバー エラーからリモートで表示されている現在のカスタム エラーの設定が、エラーの詳細を防ぐため '/' アプリケーション - で</span><span class="sxs-lookup"><span data-stu-id="efd0c-112">Server Error in '/' Application - Current Custom Error Settings Prevent Details of the Error from Being Viewed Remotely</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-113">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-113">Scenario</span></span>

<span data-ttu-id="efd0c-114">リモート ホストに、サイトを展開した後は、メンションの Web.config ファイルの customErrors 設定が、実際のエラーの原因が示されていませんが、エラー メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-114">After deploying a site to a remote host, you get an error message that mentions the customErrors setting in the Web.config file but doesn't indicate what the actual cause of the error was:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-115">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-115">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-116">既定では、ASP.NET では、web アプリケーションがローカル コンピューターで実行されている場合にのみに詳細なエラー情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="efd0c-116">By default, ASP.NET shows detailed error information only when your web application is running on the local computer.</span></span> <span data-ttu-id="efd0c-117">一般に、web アプリケーションが、ハッカーがこの情報を使用して、アプリケーションの脆弱性を検索することができますので、インターネット経由で公開されている使用可能なときに、詳細なエラー情報を表示したくないです。</span><span class="sxs-lookup"><span data-stu-id="efd0c-117">Generally you don't want to display detailed error information when your web application is publicly available over the Internet, because hackers may be able to use this information to find vulnerabilities in the application.</span></span> <span data-ttu-id="efd0c-118">ただし、サイト、サイトまたは更新プログラムを展開する場合に場合があります何かが悪化して実際のエラー メッセージを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-118">However, when you are deploying a site or updates to a site, sometimes something will go wrong and you need to get the actual error message.</span></span>

<span data-ttu-id="efd0c-119">リモート ホストで実行されるときに、詳細なエラー メッセージを表示するアプリケーションを有効にするには、設定を Web.config ファイルを編集`customErrors`モードのオフ、アプリケーションを再デプロイ、およびアプリケーションを再度実行します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-119">To enable the application to display detailed error messages when it runs on the remote host, edit the Web.config file to set `customErrors` mode off, redeploy the application, and run the application again:</span></span>

1. <span data-ttu-id="efd0c-120">アプリケーションの Web.config ファイルがある場合、`customErrors`内の要素、`system.web`要素、変更、`mode`属性を「オフ」にします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-120">If the application Web.config file has a `customErrors` element in the `system.web` element, change the `mode` attribute to "off".</span></span> <span data-ttu-id="efd0c-121">それ以外の場合、追加、`customErrors`内の要素、`system.web`を持つ要素、`mode`次の例に示すように属性が"off"に設定。</span><span class="sxs-lookup"><span data-stu-id="efd0c-121">Otherwise add a `customErrors` element in the `system.web` element with the `mode` attribute set to "off", as shown in the following example:</span></span>

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. <span data-ttu-id="efd0c-122">アプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-122">Deploy the application.</span></span>
3. <span data-ttu-id="efd0c-123">アプリケーションを実行し、どの前に行ったエラーが発生する原因となったを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-123">Run the application and repeat whatever you did earlier that caused the error to occur.</span></span> <span data-ttu-id="efd0c-124">ここでは、実際のエラー メッセージを確認できます。</span><span class="sxs-lookup"><span data-stu-id="efd0c-124">Now you can see what the actual error message is.</span></span>
4. <span data-ttu-id="efd0c-125">エラーが解決されると、復元元`customErrors`を設定し、アプリケーションを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-125">When you have resolved the error, restore the original `customErrors` setting and redeploy the application.</span></span>

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a><span data-ttu-id="efd0c-126">SQL Server の使用を最適化することで、Web ページ アクセスが拒否されました</span><span class="sxs-lookup"><span data-stu-id="efd0c-126">Access is Denied in a Web Page that Uses SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-127">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-127">Scenario</span></span>

<span data-ttu-id="efd0c-128">SQL Server Compact を使用するサイトを展開すると、データベースにアクセスするデプロイされたサイトのページを実行する、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-128">When you deploy a site that uses SQL Server Compact and you run a page in the deployed site that accesses the database, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-129">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-129">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-130">サーバー上のネットワーク サービス アカウントがされている SQL Service Compact のネイティブ バイナリを読めるようにする必要があります、 *bin\amd64*または*bin\x86*フォルダーでは、これは読み取り権限がないそれらのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="efd0c-130">The NETWORK SERVICE account on the server needs to be able to read SQL Service Compact native binaries that are in the *bin\amd64* or *bin\x86* folder, but it does not have read permissions for those folders.</span></span> <span data-ttu-id="efd0c-131">ネットワーク サービスのアクセス許可の読み取りの設定、 *bin*フォルダー、サブフォルダーへのアクセス許可を拡張することを確認します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-131">Set read permission for NETWORK SERVICE on the *bin* folder, making sure to extend the permissions to subfolders.</span></span>

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a><span data-ttu-id="efd0c-132">十分なアクセス許可により、構成ファイルを読み取ることができません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-132">Cannot Read Configuration File Due to Insufficient Permissions</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-133">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-133">Scenario</span></span>

<span data-ttu-id="efd0c-134">クリックすると、Visual Studio の発行、ローカル コンピューター上の IIS にアプリケーションを配置する発行が失敗して、**出力**ウィンドウは次のようなエラー メッセージを表示。</span><span class="sxs-lookup"><span data-stu-id="efd0c-134">When you click the Visual Studio publish button to deploy an application to IIS on your local machine, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-135">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-135">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-136">1 回のクリックを使用するローカル コンピューター上の IIS に発行、管理者のアクセス許可を持つ Visual Studio を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-136">To use one-click publish to IIS on your local machine, you must be running Visual Studio with administrator permissions.</span></span> <span data-ttu-id="efd0c-137">Visual Studio を閉じてから、管理者権限で再起動します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-137">Close Visual Studio and restart it with administrator permissions.</span></span>

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a><span data-ttu-id="efd0c-138">対象のコンピューターに接続できませんでした.指定されたプロセスを使用します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-138">Could Not Connect to the Destination Computer ... Using the Specified Process</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-139">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-139">Scenario</span></span>

<span data-ttu-id="efd0c-140">クリックすると、Visual Studio の発行、アプリケーションの展開ボタン発行が失敗して、**出力**ウィンドウは次のようなエラー メッセージを表示。</span><span class="sxs-lookup"><span data-stu-id="efd0c-140">When you click the Visual Studio publish button to deploy an application, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-141">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-141">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-142">プロキシ サーバーは、移行先サーバーとの通信を中断することです。</span><span class="sxs-lookup"><span data-stu-id="efd0c-142">A proxy server is interrupting communication with the destination server.</span></span> <span data-ttu-id="efd0c-143">Windows コントロール パネルから、または Internet Explorer で、選択**インターネット オプション**を選択し、**接続**タブ。**インターネット プロパティ**ダイアログ ボックスで、をクリックして**LAN の設定**します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-143">From the Windows Control Panel or in Internet Explorer, select **Internet Options** and select the **Connections** tab. In the **Internet Properties** dialog box, click **LAN Settings**.</span></span> <span data-ttu-id="efd0c-144">**ローカル エリア ネットワーク (LAN) 設定** ダイアログ ボックスで、クリア、**設定を自動的に検出**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-144">In the **Local Area Network (LAN) Settings** dialog box, clear the **Automatically detect settings** checkbox.</span></span> <span data-ttu-id="efd0c-145">[発行] ボタンを再度クリックします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-145">Then click the publish button again.</span></span>

<span data-ttu-id="efd0c-146">問題が解決しない場合は、プロキシまたはファイアウォールの設定で何ができる、システム管理者に問い合わせてください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-146">If the problem persists, contact your system administrator to determine what can be done with proxy or firewall settings.</span></span> <span data-ttu-id="efd0c-147">Web Deploy Web 管理サービスの展開 (8172); の非標準ポートを使用するために、問題が発生します。他の接続では、Web Deploy はポート 80 を使用します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-147">The problem happens because Web Deploy uses a non-standard port for Web Management Service deployment (8172); for other connections, Web Deploy uses port 80.</span></span> <span data-ttu-id="efd0c-148">サード パーティのホスティング プロバイダーに展開する場合に通常 Web 管理サービスを使用しています。</span><span class="sxs-lookup"><span data-stu-id="efd0c-148">When you are deploying to a third-party hosting provider, you are typically using the Web Management Service.</span></span>

## <a name="default-net-40-application-pool-does-not-exist"></a><span data-ttu-id="efd0c-149">既定の .NET 4.0 のアプリケーション プールが存在しません</span><span class="sxs-lookup"><span data-stu-id="efd0c-149">Default .NET 4.0 Application Pool Does Not Exist</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-150">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-150">Scenario</span></span>

<span data-ttu-id="efd0c-151">.NET Framework 4 が必要なアプリケーションを展開するときに、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-151">When you deploy an application that requires the .NET Framework 4, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-152">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-152">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-153">IIS では、ASP.NET 4 がインストールされていません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-153">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="efd0c-154">配置するサーバーは、開発用コンピューターに Visual Studio 2010 をインストールする場合は、ASP.NET 4 がコンピューターにインストールされますが、IIS にインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-154">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="efd0c-155">展開して、サーバーでコマンド プロンプトを開くし、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-155">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

<span data-ttu-id="efd0c-156">また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-156">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="efd0c-157">詳細については、次を参照してください。、[テスト環境として IIS への配置](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="efd0c-157">For more information, see the [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.</span></span>

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a><span data-ttu-id="efd0c-158">初期化文字列の形式は、インデックス 0 から始まる位置の仕様に準拠していません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-158">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-159">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-159">Scenario</span></span>

<span data-ttu-id="efd0c-160">1 回のクリックを使用してアプリケーションを展開した後の発行、次のエラー メッセージを取得したデータベースにアクセスするページを実行するとき。</span><span class="sxs-lookup"><span data-stu-id="efd0c-160">After you deploy an application using one-click publish, when you run a page that accesses the database you get the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-161">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-161">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-162">開く、 *Web.config*で展開されたサイトを確認するかどうか、接続文字列値で始まるファイル`$(ReplacableToken_`、次の例。</span><span class="sxs-lookup"><span data-stu-id="efd0c-162">Open the *Web.config* file in the deployed site and check to see whether the connection string values begin with `$(ReplacableToken_`, as in the following example:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

<span data-ttu-id="efd0c-163">接続文字列は、この例のような場合は、プロジェクト ファイルを編集し、次のプロパティを追加、`PropertyGroup`はすべてのビルド構成の要素。</span><span class="sxs-lookup"><span data-stu-id="efd0c-163">If the connection strings look like this example, edit the project file and add the following property to the `PropertyGroup` element that is for all build configurations:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

<span data-ttu-id="efd0c-164">アプリケーションを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-164">Then redeploy the application.</span></span>

## <a name="http-500-internal-server-error"></a><span data-ttu-id="efd0c-165">HTTP 500 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="efd0c-165">HTTP 500 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-166">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-166">Scenario</span></span>

<span data-ttu-id="efd0c-167">デプロイされたサイトを実行するとは、エラーの原因を示す特定の情報がない場合、次のエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-167">When you run the deployed site, you see the following error message without specific information indicating the cause of the error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-168">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-168">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-169">500 のエラーの原因の多くがありますが、これらのチュートリアルに従っている場合に 1 つの考えられる原因は、XML 変換ファイルのいずれかで間違った場所に XML 要素を配置すること。</span><span class="sxs-lookup"><span data-stu-id="efd0c-169">There are many causes of 500 errors, but one possible cause if you are following these tutorials is that you put an XML element in the wrong place in one of the XML transformation files.</span></span> <span data-ttu-id="efd0c-170">挿入する変換を配置する場合にこのエラーが得られるなど、`<location>`要素 `<system.web>`の代わりの直下にある`<configuration>`します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-170">For example, you would get this error if you put the transformation that inserts a `<location>` element under `<system.web>` instead of directly under `<configuration>`.</span></span> <span data-ttu-id="efd0c-171">ソリューションは、XML の変換ファイルを修正して再デプロイする場合は。</span><span class="sxs-lookup"><span data-stu-id="efd0c-171">The solution in that case is to correct the XML transformation file and redeploy.</span></span>

## <a name="http-50021-internal-server-error"></a><span data-ttu-id="efd0c-172">HTTP 500.21 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="efd0c-172">HTTP 500.21 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-173">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-173">Scenario</span></span>

<span data-ttu-id="efd0c-174">デプロイされたサイトを実行すると、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-174">When you run the deployed site, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-175">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-175">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-176">サイトが、ASP.NET 4、ASP.NET 4 に登録されていない IIS サーバー上のターゲットを導入しています。</span><span class="sxs-lookup"><span data-stu-id="efd0c-176">The site you have deployed targets ASP.NET 4, but ASP.NET 4 is not registered in IIS on the server.</span></span> <span data-ttu-id="efd0c-177">サーバーで管理者特権のコマンド プロンプトを開き、次のコマンドを実行して、ASP.NET 4 を登録します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-177">On the server open an elevated command prompt and register ASP.NET 4 by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

<span data-ttu-id="efd0c-178">また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-178">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="efd0c-179">詳細については、次を参照してください。、[テスト環境として IIS への配置](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="efd0c-179">For more information, see the [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.</span></span>

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a><span data-ttu-id="efd0c-180">アプリには、SQL Server Express のデータベースを開くはログインできませんでした\_データ</span><span class="sxs-lookup"><span data-stu-id="efd0c-180">Login Failed Opening SQL Server Express Database in App\_Data</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-181">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-181">Scenario</span></span>

<span data-ttu-id="efd0c-182">更新する、 *Web.config*ファイルとしての SQL Server Express データベースを指す接続文字列、 *.mdf*ファイル、*アプリ\_データ*フォルダー、および最初次のエラー メッセージが表示アプリケーションを実行する時刻:</span><span class="sxs-lookup"><span data-stu-id="efd0c-182">You updated the *Web.config* file connection string to point to a SQL Server Express database as an *.mdf* file in your *App\_Data* folder, and the first time you run the application you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-183">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-183">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-184">名前、 *.mdf*削除した場合でも、ファイルがコンピューターには、これまで存在していた SQL Server Express のデータベースの名前を一致ことはできません、 *.mdf*既存のデータベースのファイル。</span><span class="sxs-lookup"><span data-stu-id="efd0c-184">The name of the *.mdf* file cannot match the name of any SQL Server Express database that has ever existed on your computer, even if you deleted the *.mdf* file of the previously existing database.</span></span> <span data-ttu-id="efd0c-185">名前を変更、 *.mdf*ファイル変更とデータベースの名前として使用されていない名前を*Web.config*ファイルを新しい名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-185">Change the name of the *.mdf* file to a name that has never been used as a database name and change the *Web.config* file to use the new name.</span></span> <span data-ttu-id="efd0c-186">代わりに、使用することができます[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)データベースを以前の既存の SQL Server Express を削除します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-186">As an alternative, you can use [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete previously existing SQL Server Express databases.</span></span>

## <a name="model-compatibility-cannot-be-checked"></a><span data-ttu-id="efd0c-187">チェックするモデルの互換性ことはできません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-187">Model Compatibility Cannot be Checked</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-188">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-188">Scenario</span></span>

<span data-ttu-id="efd0c-189">更新する、 *Web.config*ファイルを新しい SQL Server Express データベースを指す接続文字列と、初めてアプリケーションを実行する次のエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-189">You updated the *Web.config* file connection string to point to a new SQL Server Express database, and the first time you run the application you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-190">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-190">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-191">場合に、Web.config ファイルを配置するデータベース名がコンピューターには、データベースが既に存在するいくつかのテーブルを含む前にこれまで使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd0c-191">If the database name you put in the Web.config file was ever used before on your computer, a database might already exist with some tables in it.</span></span> <span data-ttu-id="efd0c-192">前に、のコンピューターと変更に使用されていない新しい名前を選択、 *Web.config*ファイルをポイントして、この新しいデータベース名を使用します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-192">Select a new name that has not been used on your computer before and change the *Web.config* file to point to use this new database name.</span></span> <span data-ttu-id="efd0c-193">代わりに、使用することができます[SQL Server Express ユーティリティ](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)または[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)を既存のデータベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-193">As an alternative, you can use [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) or [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete the existing database.</span></span>

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a><span data-ttu-id="efd0c-194">スクリプトがユーザーまたはロールを作成しようとしたときに、SQL エラー</span><span class="sxs-lookup"><span data-stu-id="efd0c-194">SQL Error When a Script Attempts to Create Users or Roles</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-195">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-195">Scenario</span></span>

<span data-ttu-id="efd0c-196">構成されているデータベースの配置を使用している、**パッケージ化/発行 SQL**  タブで、デプロイ時に実行される SQL スクリプトなどのユーザーの作成またはロールの作成のコマンドをスクリプトの実行が失敗したこれらのコマンドを実行するとします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-196">You are using database deployment configured on the **Package/Publish SQL** tab, SQL scripts that run during deployment include Create User or Create Role commands, and script execution fails when those commands are executed.</span></span> <span data-ttu-id="efd0c-197">次のように、メッセージの詳細を参照して可能性があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-197">You might see more detailed messages, such as the following:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

<span data-ttu-id="efd0c-198">データベースの配置を構成したときにこのエラーが発生、 **Web の発行**ウィザードではなく、**パッケージ化/発行 SQL**  タブでスレッドを作成、[構成し、展開](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)フォーラム、およびソリューションは、このトラブルシューティングのページに追加されます。</span><span class="sxs-lookup"><span data-stu-id="efd0c-198">If this error occurs when you have configured database deployment in the **Publish Web** wizard rather than the **Package/Publish SQL** tab, create a thread in the [Configuration and Deployment](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum, and the solution will be added to this troubleshooting page.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-199">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-199">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-200">展開の実行を使用しているユーザー アカウントには、ユーザーまたはロールを作成するアクセス許可がありません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-200">The user account you are using to perform deployment does not have permission to create users or roles.</span></span> <span data-ttu-id="efd0c-201">たとえば、ホスティング企業が割り当てることができます、 `db_datareader`、 `db_datawriter`、および`db_ddladmin`ロールを設定するユーザー アカウントにします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-201">For example, the hosting company might assign the `db_datareader`, `db_datawriter`, and `db_ddladmin` roles to the user account that it sets up for you.</span></span> <span data-ttu-id="efd0c-202">これらは、十分なユーザーまたはロールの作成ではなく、ほとんどのデータベース オブジェクトを作成するためです。</span><span class="sxs-lookup"><span data-stu-id="efd0c-202">These are sufficient for creating most database objects, but not for creating users or roles.</span></span> <span data-ttu-id="efd0c-203">エラーを回避する方法の 1 つは、データベースの配置からのユーザーとロールを除外することです。</span><span class="sxs-lookup"><span data-stu-id="efd0c-203">One way to avoid the error is by excluding users and roles from database deployment.</span></span> <span data-ttu-id="efd0c-204">編集することによってこれを行う、`PreSource`次の属性が含まれるように、データベースの要素のスクリプトの生成をされた自動的に。</span><span class="sxs-lookup"><span data-stu-id="efd0c-204">You can do this by editing the `PreSource` element for the database's automatically generated script so that it includes the following attributes:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

<span data-ttu-id="efd0c-205">編集する方法については、 `PreSource` 、プロジェクト ファイル内の要素を参照してください[方法: プロジェクト ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-205">For information about how to edit the `PreSource` element in the project file, see [How to: Edit Deployment Settings in the Project File](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx).</span></span> <span data-ttu-id="efd0c-206">ユーザーまたはロールは、開発用データベースでは、移行先データベースに存在する必要がある場合、は、ホスティング プロバイダーに問い合わせてしてください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-206">If the users or roles in your development database need to be in the destination database, contact your hosting provider for assistance.</span></span>

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a><span data-ttu-id="efd0c-207">デプロイ時にカスタム スクリプトを実行するときに、SQL Server のタイムアウト エラー</span><span class="sxs-lookup"><span data-stu-id="efd0c-207">SQL Server Timeout Error When Running Custom Scripts During Deployment</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-208">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-208">Scenario</span></span>

<span data-ttu-id="efd0c-209">デプロイ時に、実行するカスタムの SQL スクリプトを指定したし、タイムアウトと Web 配置を実行して、します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-209">You have specified custom SQL scripts to run during deployment, and when Web Deploy runs them, they time out.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-210">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-210">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-211">別のトランザクション モードを持つ複数のスクリプトを実行すると、タイムアウト エラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="efd0c-211">Running multiple scripts that have different transaction modes can cause time-out errors.</span></span> <span data-ttu-id="efd0c-212">既定では、自動的に生成されたスクリプトは、トランザクションで実行しますが、カスタム スクリプトはありません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-212">By default, automatically generated scripts run in a transaction, but custom scripts do not.</span></span> <span data-ttu-id="efd0c-213">選択した場合、**データや既存のデータベースからスキーマをプル**オプション、**パッケージ化/発行 SQL**  タブで、カスタム SQL スクリプトを追加する場合は、いくつかのスクリプトでのトランザクション設定を変更する必要がありますようにすべてのスクリプトでは、同じトランザクションの設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-213">If you select the **Pull data and/or schema from an existing database** option on the **Package/Publish SQL** tab, and if you add a custom SQL script, you must change transaction settings on some scripts so that all scripts use the same transaction settings.</span></span> <span data-ttu-id="efd0c-214">詳細については、次を参照してください。[方法: Web アプリケーション プロジェクトでのデータベース配置](https://msdn.microsoft.com/library/dd465343.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-214">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343.aspx).</span></span>

<span data-ttu-id="efd0c-215">すべてが同じになるように、トランザクションの設定を構成したが、このエラーが引き続き発生すると場合、考えられる回避策とは別にスクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-215">If you have configured transaction settings so that all are the same but still get this error, a possible workaround is to run the scripts separately.</span></span> <span data-ttu-id="efd0c-216">**データベース スクリプト**グリッドで、**パッケージ化/発行**SQL タブで、、 **Include**タイムアウト エラーが発生するスクリプトのチェック ボックスは、プロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-216">In the **Database Scripts** grid in the **Package/Publish** SQL tab, clear the **Include** check box for the script that causes the timeout error, then publish the project.</span></span> <span data-ttu-id="efd0c-217">移動し、**データベース スクリプト**グリッド、スクリプトを選択します**Include**チェック ボックスをオンし、オフ、 **Include**他のスクリプトのチェック ボックスを。</span><span class="sxs-lookup"><span data-stu-id="efd0c-217">Then go back into the **Database Scripts** grid, select that script's **Include** check box, and clear the **Include** check boxes for the other scripts.</span></span> <span data-ttu-id="efd0c-218">プロジェクトをもう一度発行します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-218">Then publish the project again.</span></span> <span data-ttu-id="efd0c-219">この時間を発行すると、選択したカスタム スクリプトのみが実行されます。</span><span class="sxs-lookup"><span data-stu-id="efd0c-219">This time when you publish, only the selected custom script runs.</span></span>

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a><span data-ttu-id="efd0c-220">サイトのマニフェストの Stream データはまだ使用できません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-220">Stream Data of Site Manifest Is Not Yet Available</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-221">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-221">Scenario</span></span>

<span data-ttu-id="efd0c-222">使用してパッケージをインストールする場合、 *deploy.cmd*ファイルと、 `t` (テスト) オプションでは、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-222">When you are installing a package using the *deploy.cmd* file with the `t` (test) option, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-223">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-223">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-224">エラー メッセージでは、コマンドがテスト レポートを生成できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-224">The error message means that the command cannot produce a test report.</span></span> <span data-ttu-id="efd0c-225">ただし、コマンド実行可能性がありますを使用する、 `y` (実際のインストール) オプション。</span><span class="sxs-lookup"><span data-stu-id="efd0c-225">However, the command might run if you use the `y` (actual installation) option.</span></span> <span data-ttu-id="efd0c-226">メッセージは、テスト モードでコマンドを実行に問題があることにのみを示します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-226">The message indicates only that there is a problem with running the command in test mode.</span></span>

## <a name="this-application-requires-managedruntimeversion-v40"></a><span data-ttu-id="efd0c-227">このアプリケーションに ManagedRuntimeVersion v4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="efd0c-227">This Application Requires ManagedRuntimeVersion v4.0</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-228">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-228">Scenario</span></span>

<span data-ttu-id="efd0c-229">デプロイしようとすると、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-229">When you attempt to deploy, you see the following error message:</span></span>

 <span data-ttu-id="efd0c-230">エラー: のストリーム データが、' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' はまだ使用できません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-230">Error: The stream data of 'sitemanifest/dbFullSql[@path='C:\TEMP\AdventureWorksGrant.sql']/sqlScript' is not yet available.</span></span> <span data-ttu-id="efd0c-231">使用しようとしているアプリケーション プールが、'managedRuntimeVersion' プロパティが 'v2.0' に設定します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-231">The application pool that you are trying to use has the 'managedRuntimeVersion' property set to 'v2.0'.</span></span> <span data-ttu-id="efd0c-232">このアプリケーションでは、'v4.0' が必要です。</span><span class="sxs-lookup"><span data-stu-id="efd0c-232">This application requires 'v4.0'.</span></span> 

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-233">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-233">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-234">IIS では、ASP.NET 4 がインストールされていません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-234">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="efd0c-235">配置するサーバーは、開発用コンピューターに Visual Studio 2010 をインストールする場合は、ASP.NET 4 がコンピューターにインストールされますが、IIS にインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-235">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="efd0c-236">展開して、サーバーでコマンド プロンプトを開くし、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-236">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a><span data-ttu-id="efd0c-237">Microsoft.Web.Deployment.DeploymentProviderOptions をキャストできません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-237">Unable to cast Microsoft.Web.Deployment.DeploymentProviderOptions</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-238">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-238">Scenario</span></span>

<span data-ttu-id="efd0c-239">パッケージを展開する場合に、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-239">When you are deploying a package, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-240">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-240">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-241">Web Deploy 2.0 がインストールされているサーバーに Web デプロイ 1.1 UI を使用して IIS マネージャーからのデプロイしようとしました。</span><span class="sxs-lookup"><span data-stu-id="efd0c-241">You are trying to deploy from IIS Manager using the Web Deploy 1.1 UI to a server that has Web Deploy 2.0 installed.</span></span> <span data-ttu-id="efd0c-242">チェック、パッケージをインポートして展開する IIS のリモート管理ツールを使用している場合、**機能利用可能な新しい** ダイアログ ボックス、接続を確立するときにします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-242">If you are using the IIS Remote Administration Tool to deploy by importing a package, check the **New Features Available** dialog box when you establish the connection.</span></span> <span data-ttu-id="efd0c-243">(このダイアログ ボックスがありますのみ表示されます 1 回、接続が最初に確立されたときに。</span><span class="sxs-lookup"><span data-stu-id="efd0c-243">(This dialog box might only be shown once when the connection is first established.</span></span> <span data-ttu-id="efd0c-244">接続を消去し、最初からやり直す、IIS マネージャーを閉じます、起動もう一度入力して`inetmgr /reset`コマンド プロンプトでします)。機能のいずれかが表示されている場合は、 **Web デプロイ UI**、8 よりも低いバージョン番号があるを展開して、サーバーは 1.1 と 2.0 の両方のバージョンの Web 配置がインストールされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-244">To clear the connection and start over, close IIS Manager and start it up again by entering `inetmgr /reset` at the command prompt.) If one of the features listed is **Web Deploy UI**, and it has a version number lower than 8, the server you are deploying to might have both 1.1 and 2.0 versions of Web Deploy installed.</span></span> <span data-ttu-id="efd0c-245">2.0 をインストールしているクライアントからのデプロイ、サーバーにのみ Web Deploy 2.0 をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-245">To deploy from a client that has 2.0 installed, the server must have only Web Deploy 2.0 installed.</span></span> <span data-ttu-id="efd0c-246">この問題を解決するのには、ホスティング プロバイダーに連絡する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-246">You will have to contact your hosting provider to resolve this problem.</span></span>

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a><span data-ttu-id="efd0c-247">SQL Server Compact のネイティブ コンポーネントを読み込めませんでした。</span><span class="sxs-lookup"><span data-stu-id="efd0c-247">Unable to load the native components of SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-248">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-248">Scenario</span></span>

<span data-ttu-id="efd0c-249">デプロイされたサイトを実行すると、次のエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-249">When you run the deployed site, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-250">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-250">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-251">展開されたサイトがない*amd64*と*x86*アプリケーションの下にネイティブ アセンブリを含むサブフォルダー *bin*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efd0c-251">The deployed site does not have *amd64* and *x86* subfolders with the native assemblies in them under the application's *bin* folder.</span></span> <span data-ttu-id="efd0c-252">SQL Server Compact がインストールされているコンピューターで、ネイティブ アセンブリ内にある*C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private*します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-252">On a computer that has SQL Server Compact installed, the native assemblies are located in *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*.</span></span> <span data-ttu-id="efd0c-253">Visual Studio プロジェクトに適切なフォルダーに正しいファイルを取得する最善の方法では、NuGet SqlServerCompact パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-253">The best way to get the correct files into the correct folders in a Visual Studio project is to install the NuGet SqlServerCompact package.</span></span> <span data-ttu-id="efd0c-254">パッケージのインストールにネイティブ アセンブリをコピーするビルド後のスクリプトを追加します*amd64*と*x86*します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-254">Package installation adds a post-build script to copy the native assemblies into *amd64* and *x86*.</span></span> <span data-ttu-id="efd0c-255">これらを展開するためには、ただし、手動で、プロジェクトに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-255">In order for these to be deployed, however, you have to manually include them in the project.</span></span> <span data-ttu-id="efd0c-256">詳細については、次を参照してください。、[を展開する SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="efd0c-256">For more information, see the [Deploying SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a><span data-ttu-id="efd0c-257">Entity Framework Code First アプリケーションのデプロイ後にエラーが「パスが無効です」</span><span class="sxs-lookup"><span data-stu-id="efd0c-257">"Path is not valid" error after deploying an Entity Framework Code First application</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-258">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-258">Scenario</span></span>

<span data-ttu-id="efd0c-259">SQL Server Compact、アプリ内のファイルにそのデータベースを格納するなど、Entity Framework Code First Migrations と DBMS を使用するアプリケーションを展開する\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efd0c-259">You deploy an application that uses Entity Framework Code First Migrations and a DBMS such as SQL Server Compact which stores its database in a file in the App\_Data folder.</span></span> <span data-ttu-id="efd0c-260">Code First Migrations の最初のデプロイ後にデータベースを作成するように構成があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-260">You have Code First Migrations configured to create the database after your first deployment.</span></span> <span data-ttu-id="efd0c-261">アプリケーションを実行するときに、次の例のようなエラー メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-261">When you run the application you get an error message like the following example:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-262">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-262">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-263">コードは、データベースが、アプリを作成する試行が最初\_データ フォルダーが存在しません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-263">Code First is attempting to create the database but the App\_Data folder does not exist.</span></span> <span data-ttu-id="efd0c-264">ファイルがあるいないか、*アプリ\_データ*フォルダーを展開したか、選択したときに**を除外するアプリ\_データ**上、**パッケージ化/発行 Web**のタブ、**プロジェクト プロパティ**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="efd0c-264">Either you didn't have any files in the *App\_Data* folder when you deployed, or you selected **Exclude App\_Data** on the **Package/Publish Web** tab of the **Project Properties** window.</span></span> <span data-ttu-id="efd0c-265">サーバーにコピーするフォルダーにファイルがない場合、展開プロセス サーバーのフォルダーは作成されません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-265">The deployment process won't create a folder on the server if there are no files in the folder to be copied to the server.</span></span> <span data-ttu-id="efd0c-266">展開プロセスがファイルを削除、サイトの設定、データベースがある場合、*アプリ\_データ*フォルダー自体を選択した場合**転送先に追加のファイルを削除**で発行プロファイル。</span><span class="sxs-lookup"><span data-stu-id="efd0c-266">If you already had the database set up in the site, the deployment process will delete the files and the *App\_Data* folder itself if you selected **Remove additional files at destination** in the publish profile.</span></span> <span data-ttu-id="efd0c-267">問題を解決するには、内の .txt ファイルなどのプレース ホルダー ファイルを配置、*アプリ\_データ*フォルダーがないことを確認**を除外するアプリ\_データ**を選択し、再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-267">To solve the problem, put a placeholder file such as a .txt file in the *App\_Data* folder, make sure you do not have **Exclude App\_Data** selected, and redeploy.</span></span> 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a><span data-ttu-id="efd0c-268">「基になる RCW から分割された COM オブジェクト使用できません。」</span><span class="sxs-lookup"><span data-stu-id="efd0c-268">"COM object that has been separated from its underlying RCW cannot be used."</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-269">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-269">Scenario</span></span>

<span data-ttu-id="efd0c-270">正常にされている必要が 1 回のクリックを使用してアプリケーションをデプロイする発行開始してこのエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-270">You have been successfully using one-click publish to deploy your application and then you start getting this error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-271">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-271">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-272">閉じると、Visual Studio を再起動して、通常このエラーを解決するために必要な。</span><span class="sxs-lookup"><span data-stu-id="efd0c-272">Closing and restarting Visual Studio is usually all that is required to resolve this error.</span></span>

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a><span data-ttu-id="efd0c-273">デプロイが失敗したため、ユーザー資格情報の使用がない公開 setACL 機関</span><span class="sxs-lookup"><span data-stu-id="efd0c-273">Deployment Fails Because User Credentials Used for Publishing Don't Have setACL Authority</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-274">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-274">Scenario</span></span>

<span data-ttu-id="efd0c-275">発行が失敗することを示すエラーは、(ユーザー アカウントを使用している必要はありません setACL 機関) フォルダーのアクセス許可を設定する権限を必要はありません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-275">Publishing fails with an error that indicates you don't have authority to set folder permissions (the user account you are using doesn't have setACL authority).</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-276">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-276">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-277">既定では、Visual Studio の設定、サイトのルート フォルダーに対する読み取り権限と書き込みアクセス許可をアプリで\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efd0c-277">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="efd0c-278">追加することで、この動作を無効にするサイトのフォルダーの既定のアクセス許可が正しいことと、設定する必要はありませんがわかっている場合**&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** (1 つのプロファイルに影響を与える) に発行プロファイル ファイルをまたは (にすべてのプロファイルの影響を与える) wpp.targets ファイル。</span><span class="sxs-lookup"><span data-stu-id="efd0c-278">If you know that the default permissions on site folders are correct and do not need to be set, you disable this behavior by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="efd0c-279">これらのファイルを編集する方法については、次を参照してください。[方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-279">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span> 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a><span data-ttu-id="efd0c-280">アプリケーションがアプリケーション フォルダーへの書き込みを試みると、アクセス拒否エラー</span><span class="sxs-lookup"><span data-stu-id="efd0c-280">Access Denied Errors when the Application Tries to Write to an Application Folder</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-281">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-281">Scenario</span></span>

<span data-ttu-id="efd0c-282">アプリケーション エラーそのフォルダーに対する書き込み権限があるないため、作成、またはアプリケーションのフォルダーのいずれかのファイルを編集しようとします。</span><span class="sxs-lookup"><span data-stu-id="efd0c-282">Your application errors when it tries to create or edit a file in one of the application folders, because it does not have write authority for that folder.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-283">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-283">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-284">既定では、Visual Studio の設定、サイトのルート フォルダーに対する読み取り権限と書き込みアクセス許可をアプリで\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efd0c-284">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="efd0c-285">ように、そのフォルダーのアクセス許可に設定するアプリケーションでは、サブ フォルダーへの書き込みアクセスを必要とする場合、[フォルダーのアクセス許可の設定](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)と[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="efd0c-285">If your application needs write access to a sub-folder, you can set permissions for that folder as shown in the [Setting Folder Permissions](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) and [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorials.</span></span> <span data-ttu-id="efd0c-286">ルート フォルダーに追加することで読み取り専用アクセスを設定することを防ぐことがある場合は、アプリケーションでは、サイトのルート フォルダーへの書き込みアクセスが必要な**&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** (1 つのプロファイルに影響を与える) に発行プロファイル ファイルをまたは (にすべてのプロファイルの影響を与える) wpp.targets ファイル。</span><span class="sxs-lookup"><span data-stu-id="efd0c-286">If your application needs write access to the root folder of the site, you have to prevent it from setting read-only access on the root folder by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="efd0c-287">これらのファイルを編集する方法については、次を参照してください。[方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-287">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span> <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a><span data-ttu-id="efd0c-288">構成エラー - targetFramework 属性は、.NET Framework のインストールされているバージョンより後のバージョンを参照します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-288">Configuration Error - targetFramework attribute references a version that is later than the installed version of the .NET Framework</span></span>

### <a name="scenario"></a><span data-ttu-id="efd0c-289">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd0c-289">Scenario</span></span>

<span data-ttu-id="efd0c-290">ASP.NET 4.5 を対象とする web プロジェクトを正常に発行されたアプリケーションを実行する場合は (で、`customErrors`モードは、Web.config ファイルには、"off"に設定)、次のエラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-290">You successfully published a web project that targets ASP.NET 4.5, but when you run the application (with the `customErrors` mode set to "off" in the Web.config file) you get the following error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

<span data-ttu-id="efd0c-291">エラー ページの [ソースのエラー] ボックスは、エラーの原因として、web.config ファイルから次の行を示しています。</span><span class="sxs-lookup"><span data-stu-id="efd0c-291">The Source Error box of the error page highlights the following line from Web.config as the cause of the error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="efd0c-292">考えられる原因とソリューション</span><span class="sxs-lookup"><span data-stu-id="efd0c-292">Possible Cause and Solution</span></span>

<span data-ttu-id="efd0c-293">サーバーは、ASP.NET 4.5 をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-293">The server does not support ASP.NET 4.5.</span></span> <span data-ttu-id="efd0c-294">ASP.NET 4.5 用のサポートを追加できるかどうかを判断するホスティング プロバイダーにお問い合わせください。</span><span class="sxs-lookup"><span data-stu-id="efd0c-294">Contact the hosting provider to determine when and if support for ASP.NET 4.5 can be added.</span></span> <span data-ttu-id="efd0c-295">ASP.NET 4 またはそれ以前を対象とする web プロジェクトを配置する必要がある場合は、サーバーをアップグレードすることは、オプションではありません、代わりにします。同じ宛先へ、ASP.NET 4 またはそれ以前の web プロジェクトを展開する場合は、選択、**転送先に追加のファイルを削除**チェック ボックスをオン、**設定**のタブ、 **Web の発行**ウィザード。</span><span class="sxs-lookup"><span data-stu-id="efd0c-295">If upgrading the server is not an option, you have to deploy a web project that targets ASP.NET 4 or earlier instead.If you deploy an ASP.NET 4 or earlier web project to the same destination, select the **Remove additional files at destination** check box on the **Settings** tab of the **Publish Web** wizard.</span></span> <span data-ttu-id="efd0c-296">選択しない場合**転送先に追加のファイルを削除**、構成エラー ページを取得する続行されます。</span><span class="sxs-lookup"><span data-stu-id="efd0c-296">If you don't select **Remove additional files at destination**, you will continue to get the Configuration Error page.</span></span>

<span data-ttu-id="efd0c-297">プロジェクト**プロパティ**windows には、ターゲット フレームワークのドロップダウン リストが含まれていますが、だけを変更することでこの問題を解決できない **.NET Framework 4.5**に **.NET Framework 4**.</span><span class="sxs-lookup"><span data-stu-id="efd0c-297">The project **Properties** windows includes a Target framework drop-down list, but you can't resolve this problem by just changing that from **.NET Framework 4.5** to **.NET Framework 4**.</span></span> <span data-ttu-id="efd0c-298">以前の framework バージョンをターゲット フレームワークを変更した場合、プロジェクトは framework の以降のバージョンのアセンブリへの参照が残っているし、は実行されません。</span><span class="sxs-lookup"><span data-stu-id="efd0c-298">If you change the target framework to an earlier framework version, the project will still have references to the later framework version's assemblies and will not run.</span></span> <span data-ttu-id="efd0c-299">手動でこれらの参照を変更または .NET Framework 4 以前を対象とする新しいプロジェクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efd0c-299">You have to manually change those references or create a new project that targets .NET Framework 4 or earlier.</span></span> <span data-ttu-id="efd0c-300">詳細については、次を参照してください。 [Web サイトの .NET Framework Targeting](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="efd0c-300">For more information, see [.NET Framework Targeting for Web Sites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="efd0c-301">前へ</span><span class="sxs-lookup"><span data-stu-id="efd0c-301">Previous</span></span>](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
