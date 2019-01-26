---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 'チュートリアル: EF Database First の MVC 5 の使用の概要します。'
description: この記事では方法から始める、既存データベースし、ユーザー データと対話できるようにする web アプリケーションをすばやく作成します。
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 8b094b7c334eaad510c46b55a99ec727b9c381c2
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889926"
---
# <a name="tutorial-get-started-with-ef-database-first-using-mvc-5"></a><span data-ttu-id="aa2a0-103">チュートリアル: EF Database First の MVC 5 の使用の概要します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-103">Tutorial: Get started with EF Database First using MVC 5</span></span>

<span data-ttu-id="aa2a0-104">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="aa2a0-105">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="aa2a0-106">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-106">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="aa2a0-107">シリーズの最後の部分では、Azure をサイトとデータベースをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-107">In the last part of the series, you will deploy the site and database to Azure.</span></span>

<span data-ttu-id="aa2a0-108">この記事では方法から始める、既存データベースし、ユーザー データと対話できるようにする web アプリケーションをすばやく作成します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-108">This article shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="aa2a0-109">Entity Framework 6 と MVC 5 web アプリケーションの構築に使用します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-109">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="aa2a0-110">ASP.NET のスキャフォールディング機能では、表示、更新、作成およびデータを削除するためのコードを自動的に生成することができます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-110">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="aa2a0-111">Visual Studio 内で発行ツールを使用することができます簡単に、サイトとデータベース Azure にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-111">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="aa2a0-112">シリーズのこの部分は、データベースを作成し、データを設定することについて説明します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-112">This part of the series focuses on creating a database and populating it with data.</span></span>

<span data-ttu-id="aa2a0-113">このシリーズは、Tom Dykstra と Rick Anderson の投稿で記述されています。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-113">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="aa2a0-114">[コメント] セクションのユーザーからのフィードバックに基づいて、改良されました。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-114">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="aa2a0-115">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-115">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="aa2a0-116">データベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-116">Set up the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa2a0-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="aa2a0-117">Prerequisites</span></span>

