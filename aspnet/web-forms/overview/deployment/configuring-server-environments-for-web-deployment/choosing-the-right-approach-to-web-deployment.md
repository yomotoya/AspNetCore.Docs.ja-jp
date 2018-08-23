---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Web 配置の適切なアプローチの選択 |Microsoft Docs
author: jrjlee
description: インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用する場合は、3 つのアプローチの取得に使用できます.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 0b21852a1db2862a8452e332021b55ce7f1db423
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834385"
---
<a name="choosing-the-right-approach-to-web-deployment"></a><span data-ttu-id="dc383-103">Web 配置の適切なアプローチを選択します。</span><span class="sxs-lookup"><span data-stu-id="dc383-103">Choosing the Right Approach to Web Deployment</span></span>
====================
<span data-ttu-id="dc383-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="dc383-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="dc383-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="dc383-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="dc383-106">インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用する場合は、3 つのアプローチを web サーバーにパッケージ化された web アプリケーションの取得に使用できます。</span><span class="sxs-lookup"><span data-stu-id="dc383-106">When you work with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 or later, there are three main approaches you can use to get your packaged web applications onto a web server.</span></span> <span data-ttu-id="dc383-107">方法があります。</span><span class="sxs-lookup"><span data-stu-id="dc383-107">You can either:</span></span>
> 
> - <span data-ttu-id="dc383-108">ターゲットにするリモートの場所からアプリケーションをデプロイ、 *Web Deployment Agent サービス*(別名、「リモート エージェント」)、移行先サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="dc383-108">Deploy the application from a remote location by targeting the *Web Deployment Agent Service* (also known as the "remote agent") on the destination server.</span></span>
> - <span data-ttu-id="dc383-109">オンデマンドで Web デプロイ (別名、「一時エージェント」) を使用してリモートの場所からアプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="dc383-109">Deploy the application from a remote location using Web Deploy On Demand (also known as the "temp agent").</span></span>
> - <span data-ttu-id="dc383-110">ターゲットにするリモートの場所からアプリケーションをデプロイ、 *IIS Web 配置ハンドラー*移行先サーバーでします。</span><span class="sxs-lookup"><span data-stu-id="dc383-110">Deploy the application from a remote location by targeting the *IIS Web Deploy Handler* on the destination server.</span></span>
> - <span data-ttu-id="dc383-111">手動で移行先サーバーに web パッケージをコピーして、IIS マネージャーからインポートすることによって、アプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="dc383-111">Deploy the application by manually copying the web package to the destination server and importing it through IIS Manager.</span></span>
> 
> <span data-ttu-id="dc383-112">変換先の web サーバーを構成する方法は、使用する展開する方法に左右されます。</span><span class="sxs-lookup"><span data-stu-id="dc383-112">How you configure your destination web servers will depend on which approach to deployment you want to use.</span></span> <span data-ttu-id="dc383-113">このトピックでは、自分に適した展開する方法を判断するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="dc383-113">This topic will help you decide which approach to deployment is right for you.</span></span>


<span data-ttu-id="dc383-114">次の表は、主な利点とそれぞれのアプローチを最も一般的に合わせてシナリオと共に各展開方法の短所を示します。</span><span class="sxs-lookup"><span data-stu-id="dc383-114">This table shows the main advantages and disadvantages of each deployment approach, together with the scenarios that most typically suit each approach.</span></span>

