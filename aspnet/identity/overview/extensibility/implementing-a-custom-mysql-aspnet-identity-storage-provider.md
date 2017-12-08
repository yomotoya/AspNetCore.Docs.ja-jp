---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: "カスタムの MySQL ASP.NET Identity の記憶域プロバイダーを実装する |Microsoft ドキュメント"
author: raquelsa
description: "ASP.NET Identity は、独自の記憶域プロバイダーを作成し、アプリケーションを再動作せず、アプリケーションに接続することにより、拡張可能なシステムがしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 3bfbccd91705755fc24bb8305fff171baa26f370
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a><span data-ttu-id="8e71a-103">カスタムの MySQL ASP.NET Identity の記憶域プロバイダーの実装</span><span class="sxs-lookup"><span data-stu-id="8e71a-103">Implementing a Custom MySQL ASP.NET Identity Storage Provider</span></span>
====================
<span data-ttu-id="8e71a-104">によって[Raquel Soares De Almeida](https://github.com/raquelsa)、 [Suhas Joshi](https://github.com/suhasj)、 [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8e71a-104">by [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8e71a-105">ASP.NET Identity は、拡張可能なシステムを独自の記憶域プロバイダーを作成し、アプリケーションを再動作せず、アプリケーションに接続することができます。</span><span class="sxs-lookup"><span data-stu-id="8e71a-105">ASP.NET Identity is an extensible system which enables you to create your own storage provider and plug it into your application without re-working the application.</span></span> <span data-ttu-id="8e71a-106">このトピックでは、ASP.NET Identity の MySQL 記憶域プロバイダーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-106">This topic describes how to create a MySQL storage provider for ASP.NET Identity.</span></span> <span data-ttu-id="8e71a-107">カスタムの記憶域プロバイダーの作成の概要については、次を参照してください。[概要のカスタムの記憶域プロバイダー ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-107">For an overview of creating custom storage providers, see [Overview of Custom Storage Providers for ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).</span></span>
> 
> <span data-ttu-id="8e71a-108">このチュートリアルを完了するには、Visual Studio 2013 Update 2 が必要です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-108">To complete this tutorial, you must have Visual Studio 2013 with Update 2.</span></span>
> 
> <span data-ttu-id="8e71a-109">このチュートリアルを行います。</span><span class="sxs-lookup"><span data-stu-id="8e71a-109">This tutorial will:</span></span>
> 
> - <span data-ttu-id="8e71a-110">Azure の MySQL データベース インスタンスを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-110">Show how to create a MySQL database instance on Azure.</span></span>
> - <span data-ttu-id="8e71a-111">MySQL クライアント ツール (MySQL ワークベンチ) を使用してテーブルを作成し、Azure でリモート データベースを管理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-111">Show how to use a MySQL client tool (MySQL Workbench) to create tables and manage your remote database on Azure.</span></span>
> - <span data-ttu-id="8e71a-112">MVC アプリケーション プロジェクトのカスタム実装を既定の ASP.NET Identity の記憶域の実装を置換する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-112">Show how to replace the default ASP.NET Identity storage implementation with our custom implementation on a MVC application project.</span></span>
> 
> <span data-ttu-id="8e71a-113">このチュートリアルは Raquel Soares De Almeida および Rick Anderson によって書き込まれて ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="8e71a-113">This tutorial was originally written by Raquel Soares De Almeida and Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).</span></span> <span data-ttu-id="8e71a-114">サンプル プロジェクトは、Identity 2.0 Suhas Joshi によって更新されました。</span><span class="sxs-lookup"><span data-stu-id="8e71a-114">The sample project was updated for Identity 2.0 by Suhas Joshi.</span></span> <span data-ttu-id="8e71a-115">トピックは、Tom FitzMacken によって Identity 2.0 用に更新されました。</span><span class="sxs-lookup"><span data-stu-id="8e71a-115">The topic was updated for Identity 2.0 by Tom FitzMacken.</span></span>


## <a name="download-completed-project"></a><span data-ttu-id="8e71a-116">ダウンロードが完了したプロジェクト</span><span class="sxs-lookup"><span data-stu-id="8e71a-116">Download completed project</span></span>

<span data-ttu-id="8e71a-117">このチュートリアルの最後に、MVC アプリケーション プロジェクトを Azure でホストされている MySQL データベースを使用する ASP.NET Id を持つがあります。</span><span class="sxs-lookup"><span data-stu-id="8e71a-117">At the end of this tutorial, you will have an MVC application project with ASP.NET Identity working with a MySQL database hosted on Azure.</span></span>

