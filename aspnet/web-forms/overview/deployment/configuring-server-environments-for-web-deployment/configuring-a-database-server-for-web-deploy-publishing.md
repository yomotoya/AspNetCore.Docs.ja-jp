---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: "デプロイの発行設定 Web のデータベース サーバーの構成 |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、web デプロイおよび公開をサポートするために SQL Server 2008 R2 データベース サーバーを構成する方法について説明します。 このトピックで説明するタスクは、co."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: b225d9911246b3e2be1679b73a9f31d9f8577ba5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a><span data-ttu-id="b04c8-104">Web デプロイの発行用のデータベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="b04c8-104">Configuring a Database Server for Web Deploy Publishing</span></span>
====================
<span data-ttu-id="b04c8-105">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="b04c8-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="b04c8-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b04c8-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="b04c8-107">このトピックでは、web デプロイおよび公開をサポートするために SQL Server 2008 R2 データベース サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-107">This topic describes how to configure a SQL Server 2008 R2 database server to support web deployment and publishing.</span></span>
> 
> <span data-ttu-id="b04c8-108">このトピックで説明するタスクは、すべての展開シナリオ & #x 2014 に共通です IIS Web 配置ツール (Web 配置) のリモート エージェント サービス、Web 配置ハンドラー、またはオフラインの展開を使用する web サーバーが構成されているかにかかわらず、または。アプリケーションは、1 つの web サーバーまたはサーバー ファームで行われています。</span><span class="sxs-lookup"><span data-stu-id="b04c8-108">The tasks described in this topic are common to every deployment scenario&#x2014;it doesn't matter whether your web servers are configured to use the IIS Web Deployment Tool (Web Deploy) Remote Agent Service, the Web Deploy Handler, or offline deployment or your application is running on a single web server or a server farm.</span></span> <span data-ttu-id="b04c8-109">セキュリティ要件およびその他の注意事項に応じて、データベースを展開する方法を変更ことがあります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-109">The way you deploy the database may change according to security requirements and other considerations.</span></span> <span data-ttu-id="b04c8-110">たとえば、サンプル データの有無、データベースを展開する場合があり、ユーザー ロールのマッピングを展開または展開後に手動で構成する場合があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-110">For example, you might deploy the database with or without sample data, and you might deploy user role mappings or configure them manually after deployment.</span></span> <span data-ttu-id="b04c8-111">ただし、データベース サーバーを構成する方法は変わりません。</span><span class="sxs-lookup"><span data-stu-id="b04c8-111">However, the way you configure the database server remains the same.</span></span>


<span data-ttu-id="b04c8-112">Web 配置をサポートするためにデータベース サーバーを構成する追加の製品やツールをインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b04c8-112">You don't have to install any additional products or tools to configuring a database server to support web deployment.</span></span> <span data-ttu-id="b04c8-113">想定されるので、データベース サーバーと web サーバーは、別のコンピューターで実行、単純にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-113">Assuming that your database server and your web server run on different machines, you simply need to:</span></span>

- <span data-ttu-id="b04c8-114">SQL Server で TCP/IP を使用して通信を許可します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-114">Permit SQL Server to communicate using TCP/IP.</span></span>
- <span data-ttu-id="b04c8-115">SQL Server のファイアウォールを通過するトラフィックを許可します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-115">Allow SQL Server traffic through any firewalls.</span></span>
- <span data-ttu-id="b04c8-116">Web サーバーのコンピューター アカウントに SQL Server ログインを付けます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-116">Give the web server machine account a SQL Server login.</span></span>
- <span data-ttu-id="b04c8-117">任意の必要なデータベース ロールにマシン アカウントのログインをマップします。</span><span class="sxs-lookup"><span data-stu-id="b04c8-117">Map the machine account login to any required database roles.</span></span>
- <span data-ttu-id="b04c8-118">SQL Server ログインとデータベース作成者のアクセス許可を展開を実行するアカウントに与えます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-118">Give the account that will run the deployment a SQL Server login and database creator permissions.</span></span>
- <span data-ttu-id="b04c8-119">繰り返し展開をサポートするために、展開アカウントへのログインをマップ、 **db\_所有者**データベース ロール。</span><span class="sxs-lookup"><span data-stu-id="b04c8-119">To support repeat deployments, map the deployment account login to the **db\_owner** database role.</span></span>

<span data-ttu-id="b04c8-120">このトピックでは、各手順を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-120">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="b04c8-121">タスクとここでチュートリアルは、Windows Server 2008 R2 で実行されている SQL Server 2008 R2 の既定のインスタンスを開始していることを想定しています。</span><span class="sxs-lookup"><span data-stu-id="b04c8-121">The tasks and walkthroughs in this topic assume that you're starting with a default instance of SQL Server 2008 R2 running on Windows Server 2008 R2.</span></span> <span data-ttu-id="b04c8-122">続行する前に以下のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-122">Before you continue, ensure that:</span></span>

- <span data-ttu-id="b04c8-123">Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-123">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="b04c8-124">サーバーがドメインに参加します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-124">The server is domain-joined.</span></span>
- <span data-ttu-id="b04c8-125">サーバーは、静的 IP アドレスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-125">The server has a static IP address.</span></span>
- <span data-ttu-id="b04c8-126">SQL Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-126">SQL Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>