| <span data-ttu-id="dc383-115">方法</span><span class="sxs-lookup"><span data-stu-id="dc383-115">Approach</span></span> | <span data-ttu-id="dc383-116">長所</span><span class="sxs-lookup"><span data-stu-id="dc383-116">Advantages</span></span> | <span data-ttu-id="dc383-117">短所</span><span class="sxs-lookup"><span data-stu-id="dc383-117">Disadvantages</span></span> | <span data-ttu-id="dc383-118">一般的なシナリオ</span><span class="sxs-lookup"><span data-stu-id="dc383-118">Typical Scenarios</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="dc383-119">リモート エージェント</span><span class="sxs-lookup"><span data-stu-id="dc383-119">Remote Agent</span></span> | <span data-ttu-id="dc383-120">設定するには簡単です。</span><span class="sxs-lookup"><span data-stu-id="dc383-120">It is easy to set up.</span></span> <span data-ttu-id="dc383-121">Web アプリケーションとコンテンツへの通常の更新プログラムに適しています。</span><span class="sxs-lookup"><span data-stu-id="dc383-121">It is suitable for regular updates to web applications and content.</span></span> | <span data-ttu-id="dc383-122">ユーザーは、ターゲット サーバーの管理者である必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc383-122">The user must be an administrator on the target server.</span></span> <span data-ttu-id="dc383-123">ユーザーは、代替資格情報を提供できません。</span><span class="sxs-lookup"><span data-stu-id="dc383-123">the user can't supply alternative credentials.</span></span> | <span data-ttu-id="dc383-124">開発環境。</span><span class="sxs-lookup"><span data-stu-id="dc383-124">Development environments.</span></span> <span data-ttu-id="dc383-125">環境をテストします。</span><span class="sxs-lookup"><span data-stu-id="dc383-125">Test environments.</span></span> |
| <span data-ttu-id="dc383-126">一時エージェント</span><span class="sxs-lookup"><span data-stu-id="dc383-126">Temp Agent</span></span> | <span data-ttu-id="dc383-127">ターゲット コンピューターで Web 配置をインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="dc383-127">There is no need to install Web Deploy on the target computer.</span></span> <span data-ttu-id="dc383-128">最新バージョンの Web 配置は自動的に使用します。</span><span class="sxs-lookup"><span data-stu-id="dc383-128">The latest version of Web Deploy is automatically used.</span></span> | <span data-ttu-id="dc383-129">ユーザーは、ターゲット サーバーの管理者である必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc383-129">The user must be an administrator on the target server.</span></span> <span data-ttu-id="dc383-130">ユーザーは、代替資格情報を提供できません。</span><span class="sxs-lookup"><span data-stu-id="dc383-130">The user can't supply alternative credentials.</span></span> | <span data-ttu-id="dc383-131">開発環境。</span><span class="sxs-lookup"><span data-stu-id="dc383-131">Development environments.</span></span> <span data-ttu-id="dc383-132">環境をテストします。</span><span class="sxs-lookup"><span data-stu-id="dc383-132">Test environments.</span></span> |
| <span data-ttu-id="dc383-133">Web 配置ハンドラー</span><span class="sxs-lookup"><span data-stu-id="dc383-133">Web Deploy Handler</span></span> | <span data-ttu-id="dc383-134">管理者以外のユーザーには、コンテンツをデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="dc383-134">Non-administrator users can deploy content.</span></span> <span data-ttu-id="dc383-135">Web アプリケーションとコンテンツへの通常の更新プログラムに適しています。</span><span class="sxs-lookup"><span data-stu-id="dc383-135">It is suitable for regular updates to web applications and content.</span></span> | <span data-ttu-id="dc383-136">設定するずっと複雑です。</span><span class="sxs-lookup"><span data-stu-id="dc383-136">It is a lot more complex to set up.</span></span> | <span data-ttu-id="dc383-137">ステージング環境です。</span><span class="sxs-lookup"><span data-stu-id="dc383-137">Staging environments.</span></span> <span data-ttu-id="dc383-138">運用環境のイントラネットです。</span><span class="sxs-lookup"><span data-stu-id="dc383-138">Intranet production environments.</span></span> <span data-ttu-id="dc383-139">ホスト環境です。</span><span class="sxs-lookup"><span data-stu-id="dc383-139">Hosted environments.</span></span> |
| <span data-ttu-id="dc383-140">オフライン展開</span><span class="sxs-lookup"><span data-stu-id="dc383-140">Offline Deployment</span></span> | <span data-ttu-id="dc383-141">設定する非常に簡単になります。</span><span class="sxs-lookup"><span data-stu-id="dc383-141">It is very easy to set up.</span></span> <span data-ttu-id="dc383-142">分離環境に適しています。</span><span class="sxs-lookup"><span data-stu-id="dc383-142">It is suitable for isolated environments.</span></span> | <span data-ttu-id="dc383-143">サーバーの管理者は、手動でコピーして毎回 web パッケージをインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc383-143">The server administrator must manually copy and import the web package every time.</span></span> | <span data-ttu-id="dc383-144">運用環境をインターネットに接続します。</span><span class="sxs-lookup"><span data-stu-id="dc383-144">Internet-facing production environments.</span></span> <span data-ttu-id="dc383-145">ネットワーク分離環境。</span><span class="sxs-lookup"><span data-stu-id="dc383-145">Isolated network environments.</span></span> |
  

