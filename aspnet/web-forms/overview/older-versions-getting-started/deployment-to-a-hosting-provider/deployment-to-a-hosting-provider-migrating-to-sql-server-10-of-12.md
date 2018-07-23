---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: SQL Server/10/12 への移行 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: bc0bca18d2f6e4cdbb527ab75952e9a50eb49b20
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827363"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a><span data-ttu-id="2c253-103">SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: SQL Server/10/12 への移行</span><span class="sxs-lookup"><span data-stu-id="2c253-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Migrating to SQL Server - 10 of 12</span></span>
====================
<span data-ttu-id="2c253-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2c253-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="2c253-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2c253-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="2c253-106">この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2c253-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="2c253-107">Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="2c253-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="2c253-108">シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。</span><span class="sxs-lookup"><span data-stu-id="2c253-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="2c253-109">Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="2c253-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="2c253-110">概要</span><span class="sxs-lookup"><span data-stu-id="2c253-110">Overview</span></span>

<span data-ttu-id="2c253-111">このチュートリアルでは、SQL Server Compact から SQL Server に移行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2c253-111">This tutorial shows you how to migrate from SQL Server Compact to SQL Server.</span></span> <span data-ttu-id="2c253-112">そう理由の 1 つは、SQL Server Compact をサポートしていないストアド プロシージャ、トリガー、ビュー、またはレプリケーションなどの SQL Server 機能の活用することです。</span><span class="sxs-lookup"><span data-stu-id="2c253-112">One reason you might want to do that is to take advantage of SQL Server features that SQL Server Compact does not support, such as stored procedures, triggers, views, or replication.</span></span> <span data-ttu-id="2c253-113">SQL Server Compact および SQL Server 間の違いの詳細については、次を参照してください。、[を展開する SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="2c253-113">For more information about the differences between SQL Server Compact and SQL Server, see the [Deploying SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

### <a name="sql-server-express-versus-full-sql-server-for-development"></a><span data-ttu-id="2c253-114">開発用の完全な SQL Server と SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="2c253-114">SQL Server Express versus full SQL Server for Development</span></span>

<span data-ttu-id="2c253-115">SQL Server にアップグレードする決めたら、開発およびテスト環境内で SQL Server または SQL Server Express を使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-115">Once you've decided to upgrade to SQL Server, you might want to use SQL Server or SQL Server Express in your development and test environments.</span></span> <span data-ttu-id="2c253-116">データベース エンジンの機能とツールのサポートでの違い、SQL Server Compact とその他のバージョンの SQL Server プロバイダーの実装で違いがあります。</span><span class="sxs-lookup"><span data-stu-id="2c253-116">In addition to the differences in tool support and in database engine features, there are differences in provider implementations between SQL Server Compact and other versions of SQL Server.</span></span> <span data-ttu-id="2c253-117">これらの違いには、同じコードを異なる結果を生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-117">These differences can cause the same code to generate different results.</span></span> <span data-ttu-id="2c253-118">そのため、SQL Server Compact、開発用データベースとして保持する場合は、必ず十分なテストは、サイトで SQL Server または SQL Server Express を運用環境には、各デプロイの前に、テスト環境でします。</span><span class="sxs-lookup"><span data-stu-id="2c253-118">Therefore, if you decide to keep SQL Server Compact as your development database, you should thoroughly test your site in SQL Server or SQL Server Express in a test environment before each deployment to production.</span></span>

