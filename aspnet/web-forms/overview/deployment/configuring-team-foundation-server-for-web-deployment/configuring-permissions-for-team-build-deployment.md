---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: "ビルドの配置をチームのアクセス許可の構成 |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、自動 b の一部として、web サーバーおよびデータベース サーバーにコンテンツを展開するようにビルド サーバーを有効にする権限を構成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: cb3d013d69e36f97335ea31dd6e4997772ba2d8e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="configuring-permissions-for-team-build-deployment"></a><span data-ttu-id="765d6-103">チームのアクセス許可の構成のビルドの配置</span><span class="sxs-lookup"><span data-stu-id="765d6-103">Configuring Permissions for Team Build Deployment</span></span>
====================
<span data-ttu-id="765d6-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="765d6-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="765d6-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="765d6-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="765d6-106">このトピックでは、自動ビルド プロセスの一環として、web サーバーおよびデータベース サーバーにコンテンツを展開するようにビルド サーバーを有効にする権限を構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="765d6-106">This topic describes how to configure permissions to enable your build server to deploy content to web servers and database servers as part of an automated build process.</span></span>


<span data-ttu-id="765d6-107">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="765d6-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="765d6-108">説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスで 2 つのプロジェクト ファイル & #x 2014; 1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。</span><span class="sxs-lookup"><span data-stu-id="765d6-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="765d6-109">ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。</span><span class="sxs-lookup"><span data-stu-id="765d6-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="765d6-110">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="765d6-110">Task Overview</span></span>

<span data-ttu-id="765d6-111">Team Foundation Server (TFS) 2010年のビルド サービスをインストールするときに、サービスを実行する id を指定します。</span><span class="sxs-lookup"><span data-stu-id="765d6-111">When you install the Team Foundation Server (TFS) 2010 build service, you specify the identity with which you want the service to run.</span></span> <span data-ttu-id="765d6-112">既定では、これは、Network Service アカウントです。</span><span class="sxs-lookup"><span data-stu-id="765d6-112">By default, this is the Network Service account.</span></span> <span data-ttu-id="765d6-113">代わりに、ドメイン アカウントを使用して実行するビルド サービスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="765d6-113">Alternatively, you can configure the build service to run using a domain account.</span></span>

<span data-ttu-id="765d6-114">ビルド サービス id を使用して、Windows 認証、およびチームがビルドを使用して自動化を計画することを必要とする展開タスクが実行されます。</span><span class="sxs-lookup"><span data-stu-id="765d6-114">Any deployment tasks that require Windows authentication, and that you plan to automate using Team Build, will run using the build service identity.</span></span> <span data-ttu-id="765d6-115">そのため、ビルド サービス id に、web サーバーと、データベース サーバーに必要なアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-115">As such, you'll need to grant the build service identity any required permissions on your web servers and your database servers.</span></span>

> [!NOTE]
> <span data-ttu-id="765d6-116">ネットワーク サービス アカウントは、他のコンピューターへの認証にコンピューター アカウントを使用します。</span><span class="sxs-lookup"><span data-stu-id="765d6-116">The Network Service account uses the machine account to authenticate to other computers.</span></span> <span data-ttu-id="765d6-117">コンピューター アカウントをフォーム*[ドメイン名]\[マシン名]***$**& #x 2014。 たとえば、 **FABRIKAM\TFSBUILD$**します。</span><span class="sxs-lookup"><span data-stu-id="765d6-117">Machine accounts take the form *[domain name]\[machine name]***$**&#x2014;for example, **FABRIKAM\TFSBUILD$**.</span></span> <span data-ttu-id="765d6-118">ような場合、ビルド サービスでは、Network Service の id を使用して実行する、ビルド サーバーのコンピューター アカウント id に必要なアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-118">As such, if your build service runs using the Network Service identity, you should grant any required permissions to the machine account identity for your build server.</span></span>


