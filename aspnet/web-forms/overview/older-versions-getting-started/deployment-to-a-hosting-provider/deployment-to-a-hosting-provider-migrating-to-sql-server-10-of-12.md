---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: "SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します SQL Server - 12 の 10 への移行 |。Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: b97834e3e287645151bf927996fde63d93ae8356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a><span data-ttu-id="bb951-103">SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します SQL Server - 12 の 10 への移行。</span><span class="sxs-lookup"><span data-stu-id="bb951-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Migrating to SQL Server - 10 of 12</span></span>
====================
<span data-ttu-id="bb951-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="bb951-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="bb951-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="bb951-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="bb951-106">この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="bb951-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="bb951-107">Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="bb951-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="bb951-108">系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="bb951-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="bb951-109">チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="bb951-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="bb951-110">概要</span><span class="sxs-lookup"><span data-stu-id="bb951-110">Overview</span></span>

<span data-ttu-id="bb951-111">このチュートリアルでは、SQL Server Compact から SQL Server に移行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bb951-111">This tutorial shows you how to migrate from SQL Server Compact to SQL Server.</span></span> <span data-ttu-id="bb951-112">SQL Server Compact をサポートしていないストアド プロシージャ、トリガー、ビュー、またはレプリケーションなど、SQL Server の機能を活用するためには、理由の 1 つを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="bb951-112">One reason you might want to do that is to take advantage of SQL Server features that SQL Server Compact does not support, such as stored procedures, triggers, views, or replication.</span></span> <span data-ttu-id="bb951-113">SQL Server Compact および SQL Server の違いの詳細については、次を参照してください。、[を展開する SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="bb951-113">For more information about the differences between SQL Server Compact and SQL Server, see the [Deploying SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

### <a name="sql-server-express-versus-full-sql-server-for-development"></a><span data-ttu-id="bb951-114">開発の完全な SQL Server と SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="bb951-114">SQL Server Express versus full SQL Server for Development</span></span>

<span data-ttu-id="bb951-115">SQL Server にアップグレードすることが決まっているとは、開発およびテスト環境で SQL Server または SQL Server Express を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="bb951-115">Once you've decided to upgrade to SQL Server, you might want to use SQL Server or SQL Server Express in your development and test environments.</span></span> <span data-ttu-id="bb951-116">ツールのサポートとデータベース エンジン機能の違い、に加えて SQL Server Compact とその他のバージョンの SQL Server プロバイダーの実装で違いがあります。</span><span class="sxs-lookup"><span data-stu-id="bb951-116">In addition to the differences in tool support and in database engine features, there are differences in provider implementations between SQL Server Compact and other versions of SQL Server.</span></span> <span data-ttu-id="bb951-117">これらの相違点には、同じコードを異なる結果を生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-117">These differences can cause the same code to generate different results.</span></span> <span data-ttu-id="bb951-118">そのため、SQL Server Compact、開発用データベースとして保持する場合は、必要があります十分にテストするサイトで SQL Server または SQL Server Express 実稼働環境にそれぞれ配置する前に、テスト環境でします。</span><span class="sxs-lookup"><span data-stu-id="bb951-118">Therefore, if you decide to keep SQL Server Compact as your development database, you should thoroughly test your site in SQL Server or SQL Server Express in a test environment before each deployment to production.</span></span>

