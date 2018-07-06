---
uid: whitepapers/side-by-side-with-10
title: .NET Framework 1.0 および 1.1 の ASP.NET で並列実行 |Microsoft Docs
author: rick-anderson
description: このホワイト ペーパーでは、ASP.NET Web アプリケーションをフレームのいずれかのバージョンで実行することにより、コンピューターに .NET 1.0 と .NET 1.1 の両方をインストールする方法について説明しています.
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1018845e3d2c6603732bfbbde78f4a9183e49d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808396"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="31a7c-103">.NET Framework 1.0 および 1.1 の ASP.NET で並列実行</span><span class="sxs-lookup"><span data-stu-id="31a7c-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="31a7c-104">このホワイト ペーパーでは、ASP.NET Web アプリケーションをフレームワークのいずれかのバージョンで実行することにより、コンピューターに .NET 1.0 と .NET 1.1 の両方をインストールする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="31a7c-105">ASP.NET 1.0 と ASP.NET 1.1 に適用されます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="31a7c-106">ASP.NET では、アプリケーションが同じコンピューターでは、インストールされているものの、異なるバージョンの .NET Framework を使用して、場合、並行して実行するといいます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="31a7c-107">次のトピックでは、サイド バイ サイドで実行するための ASP.NET アプリケーションを構成する方法について説明しする詳細な手順を提供します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="31a7c-108">インストール中に .NET Framework version 1.0 への Web アプリケーションのマッピングを管理します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="31a7c-109">.NET Framework の特定のバージョンへの Web アプリケーションをマップします。</span><span class="sxs-lookup"><span data-stu-id="31a7c-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="31a7c-110">Web サイトを使用している .NET Framework のバージョンを確認します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="31a7c-111">これまでは、コンポーネントまたはアプリケーションがコンピューターに更新されると、以前のバージョンが削除され、新しいバージョンに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="31a7c-112">新しいバージョンが以前のバージョンと互換性がない場合これは、通常、コンポーネントやアプリケーションを使用するその他のアプリケーションを中断します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="31a7c-113">.NET Framework では、これにより、アセンブリまたは同時に同じコンピューターにインストールするアプリケーションの複数のバージョン、サイド バイ サイドで実行するためのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="31a7c-114">複数のバージョンを同時にインストールできるため、マネージ アプリケーションは、別のバージョンを使用するアプリケーションに影響を与えずに使用するバージョンを選択できます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="31a7c-115">既定では、.NET Framework version 1.1 のインストール中に既存のすべての ASP.NET アプリケーションが自動的に再構成、.NET Framework の最新バージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="31a7c-116">既定の .NET Framework 1.1 ASP.NET アプリケーションにしたくない場合はクリックして[ここ](#1)のインストール時にこれを回避する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="31a7c-117">.NET Framework 1.1 に、Web サーバーを更新し、1 つまたは複数の Web アプリケーションを .NET Framework 1.0 を実行すると場合、は、インターネット インフォメーション サービス (IIS) のスクリプト マップを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="31a7c-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="31a7c-118">スクリプトのマッピングは、.NET Framework のバージョンを特定の Web アプリケーションの .aspx ファイルの拡張子をマップするメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="31a7c-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="31a7c-119">クリックして[ここ](#2)に .NET Framework の特定のバージョンへの Web アプリケーションにマップする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="31a7c-120">インターネットの情報マネージャーか、ASP.NET IIS 登録ツールを使用することができます (Aspnet\_regiis.exe) を特定の Web アプリケーションを実行して .NET Framework バージョンを検索します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="31a7c-121">クリックして[ここ](#3)に Web サイトを使用している .NET Framework のバージョンを確認する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="31a7c-122">.NET Framework 1.1 への移行は、.NET Framework の各バージョンが、独自の Machine.config ファイルを使用するときに考慮を 1 つのインポート:</span><span class="sxs-lookup"><span data-stu-id="31a7c-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="31a7c-123">その結果、Web 管理者が Machine.config ファイルに変更を加えた場合これらの変更は、.NET Framework 1.1 の Machine.config ファイルに移行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="31a7c-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="31a7c-124">インストール中に .NET Framework 1.0 に Web アプリケーションのマッピングの保持</span><span class="sxs-lookup"><span data-stu-id="31a7c-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="31a7c-125">既定では、既存のすべての ASP.NET アプリケーションは、.NET Framework の新しいバージョンを使用して、インストール時に自動的に再構成されます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="31a7c-126">.NET Framework の新しいバージョンを使用して、アプリケーションできますフル活用および新しいリリースに含まれる新しい機能強化。</span><span class="sxs-lookup"><span data-stu-id="31a7c-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="31a7c-127">同時に、Web 管理者は、どのアプリケーションをきめ細かく制御を望む場合がありますが更新、自動再マップのすべての既存の ASP.NET アプリケーションの .NET Framework のインストール中に防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="31a7c-128">.NET Framework の新しいバージョンへの ASP.NET アプリケーション全体の自動再割り当てを防ぐためには、Web 管理者は、Dotnetfx.exe のセットアップ プログラムで/noaspupgrade コマンド ライン オプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="31a7c-129">**新しいバージョンに ASP.NET アプリケーションの合計の再割り当てを回避するには**</span><span class="sxs-lookup"><span data-stu-id="31a7c-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="31a7c-130">移動して**開始**します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-130">Go to **Start**.</span></span>
2. <span data-ttu-id="31a7c-131">をクリックして**実行**します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-131">Click on **run**.</span></span>
3. <span data-ttu-id="31a7c-132">「**cmd**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-132">Type **cmd**.</span></span>
4. <span data-ttu-id="31a7c-133">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31a7c-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="31a7c-134">コマンド プロンプトから、.NET Framework のインストールを開始する次の行を入力します: **Dotnetfx.exe/c:"/noaspupgrade をインストールしますか?** します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="31a7c-135">クリックして**はい**Microsoft .NET Framework 1.1 のセットアップ時にします。</span><span class="sxs-lookup"><span data-stu-id="31a7c-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="31a7c-136">これにより、.NET Framework 1.1 のセットアップ プロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="31a7c-137">.NET Framework の特定のバージョンへの Web アプリケーションをマップします。</span><span class="sxs-lookup"><span data-stu-id="31a7c-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="31a7c-138">.NET Framework のバージョンごとに、ASP.NET IIS 登録ツールのバージョンが含まれています (Aspnet\_regiis.exe)。</span><span class="sxs-lookup"><span data-stu-id="31a7c-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="31a7c-139">このツールには、管理者 Web アプリケーションが .NET Framework の特定のバージョンで実行することを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="31a7c-140">これは Web アプリケーションを .NET Framework のバージョンのマッピングと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="31a7c-141">管理者は、Aspnet を選択する必要があります\_regiis.exe、Web アプリケーションと関連付けられる .NET Framework のバージョンに対応します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="31a7c-142">たとえば、Web サイトが .NET Framework 1.1 を使用するように指定する必要のある管理者は、Aspnet を使用する必要があります\_regiis.exe .NET Framework 1.1 に付属しています。</span><span class="sxs-lookup"><span data-stu-id="31a7c-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="31a7c-143">Aspnet\_regiis.exe バージョン 1.0 には。</span><span class="sxs-lookup"><span data-stu-id="31a7c-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="31a7c-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="31a7c-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="31a7c-145">Aspnet\_regiis.exe バージョン 1, 1 にあります。</span><span class="sxs-lookup"><span data-stu-id="31a7c-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="31a7c-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="31a7c-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="31a7c-147">Aspnet\_regiis.exe スクリプト、Web アプリケーションのマッピングの 2 つのオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="31a7c-148">**-s**マップを設定、スクリプトのパスとその子ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="31a7c-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="31a7c-149">**-sn**パスだけでスクリプト マップを設定します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="31a7c-150">パスは、W3SVC/ルートの形式で定義されている Web アプリケーション IIS メタデータ パスの定義/{WebSiteNumber}/{アプリケーション\_名}。</span><span class="sxs-lookup"><span data-stu-id="31a7c-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="31a7c-151">たとえば、既定の Web サイトの下にあるポータルと呼ばれる Web アプリケーション、メタベースのパスは W3SVC/1/ルート/ポータル。</span><span class="sxs-lookup"><span data-stu-id="31a7c-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="31a7c-152">メタベース パスを取得するメタベース エディターと呼ばれるツールを使用することもできます。 注意してください。</span><span class="sxs-lookup"><span data-stu-id="31a7c-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="31a7c-153">このツールをダウンロードするには、Microsoft サポート サイトから[ https://support.microsoft.com/default.aspx?scid=kb; en-232068; 米国。](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="31a7c-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="31a7c-154">実行 Aspnet\_マップとその subapplication regiis.exe-s W3SVC/1/ルート/ポータル IIS ポータルを更新するスクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="31a7c-155">Aspnet の実行\_regiis.exe sn-たとえば、W3SVC/1/ルート/ポータル ポータルの IIS スクリプトを更新するマップのポータルでアプリケーションに影響を与えずに? s サブディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="31a7c-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="31a7c-156">Web アプリケーションを使用している .NET Framework バージョンを確認します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="31a7c-157">管理者は、インターネット サービス マネージャーを使用して、Web サイトで実行される .NET Framework のバージョンを検索します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="31a7c-158">オペレーティング システムのバージョンでは、異なる方法で、インターネット サービス マネージャーを起動します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="31a7c-159">サービス マネージャーを起動するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="31a7c-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="31a7c-160">**インターネット サービス マネージャーを起動するには**</span><span class="sxs-lookup"><span data-stu-id="31a7c-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="31a7c-161">移動して**開始**します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-161">Go to **Start**.</span></span>
2. <span data-ttu-id="31a7c-162">をクリックして**実行**します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-162">Click on **run**.</span></span>
3. <span data-ttu-id="31a7c-163">型**inetmgr**します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="31a7c-164">インターネット サービス マネージャからでは、Web アプリケーションを知りたい .NET Framework のバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="31a7c-165">Web アプリケーションを右クリックし、をクリックして**プロパティ。**</span><span class="sxs-lookup"><span data-stu-id="31a7c-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="31a7c-166">プロパティ ウィンドウで、次のように選択します。**構成します。**</span><span class="sxs-lookup"><span data-stu-id="31a7c-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="31a7c-167">アプリケーションのマッピング テーブルから選択 **.aspx**、 をクリック**編集**します。</span><span class="sxs-lookup"><span data-stu-id="31a7c-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="31a7c-168">**実行可能ファイル**テキスト ボックス、スクロールしてバージョンのディレクトリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="31a7c-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="31a7c-169">バージョンのディレクトリが v.1.1.4322 の場合は、アプリケーションは、.NET Framework 1.1 にマップされます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="31a7c-170">逆に、バージョンのディレクトリが v1.0.3705 の場合は、アプリケーションが .NET Framework 1.0 にマップされます。</span><span class="sxs-lookup"><span data-stu-id="31a7c-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
