---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "バージョン 2 に SignalR 1.x プロジェクトのアップグレード |Microsoft ドキュメント"
author: pfletcher
description: "SignalR に既存の SignalR 1.x プロジェクトをアップグレードする方法を説明 2.x、およびアップグレードの処理中に発生する可能性のある問題をトラブルシューティングする方法."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="48fc9-103">SignalR 1.x プロジェクトをバージョン 2 にアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="48fc9-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="48fc9-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="48fc9-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="48fc9-105">SignalR に既存の SignalR 1.x プロジェクトをアップグレードする方法を説明 2.x、およびアップグレードの処理中に発生する可能性のある問題をトラブルシューティングする方法です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="48fc9-106">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="48fc9-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="48fc9-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="48fc9-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="48fc9-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="48fc9-108">.NET 4.5</span></span>
> - <span data-ttu-id="48fc9-109">SignalR バージョン 1 と 2</span><span class="sxs-lookup"><span data-stu-id="48fc9-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="48fc9-110">このチュートリアルで Visual Studio 2012 を使用します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="48fc9-111">このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="48fc9-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="48fc9-112">更新プログラム、 [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。</span><span class="sxs-lookup"><span data-stu-id="48fc9-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="48fc9-113">インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="48fc9-114">Web Platform Installer で検索し、インストール**ASP.NET と Visual Studio 2012 用 Web ツール 2013.1**です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="48fc9-115">SignalR クラス用の Visual Studio テンプレートなどのインストールはこの**ハブ**です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="48fc9-116">一部のテンプレート (など**OWIN スタートアップ クラス**) できなくなります。 これらの場合には、クラス ファイルを代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="48fc9-117">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="48fc9-117">Questions and comments</span></span>
> 
> <span data-ttu-id="48fc9-118">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="48fc9-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="48fc9-119">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="48fc9-120">SignalR 2 を使用してサーバー プラットフォーム間で一貫した開発エクスペリエンスを提供する[OWIN](http://owin.org)です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="48fc9-121">この記事では、SignalR 1.x アプリケーションをバージョン 2 に更新するために必要ないくつかの手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="48fc9-122">SignalR 2 にアプリケーションをアップグレードすることをお勧め、1.x でも SignalR でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="48fc9-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="48fc9-123">このチュートリアルでは、SignalR 2 に web ホスト アプリケーションをアップグレードする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="48fc9-124">SignalR 2 では、自己ホスト型アプリケーション (コンソール アプリケーション、Windows サービス、またはその他のプロセス内のサーバーをホストしているもの) はサポートされています。</span><span class="sxs-lookup"><span data-stu-id="48fc9-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="48fc9-125">SignalR 2 自己ホスト型アプリケーションの作成を開始する方法については、次を参照してください。[チュートリアル: 自己ホスト SignalR](../deployment/tutorial-signalr-self-host.md)です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="48fc9-126">目次</span><span class="sxs-lookup"><span data-stu-id="48fc9-126">Contents</span></span>

<span data-ttu-id="48fc9-127">次のセクションでは、SignalR プロジェクト、および発生する可能性のある問題をトラブルシューティングする方法のアップグレードに関連するタスクについて説明します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="48fc9-128">例: SignalR 2 へのチュートリアル入門のアップグレード</span><span class="sxs-lookup"><span data-stu-id="48fc9-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="48fc9-129">アップグレード中に発生したエラーのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="48fc9-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="48fc9-130">例: SignalR 2 に概要チュートリアル アプリケーションのアップグレード</span><span class="sxs-lookup"><span data-stu-id="48fc9-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="48fc9-131">このセクションで作成したアプリケーションを更新するされます、[チュートリアル入門の SignalR 1.x バージョン](../older-versions/index.md)SignalR 2 を使用します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="48fc9-132">チュートリアル入門が終了したら後、プロジェクトを右クリックし、選択**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="48fc9-133">いることを確認、**ターゲット フレームワーク**に設定されている**.NET Framework 4.5。**</span><span class="sxs-lookup"><span data-stu-id="48fc9-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="48fc9-134">パッケージ マネージャー コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="48fc9-134">Open the Package Manager Console.</span></span> <span data-ttu-id="48fc9-135">SignalR を削除する、次のコマンドを使用してプロジェクトから 1.x:</span><span class="sxs-lookup"><span data-stu-id="48fc9-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="48fc9-136">次のコマンドを使用して SignalR 2 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="48fc9-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="48fc9-137">HTML ページで、プロジェクトに含まれるスクリプトのバージョンと一致する SignalR のスクリプト参照を更新します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="48fc9-138">グローバル アプリケーション クラスでは、MapHubs への呼び出しを削除します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="48fc9-139">ソリューションを右クリックし **追加**、**新しい項目の追加.**.ダイアログ ボックスで、次のように選択します。 **Owin スタートアップ クラス**です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="48fc9-140">新しいクラスの名前を**Startup.cs**です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="48fc9-141">次のコードでは、Startup.cs の内容を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="48fc9-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="48fc9-142">アセンブリの属性を実行する Owin の起動プロセスをクラスを追加、`Configuration`メソッド Owin 起動するとします。</span><span class="sxs-lookup"><span data-stu-id="48fc9-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="48fc9-143">これを呼び出して、`MapSignalR`メソッドで、アプリケーションのすべての SignalR ハブのルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="48fc9-144">プロジェクトを実行し、別に、メイン ページの URL をコピー ブラウザーまたはブラウザー ウィンドウで以前のようにします。</span><span class="sxs-lookup"><span data-stu-id="48fc9-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="48fc9-145">各ページは、ユーザー名を求められ、各ページから送信されたメッセージは両方のブラウザー ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="48fc9-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="48fc9-146">アップグレード中に発生したエラーのトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="48fc9-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="48fc9-147">このセクションでは、アップグレード中に発生する可能性のある問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="48fc9-148">包括的なの SignalR アプリケーションで発生する可能性のあるエラーや問題の一覧を表示するには、次を参照してください。 [SignalR トラブルシューティング](../testing-and-debugging/troubleshooting.md)です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="48fc9-149">' 呼び出しが次のメソッドまたはプロパティ間であいまいな '</span><span class="sxs-lookup"><span data-stu-id="48fc9-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="48fc9-150">参照がこのエラーが発生`Microsoft.AspNet.SignalR.Owin`は削除されません。</span><span class="sxs-lookup"><span data-stu-id="48fc9-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="48fc9-151">このパッケージは廃止されました。参照を削除する必要があります、自己ホスト型のパッケージのバージョン 1.x をアンインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="48fc9-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="48fc9-152">ハブ メソッドがサイレント モードで失敗します。</span><span class="sxs-lookup"><span data-stu-id="48fc9-152">Hub methods fail silently</span></span>

<span data-ttu-id="48fc9-153">日付、およびするまで、クライアント スクリプト参照があることを確認、`OwinStartup`スタートアップ クラスには、正しいクラスと、プロジェクトのアセンブリ名の属性です。</span><span class="sxs-lookup"><span data-stu-id="48fc9-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="48fc9-154">またからハブに指定されたアドレス (signalr/ハブ) お使いのブラウザーを開いてください。表示されるエラーの詳細については、問題の原因が提供されます。</span><span class="sxs-lookup"><span data-stu-id="48fc9-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>