## <a name="using-the-remote-agent"></a><span data-ttu-id="dc383-146">リモート エージェントを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc383-146">Using the Remote Agent</span></span>

<span data-ttu-id="dc383-147">Web 配置の移行先サーバーで、既定の設定を使用してをインストールするときに Web Deployment Agent サービス (「リモート エージェント」) は自動的にインストールおよび開始します。</span><span class="sxs-lookup"><span data-stu-id="dc383-147">When you install Web Deploy using the default settings on a destination server, the Web Deployment Agent Service (the "remote agent") is automatically installed and started.</span></span> <span data-ttu-id="dc383-148">既定では、リモート エージェントは、このアドレスで HTTP エンドポイントを公開します。</span><span class="sxs-lookup"><span data-stu-id="dc383-148">By default, the remote agent exposes an HTTP endpoint at this address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> <span data-ttu-id="dc383-149">置き換えることができます [*server*] で、web サーバーのコンピューター名、web サーバー、またはホスト名の IP アドレスに解決される、web サーバー。</span><span class="sxs-lookup"><span data-stu-id="dc383-149">You can replace [*server*] with the machine name of your web server, an IP address for your web server, or a hostname that resolves to your web server.</span></span>


<span data-ttu-id="dc383-150">サーバー管理者は、このエンドポイント アドレスを指定することで、開発者のコンピューターや、ビルド サーバーなどのリモートの場所から web パッケージを展開できます。</span><span class="sxs-lookup"><span data-stu-id="dc383-150">Server administrators can deploy web packages from a remote location, like a developer machine or a build server, by specifying this endpoint address.</span></span> <span data-ttu-id="dc383-151">たとえば、Fabrikam, inc. の Matt 明では、自分の開発マシン上に構築された ContactManager.Mvc web アプリケーション プロジェクトがあるとします。</span><span class="sxs-lookup"><span data-stu-id="dc383-151">For example, suppose Matt Hink at Fabrikam, Inc. has built the ContactManager.Mvc web application project on his developer machine.</span></span> <span data-ttu-id="dc383-152">ビルド プロセスと組み合わせて、web のパッケージが生成されます、 *. deploy.cmd*パッケージをインストールするために必要な Web Deploy コマンドを含むファイル。</span><span class="sxs-lookup"><span data-stu-id="dc383-152">The build process generates a web package, together with a *.deploy.cmd* file that contains the Web Deploy commands required to install the package.</span></span> <span data-ttu-id="dc383-153">Matt が TESTWEB1 サーバーでのサーバー管理者の場合は、彼の開発者のコンピューターでこのコマンドを実行して、テスト web サーバーに web アプリケーションを展開彼できます。</span><span class="sxs-lookup"><span data-stu-id="dc383-153">If Matt is a server administrator on the TESTWEB1 server, he can deploy the web application to the test web server by running this command on his developer machine:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