<span data-ttu-id="2c253-119">異なり、SQL Server Compact は SQL Server Express を同じデータベース エンジンでは基本的には、および完全な SQL Server と同じ .NET プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="2c253-119">Unlike SQL Server Compact, SQL Server Express is essentially the same database engine and uses the same .NET provider as full SQL Server.</span></span> <span data-ttu-id="2c253-120">SQL Server Express を使ってテストするときに、SQL Server では、同じ結果を取得することを確信できます。</span><span class="sxs-lookup"><span data-stu-id="2c253-120">When you test with SQL Server Express, you can be confident of getting the same results as you will with SQL Server.</span></span> <span data-ttu-id="2c253-121">SQL Server express と SQL Server に使用できるほとんどのデータベースと同じツールを使用することができます (注目すべき例外される[SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx))、SQL Server のストアド プロシージャ、ビュー、トリガーなどの他の機能をサポートしていますおよびレプリケーション。</span><span class="sxs-lookup"><span data-stu-id="2c253-121">You can use most of the same database tools with SQL Server Express that you can use with SQL Server (a notable exception being [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), and it supports other features of SQL Server like stored procedures, views, triggers, and replication.</span></span> <span data-ttu-id="2c253-122">(通常、ただし、運用環境の web サイトで完全な SQL Server を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-122">(You typically have to use full SQL Server in a production website, however.</span></span> <span data-ttu-id="2c253-123">SQL Server Express は、共有ホスティング環境で実行できますが、向けに設計されていないと多くのホスティング プロバイダーはサポートされません。)</span><span class="sxs-lookup"><span data-stu-id="2c253-123">SQL Server Express can run in a shared hosting environment, but it was not designed for that, and many hosting providers do not support it.)</span></span>

<span data-ttu-id="2c253-124">Visual Studio 2012 を使用している場合は、通常、開発環境の SQL Server Express LocalDB を選択する Visual Studio では既定でインストールされている内容であるためです。</span><span class="sxs-lookup"><span data-stu-id="2c253-124">If you are using Visual Studio 2012, you typically choose SQL Server Express LocalDB for your development environment because that is what is installed by default with Visual Studio.</span></span> <span data-ttu-id="2c253-125">ただし、LocalDB では機能しません、IIS、テスト環境は、SQL Server または SQL Server Express のいずれかを使用する必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="2c253-125">However, LocalDB does not work in IIS, so for your test environment you have to use either SQL Server or SQL Server Express.</span></span>

### <a name="combining-databases-versus-keeping-them-separate"></a><span data-ttu-id="2c253-126">データベースを個別に維持すると結合</span><span class="sxs-lookup"><span data-stu-id="2c253-126">Combining Databases versus Keeping Them Separate</span></span>

<span data-ttu-id="2c253-127">Contoso University アプリケーションが 2 つの SQL Server Compact データベース: メンバーシップ データベース (*aspnet.sdf*) とアプリケーション データベース (*School.sdf*)。</span><span class="sxs-lookup"><span data-stu-id="2c253-127">The Contoso University application has two SQL Server Compact databases: the membership database (*aspnet.sdf*) and the application database (*School.sdf*).</span></span> <span data-ttu-id="2c253-128">移行する場合、2 つの異なるデータベースまたは単一のデータベースにこれらのデータベースを移行できます。</span><span class="sxs-lookup"><span data-stu-id="2c253-128">When you migrate, you can migrate these databases to two separate databases or to a single database.</span></span> <span data-ttu-id="2c253-129">アプリケーションのデータベースと、メンバーシップ データベース間のデータベースの結合を容易にするため、それらを結合する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-129">You might want to combine them in order to facilitate database joins between your application database and your membership database.</span></span> <span data-ttu-id="2c253-130">ホスティング プランには、それらを結合するための提供も可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-130">Your hosting plan might also provide a reason to combine them.</span></span> <span data-ttu-id="2c253-131">など、ホスティング プロバイダーは、複数のデータベースの詳細は課金可能性があります。 または 1 つ以上のデータベースをも許可しない場合があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-131">For example, the hosting provider might charge more for multiple databases or might not even allow more than one database.</span></span> <span data-ttu-id="2c253-132">Cytanium Lite でこのチュートリアルでは、により、SQL Server データベースの 1 つでのみ使用されるアカウントをホストしている場合です。</span><span class="sxs-lookup"><span data-stu-id="2c253-132">That's the case with the Cytanium Lite hosting account that's used for this tutorial, which allows only a single SQL Server database.</span></span>

<span data-ttu-id="2c253-133">このチュートリアルでこのように、2 つのデータベースを移行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-133">In this tutorial, you'll migrate your two databases this way:</span></span>

- <span data-ttu-id="2c253-134">開発環境で 2 つの LocalDB データベースに移行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-134">Migrate to two LocalDB databases in the development environment.</span></span>
- <span data-ttu-id="2c253-135">テスト環境で 2 つの SQL Server Express データベースに移行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-135">Migrate to two SQL Server Express databases in the test environment.</span></span>
- <span data-ttu-id="2c253-136">1 つ結合された完全な SQL Server データベースの運用環境に移行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-136">Migrate to one combined full SQL Server database in the production environment.</span></span>

<span data-ttu-id="2c253-137">リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。</span><span class="sxs-lookup"><span data-stu-id="2c253-137">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="installing-sql-server-express"></a><span data-ttu-id="2c253-138">SQL Server Express をインストールします。</span><span class="sxs-lookup"><span data-stu-id="2c253-138">Installing SQL Server Express</span></span>

<span data-ttu-id="2c253-139">既定では、Visual Studio 2010 が自動的にインストールされている SQL Server Express しますが、既定でインストールされていない Visual Studio 2012 を使用します。</span><span class="sxs-lookup"><span data-stu-id="2c253-139">SQL Server Express is automatically installed by default with Visual Studio 2010, but by default it is not installed with Visual Studio 2012.</span></span> <span data-ttu-id="2c253-140">SQL Server 2012 Express をインストールするには、次のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c253-140">To install SQL Server 2012 Express, click the following link</span></span>

- [<span data-ttu-id="2c253-141">SQL Server Express 2012</span><span class="sxs-lookup"><span data-stu-id="2c253-141">SQL Server Express 2012</span></span>](https://www.microsoft.com/download/details.aspx?id=29062)

<span data-ttu-id="2c253-142">選択*日本語/x64/SQLEXPR\_x64\_ENU.exe*または*日本語/x86/SQLEXPR\_x86\_ENU.exe*、し、インストール ウィザードで、既定を受け入れます設定。</span><span class="sxs-lookup"><span data-stu-id="2c253-142">Choose *ENU/x64/SQLEXPR\_x64\_ENU.exe* or *ENU/x86/SQLEXPR\_x86\_ENU.exe*, and in the installation wizard accept the default settings.</span></span> <span data-ttu-id="2c253-143">インストール オプションの詳細については、次を参照してください。[インストール ウィザード (セットアップ) から SQL Server 2012 のインストール](https://msdn.microsoft.com/library/ms143219.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2c253-143">For more information about installation options, see [Install SQL Server 2012 from the Installation Wizard (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).</span></span>

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a><span data-ttu-id="2c253-144">テスト環境用の SQL Server Express データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-144">Creating SQL Server Express Databases for the Test Environment</span></span>

<span data-ttu-id="2c253-145">次の手順では、ASP.NET メンバーシップと School データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-145">The next step is to create the ASP.NET membership and School databases.</span></span>

<span data-ttu-id="2c253-146">**ビュー**メニューの **サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Visual Web Developer で)、右クリックし、**データ接続**選択**新しい SQL Server データベースの作成**です。</span><span class="sxs-lookup"><span data-stu-id="2c253-146">From the **View** menu select **Server Explorer** (**Database Explorer** in Visual Web Developer), and then right-click **Data Connections** and select **Create New SQL Server Database**.</span></span>

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

<span data-ttu-id="2c253-148">**新しい SQL Server データベースの作成** ダイアログ ボックスに、入力". \SQLExpress"で、**サーバー名**ボックスと"aspnet Test"で、**新しいデータベース名** をクリックし、**OK**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-148">In the **Create New SQL Server Database** dialog box, enter ".\SQLExpress" in the **Server name** box and "aspnet-Test" in the **New database name** box, then click **OK**.</span></span>

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

<span data-ttu-id="2c253-150">"学校 Test"という名前の新しい SQL Server Express の School データベースを作成する同じ手順に従います。</span><span class="sxs-lookup"><span data-stu-id="2c253-150">Follow the same procedure to create a new SQL Server Express School database named "School-Test".</span></span>

<span data-ttu-id="2c253-151">(追加する"Test"これらのデータベース名を後で開発環境では、各データベースの追加のインスタンスを作成し、データベースの 2 つのセットを区別するためにできるようにする必要があるためです。)</span><span class="sxs-lookup"><span data-stu-id="2c253-151">(You're appending "Test" to these database names because later you'll create an additional instance of each database for the development environment, and you need to be able to differentiate the two sets of databases.)</span></span>

<span data-ttu-id="2c253-152">**サーバー エクスプ ローラー** 2 つの新しいデータベースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c253-152">**Server Explorer** now shows the two new databases.</span></span>

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a><span data-ttu-id="2c253-154">新しいデータベースの許可スクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-154">Creating a Grant Script for the New Databases</span></span>

<span data-ttu-id="2c253-155">アプリケーションを開発用コンピューターに IIS で実行すると、アプリケーションは既定のアプリケーション プールの資格情報を使用して、データベースにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="2c253-155">When the application runs in IIS on your development computer, the application accesses the database by using the default application pool's credentials.</span></span> <span data-ttu-id="2c253-156">ただし、既定では、アプリケーション プール id は、データベースを開くアクセス許可がありません。</span><span class="sxs-lookup"><span data-stu-id="2c253-156">However, by default, the application pool identity does not have permission to open the databases.</span></span> <span data-ttu-id="2c253-157">アクセス許可を付与するスクリプトを実行する必要がようにします。</span><span class="sxs-lookup"><span data-stu-id="2c253-157">So you have to run a script to grant that permission.</span></span> <span data-ttu-id="2c253-158">このセクションでは、IIS での実行時に、アプリケーションが、データベースを開くことができるかどうかを確認する後で実行しますスクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-158">In this section you create the script that you'll run later to make sure that the application can open the databases when it runs in IIS.</span></span>

<span data-ttu-id="2c253-159">ソリューションの*SolutionFiles*で作成したフォルダー、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルでは、という名前の新しい SQL ファイルを作成する*Grant.sql*します。</span><span class="sxs-lookup"><span data-stu-id="2c253-159">In the solution's *SolutionFiles* folder that you created in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, create a new SQL file named *Grant.sql*.</span></span> <span data-ttu-id="2c253-160">ファイルに次の SQL コマンドをコピーおよび保存して、ファイルを閉じる。</span><span class="sxs-lookup"><span data-stu-id="2c253-160">Copy the following SQL commands into the file, and then save and close the file:</span></span>

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> <span data-ttu-id="2c253-161">このスクリプトは、このチュートリアルで指定されている SQL Server 2008 と Windows 7 で IIS 設定の動作設計されています。</span><span class="sxs-lookup"><span data-stu-id="2c253-161">This script is designed to work with SQL Server 2008 and with the IIS settings in Windows 7 as they are specified in this tutorial.</span></span> <span data-ttu-id="2c253-162">または、Windows の SQL Server の別のバージョンを使用している場合、または IIS を設定するコンピューターに異なる場合は、このスクリプトへの変更が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-162">If you're using a different version of SQL Server or of Windows, or if you set up IIS on your computer differently, changes to this script might be required.</span></span> <span data-ttu-id="2c253-163">SQL Server スクリプトの詳細については、次を参照してください。 [SQL Server オンライン ブックの「](https://go.microsoft.com/fwlink/?LinkId=132511)します。</span><span class="sxs-lookup"><span data-stu-id="2c253-163">For more information about SQL Server scripts, see [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="2c253-164">**セキュリティに関する注意**このスクリプトは、db\_、運用環境でがありますが、実行時にデータベースにアクセスするユーザーに対する所有者アクセス許可。</span><span class="sxs-lookup"><span data-stu-id="2c253-164">**Security Note** This script gives db\_owner permissions to the user that accesses the database at run time, which is what you'll have in the production environment.</span></span> <span data-ttu-id="2c253-165">一部のシナリオで、展開にのみアクセス許可を更新し、実行時のデータを読み書きするのみのアクセス許可を持つ別のユーザーを指定します。 データベースの完全スキーマを持つユーザーを指定する場合があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-165">In some scenarios you might want to specify a user that has full database schema update permissions only for deployment, and specify for run time a different user that has permissions only to read and write data.</span></span> <span data-ttu-id="2c253-166">詳細については、次を参照してください。 **Code First Migrations に対する自動の Web.config の変更をレビュー**で[テスト環境として IIS への配置](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)します。</span><span class="sxs-lookup"><span data-stu-id="2c253-166">For more information, see **Reviewing the Automatic Web.config Changes for Code First Migrations** in [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).</span></span>


## <a name="configuring-database-deployment-for-the-test-environment"></a><span data-ttu-id="2c253-167">テスト環境用データベース デプロイの構成</span><span class="sxs-lookup"><span data-stu-id="2c253-167">Configuring Database Deployment for the Test Environment</span></span>

<span data-ttu-id="2c253-168">次に、データベースごとに、次のタスクを行うことができるように、Visual Studio を構成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-168">Next, you'll configure Visual Studio so that it will do the following tasks for each database:</span></span>

- <span data-ttu-id="2c253-169">転送先データベースで、ソース データベースの構造 (テーブル、列、制約など) を作成する SQL スクリプトを生成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-169">Generate a SQL script that creates the source database's structure (tables, columns, constraints, etc.) in the destination database.</span></span>
- <span data-ttu-id="2c253-170">転送先データベース内のテーブルにソース データベースのデータを挿入する SQL スクリプトを生成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-170">Generate a SQL script that inserts the source database's data into the tables in the destination database.</span></span>
- <span data-ttu-id="2c253-171">生成されたスクリプト、および転送先データベースで、作成した許可スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-171">Run the generated scripts, and the Grant script that you created, in the destination database.</span></span>

<span data-ttu-id="2c253-172">開く、**プロジェクト プロパティ**ウィンドウと選択、**パッケージ化/発行 SQL**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-172">Open the **Project Properties** window and select the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="2c253-173">確認します**アクティブ (リリース)** または**リリース**でが選択されている、**構成**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="2c253-173">Make sure that **Active (Release)** or **Release** is selected in the **Configuration** drop-down list.</span></span>

<span data-ttu-id="2c253-174">クリックして**このページを有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-174">Click **Enable this Page**.</span></span>

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

<span data-ttu-id="2c253-176">**パッケージ化/発行 SQL**  タブが従来のデプロイ方法を指定するために通常無効になります。</span><span class="sxs-lookup"><span data-stu-id="2c253-176">The **Package/Publish SQL** tab is normally disabled because it specifies a legacy deployment method.</span></span> <span data-ttu-id="2c253-177">ほとんどのシナリオでデータベースの配置を構成する必要があります、 **Web の発行**ウィザード。</span><span class="sxs-lookup"><span data-stu-id="2c253-177">For most scenarios, you should configure database deployment in the **Publish Web** wizard.</span></span> <span data-ttu-id="2c253-178">SQL Server Compact から SQL Server または SQL Server Express への移行は、このメソッドは、適切な選択特殊なケースです。</span><span class="sxs-lookup"><span data-stu-id="2c253-178">Migrating from SQL Server Compact to SQL Server or SQL Server Express is a special case for which this method is a good choice.</span></span>

<span data-ttu-id="2c253-179">クリックして**web.config ファイルからインポート**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-179">Click **Import from Web.config**.</span></span>

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

<span data-ttu-id="2c253-181">Visual Studio での接続文字列では、 *Web.config*ファイル、メンバーシップ データベースに 1 つと、School データベースに 1 つを検索および内の各接続文字列に対応する行を追加します、**データベース エントリ**テーブル。</span><span class="sxs-lookup"><span data-stu-id="2c253-181">Visual Studio looks for connection strings in the *Web.config* file, finds one for the membership database and one for the School database, and adds a row corresponding to each connection string in the **Database Entries** table.</span></span> <span data-ttu-id="2c253-182">既存の SQL Server Compact データベースの接続文字列が見つかったがあり、次の手順は、方法と場所を構成するこれらのデータベースを展開します。</span><span class="sxs-lookup"><span data-stu-id="2c253-182">The connection strings it finds are for the existing SQL Server Compact databases, and your next step will be to configure how and where to deploy these databases.</span></span>

<span data-ttu-id="2c253-183">データベースの配置設定を入力する、**データベース エントリの詳細**以下のセクション、**データベース エントリ**テーブル。</span><span class="sxs-lookup"><span data-stu-id="2c253-183">You enter database deployment settings in the **Database Entry Details** section below the **Database Entries** table.</span></span> <span data-ttu-id="2c253-184">表示される設定、**データベース エントリの詳細**セクションに関連する行の方、**データベース エントリ**テーブルが選択されている、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="2c253-184">The settings shown in the **Database Entry Details** section pertain to whichever row in the **Database Entries** table is selected, as shown in the following illustration.</span></span>

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a><span data-ttu-id="2c253-186">メンバーシップ データベースの展開設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-186">Configuring Deployment Settings for the Membership Database</span></span>

<span data-ttu-id="2c253-187">選択、 **DefaultConnection 展開**行、**データベース エントリ**メンバーシップ データベースに適用される設定を構成するにはテーブルです。</span><span class="sxs-lookup"><span data-stu-id="2c253-187">Select the **DefaultConnection-Deployment** row in the **Database Entries** table in order to configure settings that apply to the membership database.</span></span>

<span data-ttu-id="2c253-188">**転送先データベースへの接続文字列**、新しい SQL Server Express のメンバーシップ データベースを指す接続文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="2c253-188">In **Connection string for destination database**, enter a connection string that points to the new SQL Server Express membership database.</span></span> <span data-ttu-id="2c253-189">必要な接続文字列を取得できます**サーバー エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-189">You can get the connection string you need from **Server Explorer**.</span></span> <span data-ttu-id="2c253-190">**サーバー エクスプ ローラー**、展開**データ接続**を選択し、 **aspnetTest**データベースから、**プロパティ**ウィンドウのコピー、**接続文字列**値。</span><span class="sxs-lookup"><span data-stu-id="2c253-190">In **Server Explorer**, expand **Data Connections** and select the **aspnetTest** database, then from the **Properties** window copy the **Connection String** value.</span></span>

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

<span data-ttu-id="2c253-192">同じ接続文字列はここに再掲します。</span><span class="sxs-lookup"><span data-stu-id="2c253-192">The same connection string is reproduced here:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

<span data-ttu-id="2c253-193">コピーして貼り付けるには、この接続文字列**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-193">Copy and paste this connection string into **Connection string for destination database** in the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="2c253-194">確認します**データや既存のデータベースからスキーマをプル**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="2c253-194">Make sure that **Pull data and/or schema from an existing database** is selected.</span></span> <span data-ttu-id="2c253-195">これは、自動的に生成され、転送先データベースで実行する SQL スクリプトの原因は何です。</span><span class="sxs-lookup"><span data-stu-id="2c253-195">This is what causes SQL scripts to be automatically generated and run in the destination database.</span></span>

<span data-ttu-id="2c253-196">**ソース データベースの接続文字列**から値を抽出、 *Web.config*ファイルと開発用の SQL Server Compact データベースへのポインター。</span><span class="sxs-lookup"><span data-stu-id="2c253-196">The **Connection string for the source database** value is extracted from the *Web.config* file and points to the development SQL Server Compact database.</span></span> <span data-ttu-id="2c253-197">これは、転送先データベースで後で実行されるスクリプトを生成するために使用するソース データベースです。</span><span class="sxs-lookup"><span data-stu-id="2c253-197">This is the source database that will be used to generate the scripts that will run later in the destination database.</span></span> <span data-ttu-id="2c253-198">データベースの運用バージョンをデプロイするため、"aspnet Dev.sdf"を"aspnet Prod.sdf"に変更します。</span><span class="sxs-lookup"><span data-stu-id="2c253-198">Since you want to deploy the production version of the database, change "aspnet-Dev.sdf" to "aspnet-Prod.sdf".</span></span>

<span data-ttu-id="2c253-199">変更**データベース スクリプト オプション**から**スキーマのみ**に**スキーマとデータ**データベース構造と (ユーザー アカウントとロール)、データをコピーするため、します。</span><span class="sxs-lookup"><span data-stu-id="2c253-199">Change **Database scripting options** from **Schema Only** to **Schema and data**, since you want to copy your data (user accounts and roles) as well as the database structure.</span></span>

<span data-ttu-id="2c253-200">先ほど作成した許可スクリプトを実行する展開を構成するに追加する必要が、**データベース スクリプト**セクション。</span><span class="sxs-lookup"><span data-stu-id="2c253-200">To configure deployment to run the grant scripts that you created earlier, you have to add them to the **Database Scripts** section.</span></span> <span data-ttu-id="2c253-201">をクリックして**スクリプトの追加**、し、 **SQL スクリプトの追加** ダイアログ ボックスで、許可スクリプトを保存したフォルダーに移動します (これは、ソリューション ファイルを含むフォルダーです)。</span><span class="sxs-lookup"><span data-stu-id="2c253-201">Click **Add Script**, and in the **Add SQL Scripts** dialog box, navigate to the folder where you stored the grant script (this is the folder that contains your solution file).</span></span> <span data-ttu-id="2c253-202">という名前のファイルを選択します。 *Grant.sql*、 をクリック**オープン**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-202">Select the file named *Grant.sql*, and click **Open**.</span></span>

<span data-ttu-id="2c253-203">[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="2c253-203">[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)</span></span>

<span data-ttu-id="2c253-204">設定、 **DefaultConnection 展開**行**データベース エントリ**次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="2c253-204">The settings for the **DefaultConnection-Deployment** row in **Database Entries** now look like the following illustration:</span></span>

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a><span data-ttu-id="2c253-206">School データベースの展開設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-206">Configuring Deployment Settings for the School Database</span></span>

<span data-ttu-id="2c253-207">次に、選択、 **SchoolContext 展開**行、**データベース エントリ**School データベースの展開設定を構成するためにテーブルです。</span><span class="sxs-lookup"><span data-stu-id="2c253-207">Next, select the **SchoolContext-Deployment** row in the **Database Entries** table in order to configure deployment settings for the School database.</span></span>

<span data-ttu-id="2c253-208">新しい SQL Server Express データベースの接続文字列を取得する前に使用した同じメソッドを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="2c253-208">You can use the same method you used earlier to get the connection string for the new SQL Server Express database.</span></span> <span data-ttu-id="2c253-209">この接続文字列をコピー**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-209">Copy this connection string into **Connection string for destination database** in the **Package/Publish SQL** tab.</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

<span data-ttu-id="2c253-210">確認します**データや既存のデータベースからスキーマをプル**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="2c253-210">Make sure that **Pull data and/or schema from an existing database** is selected.</span></span>

<span data-ttu-id="2c253-211">**ソース データベースの接続文字列**から値を抽出、 *Web.config*ファイルと開発用の SQL Server Compact データベースへのポインター。</span><span class="sxs-lookup"><span data-stu-id="2c253-211">The **Connection string for the source database** value is extracted from the *Web.config* file and points to the development SQL Server Compact database.</span></span> <span data-ttu-id="2c253-212">"学校 Prod.sdf"実稼働バージョンのデータベースを展開するには、"学校 Dev.sdf"を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c253-212">Change "School-Dev.sdf" to "School-Prod.sdf" to deploy the production version of the database.</span></span> <span data-ttu-id="2c253-213">(アプリに学校 Prod.sdf ファイルを作成することはありません\_データ フォルダー、アプリに、テスト環境からそのファイルをコピーします\_ContosoUniversity プロジェクト フォルダーを後で、データ フォルダーです)。</span><span class="sxs-lookup"><span data-stu-id="2c253-213">(You never created a School-Prod.sdf file in the App\_Data folder, so you'll copy that file from the test environment to the App\_Data folder in the ContosoUniversity project folder later.)</span></span>

<span data-ttu-id="2c253-214">変更**データベース スクリプト オプション**に**スキーマとデータ**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-214">Change **Database scripting options** to **Schema and data**.</span></span>

<span data-ttu-id="2c253-215">読み取りを許可し、アプリケーション プール id にこのデータベースに対する権限を書き込む追加して、スクリプトを実行する、 *Grant.sql*メンバーシップ データベースの場合と同様のスクリプト ファイル。</span><span class="sxs-lookup"><span data-stu-id="2c253-215">You also want to run the script to grant read and write permission for this database to the application pool identity, so add the *Grant.sql* script file as you did for the membership database.</span></span>

<span data-ttu-id="2c253-216">完了したら、設定**SchoolContext 展開**行**データベース エントリ**次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="2c253-216">When you're done, the settings for the **SchoolContext-Deployment** row in **Database Entries** look like the following illustration:</span></span>

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

<span data-ttu-id="2c253-218">変更を保存、**パッケージ化/発行 SQL**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-218">Save the changes to the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="2c253-219">コピー、*の学校 Prod.sdf*ファイルから、 *c:\inetpub\wwwroot\ContosoUniversity\App\_データ*フォルダーを*アプリ\_データ*フォルダーContosoUniversity プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="2c253-219">Copy the *School-Prod.sdf* file from the *c:\inetpub\wwwroot\ContosoUniversity\App\_Data* folder to the *App\_Data* folder in the ContosoUniversity project.</span></span>

### <a name="specifying-transacted-mode-for-the-grant-script"></a><span data-ttu-id="2c253-220">許可スクリプトのトランザクション モードを指定します。</span><span class="sxs-lookup"><span data-stu-id="2c253-220">Specifying Transacted Mode for the Grant Script</span></span>

<span data-ttu-id="2c253-221">展開プロセスでは、データベース スキーマとデータ デプロイ スクリプトを生成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-221">The deployment process generates scripts that deploy the database schema and data.</span></span> <span data-ttu-id="2c253-222">既定では、これらのスクリプトは、トランザクションで実行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-222">By default, these scripts run in a transaction.</span></span> <span data-ttu-id="2c253-223">ただし、既定で (grant スクリプト) のようなカスタム スクリプトは、トランザクションでは実行されません。</span><span class="sxs-lookup"><span data-stu-id="2c253-223">However, custom scripts (like the grant scripts) by default do not run in a transaction.</span></span> <span data-ttu-id="2c253-224">展開プロセスでは、トランザクション モードを混在、デプロイ時に、スクリプトが実行される場合にタイムアウト エラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-224">If the deployment process mixes transaction modes, you might get a timeout error when the scripts run during deployment.</span></span> <span data-ttu-id="2c253-225">このセクションでは、トランザクションで実行するカスタム スクリプトを構成するには、プロジェクト ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="2c253-225">In this section, you edit the project file in order to configure the custom scripts to run in a transaction.</span></span>

<span data-ttu-id="2c253-226">**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**順に選択して**プロジェクトのアンロード**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-226">In **Solution Explorer**, right-click the **ContosoUniversity** project and select **Unload Project**.</span></span>

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

<span data-ttu-id="2c253-228">プロジェクトをもう一度右クリックし、選択**contosouniversity.csproj の編集**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-228">Then right-click the project again and select **Edit ContosoUniversity.csproj**.</span></span>

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

<span data-ttu-id="2c253-230">Visual Studio エディターでは、プロジェクト ファイルの XML コンテンツを示します。</span><span class="sxs-lookup"><span data-stu-id="2c253-230">The Visual Studio editor shows you the XML content of the project file.</span></span> <span data-ttu-id="2c253-231">いくつに注意してください。`PropertyGroup`要素。</span><span class="sxs-lookup"><span data-stu-id="2c253-231">Notice that there are several `PropertyGroup` elements.</span></span> <span data-ttu-id="2c253-232">(図の内容で、`PropertyGroup`要素が省略されています)。</span><span class="sxs-lookup"><span data-stu-id="2c253-232">(In the image, the contents of the `PropertyGroup` elements have been omitted.)</span></span>

![プロジェクト ファイルのエディター ウィンドウ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

<span data-ttu-id="2c253-234">ない最初の 1 つは、`Condition`属性、構成のビルドに関係なく適用される設定。</span><span class="sxs-lookup"><span data-stu-id="2c253-234">The first one, which has no `Condition` attribute, is for settings that apply regardless of build configuration.</span></span> <span data-ttu-id="2c253-235">1 つ`PropertyGroup`要素が、デバッグ ビルド構成にのみ適用されます (注、`Condition`属性)、リリース ビルド構成の場合にのみ適用し、テストのビルド構成にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="2c253-235">One `PropertyGroup` element applies only to the Debug build configuration (note the `Condition` attribute), one applies only to the Release build configuration, and one applies only to the Test build configuration.</span></span> <span data-ttu-id="2c253-236">内で、`PropertyGroup`リリース ビルド構成の要素が表示されます、`PublishDatabaseSettings`で入力した設定を含む要素、**パッケージ化/発行 SQL**タブ。`Object` Grant スクリプトのそれぞれに対応する要素 ("Grant.sql"のインスタンスを 2 つに注目してください) を指定します。</span><span class="sxs-lookup"><span data-stu-id="2c253-236">Within the `PropertyGroup` element for the Release build configuration, you'll see a `PublishDatabaseSettings` element that contains the settings you entered on the **Package/Publish SQL** tab. There is an `Object` element that corresponds to each of the grant scripts you specified (notice the two instances of "Grant.sql").</span></span> <span data-ttu-id="2c253-237">既定で、`Transacted`の属性、 `Source` grant スクリプトごとに要素が`False`します。</span><span class="sxs-lookup"><span data-stu-id="2c253-237">By default, the `Transacted` attribute of the `Source` element for each grant script is `False`.</span></span>

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

<span data-ttu-id="2c253-239">値を変更、`Transacted`の属性、`Source`要素`True`します。</span><span class="sxs-lookup"><span data-stu-id="2c253-239">Change the value of the `Transacted` attribute of the `Source` element to `True`.</span></span>

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

<span data-ttu-id="2c253-241">保存して、プロジェクト ファイルを閉じてでプロジェクトを右クリックし、**ソリューション エクスプ ローラー**選択**プロジェクトの再読み込み**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-241">Save and close the project file, and then right-click the project in **Solution Explorer** and select **Reload Project**.</span></span>

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a><span data-ttu-id="2c253-243">接続文字列の Web.Config 変換の設定</span><span class="sxs-lookup"><span data-stu-id="2c253-243">Setting up Web.Config Transformations for the Connection Strings</span></span>

<span data-ttu-id="2c253-244">接続文字列入力した新しい SQL Express データベースの**パッケージ化/発行 SQL**  タブは、デプロイ中に転送先データベースを更新するためだけ Web Deploy によって使用されます。</span><span class="sxs-lookup"><span data-stu-id="2c253-244">The connection strings for the new SQL Express databases that you entered on the **Package/Publish SQL** tab are used by Web Deploy only for updating the destination database during deployment.</span></span> <span data-ttu-id="2c253-245">設定する必要がある*Web.config*変換、接続文字列ので、デプロイされているように*Web.config*ファイルの新しい SQL Server Express データベースをポイントします。</span><span class="sxs-lookup"><span data-stu-id="2c253-245">You still have to set up *Web.config* transformations so that the connection strings in the deployed *Web.config* file point to the new SQL Server Express databases.</span></span> <span data-ttu-id="2c253-246">(使用すると、**パッケージ化/発行 SQL**  タブで、発行プロファイルの接続文字列を構成することはできません)。</span><span class="sxs-lookup"><span data-stu-id="2c253-246">(When you use the **Package/Publish SQL** tab, you can't configure connection strings in the publish profile.)</span></span>

<span data-ttu-id="2c253-247">開いている*Web.Test.config*と置換、`connectionStrings`を持つ要素、`connectionStrings`次の例では、内の要素。</span><span class="sxs-lookup"><span data-stu-id="2c253-247">Open *Web.Test.config* and replace the `connectionStrings` element with the `connectionStrings` element in the following example.</span></span> <span data-ttu-id="2c253-248">(コンテキストを提供するは、ここに表示される周囲のコードされません、connectionStrings 要素のみをコピーすることを確認してください。)</span><span class="sxs-lookup"><span data-stu-id="2c253-248">(Make sure you only copy the connectionStrings element, not the surrounding code that is shown here to provide context.)</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

<span data-ttu-id="2c253-249">このコードにより、`connectionString`と`providerName`の各属性`add`置き換えられる要素で、デプロイされている*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2c253-249">This code causes the `connectionString` and `providerName` attributes of each `add` element to be replaced in the deployed *Web.config* file.</span></span> <span data-ttu-id="2c253-250">これらの接続文字列で入力したものとは異なるが、**パッケージ化/発行 SQL**タブ。設定"MultipleActiveResultSets = True"の Entity Framework およびユニバーサル プロバイダーことが必要なため、それらに追加されました。</span><span class="sxs-lookup"><span data-stu-id="2c253-250">These connection strings are not identical to the ones you entered in the **Package/Publish SQL** tab. The setting "MultipleActiveResultSets=True" has been added to them because it's required for the Entity Framework and the Universal Providers.</span></span>

## <a name="installing-sql-server-compact"></a><span data-ttu-id="2c253-251">SQL Server Compact のインストール</span><span class="sxs-lookup"><span data-stu-id="2c253-251">Installing SQL Server Compact</span></span>

<span data-ttu-id="2c253-252">SqlServerCompact NuGet パッケージは、Contoso University アプリケーションのアセンブリをエンジン、SQL Server Compact データベースを提供します。</span><span class="sxs-lookup"><span data-stu-id="2c253-252">The SqlServerCompact NuGet package provides the SQL Server Compact database engine assemblies for the Contoso University application.</span></span> <span data-ttu-id="2c253-253">ここではありませんが、アプリケーション、Web Deploy、SQL Server データベースで実行するスクリプトを作成するには、SQL Server Compact のデータベースを読めるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-253">But now it is not the application but Web Deploy that must be able to read the SQL Server Compact databases, in order to create scripts to run in the SQL Server databases.</span></span> <span data-ttu-id="2c253-254">Web 配置で SQL Server Compact データベースの読み取りを有効にする SQL Server Compact 開発用コンピューターで、次のリンクを使用してインストール: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6)します。</span><span class="sxs-lookup"><span data-stu-id="2c253-254">To enable Web Deploy to read SQL Server Compact databases, install SQL Server Compact on the development computer by using the following link: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).</span></span>

## <a name="deploying-to-the-test-environment"></a><span data-ttu-id="2c253-255">テスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="2c253-255">Deploying to the Test Environment</span></span>

<span data-ttu-id="2c253-256">テスト環境に発行するためには、発行プロファイルを使用するように構成を作成する必要が、**パッケージ化/発行 SQL**データベース発行プロファイルのデータベース設定ではなく公開のタブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-256">In order to publish to the Test environment, you have to create a publish profile that is configured to use the **Package/Publish SQL** tab for database publishing instead of the publish profile database settings.</span></span>

<span data-ttu-id="2c253-257">最初に、既存のテストのプロファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="2c253-257">First, delete the existing Test profile.</span></span>

<span data-ttu-id="2c253-258">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-258">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="2c253-259">選択、**プロファイル**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-259">Select the **Profile** tab.</span></span>

<span data-ttu-id="2c253-260">クリックして**プロファイルを管理する**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-260">Click **Manage Profiles**.</span></span>

<span data-ttu-id="2c253-261">選択**テスト**、 をクリックして**削除**、 をクリックし、**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-261">Select **Test**, click **Remove**, and then click **Close**.</span></span>

<span data-ttu-id="2c253-262">閉じる、 **Web の発行**ウィザードでこの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="2c253-262">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="2c253-263">次に、新しいテスト プロファイルを作成し、それを使用してプロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-263">Next, create a new Test profile and use it to publish the project.</span></span>

<span data-ttu-id="2c253-264">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-264">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="2c253-265">選択、**プロファイル**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-265">Select the **Profile** tab.</span></span>

<span data-ttu-id="2c253-266">選択**&lt;新しい.&gt;** ドロップダウン リストから、プロファイル名として"Test"を入力します。</span><span class="sxs-lookup"><span data-stu-id="2c253-266">Select **&lt;New...&gt;** from the drop-down list, and enter "Test" as the profile name.</span></span>

<span data-ttu-id="2c253-267">**サービス URL**ボックスに、入力*localhost*します。</span><span class="sxs-lookup"><span data-stu-id="2c253-267">In the **Service URL** box, enter *localhost*.</span></span>

<span data-ttu-id="2c253-268">**サイト/アプリケーション**ボックスに、入力*既定の Web サイト/ContosoUniversity*します。</span><span class="sxs-lookup"><span data-stu-id="2c253-268">In the **Site/application** box, enter *Default Web Site/ContosoUniversity*.</span></span>

<span data-ttu-id="2c253-269">**送信先 URL**ボックスに、入力`http://localhost/ContosoUniversity/`します。</span><span class="sxs-lookup"><span data-stu-id="2c253-269">In the **Destination URL** box, enter `http://localhost/ContosoUniversity/`.</span></span>

<span data-ttu-id="2c253-270">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c253-270">Click **Next**.</span></span>

<span data-ttu-id="2c253-271">**設定**警告を表示するタブを**パッケージ化/発行 SQL**タブが構成されている、および有効にする をクリックして新しいデータベース発行の機能強化をオーバーライドする機会を提供します。</span><span class="sxs-lookup"><span data-stu-id="2c253-271">The **Settings** tab warns you that the **Package/Publish SQL** tab has been configured, and it gives you an opportunity to override them by clicking enable the new database publishing improvements.</span></span> <span data-ttu-id="2c253-272">この展開をオーバーライドする必要はありません、**パッケージ化/発行 SQL**設定 タブをクリックしますので、**次**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-272">For this deployment you don't want to override the **Package/Publish SQL** tab settings, so just click **Next**.</span></span>

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

<span data-ttu-id="2c253-274">メッセージを**プレビュー**  タブには、ことを示します**パブリッシュするデータベースが選択されていない**が単データベースの発行が、発行プロファイルで構成されていないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="2c253-274">A message on the **Preview** tab indicates that **No databases are selected to publish**, but this only means that database publishing is not configured in the publish profile.</span></span>

<span data-ttu-id="2c253-275">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c253-275">Click **Publish**.</span></span>

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

<span data-ttu-id="2c253-277">Visual Studio では、アプリケーションを展開し、テスト環境で、サイトのホーム ページをブラウザーが開きます。</span><span class="sxs-lookup"><span data-stu-id="2c253-277">Visual Studio deploys the application and opens the browser to the home page of the site in the test environment.</span></span> <span data-ttu-id="2c253-278">先ほど見たを同じデータを表示するかを確認する、Instructors ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-278">Run the Instructors page to see that it displays the same data that you saw earlier.</span></span> <span data-ttu-id="2c253-279">実行、**受講者の追加** ページで、新しい生徒を追加して、新しい学生を表示し、**学生**ページ。</span><span class="sxs-lookup"><span data-stu-id="2c253-279">Run the **Add Students** page, add a new student, and then view the new student in the **Students** page.</span></span> <span data-ttu-id="2c253-280">これは、データベースを更新することができますを確認します。</span><span class="sxs-lookup"><span data-stu-id="2c253-280">This verifies that the you can update the database.</span></span> <span data-ttu-id="2c253-281">選択、**更新クレジット**ページ (にログインする必要があります) をメンバーシップ データベースが配置され、アクセス権があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2c253-281">Select the **Update Credits** page (you'll need to log in) to verify that the membership database was deployed and you have access to it.</span></span>

## <a name="creating-a-sql-server-database-for-the-production-environment"></a><span data-ttu-id="2c253-282">運用環境の SQL Server データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-282">Creating a SQL Server Database for the Production Environment</span></span>

<span data-ttu-id="2c253-283">テスト環境にデプロイしたは、これでは、運用環境にデプロイをセットアップする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="2c253-283">Now that you've deployed to the test environment, you're ready to set up deployment to production.</span></span> <span data-ttu-id="2c253-284">場合と同様、テスト環境にデプロイするデータベースを作成して開始します。</span><span class="sxs-lookup"><span data-stu-id="2c253-284">You begin as you did for the test environment, by creating a database to deploy to.</span></span> <span data-ttu-id="2c253-285">概要から学習したように、Cytanium Lite のホスティング プランはない 2 つの 1 つだけデータベースをセットアップするため、単一の SQL Server データベースを許可するだけです。</span><span class="sxs-lookup"><span data-stu-id="2c253-285">As you recall from the Overview, the Cytanium Lite hosting plan only allows a single SQL Server database, so you will set up only one database, not two.</span></span> <span data-ttu-id="2c253-286">すべてのテーブルとメンバーシップおよび学校 SQL Server Compact のデータベースからのデータは、実稼働環境で 1 つの SQL Server データベースにデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="2c253-286">All of the tables and data from the membership and School SQL Server Compact databases will be deployed into one SQL Server database in production.</span></span>

<span data-ttu-id="2c253-287">Cytanium コントロール パネルに移動して[ http://panel.cytanium.com](http://panel.cytanium.com)します。</span><span class="sxs-lookup"><span data-stu-id="2c253-287">Go to the Cytanium control panel at [http://panel.cytanium.com](http://panel.cytanium.com).</span></span> <span data-ttu-id="2c253-288">マウスで**データベース** をクリックし、 **SQL Server 2008**です。</span><span class="sxs-lookup"><span data-stu-id="2c253-288">Hold the mouse over **Databases** and then click **SQL Server 2008**.</span></span>

<span data-ttu-id="2c253-289">[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="2c253-289">[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)</span></span>

<span data-ttu-id="2c253-290">**SQL Server 2008** ] ページで [ **Create Database**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-290">In the **SQL Server 2008** page, click **Create Database**.</span></span>

<span data-ttu-id="2c253-291">[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="2c253-291">[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)</span></span>

<span data-ttu-id="2c253-292">データベースの名前を「学校」にして**保存**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-292">Name the database "School" and click **Save**.</span></span> <span data-ttu-id="2c253-293">(ページに自動的にプレフィックスを追加"contosou"ため、有効な名前が"contosouSchool"になります。)</span><span class="sxs-lookup"><span data-stu-id="2c253-293">(The page automatically adds the prefix "contosou", so the effective name will be "contosouSchool".)</span></span>

<span data-ttu-id="2c253-294">[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="2c253-294">[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)</span></span>

<span data-ttu-id="2c253-295">同じ ページで、 **Create User**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-295">On the same page, click **Create User**.</span></span> <span data-ttu-id="2c253-296">Cytanium のサーバーではなく、統合された Windows セキュリティを使用して、およびアプリケーション プール id が、データベースを開くことができますを開き、データベースに権限を持つユーザーを作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-296">On Cytanium's servers, rather than using integrated Windows security and letting the application pool identity open your database, you'll create a user that has authority to open your database.</span></span> <span data-ttu-id="2c253-297">運用環境に追加する接続文字列をユーザーの資格情報を追加します*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2c253-297">You'll add the user's credentials to the connection strings that go in the production *Web.config* file.</span></span> <span data-ttu-id="2c253-298">この手順では、これらの資格情報を作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-298">In this step you create those credentials.</span></span>

<span data-ttu-id="2c253-299">[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="2c253-299">[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)</span></span>

<span data-ttu-id="2c253-300">必要なフィールドに入力、 **SQL ユーザー プロパティ**ページ。</span><span class="sxs-lookup"><span data-stu-id="2c253-300">Fill in the required fields in the **SQL User Properties** page:</span></span>

- <span data-ttu-id="2c253-301">名前として"ContosoUniversityUser"を入力します。</span><span class="sxs-lookup"><span data-stu-id="2c253-301">Enter "ContosoUniversityUser" as the name.</span></span>
- <span data-ttu-id="2c253-302">パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="2c253-302">Enter a password.</span></span>
- <span data-ttu-id="2c253-303">選択**contosouSchool**既定のデータベースとして。</span><span class="sxs-lookup"><span data-stu-id="2c253-303">Select **contosouSchool** as the default database.</span></span>
- <span data-ttu-id="2c253-304">選択、 **contosouSchool**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="2c253-304">Select the **contosouSchool** check box.</span></span>

<span data-ttu-id="2c253-305">[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="2c253-305">[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)</span></span>

## <a name="configuring-database-deployment-for-the-production-environment"></a><span data-ttu-id="2c253-306">運用環境用データベース デプロイの構成</span><span class="sxs-lookup"><span data-stu-id="2c253-306">Configuring Database Deployment for the Production Environment</span></span>

<span data-ttu-id="2c253-307">データベースの配置設定を設定する準備ができたので、**パッケージ化/発行 SQL**タブの前、テスト環境用と同じです。</span><span class="sxs-lookup"><span data-stu-id="2c253-307">Now you're ready to set up database deployment settings in the **Package/Publish SQL** tab, as you did earlier for the test environment.</span></span>

<span data-ttu-id="2c253-308">開く、**プロジェクト プロパティ**ウィンドウで、**パッケージ化/発行 SQL**  タブを確認して**アクティブ (リリース)** または**リリース**は選択された状態で、**構成**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="2c253-308">Open the **Project Properties** window, select the **Package/Publish SQL** tab, and make sure that **Active (Release)** or **Release** is selected in the **Configuration** drop-down list.</span></span>

<span data-ttu-id="2c253-309">各データベースの展開設定を構成するときに何が運用環境とテスト環境用の主な違いが、接続文字列を構成する方法。</span><span class="sxs-lookup"><span data-stu-id="2c253-309">When you configure deployment settings for each database, the key difference between what you do for production and test environments is in how you configure connection strings.</span></span> <span data-ttu-id="2c253-310">テスト環境の別の変換先データベースの接続文字列を入力したが、運用環境のターゲットの接続文字列は同じになります、両方のデータベース。</span><span class="sxs-lookup"><span data-stu-id="2c253-310">For the test environment you entered different destination database connection strings, but for the production environment the destination connection string will be the same for both databases.</span></span> <span data-ttu-id="2c253-311">両方のデータベースを運用環境で 1 つのデータベースにデプロイするためです。</span><span class="sxs-lookup"><span data-stu-id="2c253-311">This is because you are deploying both databases to one database in production.</span></span>

### <a name="configuring-deployment-settings-for-the-membership-database"></a><span data-ttu-id="2c253-312">メンバーシップ データベースの展開設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-312">Configuring Deployment Settings for the Membership Database</span></span>

<span data-ttu-id="2c253-313">メンバーシップ データベースに適用される設定を構成するには、選択、 **DefaultConnection 展開**行、**データベース エントリ**テーブル。</span><span class="sxs-lookup"><span data-stu-id="2c253-313">To configure settings that apply to the membership database, select the **DefaultConnection-Deployment** row in the **Database Entries** table.</span></span>

<span data-ttu-id="2c253-314">**転送先データベースへの接続文字列**、先ほど作成した新しい実稼働 SQL Server データベースを指す接続文字列を入力します。</span><span class="sxs-lookup"><span data-stu-id="2c253-314">In **Connection string for destination database**, enter a connection string that points to the new production SQL Server database that you just created.</span></span> <span data-ttu-id="2c253-315">接続文字列は、ウェルカム メールから取得できます。</span><span class="sxs-lookup"><span data-stu-id="2c253-315">You can get the connection string from your welcome email.</span></span> <span data-ttu-id="2c253-316">電子メールの関連部分には、次のサンプルの接続文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2c253-316">The relevant part of the email contains the following sample connection string:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

<span data-ttu-id="2c253-317">3 つの変数を置換した後、必要な接続文字列は、この例のようになります。</span><span class="sxs-lookup"><span data-stu-id="2c253-317">After you replace the three variables, the connection string you need looks like this example:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

<span data-ttu-id="2c253-318">コピーして貼り付けるには、この接続文字列**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-318">Copy and paste this connection string into **Connection string for destination database** in the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="2c253-319">確認します**データや既存のデータベースからスキーマをプル**がオンのままとする**データベース スクリプト オプション**が**スキーマとデータ**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-319">Make sure that **Pull data and/or schema from an existing database** is still selected, and that **Database scripting options** is still **Schema and Data**.</span></span>

<span data-ttu-id="2c253-320">**データベース スクリプト**ボックスに、Grant.sql スクリプトの横にあるチェック ボックスをオフにします。</span><span class="sxs-lookup"><span data-stu-id="2c253-320">In the **Database Scripts** box, clear the check box next to the Grant.sql script.</span></span>

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a><span data-ttu-id="2c253-322">School データベースの展開設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-322">Configuring Deployment Settings for the School Database</span></span>

<span data-ttu-id="2c253-323">次に、選択、 **SchoolContext 展開**行、**データベース エントリ**School データベースの設定を構成するためにテーブルです。</span><span class="sxs-lookup"><span data-stu-id="2c253-323">Next, select the **SchoolContext-Deployment** row in the **Database Entries** table in order to configure the School database settings.</span></span>

<span data-ttu-id="2c253-324">同じ接続文字列をコピー**転送先データベースへの接続文字列**メンバーシップ データベースには、そのフィールドにコピーします。</span><span class="sxs-lookup"><span data-stu-id="2c253-324">Copy the same connection string into **Connection string for destination database** that you copied into that field for the membership database.</span></span>

<span data-ttu-id="2c253-325">確認します**データや既存のデータベースからスキーマをプル**がオンのままとする**データベース スクリプト オプション**が**スキーマとデータ**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-325">Make sure that **Pull data and/or schema from an existing database** is still selected, and that **Database scripting options** is still **Schema and Data**.</span></span>

<span data-ttu-id="2c253-326">**データベース スクリプト**ボックスに、Grant.sql スクリプトの横にあるチェック ボックスをオフにします。</span><span class="sxs-lookup"><span data-stu-id="2c253-326">In the **Database Scripts** box, clear the check box next to the Grant.sql script.</span></span>

<span data-ttu-id="2c253-327">変更を保存、**パッケージ化/発行 SQL**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-327">Save the changes to the **Package/Publish SQL** tab.</span></span>

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a><span data-ttu-id="2c253-328">Web.Config の設定は、実稼働データベースへの接続文字列の変換します。</span><span class="sxs-lookup"><span data-stu-id="2c253-328">Setting Up Web.Config Transforms for the Connection Strings to Production Databases</span></span>

<span data-ttu-id="2c253-329">次を設定します*Web.config*変換、接続文字列ので、デプロイされているように*Web.config*新しい実稼働データベースを指すファイル。</span><span class="sxs-lookup"><span data-stu-id="2c253-329">Next, you'll set up *Web.config* transformations so that the connection strings in the deployed *Web.config* file to point to the new production database.</span></span> <span data-ttu-id="2c253-330">接続文字列に入力した、**パッケージ化/発行 SQL** Web 配置で使用してのタブは、同じの追加を除き、使用するアプリケーションに必要な 1 つとして、`MultipleResultSets`オプション。</span><span class="sxs-lookup"><span data-stu-id="2c253-330">The connection string that you entered on the **Package/Publish SQL** tab for Web Deploy to use is the same as the one the application needs to use, except for the addition of the `MultipleResultSets` option.</span></span>

<span data-ttu-id="2c253-331">開いている*Web.Production.config*と置換、`connectionStrings`を持つ要素を`connectionStrings`次の例のような要素です。</span><span class="sxs-lookup"><span data-stu-id="2c253-331">Open *Web.Production.config* and replace the `connectionStrings` element with a `connectionStrings` element that looks like the following example.</span></span> <span data-ttu-id="2c253-332">(コピーのみの`connectionStrings`要素では、周囲タグをコンテキストを示すために用意されていません)。</span><span class="sxs-lookup"><span data-stu-id="2c253-332">(Only copy the `connectionStrings` element, not the surrounding tags that are provided to show the context.)</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

<span data-ttu-id="2c253-333">場合があります内の接続文字列は常に暗号化することを促すアドバイスを参照してください、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2c253-333">You sometimes see advice that tells you to always encrypt connection strings in the *Web.config* file.</span></span> <span data-ttu-id="2c253-334">適切なは、会社のネットワーク上のサーバーにデプロイする場合があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-334">This might be appropriate if you were deploying to servers on your own company's network.</span></span> <span data-ttu-id="2c253-335">共有ホスティング環境に展開する場合に、ホスティング プロバイダーのセキュリティ プラクティスを信頼しているし、必要なまたは接続文字列を暗号化するは実用的ではありません。</span><span class="sxs-lookup"><span data-stu-id="2c253-335">When you are deploying to a shared hosting environment, though, you're trusting the security practices of the hosting provider, and it's not necessary or practical to encrypt the connection strings.</span></span>

## <a name="deploying-to-the-production-environment"></a><span data-ttu-id="2c253-336">実稼働環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="2c253-336">Deploying to the Production Environment</span></span>

<span data-ttu-id="2c253-337">これで、運用環境にデプロイする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="2c253-337">Now you're ready to deploy to production.</span></span> <span data-ttu-id="2c253-338">Web Deploy は、プロジェクトの SQL Server Compact データベースを読み取り*アプリ\_データ*フォルダーとすべてのテーブルと実稼働 SQL Server データベース内のデータを再作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-338">Web Deploy will read the SQL Server Compact databases in your project's *App\_Data* folder and re-create all of their tables and data in the production SQL Server database.</span></span> <span data-ttu-id="2c253-339">使用して発行するために、**パッケージ化/発行 Web**  タブの設定、運用環境の新しい発行プロファイルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-339">In order to publish by using the **Package/Publish Web** tab settings, you have to create a new publish profile for production.</span></span>

<span data-ttu-id="2c253-340">最初に、前と同じテスト プロファイルは、既存の実稼働プロファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="2c253-340">First, delete the existing Production profile as you did the Test profile earlier.</span></span>

<span data-ttu-id="2c253-341">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-341">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="2c253-342">選択、**プロファイル**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-342">Select the **Profile** tab.</span></span>

<span data-ttu-id="2c253-343">クリックして**プロファイルを管理する**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-343">Click **Manage Profiles**.</span></span>

<span data-ttu-id="2c253-344">選択**運用**、 をクリックして**削除**、順にクリックします**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-344">Select **Production**, click **Remove**, and then click **Close**.</span></span>

<span data-ttu-id="2c253-345">閉じる、 **Web の発行**ウィザードでこの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="2c253-345">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="2c253-346">次に、新しい実稼働プロファイルを作成し、それを使用してプロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-346">Next, create a new Production profile and use it to publish the project.</span></span>

<span data-ttu-id="2c253-347">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-347">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="2c253-348">選択、**プロファイル**タブ。</span><span class="sxs-lookup"><span data-stu-id="2c253-348">Select the **Profile** tab.</span></span>

<span data-ttu-id="2c253-349">クリックして**インポート**、先ほどダウンロードした .publishsettings ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="2c253-349">Click **Import**, and select the .publishsettings file that you downloaded earlier.</span></span>

<span data-ttu-id="2c253-350">**接続** タブで、変更、**送信先 URL**この例では、一時的な URL が正しいに http://contosouniversity.com.vserver01.cytanium.com します。</span><span class="sxs-lookup"><span data-stu-id="2c253-350">On the **Connection** tab, change the **Destination URL** to the correct temporary URL, which in this example is http://contosouniversity.com.vserver01.cytanium.com.</span></span>

<span data-ttu-id="2c253-351">運用環境に、プロファイルの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c253-351">Rename the profile to Production.</span></span> <span data-ttu-id="2c253-352">(選択、**プロファイル** タブでをクリックし、**プロファイルの管理**そのために)。</span><span class="sxs-lookup"><span data-stu-id="2c253-352">(Select the **Profile** tab and click **Manage Profiles** to do that).</span></span>

<span data-ttu-id="2c253-353">閉じる、 **Web の発行**ウィザード、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="2c253-353">Close the **Publish Web** wizard to save your changes.</span></span>

<span data-ttu-id="2c253-354">実際のアプリケーションを実稼働環境で、データベースの更新中に行う 2 つの追加手順今すぐ発行する前に。</span><span class="sxs-lookup"><span data-stu-id="2c253-354">In a real application in which the database was being updated in production, you would do two additional steps now before you publish:</span></span>

1. <span data-ttu-id="2c253-355">アップロード*アプリ\_offline.htm*ように、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="2c253-355">Upload *app\_offline.htm*, as shown in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span>
2. <span data-ttu-id="2c253-356">使用して、**ファイル マネージャー**をコピーする Cytanium コントロール パネルの機能、 *aspnet Prod.sdf*と*の学校 Prod.sdf* に運用サイトからファイル*アプリ\_データ*ContosoUniversity プロジェクトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="2c253-356">Use the **File Manager** feature of the Cytanium control panel to copy the *aspnet-Prod.sdf* and *School-Prod.sdf* files from the production site to the *App\_Data* folder of the ContosoUniversity project.</span></span> <span data-ttu-id="2c253-357">これにより、新しい SQL Server データベースにデプロイするデータには、実稼働 web サイトによって行われた最新の更新プログラムが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2c253-357">This ensures that the data you're deploying to the new SQL Server database includes the latest updates made by your production website.</span></span>

<span data-ttu-id="2c253-358">**Web の 1 クリックして発行**ツールバーで、ことを確認します、**運用**プロファイルが選択されているし をクリックし、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-358">In the **Web One Click Publish** toolbar, make sure that the **Production** profile is selected, and then click **Publish**.</span></span>

<span data-ttu-id="2c253-359">アップロードした場合<em>アプリ\_offline.htm</em>使用しなければ、パブリッシュする前に、<strong>ファイル マネージャー</strong> Cytanium のコントロール パネルを削除するユーティリティ<em>アプリ\_オフライン</em>。htm テストする前にします。</span><span class="sxs-lookup"><span data-stu-id="2c253-359">If you uploaded <em>app\_offline.htm</em> before publishing, you have to use the <strong>File Manager</strong> utility in the Cytanium control panel to delete <em>app\_offline.</em>htm before you test.</span></span> <span data-ttu-id="2c253-360">削除することも同時に、 <em>.sdf</em>ファイルから、<em>アプリ\_データ</em>フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2c253-360">You can also at the same time delete the <em>.sdf</em> files from the <em>App\_Data</em> folder.</span></span>

<span data-ttu-id="2c253-361">ブラウザーを開き、アプリケーションをテストするには、テスト環境に配置した後と同様に、公開サイトの URL に移動できますようになりました。</span><span class="sxs-lookup"><span data-stu-id="2c253-361">You can now open a browser and go to the URL of your public site to test the application the same way you did after deploying to the test environment.</span></span>

## <a name="switching-to-sql-server-express-localdb-in-development"></a><span data-ttu-id="2c253-362">開発での SQL Server Express LocalDB への切り替え</span><span class="sxs-lookup"><span data-stu-id="2c253-362">Switching to SQL Server Express LocalDB in Development</span></span>

<span data-ttu-id="2c253-363">この概要で説明した、ようには、テストと運用環境で使用する開発で同じデータベース エンジンを使用する通常お勧めします。</span><span class="sxs-lookup"><span data-stu-id="2c253-363">As was explained in the Overview, it's generally best to use the same database engine in development that you use in test and production.</span></span> <span data-ttu-id="2c253-364">(開発で SQL Server Express を使用する利点は、データベースは、開発、テスト、および実稼働環境で同じに機能に注意してください)。このセクションでは、Visual Studio からアプリケーションを実行するときに、SQL Server Express LocalDB を使用する ContosoUniversity プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="2c253-364">(Remember that the advantage to using SQL Server Express in development is that the database will work the same in your development, test, and production environments.) In this section you'll set up the ContosoUniversity project to use SQL Server Express LocalDB when you run the application from Visual Studio.</span></span>

<span data-ttu-id="2c253-365">この移行を実行する最も簡単な方法は、Code First を使用して、メンバーシップ システムでは、両方の新しい開発データベースを作成できます。</span><span class="sxs-lookup"><span data-stu-id="2c253-365">The simplest way to perform this migration is to let Code First and the membership system create both new development databases for you.</span></span> <span data-ttu-id="2c253-366">このメソッドを使用して移行するには、3 つの手順が必要です。</span><span class="sxs-lookup"><span data-stu-id="2c253-366">Using this method to migrate requires three steps:</span></span>

1. <span data-ttu-id="2c253-367">新しい SQL Express LocalDB データベースを指定する接続文字列を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c253-367">Change the connection strings to specify new SQL Express LocalDB databases.</span></span>
2. <span data-ttu-id="2c253-368">管理者ユーザーを作成する Web サイト管理ツールを実行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-368">Run the Web Site Administration Tool to create an administrator user.</span></span> <span data-ttu-id="2c253-369">これは、メンバーシップ データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-369">This creates the membership database.</span></span>
3. <span data-ttu-id="2c253-370">作成し、アプリケーション データベースのシードには、Code First Migrations の update-database コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="2c253-370">Use the Code First Migrations update-database command to create and seed the application database.</span></span>

### <a name="updating-connection-strings-in-the-webconfig-file"></a><span data-ttu-id="2c253-371">Web.config ファイル内の接続文字列を更新しています</span><span class="sxs-lookup"><span data-stu-id="2c253-371">Updating Connection Strings in the Web.config file</span></span>

<span data-ttu-id="2c253-372">開く、 *Web.config*ファイルを開き、`connectionStrings`要素を次のコード。</span><span class="sxs-lookup"><span data-stu-id="2c253-372">Open the *Web.config* file and replace the `connectionStrings` element with the following code:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a><span data-ttu-id="2c253-373">メンバーシップ データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="2c253-373">Creating the Membership Database</span></span>

<span data-ttu-id="2c253-374">**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを選択し、クリックして**ASP.NET 構成**で、**プロジェクト**メニュー。</span><span class="sxs-lookup"><span data-stu-id="2c253-374">In **Solution Explorer**, select the ContosoUniversity project, and then click **ASP.NET Configuration** in the **Project** menu.</span></span>

<span data-ttu-id="2c253-375">[セキュリティ] タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="2c253-375">Select the Security tab.</span></span>

<span data-ttu-id="2c253-376">クリックして**管理ロールの作成または**、し、作成、**管理者**ロール。</span><span class="sxs-lookup"><span data-stu-id="2c253-376">Click **Create or Manage Roles**, and then create an **Administrator** role.</span></span>

<span data-ttu-id="2c253-377">[セキュリティ] タブに戻ります。</span><span class="sxs-lookup"><span data-stu-id="2c253-377">Return to the Security tab.</span></span>

<span data-ttu-id="2c253-378">をクリックして**ユーザーの作成**を選び、**管理者**チェック ボックスをオンし、管理者をという名前のユーザーを作成</span><span class="sxs-lookup"><span data-stu-id="2c253-378">Click **Create user**, and then select the **Administrator** check box and create a user named admin.</span></span>

<span data-ttu-id="2c253-379">閉じる、 **Web サイト管理ツール**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-379">Close the **Web Site Administration Tool**.</span></span>

### <a name="creating-the-school-database"></a><span data-ttu-id="2c253-380">School データベースの作成</span><span class="sxs-lookup"><span data-stu-id="2c253-380">Creating the School Database</span></span>

<span data-ttu-id="2c253-381">パッケージ マネージャー コンソール ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="2c253-381">Open the Package Manager Console window.</span></span>

<span data-ttu-id="2c253-382">**既定のプロジェクト**ドロップダウン リストで、ContosoUniversity.DAL プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="2c253-382">In the **Default project** drop-down list, select the ContosoUniversity.DAL project.</span></span>

<span data-ttu-id="2c253-383">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2c253-383">Enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

<span data-ttu-id="2c253-384">Code First Migrations は、データベースを作成し、AddBirthDate の移行を適用する初期移行を適用し、Seed メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-384">Code First Migrations applies the Initial migration that creates the database and then applies the AddBirthDate migration, then it runs the Seed method.</span></span>

<span data-ttu-id="2c253-385">コントロール f5 キーを押してサイトを実行します。</span><span class="sxs-lookup"><span data-stu-id="2c253-385">Run the site by pressing Control-F5.</span></span> <span data-ttu-id="2c253-386">テストおよび運用環境の場合と、実行、**受講者の追加** ページで、新しい生徒を追加して、新しい学生を表示し、**学生**ページ。</span><span class="sxs-lookup"><span data-stu-id="2c253-386">As you did for the test and production environments, run the **Add Students** page, add a new student, and then view the new student in the **Students** page.</span></span> <span data-ttu-id="2c253-387">これは、School データベースが作成され初期化された、およびその読み取りが書き込みアクセス権を確認します。</span><span class="sxs-lookup"><span data-stu-id="2c253-387">This verifies that the School database was created and initialized and that you have read and write access to it.</span></span>

<span data-ttu-id="2c253-388">選択、**更新クレジット**ページし、メンバーシップ データベースがデプロイされたことと、アクセス権があることを確認するにログインします。</span><span class="sxs-lookup"><span data-stu-id="2c253-388">Select the **Update Credits** page and log in to verify that the membership database was deployed and that you have access to it.</span></span> <span data-ttu-id="2c253-389">管理者アカウントを作成し、ユーザー アカウントを移行しなかった場合、**更新クレジット**ページの動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="2c253-389">If you did not migrate your user accounts, create an administrator account and then select the **Update Credits** page to verify that it works.</span></span>

## <a name="cleaning-up-sql-server-compact-files"></a><span data-ttu-id="2c253-390">SQL Server Compact ファイルのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="2c253-390">Cleaning Up SQL Server Compact Files</span></span>

<span data-ttu-id="2c253-391">ファイルと SQL Server Compact をサポートするために含まれていた NuGet パッケージが不要になった。</span><span class="sxs-lookup"><span data-stu-id="2c253-391">You no longer need files and NuGet packages that were included to support SQL Server Compact.</span></span> <span data-ttu-id="2c253-392">場合 (この手順が必要ない) 不要なファイルと参照をクリーンアップすることができます。</span><span class="sxs-lookup"><span data-stu-id="2c253-392">If you want (this step is not required), you can clean up unneeded files and references.</span></span>

<span data-ttu-id="2c253-393">**ソリューション エクスプ ローラー**、削除、 *.sdf*ファイルから、*アプリ\_データ*フォルダーと*amd64*と*x86*フォルダーから、 *bin*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2c253-393">In **Solution Explorer**, delete the *.sdf* files from the *App\_Data* folder and the *amd64* and *x86* folders from the *bin* folder.</span></span>

<span data-ttu-id="2c253-394">**ソリューション エクスプ ローラー**(いないプロジェクトの 1 つ、)、ソリューションを右クリックし、クリックして**ソリューションの NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-394">In **Solution Explorer**, right-click the solution (not one of the projects), and then click **Manage NuGet Packages for Solution**.</span></span>

<span data-ttu-id="2c253-395">左側のウィンドウで、 **NuGet パッケージの管理**ダイアログ ボックスで、**パッケージがインストールされている**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-395">In the left pane of the **Manage NuGet Packages** dialog box, select **Installed packages**.</span></span>

<span data-ttu-id="2c253-396">選択、 **EntityFramework.SqlServerCompact**パッケージ化し、をクリックして**管理**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-396">Select the **EntityFramework.SqlServerCompact** package and click **Manage**.</span></span>

<span data-ttu-id="2c253-397">**プロジェクトの選択**ダイアログ ボックスで、両方のプロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="2c253-397">In the **Select Projects** dialog box, both projects are selected.</span></span> <span data-ttu-id="2c253-398">両方のプロジェクトでパッケージをアンインストールするには、両方のチェック ボックスをオフにし、クリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="2c253-398">To uninstall the package in both projects, clear both check boxes, then click **OK**.</span></span>

<span data-ttu-id="2c253-399">場合も、依存パッケージをアンインストールするかを確認するダイアログ ボックスで、次のようにクリックします。 いいえ。</span><span class="sxs-lookup"><span data-stu-id="2c253-399">In the dialog box that asks if you want to uninstall the dependent packages also, click No.</span></span> <span data-ttu-id="2c253-400">1 つは、Entity Framework パッケージを保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c253-400">One of these is the Entity Framework package that you have to keep.</span></span>

<span data-ttu-id="2c253-401">アンインストールする同じ手順に従って、 **SqlServerCompact**パッケージ。</span><span class="sxs-lookup"><span data-stu-id="2c253-401">Follow the same procedure to uninstall the **SqlServerCompact** package.</span></span> <span data-ttu-id="2c253-402">(この順序で、パッケージをアンインストールする必要があります、 **EntityFramework.SqlServerCompact**パッケージによって異なります、 **SqlServerCompact**パッケージです)。</span><span class="sxs-lookup"><span data-stu-id="2c253-402">(The packages must be uninstalled in this order because the **EntityFramework.SqlServerCompact** package depends on the **SqlServerCompact** package.)</span></span>

<span data-ttu-id="2c253-403">SQL Server Express と完全な SQL Server に正常に移行したようになりました。</span><span class="sxs-lookup"><span data-stu-id="2c253-403">You have now successfully migrated to SQL Server Express and full SQL Server.</span></span> <span data-ttu-id="2c253-404">次のチュートリアルが別のデータベースの変更とすることになりますが、テストおよび運用データベースを SQL Server Express と完全な SQL Server を使用する場合は、データベースの変更をデプロイする方法表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c253-404">In the next tutorial you'll make another database change, and you'll see how to deploy database changes when your test and production databases use SQL Server Express and full SQL Server.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2c253-405">[前へ](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [次へ](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="2c253-405">[Previous](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[Next](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)</span></span>
