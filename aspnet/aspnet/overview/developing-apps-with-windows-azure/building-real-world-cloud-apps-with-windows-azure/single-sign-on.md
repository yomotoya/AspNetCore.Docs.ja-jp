---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: シングル サインオン (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント
author: MikeWasson
description: Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 82f2f99154d94074b03d580a0f491053d6f53bde
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="d48ea-104">シングル サインオン (Azure と実際のクラウド アプリのビルド)</span><span class="sxs-lookup"><span data-stu-id="d48ea-104">Single Sign-On (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="d48ea-105">によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d48ea-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d48ea-106">[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="d48ea-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="d48ea-107">**現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。</span><span class="sxs-lookup"><span data-stu-id="d48ea-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="d48ea-108">13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="d48ea-109">電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="d48ea-110">クラウド アプリケーションを開発する場合について検討する多くのセキュリティの問題がありますが、この系列のうちの 1 つに注目します。 シングル サインオンします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-110">There are many security issues to think about when you're developing a cloud app, but for this series we'll focus on just one: single sign-on.</span></span> <span data-ttu-id="d48ea-111">これは、質問人が多くの場合、要求:"、主に構築していてアプリ自分の会社の従業員に対してクラウドでこれらのアプリケーションをホストしてすれば従業員が理解し、アプリを実行しているときに、内部設置型環境で使用する同じセキュリティ モデルを使用するようにでも有効にはホスト ファイアウォールの内側にありますか。"</span><span class="sxs-lookup"><span data-stu-id="d48ea-111">A question people often ask is this: "I'm primarily building apps for the employees of my company; how do I host these apps in the cloud and still enable them to use the same security model that my employees know and use in the on-premises environment when they're running apps that are hosted inside the firewall?"</span></span> <span data-ttu-id="d48ea-112">このシナリオを有効にする方法の 1 つは Azure Active Directory (Azure AD) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-112">One of the ways we enable this scenario is called Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="d48ea-113">Azure AD では、エンタープライズ基幹業務 (LOB) アプリ使用できるように、インターネットを経由でき、ビジネス パートナーにもこれらのアプリを使用できるようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-113">Azure AD enables you to make enterprise line-of-business (LOB) apps available over the Internet, and it enables you to make these apps available to business partners as well.</span></span>

## <a name="introduction-to-azure-ad"></a><span data-ttu-id="d48ea-114">Azure AD の概要</span><span class="sxs-lookup"><span data-stu-id="d48ea-114">Introduction to Azure AD</span></span>

<span data-ttu-id="d48ea-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/)提供[Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx)クラウドでします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) provides [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in the cloud.</span></span> <span data-ttu-id="d48ea-116">主な機能を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-116">Key features include the following:</span></span>

- <span data-ttu-id="d48ea-117">内部設置型 Active Directory と統合できます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-117">It integrates with on-premises Active Directory.</span></span>
- <span data-ttu-id="d48ea-118">アプリでのシングル サインオンが有効にします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-118">It enables single sign-on with your apps.</span></span>
- <span data-ttu-id="d48ea-119">などのオープン スタンダードをサポートしている[SAML](http://en.wikipedia.org/wiki/SAML_2.0)、 [Ws-fed](http://en.wikipedia.org/wiki/WS-Federation)、および[OAuth 2.0](http://oauth.net/2/)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-119">It supports open standards such as [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), and [OAuth 2.0](http://oauth.net/2/).</span></span>
- <span data-ttu-id="d48ea-120">エンタープライズがサポートしている[Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-120">It supports Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).</span></span>

<span data-ttu-id="d48ea-121">イントラネット アプリケーションにサインオンする従業員を有効にするために使用、内部設置型 Windows Server Active Directory 環境があるとします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-121">Suppose you have an on-premises Windows Server Active Directory environment that you use to enable employees to sign on to Intranet apps:</span></span>

![](single-sign-on/_static/image1.png)

<span data-ttu-id="d48ea-122">クラウドにディレクトリを作成は、どのような Azure AD を実行できます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-122">What Azure AD enables you to do is create a directory in the cloud.</span></span> <span data-ttu-id="d48ea-123">これは無料の機能と簡単にセットアップします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-123">It's a free feature and easy to set up.</span></span>

<span data-ttu-id="d48ea-124">内部設置型 Active Directory; から完全に独立してできます。すべてのユーザーに選択して、インターネットのアプリでの認証を配置することができます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-124">It can be entirely independent from your on-premises Active Directory; you can put anyone you want in it and authenticate them in Internet apps.</span></span>

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

<span data-ttu-id="d48ea-126">これを統合するには、内部設置型または AD です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-126">Or you can integrate it with your on-premises AD.</span></span>

![AD と WAAD の統合](single-sign-on/_static/image3.png)

<span data-ttu-id="d48ea-128">今すぐ、内部設置型を認証できるすべての従業員は、しなくてもファイアウォールを開くか、データ センター内の新しいサーバーを展開する – インターネット経由で認証もできます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-128">Now all the employees who can authenticate on-premises can also authenticate over the Internet – without you having to open up a firewall or deploy any new servers in your data center.</span></span> <span data-ttu-id="d48ea-129">すべて既存 Active Directory 環境を把握し、現在使用して、社内アプリのシングル サインオン機能に与えるを活用する続行することができます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-129">You can continue to leverage all the existing Active Directory environment that you know and use today to give your internal apps single-sign on capability.</span></span>

<span data-ttu-id="d48ea-130">AD と Azure AD の間には、この接続を確立した後に、web アプリとクラウドでは、従業員を認証するようにモバイル デバイスを有効することを受け入れるように、Office 365、SalesForce.com、または Google のアプリなどのサード パーティ製アプリを有効にすることができます、従業員の資格情報です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-130">Once you've made this connection between AD and Azure AD, you can also enable your web apps and your mobile devices to authenticate your employees in the cloud, and you can enable third-party apps, such as Office 365, SalesForce.com, or Google apps, to accept your employees' credentials.</span></span> <span data-ttu-id="d48ea-131">Office 365 を使用している場合は、既にセットアップが完了を Azure AD と Office 365 では、Azure AD を使用して、認証および承認するためです。</span><span class="sxs-lookup"><span data-stu-id="d48ea-131">If you're using Office 365, you're already set up with Azure AD because Office 365 uses Azure AD for authentication and authorization.</span></span>

![サード パーティ製アプリ](single-sign-on/_static/image4.png)

<span data-ttu-id="d48ea-133">このアプローチの長所はいつでも、組織を追加または、ユーザーを削除、またはユーザーがパスワードを変更する、内部設置型環境で現在使用している同じプロセスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-133">The beauty of this approach is that any time your organization adds or deletes a user, or a user changes a password, you use the same process that you use today in your on-premises environment.</span></span> <span data-ttu-id="d48ea-134">すべての内部設置型の AD 変更は自動的にクラウド環境に反映します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-134">All of your on-premises AD changes are automatically propagated to the cloud environment.</span></span>

<span data-ttu-id="d48ea-135">会社を使用してまたは良いニュースを Office 365 へ移動する場合は、Azure AD が Office 365 では、Azure AD を使用して、認証するために自動的に設定があります。</span><span class="sxs-lookup"><span data-stu-id="d48ea-135">If your company is using or moving to Office 365, the good news is that you'll have Azure AD set up automatically because Office 365 uses Azure AD for authentication.</span></span> <span data-ttu-id="d48ea-136">ように、簡単に、独自のアプリでは、Office 365 を使用する同じ認証を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-136">So you can easily use in your own apps the same authentication that Office 365 uses.</span></span>

## <a name="set-up-an-azure-ad-tenant"></a><span data-ttu-id="d48ea-137">Azure AD テナントを設定します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-137">Set up an Azure AD tenant</span></span>

<span data-ttu-id="d48ea-138">Azure AD、Azure AD ディレクトリと呼びます[テナント](https://technet.microsoft.com/library/jj573650.aspx)、非常に簡単には、テナントを設定するとします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-138">an Azure AD directory is called an Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx), and setting up a tenant is pretty easy.</span></span> <span data-ttu-id="d48ea-139">紹介する方法、Azure 管理ポータルで、概念を説明するためには、当然のことながら、他のポータル関数と同様に行うことも、スクリプトまたは管理 API を使用しています。</span><span class="sxs-lookup"><span data-stu-id="d48ea-139">We'll show you how it's done in the Azure Management Portal in order to illustrate the concepts, but of course like the other portal functions you can also do it by using a script or management API.</span></span>

<span data-ttu-id="d48ea-140">管理ポータルでは、Active Directory タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-140">In the management portal click the Active Directory tab.</span></span>

![ポータルで WAAD](single-sign-on/_static/image5.png)

<span data-ttu-id="d48ea-142">Azure アカウントに自動的に 1 つの Azure AD テナントがあるしをクリックして、**追加**追加のディレクトリを作成するページの下部にあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-142">You automatically have one Azure AD tenant for your Azure account, and you can click the **Add** button at the bottom of the page to create additional directories.</span></span> <span data-ttu-id="d48ea-143">テスト環境を実稼働環境での 1 つをたとえば場合があります。</span><span class="sxs-lookup"><span data-stu-id="d48ea-143">You might want one for a test environment and one for production, for example.</span></span> <span data-ttu-id="d48ea-144">新しいディレクトリの名前について慎重に検討します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-144">Think carefully about what you name a new directory.</span></span> <span data-ttu-id="d48ea-145">場合は、ディレクトリの名前を使用して、いずれかの混乱を招くことができると、ユーザー用にもう一度、名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-145">If you use your name for the directory and then you use your name again for one of the users, that can be confusing.</span></span>

![ディレクトリを追加します。](single-sign-on/_static/image6.png)

<span data-ttu-id="d48ea-147">ポータルが、作成、削除、およびこの環境内でユーザーの管理を完全にサポートします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-147">The portal has full support for creating, deleting, and managing users within this environment.</span></span> <span data-ttu-id="d48ea-148">など、追加するユーザーに、**ユーザー**  タブでをクリックし、**ユーザーの追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-148">For example, to add a user go to the **Users** tab and click the **Add User** button.</span></span>

![ユーザー ボタンを追加します。](single-sign-on/_static/image7.png)

![追加のユーザー ダイアログ ボックス](single-sign-on/_static/image8.png)

<span data-ttu-id="d48ea-151">存在する新しいユーザーを作成するには、このディレクトリでのみ、またはこのディレクトリ内のユーザーとして、このディレクトリまたはレジスタ内のユーザーまたは別の Azure AD ディレクトリからユーザーとして Microsoft アカウントを登録することができます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-151">You can create a new user who exists only in this directory, or you can register a Microsoft Account as a user in this directory, or register or a user from another Azure AD directory as a user in this directory.</span></span> <span data-ttu-id="d48ea-152">(実際のディレクトリに既定のドメインになります ContosoTest.onmicrosoft.com です。使用できます、ドメイン contoso.com のように、独自に選択します。)</span><span class="sxs-lookup"><span data-stu-id="d48ea-152">(In a real directory, the default domain would be ContosoTest.onmicrosoft.com. You can also use a domain of your own choosing, like contoso.com.)</span></span>

![ユーザーの種類](single-sign-on/_static/image9.png)

![追加のユーザー ダイアログ ボックス](single-sign-on/_static/image10.png)

<span data-ttu-id="d48ea-155">ロールにユーザーを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-155">You can assign the user to a role.</span></span>

![ユーザー プロファイル](single-sign-on/_static/image11.png)

<span data-ttu-id="d48ea-157">一時パスワードを持つアカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-157">And the account is created with a temporary password.</span></span>

![一時パスワード](single-sign-on/_static/image12.png)

<span data-ttu-id="d48ea-159">この方法で作成したユーザーは、このクラウド ディレクトリを使用して、web アプリにすぐにログインできます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-159">The users you create this way can immediately log in to your web apps using this cloud directory.</span></span>

<span data-ttu-id="d48ea-160">ただし、エンタープライズ シングル サインオンに最適なが、**ディレクトリ統合** タブ。</span><span class="sxs-lookup"><span data-stu-id="d48ea-160">What's great for enterprise single sign-on, though, is the **Directory Integration** tab:</span></span>

![ディレクトリ統合 タブ](single-sign-on/_static/image13.png)

<span data-ttu-id="d48ea-162">ディレクトリ統合を有効にした場合と[ツールをダウンロード](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)組織内既に使用している既存の内部設置型 Active Directory とは、このクラウド ディレクトリを同期することができます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-162">If you enable directory integration, and [download a tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), you can sync this cloud directory with your existing on-premises Active Directory that you're already using inside your organization.</span></span> <span data-ttu-id="d48ea-163">すべてのディレクトリに格納されているユーザーに表示されますこのクラウド ディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="d48ea-163">Then all of the users stored in your directory will show up in this cloud directory.</span></span> <span data-ttu-id="d48ea-164">クラウド アプリでは、すべての既存の Active Directory の資格情報を使用して、従業員の認証できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="d48ea-164">Your cloud apps can now authenticate all of your employees using their existing Active Directory credentials.</span></span> <span data-ttu-id="d48ea-165">同期ツールと Azure AD 自体 – これはすべて無料です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-165">And all this is free – both the sync tool and Azure AD itself.</span></span>

<span data-ttu-id="d48ea-166">ツールは、以下のスクリーン ショットからわかるように簡単に使用されるウィザードです。</span><span class="sxs-lookup"><span data-stu-id="d48ea-166">The tool is a wizard that is easy to use, as you can see from these screen shots.</span></span> <span data-ttu-id="d48ea-167">これらは、完全な手順については、単なる基本的なプロセスを示す例ではありません。</span><span class="sxs-lookup"><span data-stu-id="d48ea-167">These are not complete instructions, just an example showing you the basic process.</span></span> <span data-ttu-id="d48ea-168">詳細 how に-は it については、リンクを参照して、[リソース](#resources)章の末尾。</span><span class="sxs-lookup"><span data-stu-id="d48ea-168">For more detailed how-to-do-it information, see the links in the [Resources](#resources) section at the end of the chapter.</span></span>

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image14.png)

<span data-ttu-id="d48ea-170">をクリックして**次**、Azure Active Directory の資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-170">Click **Next**, and then enter your Azure Active Directory credentials.</span></span>

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image15.png)

<span data-ttu-id="d48ea-172">をクリックして**[次へ]**、し、内部設置型を入力し、AD の資格情報。</span><span class="sxs-lookup"><span data-stu-id="d48ea-172">Click **Next**, and then enter your on-premises AD credentials.</span></span>

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image16.png)

<span data-ttu-id="d48ea-174">をクリックして**次**を示し、クラウドで AD パスワードのハッシュを保存するかどうか。</span><span class="sxs-lookup"><span data-stu-id="d48ea-174">Click **Next**, and then indicate if you want to store a hash of your AD passwords in the cloud.</span></span>

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image17.png)

<span data-ttu-id="d48ea-176">クラウドに格納できるパスワード ハッシュは、一方向のハッシュです。実際のパスワードは、Azure AD では格納されません。</span><span class="sxs-lookup"><span data-stu-id="d48ea-176">The password hash that you can store in the cloud is a one-way hash; actual passwords are never stored in Azure AD.</span></span> <span data-ttu-id="d48ea-177">に対してクラウドでハッシュを保存する場合は、使用する必要があります[Active Directory フェデレーション サービス](https://technet.microsoft.com/library/hh831502.aspx)(ADFS)。</span><span class="sxs-lookup"><span data-stu-id="d48ea-177">If you decide against storing hashes in the cloud, you'll have to use [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span></span> <span data-ttu-id="d48ea-178">[ときに考慮するその他の要因 ADFS を使用するかどうかを選択する](https://technet.microsoft.com/library/jj573653.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-178">There are also [other factors to consider when choosing whether or not to use ADFS](https://technet.microsoft.com/library/jj573653.aspx).</span></span> <span data-ttu-id="d48ea-179">ADFS オプションには、いくつかの追加の構成手順が必要です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-179">The ADFS option requires a few additional configuration steps.</span></span>

<span data-ttu-id="d48ea-180">クラウド内のハッシュを保存する場合は、完了したら、ツールは、クリックすると、ディレクトリの同期を開始**次**です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-180">If you choose to store hashes in the cloud, you're done, and the tool starts synchronizing directories when you click **Next**.</span></span>

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image18.png)

<span data-ttu-id="d48ea-182">完了したら、数分後にします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-182">And in a few minutes you're done.</span></span>

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image19.png)

<span data-ttu-id="d48ea-184">のみ、以降 Windows 2003 では、組織内の 1 つのドメイン コント ローラーでこれを実行する必要があるとします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-184">You only have to run this on one domain controller in the organization, on Windows 2003 or higher.</span></span> <span data-ttu-id="d48ea-185">再起動する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d48ea-185">And no need to reboot.</span></span> <span data-ttu-id="d48ea-186">完了したら、すべてのユーザーは、クラウドと行うことができますでのシングル サインオンから任意の web またはモバイル アプリケーション、SAML、OAuth、または Ws-fed を使用します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-186">When you're done, all of your users are in the cloud and you can do single sign-on from any web or mobile application, using SAML, OAuth, or WS-Fed.</span></span>

<span data-ttu-id="d48ea-187">これは、セキュリティで保護する方法について尋ね場合があります: は Microsoft を使用して、自分の機密性の高いビジネス データのですか。</span><span class="sxs-lookup"><span data-stu-id="d48ea-187">Sometimes we get asked about how secure this is – does Microsoft use it for their own sensitive business data?</span></span> <span data-ttu-id="d48ea-188">答えがはいの作業を行います。</span><span class="sxs-lookup"><span data-stu-id="d48ea-188">And the answer is yes we do.</span></span> <span data-ttu-id="d48ea-189">内部の Microsoft SharePoint サイトにアクセスする場合など、 [ https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/)にログインを要求を取得します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-189">For example, if you go to the internal Microsoft SharePoint site at [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), you get prompted to log in.</span></span>

![Office 365 のサインイン](single-sign-on/_static/image20.png)

<span data-ttu-id="d48ea-191">Microsoft には、ad FS が有効に Microsoft ID を入力すると、ADFS ログイン ページにリダイレクトを取得するようにします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-191">Microsoft has enabled ADFS, so when you enter a Microsoft ID, you get redirected to an ADFS log-in page.</span></span>

![Ad FS のサインイン](single-sign-on/_static/image21.png)

<span data-ttu-id="d48ea-193">内部 Microsoft AD アカウントに格納されている資格情報を入力するとを使用してこの社内アプリケーションにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-193">And once you enter credentials stored in an internal Microsoft AD account, you have access to this internal application.</span></span>

![MS SharePoint サイト](single-sign-on/_static/image22.png)

<span data-ttu-id="d48ea-195">既に ADFS が Azure AD が使用可能になりましたが、ログイン プロセスは、クラウドで Azure AD ディレクトリを通過する前に設定されていたために主にお AD サインオンでサーバーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="d48ea-195">We're using an AD sign-in server mainly because we already had ADFS set up before Azure AD became available, but the log-in process is going through an Azure AD directory in the cloud.</span></span> <span data-ttu-id="d48ea-196">重要なドキュメント、ソース管理、パフォーマンス管理ファイル、売上レポート、および詳細は、クラウド内に配置してを保護するため、これとまったく同じソリューションを使用しています。</span><span class="sxs-lookup"><span data-stu-id="d48ea-196">We put our important documents, source control, performance management files, sales reports, and more, in the cloud and are using this exact same solution to secure them.</span></span>

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a><span data-ttu-id="d48ea-197">Azure AD でのシングル サインオンを使用する ASP.NET アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-197">Create an ASP.NET app that uses Azure AD for single sign-on</span></span>

<span data-ttu-id="d48ea-198">Visual Studio では、本当に楽にいくつかのスクリーン ショットからわかるように、Azure AD シングル サインオンを使用するアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-198">Visual Studio makes it really easy to create an app that uses Azure AD for single sign-on, as you can see from a few screen shots.</span></span>

<span data-ttu-id="d48ea-199">新しい ASP.NET アプリケーション、MVC または Web フォームのいずれかを作成するとき、既定の認証方法は ASP.NET Identity です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-199">When you create a new ASP.NET application, either MVC or Web Forms, the default authentication method is ASP.NET Identity.</span></span> <span data-ttu-id="d48ea-200">クリックすることを Azure AD に変更するには**変更認証**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d48ea-200">To change that to Azure AD, you click a **Change Authentication** button.</span></span>

![認証の変更](single-sign-on/_static/image23.png)

<span data-ttu-id="d48ea-202">組織アカウントを選択して、ドメイン名を入力およびシングル サインオンを選択し、します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-202">Select Organizational Accounts, enter your domain name, and then select Single Sign On.</span></span>

![認証のダイアログ ボックスを構成します。](single-sign-on/_static/image24.png)

<span data-ttu-id="d48ea-204">また、アプリの読み取りを付与したりディレクトリ データに対するアクセス許可の読み取り/書き込みできます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-204">You can also give the app read or read/write permission for directory data.</span></span> <span data-ttu-id="d48ea-205">使用できることを行うと場合、 [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx)をユーザーの電話番号を調べる調べる、最終ログオンしているときなどに、オフィスにいるかどうか。</span><span class="sxs-lookup"><span data-stu-id="d48ea-205">If you do that, it can use the [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) to look up users' phone number, find out if they're in the office, when they last logged on, etc.</span></span>

<span data-ttu-id="d48ea-206">行うに必要である Visual Studio の資格情報を要求、Azure AD テナントの管理者と、新しいアプリケーションの Azure AD テナントと、プロジェクトの両方を構成します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-206">That's all you have to do - Visual Studio asks for the credentials for an administrator for your Azure AD tenant, and then it configures both your project and your Azure AD tenant for the new application.</span></span>

<span data-ttu-id="d48ea-207">プロジェクトを実行してサインイン ページが表示されます、Azure AD ディレクトリ内のユーザーの資格情報でサインインすることができます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-207">When you run the project, you'll see a sign-in page, and you can sign in with credentials of a user in your Azure AD directory.</span></span>

![組織アカウントでサインイン](single-sign-on/_static/image25.png)

![現在のサインイン](single-sign-on/_static/image26.png)

<span data-ttu-id="d48ea-210">アプリを Azure にデプロイするときに必要なを行うには、選択、**組織認証を有効にする**チェック ボックスをオンし、もう一度 Visual Studio はのすべての構成を処理します。</span><span class="sxs-lookup"><span data-stu-id="d48ea-210">When you deploy the app to Azure, all you have to do is select an **Enable Organizational Authentication** check box, and once again Visual Studio takes care of all the configuration for you.</span></span>

![Web を発行します。](single-sign-on/_static/image27.png)

<span data-ttu-id="d48ea-212">Azure AD の認証を使用するアプリをビルドする方法を示す完全なステップ バイ ステップ チュートリアル由来以下のスクリーン ショット: [Azure Active Directory での ASP.NET アプリの開発](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-212">These screen shots come from a complete step-by-step tutorial that shows how to build an app that uses Azure AD authentication: [Developing ASP.NET Apps with Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span></span>

## <a name="summary"></a><span data-ttu-id="d48ea-213">まとめ</span><span class="sxs-lookup"><span data-stu-id="d48ea-213">Summary</span></span>

<span data-ttu-id="d48ea-214">この章で、Azure Active Directory、Visual Studio、および ASP.NET を簡単に、組織のユーザーのインターネット アプリケーションでのシングル サインオンを設定を確認しました。</span><span class="sxs-lookup"><span data-stu-id="d48ea-214">In this chapter you saw that Azure Active Directory, Visual Studio, and ASP.NET, make it easy to set up single sign-on in Internet applications for your organization's users.</span></span> <span data-ttu-id="d48ea-215">ユーザーは、内部ネットワークの Active Directory を使用してサインオンを使用して、同じ資格情報を使用してアプリをインターネットでサインオンできます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-215">Your users can sign on in Internet apps using the same credentials they use to sign on using Active Directory in your internal network.</span></span>

<span data-ttu-id="d48ea-216">[次のチャプター](data-storage-options.md)クラウド アプリの使用可能なデータ ストレージのオプションを見ます。</span><span class="sxs-lookup"><span data-stu-id="d48ea-216">The [next chapter](data-storage-options.md) looks at the data storage options available for a cloud app.</span></span>

<a id="resources"></a>
## <a name="resources"></a><span data-ttu-id="d48ea-217">リソース</span><span class="sxs-lookup"><span data-stu-id="d48ea-217">Resources</span></span>

<span data-ttu-id="d48ea-218">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d48ea-218">For more information, see the following resources:</span></span>

- <span data-ttu-id="d48ea-219">[Azure Active Directory のドキュメント](https://docs.microsoft.com/azure/active-directory/)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-219">[Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="d48ea-220">Windowsazure.com サイトのドキュメントについては Azure AD ポータルのページです。</span><span class="sxs-lookup"><span data-stu-id="d48ea-220">Portal page for Azure AD documentation on the windowsazure.com site.</span></span> <span data-ttu-id="d48ea-221">ステップ バイ ステップ チュートリアルについては、次を参照してください。、**開発**セクションです。</span><span class="sxs-lookup"><span data-stu-id="d48ea-221">For step by step tutorials, see the **Develop** section.</span></span>
- <span data-ttu-id="d48ea-222">[Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-222">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span></span> <span data-ttu-id="d48ea-223">Azure で多要素認証に関するドキュメントについてはポータル ページです。</span><span class="sxs-lookup"><span data-stu-id="d48ea-223">Portal page for documentation about multi-factor authentication in Azure.</span></span>
- <span data-ttu-id="d48ea-224">[組織アカウントの認証オプション](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-224">[Organizational account authentication options](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span></span> <span data-ttu-id="d48ea-225">Visual Studio 2013 の [新しいプロジェクト] ダイアログ ボックスで、Azure AD の認証オプションの説明です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-225">Explanation of the Azure AD authentication options in the Visual Studio 2013 new-project dialog.</span></span>
- <span data-ttu-id="d48ea-226">[Microsoft Patterns and Practices - Federated Identity パターン](https://msdn.microsoft.com/library/dn589790.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-226">[Microsoft Patterns and Practices - Federated Identity Pattern](https://msdn.microsoft.com/library/dn589790.aspx).</span></span>
- <span data-ttu-id="d48ea-227">[方法: Azure Active Directory 同期ツールのインストール](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-227">[HowTo: Install the Azure Active Directory Sync Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span></span>
- <span data-ttu-id="d48ea-228">[Active Directory フェデレーション サービス 2.0 コンテンツ マップ](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-228">[Active Directory Federation Services 2.0 Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span></span> <span data-ttu-id="d48ea-229">Ad FS 2.0 に関するドキュメントへのリンク。</span><span class="sxs-lookup"><span data-stu-id="d48ea-229">Links to documentation about ADFS 2.0.</span></span>
- <span data-ttu-id="d48ea-230">[Windows Azure AD アプリケーションでのロールベースおよび ACL ベース承認](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-230">[Role-Based and ACL-Based Authorization in a Windows Azure AD Application](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span></span> <span data-ttu-id="d48ea-231">サンプル アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="d48ea-231">Sample application.</span></span>
- <span data-ttu-id="d48ea-232">[Azure Active Directory Graph API のブログ](https://blogs.msdn.com/b/aadgraphteam/)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-232">[Azure Active Directory Graph API blog](https://blogs.msdn.com/b/aadgraphteam/).</span></span>
- <span data-ttu-id="d48ea-233">[アクセス制御では、BYOD とハイブリッド Id インフラストラクチャのディレクトリ統合](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=)です。</span><span class="sxs-lookup"><span data-stu-id="d48ea-233">[Access Control in BYOD and Directory Integration in a Hybrid Identity Infrastructure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span></span> <span data-ttu-id="d48ea-234">技術 Ed 2014 セッション ビデオ Gayana Bagdasaryan によってです。</span><span class="sxs-lookup"><span data-stu-id="d48ea-234">Tech Ed 2014 session video by Gayana Bagdasaryan.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d48ea-235">[前へ](web-development-best-practices.md)
> [次へ](data-storage-options.md)</span><span class="sxs-lookup"><span data-stu-id="d48ea-235">[Previous](web-development-best-practices.md)
[Next](data-storage-options.md)</span></span>