<span data-ttu-id="dc383-154">実際には、Web デプロイの実行可能ファイルがリモート エージェントのエンドポイント アドレスを推論できる Matt がのみこれを入力する必要があるため、コンピューター名を指定する場合。</span><span class="sxs-lookup"><span data-stu-id="dc383-154">In actual fact, the Web Deploy executable can infer the endpoint address of the remote agent if you provide the machine name, so Matt only needs to type this:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="dc383-155">Web デプロイ コマンドライン構文の詳細については、 *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)。</span><span class="sxs-lookup"><span data-stu-id="dc383-155">For more information on Web Deploy command-line syntax and *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>


<span data-ttu-id="dc383-156">リモート エージェントは、リモートの場所からコンテンツを展開する簡単な方法を提供し、この方法でも 1 回のクリックや自動化されたデプロイでうまく機能します。</span><span class="sxs-lookup"><span data-stu-id="dc383-156">The remote agent offers a straightforward way to deploy content from a remote location, and this approach can work well with one-click or automated deployment.</span></span> <span data-ttu-id="dc383-157">ただし、デプロイ コマンドを実行しているユーザーがドメイン管理者または移行先サーバーでローカルの administrators グループのメンバーのいずれかあるも必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc383-157">However, the user who runs the deployment command must also be either a domain administrator or a member of the local administrators group on the destination server.</span></span> <span data-ttu-id="dc383-158">さらに、リモート エージェントは、コマンドラインで代替資格情報を渡すことはできませんので、基本認証をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="dc383-158">In addition, the remote agent doesn't support basic authentication, so you can't pass alternative credentials on the command line.</span></span>

<span data-ttu-id="dc383-159">リモート エージェントは、開発またはテスト シナリオで、開発者、テスト サーバー環境を完全な管理者に制御することは珍しくない、アプリケーションの再構築し非常に再デプロイは通常の展開に役立つアプローチを提供します。頻繁にします。</span><span class="sxs-lookup"><span data-stu-id="dc383-159">The remote agent provides a useful approach to deployment in development or test scenarios, where it's not uncommon for developers to have full administrator control over a test server environment, and applications are typically rebuilt and redeployed very frequently.</span></span> <span data-ttu-id="dc383-160">ただし、この手法は、通常、ステージング環境または運用環境の小さい問題です。</span><span class="sxs-lookup"><span data-stu-id="dc383-160">However, this approach is usually less acceptable for staging or production environments.</span></span>

<span data-ttu-id="dc383-161">リモート エージェントのアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web 展開のテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-161">For an end-to-end example of a scenario that uses the remote agent approach, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span>

## <a name="using-the-temp-agent"></a><span data-ttu-id="dc383-162">一時エージェントを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc383-162">Using the Temp Agent</span></span>

<span data-ttu-id="dc383-163">展開するための一時エージェント アプローチは、リモート エージェントのアプローチに似ています。</span><span class="sxs-lookup"><span data-stu-id="dc383-163">The temp agent approach to deployment is similar to the remote agent approach.</span></span> <span data-ttu-id="dc383-164">ただし、リモート エージェントのアプローチとは異なり、移行先の web サーバーで Web 配置をインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="dc383-164">However, in contrast to the remote agent approach, you don't need to install Web Deploy on the destination web server.</span></span> <span data-ttu-id="dc383-165">代わりに、展開を実行すると、Web、移行先サーバーで web デプロイ エージェントのサービスの一時的なバージョンをインストールし、展開これを使用して、コンテンツを IIS に展開します。</span><span class="sxs-lookup"><span data-stu-id="dc383-165">Instead, when you perform the deployment, Web Deploy will install a temporary version of the web deployment agent service on the destination server and will use this to deploy your content to IIS.</span></span> <span data-ttu-id="dc383-166">デプロイが完了したら、すべての一時ファイルは削除されます。</span><span class="sxs-lookup"><span data-stu-id="dc383-166">When the deployment is complete, all temporary files are removed.</span></span>