<span data-ttu-id="bb951-119">異なり、SQL Server Compact は SQL Server Express を同じデータベース エンジンでは基本的には、および完全な SQL Server と同じ .NET プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="bb951-119">Unlike SQL Server Compact, SQL Server Express is essentially the same database engine and uses the same .NET provider as full SQL Server.</span></span> <span data-ttu-id="bb951-120">SQL Server Express を使ってテストするときに、SQL Server では、同じ結果を取得することを確信できます。</span><span class="sxs-lookup"><span data-stu-id="bb951-120">When you test with SQL Server Express, you can be confident of getting the same results as you will with SQL Server.</span></span> <span data-ttu-id="bb951-121">SQL Server Express の SQL Server で使用できると同じデータベース ツールのほとんどを行うこともできます (されている主な例外[SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx))、ストアド プロシージャ、ビュー、トリガーなどのように SQL Server の他の機能がサポートされますおよびレプリケーション。</span><span class="sxs-lookup"><span data-stu-id="bb951-121">You can use most of the same database tools with SQL Server Express that you can use with SQL Server (a notable exception being [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), and it supports other features of SQL Server like stored procedures, views, triggers, and replication.</span></span> <span data-ttu-id="bb951-122">(通常ただし、実稼働 web サイトで完全な SQL Server を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-122">(You typically have to use full SQL Server in a production website, however.</span></span> <span data-ttu-id="bb951-123">共有ホスティング環境で SQL Server Express を実行できますが、向けに設計されていないと多くのホスティング プロバイダーではサポートされません。)</span><span class="sxs-lookup"><span data-stu-id="bb951-123">SQL Server Express can run in a shared hosting environment, but it was not designed for that, and many hosting providers do not support it.)</span></span>

<span data-ttu-id="bb951-124">Visual Studio 2012 を使用している場合は、通常開発環境に合わせて SQL Server Express LocalDB を選択する Visual Studio で既定でインストールされるものであるため。</span><span class="sxs-lookup"><span data-stu-id="bb951-124">If you are using Visual Studio 2012, you typically choose SQL Server Express LocalDB for your development environment because that is what is installed by default with Visual Studio.</span></span> <span data-ttu-id="bb951-125">ただし、LocalDB が動作しない IIS では、ので、テスト環境用には、SQL Server または SQL Server Express のいずれかを使用する必要があるあります。</span><span class="sxs-lookup"><span data-stu-id="bb951-125">However, LocalDB does not work in IIS, so for your test environment you have to use either SQL Server or SQL Server Express.</span></span>

### <a name="combining-databases-versus-keeping-them-separate"></a><span data-ttu-id="bb951-126">データベースを個別に維持すると結合</span><span class="sxs-lookup"><span data-stu-id="bb951-126">Combining Databases versus Keeping Them Separate</span></span>

<span data-ttu-id="bb951-127">Contoso 大学アプリケーションが 2 つの SQL Server Compact データベース: メンバーシップ データベース (*aspnet.sdf*) と、アプリケーション データベース (*School.sdf*)。</span><span class="sxs-lookup"><span data-stu-id="bb951-127">The Contoso University application has two SQL Server Compact databases: the membership database (*aspnet.sdf*) and the application database (*School.sdf*).</span></span> <span data-ttu-id="bb951-128">移行する場合、2 つの異なるデータベースまたは 1 つのデータベースにこれらのデータベースを移行できます。</span><span class="sxs-lookup"><span data-stu-id="bb951-128">When you migrate, you can migrate these databases to two separate databases or to a single database.</span></span> <span data-ttu-id="bb951-129">アプリケーションのデータベースと、メンバーシップ データベース間の結合がデータベースを容易にするために結合する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-129">You might want to combine them in order to facilitate database joins between your application database and your membership database.</span></span> <span data-ttu-id="bb951-130">ホスティング プランには、それらを結合する理由を指定も可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-130">Your hosting plan might also provide a reason to combine them.</span></span> <span data-ttu-id="bb951-131">ホスティング プロバイダーの複数のデータベースの課金する場合がありますなど、複数のデータベースをも許可しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-131">For example, the hosting provider might charge more for multiple databases or might not even allow more than one database.</span></span> <span data-ttu-id="bb951-132">これは、ような Cytanium Lite でこのチュートリアルでは、により、1 つの SQL Server データベースだけに使用されるアカウントをホストするいるとします。</span><span class="sxs-lookup"><span data-stu-id="bb951-132">That's the case with the Cytanium Lite hosting account that's used for this tutorial, which allows only a single SQL Server database.</span></span>

<span data-ttu-id="bb951-133">このチュートリアルではこのように、2 つのデータベースを移行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-133">In this tutorial, you'll migrate your two databases this way:</span></span>

- <span data-ttu-id="bb951-134">開発環境で 2 つの LocalDB データベースを移行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-134">Migrate to two LocalDB databases in the development environment.</span></span>
- <span data-ttu-id="bb951-135">テスト環境で 2 つの SQL Server Express データベースを移行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-135">Migrate to two SQL Server Express databases in the test environment.</span></span>
- <span data-ttu-id="bb951-136">1 つ結合された SQL Server データベースの完全実稼働環境で移行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-136">Migrate to one combined full SQL Server database in the production environment.</span></span>

<span data-ttu-id="bb951-137">アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="bb951-137">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="installing-sql-server-express"></a><span data-ttu-id="bb951-138">SQL Server Express をインストールします。</span><span class="sxs-lookup"><span data-stu-id="bb951-138">Installing SQL Server Express</span></span>

<span data-ttu-id="bb951-139">既定では、Visual Studio 2010 が自動的にインストールされている SQL Server Express しますが、既定でインストールされていない Visual Studio 2012 で。</span><span class="sxs-lookup"><span data-stu-id="bb951-139">SQL Server Express is automatically installed by default with Visual Studio 2010, but by default it is not installed with Visual Studio 2012.</span></span> <span data-ttu-id="bb951-140">SQL Server 2012 Express をインストールするには、次のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="bb951-140">To install SQL Server 2012 Express, click the following link</span></span>

- [<span data-ttu-id="bb951-141">SQL Server Express 2012</span><span class="sxs-lookup"><span data-stu-id="bb951-141">SQL Server Express 2012</span></span>](https://www.microsoft.com/download/details.aspx?id=29062)

<span data-ttu-id="bb951-142">選択*日本語/x64/SQLEXPR\_x64\_ENU.exe*または*日本語/x86/SQLEXPR\_x86\_ENU.exe*、インストール ウィザードで、既定設定。</span><span class="sxs-lookup"><span data-stu-id="bb951-142">Choose *ENU/x64/SQLEXPR\_x64\_ENU.exe* or *ENU/x86/SQLEXPR\_x86\_ENU.exe*, and in the installation wizard accept the default settings.</span></span> <span data-ttu-id="bb951-143">インストール オプションの詳細については、次を参照してください。[インストール ウィザード (セットアップ) からの SQL Server 2012 のインストール](https://msdn.microsoft.com/library/ms143219.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="bb951-143">For more information about installation options, see [Install SQL Server 2012 from the Installation Wizard (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).</span></span>

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a><span data-ttu-id="bb951-144">テスト環境の SQL Server Express データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-144">Creating SQL Server Express Databases for the Test Environment</span></span>

<span data-ttu-id="bb951-145">次の手順では、ASP.NET メンバーシップおよび School データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-145">The next step is to create the ASP.NET membership and School databases.</span></span>

<span data-ttu-id="bb951-146">**ビュー**メニュー選択**サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Visual Web Developer で)、し、右クリックし、**データ接続**選択**新しい SQL Server データベースの作成**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-146">From the **View** menu select **Server Explorer** (**Database Explorer** in Visual Web Developer), and then right-click **Data Connections** and select **Create New SQL Server Database**.</span></span>

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

<span data-ttu-id="bb951-148">**新しい SQL Server データベースの作成**] ダイアログ ボックスで、入力". \SQLExpress"で、**サーバー名**ボックスと"aspnet Test"で、**新しいデータベース名**のボックスで、[**OK**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-148">In the **Create New SQL Server Database** dialog box, enter ".\SQLExpress" in the **Server name** box and "aspnet-Test" in the **New database name** box, then click **OK**.</span></span>

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

<span data-ttu-id="bb951-150">"学校 Test"という新しい SQL Server Express の School データベースを作成する同じ手順に従います。</span><span class="sxs-lookup"><span data-stu-id="bb951-150">Follow the same procedure to create a new SQL Server Express School database named "School-Test".</span></span>

<span data-ttu-id="bb951-151">(追加する"Test"これらのデータベース名を後で、開発環境の各データベースの追加インスタンスを作成し、データベースの 2 つのセットを区別する必要があるためです。)</span><span class="sxs-lookup"><span data-stu-id="bb951-151">(You're appending "Test" to these database names because later you'll create an additional instance of each database for the development environment, and you need to be able to differentiate the two sets of databases.)</span></span>

<span data-ttu-id="bb951-152">**サーバー エクスプ ローラー** 2 つの新しいデータベースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb951-152">**Server Explorer** now shows the two new databases.</span></span>

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a><span data-ttu-id="bb951-154">新しいデータベースの Grant スクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-154">Creating a Grant Script for the New Databases</span></span>

<span data-ttu-id="bb951-155">アプリケーションを開発用コンピューターに IIS で実行すると、アプリケーションは既定のアプリケーション プールの資格情報を使用して、データベースにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="bb951-155">When the application runs in IIS on your development computer, the application accesses the database by using the default application pool's credentials.</span></span> <span data-ttu-id="bb951-156">ただし、既定では、アプリケーション プール id は、データベースを開くアクセス許可がありません。</span><span class="sxs-lookup"><span data-stu-id="bb951-156">However, by default, the application pool identity does not have permission to open the databases.</span></span> <span data-ttu-id="bb951-157">ように権限を付与するスクリプトを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-157">So you have to run a script to grant that permission.</span></span> <span data-ttu-id="bb951-158">このセクションでは、後で、アプリケーションも、IIS での実行時に、データベースを開くことができるかどうかを確認を実行しますスクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-158">In this section you create the script that you'll run later to make sure that the application can open the databases when it runs in IIS.</span></span>

<span data-ttu-id="bb951-159">ソリューションの*SolutionFiles*で作成したフォルダー、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルでは、という名前の新しい SQL ファイルを作成する*Grant.sql*です。</span><span class="sxs-lookup"><span data-stu-id="bb951-159">In the solution's *SolutionFiles* folder that you created in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, create a new SQL file named *Grant.sql*.</span></span> <span data-ttu-id="bb951-160">ファイルに次の SQL コマンドをコピーおよび保存し、ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="bb951-160">Copy the following SQL commands into the file, and then save and close the file:</span></span>

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> <span data-ttu-id="bb951-161">このスクリプトは、このチュートリアルでは指定されているように SQL Server 2008 と Windows 7 で IIS 設定を操作する設計されています。</span><span class="sxs-lookup"><span data-stu-id="bb951-161">This script is designed to work with SQL Server 2008 and with the IIS settings in Windows 7 as they are specified in this tutorial.</span></span> <span data-ttu-id="bb951-162">または、Windows の SQL Server の別のバージョンを使用している場合、または IIS を設定するコンピューターに異なる場合は、次のスクリプトへの変更が必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-162">If you're using a different version of SQL Server or of Windows, or if you set up IIS on your computer differently, changes to this script might be required.</span></span> <span data-ttu-id="bb951-163">SQL Server スクリプトの詳細については、次を参照してください。 [SQL Server オンライン ブック](https://go.microsoft.com/fwlink/?LinkId=132511)です。</span><span class="sxs-lookup"><span data-stu-id="bb951-163">For more information about SQL Server scripts, see [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="bb951-164">**セキュリティに関する注意**このスクリプトは、db\_、実稼働環境で必要がありますが、実行時に、データベースにアクセスするユーザーを所有者のアクセス許可。</span><span class="sxs-lookup"><span data-stu-id="bb951-164">**Security Note** This script gives db\_owner permissions to the user that accesses the database at run time, which is what you'll have in the production environment.</span></span> <span data-ttu-id="bb951-165">一部のシナリオでのみ展開については、アクセス許可を更新し、実行時のデータを読み書きするのみのアクセス許可を持つ別のユーザーを指定します。 完全なデータベース スキーマを持つユーザーを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="bb951-165">In some scenarios you might want to specify a user that has full database schema update permissions only for deployment, and specify for run time a different user that has permissions only to read and write data.</span></span> <span data-ttu-id="bb951-166">詳細については、次を参照してください。 **Code First Migrations を自動の Web.config の変更を確認**で[テスト環境として IIS に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="bb951-166">For more information, see **Reviewing the Automatic Web.config Changes for Code First Migrations** in [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).</span></span>


## <a name="configuring-database-deployment-for-the-test-environment"></a><span data-ttu-id="bb951-167">データベースの配置、テスト環境用の構成</span><span class="sxs-lookup"><span data-stu-id="bb951-167">Configuring Database Deployment for the Test Environment</span></span>

<span data-ttu-id="bb951-168">次に、データベースごとに、次のタスクを実行できるように、Visual Studio を構成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-168">Next, you'll configure Visual Studio so that it will do the following tasks for each database:</span></span>

- <span data-ttu-id="bb951-169">転送先データベースに、ソース データベースの構造 (テーブル、列、制約など) を作成する SQL スクリプトを生成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-169">Generate a SQL script that creates the source database's structure (tables, columns, constraints, etc.) in the destination database.</span></span>
- <span data-ttu-id="bb951-170">転送先データベース内のテーブルに、ソース データベースのデータを挿入する SQL スクリプトを生成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-170">Generate a SQL script that inserts the source database's data into the tables in the destination database.</span></span>
- <span data-ttu-id="bb951-171">生成されたスクリプト、および転送先データベースで、作成した許可スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-171">Run the generated scripts, and the Grant script that you created, in the destination database.</span></span>

<span data-ttu-id="bb951-172">開く、**プロジェクト プロパティ**ウィンドウを選択、**パッケージ化/発行 SQL**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-172">Open the **Project Properties** window and select the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="bb951-173">確認して**アクティブ (リリース)**または**リリース**でが選択されている、**構成**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="bb951-173">Make sure that **Active (Release)** or **Release** is selected in the **Configuration** drop-down list.</span></span>

<span data-ttu-id="bb951-174">をクリックして**このページを有効にする**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-174">Click **Enable this Page**.</span></span>

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

<span data-ttu-id="bb951-176">**パッケージ化/発行 SQL**従来の配置方法を指定するために通常タブは無効になります。</span><span class="sxs-lookup"><span data-stu-id="bb951-176">The **Package/Publish SQL** tab is normally disabled because it specifies a legacy deployment method.</span></span> <span data-ttu-id="bb951-177">ほとんどのシナリオでデータベースの配置を構成する必要があります、 **Web の発行**ウィザード。</span><span class="sxs-lookup"><span data-stu-id="bb951-177">For most scenarios, you should configure database deployment in the **Publish Web** wizard.</span></span> <span data-ttu-id="bb951-178">SQL Server Compact から SQL Server または SQL Server Express への移行は、このメソッドは、適切な選択特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="bb951-178">Migrating from SQL Server Compact to SQL Server or SQL Server Express is a special case for which this method is a good choice.</span></span>

<span data-ttu-id="bb951-179">をクリックして**web.config ファイルからインポート**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-179">Click **Import from Web.config**.</span></span>

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

<span data-ttu-id="bb951-181">Visual Studio が内の接続文字列を探して、 *Web.config*ファイル、メンバーシップ データベースに 1 つ、School データベースの 1 つを検索および内の各接続文字列に対応する行を追加、**データベース エントリ**テーブル。</span><span class="sxs-lookup"><span data-stu-id="bb951-181">Visual Studio looks for connection strings in the *Web.config* file, finds one for the membership database and one for the School database, and adds a row corresponding to each connection string in the **Database Entries** table.</span></span> <span data-ttu-id="bb951-182">接続文字列が見つかったが、既存の SQL Server Compact データベースに対して、次の手順は、方法と場所を構成するこれらのデータベースを展開します。</span><span class="sxs-lookup"><span data-stu-id="bb951-182">The connection strings it finds are for the existing SQL Server Compact databases, and your next step will be to configure how and where to deploy these databases.</span></span>

<span data-ttu-id="bb951-183">データベースのデプロイ設定を入力する、**データベース エントリの詳細**以下のセクション、**データベース エントリ**テーブル。</span><span class="sxs-lookup"><span data-stu-id="bb951-183">You enter database deployment settings in the **Database Entry Details** section below the **Database Entries** table.</span></span> <span data-ttu-id="bb951-184">表示される設定、**データベース エントリの詳細**セクションの行のどちらかに関係するもの、**データベース エントリ**テーブルが選択されている、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="bb951-184">The settings shown in the **Database Entry Details** section pertain to whichever row in the **Database Entries** table is selected, as shown in the following illustration.</span></span>

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a><span data-ttu-id="bb951-186">メンバーシップ データベースのデプロイ設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-186">Configuring Deployment Settings for the Membership Database</span></span>

<span data-ttu-id="bb951-187">選択、 **DefaultConnection 展開**行が、**データベース エントリ**メンバーシップ データベースに適用される設定を構成するためにテーブルです。</span><span class="sxs-lookup"><span data-stu-id="bb951-187">Select the **DefaultConnection-Deployment** row in the **Database Entries** table in order to configure settings that apply to the membership database.</span></span>

<span data-ttu-id="bb951-188">**転送先データベースへの接続文字列**、新しい SQL Server Express メンバーシップ データベースを指す接続文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="bb951-188">In **Connection string for destination database**, enter a connection string that points to the new SQL Server Express membership database.</span></span> <span data-ttu-id="bb951-189">必要な接続文字列を取得できます**サーバー エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-189">You can get the connection string you need from **Server Explorer**.</span></span> <span data-ttu-id="bb951-190">**サーバー エクスプ ローラー**、展開**データ接続**を選択し、 **aspnetTest**データベース、してから、**プロパティ**ウィンドウのコピー、**接続文字列**値。</span><span class="sxs-lookup"><span data-stu-id="bb951-190">In **Server Explorer**, expand **Data Connections** and select the **aspnetTest** database, then from the **Properties** window copy the **Connection String** value.</span></span>

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

<span data-ttu-id="bb951-192">同じ接続文字列はここで再作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-192">The same connection string is reproduced here:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

<span data-ttu-id="bb951-193">コピーして貼り付けるには、この接続文字列**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-193">Copy and paste this connection string into **Connection string for destination database** in the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="bb951-194">確認して**または既存のデータベースからスキーマのデータをプル**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="bb951-194">Make sure that **Pull data and/or schema from an existing database** is selected.</span></span> <span data-ttu-id="bb951-195">これは、何が原因で自動的に生成され、転送先データベースで実行する SQL スクリプトです。</span><span class="sxs-lookup"><span data-stu-id="bb951-195">This is what causes SQL scripts to be automatically generated and run in the destination database.</span></span>

<span data-ttu-id="bb951-196">**ソース データベースの接続文字列**から値を抽出、 *Web.config*ファイルと、開発用の SQL Server Compact データベースへのポインター。</span><span class="sxs-lookup"><span data-stu-id="bb951-196">The **Connection string for the source database** value is extracted from the *Web.config* file and points to the development SQL Server Compact database.</span></span> <span data-ttu-id="bb951-197">これは、ソース データベースをコピー先データベースに後で実行されるスクリプトの生成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="bb951-197">This is the source database that will be used to generate the scripts that will run later in the destination database.</span></span> <span data-ttu-id="bb951-198">データベースの運用バージョンを展開するには、以降は、"aspnet Dev.sdf"を"aspnet Prod.sdf"に変更します。</span><span class="sxs-lookup"><span data-stu-id="bb951-198">Since you want to deploy the production version of the database, change "aspnet-Dev.sdf" to "aspnet-Prod.sdf".</span></span>

<span data-ttu-id="bb951-199">変更**データベース スクリプト オプション**から**スキーマのみ**に**スキーマとデータ**だけでなく、データベースの構造 (ユーザー アカウントとロール) のデータをコピーするため、します。</span><span class="sxs-lookup"><span data-stu-id="bb951-199">Change **Database scripting options** from **Schema Only** to **Schema and data**, since you want to copy your data (user accounts and roles) as well as the database structure.</span></span>

<span data-ttu-id="bb951-200">先ほど作成した許可スクリプトを実行する配置を構成するには、それらを追加する必要が、**データベース スクリプト**セクションです。</span><span class="sxs-lookup"><span data-stu-id="bb951-200">To configure deployment to run the grant scripts that you created earlier, you have to add them to the **Database Scripts** section.</span></span> <span data-ttu-id="bb951-201">をクリックして**スクリプトの追加**、し、[、 **SQL スクリプトの追加**] ダイアログ ボックスで、許可スクリプトを保存したフォルダーに移動し (これは、ソリューション ファイルが含まれているフォルダーです)。</span><span class="sxs-lookup"><span data-stu-id="bb951-201">Click **Add Script**, and in the **Add SQL Scripts** dialog box, navigate to the folder where you stored the grant script (this is the folder that contains your solution file).</span></span> <span data-ttu-id="bb951-202">という名前のファイルを選択して*Grant.sql*、 をクリック**開く**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-202">Select the file named *Grant.sql*, and click **Open**.</span></span>

<span data-ttu-id="bb951-203">[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="bb951-203">[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)</span></span>

<span data-ttu-id="bb951-204">設定、 **DefaultConnection 展開**で行**データベース エントリ**次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="bb951-204">The settings for the **DefaultConnection-Deployment** row in **Database Entries** now look like the following illustration:</span></span>

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a><span data-ttu-id="bb951-206">School データベースのデプロイ設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-206">Configuring Deployment Settings for the School Database</span></span>

<span data-ttu-id="bb951-207">次に、選択、 **SchoolContext 展開**行が、**データベース エントリ**School データベースのデプロイ設定を構成するためにテーブルです。</span><span class="sxs-lookup"><span data-stu-id="bb951-207">Next, select the **SchoolContext-Deployment** row in the **Database Entries** table in order to configure deployment settings for the School database.</span></span>

<span data-ttu-id="bb951-208">新しい SQL Server Express データベースの接続文字列を取得する前に使用した同じメソッドを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="bb951-208">You can use the same method you used earlier to get the connection string for the new SQL Server Express database.</span></span> <span data-ttu-id="bb951-209">この接続文字列にコピー**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-209">Copy this connection string into **Connection string for destination database** in the **Package/Publish SQL** tab.</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

<span data-ttu-id="bb951-210">確認して**または既存のデータベースからスキーマのデータをプル**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="bb951-210">Make sure that **Pull data and/or schema from an existing database** is selected.</span></span>

<span data-ttu-id="bb951-211">**ソース データベースの接続文字列**から値を抽出、 *Web.config*ファイルと、開発用の SQL Server Compact データベースへのポインター。</span><span class="sxs-lookup"><span data-stu-id="bb951-211">The **Connection string for the source database** value is extracted from the *Web.config* file and points to the development SQL Server Compact database.</span></span> <span data-ttu-id="bb951-212">"学校 Prod.sdf"データベースの運用バージョンを展開するには、"学校 Dev.sdf"を変更します。</span><span class="sxs-lookup"><span data-stu-id="bb951-212">Change "School-Dev.sdf" to "School-Prod.sdf" to deploy the production version of the database.</span></span> <span data-ttu-id="bb951-213">(アプリの学校 Prod.sdf ファイルを作成することはありません\_データ フォルダー、アプリをテスト環境からそのファイルをコピーしますので\_ContosoUniversity プロジェクト フォルダーを後で、データ フォルダーです)。</span><span class="sxs-lookup"><span data-stu-id="bb951-213">(You never created a School-Prod.sdf file in the App\_Data folder, so you'll copy that file from the test environment to the App\_Data folder in the ContosoUniversity project folder later.)</span></span>

<span data-ttu-id="bb951-214">変更**データベース スクリプト オプション**に**スキーマとデータ**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-214">Change **Database scripting options** to **Schema and data**.</span></span>

<span data-ttu-id="bb951-215">読み取りを許可し、アプリケーション プール id にこのデータベースの権限を書き込み、ため、追加するスクリプトを実行する、 *Grant.sql*ファイル、メンバーシップ データベースの場合と同様のスクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-215">You also want to run the script to grant read and write permission for this database to the application pool identity, so add the *Grant.sql* script file as you did for the membership database.</span></span>

<span data-ttu-id="bb951-216">終了すると、設定**SchoolContext 展開**で行**データベース エントリ**次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="bb951-216">When you're done, the settings for the **SchoolContext-Deployment** row in **Database Entries** look like the following illustration:</span></span>

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

<span data-ttu-id="bb951-218">変更を保存、**パッケージ化/発行 SQL**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-218">Save the changes to the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="bb951-219">コピー、*学校 Prod.sdf*ファイルから、 *c:\inetpub\wwwroot\ContosoUniversity\App\_データ*フォルダーを*アプリ\_データ*フォルダーContosoUniversity のプロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="bb951-219">Copy the *School-Prod.sdf* file from the *c:\inetpub\wwwroot\ContosoUniversity\App\_Data* folder to the *App\_Data* folder in the ContosoUniversity project.</span></span>

### <a name="specifying-transacted-mode-for-the-grant-script"></a><span data-ttu-id="bb951-220">Grant スクリプトのトランザクション モードを指定します。</span><span class="sxs-lookup"><span data-stu-id="bb951-220">Specifying Transacted Mode for the Grant Script</span></span>

<span data-ttu-id="bb951-221">展開プロセスでは、データベース スキーマとデータを配置するスクリプトを生成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-221">The deployment process generates scripts that deploy the database schema and data.</span></span> <span data-ttu-id="bb951-222">既定では、これらのスクリプトは、トランザクションで実行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-222">By default, these scripts run in a transaction.</span></span> <span data-ttu-id="bb951-223">ただし、既定で (grant スクリプト) のようなカスタム スクリプトは、トランザクションでは実行されません。</span><span class="sxs-lookup"><span data-stu-id="bb951-223">However, custom scripts (like the grant scripts) by default do not run in a transaction.</span></span> <span data-ttu-id="bb951-224">展開プロセスでは、トランザクション モードを混在、展開時に、スクリプトの実行時にタイムアウト エラーが発生した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-224">If the deployment process mixes transaction modes, you might get a timeout error when the scripts run during deployment.</span></span> <span data-ttu-id="bb951-225">このセクションでは、トランザクションで実行するカスタム スクリプトを構成するためにプロジェクト ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="bb951-225">In this section, you edit the project file in order to configure the custom scripts to run in a transaction.</span></span>

<span data-ttu-id="bb951-226">**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクトし、選択**プロジェクトのアンロード**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-226">In **Solution Explorer**, right-click the **ContosoUniversity** project and select **Unload Project**.</span></span>

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

<span data-ttu-id="bb951-228">プロジェクトをもう一度右クリックし、選択**編集 ContosoUniversity.csproj**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-228">Then right-click the project again and select **Edit ContosoUniversity.csproj**.</span></span>

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

<span data-ttu-id="bb951-230">Visual Studio エディターでは、プロジェクト ファイルの XML コンテンツを示します。</span><span class="sxs-lookup"><span data-stu-id="bb951-230">The Visual Studio editor shows you the XML content of the project file.</span></span> <span data-ttu-id="bb951-231">含まれているいくつか`PropertyGroup`要素。</span><span class="sxs-lookup"><span data-stu-id="bb951-231">Notice that there are several `PropertyGroup` elements.</span></span> <span data-ttu-id="bb951-232">(図では、内容、`PropertyGroup`要素が省略されています)。</span><span class="sxs-lookup"><span data-stu-id="bb951-232">(In the image, the contents of the `PropertyGroup` elements have been omitted.)</span></span>

![プロジェクト ファイルのエディター ウィンドウ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

<span data-ttu-id="bb951-234">No を持つ最初の 1 つ`Condition`属性、ビルド構成の設定に関係なく適用されるは、します。</span><span class="sxs-lookup"><span data-stu-id="bb951-234">The first one, which has no `Condition` attribute, is for settings that apply regardless of build configuration.</span></span> <span data-ttu-id="bb951-235">1 つ`PropertyGroup`要素は、デバッグ ビルド構成にのみ適用されます (注、`Condition`属性) 1 つが、リリース ビルド構成にのみ適用されます、およびテストのビルド構成にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="bb951-235">One `PropertyGroup` element applies only to the Debug build configuration (note the `Condition` attribute), one applies only to the Release build configuration, and one applies only to the Test build configuration.</span></span> <span data-ttu-id="bb951-236">内で、 `PropertyGroup` 、リリース ビルド構成の要素が表示されます、`PublishDatabaseSettings`で入力した設定を含む要素、**パッケージ化/発行 SQL**タブです。`Object` Grant スクリプトのそれぞれに対応する要素 ("Grant.sql"のインスタンスを 2 つに注意してください) を指定します。</span><span class="sxs-lookup"><span data-stu-id="bb951-236">Within the `PropertyGroup` element for the Release build configuration, you'll see a `PublishDatabaseSettings` element that contains the settings you entered on the **Package/Publish SQL** tab. There is an `Object` element that corresponds to each of the grant scripts you specified (notice the two instances of "Grant.sql").</span></span> <span data-ttu-id="bb951-237">既定では、`Transacted`の属性、 `Source` grant スクリプトごとに要素が`False`です。</span><span class="sxs-lookup"><span data-stu-id="bb951-237">By default, the `Transacted` attribute of the `Source` element for each grant script is `False`.</span></span>

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

<span data-ttu-id="bb951-239">値を変更、`Transacted`の属性、`Source`要素を`True`です。</span><span class="sxs-lookup"><span data-stu-id="bb951-239">Change the value of the `Transacted` attribute of the `Source` element to `True`.</span></span>

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

<span data-ttu-id="bb951-241">保存して、プロジェクト ファイルを閉じでプロジェクトを右クリックし、**ソリューション エクスプ ローラー**選択**プロジェクトの再読み込み**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-241">Save and close the project file, and then right-click the project in **Solution Explorer** and select **Reload Project**.</span></span>

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a><span data-ttu-id="bb951-243">接続文字列の Web.Config 変換の設定</span><span class="sxs-lookup"><span data-stu-id="bb951-243">Setting up Web.Config Transformations for the Connection Strings</span></span>

<span data-ttu-id="bb951-244">接続文字列入力した新しい SQL Express データベースの**パッケージ化/発行 SQL**  タブは、配置時に転送先データベースを更新時にのみ Web Deploy によって使用されます。</span><span class="sxs-lookup"><span data-stu-id="bb951-244">The connection strings for the new SQL Express databases that you entered on the **Package/Publish SQL** tab are used by Web Deploy only for updating the destination database during deployment.</span></span> <span data-ttu-id="bb951-245">設定する必要がある*Web.config*変換できるように、接続文字列、デプロイされている*Web.config*ファイルの新しい SQL Server Express データベースをポイントします。</span><span class="sxs-lookup"><span data-stu-id="bb951-245">You still have to set up *Web.config* transformations so that the connection strings in the deployed *Web.config* file point to the new SQL Server Express databases.</span></span> <span data-ttu-id="bb951-246">(使用すると、**パッケージ化/発行 SQL**  タブで、発行プロファイルで接続文字列を構成することはできません)。</span><span class="sxs-lookup"><span data-stu-id="bb951-246">(When you use the **Package/Publish SQL** tab, you can't configure connection strings in the publish profile.)</span></span>

<span data-ttu-id="bb951-247">開いている*Web.Test.config*と置換、`connectionStrings`を持つ要素、`connectionStrings`例を次の要素。</span><span class="sxs-lookup"><span data-stu-id="bb951-247">Open *Web.Test.config* and replace the `connectionStrings` element with the `connectionStrings` element in the following example.</span></span> <span data-ttu-id="bb951-248">(周囲コードではなくコンテキストを提供するのには、ここに表示される、connectionStrings 要素のみをコピーすることを確認してください。)</span><span class="sxs-lookup"><span data-stu-id="bb951-248">(Make sure you only copy the connectionStrings element, not the surrounding code that is shown here to provide context.)</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

<span data-ttu-id="bb951-249">このコードにより、`connectionString`と`providerName`の各属性`add`置き換えられる要素で、デプロイされている*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bb951-249">This code causes the `connectionString` and `providerName` attributes of each `add` element to be replaced in the deployed *Web.config* file.</span></span> <span data-ttu-id="bb951-250">これらの接続文字列で入力したものとは異なるが、**パッケージ化/発行 SQL**タブです。設定"MultipleActiveResultSets = True"、Entity Framework とユニバーサル プロバイダーのために必要なためそれらに追加されました。</span><span class="sxs-lookup"><span data-stu-id="bb951-250">These connection strings are not identical to the ones you entered in the **Package/Publish SQL** tab. The setting "MultipleActiveResultSets=True" has been added to them because it's required for the Entity Framework and the Universal Providers.</span></span>

## <a name="installing-sql-server-compact"></a><span data-ttu-id="bb951-251">SQL Server Compact のインストール</span><span class="sxs-lookup"><span data-stu-id="bb951-251">Installing SQL Server Compact</span></span>

<span data-ttu-id="bb951-252">SqlServerCompact NuGet パッケージは、SQL Server Compact データベース エンジン アプリケーションのアセンブリを Contoso 大学を提供します。</span><span class="sxs-lookup"><span data-stu-id="bb951-252">The SqlServerCompact NuGet package provides the SQL Server Compact database engine assemblies for the Contoso University application.</span></span> <span data-ttu-id="bb951-253">ここでは、アプリケーションが Web デプロイ、SQL Server データベースで実行するスクリプトを作成するために、SQL Server Compact データベースを読み取りできる必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-253">But now it is not the application but Web Deploy that must be able to read the SQL Server Compact databases, in order to create scripts to run in the SQL Server databases.</span></span> <span data-ttu-id="bb951-254">Web デプロイを SQL Server Compact データベースの読み取りを有効にする SQL Server Compact 開発用コンピューターに、次のリンクを使用してインストールします。 [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6)です。</span><span class="sxs-lookup"><span data-stu-id="bb951-254">To enable Web Deploy to read SQL Server Compact databases, install SQL Server Compact on the development computer by using the following link: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).</span></span>

## <a name="deploying-to-the-test-environment"></a><span data-ttu-id="bb951-255">テスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="bb951-255">Deploying to the Test Environment</span></span>

<span data-ttu-id="bb951-256">使用するように構成する発行プロファイルを作成する必要がテスト環境に発行するために、**パッケージ化/発行 SQL**データベース発行プロファイルのデータベース設定ではなく公開のタブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-256">In order to publish to the Test environment, you have to create a publish profile that is configured to use the **Package/Publish SQL** tab for database publishing instead of the publish profile database settings.</span></span>

<span data-ttu-id="bb951-257">最初に、既存のテストのプロファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="bb951-257">First, delete the existing Test profile.</span></span>

<span data-ttu-id="bb951-258">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-258">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="bb951-259">選択、**プロファイル**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-259">Select the **Profile** tab.</span></span>

<span data-ttu-id="bb951-260">をクリックして**プロファイルを管理する**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-260">Click **Manage Profiles**.</span></span>

<span data-ttu-id="bb951-261">選択**テスト**、 をクリックして**削除**、クリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-261">Select **Test**, click **Remove**, and then click **Close**.</span></span>

<span data-ttu-id="bb951-262">閉じる、 **Web の発行**ウィザードにこの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="bb951-262">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="bb951-263">次に、新しいテスト プロファイルを作成して、プロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-263">Next, create a new Test profile and use it to publish the project.</span></span>

<span data-ttu-id="bb951-264">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-264">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="bb951-265">選択、**プロファイル**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-265">Select the **Profile** tab.</span></span>

<span data-ttu-id="bb951-266">選択**&lt;新規作成しています.&gt;** ドロップダウン リスト ボックスの一覧し、プロファイル名として"Test"を入力します。</span><span class="sxs-lookup"><span data-stu-id="bb951-266">Select **&lt;New...&gt;** from the drop-down list, and enter "Test" as the profile name.</span></span>

<span data-ttu-id="bb951-267">**サービス URL**ボックスに、入力*localhost*です。</span><span class="sxs-lookup"><span data-stu-id="bb951-267">In the **Service URL** box, enter *localhost*.</span></span>

<span data-ttu-id="bb951-268">**サイト/アプリケーション**ボックスに、入力*既定の Web サイト/ContosoUniversity*です。</span><span class="sxs-lookup"><span data-stu-id="bb951-268">In the **Site/application** box, enter *Default Web Site/ContosoUniversity*.</span></span>

<span data-ttu-id="bb951-269">**送信先 URL**ボックスに、入力`http://localhost/ContosoUniversity/`です。</span><span class="sxs-lookup"><span data-stu-id="bb951-269">In the **Destination URL** box, enter `http://localhost/ContosoUniversity/`.</span></span>

<span data-ttu-id="bb951-270">**[次へ]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="bb951-270">Click **Next**.</span></span>

<span data-ttu-id="bb951-271">**設定** タブに警告すること、**パッケージ化/発行 SQL**  タブが構成済みであり、有効にする をクリックして、新しいデータベース発行機能強化をオーバーライドする機会を提供します。</span><span class="sxs-lookup"><span data-stu-id="bb951-271">The **Settings** tab warns you that the **Package/Publish SQL** tab has been configured, and it gives you an opportunity to override them by clicking enable the new database publishing improvements.</span></span> <span data-ttu-id="bb951-272">この展開のたくを上書きする、**パッケージ化/発行 SQL**タブの設定をクリックするだけのため**次**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-272">For this deployment you don't want to override the **Package/Publish SQL** tab settings, so just click **Next**.</span></span>

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

<span data-ttu-id="bb951-274">メッセージ、**プレビュー**  タブには、ことを示します**パブリッシュするデータベースが選択されていない**、のみつまり、データベースの発行が、発行プロファイルで構成されていないことができます。</span><span class="sxs-lookup"><span data-stu-id="bb951-274">A message on the **Preview** tab indicates that **No databases are selected to publish**, but this only means that database publishing is not configured in the publish profile.</span></span>

<span data-ttu-id="bb951-275">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="bb951-275">Click **Publish**.</span></span>

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

<span data-ttu-id="bb951-277">Visual Studio では、アプリケーションを展開し、テスト環境で、サイトのホーム ページをブラウザーが開きます。</span><span class="sxs-lookup"><span data-stu-id="bb951-277">Visual Studio deploys the application and opens the browser to the home page of the site in the test environment.</span></span> <span data-ttu-id="bb951-278">先ほど見たを同じデータが表示されるを表示するインストラクター page を実行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-278">Run the Instructors page to see that it displays the same data that you saw earlier.</span></span> <span data-ttu-id="bb951-279">実行、**受講者を追加** ページで新しい学生を追加して、新しい学生を表示、**受講者**ページ。</span><span class="sxs-lookup"><span data-stu-id="bb951-279">Run the **Add Students** page, add a new student, and then view the new student in the **Students** page.</span></span> <span data-ttu-id="bb951-280">これは、データベースを更新することを確認します。</span><span class="sxs-lookup"><span data-stu-id="bb951-280">This verifies that the you can update the database.</span></span> <span data-ttu-id="bb951-281">選択、**更新クレジット**(にログインする必要があります) ページ メンバーシップ データベースが配置されたされへのアクセス権があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bb951-281">Select the **Update Credits** page (you'll need to log in) to verify that the membership database was deployed and you have access to it.</span></span>

## <a name="creating-a-sql-server-database-for-the-production-environment"></a><span data-ttu-id="bb951-282">運用環境の SQL Server データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-282">Creating a SQL Server Database for the Production Environment</span></span>

<span data-ttu-id="bb951-283">をテスト環境にデプロイした実稼働環境にデプロイをセットアップする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="bb951-283">Now that you've deployed to the test environment, you're ready to set up deployment to production.</span></span> <span data-ttu-id="bb951-284">同様、テスト環境用に展開するデータベースを作成することで開始します。</span><span class="sxs-lookup"><span data-stu-id="bb951-284">You begin as you did for the test environment, by creating a database to deploy to.</span></span> <span data-ttu-id="bb951-285">概要から学習したように、Cytanium Lite のホスティング プランのみで、単一の SQL Server データベースためはより多くのデータベースを 1 つしかない 2 つを設定にできます。</span><span class="sxs-lookup"><span data-stu-id="bb951-285">As you recall from the Overview, the Cytanium Lite hosting plan only allows a single SQL Server database, so you will set up only one database, not two.</span></span> <span data-ttu-id="bb951-286">すべてのテーブルと、メンバーシップおよび学校 SQL Server Compact データベースからのデータは、実稼働環境での 1 つの SQL Server データベースに展開されます。</span><span class="sxs-lookup"><span data-stu-id="bb951-286">All of the tables and data from the membership and School SQL Server Compact databases will be deployed into one SQL Server database in production.</span></span>

<span data-ttu-id="bb951-287">Cytanium コントロール パネルに移動して[http://panel.cytanium.com](http://panel.cytanium.com)です。マウスで**データベース** をクリックし、 **SQL Server 2008**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-287">Go to the Cytanium control panel at [http://panel.cytanium.com](http://panel.cytanium.com). Hold the mouse over **Databases** and then click **SQL Server 2008**.</span></span>

<span data-ttu-id="bb951-288">[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="bb951-288">[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)</span></span>

<span data-ttu-id="bb951-289">**SQL Server 2008** ] ページで [ **Create Database**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-289">In the **SQL Server 2008** page, click **Create Database**.</span></span>

<span data-ttu-id="bb951-290">[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="bb951-290">[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)</span></span>

<span data-ttu-id="bb951-291">データベース「学校」の名前をクリックして**保存**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-291">Name the database "School" and click **Save**.</span></span> <span data-ttu-id="bb951-292">(ページに自動的にプレフィックスを追加"contosou"、有効な名前は"contosouSchool"になります。)</span><span class="sxs-lookup"><span data-stu-id="bb951-292">(The page automatically adds the prefix "contosou", so the effective name will be "contosouSchool".)</span></span>

<span data-ttu-id="bb951-293">[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="bb951-293">[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)</span></span>

<span data-ttu-id="bb951-294">同じページで、をクリックして**Create User**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-294">On the same page, click **Create User**.</span></span> <span data-ttu-id="bb951-295">Cytanium のサーバーではなく、Windows 統合セキュリティを使用して、および、データベースを開くアプリケーション プール id をできるようにすることで、データベースを開く権限が付与されているユーザーを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-295">On Cytanium's servers, rather than using integrated Windows security and letting the application pool identity open your database, you'll create a user that has authority to open your database.</span></span> <span data-ttu-id="bb951-296">ユーザーの資格情報にも、実稼働環境での接続文字列を追加する*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bb951-296">You'll add the user's credentials to the connection strings that go in the production *Web.config* file.</span></span> <span data-ttu-id="bb951-297">この手順では、これらの資格情報を作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-297">In this step you create those credentials.</span></span>

<span data-ttu-id="bb951-298">[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="bb951-298">[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)</span></span>

<span data-ttu-id="bb951-299">必要なフィールドに入力、 **SQL ユーザー プロパティ**ページ。</span><span class="sxs-lookup"><span data-stu-id="bb951-299">Fill in the required fields in the **SQL User Properties** page:</span></span>

- <span data-ttu-id="bb951-300">名前として"ContosoUniversityUser"を入力します。</span><span class="sxs-lookup"><span data-stu-id="bb951-300">Enter "ContosoUniversityUser" as the name.</span></span>
- <span data-ttu-id="bb951-301">パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="bb951-301">Enter a password.</span></span>
- <span data-ttu-id="bb951-302">選択**contosouSchool**既定のデータベースとして。</span><span class="sxs-lookup"><span data-stu-id="bb951-302">Select **contosouSchool** as the default database.</span></span>
- <span data-ttu-id="bb951-303">選択、 **contosouSchool**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="bb951-303">Select the **contosouSchool** check box.</span></span>

<span data-ttu-id="bb951-304">[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="bb951-304">[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)</span></span>

## <a name="configuring-database-deployment-for-the-production-environment"></a><span data-ttu-id="bb951-305">運用環境のデータベースの配置を構成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-305">Configuring Database Deployment for the Production Environment</span></span>

<span data-ttu-id="bb951-306">これで、データベースでのデプロイ設定を設定する準備ができたら、**パッケージ化/発行 SQL**  タブで、テスト環境の前に行ったようにします。</span><span class="sxs-lookup"><span data-stu-id="bb951-306">Now you're ready to set up database deployment settings in the **Package/Publish SQL** tab, as you did earlier for the test environment.</span></span>

<span data-ttu-id="bb951-307">開く、**プロジェクトのプロパティ**ウィンドウで、**パッケージ化/発行 SQL**  タブを確認して**アクティブ (リリース)**または**リリース**は選択された状態で、**構成**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="bb951-307">Open the **Project Properties** window, select the **Package/Publish SQL** tab, and make sure that **Active (Release)** or **Release** is selected in the **Configuration** drop-down list.</span></span>

<span data-ttu-id="bb951-308">各データベースのデプロイ設定を構成するときに運用環境とテスト環境の場合の重要な違いが、接続文字列を構成する方法です。</span><span class="sxs-lookup"><span data-stu-id="bb951-308">When you configure deployment settings for each database, the key difference between what you do for production and test environments is in how you configure connection strings.</span></span> <span data-ttu-id="bb951-309">テスト環境の別のインストール先データベースの接続文字列を入力したが、運用環境のコピー先の接続文字列は、両方のデータベースの同じです。</span><span class="sxs-lookup"><span data-stu-id="bb951-309">For the test environment you entered different destination database connection strings, but for the production environment the destination connection string will be the same for both databases.</span></span> <span data-ttu-id="bb951-310">実稼働環境で 1 つのデータベースを両方のデータベースを展開しているためにです。</span><span class="sxs-lookup"><span data-stu-id="bb951-310">This is because you are deploying both databases to one database in production.</span></span>

### <a name="configuring-deployment-settings-for-the-membership-database"></a><span data-ttu-id="bb951-311">メンバーシップ データベースのデプロイ設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-311">Configuring Deployment Settings for the Membership Database</span></span>

<span data-ttu-id="bb951-312">メンバーシップ データベースに適用される設定を構成するには、選択、 **DefaultConnection 展開**行が、**データベース エントリ**テーブル。</span><span class="sxs-lookup"><span data-stu-id="bb951-312">To configure settings that apply to the membership database, select the **DefaultConnection-Deployment** row in the **Database Entries** table.</span></span>

<span data-ttu-id="bb951-313">**転送先データベースへの接続文字列**、先ほど作成した新しい運用 SQL Server データベースを指す接続文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="bb951-313">In **Connection string for destination database**, enter a connection string that points to the new production SQL Server database that you just created.</span></span> <span data-ttu-id="bb951-314">ようこそ電子メールから、接続文字列を取得できます。</span><span class="sxs-lookup"><span data-stu-id="bb951-314">You can get the connection string from your welcome email.</span></span> <span data-ttu-id="bb951-315">電子メールの関連する部分には、次のサンプルの接続文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bb951-315">The relevant part of the email contains the following sample connection string:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

<span data-ttu-id="bb951-316">3 つの変数を交換した後、必要な接続文字列は、この例のようになります。</span><span class="sxs-lookup"><span data-stu-id="bb951-316">After you replace the three variables, the connection string you need looks like this example:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

<span data-ttu-id="bb951-317">コピーして貼り付けるには、この接続文字列**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-317">Copy and paste this connection string into **Connection string for destination database** in the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="bb951-318">確認して**または既存のデータベースからスキーマのデータをプル**がオンのままとする**データベース スクリプト オプション**まだ**スキーマとデータ**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-318">Make sure that **Pull data and/or schema from an existing database** is still selected, and that **Database scripting options** is still **Schema and Data**.</span></span>

<span data-ttu-id="bb951-319">**データベース スクリプト**ボックスで、Grant.sql スクリプトの横にあるチェック ボックスをオフにします。</span><span class="sxs-lookup"><span data-stu-id="bb951-319">In the **Database Scripts** box, clear the check box next to the Grant.sql script.</span></span>

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a><span data-ttu-id="bb951-321">School データベースのデプロイ設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-321">Configuring Deployment Settings for the School Database</span></span>

<span data-ttu-id="bb951-322">次に、選択、 **SchoolContext 展開**行が、**データベース エントリ**School データベースの設定を構成するためにテーブルです。</span><span class="sxs-lookup"><span data-stu-id="bb951-322">Next, select the **SchoolContext-Deployment** row in the **Database Entries** table in order to configure the School database settings.</span></span>

<span data-ttu-id="bb951-323">同じ接続文字列をコピー**転送先データベースへの接続文字列**をメンバーシップ データベースにそのフィールドにコピーします。</span><span class="sxs-lookup"><span data-stu-id="bb951-323">Copy the same connection string into **Connection string for destination database** that you copied into that field for the membership database.</span></span>

<span data-ttu-id="bb951-324">確認して**または既存のデータベースからスキーマのデータをプル**がオンのままとする**データベース スクリプト オプション**まだ**スキーマとデータ**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-324">Make sure that **Pull data and/or schema from an existing database** is still selected, and that **Database scripting options** is still **Schema and Data**.</span></span>

<span data-ttu-id="bb951-325">**データベース スクリプト**ボックスで、Grant.sql スクリプトの横にあるチェック ボックスをオフにします。</span><span class="sxs-lookup"><span data-stu-id="bb951-325">In the **Database Scripts** box, clear the check box next to the Grant.sql script.</span></span>

<span data-ttu-id="bb951-326">変更を保存、**パッケージ化/発行 SQL**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-326">Save the changes to the **Package/Publish SQL** tab.</span></span>

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a><span data-ttu-id="bb951-327">Web.Config の設定を実稼働データベースへの接続文字列の変換します。</span><span class="sxs-lookup"><span data-stu-id="bb951-327">Setting Up Web.Config Transforms for the Connection Strings to Production Databases</span></span>

<span data-ttu-id="bb951-328">次を設定します*Web.config*変換できるように、接続文字列、デプロイされている*Web.config*新しい運用データベースを指すファイル。</span><span class="sxs-lookup"><span data-stu-id="bb951-328">Next, you'll set up *Web.config* transformations so that the connection strings in the deployed *Web.config* file to point to the new production database.</span></span> <span data-ttu-id="bb951-329">接続文字列で入力した、**パッケージ化/発行 SQL** Web を使用する展開のタブは、同じの追加を除く、使用するアプリケーションに必要な 1 つとして、`MultipleResultSets`オプション。</span><span class="sxs-lookup"><span data-stu-id="bb951-329">The connection string that you entered on the **Package/Publish SQL** tab for Web Deploy to use is the same as the one the application needs to use, except for the addition of the `MultipleResultSets` option.</span></span>

<span data-ttu-id="bb951-330">開いている*Web.Production.config*と置換、`connectionStrings`を持つ要素を`connectionStrings`次の例のような要素です。</span><span class="sxs-lookup"><span data-stu-id="bb951-330">Open *Web.Production.config* and replace the `connectionStrings` element with a `connectionStrings` element that looks like the following example.</span></span> <span data-ttu-id="bb951-331">(コピーのみの`connectionStrings`要素では、周囲タグをコンテキストを示すために用意されていません)。</span><span class="sxs-lookup"><span data-stu-id="bb951-331">(Only copy the `connectionStrings` element, not the surrounding tags that are provided to show the context.)</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

<span data-ttu-id="bb951-332">場合があります内の接続文字列は常に暗号化するよう指示するためのアドバイスを参照してください、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bb951-332">You sometimes see advice that tells you to always encrypt connection strings in the *Web.config* file.</span></span> <span data-ttu-id="bb951-333">適切なは、会社のネットワーク上のサーバーに配置する場合があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-333">This might be appropriate if you were deploying to servers on your own company's network.</span></span> <span data-ttu-id="bb951-334">共有ホスティング環境に展開する場合は、ただし、ホスティング プロバイダーの場合のセキュリティ プラクティスを信頼しているし、必要なまたは接続文字列を暗号化することは実用的ではありません。</span><span class="sxs-lookup"><span data-stu-id="bb951-334">When you are deploying to a shared hosting environment, though, you're trusting the security practices of the hosting provider, and it's not necessary or practical to encrypt the connection strings.</span></span>

## <a name="deploying-to-the-production-environment"></a><span data-ttu-id="bb951-335">実稼働環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="bb951-335">Deploying to the Production Environment</span></span>

<span data-ttu-id="bb951-336">これで、実稼働環境に展開する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="bb951-336">Now you're ready to deploy to production.</span></span> <span data-ttu-id="bb951-337">Web Deploy は、プロジェクトの SQL Server Compact データベースを読み取り*アプリ\_データ*フォルダーとすべてのテーブルと、実稼働 SQL Server データベース内のデータを再作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-337">Web Deploy will read the SQL Server Compact databases in your project's *App\_Data* folder and re-create all of their tables and data in the production SQL Server database.</span></span> <span data-ttu-id="bb951-338">使用して発行するために、**パッケージ化/発行 Web**  タブの設定、実稼働環境用の新しい発行プロファイルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-338">In order to publish by using the **Package/Publish Web** tab settings, you have to create a new publish profile for production.</span></span>

<span data-ttu-id="bb951-339">最初に、テスト プロファイルを前述した方法では、既存の実稼働プロファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="bb951-339">First, delete the existing Production profile as you did the Test profile earlier.</span></span>

<span data-ttu-id="bb951-340">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-340">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="bb951-341">選択、**プロファイル**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-341">Select the **Profile** tab.</span></span>

<span data-ttu-id="bb951-342">をクリックして**プロファイルを管理する**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-342">Click **Manage Profiles**.</span></span>

<span data-ttu-id="bb951-343">選択**運用**、 をクリックして**削除**、順にクリック**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-343">Select **Production**, click **Remove**, and then click **Close**.</span></span>

<span data-ttu-id="bb951-344">閉じる、 **Web の発行**ウィザードにこの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="bb951-344">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="bb951-345">次に、実稼働環境に、新しいプロファイルを作成して、プロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-345">Next, create a new Production profile and use it to publish the project.</span></span>

<span data-ttu-id="bb951-346">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-346">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="bb951-347">選択、**プロファイル**タブです。</span><span class="sxs-lookup"><span data-stu-id="bb951-347">Select the **Profile** tab.</span></span>

<span data-ttu-id="bb951-348">をクリックして**インポート**、以前にダウンロードした .publishsettings ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="bb951-348">Click **Import**, and select the .publishsettings file that you downloaded earlier.</span></span>

<span data-ttu-id="bb951-349">**接続** タブで、変更、**送信先 URL**正しいの一時的な url では、この例では http://contosouniversity.com.vserver01.cytanium.com します。</span><span class="sxs-lookup"><span data-stu-id="bb951-349">On the **Connection** tab, change the **Destination URL** to the correct temporary URL, which in this example is http://contosouniversity.com.vserver01.cytanium.com.</span></span>

<span data-ttu-id="bb951-350">実稼働環境に、プロファイルの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="bb951-350">Rename the profile to Production.</span></span> <span data-ttu-id="bb951-351">(選択、**プロファイル** タブでをクリックし、**プロファイルの管理**を行うには)。</span><span class="sxs-lookup"><span data-stu-id="bb951-351">(Select the **Profile** tab and click **Manage Profiles** to do that).</span></span>

<span data-ttu-id="bb951-352">閉じる、 **Web の発行**変更を保存するウィザード。</span><span class="sxs-lookup"><span data-stu-id="bb951-352">Close the **Publish Web** wizard to save your changes.</span></span>

<span data-ttu-id="bb951-353">実稼働環境でこれで、データベースが更新されている実際のアプリケーションでは手順を実行する 2 つ追加今すぐ発行する前に。</span><span class="sxs-lookup"><span data-stu-id="bb951-353">In a real application in which the database was being updated in production, you would do two additional steps now before you publish:</span></span>

1. <span data-ttu-id="bb951-354">アップロード*アプリ\_offline.htm*のように、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="bb951-354">Upload *app\_offline.htm*, as shown in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span>
2. <span data-ttu-id="bb951-355">使用して、**ファイル マネージャー**をコピーする Cytanium コントロール パネル の機能、 *aspnet Prod.sdf*と*学校 Prod.sdf*ファイルを実稼働サイトから、 *アプリ\_データ*ContosoUniversity プロジェクトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="bb951-355">Use the **File Manager** feature of the Cytanium control panel to copy the *aspnet-Prod.sdf* and *School-Prod.sdf* files from the production site to the *App\_Data* folder of the ContosoUniversity project.</span></span> <span data-ttu-id="bb951-356">これにより、新しい SQL Server データベースに配置するデータには、実稼働 web サイトによって行われた最新の更新プログラムが含まれます。</span><span class="sxs-lookup"><span data-stu-id="bb951-356">This ensures that the data you're deploying to the new SQL Server database includes the latest updates made by your production website.</span></span>

<span data-ttu-id="bb951-357">**Web 1 つをクリックして 発行**ツールバー、ことを確認して、**運用**プロファイルを選択して、をクリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-357">In the **Web One Click Publish** toolbar, make sure that the **Production** profile is selected, and then click **Publish**.</span></span>

<span data-ttu-id="bb951-358">アップロードした場合*アプリ\_offline.htm* 、パブリッシュする前に、使用する必要が、**ファイル マネージャー**を削除する Cytanium コントロール パネルの ユーティリティ*アプリ\_オフライン*。htm ファイルをテストする前にします。</span><span class="sxs-lookup"><span data-stu-id="bb951-358">If you uploaded *app\_offline.htm* before publishing, you have to use the **File Manager** utility in the Cytanium control panel to delete *app\_offline.*htm before you test.</span></span> <span data-ttu-id="bb951-359">削除することも、同時に、 *.sdf*ファイルから、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="bb951-359">You can also at the same time delete the *.sdf* files from the *App\_Data* folder.</span></span>

<span data-ttu-id="bb951-360">ブラウザーを開き、アプリケーションをテストするには、テスト環境に配置した後で実行したのと同様に、パブリックのサイトの URL に移動できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="bb951-360">You can now open a browser and go to the URL of your public site to test the application the same way you did after deploying to the test environment.</span></span>

## <a name="switching-to-sql-server-express-localdb-in-development"></a><span data-ttu-id="bb951-361">開発中の SQL Server Express LocalDB への切り替え</span><span class="sxs-lookup"><span data-stu-id="bb951-361">Switching to SQL Server Express LocalDB in Development</span></span>

<span data-ttu-id="bb951-362">概要」に説明したとおりには、開発、テストや実稼働環境を使用することで、同じデータベース エンジンを使用する通常お勧めします。</span><span class="sxs-lookup"><span data-stu-id="bb951-362">As was explained in the Overview, it's generally best to use the same database engine in development that you use in test and production.</span></span> <span data-ttu-id="bb951-363">(開発における SQL Server Express を使用する利点は、データベースが動作する、同じ開発、テスト、実稼働環境では注意してください)。このセクションでは、Visual Studio からアプリケーションを実行するときに、SQL Server Express LocalDB を使用する ContosoUniversity プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="bb951-363">(Remember that the advantage to using SQL Server Express in development is that the database will work the same in your development, test, and production environments.) In this section you'll set up the ContosoUniversity project to use SQL Server Express LocalDB when you run the application from Visual Studio.</span></span>

<span data-ttu-id="bb951-364">この移行を実行する最も簡単な方法として、Code First を使用でき、メンバーシップ システムの両方の新しい開発データベースを作成するという方法があります。</span><span class="sxs-lookup"><span data-stu-id="bb951-364">The simplest way to perform this migration is to let Code First and the membership system create both new development databases for you.</span></span> <span data-ttu-id="bb951-365">このメソッドを使用して移行するには、3 つの手順が必要です。</span><span class="sxs-lookup"><span data-stu-id="bb951-365">Using this method to migrate requires three steps:</span></span>

1. <span data-ttu-id="bb951-366">新しい SQL Express LocalDB データベースを指定する接続文字列を変更します。</span><span class="sxs-lookup"><span data-stu-id="bb951-366">Change the connection strings to specify new SQL Express LocalDB databases.</span></span>
2. <span data-ttu-id="bb951-367">管理者ユーザーを作成する Web サイト管理ツールを実行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-367">Run the Web Site Administration Tool to create an administrator user.</span></span> <span data-ttu-id="bb951-368">これには、メンバーシップ データベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="bb951-368">This creates the membership database.</span></span>
3. <span data-ttu-id="bb951-369">作成し、アプリケーション データベースのシードには、Code First Migrations データベースの更新コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="bb951-369">Use the Code First Migrations update-database command to create and seed the application database.</span></span>

### <a name="updating-connection-strings-in-the-webconfig-file"></a><span data-ttu-id="bb951-370">Web.config ファイル内の接続文字列を更新</span><span class="sxs-lookup"><span data-stu-id="bb951-370">Updating Connection Strings in the Web.config file</span></span>

<span data-ttu-id="bb951-371">開く、 *Web.config*ファイルし、置換、`connectionStrings`要素を次のコード。</span><span class="sxs-lookup"><span data-stu-id="bb951-371">Open the *Web.config* file and replace the `connectionStrings` element with the following code:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a><span data-ttu-id="bb951-372">メンバーシップ データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-372">Creating the Membership Database</span></span>

<span data-ttu-id="bb951-373">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを選択し、クリックして**ASP.NET 構成**で、**プロジェクト**メニュー。</span><span class="sxs-lookup"><span data-stu-id="bb951-373">In **Solution Explorer**, select the ContosoUniversity project, and then click **ASP.NET Configuration** in the **Project** menu.</span></span>

<span data-ttu-id="bb951-374">[セキュリティ] タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="bb951-374">Select the Security tab.</span></span>

<span data-ttu-id="bb951-375">をクリックして**作成またはロールの管理**、し、作成、**管理者**ロール。</span><span class="sxs-lookup"><span data-stu-id="bb951-375">Click **Create or Manage Roles**, and then create an **Administrator** role.</span></span>

<span data-ttu-id="bb951-376">[セキュリティ] タブに戻ります。</span><span class="sxs-lookup"><span data-stu-id="bb951-376">Return to the Security tab.</span></span>

<span data-ttu-id="bb951-377">をクリックして**Create user**、クリックして、**管理者**チェック ボックスをオンし、管理者をという名前のユーザーを作成</span><span class="sxs-lookup"><span data-stu-id="bb951-377">Click **Create user**, and then select the **Administrator** check box and create a user named admin.</span></span>

<span data-ttu-id="bb951-378">閉じる、 **Web サイト管理ツール**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-378">Close the **Web Site Administration Tool**.</span></span>

### <a name="creating-the-school-database"></a><span data-ttu-id="bb951-379">School データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb951-379">Creating the School Database</span></span>

<span data-ttu-id="bb951-380">パッケージ マネージャー コンソール ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="bb951-380">Open the Package Manager Console window.</span></span>

<span data-ttu-id="bb951-381">**既定のプロジェクト**ドロップダウン リストで、ContosoUniversity.DAL プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="bb951-381">In the **Default project** drop-down list, select the ContosoUniversity.DAL project.</span></span>

<span data-ttu-id="bb951-382">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="bb951-382">Enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

<span data-ttu-id="bb951-383">Code First Migrations がデータベースを作成し、AddBirthDate 移行を適用する初期の移行を適用し、シード メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-383">Code First Migrations applies the Initial migration that creates the database and then applies the AddBirthDate migration, then it runs the Seed method.</span></span>

<span data-ttu-id="bb951-384">コントロール f5 キーを押して、サイトを実行します。</span><span class="sxs-lookup"><span data-stu-id="bb951-384">Run the site by pressing Control-F5.</span></span> <span data-ttu-id="bb951-385">テストおよび運用環境の場合と、実行、**受講者を追加** ページで新しい学生を追加して、新しい学生を表示、**受講者**ページ。</span><span class="sxs-lookup"><span data-stu-id="bb951-385">As you did for the test and production environments, run the **Add Students** page, add a new student, and then view the new student in the **Students** page.</span></span> <span data-ttu-id="bb951-386">これは、School データベースが作成され、初期化することと読み取り書き込みアクセス権を確認します。</span><span class="sxs-lookup"><span data-stu-id="bb951-386">This verifies that the School database was created and initialized and that you have read and write access to it.</span></span>

<span data-ttu-id="bb951-387">選択、**更新クレジット** ページで、メンバーシップ データベースが展開されたことへのアクセス権があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="bb951-387">Select the **Update Credits** page and log in to verify that the membership database was deployed and that you have access to it.</span></span> <span data-ttu-id="bb951-388">管理者アカウントを作成し、[ユーザー アカウントを移行しなかった場合、**更新クレジット**] ページの動作を確認するためです。</span><span class="sxs-lookup"><span data-stu-id="bb951-388">If you did not migrate your user accounts, create an administrator account and then select the **Update Credits** page to verify that it works.</span></span>

## <a name="cleaning-up-sql-server-compact-files"></a><span data-ttu-id="bb951-389">SQL Server Compact ファイルのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="bb951-389">Cleaning Up SQL Server Compact Files</span></span>

<span data-ttu-id="bb951-390">ファイルと SQL Server Compact をサポートするために含まれている NuGet パッケージが不要になった。</span><span class="sxs-lookup"><span data-stu-id="bb951-390">You no longer need files and NuGet packages that were included to support SQL Server Compact.</span></span> <span data-ttu-id="bb951-391">場合 (この手順は不要)、不要なファイルとの参照をクリーンアップすることができます。</span><span class="sxs-lookup"><span data-stu-id="bb951-391">If you want (this step is not required), you can clean up unneeded files and references.</span></span>

<span data-ttu-id="bb951-392">**ソリューション エクスプ ローラー**、削除、 *.sdf*ファイルから、*アプリ\_データ*フォルダーおよび*amd64*と*x86*フォルダーから、 *bin*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="bb951-392">In **Solution Explorer**, delete the *.sdf* files from the *App\_Data* folder and the *amd64* and *x86* folders from the *bin* folder.</span></span>

<span data-ttu-id="bb951-393">**ソリューション エクスプ ローラー**(いない 1 つのプロジェクトの)、ソリューションを右クリックし、クリックして**Manage NuGet Packages for Solution**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-393">In **Solution Explorer**, right-click the solution (not one of the projects), and then click **Manage NuGet Packages for Solution**.</span></span>

<span data-ttu-id="bb951-394">左側のウィンドウで、 **NuGet パッケージの管理**ダイアログ ボックスで、**インストールされているパッケージ**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-394">In the left pane of the **Manage NuGet Packages** dialog box, select **Installed packages**.</span></span>

<span data-ttu-id="bb951-395">選択、 **EntityFramework.SqlServerCompact**パッケージ化し、をクリックして**管理**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-395">Select the **EntityFramework.SqlServerCompact** package and click **Manage**.</span></span>

<span data-ttu-id="bb951-396">**プロジェクトの選択**ダイアログ ボックスで、両方のプロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="bb951-396">In the **Select Projects** dialog box, both projects are selected.</span></span> <span data-ttu-id="bb951-397">両方のプロジェクトでパッケージをアンインストールするには、両方のチェック ボックスをオフにし、クリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="bb951-397">To uninstall the package in both projects, clear both check boxes, then click **OK**.</span></span>

<span data-ttu-id="bb951-398">依存パッケージをアンインストールするかどうかたずねるダイアログ ボックス、いいえ</span><span class="sxs-lookup"><span data-stu-id="bb951-398">In the dialog box that asks if you want to uninstall the dependent packages also, click No.</span></span> <span data-ttu-id="bb951-399">これらの 1 つは、保持する必要のある Entity Framework パッケージです。</span><span class="sxs-lookup"><span data-stu-id="bb951-399">One of these is the Entity Framework package that you have to keep.</span></span>

<span data-ttu-id="bb951-400">アンインストールする同じ手順に従って、 **SqlServerCompact**パッケージです。</span><span class="sxs-lookup"><span data-stu-id="bb951-400">Follow the same procedure to uninstall the **SqlServerCompact** package.</span></span> <span data-ttu-id="bb951-401">(この順序で、パッケージをアンインストールする必要があります、 **EntityFramework.SqlServerCompact**パッケージによって異なります、 **SqlServerCompact**パッケージです)。</span><span class="sxs-lookup"><span data-stu-id="bb951-401">(The packages must be uninstalled in this order because the **EntityFramework.SqlServerCompact** package depends on the **SqlServerCompact** package.)</span></span>

<span data-ttu-id="bb951-402">SQL Server Express と完全な SQL Server に正常に移行したようになりました。</span><span class="sxs-lookup"><span data-stu-id="bb951-402">You have now successfully migrated to SQL Server Express and full SQL Server.</span></span> <span data-ttu-id="bb951-403">すれば、別のデータベースの変更、およびするチュートリアルでは、次に、テストおよび実稼働データベースを SQL Server Express と完全な SQL Server を使用すると、データベースに対する変更を配置する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb951-403">In the next tutorial you'll make another database change, and you'll see how to deploy database changes when your test and production databases use SQL Server Express and full SQL Server.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bb951-404">[前へ](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[次へ](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="bb951-404">[Previous](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[Next](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)</span></span>
