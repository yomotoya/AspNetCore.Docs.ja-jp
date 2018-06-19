---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Web 配置の適切なアプローチの選択 |Microsoft ドキュメント
author: jrjlee
description: インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用する場合が 3 つのメイン アプローチを使用することができますを取得しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d690744687af93a69743dc6ce6c853629f61f5d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890503"
---
<a name="choosing-the-right-approach-to-web-deployment"></a><span data-ttu-id="55fcb-103">Web 配置の適切なアプローチを選択します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-103">Choosing the Right Approach to Web Deployment</span></span>
====================
<span data-ttu-id="55fcb-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="55fcb-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="55fcb-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="55fcb-106">インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用する場合は、3 つの主要なアプローチを web サーバー上にパッケージ化された web アプリケーションの取得を行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-106">When you work with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 or later, there are three main approaches you can use to get your packaged web applications onto a web server.</span></span> <span data-ttu-id="55fcb-107">方法があります。</span><span class="sxs-lookup"><span data-stu-id="55fcb-107">You can either:</span></span>
> 
> - <span data-ttu-id="55fcb-108">対象にするリモートの場所からアプリケーションを展開、 *Web Deployment Agent サービス*(とも呼ばれる、「リモート エージェント」)、移行先サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-108">Deploy the application from a remote location by targeting the *Web Deployment Agent Service* (also known as the "remote agent") on the destination server.</span></span>
> - <span data-ttu-id="55fcb-109">Web 展開要求時に (とも呼ばれる、一時エージェント」) を使用してリモートの場所からアプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-109">Deploy the application from a remote location using Web Deploy On Demand (also known as the "temp agent").</span></span>
> - <span data-ttu-id="55fcb-110">対象にするリモートの場所からアプリケーションを展開、 *IIS Web 配置ハンドラー*移行先サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-110">Deploy the application from a remote location by targeting the *IIS Web Deploy Handler* on the destination server.</span></span>
> - <span data-ttu-id="55fcb-111">手動で移行先サーバーに web パッケージをコピーして、IIS マネージャーからインポートすることによって、アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-111">Deploy the application by manually copying the web package to the destination server and importing it through IIS Manager.</span></span>
> 
> <span data-ttu-id="55fcb-112">移行先 web サーバーを構成する方法を使用する展開するための方法に左右されます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-112">How you configure your destination web servers will depend on which approach to deployment you want to use.</span></span> <span data-ttu-id="55fcb-113">このトピックの内容を展開するための方法が適しているかを決定できます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-113">This topic will help you decide which approach to deployment is right for you.</span></span>


<span data-ttu-id="55fcb-114">次の表は、主な利点とそれぞれの方法に合わせて最も多くのシナリオと、各展開方法の短所を示します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-114">This table shows the main advantages and disadvantages of each deployment approach, together with the scenarios that most typically suit each approach.</span></span>