<span data-ttu-id="dc383-167">一時エージェント プロバイダー設定を使用してを追加する場合、 **/g**展開コマンドにフラグ。</span><span class="sxs-lookup"><span data-stu-id="dc383-167">If you want to use the temp agent provider setting, add the **/g** flag to your deployment command:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> <span data-ttu-id="dc383-168">サービスが実行されていない場合でも、対象のコンピューターで、web deployment agent サービスがインストールされている場合に、temp のエージェントを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="dc383-168">You can't use the temp agent if the web deployment agent service is installed on the destination computer, even if the service is not running.</span></span>


<span data-ttu-id="dc383-169">このアプローチの利点は、移行先サーバーへの Web 配置のインストールを維持する必要があることです。</span><span class="sxs-lookup"><span data-stu-id="dc383-169">The advantage of this approach is that you don't need to maintain installations of Web Deploy on your destination servers.</span></span> <span data-ttu-id="dc383-170">さらに、元と変換先のコンピューターが同じバージョンの Web 配置を実行していることを確認する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="dc383-170">Furthermore, you don't need to ensure that the source and destination computers are running the same version of Web Deploy.</span></span> <span data-ttu-id="dc383-171">ただし、このアプローチに苦しんでリモート エージェントのアプローチと同じプリンシパル制限 namely NTLM 認証のみがサポートされている、移行先サーバー上のローカル管理者は、コンテンツを展開するためにある必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc383-171">However, this approach suffers from the same principal limitations as the remote agent approach, namely that you must be a local administrator on the destination server in order to deploy content, and only NTLM authentication is supported.</span></span> <span data-ttu-id="dc383-172">一時エージェント アプローチには、移行先の環境の多くの初期構成も必要です。</span><span class="sxs-lookup"><span data-stu-id="dc383-172">The temp agent approach also requires a lot more initial configuration of the destination environment.</span></span>

