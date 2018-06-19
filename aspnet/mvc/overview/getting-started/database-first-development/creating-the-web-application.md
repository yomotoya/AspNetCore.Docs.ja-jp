---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'ASP.NET MVC で最初の EF データベース: Web アプリケーションとデータ モデルの作成 |Microsoft ドキュメント'
author: tfitzmac
description: MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 04ccc00fa48702608fdc7b5b00d73778985852f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878777"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="24d42-104">ASP.NET MVC で最初の EF データベース: Web アプリケーションとデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="24d42-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="24d42-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="24d42-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="24d42-106">MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="24d42-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="24d42-107">このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="24d42-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="24d42-108">生成されたコードは、データベース テーブルの列に対応します。</span><span class="sxs-lookup"><span data-stu-id="24d42-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="24d42-109">系列のこの部分は、web アプリケーションを作成して、データベース テーブルに基づくデータ モデルの生成について説明します。</span><span class="sxs-lookup"><span data-stu-id="24d42-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="24d42-110">新しい ASP.NET Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="24d42-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="24d42-111">新しいソリューションまたはデータベース プロジェクトと同じソリューションで、Visual Studio で新しいプロジェクトを作成し、選択、 **ASP.NET Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="24d42-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="24d42-112">プロジェクトに名前を**ContosoSite**です。</span><span class="sxs-lookup"><span data-stu-id="24d42-112">Name the project **ContosoSite**.</span></span>

![プロジェクトを作成します。](creating-the-web-application/_static/image1.png)

<span data-ttu-id="24d42-114">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="24d42-114">Click **OK**.</span></span>

<span data-ttu-id="24d42-115">新しい ASP.NET プロジェクト ウィンドウで、選択、 **MVC**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="24d42-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="24d42-116">オフにすることができます、**クラウド内のホスト**後でアプリケーションをクラウドに展開するために、ここではオプションです。</span><span class="sxs-lookup"><span data-stu-id="24d42-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="24d42-117">をクリックして**OK**アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="24d42-117">Click **OK** to create the application.</span></span>

![mvc テンプレートを選択します。](creating-the-web-application/_static/image2.png)

<span data-ttu-id="24d42-119">プロジェクトには、既定のファイルとフォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="24d42-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="24d42-120">このチュートリアルでは、Entity Framework 6 を使用します。</span><span class="sxs-lookup"><span data-stu-id="24d42-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="24d42-121">[NuGet パッケージの管理] ウィンドウから、プロジェクトの Entity Framework のバージョンを再確認してくださいことができます。</span><span class="sxs-lookup"><span data-stu-id="24d42-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="24d42-122">必要に応じて、Entity Framework のバージョンを更新します。</span><span class="sxs-lookup"><span data-stu-id="24d42-122">If necessary, update your version of Entity Framework.</span></span>