## <a name="configuring-web-server-permissions"></a><span data-ttu-id="765d6-119">Web サーバーのアクセス許可の構成</span><span class="sxs-lookup"><span data-stu-id="765d6-119">Configuring Web Server Permissions</span></span>

<span data-ttu-id="765d6-120">」の説明に従って[Web 配置を右側の方法を選択する](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)、2 つの主要なアプローチがリモート web サーバーに web パッケージを展開する場合に使用することができます。</span><span class="sxs-lookup"><span data-stu-id="765d6-120">As described in [Choosing the Right Approach to Web Deployment](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), there are two main approaches you can use if you want to deploy web packages to a remote web server:</span></span>

- <span data-ttu-id="765d6-121">対象にするリモートの場所からアプリケーションを展開、 *Web Deployment Agent サービス*(リモート エージェントとも呼ばれます)、移行先サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="765d6-121">Deploy the application from a remote location by targeting the *Web Deployment Agent Service* (also known as the remote agent) on the destination server.</span></span>
- <span data-ttu-id="765d6-122">対象にするリモートの場所からアプリケーションを展開、*インターネット インフォメーション サービス*(*IIS) Web 展開ハンドラー*移行先サーバーにします。</span><span class="sxs-lookup"><span data-stu-id="765d6-122">Deploy the application from a remote location by targeting the *Internet Information Services* (*IIS) Web Deploy Handler* on the destination server.</span></span>

<span data-ttu-id="765d6-123">リモート エージェントでは、ここでは 2 つのキーの制限があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-123">The remote agent has two key limitations in this case:</span></span>

- <span data-ttu-id="765d6-124">リモート エージェントには、NTLM 認証のみがサポートしています。</span><span class="sxs-lookup"><span data-stu-id="765d6-124">The remote agent supports only NTLM authentication.</span></span> <span data-ttu-id="765d6-125">つまり、展開には、ビルド サービス id & #x 2014 を使用する必要があります。 別のアカウントを偽装することはできません。</span><span class="sxs-lookup"><span data-stu-id="765d6-125">In other words, the deployment must use the build service identity&#x2014;you can't impersonate another account.</span></span>
- <span data-ttu-id="765d6-126">リモート エージェントを使用するのには、ターゲット サーバーの管理者が展開を実行するアカウントにあります。</span><span class="sxs-lookup"><span data-stu-id="765d6-126">To use the remote agent, the account that performs the deployment must be an administrator on the target server.</span></span>

<span data-ttu-id="765d6-127">同時に、これら 2 つの制限にするようにリモート エージェントのアプローチは自動化されたチーム ビルド展開の望ましくないにします。</span><span class="sxs-lookup"><span data-stu-id="765d6-127">Together, these two limitations make the remote agent approach undesirable for an automated Team Build deployment.</span></span> <span data-ttu-id="765d6-128">このアプローチを使用するのには、ビルド サービスがターゲット web サーバー上の管理者のアカウントを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-128">To use this approach, you'd need to make the build service account an administrator on any target web servers.</span></span>

<span data-ttu-id="765d6-129">これに対し、Web 配置ハンドラー アプローチには、さまざまな利点が提供しています。</span><span class="sxs-lookup"><span data-stu-id="765d6-129">In contrast, the Web Deploy Handler approach offers various advantages:</span></span>

- <span data-ttu-id="765d6-130">Web 展開ハンドラーは、IIS Web 配置ツール (Web 配置) を別のアカウントの資格情報を渡すことができる HTTPS 経由で基本認証をサポートします。</span><span class="sxs-lookup"><span data-stu-id="765d6-130">The Web Deploy Handler supports basic authentication over HTTPS, which allows you to pass the credentials of an alternative account to the IIS Web Deployment Tool (Web Deploy).</span></span>
- <span data-ttu-id="765d6-131">Web 展開ハンドラーを使用して特定の IIS web サイトにコンテンツを展開する管理者以外のユーザーを許可する対象の web サーバーを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="765d6-131">You can configure target web servers to allow non-administrator users to deploy content to specific IIS websites using the Web Deploy Handler.</span></span>