| <span data-ttu-id="55fcb-115">方法</span><span class="sxs-lookup"><span data-stu-id="55fcb-115">Approach</span></span> | <span data-ttu-id="55fcb-116">長所</span><span class="sxs-lookup"><span data-stu-id="55fcb-116">Advantages</span></span> | <span data-ttu-id="55fcb-117">短所</span><span class="sxs-lookup"><span data-stu-id="55fcb-117">Disadvantages</span></span> | <span data-ttu-id="55fcb-118">一般的なシナリオ</span><span class="sxs-lookup"><span data-stu-id="55fcb-118">Typical Scenarios</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="55fcb-119">リモート エージェント</span><span class="sxs-lookup"><span data-stu-id="55fcb-119">Remote Agent</span></span> | <span data-ttu-id="55fcb-120">セットアップに簡単です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-120">It is easy to set up.</span></span> <span data-ttu-id="55fcb-121">Web アプリケーションとコンテンツへの通常の更新プログラムに適しています。</span><span class="sxs-lookup"><span data-stu-id="55fcb-121">It is suitable for regular updates to web applications and content.</span></span> | <span data-ttu-id="55fcb-122">ユーザーは、ターゲット サーバーの管理者にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="55fcb-122">The user must be an administrator on the target server.</span></span> <span data-ttu-id="55fcb-123">ユーザーは、代替資格情報を提供できません。</span><span class="sxs-lookup"><span data-stu-id="55fcb-123">the user can't supply alternative credentials.</span></span> | <span data-ttu-id="55fcb-124">開発環境です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-124">Development environments.</span></span> <span data-ttu-id="55fcb-125">環境をテストします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-125">Test environments.</span></span> |
| <span data-ttu-id="55fcb-126">一時エージェント</span><span class="sxs-lookup"><span data-stu-id="55fcb-126">Temp Agent</span></span> | <span data-ttu-id="55fcb-127">ターゲット コンピューターに Web 配置をインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="55fcb-127">There is no need to install Web Deploy on the target computer.</span></span> <span data-ttu-id="55fcb-128">Web Deploy の最新バージョンは自動的に使用します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-128">The latest version of Web Deploy is automatically used.</span></span> | <span data-ttu-id="55fcb-129">ユーザーは、ターゲット サーバーの管理者にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="55fcb-129">The user must be an administrator on the target server.</span></span> <span data-ttu-id="55fcb-130">ユーザーは、代替資格情報を提供できません。</span><span class="sxs-lookup"><span data-stu-id="55fcb-130">The user can't supply alternative credentials.</span></span> | <span data-ttu-id="55fcb-131">開発環境です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-131">Development environments.</span></span> <span data-ttu-id="55fcb-132">環境をテストします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-132">Test environments.</span></span> |
| <span data-ttu-id="55fcb-133">Web Deploy ハンドラー</span><span class="sxs-lookup"><span data-stu-id="55fcb-133">Web Deploy Handler</span></span> | <span data-ttu-id="55fcb-134">管理者以外のユーザーには、コンテンツを展開できます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-134">Non-administrator users can deploy content.</span></span> <span data-ttu-id="55fcb-135">Web アプリケーションとコンテンツへの通常の更新プログラムに適しています。</span><span class="sxs-lookup"><span data-stu-id="55fcb-135">It is suitable for regular updates to web applications and content.</span></span> | <span data-ttu-id="55fcb-136">これを設定するよりもずっと複雑です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-136">It is a lot more complex to set up.</span></span> | <span data-ttu-id="55fcb-137">ステージング環境。</span><span class="sxs-lookup"><span data-stu-id="55fcb-137">Staging environments.</span></span> <span data-ttu-id="55fcb-138">イントラネットの実稼働環境。</span><span class="sxs-lookup"><span data-stu-id="55fcb-138">Intranet production environments.</span></span> <span data-ttu-id="55fcb-139">ホストされている環境です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-139">Hosted environments.</span></span> |
| <span data-ttu-id="55fcb-140">オフラインの展開</span><span class="sxs-lookup"><span data-stu-id="55fcb-140">Offline Deployment</span></span> | <span data-ttu-id="55fcb-141">これは非常に簡単にセットアップできます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-141">It is very easy to set up.</span></span> <span data-ttu-id="55fcb-142">分離環境に適しています。</span><span class="sxs-lookup"><span data-stu-id="55fcb-142">It is suitable for isolated environments.</span></span> | <span data-ttu-id="55fcb-143">サーバーの管理者は、手動でコピーしてたびに、web のパッケージをインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="55fcb-143">The server administrator must manually copy and import the web package every time.</span></span> | <span data-ttu-id="55fcb-144">インターネットに接続された運用環境。</span><span class="sxs-lookup"><span data-stu-id="55fcb-144">Internet-facing production environments.</span></span> <span data-ttu-id="55fcb-145">ネットワーク分離環境。</span><span class="sxs-lookup"><span data-stu-id="55fcb-145">Isolated network environments.</span></span> |
  

## <a name="using-the-remote-agent"></a><span data-ttu-id="55fcb-146">リモート エージェントを使用します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-146">Using the Remote Agent</span></span>

<span data-ttu-id="55fcb-147">Web 配置の移行先サーバーで、既定の設定を使用してをインストールするときに Web Deployment Agent サービス (「リモート エージェント」) が自動的にインストールされ、開始します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-147">When you install Web Deploy using the default settings on a destination server, the Web Deployment Agent Service (the "remote agent") is automatically installed and started.</span></span> <span data-ttu-id="55fcb-148">既定では、リモート エージェントは、このアドレスに HTTP エンドポイントを公開します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-148">By default, the remote agent exposes an HTTP endpoint at this address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> <span data-ttu-id="55fcb-149">置き換えることができます [*サーバー*]、web サーバーのコンピューターの名前を持つ、web サーバーまたはホスト名の IP アドレスに解決される、web サーバー。</span><span class="sxs-lookup"><span data-stu-id="55fcb-149">You can replace [*server*] with the machine name of your web server, an IP address for your web server, or a hostname that resolves to your web server.</span></span>