<span data-ttu-id="dc383-173">一時エージェントの使用に関する詳細については、次を参照してください。[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)と[オンデマンドで Web デプロイ](https://technet.microsoft.com/library/ee517345(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-173">For more information on using the temp agent, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx) and [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span></span>

## <a name="using-the-web-deploy-handler"></a><span data-ttu-id="dc383-174">ハンドラー、Web を使用してデプロイ</span><span class="sxs-lookup"><span data-stu-id="dc383-174">Using the Web Deploy Handler</span></span>

<span data-ttu-id="dc383-175">IIS 7 以降では、Web Deploy は、IIS Web 配置ハンドラーを別の展開方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="dc383-175">For IIS 7 onwards, Web Deploy offers an alternative deployment approach through the IIS Web Deploy Handler.</span></span> <span data-ttu-id="dc383-176">Web 配置ハンドラーは、遠隔地から IIS の web サイトを管理するために設計された IIS Web 管理サービス (WMSvc) と密接に統合します。</span><span class="sxs-lookup"><span data-stu-id="dc383-176">The Web Deploy Handler is closely integrated with the IIS Web Management Service (WMSvc), which is designed to allow users to manage IIS websites from remote locations.</span></span>

<span data-ttu-id="dc383-177">既定では、リモート エージェントは、このアドレスで HTTP エンドポイントを公開します。</span><span class="sxs-lookup"><span data-stu-id="dc383-177">By default, the remote agent exposes an HTTP endpoint at this address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="dc383-178">置き換えることができます [*server*] で、web サーバーのコンピューター名、web サーバー、またはホスト名の IP アドレスに解決される、web サーバー。</span><span class="sxs-lookup"><span data-stu-id="dc383-178">You can replace [*server*] with the machine name of your web server, an IP address for your web server, or a hostname that resolves to your web server.</span></span>


<span data-ttu-id="dc383-179">リモート エージェント、および一時のエージェント経由で Web 配置ハンドラーの大きな利点は、特定の IIS web サイトにアプリケーションとコンテンツを展開する管理者以外のユーザーを許可するように IIS を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="dc383-179">The big advantage of the Web Deploy Handler over the remote agent, and the temp agent, is that you can configure IIS to allow non-administrator users to deploy applications and content to specific IIS websites.</span></span> <span data-ttu-id="dc383-180">Web 配置ハンドラーでは、Web Deploy コマンドでパラメーターとして別の資格情報を提供できるように、基本認証もサポートします。</span><span class="sxs-lookup"><span data-stu-id="dc383-180">The Web Deploy Handler also supports basic authentication, so you can provide alternative credentials as parameters in your Web Deploy commands.</span></span> <span data-ttu-id="dc383-181">主な短所は、Web 配置ハンドラーが最初にセットアップして構成をはるかに複雑です。</span><span class="sxs-lookup"><span data-stu-id="dc383-181">The major drawback is that the Web Deploy Handler is initially a lot more complicated to set up and configure.</span></span>

<span data-ttu-id="dc383-182">管理者以外のユーザーの場合 Web 管理サービス (WMSvc) のみにより、ユーザーを IIS に接続するサーバー レベルの接続ではなくサイト レベルの接続を使用します。</span><span class="sxs-lookup"><span data-stu-id="dc383-182">In the case of non-administrator users, the Web Management Service (WMSvc) will only allow the user to connect to IIS using a site-level connection, rather than a server-level connection.</span></span> <span data-ttu-id="dc383-183">特定のサイトにアクセスするには、エンドポイント アドレスのサイト固有のクエリ文字列を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="dc383-183">To access a particular site, you can include a site-specific query string in the endpoint address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


<span data-ttu-id="dc383-184">たとえば、ビルド プロセスが成功したビルドの後、ステージング環境への web アプリケーションを自動的に展開する構成されているとします。</span><span class="sxs-lookup"><span data-stu-id="dc383-184">For example, suppose a build process is configured to automatically deploy a web application to a staging environment after every successful build.</span></span> <span data-ttu-id="dc383-185">リモート エージェントのアプローチを使用した場合は、ビルド プロセスの id を移行先サーバーで管理者にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc383-185">If you used the remote agent approach, you'd need to make the build process identity an administrator on your destination servers.</span></span> <span data-ttu-id="dc383-186">これに対し、Web 配置ハンドラーのアプローチを使用することが管理者以外をユーザーに与える&#x2014;**FABRIKAM\stagingdeployer**ここで&#x2014;特定 IIS の web サイトのみ、およびビルド プロセスにアクセス許可は、これらを指定できますweb パッケージを展開する資格情報。</span><span class="sxs-lookup"><span data-stu-id="dc383-186">In contrast, using the Web Deploy Handler approach you can give a non-administrator user&#x2014;**FABRIKAM\stagingdeployer** in this case&#x2014;permission to a specific IIS website only, and the build process can provide these credentials to deploy the web package.</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> <span data-ttu-id="dc383-187">Web デプロイ コマンドライン操作と構文の詳細については、次を参照してください。 [Web 展開コマンド ライン リファレンス](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-187">For more information on Web Deploy command-line operations and syntax, see [Web Deploy Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span></span> <span data-ttu-id="dc383-188">使用しての詳細については、 *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-188">For more information on using the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>


<span data-ttu-id="dc383-189">Web 配置ハンドラーは、ステージング環境、ホストされている環境、およびサーバーへのリモート アクセスがあるが管理者の資格情報が、イントラネット ベースの実稼働環境で展開するのに役立つアプローチを提供します。</span><span class="sxs-lookup"><span data-stu-id="dc383-189">The Web Deploy Handler provides a useful approach to deployment in staging environments, hosted environments, and intranet-based production environments, where remote access to the server is available but administrator credentials are not.</span></span>

<span data-ttu-id="dc383-190">Web 配置ハンドラーのアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web デプロイのステージング環境を構成する](scenario-configuring-a-staging-environment-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-190">For an end-to-end example of a scenario that uses the Web Deploy Handler approach, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span>

## <a name="using-offline-deployment"></a><span data-ttu-id="dc383-191">オフライン展開を使用します。</span><span class="sxs-lookup"><span data-stu-id="dc383-191">Using Offline Deployment</span></span>

<span data-ttu-id="dc383-192">場合によっては、それが不可能または IIS の web サイトにリモートの場所からアプリケーションとコンテンツを展開するは実用的です。</span><span class="sxs-lookup"><span data-stu-id="dc383-192">In some cases, it's not possible or practical to deploy applications and content to an IIS website from a remote location.</span></span> <span data-ttu-id="dc383-193">たとえば、分離されたネットワークまたはネットワークのセグメントの元と変換先のコンピューターがあります。 またはファイアウォールのポリシーは、リモート アクセスを許可しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="dc383-193">For example, the source and destination computers may be in isolated networks or network segments, or firewall policy may not permit remote access.</span></span>

<span data-ttu-id="dc383-194">上記のようなシナリオで使用することできますも、パッケージ化と Web Deploy; の機能を公開だけは使用できませんにリモートの場所から。</span><span class="sxs-lookup"><span data-stu-id="dc383-194">In scenarios like these, you can still use the packaging and publishing capabilities of Web Deploy; you just can't use them from a remote location.</span></span> <span data-ttu-id="dc383-195">代わりに、移行先サーバーの管理者でする必要があります、サーバー上に web パッケージをコピーして、IIS マネージャーを使用をインポートします。</span><span class="sxs-lookup"><span data-stu-id="dc383-195">Instead, an administrator on the destination server must copy the web package onto the server and import it through IIS Manager.</span></span>

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

<span data-ttu-id="dc383-196">オフライン展開方法は、通常、場所、この境界ネットワーク内のサーバーは内部ネットワーク内のコンピューターとの接続と制限がある場合があります、インターネットに接続する実稼働環境で便利です。</span><span class="sxs-lookup"><span data-stu-id="dc383-196">The offline deployment approach is typically useful in Internet-facing production environments, where servers in a perimeter network may have restricted connectivity with computers in the internal network.</span></span>

<span data-ttu-id="dc383-197">オフライン展開のアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web 展開、運用環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-197">For an end-to-end example of a scenario that uses the offline deployment approach, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

## <a name="further-reading"></a><span data-ttu-id="dc383-198">関連項目</span><span class="sxs-lookup"><span data-stu-id="dc383-198">Further Reading</span></span>

<span data-ttu-id="dc383-199">Web デプロイ コマンドライン操作と構文の詳細については、次を参照してください。 [Web 展開コマンド ライン リファレンス](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-199">For more information on Web Deploy command-line operations and syntax, see [Web Deploy Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span></span> <span data-ttu-id="dc383-200">使用しての詳細については、 *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-200">For more information on using the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>

<span data-ttu-id="dc383-201">リモート コンピューターから web パッケージをデプロイするさまざまな方法に関する一般的なガイダンスについては、次を参照してください。[を使用して Web デプロイ リモート](https://technet.microsoft.com/library/ee461175(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-201">For more general guidance on the different ways in which you can deploy web packages from a remote computer, see [Using Web Deploy Remotely](https://technet.microsoft.com/library/ee461175(WS.10).aspx).</span></span> <span data-ttu-id="dc383-202">オンデマンドで Web デプロイの使用に関する詳細については、次を参照してください。[オンデマンドで Web デプロイ](https://technet.microsoft.com/library/ee517345(WS.10).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="dc383-202">For more information on using Web Deploy On Demand, see [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dc383-203">[前へ](configuring-server-environments-for-web-deployment.md)
> [次へ](scenario-configuring-a-test-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="dc383-203">[Previous](configuring-server-environments-for-web-deployment.md)
[Next](scenario-configuring-a-test-environment-for-web-deployment.md)</span></span>