<span data-ttu-id="8e71a-118">完了した MySQL の記憶域プロバイダーをダウンロードする[AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-118">You can download the completed MySQL storage provider at [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).</span></span>

## <a name="the-steps-you-will-perform"></a><span data-ttu-id="8e71a-119">実行する手順</span><span class="sxs-lookup"><span data-stu-id="8e71a-119">The steps you will perform</span></span>

<span data-ttu-id="8e71a-120">このチュートリアルでは説明します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-120">In this tutorial you will:</span></span>

1. <span data-ttu-id="8e71a-121">Azure の MySQL データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-121">Create a MySQL database on Azure</span></span>
2. <span data-ttu-id="8e71a-122">MySQL の ASP.NET Identity テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-122">Create the ASP.NET Identity tables in MySQL</span></span>
3. <span data-ttu-id="8e71a-123">MVC アプリケーションを作成し、MySQL プロバイダーを使用するように構成</span><span class="sxs-lookup"><span data-stu-id="8e71a-123">Create an MVC application and configure it to use the MySQL provider</span></span>
4. <span data-ttu-id="8e71a-124">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="8e71a-124">Run the app</span></span>

<span data-ttu-id="8e71a-125">このトピックでは、ASP.NET の Id と顧客の記憶域プロバイダーを実装する際に、決定のアーキテクチャは説明しません。</span><span class="sxs-lookup"><span data-stu-id="8e71a-125">This topic does not cover the architecture of ASP.NET Identity and the decisions you must make when implementing a customer storage provider.</span></span> <span data-ttu-id="8e71a-126">その情報を参照してください。[概要のカスタムの記憶域プロバイダー ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-126">For that information, see [Overview of Custom Storage Providers for ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).</span></span>

## <a name="review-mysql-storage-provider-classes"></a><span data-ttu-id="8e71a-127">MySQL の記憶域プロバイダーのクラスを確認します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-127">Review MySQL storage provider classes</span></span>

<span data-ttu-id="8e71a-128">MySQL の記憶域プロバイダーを作成するステップにジャンプ、前に、記憶域プロバイダーを構成するクラスを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="8e71a-128">Before jumping into the steps to create the MySQL storage provider, let's look at the classes that make up the storage provider.</span></span> <span data-ttu-id="8e71a-129">データベース操作を管理するクラスおよびクラスのユーザーおよびロールを管理するアプリケーションから呼び出される必要があります。</span><span class="sxs-lookup"><span data-stu-id="8e71a-129">You will need classes that manage the database operations and classes that are called from the application to manage users and roles.</span></span>

### <a name="storage-classes"></a><span data-ttu-id="8e71a-130">ストレージ クラス</span><span class="sxs-lookup"><span data-stu-id="8e71a-130">Storage classes</span></span>

- <span data-ttu-id="8e71a-131">[IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -ユーザーのプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-131">[IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) - contains properties for the user.</span></span>
- <span data-ttu-id="8e71a-132">[UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -追加、更新、またはユーザーを取得するための操作が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-132">[UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) - contains operations for adding, updating or retrieving users.</span></span>
- <span data-ttu-id="8e71a-133">[IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -ロールのプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-133">[IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) - contains properties for roles.</span></span>
- <span data-ttu-id="8e71a-134">[RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -追加、削除、更新、およびロールを取得するための操作が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-134">[RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) - contains operations for adding, deleting, updating and retrieving roles.</span></span>

### <a name="data-access-layer-classes"></a><span data-ttu-id="8e71a-135">データ アクセス レイヤーのクラス</span><span class="sxs-lookup"><span data-stu-id="8e71a-135">Data access layer classes</span></span>

<span data-ttu-id="8e71a-136">この例では、データ アクセス レイヤーのクラスが、テーブルを操作するための SQL ステートメントを含めるただし、コードは、Entity Framework や NHibernate などオブジェクト リレーショナル マッピング (ORM) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8e71a-136">For this example, the data access layer classes contain SQL statements for working with the tables; however, in your code you might want to use object-relational mapping (ORM) such as Entity Framework or NHibernate.</span></span> <span data-ttu-id="8e71a-137">具体的には、アプリケーションは、遅延読み込みとキャッシュのオブジェクトを含む ORM せずパフォーマンスの低下を発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8e71a-137">In particular, your application may experience poor performance without an ORM that includes lazy loading and object caching.</span></span> <span data-ttu-id="8e71a-138">詳細については、次を参照してください[Entity Framework せず ASP.NET Identity 2.0 しますか?。](https://aspnetidentity.codeplex.com/discussions/561828)</span><span class="sxs-lookup"><span data-stu-id="8e71a-138">For more information, see [ASP.NET Identity 2.0 without Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)</span></span>

- <span data-ttu-id="8e71a-139">[MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -MySQL データベースの接続とデータベースの操作を実行するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-139">[MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) - contains the MySQL database connection and methods for performing database operations.</span></span> <span data-ttu-id="8e71a-140">UserStore と RoleStore このクラスのインスタンスとインスタンスします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-140">UserStore and RoleStore are both instantiated with an instance of this class.</span></span>
- <span data-ttu-id="8e71a-141">[RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -ロールを格納するテーブルのデータベース操作が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-141">[RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) - contains database operations for the table that stores roles.</span></span>
- <span data-ttu-id="8e71a-142">[UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -ユーザーの信頼性情報を格納するテーブルのデータベース操作が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-142">[UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) - contains database operations for the table that stores user claims.</span></span>
- <span data-ttu-id="8e71a-143">[UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -ユーザーのログイン情報を格納するテーブルのデータベース操作が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-143">[UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) - contains database operations for the table that stores user login information.</span></span>
- <span data-ttu-id="8e71a-144">[UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -ユーザーがどのロールに割り当てられたを格納するテーブルのデータベース操作が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-144">[UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) - contains database operations for the table that stores which users are assigned to which roles.</span></span>
- <span data-ttu-id="8e71a-145">[UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -ユーザーを格納するテーブルのデータベース操作が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-145">[UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) - contains database operations for the table that stores users.</span></span>

## <a name="create-a-mysql-database-instance-on-azure"></a><span data-ttu-id="8e71a-146">Azure の MySQL データベース インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-146">Create a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="8e71a-147">ログインに、 [Azure ポータル](https://manage.windowsazure.com/)です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-147">Log in to the [Azure Portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="8e71a-148">をクリックして**+ 新規**クリックし、ページの下部にある**ストア**です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-148">Click **+NEW** at the bottom of the page, and then select **STORE**.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. <span data-ttu-id="8e71a-149">**選択してアドオン**ウィザードで、 **ClearDB MySQL データベース** ダイアログ ボックスの右下にある次へ進む矢印をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-149">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database** and click on the next arrow at the bottom right of the dialog.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. <span data-ttu-id="8e71a-150">既定値を保持**空き**計画し、変更、**名前**に**IdentityMySQLDatabase**です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-150">Keep the default **Free** plan and change the **Name** to **IdentityMySQLDatabase**.</span></span> <span data-ttu-id="8e71a-151">一番近い地域を選択し、[次へ] 矢印をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-151">Select the region nearest you and then click the next arrow.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. <span data-ttu-id="8e71a-152">データベースの作成を完了するチェック マークをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-152">Click the checkmark to complete the database creation.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. <span data-ttu-id="8e71a-153">データベースが作成された後をから管理できる、**アドオン** タブで、管理ポータル。</span><span class="sxs-lookup"><span data-stu-id="8e71a-153">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span>   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. <span data-ttu-id="8e71a-154">をクリックして、データベース接続情報を取得できます**接続情報**ページの下部にあります。</span><span class="sxs-lookup"><span data-stu-id="8e71a-154">You can get the database connection information by clicking on **CONNECTION INFO** at the bottom of the page.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. <span data-ttu-id="8e71a-155">[コピー] ボタンをクリックすると、接続文字列をコピーし、後で、MVC アプリケーションで使用できるように保存します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-155">Copy the connection string by clicking on the copy button and save it so you can use later in your MVC application.</span></span>   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a><span data-ttu-id="8e71a-156">ASP.NET Identity に MySQL データベース内のテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-156">Create the ASP.NET Identity tables in a MySQL database</span></span>

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a><span data-ttu-id="8e71a-157">接続して MySQL データベースの管理の MySQL Workbench ツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-157">Install MySQL Workbench tool to connect and manage MySQL database</span></span>

1. <span data-ttu-id="8e71a-158">インストール、 **MySQL Workbench**ツールから、 [MySQL がダウンロード ページ](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="8e71a-158">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="8e71a-159">アプリを起動し、をクリックを追加、 **MySQLConnections +**ボタンをクリックして新しい接続を追加します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-159">Launch the app and add click on the **MySQLConnections +** button to add a new connection.</span></span> <span data-ttu-id="8e71a-160">このチュートリアルで先ほど作成した Azure の MySQL データベースからコピーした接続文字列データを使用します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-160">Use the connection string data you copied from the Azure MySQL database you created earlier in this tutorial.</span></span>
3. <span data-ttu-id="8e71a-161">接続を確立した後、新しく開きます**クエリ**タブ; からのコマンドを貼り付ける[MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql)クエリにし、データベース テーブルを作成するために実行します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-161">After establishing the connection, open a new **Query** tab; paste the commands from [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) into the query and execute it in order to create the database tables.</span></span>
4. <span data-ttu-id="8e71a-162">次に示すように、Azure でホストされている MySQL データベースで作成されるすべての ASP.NET Identity 必要なテーブルがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="8e71a-162">You now have all the ASP.NET Identity necessary tables created on a MySQL database hosted on Azure as shown below.</span></span>  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a><span data-ttu-id="8e71a-163">MVC アプリケーション プロジェクト テンプレートから作成し、MySQL プロバイダーを使用するように構成</span><span class="sxs-lookup"><span data-stu-id="8e71a-163">Create an MVC application project from template and configure it to use MySQL provider</span></span>

<span data-ttu-id="8e71a-164">必要な場合、インストールするか[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) Update 2 です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-164">If needed, install either [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) with Update 2.</span></span>

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a><span data-ttu-id="8e71a-165">CodePlex から ASP.NET.Identity.MySQL プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-165">Download the ASP.NET.Identity.MySQL project from CodePlex</span></span>

1. <span data-ttu-id="8e71a-166">リポジトリの URL を参照[AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/)です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-166">Browse to the repository URL at [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).</span></span>
2. <span data-ttu-id="8e71a-167">ソース コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-167">Download the source code.</span></span>
3. <span data-ttu-id="8e71a-168">ローカル フォルダーに .zip ファイルを抽出します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-168">Extract the .zip file into a local folder.</span></span>
4. <span data-ttu-id="8e71a-169">AspNet.Identity.MySQL ソリューションを開き、それをビルドします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-169">Open the AspNet.Identity.MySQL solution and build it.</span></span>

### <a name="create-a-new-mvc-application-project-from-template"></a><span data-ttu-id="8e71a-170">テンプレートから新しい MVC アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-170">Create a new MVC application project from template</span></span>

1. <span data-ttu-id="8e71a-171">右クリックして、 **AspNet.Identity.MySQL**ソリューションと**追加**、**新しいプロジェクト**</span><span class="sxs-lookup"><span data-stu-id="8e71a-171">Right click the **AspNet.Identity.MySQL** solution and **Add**, **New Project**</span></span>
2. <span data-ttu-id="8e71a-172">**新しいプロジェクトの追加**ダイアログの  **Visual c#**し、左側の**Web**し、 **ASP.NET Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-172">In the **Add New Project** Dialog select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="8e71a-173">プロジェクトの名前を付けます**IdentityMySQLDemo**; [ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-173">Name your project **IdentityMySQLDemo**; and then click OK.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. <span data-ttu-id="8e71a-174">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、既定のオプションで MVC テンプレートを選択 (が含まれている**個々 のユーザー アカウント**認証方法として) をクリック**OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)</span><span class="sxs-lookup"><span data-stu-id="8e71a-174">In the **New ASP.NET Project** dialog, select the MVC template with the default options (that includes **Individual User Accounts** as authentication method) and click **OK**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)</span></span>
4. <span data-ttu-id="8e71a-175">ソリューション エクスプ ローラーで、IdentityMySQLDemo プロジェクトを右クリックして**NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-175">In Solution Explorer, right-click your IdentityMySQLDemo project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="8e71a-176">検索テキスト ボックス ダイアログ ボックスで次のように入力します。 **Identity.EntityFramework**です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-176">In the search text box dialog, type **Identity.EntityFramework**.</span></span> <span data-ttu-id="8e71a-177">結果の一覧で、このパッケージを選択し、をクリックして**アンインストール**です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-177">Select this package in the list of results and click **Uninstall**.</span></span> <span data-ttu-id="8e71a-178">依存関係パッケージ EntityFramework をアンインストールするように促されます。</span><span class="sxs-lookup"><span data-stu-id="8e71a-178">You will be prompted to uninstall the dependency package EntityFramework.</span></span> <span data-ttu-id="8e71a-179">このアプリケーションには、このパッケージでは不要になったように [はい] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8e71a-179">Click on Yes as we will no longer this package on this application.</span></span>
5. <span data-ttu-id="8e71a-180">IdentityMySQLDemo プロジェクトを右クリックしてで、選択**追加**、**参照、ソリューション、プロジェクト;** AspNet.Identity.MySQL プロジェクトを選択し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-180">Right click the IdentityMySQLDemo project, select **Add**, **Reference, Solution, Projects;** select the AspNet.Identity.MySQL project and click **OK**.</span></span>
6. <span data-ttu-id="8e71a-181">IdentityMySQLDemo プロジェクトに対するすべての参照を置き換える</span><span class="sxs-lookup"><span data-stu-id="8e71a-181">In the IdentityMySQLDemo project, replace all references to</span></span>  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
 <span data-ttu-id="8e71a-182">代入</span><span class="sxs-lookup"><span data-stu-id="8e71a-182">with</span></span>  
     `using AspNet.Identity.MySQL;`
7. <span data-ttu-id="8e71a-183">IdentityModels.cs で次のように設定します。 **ApplicationDbContext**から派生する**MySqlDatabase**接続名に 1 つのパラメーターを受け取るコンス トラクターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e71a-183">In IdentityModels.cs, set **ApplicationDbContext** to derive from **MySqlDatabase** and include a contructor that take a single parameter with the connection name.</span></span>  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. <span data-ttu-id="8e71a-184">IdentityConfig.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8e71a-184">Open the IdentityConfig.cs file.</span></span> <span data-ttu-id="8e71a-185">**ApplicationUserManager.Create**メソッドは、次のコードで UserManager をインスタンス化する置換します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-185">In the **ApplicationUserManager.Create** method, replace instantiating UserManager with the following code:</span></span>  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. <span data-ttu-id="8e71a-186">Web.config ファイルを開き、このエントリを強調表示された値を置き換える前の手順で作成した MySQL データベースの接続文字列に DefaultConnection 文字列を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8e71a-186">Open the web.config file and replace the DefaultConnection string with this entry replacing the highlighted values with the connection string of the MySQL database you created on previous steps:</span></span>  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a><span data-ttu-id="8e71a-187">アプリを実行して、MySQL データベースへの接続</span><span class="sxs-lookup"><span data-stu-id="8e71a-187">Run the app and connect to the MySQL DB</span></span>

1. <span data-ttu-id="8e71a-188">右クリックして、 **IdentityMySQLDemo**プロジェクトし、選択**スタートアップ プロジェクトとして設定**</span><span class="sxs-lookup"><span data-stu-id="8e71a-188">Right click the **IdentityMySQLDemo** project and select **Set as Startup Project**</span></span>
2. <span data-ttu-id="8e71a-189">キーを押して**ctrl キーを押しながら f5 キーを押して**アプリをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-189">Press **Ctrl + F5** to build and run the app.</span></span>
3. <span data-ttu-id="8e71a-190">をクリックして**登録**ページの上部のタブです。</span><span class="sxs-lookup"><span data-stu-id="8e71a-190">Click on **Register** tab on the top of the page.</span></span>
4. <span data-ttu-id="8e71a-191">新しいユーザー名とパスワードを入力し、をクリックして**登録**です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-191">Enter a new user name and password and then click on **Register**.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. <span data-ttu-id="8e71a-192">今すぐ新しいユーザーが登録されログに記録します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-192">The new user is now registered and logged in.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. <span data-ttu-id="8e71a-193">MySQL Workbench ツールに戻るし、検査、 **IdentityMySQLDatabase**テーブルの内容。</span><span class="sxs-lookup"><span data-stu-id="8e71a-193">Go back to the MySQL Workbench tool and inspect the **IdentityMySQLDatabase** table's contents.</span></span> <span data-ttu-id="8e71a-194">新しいユーザーを登録すると、ユーザー テーブルのエントリを検査します。</span><span class="sxs-lookup"><span data-stu-id="8e71a-194">Inspect the users table for the entries as you register new users.</span></span>  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a><span data-ttu-id="8e71a-195">次の手順</span><span class="sxs-lookup"><span data-stu-id="8e71a-195">Next Steps</span></span>

<span data-ttu-id="8e71a-196">このアプリで他の認証方法を有効にする方法の詳細についてを参照してください[Facebook、Google OAuth2 および OpenID サイン オンで、ASP.NET MVC 5 アプリを作成する](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-196">For more information on how to enable other authentication methods on this app, refer to [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<span data-ttu-id="8e71a-197">OAuth と、データベースを統合して、アプリへのユーザー アクセスを制限するロールを設定する方法についてを参照してください。[メンバーシップ、OAuth、SQL データベースでの ASP.NET MVC 5 のセキュリティで保護されたアプリケーションを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。</span><span class="sxs-lookup"><span data-stu-id="8e71a-197">To learn how to integrate your DB with OAuth and to set up roles to limit users access to your app, see [Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>