<span data-ttu-id="55fcb-150">サーバー管理者は、このエンドポイント アドレスを指定することによって、開発者のコンピューターなど、ビルド サーバー、リモートの場所から web パッケージを配置できます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-150">Server administrators can deploy web packages from a remote location, like a developer machine or a build server, by specifying this endpoint address.</span></span> <span data-ttu-id="55fcb-151">たとえば、Fabrikam, inc. Matt 明は開発者のコンピューター上、ContactManager.Mvc web アプリケーション プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-151">For example, suppose Matt Hink at Fabrikam, Inc. has built the ContactManager.Mvc web application project on his developer machine.</span></span> <span data-ttu-id="55fcb-152">ビルド プロセスと共に web パッケージを生成する、 *. deploy.cmd*パッケージをインストールするために必要な Web Deploy コマンドを含むファイルです。</span><span class="sxs-lookup"><span data-stu-id="55fcb-152">The build process generates a web package, together with a *.deploy.cmd* file that contains the Web Deploy commands required to install the package.</span></span> <span data-ttu-id="55fcb-153">Matt が TESTWEB1 サーバーでのサーバー管理者である場合は開発者のコンピューターでこのコマンドを実行してテストの web サーバーに web アプリケーションを展開できます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-153">If Matt is a server administrator on the TESTWEB1 server, he can deploy the web application to the test web server by running this command on his developer machine:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