<span data-ttu-id="765d6-132">その結果、お勧め明確にチーム ビルドから web パッケージの展開を自動化するときに、Web 配置のハンドラーを対象にします。</span><span class="sxs-lookup"><span data-stu-id="765d6-132">As a result, it's clearly preferable to target the Web Deploy Handler when you automate web package deployment from Team Build.</span></span> <span data-ttu-id="765d6-133">これは、推奨されるプロセスです。</span><span class="sxs-lookup"><span data-stu-id="765d6-133">This is the recommended process:</span></span>

1. <span data-ttu-id="765d6-134">展開に使用する低特権のドメイン アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="765d6-134">Create a low-privileged domain account to use for the deployment.</span></span>
2. <span data-ttu-id="765d6-135">Web 展開ハンドラーを構成および」の説明に従って、アカウントに特定の IIS web サイトにコンテンツを展開に必要なアクセス許可を付与[Web 配置発行 (Web 配置ハンドラー) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)です。</span><span class="sxs-lookup"><span data-stu-id="765d6-135">Configure the Web Deploy Handler and grant the account the required permissions to deploy content to a specific IIS website, as described in [Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).</span></span>
3. <span data-ttu-id="765d6-136">Web Deploy を呼び出し、Web 配置のハンドラーを対象し、基本認証を使用し、ドメイン アカウントの資格情報を指定して、作成した配置を実行します。</span><span class="sxs-lookup"><span data-stu-id="765d6-136">Invoke Web Deploy and target the Web Deploy Handler, using basic authentication and supplying the credentials of the domain account you created, to perform the deployment.</span></span>

