---
uid: whitepapers/aspnet-and-iis6
title: IIS 6.0 と ASP.NET 1.1 を実行している |Microsoft Docs
author: rick-anderson
description: Windows Server 2003 には、IIS 6.0 と ASP.NET 1.1 の両方が含まれているときに、これらのコンポーネントは、既定で無効にします。 このホワイト ペーパーでは、IIS 6.0 を有効にする方法を説明する.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1983ef5b4902acc303ae224d5973eb2644284585
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383426"
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="173d8-104">IIS 6.0 と ASP.NET 1.1 を実行します。</span><span class="sxs-lookup"><span data-stu-id="173d8-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="173d8-105">Windows Server 2003 には、IIS 6.0 と ASP.NET 1.1 の両方が含まれているときに、これらのコンポーネントは、既定で無効にします。</span><span class="sxs-lookup"><span data-stu-id="173d8-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="173d8-106">このホワイト ペーパーでは、IIS 6.0 と ASP.NET 1.1 を有効にする方法について説明し、IIS と ASP.NET から最適なパフォーマンスを取得するいくつかの構成設定を推奨します。</span><span class="sxs-lookup"><span data-stu-id="173d8-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="173d8-107">ASP.NET 1.1 および IIS 6.0 に適用されます。</span><span class="sxs-lookup"><span data-stu-id="173d8-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="173d8-108">ASP.NET 1.1 に付属 Windows Server 2003 は、インターネット インフォメーション サーバー (IIS) バージョン 6.0 の最新のバージョンも含まれます。</span><span class="sxs-lookup"><span data-stu-id="173d8-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="173d8-109">IIS 6.0 および ASP.NET 1.1 がシームレスに統合するように設計および ASP.NET の新しい IIS 6.0 ワーカー プロセス モデルを今すぐ既定値します。</span><span class="sxs-lookup"><span data-stu-id="173d8-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="173d8-110">既定では ASP.NET 1.1 がインストールされていません</span><span class="sxs-lookup"><span data-stu-id="173d8-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="173d8-111">Microsoft のサーバー オペレーティング システムの以前のバージョンとは異なりインターネット インフォメーション サーバー (IIS) が有効でない既定ではまた ASP.NET 1.1 ではありません。</span><span class="sxs-lookup"><span data-stu-id="173d8-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="173d8-112">IIS を有効にするための 2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="173d8-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="173d8-113">サーバーの構成ウィザード、オプション 1: IIS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="173d8-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="173d8-114">Windows Server 2003 には、新しい ' サーバーの構成ウィザード '、目的のモードで、サーバーを適切に構成するためが付属しています。</span><span class="sxs-lookup"><span data-stu-id="173d8-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="173d8-115">移動する - 管理者としてログインする必要がありますウィザードを実行するに注意してください - ウィザードを起動します開始 |。プログラム |管理ツールと select '、サーバーの構成 '。</span><span class="sxs-lookup"><span data-stu-id="173d8-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="173d8-116">1 回選択した 'サーバーの構成ウィザード' の最初の画面を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="173d8-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="173d8-117">クリックして ' [次へ] &gt;'。</span><span class="sxs-lookup"><span data-stu-id="173d8-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="173d8-118">クリックして ' [次へ] &gt;'</span><span class="sxs-lookup"><span data-stu-id="173d8-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="173d8-119">この画面で選択する必要があります ' を構成するオプションとしてのアプリケーション サーバー (IIS、ASP.NET)。</span><span class="sxs-lookup"><span data-stu-id="173d8-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="173d8-120">クリックして ' [次へ] &gt;'。</span><span class="sxs-lookup"><span data-stu-id="173d8-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="173d8-121">アプリケーション サーバーとして、サーバーの構成を選択すると、この画面が表示されますどのような追加機能をインストールするかを確認します。</span><span class="sxs-lookup"><span data-stu-id="173d8-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="173d8-122">どちらのオプションは、既定で選択されます。</span><span class="sxs-lookup"><span data-stu-id="173d8-122">Neither option is selected by default.</span></span> <span data-ttu-id="173d8-123">ASP.NET を自動的に有効にするを選択する必要があります。 ' ASP を有効にします。NET'。</span><span class="sxs-lookup"><span data-stu-id="173d8-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="173d8-124">クリックして ' [次へ] &gt;'。</span><span class="sxs-lookup"><span data-stu-id="173d8-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="173d8-125">この画面がインストールのオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="173d8-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="173d8-126">クリックして ' [次へ] &gt;'。</span><span class="sxs-lookup"><span data-stu-id="173d8-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="173d8-127">選択したオプションのインストール中に、この画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="173d8-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="173d8-128">通常、サービスがインストールされているボックスが表示されるその他のダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="173d8-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="173d8-129">さらに、Windows Server 2003 インストール CD の場所を求められます。</span><span class="sxs-lookup"><span data-stu-id="173d8-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="173d8-130">クリックして '[次へ] &gt;' を完了するとします。</span><span class="sxs-lookup"><span data-stu-id="173d8-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="173d8-131">IIS 6.0 および ASP.NET 1.1 をサポートするためには、Windows Server 2003 が構成されました - [完了] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="173d8-132">オプション #2 - 手動で IIS と ASP.NET を構成する、IIS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="173d8-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="173d8-133">'サーバーの構成ウィザード' を使用したくない場合、コントロール パネルから IIS 6.0 およびプログラムの追加と削除' を使用して ASP.NET 1.1 をインストールすることができます必要に応じて。</span><span class="sxs-lookup"><span data-stu-id="173d8-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="173d8-134">最初に、コントロール パネルを開きます。</span><span class="sxs-lookup"><span data-stu-id="173d8-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="173d8-135">次に、' 追加/削除 Windows コンポーネント、Windows コンポーネント ウィザード が開きますをクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="173d8-136">強調表示し、「アプリケーション サーバー」を確認し、'詳細' でしょうか。</span><span class="sxs-lookup"><span data-stu-id="173d8-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="173d8-137">ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="173d8-138">ASP.NET をインストールするには、次のように確認します。 ' ASP します。NET'。</span><span class="sxs-lookup"><span data-stu-id="173d8-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="173d8-139">Windows コンポーネント ウィザードに戻るには、[OK] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="173d8-140">クリックして '[次へ] &gt;' からのインストールを開始する Windows コンポーネント ウィザード。</span><span class="sxs-lookup"><span data-stu-id="173d8-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="173d8-141">通常、サービスがインストールされているボックスが表示されるその他のダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="173d8-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="173d8-142">さらに、Windows Server 2003 インストール CD の場所を求められます。</span><span class="sxs-lookup"><span data-stu-id="173d8-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="173d8-143">インストールが完了したら、Windows コンポーネント ウィザードの最後の画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="173d8-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="173d8-144">IIS 6.0 および ASP.NET 1.1 が構成および使用可能な。</span><span class="sxs-lookup"><span data-stu-id="173d8-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="173d8-145">推奨設定</span><span class="sxs-lookup"><span data-stu-id="173d8-145">Recommended Settings</span></span>

