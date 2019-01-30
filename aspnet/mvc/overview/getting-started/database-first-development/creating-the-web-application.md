---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'チュートリアル: 作成、ASP.NET MVC での最初のデータベース、Web アプリケーションと ef データ モデル'
description: このチュートリアルでは、web アプリケーションを作成して、データベース テーブルに基づくデータ モデルの生成に焦点を当てています。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: dced55386c3f810e406c5c2b3f0071b45e3b2dbd
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236368"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="207ff-103">チュートリアル: 作成、ASP.NET MVC での最初のデータベース、Web アプリケーションと ef データ モデル</span><span class="sxs-lookup"><span data-stu-id="207ff-103">Tutorial: Create the the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="207ff-104">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="207ff-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="207ff-105">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="207ff-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="207ff-106">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="207ff-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="207ff-107">このチュートリアルでは、web アプリケーションを作成して、データベース テーブルに基づくデータ モデルの生成に焦点を当てています。</span><span class="sxs-lookup"><span data-stu-id="207ff-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="207ff-108">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="207ff-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="207ff-109">ASP.NET Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="207ff-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="207ff-110">モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="207ff-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="207ff-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="207ff-111">Prerequisites</span></span>

* [<span data-ttu-id="207ff-112">Entity Framework 6 Database First と MVC 5 の使用の概要</span><span class="sxs-lookup"><span data-stu-id="207ff-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="207ff-113">ASP.NET Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="207ff-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="207ff-114">新しいソリューションまたはデータベース プロジェクトと同じソリューションで、Visual Studio で新しいプロジェクトを作成し、 **ASP.NET Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="207ff-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="207ff-115">プロジェクトに名前を**ContosoSite**します。</span><span class="sxs-lookup"><span data-stu-id="207ff-115">Name the project **ContosoSite**.</span></span>

![プロジェクトを作成します。](creating-the-web-application/_static/image1.png)

<span data-ttu-id="207ff-117">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="207ff-117">Click **OK**.</span></span>

<span data-ttu-id="207ff-118">新しい ASP.NET プロジェクト ウィンドウで、選択、 **MVC**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="207ff-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="207ff-119">オフにすることができます、**クラウドでホスト**は後で、クラウドにアプリケーションを展開するために、ここではオプションです。</span><span class="sxs-lookup"><span data-stu-id="207ff-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="207ff-120">クリックして**OK**アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="207ff-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="207ff-121">既定のファイルとフォルダー、プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="207ff-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="207ff-122">このチュートリアルでは、Entity Framework 6 を使用します。</span><span class="sxs-lookup"><span data-stu-id="207ff-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="207ff-123">NuGet パッケージの管理 ウィンドウを使用してプロジェクトで Entity Framework のバージョンを再確認することができます。</span><span class="sxs-lookup"><span data-stu-id="207ff-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="207ff-124">必要に応じて、Entity Framework のバージョンを更新します。</span><span class="sxs-lookup"><span data-stu-id="207ff-124">If necessary, update your version of Entity Framework.</span></span>

![バージョンを表示します。](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="207ff-126">モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="207ff-126">Generate the models</span></span>

<span data-ttu-id="207ff-127">Entity Framework モデルをデータベース テーブルから作成されます。</span><span class="sxs-lookup"><span data-stu-id="207ff-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="207ff-128">これらのモデルは、データを操作に使用するクラスです。</span><span class="sxs-lookup"><span data-stu-id="207ff-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="207ff-129">各モデルでは、ミラー データベースのテーブルとテーブルの列に対応するプロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="207ff-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="207ff-130">右クリックし、**モデル**フォルダー、および選択**追加**と**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="207ff-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="207ff-131">新しい項目の追加 ウィンドウで、次のように選択します。**データ**左側のウィンドウで、 **ADO.NET Entity Data Model**から中央のウィンドウのオプション。</span><span class="sxs-lookup"><span data-stu-id="207ff-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="207ff-132">新しいモデル ファイルに名前**ContosoModel**します。</span><span class="sxs-lookup"><span data-stu-id="207ff-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="207ff-133">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="207ff-133">Click **Add**.</span></span>

<span data-ttu-id="207ff-134">Entity Data Model ウィザード、選択**データベースの EF デザイナー**します。</span><span class="sxs-lookup"><span data-stu-id="207ff-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="207ff-135">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="207ff-135">Click **Next**.</span></span>

<span data-ttu-id="207ff-136">開発環境内で定義されているデータベースの接続があれば、事前選択されているこれらの接続のいずれかを表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="207ff-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="207ff-137">ただし、このチュートリアルの最初の部分で作成したデータベースへの新しい接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="207ff-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="207ff-138">をクリックして、**新しい接続**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="207ff-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="207ff-139">接続のプロパティ ウィンドウで、作成されたデータベースのローカル サーバーの名前を指定します (ここで **(localdb) \Projects13**)。</span><span class="sxs-lookup"><span data-stu-id="207ff-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\Projects13**).</span></span> <span data-ttu-id="207ff-140">サーバーの名前を指定するには、利用可能なデータベースから、ContosoUniversityData を選択します。</span><span class="sxs-lookup"><span data-stu-id="207ff-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![接続プロパティの設定](creating-the-web-application/_static/image8.png)

<span data-ttu-id="207ff-142">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="207ff-142">Click **OK**.</span></span>

<span data-ttu-id="207ff-143">適切な接続プロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="207ff-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="207ff-144">Web.Config ファイルでは、接続の既定の名前を使用できます。</span><span class="sxs-lookup"><span data-stu-id="207ff-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="207ff-145">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="207ff-145">Click **Next**.</span></span>

<span data-ttu-id="207ff-146">Entity Framework の最新バージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="207ff-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="207ff-147">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="207ff-147">Click **Next**.</span></span>

<span data-ttu-id="207ff-148">選択**テーブル**の 3 つすべてのテーブル モデルを生成します。</span><span class="sxs-lookup"><span data-stu-id="207ff-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="207ff-149">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="207ff-149">Click **Finish**.</span></span>

<span data-ttu-id="207ff-150">でセキュリティ警告が発生する場合は、選択**OK**テンプレートの実行を続行します。</span><span class="sxs-lookup"><span data-stu-id="207ff-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="207ff-151">モデルは、データベースのテーブルから生成され、プロパティと、テーブル間のリレーションシップを示すダイアグラムが表示されます。</span><span class="sxs-lookup"><span data-stu-id="207ff-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![モデルのダイアグラム](creating-the-web-application/_static/image11.png)

<span data-ttu-id="207ff-153">今すぐ、Models フォルダーには、データベースから生成されたモデルに関連する多くの新しいファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="207ff-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="207ff-154">**ContosoModel.Context.cs**ファイルにはから派生したクラスが含まれています、 **DbContext**クラス、および各データベース テーブルに対応するモデル クラスのプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="207ff-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="207ff-155">**Course.cs**、 **Enrollment.cs**、および**Student.cs**ファイルがデータベース テーブルを表すモデル クラスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="207ff-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="207ff-156">スキャフォールディングを使用する場合は、モデル クラスとコンテキスト クラスの両方を使用します。</span><span class="sxs-lookup"><span data-stu-id="207ff-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="207ff-157">このチュートリアルで前に、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="207ff-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="207ff-158">セクションでは、プロジェクトがビルドされていない場合は機能しませんが、次のセクションで、データ モデルに基づくコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="207ff-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="207ff-159">次の手順</span><span class="sxs-lookup"><span data-stu-id="207ff-159">Next steps</span></span>

<span data-ttu-id="207ff-160">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="207ff-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="207ff-161">ASP.NET web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="207ff-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="207ff-162">モデルの生成</span><span class="sxs-lookup"><span data-stu-id="207ff-162">Generated the models</span></span>

<span data-ttu-id="207ff-163">チュートリアルに進み、[次へ] を作成する方法については、データ モデルに基づくコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="207ff-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="207ff-164">ビューの生成</span><span class="sxs-lookup"><span data-stu-id="207ff-164">Generating views</span></span>](generating-views.md)