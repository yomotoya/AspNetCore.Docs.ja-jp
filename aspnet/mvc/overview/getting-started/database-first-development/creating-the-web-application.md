---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF Database First と ASP.NET MVC: Web アプリケーションとデータ モデルの作成 |Microsoft Docs'
author: tfitzmac
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: riande
ms.date: 10/01/2014
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 343131d45ed0b2442f1b0b557c5b63f3877e5d0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833573"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="fcf8b-104">EF Database First と ASP.NET MVC: Web アプリケーションとデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="fcf8b-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fcf8b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fcf8b-106">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="fcf8b-107">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="fcf8b-108">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="fcf8b-109">シリーズのこの部分は、web アプリケーションを作成して、データベース テーブルに基づくデータ モデルの生成に焦点を当てます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="fcf8b-110">新しい ASP.NET Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="fcf8b-111">新しいソリューションまたはデータベース プロジェクトと同じソリューションで、Visual Studio で新しいプロジェクトを作成し、 **ASP.NET Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="fcf8b-112">プロジェクトに名前を**ContosoSite**します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-112">Name the project **ContosoSite**.</span></span>

![プロジェクトを作成します。](creating-the-web-application/_static/image1.png)

<span data-ttu-id="fcf8b-114">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-114">Click **OK**.</span></span>

<span data-ttu-id="fcf8b-115">新しい ASP.NET プロジェクト ウィンドウで、選択、 **MVC**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="fcf8b-116">オフにすることができます、**クラウドでホスト**は後で、クラウドにアプリケーションを展開するために、ここではオプションです。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="fcf8b-117">クリックして**OK**アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-117">Click **OK** to create the application.</span></span>

![mvc テンプレートを選択します。](creating-the-web-application/_static/image2.png)

<span data-ttu-id="fcf8b-119">既定のファイルとフォルダー、プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="fcf8b-120">このチュートリアルでは、Entity Framework 6 を使用します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="fcf8b-121">NuGet パッケージの管理 ウィンドウを使用してプロジェクトで Entity Framework のバージョンを再確認することができます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="fcf8b-122">必要に応じて、Entity Framework のバージョンを更新します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-122">If necessary, update your version of Entity Framework.</span></span>

![バージョンを表示します。](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="fcf8b-124">モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-124">Generate the models</span></span>

<span data-ttu-id="fcf8b-125">Entity Framework モデルをデータベース テーブルから作成されます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="fcf8b-126">これらのモデルは、データを操作に使用するクラスです。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="fcf8b-127">各モデルでは、ミラー データベースのテーブルとテーブルの列に対応するプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="fcf8b-128">右クリックし、**モデル**フォルダー、および選択**追加**と**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![新しい項目を追加します。](creating-the-web-application/_static/image4.png)

<span data-ttu-id="fcf8b-130">新しい項目の追加 ウィンドウで、次のように選択します。**データ**左側のウィンドウで、 **ADO.NET Entity Data Model**から中央のウィンドウのオプション。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="fcf8b-131">新しいモデル ファイルに名前**ContosoModel**します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-131">Name the new model file **ContosoModel**.</span></span>

![モデルを作成します。](creating-the-web-application/_static/image5.png)

<span data-ttu-id="fcf8b-133">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-133">Click **Add**.</span></span>

<span data-ttu-id="fcf8b-134">Entity Data Model ウィザード、選択**データベースの EF デザイナー**します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![データベースから生成します。](creating-the-web-application/_static/image6.png)

<span data-ttu-id="fcf8b-136">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-136">Click **Next**.</span></span>

<span data-ttu-id="fcf8b-137">開発環境内で定義されているデータベースの接続があれば、事前選択されているこれらの接続のいずれかを表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="fcf8b-138">ただし、このチュートリアルの最初の部分で作成したデータベースへの新しい接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="fcf8b-139">をクリックして、**新しい接続**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-139">Click the **New Connection** button.</span></span>

![データベースへの接続します。](creating-the-web-application/_static/image7.png)

<span data-ttu-id="fcf8b-141">接続のプロパティ ウィンドウで、作成されたデータベースのローカル サーバーの名前を指定します (ここで **(localdb) \ProjectsV12**)。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="fcf8b-142">サーバーの名前を指定するには、利用可能なデータベースから、ContosoUniversityData を選択します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![接続プロパティの設定](creating-the-web-application/_static/image8.png)

<span data-ttu-id="fcf8b-144">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-144">Click **OK**.</span></span>

<span data-ttu-id="fcf8b-145">適切な接続プロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="fcf8b-146">Web.Config ファイルで接続の既定の名前を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-146">You can use the default name for connection in the Web.Config file</span></span>

![接続の設定](creating-the-web-application/_static/image9.png)

<span data-ttu-id="fcf8b-148">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-148">Click **Next**.</span></span>

<span data-ttu-id="fcf8b-149">選択**テーブル**の 3 つすべてのテーブル モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-149">Select **Tables** to generate models for all three tables.</span></span>

![テーブルを選択します。](creating-the-web-application/_static/image10.png)

<span data-ttu-id="fcf8b-151">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-151">Click **Finish**.</span></span>

<span data-ttu-id="fcf8b-152">でセキュリティ警告が発生する場合は、選択**OK**テンプレートの実行を続行します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="fcf8b-153">モデルは、データベースのテーブルから生成され、プロパティと、テーブル間のリレーションシップを示すダイアグラムが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![モデルのダイアグラム](creating-the-web-application/_static/image11.png)

<span data-ttu-id="fcf8b-155">今すぐ、Models フォルダーには、データベースから生成されたモデルに関連する多くの新しいファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![新しいモデル ファイルを表示します。](creating-the-web-application/_static/image12.png)

<span data-ttu-id="fcf8b-157">**ContosoModel.Context.cs**ファイルにはから派生したクラスが含まれています、 **DbContext**クラス、および各データベース テーブルに対応するモデル クラスのプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="fcf8b-158">**Course.cs**、 **Enrollment.cs**、および**Student.cs**ファイルがデータベース テーブルを表すモデル クラスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="fcf8b-159">スキャフォールディングを使用する場合は、モデル クラスとコンテキスト クラスの両方を使用します。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="fcf8b-160">このチュートリアルで前に、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="fcf8b-161">セクションでは、プロジェクトがビルドされていない場合は機能しませんが、次のセクションで、データ モデルに基づくコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="fcf8b-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fcf8b-162">[前へ](setting-up-database.md)
> [次へ](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="fcf8b-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