![バージョンを表示します。](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="24d42-124">モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="24d42-124">Generate the models</span></span>

<span data-ttu-id="24d42-125">Entity Framework モデルをデータベース テーブルから作成されます。</span><span class="sxs-lookup"><span data-stu-id="24d42-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="24d42-126">これらのモデルは、データを操作に使用するクラスです。</span><span class="sxs-lookup"><span data-stu-id="24d42-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="24d42-127">各モデルでは、ミラー化データベースのテーブルと、テーブル内の列に対応するプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="24d42-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="24d42-128">右クリックし、**モデル**フォルダー、および選択**追加**と**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="24d42-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![新しい項目を追加します。](creating-the-web-application/_static/image4.png)

<span data-ttu-id="24d42-130">[新しい項目の追加] ウィンドウで、次のように選択します。**データ**左側のウィンドウで、 **ADO.NET エンティティ データ モデル**中央のペインにあるオプションからです。</span><span class="sxs-lookup"><span data-stu-id="24d42-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="24d42-131">新しいモデルのファイルの名前を付けます**ContosoModel**です。</span><span class="sxs-lookup"><span data-stu-id="24d42-131">Name the new model file **ContosoModel**.</span></span>

![モデルを作成します。](creating-the-web-application/_static/image5.png)

<span data-ttu-id="24d42-133">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="24d42-133">Click **Add**.</span></span>

<span data-ttu-id="24d42-134">エンティティ データ モデル ウィザードで、次のように選択します。**データベースから EF Designer**です。</span><span class="sxs-lookup"><span data-stu-id="24d42-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![データベースから生成します。](creating-the-web-application/_static/image6.png)

<span data-ttu-id="24d42-136">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="24d42-136">Click **Next**.</span></span>

<span data-ttu-id="24d42-137">場合は、開発環境内で定義されているデータベースの接続がある場合は、事前に選択したこれらの接続のいずれかを参照してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="24d42-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="24d42-138">ただし、このチュートリアルの最初の部分で作成したデータベースへの新しい接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="24d42-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="24d42-139">クリックして、**新しい接続**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="24d42-139">Click the **New Connection** button.</span></span>

![データベースへの接続します。](creating-the-web-application/_static/image7.png)

<span data-ttu-id="24d42-141">接続のプロパティ ウィンドウで作成されたデータベースのローカル サーバーの名前を指定します (ここでは **(localdb) \ProjectsV12**)。</span><span class="sxs-lookup"><span data-stu-id="24d42-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="24d42-142">サーバー名を入力すた後には、使用可能なデータベースから、ContosoUniversityData を選択します。</span><span class="sxs-lookup"><span data-stu-id="24d42-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![接続プロパティの設定](creating-the-web-application/_static/image8.png)

<span data-ttu-id="24d42-144">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="24d42-144">Click **OK**.</span></span>

<span data-ttu-id="24d42-145">正しい接続プロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="24d42-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="24d42-146">Web.Config ファイルで接続の既定の名前を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="24d42-146">You can use the default name for connection in the Web.Config file</span></span>

![接続の設定](creating-the-web-application/_static/image9.png)

<span data-ttu-id="24d42-148">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="24d42-148">Click **Next**.</span></span>

<span data-ttu-id="24d42-149">選択**テーブル**をすべて次の 3 つのテーブルのモデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="24d42-149">Select **Tables** to generate models for all three tables.</span></span>

![テーブルを選択します。](creating-the-web-application/_static/image10.png)

<span data-ttu-id="24d42-151">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="24d42-151">Click **Finish**.</span></span>

<span data-ttu-id="24d42-152">セキュリティの警告を受信する場合は、選択**OK**テンプレートの実行を続行します。</span><span class="sxs-lookup"><span data-stu-id="24d42-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="24d42-153">モデルは、データベースのテーブルから生成され、プロパティと、テーブル間のリレーションシップを表示する図が表示されます。</span><span class="sxs-lookup"><span data-stu-id="24d42-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![モデルのダイアグラム](creating-the-web-application/_static/image11.png)

<span data-ttu-id="24d42-155">Models フォルダーには、データベースから生成されたモデルに関連する多くの新しいファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="24d42-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![新しいモデル ファイルを表示します。](creating-the-web-application/_static/image12.png)

<span data-ttu-id="24d42-157">**ContosoModel.Context.cs**ファイルにはから派生するクラスが含まれています、 **DbContext**クラス、し、データベース テーブルに対応する各モデル クラスのプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="24d42-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="24d42-158">**Course.cs**、 **Enrollment.cs**、および**Student.cs**ファイルには、データベースのテーブルを表すモデル クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="24d42-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="24d42-159">スキャフォールディングを使用する場合は、モデルのクラスとコンテキスト クラスの両方を使用します。</span><span class="sxs-lookup"><span data-stu-id="24d42-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="24d42-160">このチュートリアルの前に、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="24d42-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="24d42-161">次のセクションでは、プロジェクトがビルドされていない場合、セクションでは機能しませんが、そのデータ モデルに基づいたコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="24d42-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="24d42-162">[前へ](setting-up-database.md)
> [次へ](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="24d42-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