<span data-ttu-id="b04c8-127">のみ、SQL Server インスタンスを含める必要がある、**データベース エンジン サービス**ロールは、任意の SQL Server のインストールに自動的に含まれています。</span><span class="sxs-lookup"><span data-stu-id="b04c8-127">The SQL Server instance only needs to include the **Database Engine Services** role, which is included automatically in any SQL Server installation.</span></span> <span data-ttu-id="b04c8-128">ただし、構成と保守の容易にするため、推奨を含めること、**管理ツール-基本**と**管理ツール-完全**サーバーの役割です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-128">However, for ease of configuration and maintenance, we recommend that you include the **Management Tools – Basic** and **Management Tools – Complete** server roles.</span></span>

> [!NOTE]
> <span data-ttu-id="b04c8-129">コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューター、ドメインとログオン](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-129">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="b04c8-130">静的 IP アドレスを構成する方法については、次を参照してください。[静的 IP アドレスを構成](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-130">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx).</span></span> <span data-ttu-id="b04c8-131">SQL Server のインストールの詳細については、次を参照してください。 [SQL Server 2008 R2 のインストール](https://technet.microsoft.com/en-us/library/bb500395.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-131">For more information on installing SQL Server, see [Installing SQL Server 2008 R2](https://technet.microsoft.com/en-us/library/bb500395.aspx).</span></span>


## <a name="enable-remote-access-to-sql-server"></a><span data-ttu-id="b04c8-132">SQL Server へのリモート アクセスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="b04c8-132">Enable Remote Access to SQL Server</span></span>

<span data-ttu-id="b04c8-133">SQL Server では、リモート コンピューターと通信するために TCP/IP が使用されます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-133">SQL Server uses TCP/IP to communicate with remote computers.</span></span> <span data-ttu-id="b04c8-134">データベース サーバーと web サーバーが異なるコンピューター上にある場合は、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-134">If your database server and your web server are on different machines, you need to:</span></span>

- <span data-ttu-id="b04c8-135">TCP/IP 経由で通信を許可する SQL Server ネットワークの設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-135">Configure SQL Server networking settings to allow communication over TCP/IP.</span></span>
- <span data-ttu-id="b04c8-136">TCP トラフィックを許可する (および場合によってはユーザー データグラム プロトコル (UDP) トラフィック) に、SQL Server インスタンスを使用するポートで、ハードウェアまたはソフトウェア ファイアウォールを構成します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-136">Configure any hardware or software firewalls to allow TCP traffic (and in some cases User Datagram Protocol (UDP) traffic) on the ports that the SQL Server instance uses.</span></span>

<span data-ttu-id="b04c8-137">TCP/IP 経由で通信するために SQL Server を有効にするのにには、SQL Server インスタンスのネットワーク構成を変更するのに SQL Server 構成マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-137">To enable SQL Server to communicate over TCP/IP, use SQL Server Configuration Manager to change the network configuration for your SQL Server instance.</span></span>

<span data-ttu-id="b04c8-138">**SQL Server で TCP/IP を使用して通信を有効にするには**</span><span class="sxs-lookup"><span data-stu-id="b04c8-138">**To enable SQL Server to communicate using TCP/IP**</span></span>

1. <span data-ttu-id="b04c8-139">**開始** メニューのをポイント**すべてのプログラム**、 をクリックして**Microsoft SQL Server 2008 R2**、 をクリックして**構成ツール**をクリックし、**SQL Server 構成マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-139">On the **Start** menu, point to **All Programs**, click **Microsoft SQL Server 2008 R2**, click **Configuration Tools**, and then click **SQL Server Configuration Manager**.</span></span>
2. <span data-ttu-id="b04c8-140">ツリー ビュー ペインで、展開**SQL Server ネットワーク構成**、クリックして**MSSQLSERVER のプロトコル**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-140">In the tree view pane, expand **SQL Server Network Configuration**, and then click **Protocols for MSSQLSERVER**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b04c8-141">SQL Server の複数のインスタンスをインストールする場合が表示されます、**プロトコル***[インスタンス名]*インスタンスごとの項目。</span><span class="sxs-lookup"><span data-stu-id="b04c8-141">If you have installed multiple instances of SQL Server, you'll see a **Protocols for***[instance name]* item for each instance.</span></span> <span data-ttu-id="b04c8-142">インスタンスで、インスタンスごとにネットワーク設定を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-142">You need to configure network settings on an instance-by-instance basis.</span></span>
3. <span data-ttu-id="b04c8-143">詳細ウィンドウで右クリックし、 **TCP/IP**行をクリックして**を有効にする**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-143">In the details pane, right-click the **TCP/IP** row, and then click **Enable**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. <span data-ttu-id="b04c8-144">**警告**ダイアログ ボックスで、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-144">In the **Warning** dialog box, click **OK**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. <span data-ttu-id="b04c8-145">新しいネットワーク構成を有効にするには、MSSQLSERVER サービスを再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-145">You need to restart the MSSQLSERVER service before your new network configuration will take effect.</span></span> <span data-ttu-id="b04c8-146">サービス コンソールで、または SQL Server Management Studio から、コマンド プロンプトでを実行できます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-146">You can do that at a command prompt, from the Services console, or from SQL Server Management Studio.</span></span> <span data-ttu-id="b04c8-147">この手順では SQL Server Management Studio を使用します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-147">In this procedure, you'll use SQL Server Management Studio.</span></span>
6. <span data-ttu-id="b04c8-148">SQL Server 構成マネージャーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-148">Close SQL Server Configuration Manager.</span></span>
7. <span data-ttu-id="b04c8-149">**開始** メニューのをポイント**すべてのプログラム**、 をクリックして**Microsoft SQL Server 2008 R2**、クリックして**SQL Server Management Studio**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-149">On the **Start** menu, point to **All Programs**, click **Microsoft SQL Server 2008 R2**, and then click **SQL Server Management Studio**.</span></span>
8. <span data-ttu-id="b04c8-150">**サーバーへの接続** ダイアログ ボックスで、**サーバー名**ボックスで、データベース サーバーの名前を入力し、をクリックして**接続**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-150">In the **Connect to Server** dialog box, in the **Server name** box, type the name of the database server, and then click **Connect**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. <span data-ttu-id="b04c8-151">**オブジェクト エクスプ ローラー**  ウィンドウで、親サーバー ノードを右クリックして (たとえば、 **TESTDB1**)、をクリックし、**再起動**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-151">In the **Object Explorer** pane, right-click the parent server node (for example, **TESTDB1**), and then click **Restart**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. <span data-ttu-id="b04c8-152">**Microsoft SQL Server Management Studio**ダイアログ ボックスで、をクリックして**はい**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-152">In the **Microsoft SQL Server Management Studio** dialog box, click **Yes**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. <span data-ttu-id="b04c8-153">サービスが再起動されたときに、SQL Server Management Studio を閉じます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-153">When the service has restarted, close SQL Server Management Studio.</span></span>

<span data-ttu-id="b04c8-154">SQL Server ファイアウォールを通過するトラフィックを許可するのには、まず、SQL Server インスタンスを使用するポートを知っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-154">To allow SQL Server traffic through a firewall, you first need to know which ports your SQL Server instance is using.</span></span> <span data-ttu-id="b04c8-155">これは、SQL Server インスタンスを作成および構成方法に依存は。</span><span class="sxs-lookup"><span data-stu-id="b04c8-155">This will depend on how the SQL Server instance was created and configured:</span></span>

- <span data-ttu-id="b04c8-156">A*既定のインスタンス*SQL Server の受信 (およびに応答) の TCP ポート 1433 で要求します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-156">A *default instance* of SQL Server listens for (and responds to) requests on TCP port 1433.</span></span>
- <span data-ttu-id="b04c8-157">A*名前付きインスタンス*SQL Server のリッスン (し応答する)、動的に割り当てられる TCP ポートで要求します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-157">A *named instance* of SQL Server listens for (and responds to) requests on a dynamically assigned TCP port.</span></span>
- <span data-ttu-id="b04c8-158">SQL Server Browser サービスが有効になっている場合、クライアントは UDP ポート 1434 を特定の SQL Server インスタンスを使用する TCP ポートを調べるには上のサービスをクエリできます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-158">If the SQL Server Browser service is enabled, clients can query the service on UDP port 1434 to find out which TCP port to use for a particular SQL Server instance.</span></span> <span data-ttu-id="b04c8-159">ただし、このサービスは、多くの場合、セキュリティの理由により無効です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-159">However, this service is often disabled for security reasons.</span></span>

<span data-ttu-id="b04c8-160">SQL Server の既定のインスタンスを使用するいると想定されるので、トラフィックを許可するようにファイアウォールを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-160">Assuming that you're using a default instance of SQL Server, you need to configure your firewall to allow traffic.</span></span>

| <span data-ttu-id="b04c8-161">Direction</span><span class="sxs-lookup"><span data-stu-id="b04c8-161">Direction</span></span> | <span data-ttu-id="b04c8-162">ポートから</span><span class="sxs-lookup"><span data-stu-id="b04c8-162">From Port</span></span> | <span data-ttu-id="b04c8-163">ポートに</span><span class="sxs-lookup"><span data-stu-id="b04c8-163">To Port</span></span> | <span data-ttu-id="b04c8-164">ポートの種類</span><span class="sxs-lookup"><span data-stu-id="b04c8-164">Port Type</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b04c8-165">受信</span><span class="sxs-lookup"><span data-stu-id="b04c8-165">Inbound</span></span> | <span data-ttu-id="b04c8-166">どれでも可</span><span class="sxs-lookup"><span data-stu-id="b04c8-166">Any</span></span> | <span data-ttu-id="b04c8-167">1433</span><span class="sxs-lookup"><span data-stu-id="b04c8-167">1433</span></span> | <span data-ttu-id="b04c8-168">TCP</span><span class="sxs-lookup"><span data-stu-id="b04c8-168">TCP</span></span> |
| <span data-ttu-id="b04c8-169">送信</span><span class="sxs-lookup"><span data-stu-id="b04c8-169">Outbound</span></span> | <span data-ttu-id="b04c8-170">1433</span><span class="sxs-lookup"><span data-stu-id="b04c8-170">1433</span></span> | <span data-ttu-id="b04c8-171">どれでも可</span><span class="sxs-lookup"><span data-stu-id="b04c8-171">Any</span></span> | <span data-ttu-id="b04c8-172">TCP</span><span class="sxs-lookup"><span data-stu-id="b04c8-172">TCP</span></span> |
  

> [!NOTE]
> <span data-ttu-id="b04c8-173">技術的には、クライアント コンピューターは、SQL Server との通信に 1024 ~ 5000 でランダムに割り当てられた TCP ポートを使用し、それに従ってファイアウォールのルールを制限することができます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-173">Technically, a client computer will use a randomly assigned TCP port between 1024 and 5000 to communicate with SQL Server, and you can restrict your firewall rules accordingly.</span></span> <span data-ttu-id="b04c8-174">SQL Server のポートとファイアウォールの詳細については、次を参照してください[SQL ファイアウォール経由で通信するために必要な TCP/IP ポート番号](https://go.microsoft.com/?linkid=9805125)と[する方法: 特定の TCP ポート (SQL Server の構成でリッスンするようにサーバーを構成する。Manager)](https://msdn.microsoft.com/en-us/library/ms177440.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-174">For more information on SQL Server ports and firewalls, see [TCP/IP port numbers required to communicate to SQL over a firewall](https://go.microsoft.com/?linkid=9805125) and [How to: Configure a Server to Listen on a Specific TCP Port (SQL Server Configuration Manager)](https://msdn.microsoft.com/en-us/library/ms177440.aspx).</span></span>


<span data-ttu-id="b04c8-175">ほとんどの Windows Server 環境では、可能性があります、データベース サーバーで Windows ファイアウォールを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-175">In most Windows Server environments, you'll likely have to configure Windows Firewall on the database server.</span></span> <span data-ttu-id="b04c8-176">既定では、Windows ファイアウォールは、ルールによって特に禁止されている場合を除き、すべての送信トラフィックを許可します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-176">By default, Windows Firewall allows all outbound traffic unless a rule specifically prohibits it.</span></span> <span data-ttu-id="b04c8-177">Web サーバーをデータベースにアクセスを有効にするには、SQL Server インスタンスが使用するポート番号で TCP トラフィックを許可する受信規則を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-177">To enable your web server to reach your database, you need to configure an inbound rule that allows TCP traffic on the port number that the SQL Server instance uses.</span></span> <span data-ttu-id="b04c8-178">SQL Server の既定のインスタンスを使用している場合は、このルールを構成する次の手順を使用できます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-178">If you're using a default instance of SQL Server, you can use the next procedure to configure this rule.</span></span>

<span data-ttu-id="b04c8-179">**既定の SQL Server インスタンスとの通信を許可するための Windows ファイアウォールを構成するには**</span><span class="sxs-lookup"><span data-stu-id="b04c8-179">**To configure Windows Firewall to allow communication with a default SQL Server instance**</span></span>

1. <span data-ttu-id="b04c8-180">データベース サーバー上で、**開始** メニューのをポイント**管理ツール**、順にクリック**セキュリティが強化された Windows ファイアウォール**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-180">On the database server, on the **Start** menu, point to **Administrative Tools**, and then click **Windows Firewall with Advanced Security**.</span></span>
2. <span data-ttu-id="b04c8-181">ツリー ビュー ウィンドウでをクリックして**受信の規則**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-181">In the tree view pane, click **Inbound Rules**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. <span data-ttu-id="b04c8-182">**アクション**ペイン、下にある**受信の規則**、 をクリックして**新しいルール**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-182">In the **Actions** pane, under **Inbound Rules**, click **New Rule**.</span></span>
4. <span data-ttu-id="b04c8-183">新規の受信の規則ウィザードでの**規則の種類**] ページで、[**ポート**、順にクリック**[次へ]**。</span><span class="sxs-lookup"><span data-stu-id="b04c8-183">In the New Inbound Rule Wizard, on the **Rule Type** page, select **Port**, and then click **Next**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. <span data-ttu-id="b04c8-184">**プロトコルおよびポート**いることを確認] ページで、 **TCP**が選択されているし、[、**特定のローカル ポート**ボックスに、入力**1433**をクリックし、**次**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-184">On the **Protocol and Ports** page, ensure that **TCP** is selected, and in the **Specific local ports** box, type **1433**, and then click **Next**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. <span data-ttu-id="b04c8-185">**アクション** ページのままに**接続を許可する**を選択し、をクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-185">On the **Action** page, leave **Allow the connection** selected and click **Next**.</span></span>
7. <span data-ttu-id="b04c8-186">**プロファイル** ページのままにして**ドメイン**選択すると、オフ、**プライベート**と**パブリック**チェック ボックスをオンし、をクリックして**次へ**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-186">On the **Profile** page, leave **Domain** selected, clear the **Private** and **Public** check boxes, and then click **Next**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. <span data-ttu-id="b04c8-187">**名** ページで、ルールに適切にわかりやすい名前を付けます (たとえば、 **SQL Server の既定のインスタンス: ネットワーク アクセス**)、をクリックし、 **完了**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-187">On the **Name** page, give the rule a suitably descriptive name (for example, **SQL Server default instance – network access**), and then click **Finish**.</span></span>

<span data-ttu-id="b04c8-188">SQL Server と標準または動的ポート経由の通信を参照して必要がある場合に特に、SQL Server 用 Windows ファイアウォールを構成する方法についての[する方法: データベース エンジン アクセスの Windows ファイアウォールを構成する](https://technet.microsoft.com/en-us/library/ms175043.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-188">For more information on configuring Windows Firewall for SQL Server, particularly if you need to communicate with SQL Server over non-standard or dynamic ports, see [How to: Configure a Windows Firewall for Database Engine Access](https://technet.microsoft.com/en-us/library/ms175043.aspx).</span></span>

## <a name="configure-logins-and-database-permissions"></a><span data-ttu-id="b04c8-189">ログインを構成し、データベース権限</span><span class="sxs-lookup"><span data-stu-id="b04c8-189">Configure Logins and Database Permissions</span></span>

<span data-ttu-id="b04c8-190">Web アプリケーションにインターネット インフォメーション サービス (IIS) を展開するときに、アプリケーション プールの id を使用して、アプリケーションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-190">When you deploy a web application to Internet Information Services (IIS), the application runs using the identity of the application pool.</span></span> <span data-ttu-id="b04c8-191">ドメイン環境では、アプリケーション プール id は、ネットワーク リソースにアクセスするが、実行するサーバーのマシン アカウントを使用します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-191">In a domain environment, application pool identities use the machine account of the server on which they run to access network resources.</span></span> <span data-ttu-id="b04c8-192">コンピューター アカウントは、形式をとる*[ドメイン名]***\***[マシン名] ***$**& #x 2014 たとえば、 **FABRIKAM\。TESTWEB1$**します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-192">Machine accounts take the form *[domain name]***\***[machine name]***$**&#x2014;for example, **FABRIKAM\TESTWEB1$**.</span></span> <span data-ttu-id="b04c8-193">Web アプリケーションをネットワーク経由で、データベースへのアクセスを許可するのには、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-193">To allow your web application to access a database across the network, you need to:</span></span>

- <span data-ttu-id="b04c8-194">SQL Server インスタンスに、web サーバーのコンピューター アカウントのログインを追加します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-194">Add a login for the web server machine account to the SQL Server instance.</span></span>
- <span data-ttu-id="b04c8-195">必要なデータベース ロールにマシン アカウントのログインをマップ (通常**db\_datareader**と**db\_datawriter**)。</span><span class="sxs-lookup"><span data-stu-id="b04c8-195">Map the machine account login to any required database roles (typically **db\_datareader** and **db\_datawriter**).</span></span>

<span data-ttu-id="b04c8-196">Web アプリケーションが 1 台のサーバーではなく、サーバー ファームで実行されている場合は、サーバー ファーム内のすべての web サーバーに対してこれらの手順を繰り返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-196">If your web application is running on a server farm, rather than a single server, you'll need to repeat these procedures for every web server in the server farm.</span></span>

> [!NOTE]
> <span data-ttu-id="b04c8-197">アプリケーション プールの id とネットワークのリソースへのアクセスの詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-197">For more information on application pool identities and accessing network resources, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="b04c8-198">さまざまな方法でこれらのタスクを得ることができます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-198">You can approach these tasks in various ways.</span></span> <span data-ttu-id="b04c8-199">ログインを作成するには、方法があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-199">To create the login, you can either:</span></span>

- <span data-ttu-id="b04c8-200">TRANSACT-SQL または SQL Server Management Studio を使用して、データベース サーバーで手動でログインを作成します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-200">Create the login manually on the database server, using Transact-SQL or SQL Server Management Studio.</span></span>
- <span data-ttu-id="b04c8-201">Visual Studio で作成し、ログインを展開する SQL Server 2008 サーバー プロジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-201">Use a SQL Server 2008 Server Project in Visual Studio to create and deploy the login.</span></span>

<span data-ttu-id="b04c8-202">SQL Server ログインを展開するデータベースに依存しないように、データベース レベルのオブジェクトではなく、サーバー レベル オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="b04c8-202">A SQL Server login is a server-level object, rather than a database-level object, so it's not dependent on the database you want to deploy.</span></span> <span data-ttu-id="b04c8-203">そのため、任意の時点では、ログインを作成することができ、データベースのデプロイを開始する前に、データベース サーバーでログインを手動で作成する最も簡単な方法が多くの場合は。</span><span class="sxs-lookup"><span data-stu-id="b04c8-203">As such, you can create the login at any point, and the easiest approach is often to create the login manually on the database server before you start deploying databases.</span></span> <span data-ttu-id="b04c8-204">次の手順を使用して、SQL Server Management Studio でのログインを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-204">You can use the next procedure to create a login in SQL Server Management Studio.</span></span>

<span data-ttu-id="b04c8-205">**Web サーバー コンピューター アカウントに SQL Server ログインを作成するには**</span><span class="sxs-lookup"><span data-stu-id="b04c8-205">**To create a SQL Server login for the web server machine account**</span></span>

1. <span data-ttu-id="b04c8-206">データベース サーバー上で、**開始** メニューのをポイント**すべてのプログラム**、 をクリックして**Microsoft SQL Server 2008 R2**をクリックし、 **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="b04c8-206">On the database server, on the **Start** menu, point to **All Programs**, click **Microsoft SQL Server 2008 R2**, and then click **SQL Server Management Studio**.</span></span>
2. <span data-ttu-id="b04c8-207">**サーバーへの接続** ダイアログ ボックスで、**サーバー名**ボックスで、データベース サーバーの名前を入力し、をクリックして**接続**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-207">In the **Connect to Server** dialog box, in the **Server name** box, type the name of the database server, and then click **Connect**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. <span data-ttu-id="b04c8-208">**オブジェクト エクスプ ローラー**  ウィンドウを右クリックして**セキュリティ**、 をポイント**新規**、クリックして**ログイン**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-208">In the **Object Explorer** pane, right-click **Security**, point to **New**, and then click **Login**.</span></span>
4. <span data-ttu-id="b04c8-209">**ログイン-新規** ダイアログ ボックスで、**ログイン名**ボックスに、web サーバー マシン アカウントの名前を入力 (たとえば、 **FABRIKAM\TESTWEB1$**)。</span><span class="sxs-lookup"><span data-stu-id="b04c8-209">In the **Login – New** dialog box, in the **Login name** box, type the name of your web server machine account (for example, **FABRIKAM\TESTWEB1$**).</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. <span data-ttu-id="b04c8-210">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b04c8-210">Click **OK**.</span></span>

<span data-ttu-id="b04c8-211">この時点では、データベース サーバーは、Web Deploy の発行可能な状態です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-211">At this point, your database server is ready for Web Deploy publishing.</span></span> <span data-ttu-id="b04c8-212">ただし、展開するソリューションは、必要なデータベース ロールにマシン アカウントのログインをマップするまでに動作しません。</span><span class="sxs-lookup"><span data-stu-id="b04c8-212">However, any solutions you deploy won't work until you map the machine account login to the required database roles.</span></span> <span data-ttu-id="b04c8-213">考えるより多くのデータベース ロールにログインにマッピングが必要です、することはできません後まで、マップのロール配置したデータベースです。</span><span class="sxs-lookup"><span data-stu-id="b04c8-213">Mapping the login to database roles requires a lot more thought, as you can't map roles until after you've deployed the database.</span></span> <span data-ttu-id="b04c8-214">必要なデータベース ロールにマシン アカウントのログインをマップするには、方法があります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-214">To map the machine account login to the required database roles, you can either:</span></span>

- <span data-ttu-id="b04c8-215">データベース ロールを割り当てるログインに手動で、最初にデータベースを配置した後にします。</span><span class="sxs-lookup"><span data-stu-id="b04c8-215">Assign the database roles to the login manually, after you've deployed the database for the first time.</span></span>
- <span data-ttu-id="b04c8-216">配置後スクリプトを使用して、ログインにデータベース ロールを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-216">Use a post-deployment script to assign the database roles to the login.</span></span>

<span data-ttu-id="b04c8-217">ログインとデータベース ロールのマッピングの作成を自動化する方法については、次を参照してください。[を展開するデータベース ロール メンバーシップを変更するテスト環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-217">For more information on automating the creation of logins and database role mappings, see [Deploying Database Role Memberships to Test Environments](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).</span></span> <span data-ttu-id="b04c8-218">代わりに、次の手順を使用して、必要なデータベース ロールに手動でコンピューター アカウントのログインをマップすることができます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-218">Alternatively, you can use the next procedure to map the machine account login to the required database roles manually.</span></span> <span data-ttu-id="b04c8-219">までこの手順を実行することはできませんに注意してください*後*データベースを配置しました。</span><span class="sxs-lookup"><span data-stu-id="b04c8-219">Remember that you can't perform this procedure until *after* you've deployed the database.</span></span>

<span data-ttu-id="b04c8-220">**データベース ロールを web サーバーのマシン アカウントのログインにマップするには**</span><span class="sxs-lookup"><span data-stu-id="b04c8-220">**To map database roles to the web server machine account login**</span></span>

1. <span data-ttu-id="b04c8-221">以前と同様、SQL Server Management Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-221">Open SQL Server Management Studio as before.</span></span>
2. <span data-ttu-id="b04c8-222">**オブジェクト エクスプ ローラー**  ウィンドウで、展開、**セキュリティ**ノード、展開、**ログイン**ノード、マシン アカウントのログインをダブルクリックし、(たとえば、 **FABRIKAM\TESTWEB1$**)。</span><span class="sxs-lookup"><span data-stu-id="b04c8-222">In the **Object Explorer** pane, expand the **Security** node, expand the **Logins** node, and then double-click the machine account login (for example, **FABRIKAM\TESTWEB1$**).</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. <span data-ttu-id="b04c8-223">**ログイン プロパティ**ダイアログ ボックスで、をクリックして**ユーザー マッピング**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-223">In the **Login Properties** dialog box, click **User Mapping**.</span></span>
4. <span data-ttu-id="b04c8-224">**このログインにマップされたユーザー**テーブルで、データベースの名前を選択 (たとえば、 **ContactManager**)。</span><span class="sxs-lookup"><span data-stu-id="b04c8-224">In the **Users mapped to this login** table, select the name of your database (for example, **ContactManager**).</span></span>
5. <span data-ttu-id="b04c8-225">**データベース ロールのメンバーシップ:** *[データベース名]*一覧で、必要なアクセス許可を選択します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-225">In the **Database role membership for:** *[database name]* list, select the permissions required.</span></span> <span data-ttu-id="b04c8-226">選択する必要があります、連絡先のマネージャーのサンプル ソリューションの場合、 **db\_datareader**と**db\_datawriter**ロール。</span><span class="sxs-lookup"><span data-stu-id="b04c8-226">In the case of the Contact Manager sample solution, you must select the **db\_datareader** and **db\_datawriter** roles.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. <span data-ttu-id="b04c8-227">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b04c8-227">Click **OK**.</span></span>

<span data-ttu-id="b04c8-228">データベース ロールを手動でマッピングでは、テスト環境の複数の適切な多くの場合は、ステージング環境または実稼働環境に自動または 1 回のクリックは展開の望ましいです。</span><span class="sxs-lookup"><span data-stu-id="b04c8-228">While manually mapping database roles is often more than adequate for test environments, it's less desirable for automated or one-click deployments to staging or production environments.</span></span> <span data-ttu-id="b04c8-229">詳細についてはこの種の配置後のスクリプトを使用して作業を自動化することで見つかります[を展開するデータベース ロール メンバーシップを変更するテスト環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-229">You can find more information on automating this kind of task using post-deployment scripts in [Deploying Database Role Memberships to Test Environments](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b04c8-230">サーバーのプロジェクトおよびデータベースの詳細については、次を参照してください。 [Visual Studio 2010 の SQL Server データベース プロジェクト](https://msdn.microsoft.com/en-us/library/ff678491.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-230">For more information on server projects and database projects, see [Visual Studio 2010 SQL Server Database Projects](https://msdn.microsoft.com/en-us/library/ff678491.aspx).</span></span>


## <a name="configure-permissions-for-the-deployment-account"></a><span data-ttu-id="b04c8-231">展開アカウントのアクセス許可を構成します。</span><span class="sxs-lookup"><span data-stu-id="b04c8-231">Configure Permissions for the Deployment Account</span></span>

<span data-ttu-id="b04c8-232">展開を実行するときに使用するアカウントが SQL Server の管理者でない場合は、このアカウントのログインを作成する必要ありますも。</span><span class="sxs-lookup"><span data-stu-id="b04c8-232">If the account that you'll use to run the deployment is not a SQL Server administrator, you'll also need to create a login for this account.</span></span> <span data-ttu-id="b04c8-233">アカウント データベースを作成するためのメンバーである必要があります、 **dbcreator**サーバーの役割と同等の権限を持っているか。</span><span class="sxs-lookup"><span data-stu-id="b04c8-233">In order to create the database, the account must be a member of the **dbcreator** server role or have equivalent permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="b04c8-234">データベースを配置する Web デプロイまたは VSDBCMD を使用する場合は、(SQL Server のインスタンスが混合モード認証をサポートするために構成されている) 場合、Windows 資格情報または SQL Server 資格情報を使用できます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-234">When you use Web Deploy or VSDBCMD to deploy a database, you can use Windows credentials or SQL Server credentials (if your SQL Server instance is configured to support mixed mode authentication).</span></span> <span data-ttu-id="b04c8-235">次の手順には、Windows 資格情報を使用するが、展開を構成するときに、接続文字列での SQL Server のユーザー名とパスワードの指定から停止する nothing を使用する必要があることが前提とします。</span><span class="sxs-lookup"><span data-stu-id="b04c8-235">The next procedure assumes that you want to use Windows credentials, but there's nothing stopping you from specifying a SQL Server user name and password in your connection string when you configure the deployment.</span></span>


<span data-ttu-id="b04c8-236">**展開アカウントのアクセス許可を設定するには**</span><span class="sxs-lookup"><span data-stu-id="b04c8-236">**To set up permissions for the deployment account**</span></span>

1. <span data-ttu-id="b04c8-237">以前と同様、SQL Server Management Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-237">Open SQL Server Management Studio as before.</span></span>
2. <span data-ttu-id="b04c8-238">**オブジェクト エクスプ ローラー**  ウィンドウを右クリックして**セキュリティ**、 をポイント**新規**、クリックして**ログイン**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-238">In the **Object Explorer** pane, right-click **Security**, point to **New**, and then click **Login**.</span></span>
3. <span data-ttu-id="b04c8-239">**ログイン-新規** ダイアログ ボックスで、**ログイン名**ボックスに、展開アカウントの名前を入力 (たとえば、 **FABRIKAM\matt**)。</span><span class="sxs-lookup"><span data-stu-id="b04c8-239">In the **Login – New** dialog box, in the **Login name** box, type the name of your deployment account (for example, **FABRIKAM\matt**).</span></span>
4. <span data-ttu-id="b04c8-240">**ページの選択** ウィンドウで、をクリックして**サーバーの役割**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-240">In the **Select a page** pane, click **Server Roles**.</span></span>
5. <span data-ttu-id="b04c8-241">選択**dbcreator**、順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-241">Select **dbcreator**, and then click **OK**.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

<span data-ttu-id="b04c8-242">後続のデプロイをサポートするためにもする必要がありますに展開するアカウントを追加、 **db\_所有者**最初の展開後に、データベースのロール。</span><span class="sxs-lookup"><span data-stu-id="b04c8-242">To support subsequent deployments, you'll also need to add the deploying account to the **db\_owner** role on the database after the first deployment.</span></span> <span data-ttu-id="b04c8-243">後続のデプロイで、既存のデータベースのスキーマの変更はなくする新しいデータベースを作成するためです。</span><span class="sxs-lookup"><span data-stu-id="b04c8-243">This is because on subsequent deployments you're modifying the schema of an existing database, rather than creating a new database.</span></span> <span data-ttu-id="b04c8-244">前のセクションで説明した、明確な理由により、データベースを作成するまで、データベース ロールにユーザーを追加することはできません。</span><span class="sxs-lookup"><span data-stu-id="b04c8-244">As described in the previous section, you can't add a user to a database role until you've created the database, for obvious reasons.</span></span>

<span data-ttu-id="b04c8-245">**データベースに展開アカウントのログインをマップする\_データベース ロールの所有者**</span><span class="sxs-lookup"><span data-stu-id="b04c8-245">**To map the deployment account login to the db\_owner database role**</span></span>

1. <span data-ttu-id="b04c8-246">以前と同様、SQL Server Management Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="b04c8-246">Open SQL Server Management Studio as before.</span></span>
2. <span data-ttu-id="b04c8-247">**オブジェクト エクスプ ローラー**ウィンドウで、展開、**セキュリティ**ノード、展開、**ログイン**ノード、マシン アカウントのログインをダブルクリックし、(たとえば、 **FABRIKAM\matt**)。</span><span class="sxs-lookup"><span data-stu-id="b04c8-247">In the **Object Explorer** window, expand the **Security** node, expand the **Logins** node, and then double-click the machine account login (for example, **FABRIKAM\matt**).</span></span>
3. <span data-ttu-id="b04c8-248">**ログイン プロパティ**ダイアログ ボックスで、をクリックして**ユーザー マッピング**です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-248">In the **Login Properties** dialog box, click **User Mapping**.</span></span>
4. <span data-ttu-id="b04c8-249">**このログインにマップされたユーザー**テーブルで、データベースの名前を選択 (たとえば、 **ContactManager**)。</span><span class="sxs-lookup"><span data-stu-id="b04c8-249">In the **Users mapped to this login** table, select the name of your database (for example, **ContactManager**).</span></span>
5. <span data-ttu-id="b04c8-250">**データベース ロールのメンバーシップ:** *[データベース名]*一覧で、、 **db\_所有者**ロール。</span><span class="sxs-lookup"><span data-stu-id="b04c8-250">In the **Database role membership for:** *[database name]* list, select the **db\_owner** role.</span></span>

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. <span data-ttu-id="b04c8-251">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b04c8-251">Click **OK**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b04c8-252">まとめ</span><span class="sxs-lookup"><span data-stu-id="b04c8-252">Conclusion</span></span>

<span data-ttu-id="b04c8-253">データベース サーバーをリモート データベースの配置をそのまま使用して、データベースにアクセスするリモートの IIS web サーバーを許可する準備ができてはずです。</span><span class="sxs-lookup"><span data-stu-id="b04c8-253">Your database server should now be ready to accept remote database deployments and to allow remote IIS web servers to access your databases.</span></span> <span data-ttu-id="b04c8-254">展開してデータベースを使用しようとすると、前にこれらの要点をチェックすることがあります。</span><span class="sxs-lookup"><span data-stu-id="b04c8-254">Before you attempt to deploy and use databases, you may want to check these key points:</span></span>

- <span data-ttu-id="b04c8-255">リモートの TCP/IP 接続を受け入れるように SQL Server を構成しましたか。</span><span class="sxs-lookup"><span data-stu-id="b04c8-255">Have you configured SQL Server to accept remote TCP/IP connections?</span></span>
- <span data-ttu-id="b04c8-256">SQL Server トラフィックを許可するようにファイアウォールを構成しましたか。</span><span class="sxs-lookup"><span data-stu-id="b04c8-256">Have you configured any firewalls to permit SQL Server traffic?</span></span>
- <span data-ttu-id="b04c8-257">SQL Server にアクセスするすべての web サーバーのマシン アカウントのログインを作成しましたか。</span><span class="sxs-lookup"><span data-stu-id="b04c8-257">Have you created a machine account login for every web server that will access SQL Server?</span></span>
- <span data-ttu-id="b04c8-258">データベースの配置は、ユーザー ロールのマッピングを作成するためのスクリプトを含めますが最初にデータベースを配置した後に手動で作成これらする必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="b04c8-258">Does your database deployment include a script to create user role mappings, or do you need to create these manually after you deploy the database for the first time?</span></span>
- <span data-ttu-id="b04c8-259">展開アカウントのログインを作成し、追加してに、 **dbcreator**サーバーの役割ですか?</span><span class="sxs-lookup"><span data-stu-id="b04c8-259">Have you created a login for the deployment account and added it to the **dbcreator** server role?</span></span>

## <a name="further-reading"></a><span data-ttu-id="b04c8-260">関連項目</span><span class="sxs-lookup"><span data-stu-id="b04c8-260">Further Reading</span></span>

<span data-ttu-id="b04c8-261">データベース プロジェクトを配置する方法については、次を参照してください。[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-261">For guidance on deploying database projects, see [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="b04c8-262">配置後スクリプトを実行してデータベース ロールのメンバーシップを作成する方法のガイダンスについては、次を参照してください。[を展開するデータベース ロール メンバーシップを変更するテスト環境](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-262">For guidance on creating database role memberships by running a post-deployment script, see [Deploying Database Role Memberships to Test Environments](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).</span></span> <span data-ttu-id="b04c8-263">メンバーシップ データベースがもたらされる独自の展開の課題に対応する方法のガイダンスについては、次を参照してください。[エンタープライズ環境にメンバーシップ データベースを展開する](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)です。</span><span class="sxs-lookup"><span data-stu-id="b04c8-263">For guidance on how to meet the unique deployment challenges that membership databases pose, see [Deploying Membership Databases to Enterprise Environments](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b04c8-264">[前へ](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
[次へ](creating-a-server-farm-with-the-web-farm-framework.md)</span><span class="sxs-lookup"><span data-stu-id="b04c8-264">[Previous](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
[Next](creating-a-server-farm-with-the-web-farm-framework.md)</span></span>