* [<span data-ttu-id="aa2a0-118">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="aa2a0-118">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="introduction"></a><span data-ttu-id="aa2a0-119">はじめに</span><span class="sxs-lookup"><span data-stu-id="aa2a0-119">Introduction</span></span>

<span data-ttu-id="aa2a0-120">この記事では、データベースがあり、そのデータベースのフィールドに基づく web アプリケーションのコードを生成するような状況を説明します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-120">This article addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="aa2a0-121">このアプローチには、Database First の開発が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-121">This approach is called Database First development.</span></span> <span data-ttu-id="aa2a0-122">既存のデータベースがあるまだない場合は、データ クラスを定義し、クラスのプロパティからデータベースを生成するには Code First の開発と呼ばれるアプローチを代わりに使用できます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-122">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="aa2a0-123">データベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-123">Set up the database</span></span>

<span data-ttu-id="aa2a0-124">場合、既存のデータベース環境を模倣するためには最初に自動的に入力データ、データベースが作成され、データベースに接続する web アプリケーションを作成し。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-124">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="aa2a0-125">このチュートリアルは、LocalDB を使用して開発されました。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-125">This tutorial was developed using LocalDB.</span></span> <span data-ttu-id="aa2a0-126">LocalDB は、代わりに既存のデータベース サーバーを使用することができますが、によって、バージョンの Visual Studio とデータベースの種類では、すべて Visual Studio でデータ ツールの可能性がありますがサポートされません。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-126">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="aa2a0-127">ツールが、データベースの利用できない場合は、データベースの管理スイートに含まれるデータベース固有の手順の一部を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-127">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="aa2a0-128">Visual Studio のバージョンのデータベース ツールの問題があれば、データベース ツールの最新バージョンをインストールしておくことを確認します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-128">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="aa2a0-129">更新またはデータベース ツールをインストールする方法については、次を参照してください。 [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027)します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-129">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="aa2a0-130">Visual Studio を起動し、作成、 **SQL Server データベース プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-130">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="aa2a0-131">プロジェクトに名前を**ContosoUniversityData**します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-131">Name the project **ContosoUniversityData**.</span></span>

![データベース プロジェクトを作成します。](setting-up-database/_static/image1.png)

<span data-ttu-id="aa2a0-133">空のデータベース プロジェクトがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-133">You now have an empty database project.</span></span> <span data-ttu-id="aa2a0-134">プロジェクトのターゲット プラットフォームとして Azure SQL Database を設定する必要があります、このチュートリアルで後で Azure には、このデータベースはデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-134">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="aa2a0-135">ターゲット プラットフォームの設定も、データベースは実際には配置しませんデータベースの設計が、ターゲット プラットフォームと互換性があるデータベース プロジェクトを確認するということです。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-135">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="aa2a0-136">ターゲット プラットフォームを設定するには、開く、**プロパティ**を選択してプロジェクト**Microsoft Azure SQL Database**のターゲット プラットフォーム。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-136">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

<span data-ttu-id="aa2a0-137">テーブルを定義する SQL スクリプトを追加することで、このチュートリアルに必要なテーブルを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-137">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="aa2a0-138">プロジェクトを右クリックし、新しい項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-138">Right-click your project and add a new item.</span></span> <span data-ttu-id="aa2a0-139">選択**テーブルとビュー** > **テーブル**名前を付けます*学生*します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-139">Select **Tables and Views** > **Table** and name it *Student*.</span></span>

<span data-ttu-id="aa2a0-140">テーブルのファイルでの T-SQL コマンドをテーブルを作成する次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-140">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="aa2a0-141">デザイン ウィンドウが、コードと自動的に同期することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-141">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="aa2a0-142">コードまたはデザイナーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-142">You can work with either the code or designer.</span></span>

![コードと設計を表示します。](setting-up-database/_static/image5.png)

<span data-ttu-id="aa2a0-144">別のテーブルを追加します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-144">Add another table.</span></span> <span data-ttu-id="aa2a0-145">この時点では、コースという名前を付けますし、次の T-SQL コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-145">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="aa2a0-146">1 つの登録をという名前のテーブルの作成に時間を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-146">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="aa2a0-147">データベースを展開した後に実行されるスクリプトを使用してデータを使用してデータベースを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-147">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="aa2a0-148">配置後スクリプトをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-148">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="aa2a0-149">プロジェクトを右クリックし、新しい項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-149">Right-click your project and add a new item.</span></span> <span data-ttu-id="aa2a0-150">選択**ユーザー スクリプト** > **配置後スクリプト**します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-150">Select **User Scripts** > **Post-Deployment Script**.</span></span> <span data-ttu-id="aa2a0-151">既定の名前を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-151">You can use the default name.</span></span>

<span data-ttu-id="aa2a0-152">次の T-SQL コードを配置後スクリプトに追加します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-152">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="aa2a0-153">このスクリプトは、一致するレコードが見つからない場合単データとデータベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-153">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="aa2a0-154">上書きしたり、データベースに入力したデータを削除しません。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-154">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="aa2a0-155">データベース プロジェクトをデプロイするたびに、配置後スクリプトが実行されるに注意してください。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-155">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="aa2a0-156">そのため、このスクリプトを記述するときに、要件を慎重に検討する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-156">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="aa2a0-157">場合によっては、プロジェクトを配置するたびに既知のデータ セットから開始する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-157">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="aa2a0-158">それ以外の場合、何らかの方法で既存のデータを変更することがありますしません。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-158">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="aa2a0-159">お客様の要件に基づき、配置後スクリプトまたはスクリプトに含める必要がある必要があるかどうかを決定できます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-159">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="aa2a0-160">配置後スクリプトを使用してデータベースの作成についての詳細については、次を参照してください。[などのデータを SQL Server データベース プロジェクトで](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-160">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="aa2a0-161">4 つの SQL スクリプト ファイルがない実際のテーブルがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-161">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="aa2a0-162">Localdb にデータベース プロジェクトをデプロイする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-162">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="aa2a0-163">Visual Studio で構築して、データベース プロジェクトをデプロイする [スタート] ボタン (または f5 キー) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-163">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="aa2a0-164">チェック、**出力** タブをビルド、配置が成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-164">Check the **Output** tab to verify that the build and deployment succeeded.</span></span>

<span data-ttu-id="aa2a0-165">新しいデータベースが作成されたことを表示する**SQL Server オブジェクト エクスプ ローラー**し、適切なローカル データベース サーバーで、プロジェクトの名前を探します (この場合 **(localdb) \ProjectsV13**)。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-165">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV13**).</span></span>

<span data-ttu-id="aa2a0-166">テーブルにデータが設定されているを表示するには、テーブルを右クリックして**ビュー データ**します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-166">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![テーブル データを表示します。](setting-up-database/_static/image9.png)

<span data-ttu-id="aa2a0-168">テーブル データの編集可能なビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-168">An editable view of the table data is displayed.</span></span> <span data-ttu-id="aa2a0-169">たとえば、選択した**テーブル** > **dbo.course** > **データの表示**、3 つの列を含むテーブルを参照してください (**コース**、**タイトル**、および**クレジット**) と 4 つの行。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-169">For example, if you select **Tables** > **dbo.course** > **View Data**, you see a table with three columns (**Course**, **Title**, and **Credits**) and four rows.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aa2a0-170">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="aa2a0-170">Additional resources</span></span>

<span data-ttu-id="aa2a0-171">Code First の開発の基本的な例を参照してください。 [ASP.NET MVC 5 の概要](../introduction/getting-started.md)します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-171">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="aa2a0-172">高度な例では、次を参照してください。 [ASP.NET MVC 4 アプリケーションの Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-172">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="aa2a0-173">Entity Framework を使用する方法の選択に関するガイダンスについては、次を参照してください。 [Entity Framework 開発方法](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-173">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa2a0-174">次の手順</span><span class="sxs-lookup"><span data-stu-id="aa2a0-174">Next steps</span></span>

<span data-ttu-id="aa2a0-175">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-175">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="aa2a0-176">データベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-176">Set up the database</span></span>

<span data-ttu-id="aa2a0-177">Web アプリケーションとデータ モデルを作成する方法については、次の記事に進んでください。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-177">Advance to the next article to learn how to create the web application and data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="aa2a0-178">Web アプリケーションとデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="aa2a0-178">Create the web application and data models</span></span>](creating-the-web-application.md)