<span data-ttu-id="173d8-146">IIS 6.0 と ASP.NET 1.1 を実行している場合は、ASP.NET から最適なパフォーマンスを得るに推奨されるいくつかの構成設定があります。</span><span class="sxs-lookup"><span data-stu-id="173d8-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="173d8-147">ワーカー プロセス メモリ制限の構成</span><span class="sxs-lookup"><span data-stu-id="173d8-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="173d8-148">ワーカー プロセスのリサイクルを構成します。</span><span class="sxs-lookup"><span data-stu-id="173d8-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="173d8-149">ワーカー プロセス メモリ制限の構成</span><span class="sxs-lookup"><span data-stu-id="173d8-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="173d8-150">既定で IIS 6.0 は、IIS が使用できるメモリの量に制限を設定しません。</span><span class="sxs-lookup"><span data-stu-id="173d8-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="173d8-151">ASP します。NET のキャッシュ機能は、キャッシュは積極的にメモリから未使用の項目を削除するために、メモリの制限事項に依存します。</span><span class="sxs-lookup"><span data-stu-id="173d8-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="173d8-152">IIS 6.0 の機能をリサイクルするメモリを構成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="173d8-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="173d8-153">この開いているインターネット インフォメーション サービス マネージャーを構成する (開始 |プログラム |管理ツール |インターネット インフォメーション サービス)。</span><span class="sxs-lookup"><span data-stu-id="173d8-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="173d8-154">開いている場合は、' アプリケーション プール ' フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="173d8-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="173d8-155">各アプリケーション プール。</span><span class="sxs-lookup"><span data-stu-id="173d8-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="173d8-156">例: アプリケーション プールを右クリックします。'DefaultAppPool' と 'プロパティ' を選択します。</span><span class="sxs-lookup"><span data-stu-id="173d8-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="173d8-157">次に、いずれかをクリックしてメモリのリサイクルを有効にする ' 使用された最大メモリ (メガバイト単位)。 '。</span><span class="sxs-lookup"><span data-stu-id="173d8-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="173d8-158">値することはできません、サーバー上の物理 (仮想) のメモリ量以上で、十分な概算は 512 mb の物理メモリの選択 310 サーバーなどの物理メモリの 60% です。</span><span class="sxs-lookup"><span data-stu-id="173d8-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="173d8-159">最大値を超えていないこと 800 MB 2 GB のアドレス空間を使用する場合もお勧めします。</span><span class="sxs-lookup"><span data-stu-id="173d8-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="173d8-160">サーバーのメモリ アドレス空間が 3 GB の場合、ワーカー プロセスのメモリの上限は 1、800 MB の最高になります。</span><span class="sxs-lookup"><span data-stu-id="173d8-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="173d8-161">プロパティ ダイアログを終了するには、適用 と OK をクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="173d8-162">すべての利用可能なアプリケーション プールには、これを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="173d8-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="173d8-163">ワーカーのリサイクルを構成します。</span><span class="sxs-lookup"><span data-stu-id="173d8-163">Configuring worker recycling</span></span>