<span data-ttu-id="765d6-137">[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューション、認証の種類を指定する (基本または NTLM)、Web Deploy の資格情報と環境固有のプロジェクト ファイル内のエンドポイント アドレス (リモート エージェントまたは Web デプロイ ハンドラー)。</span><span class="sxs-lookup"><span data-stu-id="765d6-137">In the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, you specify the authentication type (basic or NTLM), the Web Deploy credentials, and the endpoint address (remote agent or Web Deploy Handler) in the environment-specific project file.</span></span> <span data-ttu-id="765d6-138">これらの値は、作成し、プロジェクト ファイルを実行すると、Web Deploy コマンドを実行に使用されます。</span><span class="sxs-lookup"><span data-stu-id="765d6-138">These values are used to formulate and run a Web Deploy command when the project file is executed.</span></span> <span data-ttu-id="765d6-139">詳細については、次を参照してください。 [Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)です。</span><span class="sxs-lookup"><span data-stu-id="765d6-139">For more information, see [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span></span>

<span data-ttu-id="765d6-140">Web 配置のハンドラーを許可を構成する方法を含む構成の詳細については「 [Web 配置発行 (Web 配置ハンドラー) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)です。</span><span class="sxs-lookup"><span data-stu-id="765d6-140">For more information on configuring the Web Deploy Handler, including how to configure permissions, see [Configuring a Web Server for Web Deploy Publishing (Web Deploy Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).</span></span> <span data-ttu-id="765d6-141">リモート エージェントを構成する方法については、次を参照してください。 [Web 配置発行 (リモート エージェント) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)です。</span><span class="sxs-lookup"><span data-stu-id="765d6-141">For more information on configuring the remote agent, see [Configuring a Web Server for Web Deploy Publishing (Remote Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span>

## <a name="configuring-database-server-permissions"></a><span data-ttu-id="765d6-142">データベース サーバーのアクセス許可の構成</span><span class="sxs-lookup"><span data-stu-id="765d6-142">Configuring Database Server Permissions</span></span>

<span data-ttu-id="765d6-143">SQL Server にデータベースを展開するには、次の必要があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-143">To deploy a database to SQL Server, you must:</span></span>

- <span data-ttu-id="765d6-144">SQL Server インスタンスに展開するアカウントのログインを作成します。</span><span class="sxs-lookup"><span data-stu-id="765d6-144">Create a login for the deploying account on the SQL Server instance.</span></span>
- <span data-ttu-id="765d6-145">ログインを与える**DBCreator** SQL Server インスタンスに対する権限。</span><span class="sxs-lookup"><span data-stu-id="765d6-145">Grant the login **DBCreator** permissions on the SQL Server instance.</span></span>
- <span data-ttu-id="765d6-146">最初の展開後にログインを追加、 **db\_所有者**ターゲット データベースのロール。</span><span class="sxs-lookup"><span data-stu-id="765d6-146">After the initial deployment, add the login to the **db\_owner** role on the target database.</span></span> <span data-ttu-id="765d6-147">後続のデプロイの既存のデータベースの変更はなくする新しいデータベースを作成するために必要です。</span><span class="sxs-lookup"><span data-stu-id="765d6-147">This is required because on subsequent deployments, you're modifying an existing database rather than creating a new database.</span></span>

<span data-ttu-id="765d6-148">NTLM 認証または SQL Server 認証を使用して SQL Server インスタンスに認証することができます。</span><span class="sxs-lookup"><span data-stu-id="765d6-148">You can authenticate to a SQL Server instance using either NTLM authentication or SQL Server authentication:</span></span>

- <span data-ttu-id="765d6-149">NTLM 認証を使用する場合は、ビルド サービス アカウントに上記のアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-149">If you use NTLM authentication, you need to grant the permissions described above to the build service account.</span></span>
- <span data-ttu-id="765d6-150">SQL Server 認証を使用する場合は、SQL Server アカウントに上記のアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-150">If you use SQL Server authentication, you need to grant the permissions described above to the SQL Server account.</span></span> <span data-ttu-id="765d6-151">また、データベースの配置に使用する接続文字列に、SQL Server のユーザー名とパスワードを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-151">You also need to include the SQL Server user name and password in the connection string you use to deploy the database.</span></span>

<span data-ttu-id="765d6-152">データベースの配置のアクセス許可を構成する方法の詳細な手順については、次を参照してください。 [Web 配置発行のデータベース サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)です。</span><span class="sxs-lookup"><span data-stu-id="765d6-152">For step-by-step details on how to configure permissions for database deployment, see [Configuring a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

## <a name="conclusion"></a><span data-ttu-id="765d6-153">まとめ</span><span class="sxs-lookup"><span data-stu-id="765d6-153">Conclusion</span></span>

<span data-ttu-id="765d6-154">この時点で、オプションと共に、認証するには、開いているときに必要なチーム ビルドからの web アプリケーションとデータベース デプロイを自動化するアクセス許可を理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-154">At this point, you should understand the permissions required, together with the authentication options open to you, when you automate web application and database deployments from Team Build.</span></span> <span data-ttu-id="765d6-155">IIS web サーバーと SQL Server データベース サーバーに必要なアクセス許可を実装できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="765d6-155">You should also be able to implement the necessary permissions on IIS web servers and SQL Server database servers.</span></span>

## <a name="further-reading"></a><span data-ttu-id="765d6-156">関連項目</span><span class="sxs-lookup"><span data-stu-id="765d6-156">Further Reading</span></span>

<span data-ttu-id="765d6-157">リモート展開をサポートするために Windows server の環境を構成する方法については、次を参照してください。 [Web 配置のサーバー環境の構成](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="765d6-157">For more information on configuring Windows server environments to support remote deployment, see [Configuring Server Environments for Web Deployment](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="765d6-158">前へ</span><span class="sxs-lookup"><span data-stu-id="765d6-158">Previous</span></span>](deploying-a-specific-build.md)