<span data-ttu-id="55fcb-154">実際には、Web 配置の実行可能ファイルがリモート エージェントのエンドポイント アドレスを推論できる場合はコンピューター名を指定するために入力 Matt がのみ必要があります。</span><span class="sxs-lookup"><span data-stu-id="55fcb-154">In actual fact, the Web Deploy executable can infer the endpoint address of the remote agent if you provide the machine name, so Matt only needs to type this:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="55fcb-155">Web Deploy のコマンドライン構文についての詳細と *. deploy.cmd*ファイルを参照してください[する方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-155">For more information on Web Deploy command-line syntax and *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>


<span data-ttu-id="55fcb-156">リモート エージェントがリモートの場所からコンテンツを展開する簡単な方法を提供し、この方法でも 1 回のクリックまたは自動の展開でうまく機能します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-156">The remote agent offers a straightforward way to deploy content from a remote location, and this approach can work well with one-click or automated deployment.</span></span> <span data-ttu-id="55fcb-157">ただし、展開コマンドを実行するユーザーにも必要があります、ドメイン管理者または移行先サーバーでローカルの administrators グループのメンバーのいずれか。</span><span class="sxs-lookup"><span data-stu-id="55fcb-157">However, the user who runs the deployment command must also be either a domain administrator or a member of the local administrators group on the destination server.</span></span> <span data-ttu-id="55fcb-158">さらに、コマンドラインで代替資格情報を渡すことはできませんので、リモート エージェントは、基本認証をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="55fcb-158">In addition, the remote agent doesn't support basic authentication, so you can't pass alternative credentials on the command line.</span></span>

<span data-ttu-id="55fcb-159">リモート エージェント提供方法としては有効では、開発またはテストのシナリオでは珍しくありません開発者が完全な管理者は、テスト サーバー環境を制御するため、アプリケーションは通常再構築し、非常に再展開の展開頻繁にします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-159">The remote agent provides a useful approach to deployment in development or test scenarios, where it's not uncommon for developers to have full administrator control over a test server environment, and applications are typically rebuilt and redeployed very frequently.</span></span> <span data-ttu-id="55fcb-160">ただし、この手法は、通常、ステージング環境または実稼働環境に許容される小さいです。</span><span class="sxs-lookup"><span data-stu-id="55fcb-160">However, this approach is usually less acceptable for staging or production environments.</span></span>

<span data-ttu-id="55fcb-161">リモート エージェントのアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web デプロイのテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-161">For an end-to-end example of a scenario that uses the remote agent approach, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span>

## <a name="using-the-temp-agent"></a><span data-ttu-id="55fcb-162">一時のエージェントを使用します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-162">Using the Temp Agent</span></span>

<span data-ttu-id="55fcb-163">展開するための一時エージェント アプローチは、リモート エージェントのアプローチに似ています。</span><span class="sxs-lookup"><span data-stu-id="55fcb-163">The temp agent approach to deployment is similar to the remote agent approach.</span></span> <span data-ttu-id="55fcb-164">ただし、リモート エージェントのアプローチとは異なり、ターゲット web サーバーに Web 配置をインストールする必要ありません。</span><span class="sxs-lookup"><span data-stu-id="55fcb-164">However, in contrast to the remote agent approach, you don't need to install Web Deploy on the destination web server.</span></span> <span data-ttu-id="55fcb-165">代わりに、展開を実行するときに Web デプロイ、移行先サーバーに web deployment agent サービスの一時的なバージョンをインストールおよびこれを使用して、コンテンツを IIS に展開します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-165">Instead, when you perform the deployment, Web Deploy will install a temporary version of the web deployment agent service on the destination server and will use this to deploy your content to IIS.</span></span> <span data-ttu-id="55fcb-166">展開が完了したら、すべての一時ファイルは削除されます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-166">When the deployment is complete, all temporary files are removed.</span></span>

<span data-ttu-id="55fcb-167">一時エージェント プロバイダー設定を使用して、追加する場合、 **/g**には、展開コマンド フラグ。</span><span class="sxs-lookup"><span data-stu-id="55fcb-167">If you want to use the temp agent provider setting, add the **/g** flag to your deployment command:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> <span data-ttu-id="55fcb-168">サービスが実行されていない場合でも、web deployment agent サービスが移行先コンピューターにインストールされている場合に、temp のエージェントを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="55fcb-168">You can't use the temp agent if the web deployment agent service is installed on the destination computer, even if the service is not running.</span></span>


<span data-ttu-id="55fcb-169">このアプローチの利点は、移行先サーバーへの Web Deploy のインストールを管理する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="55fcb-169">The advantage of this approach is that you don't need to maintain installations of Web Deploy on your destination servers.</span></span> <span data-ttu-id="55fcb-170">さらに、元と移行先のコンピューターが同じバージョンの Web 配置を実行していることを確認する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="55fcb-170">Furthermore, you don't need to ensure that the source and destination computers are running the same version of Web Deploy.</span></span> <span data-ttu-id="55fcb-171">ただし、このアプローチが低下、プリンシパルと同じ制限リモート エージェントのアプローチからつまり、コンテンツを展開するために、移行先サーバー上のローカル管理者をする必要があり、NTLM 認証のみがサポートされていること。</span><span class="sxs-lookup"><span data-stu-id="55fcb-171">However, this approach suffers from the same principal limitations as the remote agent approach, namely that you must be a local administrator on the destination server in order to deploy content, and only NTLM authentication is supported.</span></span> <span data-ttu-id="55fcb-172">一時エージェント アプローチには、展開先の環境のもっと初期構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-172">The temp agent approach also requires a lot more initial configuration of the destination environment.</span></span>

<span data-ttu-id="55fcb-173">一時のエージェントを使用する方法については、次を参照してください。[する方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)と[Web 展開オンデマンド](https://technet.microsoft.com/library/ee517345(WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-173">For more information on using the temp agent, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx) and [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span></span>

## <a name="using-the-web-deploy-handler"></a><span data-ttu-id="55fcb-174">Web を使用してハンドラーを展開</span><span class="sxs-lookup"><span data-stu-id="55fcb-174">Using the Web Deploy Handler</span></span>

<span data-ttu-id="55fcb-175">IIS 7 以降の場合は、Web Deploy は、IIS Web 配置ハンドラーから代替の展開方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-175">For IIS 7 onwards, Web Deploy offers an alternative deployment approach through the IIS Web Deploy Handler.</span></span> <span data-ttu-id="55fcb-176">Web 展開ハンドラーには、ユーザーをリモートの場所からの IIS web サイトの管理を許可するように設計された IIS Web 管理サービス (WMSvc) は緊密に統合されています。</span><span class="sxs-lookup"><span data-stu-id="55fcb-176">The Web Deploy Handler is closely integrated with the IIS Web Management Service (WMSvc), which is designed to allow users to manage IIS websites from remote locations.</span></span>

<span data-ttu-id="55fcb-177">既定では、リモート エージェントは、このアドレスに HTTP エンドポイントを公開します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-177">By default, the remote agent exposes an HTTP endpoint at this address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="55fcb-178">置き換えることができます [*サーバー*]、web サーバーのコンピューターの名前を持つ、web サーバーまたはホスト名の IP アドレスに解決される、web サーバー。</span><span class="sxs-lookup"><span data-stu-id="55fcb-178">You can replace [*server*] with the machine name of your web server, an IP address for your web server, or a hostname that resolves to your web server.</span></span>


<span data-ttu-id="55fcb-179">リモート エージェントをおよび一時エージェントを経由で Web 展開ハンドラーの大きな利点は、特定の IIS web サイトにアプリケーションとコンテンツを展開する管理者以外のユーザーを許可するように IIS を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-179">The big advantage of the Web Deploy Handler over the remote agent, and the temp agent, is that you can configure IIS to allow non-administrator users to deploy applications and content to specific IIS websites.</span></span> <span data-ttu-id="55fcb-180">Web 展開ハンドラーでは、Web Deploy コマンドでパラメーターとして別の資格情報を提供できるように、基本認証もサポートします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-180">The Web Deploy Handler also supports basic authentication, so you can provide alternative credentials as parameters in your Web Deploy commands.</span></span> <span data-ttu-id="55fcb-181">主な欠点は、Web 配置ハンドラーが最初に、セットアップし、構成をはるかに複雑です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-181">The major drawback is that the Web Deploy Handler is initially a lot more complicated to set up and configure.</span></span>

<span data-ttu-id="55fcb-182">管理者以外のユーザーの場合 Web 管理サービス (WMSvc) が許可のみを IIS に接続するサーバー レベルの接続ではなくサイト レベルの接続を使用します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-182">In the case of non-administrator users, the Web Management Service (WMSvc) will only allow the user to connect to IIS using a site-level connection, rather than a server-level connection.</span></span> <span data-ttu-id="55fcb-183">特定のサイトにアクセスするには、エンドポイントのアドレスにサイト固有のクエリ文字列を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-183">To access a particular site, you can include a site-specific query string in the endpoint address:</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


<span data-ttu-id="55fcb-184">たとえば、ビルド プロセスが自動的にデプロイする web アプリケーションをステージング環境を正常にビルドするたびに構成されているとします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-184">For example, suppose a build process is configured to automatically deploy a web application to a staging environment after every successful build.</span></span> <span data-ttu-id="55fcb-185">リモート エージェントのアプローチを使用した場合は、ビルド プロセスの id を移行先サーバーを管理者にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="55fcb-185">If you used the remote agent approach, you'd need to make the build process identity an administrator on your destination servers.</span></span> <span data-ttu-id="55fcb-186">これに対し、Web 配置ハンドラー アプローチを使用することができます管理者以外のユーザーをユーザーに与える&#x2014;**FABRIKAM\stagingdeployer**ここでは&#x2014;特定 IIS の web サイトのみ、およびビルド プロセスにアクセス許可は、これらを指定できますweb パッケージを展開する資格情報。</span><span class="sxs-lookup"><span data-stu-id="55fcb-186">In contrast, using the Web Deploy Handler approach you can give a non-administrator user&#x2014;**FABRIKAM\stagingdeployer** in this case&#x2014;permission to a specific IIS website only, and the build process can provide these credentials to deploy the web package.</span></span>


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> <span data-ttu-id="55fcb-187">Web Deploy のコマンドライン操作と構文の詳細については、次を参照してください。 [Web 展開コマンド ライン リファレンス](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-187">For more information on Web Deploy command-line operations and syntax, see [Web Deploy Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span></span> <span data-ttu-id="55fcb-188">使用の詳細について、 *. deploy.cmd*ファイルを参照してください[する方法: インストールの展開パッケージを使用して、deploy.cmd ファイル](https://msdn.microsoft.com/library/ff356104.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-188">For more information on using the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>


<span data-ttu-id="55fcb-189">Web 展開ハンドラーは、環境、ホストされる環境、およびイントラネット ベースの運用環境で、サーバーへのリモート アクセスは使用できますが、管理者の資格情報がステージング環境の配置に有効な解決方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="55fcb-189">The Web Deploy Handler provides a useful approach to deployment in staging environments, hosted environments, and intranet-based production environments, where remote access to the server is available but administrator credentials are not.</span></span>

<span data-ttu-id="55fcb-190">Web 展開ハンドラー アプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web 配置用のステージング環境構成](scenario-configuring-a-staging-environment-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-190">For an end-to-end example of a scenario that uses the Web Deploy Handler approach, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span>

## <a name="using-offline-deployment"></a><span data-ttu-id="55fcb-191">オフラインの展開を使用してください。</span><span class="sxs-lookup"><span data-stu-id="55fcb-191">Using Offline Deployment</span></span>

<span data-ttu-id="55fcb-192">場合によっては、それが不可能または IIS の web サイトにリモートの場所からアプリケーションおよびコンテンツを展開することは実用的です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-192">In some cases, it's not possible or practical to deploy applications and content to an IIS website from a remote location.</span></span> <span data-ttu-id="55fcb-193">たとえば、元と移行先コンピューターは、分離されたネットワークまたはネットワーク セグメント内にある場合がありますか、ファイアウォール ポリシーは、リモート アクセスを許可しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="55fcb-193">For example, the source and destination computers may be in isolated networks or network segments, or firewall policy may not permit remote access.</span></span>

<span data-ttu-id="55fcb-194">上記のようなシナリオで使用することできますも、パッケージと Web 配置の機能を公開だけ使うことはできませんにリモートの場所からします。</span><span class="sxs-lookup"><span data-stu-id="55fcb-194">In scenarios like these, you can still use the packaging and publishing capabilities of Web Deploy; you just can't use them from a remote location.</span></span> <span data-ttu-id="55fcb-195">代わりに、移行先サーバーで管理者は web パッケージをサーバーにコピーし、IIS マネージャーからインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="55fcb-195">Instead, an administrator on the destination server must copy the web package onto the server and import it through IIS Manager.</span></span>

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

<span data-ttu-id="55fcb-196">オフラインの展開方法は、境界ネットワーク内のサーバー可能性がありますが制限されている内部ネットワーク内のコンピューターとの接続、インターネットに接続された運用環境で通常役立ちます。</span><span class="sxs-lookup"><span data-stu-id="55fcb-196">The offline deployment approach is typically useful in Internet-facing production environments, where servers in a perimeter network may have restricted connectivity with computers in the internal network.</span></span>

<span data-ttu-id="55fcb-197">オフライン展開のアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web 配置の実稼働環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-197">For an end-to-end example of a scenario that uses the offline deployment approach, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

## <a name="further-reading"></a><span data-ttu-id="55fcb-198">関連項目</span><span class="sxs-lookup"><span data-stu-id="55fcb-198">Further Reading</span></span>

<span data-ttu-id="55fcb-199">Web Deploy のコマンドライン操作と構文の詳細については、次を参照してください。 [Web 展開コマンド ライン リファレンス](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-199">For more information on Web Deploy command-line operations and syntax, see [Web Deploy Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx).</span></span> <span data-ttu-id="55fcb-200">使用の詳細について、 *. deploy.cmd*ファイルを参照してください[する方法: インストールの展開パッケージを使用して、deploy.cmd ファイル](https://msdn.microsoft.com/library/ff356104.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-200">For more information on using the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span>

<span data-ttu-id="55fcb-201">リモート コンピューターから web パッケージを展開するさまざまな方法に関する一般的なガイダンスについては、次を参照してください。[を使用して Web 展開でのリモート](https://technet.microsoft.com/library/ee461175(WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-201">For more general guidance on the different ways in which you can deploy web packages from a remote computer, see [Using Web Deploy Remotely](https://technet.microsoft.com/library/ee461175(WS.10).aspx).</span></span> <span data-ttu-id="55fcb-202">Web 展開要求時に使用する方法については、次を参照してください。 [Web 展開オンデマンド](https://technet.microsoft.com/library/ee517345(WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="55fcb-202">For more information on using Web Deploy On Demand, see [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="55fcb-203">[前へ](configuring-server-environments-for-web-deployment.md)
> [次へ](scenario-configuring-a-test-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="55fcb-203">[Previous](configuring-server-environments-for-web-deployment.md)
[Next](scenario-configuring-a-test-environment-for-web-deployment.md)</span></span>