<span data-ttu-id="173d8-164">既定では、IIS 6.0 は 29 時間ごとのワーカー プロセスをリサイクルする構成します。</span><span class="sxs-lookup"><span data-stu-id="173d8-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="173d8-165">これは、ASP.NET を実行するアプリケーションの少しアグレッシブなと自動のワーカー プロセスのリサイクルが無効になっていることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="173d8-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="173d8-166">自動のワーカー プロセスのリサイクルを無効にするには、まずインターネット インフォメーション サービス マネージャーを開きます (開始 |プログラム |管理ツール |インターネット インフォメーション サービス)。</span><span class="sxs-lookup"><span data-stu-id="173d8-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="173d8-167">開いている場合は、' アプリケーション プール ' フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="173d8-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="173d8-168">各アプリケーション プール。</span><span class="sxs-lookup"><span data-stu-id="173d8-168">For each application pool:</span></span>

1. <span data-ttu-id="173d8-169">例: アプリケーション プールを右クリックします。'DefaultAppPool' と 'プロパティ' を選択します。</span><span class="sxs-lookup"><span data-stu-id="173d8-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="173d8-170">オフに ' (分単位) をワーカー プロセスのリサイクル:'。</span><span class="sxs-lookup"><span data-stu-id="173d8-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="173d8-171">プロパティ ダイアログを終了するには、適用 と OK をクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="173d8-172">すべての利用可能なアプリケーション プールには、これを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="173d8-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="173d8-173">ファイル システムへの書き込みアクセス許可</span><span class="sxs-lookup"><span data-stu-id="173d8-173">Granting write access to the file system</span></span>

<span data-ttu-id="173d8-174">アプリケーションがファイル システムへの書き込みアクセスを必要とし、NTFS を使用している場合は、変更、アクセス制御リスト (ACL)、フォルダーまたはファイルの ASP.NET のアクセス権を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="173d8-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="173d8-175">たとえば、ASP.NET を許可する、c:\inetpub\wwwroot への書き込みアクセス最初エクスプ ローラーを開きし、ディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="173d8-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="173d8-176">次に、ディレクトリ、たとえば 'wwwroot' し、プロパティの右クリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="173d8-177">プロパティ ダイアログ ボックスが開いたら、[セキュリティ] タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="173d8-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="173d8-178">C:\inetpub\wwwroot\ ディレクトリに特別なディレクトリでは、特別な IIS 6.0 グループ ' IIS\_WPG' は既に読み取りが付与されて&amp;実行、フォルダーの内容の一覧と読み取りアクセス許可。</span><span class="sxs-lookup"><span data-stu-id="173d8-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="173d8-179">ただし、書き込みアクセス許可を付与する書き込みを許可するチェック ボックスをクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="173d8-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="173d8-180">IIS 6.0 で、このフォルダーに対する書き込みアクセス許可できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="173d8-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="173d8-181">-以下の手順に従って、他のフォルダーに対する書き込みアクセス許可を付与するに注意してください、IIS を追加する必要があります\_WPG グループが既に存在しない場合。</span><span class="sxs-lookup"><span data-stu-id="173d8-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="173d8-182">IIS への書き込みアクセス権の付与\_WPG が ASP.NET アプリケーションをこのディレクトリへの書き込みを許可します。</span><span class="sxs-lookup"><span data-stu-id="173d8-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="173d8-183">SQL Server による統合認証をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="173d8-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="173d8-184">統合認証は、Windows NT 認証を利用する SQL Server の SQL Server ログオン アカウントを検証できます。</span><span class="sxs-lookup"><span data-stu-id="173d8-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="173d8-185">これにより、ユーザーは、標準の SQL Server ログオン プロセスを回避できます。</span><span class="sxs-lookup"><span data-stu-id="173d8-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="173d8-186">この方法により、ネットワーク ユーザーは、SQL Server、Windows NT のネットワーク セキュリティの処理から、ユーザーとパスワードの情報を取得するために、個別のログインの id またはパスワードを指定せず、SQL Server データベースをアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="173d8-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="173d8-187">アプリケーションの接続文字列内でこれまで格納されている資格情報はないためには、ASP.NET アプリケーションの統合認証を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="173d8-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="173d8-188">はなく SQL への接続に使用する接続文字列はようになります。</span><span class="sxs-lookup"><span data-stu-id="173d8-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="173d8-189">この接続文字列は、SQL Server にアクセスしようとすると、アプリケーションの Windows 資格情報を使用する SQL Server に指示します。</span><span class="sxs-lookup"><span data-stu-id="173d8-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="173d8-190">Iis のアカウントであるこの ASP.NET/IIS 6 の場合は\_WPG グループ。</span><span class="sxs-lookup"><span data-stu-id="173d8-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="173d8-191">SQL Server と ASP.NET 間で統合認証を有効にする最初の統合認証または混合モード認証 - SQL Server が構成されていることを確認する必要がありますこれを確認するようにデータベース管理者に確認します。</span><span class="sxs-lookup"><span data-stu-id="173d8-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="173d8-192">SQL Server がこれら 2 つのモードのいずれかである場合は、統合認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="173d8-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="173d8-193">SQL Server Enterprise Manager を開きます (開始 |プログラム |Microsoft SQL Server |Enterprise Manager) は、適切なサーバーを選択し、セキュリティ フォルダーを展開します。</span><span class="sxs-lookup"><span data-stu-id="173d8-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="173d8-194">場合 ' BUILTINT\IIS\_WPG' グループが表示されていない、ログインを右クリックし、[新しいログイン] を選択します。</span><span class="sxs-lookup"><span data-stu-id="173d8-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="173d8-195">'名:' テキスト ボックスに入力するか' [サーバー/ドメイン名] \IIS\_WPG' Windows NT ユーザー/グループの選択を開く省略記号ボタンをクリックしてまたは。</span><span class="sxs-lookup"><span data-stu-id="173d8-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="173d8-196">現在のコンピューターの IIS 選択\_'Add' アクセサーとピッカーを閉じてよい WPG グループをクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="173d8-197">既定のデータベースおよびデータベースにアクセスするアクセス許可も設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="173d8-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="173d8-198">設定するには、既定のデータベースは 例: 次の Northwind が選択されている ドロップダウン リストから選択します。</span><span class="sxs-lookup"><span data-stu-id="173d8-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="173d8-199">次に、データベースへのアクセス タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="173d8-200">アクセスを許可するすべてのデータベースの [許可] チェック ボックスをクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="173d8-201">必要になります、データベースの役割の選択に db をチェック\_所有者は、ログインを管理し、選択したデータベースを使用して、すべての必要なアクセス許可を持つことを確認します。</span><span class="sxs-lookup"><span data-stu-id="173d8-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="173d8-202">プロパティ ダイアログ ボックスを終了するには、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="173d8-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="173d8-203">SQL Server の統合認証をサポートするために、ASP.NET アプリケーションが構成されているようになりました。</span><span class="sxs-lookup"><span data-stu-id="173d8-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="173d8-204">ネイティブ モードの IIS 6.0 で ASP.NET 1.0 を実行しないでください。</span><span class="sxs-lookup"><span data-stu-id="173d8-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="173d8-205">IIS 6.0 で ASP.NET 1.0 は、IIS 5 互換モードでのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="173d8-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="173d8-206">IIS 5.0 互換モードで実行する ASP.NET 1.0 を構成するには、インターネット サービス マネージャを開く Web サイトを右クリックし、プロパティを選択します。</span><span class="sxs-lookup"><span data-stu-id="173d8-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="173d8-207">サービス タブに切り替えて、チェックでしょうか。IIS 5.0 分離モードで WWW サービスを実行します。</span><span class="sxs-lookup"><span data-stu-id="173d8-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
