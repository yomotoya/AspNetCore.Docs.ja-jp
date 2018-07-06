---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: モデル クラスを作成、Entity framework (c#) |Microsoft Docs
author: microsoft
description: このチュートリアルでは、Microsoft Entity Framework で ASP.NET MVC を使用する方法について説明します。 エンティティ ウィザードを使用して、ADO.NET エンティティ データを作成する方法を学習します.
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b88aaaae21323fe3e3e8548cc04110c9caafef7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808143"
---
<a name="creating-model-classes-with-the-entity-framework-c"></a><span data-ttu-id="e55f4-104">Entity Framework (c#) でモデル クラスを作成</span><span class="sxs-lookup"><span data-stu-id="e55f4-104">Creating Model Classes with the Entity Framework (C#)</span></span>
====================
<span data-ttu-id="e55f4-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e55f4-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e55f4-106">このチュートリアルでは、Microsoft Entity Framework で ASP.NET MVC を使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-106">In this tutorial, you learn how to use ASP.NET MVC with the Microsoft Entity Framework.</span></span> <span data-ttu-id="e55f4-107">エンティティ ウィザードを使用して、ADO.NET Entity Data Model を作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-107">You learn how to use the Entity Wizard to create an ADO.NET Entity Data Model.</span></span> <span data-ttu-id="e55f4-108">このチュートリアルの過程で、選択、挿入、更新、および Entity Framework を使用してデータベースのデータを削除する方法について説明する web アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-108">Over the course of this tutorial, we build a web application that illustrates how to select, insert, update, and delete database data by using the Entity Framework.</span></span>


<span data-ttu-id="e55f4-109">このチュートリアルの目的では、ASP.NET MVC アプリケーションを構築するときに、Microsoft Entity Framework を使用してデータ アクセス クラスを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-109">The goal of this tutorial is to explain how you can create data access classes using the Microsoft Entity Framework when building an ASP.NET MVC application.</span></span> <span data-ttu-id="e55f4-110">このチュートリアルは、Microsoft Entity Framework の以前の知識を負いません。</span><span class="sxs-lookup"><span data-stu-id="e55f4-110">This tutorial assumes no previous knowledge of the Microsoft Entity Framework.</span></span> <span data-ttu-id="e55f4-111">このチュートリアルの目的は、Entity Framework を使用して、選択、挿入、更新、およびデータベースのレコードを削除する方法を理解します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-111">By the end of this tutorial, you'll understand how to use the Entity Framework to select, insert, update, and delete database records.</span></span>

<span data-ttu-id="e55f4-112">Microsoft Entity Framework は、オブジェクト リレーショナル マッピング (O/RM) ツール、データベースからデータ アクセス層を自動的に生成することができます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-112">The Microsoft Entity Framework is an Object Relational Mapping (O/RM) tool that enables you to generate a data access layer from a database automatically.</span></span> <span data-ttu-id="e55f4-113">Entity Framework では、手動で、データ アクセス クラスを構築する面倒な作業を回避することができます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-113">The Entity Framework enables you to avoid the tedious work of building your data access classes by hand.</span></span>

<span data-ttu-id="e55f4-114">ASP.NET MVC を使用した Microsoft Entity Framework を使用する方法について説明するには、簡単なサンプル アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e55f4-114">In order to illustrate how you can use the Microsoft Entity Framework with ASP.NET MVC, we'll build a simple sample application.</span></span> <span data-ttu-id="e55f4-115">表示し、映画データベース レコードを編集することができます、映画データベース アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-115">We'll create a Movie Database application that enables you to display and edit movie database records.</span></span>

<span data-ttu-id="e55f4-116">このチュートリアルでは、Visual Studio 2008 または Service Pack 1 の Visual Web Developer 2008 があることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="e55f4-116">This tutorial assumes that you have Visual Studio 2008 or Visual Web Developer 2008 with Service Pack 1.</span></span> <span data-ttu-id="e55f4-117">Entity Framework を使用するために Service Pack 1 を必要があります。</span><span class="sxs-lookup"><span data-stu-id="e55f4-117">You need Service Pack 1 in order to use the Entity Framework.</span></span> <span data-ttu-id="e55f4-118">Visual Studio 2008 Service Pack 1 または Service Pack 1 の Visual Web Developer は、次のアドレスからダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-118">You can download Visual Studio 2008 Service Pack 1 or Visual Web Developer with Service Pack 1 from the following address:</span></span>

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


> [!NOTE] 
> 
> <span data-ttu-id="e55f4-119">ASP.NET MVC と Entity Framework の Microsoft 必須の接続がありません。</span><span class="sxs-lookup"><span data-stu-id="e55f4-119">There is no essential connection between ASP.NET MVC and the Microsoft Entity Framework.</span></span> <span data-ttu-id="e55f4-120">ASP.NET MVC で使用できる Entity Framework のいくつかの選択肢があります。</span><span class="sxs-lookup"><span data-stu-id="e55f4-120">There are several alternatives to the Entity Framework that you can use with ASP.NET MVC.</span></span> <span data-ttu-id="e55f4-121">たとえば、Microsoft LINQ to SQL や NHibernate など、SubSonic などその他の O/RM ツールを使用して、MVC のモデル クラスをビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-121">For example, you can build your MVC Model classes using other O/RM tools such as Microsoft LINQ to SQL, NHibernate, or SubSonic.</span></span>


## <a name="creating-the-movie-sample-database"></a><span data-ttu-id="e55f4-122">ムービーのサンプル データベースの作成</span><span class="sxs-lookup"><span data-stu-id="e55f4-122">Creating the Movie Sample Database</span></span>

<span data-ttu-id="e55f4-123">映画データベース アプリケーションでは、次の列を含むデータベース テーブルのムービーをという名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-123">The Movie Database application uses a database table named Movies that contains the following columns:</span></span>

| <span data-ttu-id="e55f4-124">列名</span><span class="sxs-lookup"><span data-stu-id="e55f4-124">Column Name</span></span> | <span data-ttu-id="e55f4-125">データの種類</span><span class="sxs-lookup"><span data-stu-id="e55f4-125">Data Type</span></span> | <span data-ttu-id="e55f4-126">Null 値を許可しますか。</span><span class="sxs-lookup"><span data-stu-id="e55f4-126">Allow Nulls?</span></span> | <span data-ttu-id="e55f4-127">プライマリ キーとは</span><span class="sxs-lookup"><span data-stu-id="e55f4-127">Is Primary Key?</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e55f4-128">ID</span><span class="sxs-lookup"><span data-stu-id="e55f4-128">Id</span></span> | <span data-ttu-id="e55f4-129">int</span><span class="sxs-lookup"><span data-stu-id="e55f4-129">int</span></span> | <span data-ttu-id="e55f4-130">False</span><span class="sxs-lookup"><span data-stu-id="e55f4-130">False</span></span> | <span data-ttu-id="e55f4-131">True</span><span class="sxs-lookup"><span data-stu-id="e55f4-131">True</span></span> |
| <span data-ttu-id="e55f4-132">Title</span><span class="sxs-lookup"><span data-stu-id="e55f4-132">Title</span></span> | <span data-ttu-id="e55f4-133">nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="e55f4-133">nvarchar(100)</span></span> | <span data-ttu-id="e55f4-134">False</span><span class="sxs-lookup"><span data-stu-id="e55f4-134">False</span></span> | <span data-ttu-id="e55f4-135">False</span><span class="sxs-lookup"><span data-stu-id="e55f4-135">False</span></span> |
| <span data-ttu-id="e55f4-136">ディレクター</span><span class="sxs-lookup"><span data-stu-id="e55f4-136">Director</span></span> | <span data-ttu-id="e55f4-137">nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="e55f4-137">nvarchar(100)</span></span> | <span data-ttu-id="e55f4-138">False</span><span class="sxs-lookup"><span data-stu-id="e55f4-138">False</span></span> | <span data-ttu-id="e55f4-139">False</span><span class="sxs-lookup"><span data-stu-id="e55f4-139">False</span></span> |

<span data-ttu-id="e55f4-140">このテーブルは、次の手順に従って、ASP.NET MVC プロジェクトに追加できます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-140">You can add this table to an ASP.NET MVC project by following these steps:</span></span>

1. <span data-ttu-id="e55f4-141">アプリを右クリックして\_メニュー オプションを選択して、ソリューション エクスプ ローラー ウィンドウで、データ フォルダー**追加]、[新しい項目。**</span><span class="sxs-lookup"><span data-stu-id="e55f4-141">Right-click the App\_Data folder in the Solution Explorer window and select the menu option **Add, New Item.**</span></span>
2. <span data-ttu-id="e55f4-142">**新しい項目の追加**ダイアログ ボックスで、 **SQL Server データベース**MoviesDB.mdf、名、データベースを提供し、クリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e55f4-142">From the **Add New Item** dialog box, select **SQL Server Database**, give the database the name MoviesDB.mdf, and click the **Add** button.</span></span>
3. <span data-ttu-id="e55f4-143">サーバー エクスプ ローラー/データベース エクスプ ローラー ウィンドウを開く MoviesDB.mdf ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="e55f4-143">Double-click the MoviesDB.mdf file to open the Server Explorer/Database Explorer window.</span></span>
4. <span data-ttu-id="e55f4-144">MoviesDB.mdf データベース接続を展開し、[テーブル] フォルダーを右クリックしてメニュー オプションを選択**新しいテーブルの追加**します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-144">Expand the MoviesDB.mdf database connection, right-click the Tables folder, and select the menu option **Add New Table**.</span></span>
5. <span data-ttu-id="e55f4-145">テーブル デザイナーでは、Id、タイトル、およびディレクター列を追加します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-145">In the Table Designer, add the Id, Title, and Director columns.</span></span>
6. <span data-ttu-id="e55f4-146">をクリックして、**保存**ボタン (フロッピー ディスクのアイコンがある) 映画名と、新しいテーブルを保存します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-146">Click the **Save** button (it has the icon of the floppy) to save the new table with the name Movies.</span></span>

<span data-ttu-id="e55f4-147">映画データベースのテーブルを作成した後は、テーブルにサンプル データを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e55f4-147">After you create the Movies database table, you should add some sample data to the table.</span></span> <span data-ttu-id="e55f4-148">映画のテーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-148">Right-click the Movies table and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="e55f4-149">表示されたグリッドには、偽の映画のデータを入力できます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-149">You can enter fake movie data into the grid that appears.</span></span>

## <a name="creating-the-adonet-entity-data-model"></a><span data-ttu-id="e55f4-150">ADO.NET Entity Data Model を作成します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-150">Creating the ADO.NET Entity Data Model</span></span>

<span data-ttu-id="e55f4-151">Entity Framework を使用するには、Entity Data Model を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e55f4-151">In order to use the Entity Framework, you need to create an Entity Data Model.</span></span> <span data-ttu-id="e55f4-152">Visual Studio の利用*Entity Data Model ウィザード*データベースからエンティティ データ モデルを自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-152">You can take advantage of the Visual Studio *Entity Data Model Wizard* to generate an Entity Data Model from a database automatically.</span></span>

<span data-ttu-id="e55f4-153">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e55f4-153">Follow these steps:</span></span>

1. <span data-ttu-id="e55f4-154">ソリューション エクスプ ローラー ウィンドウで、Models フォルダーを右クリックし、メニュー オプションを選択**追加、新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-154">Right-click the Models folder in the Solution Explorer window and select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="e55f4-155">**新しい項目の追加**ダイアログ ボックスで、データ カテゴリを選択します (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="e55f4-155">In the **Add New Item** dialog, select the Data category (see Figure 1).</span></span>
3. <span data-ttu-id="e55f4-156">選択、 **ADO.NET Entity Data Model**テンプレート、名 MoviesDBModel.edmx、エンティティ データ モデルを提供し、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e55f4-156">Select the **ADO.NET Entity Data Model** template, give the Entity Data Model the name MoviesDBModel.edmx, and click the **Add** button.</span></span> <span data-ttu-id="e55f4-157">クリックすると、**追加**ボタンは、データ モデル ウィザードを起動します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-157">Clicking the **Add** button launches the Data Model Wizard.</span></span>
4. <span data-ttu-id="e55f4-158">**モデルのコンテンツの選択**ステップで、選択、**をデータベースから生成**オプションを選択し、をクリックして、**次**ボタン (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="e55f4-158">In the **Choose Model Contents** step, choose the **Generate from a database** option and click the **Next** button (see Figure 2).</span></span>
5. <span data-ttu-id="e55f4-159">**データ接続の選択**ステップ、MoviesDB.mdf データベース接続を選択し、エンティティ接続設定 MoviesDBEntities、という名前を入力してください、**次**ボタン (図 3 参照)。</span><span class="sxs-lookup"><span data-stu-id="e55f4-159">In the **Choose Your Data Connection** step, select the MoviesDB.mdf database connection, enter the entities connection settings name MoviesDBEntities, and click the **Next** button (see Figure 3).</span></span>
6. <span data-ttu-id="e55f4-160">**データベース オブジェクトの選択**ステップ ムービー データベースのテーブルを選択して、クリックして、**完了**(図 4 参照) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e55f4-160">In the **Choose Your Database Objects** step, select the Movie database table and click the **Finish** button (see Figure 4).</span></span>

<span data-ttu-id="e55f4-161">次の手順を完了すると、ADO.NET Entity Data Model デザイナー (エンティティ デザイナー) を開きます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-161">After you complete these steps, the ADO.NET Entity Data Model Designer (Entity Designer) opens.</span></span>

<span data-ttu-id="e55f4-162">**図 1 – 新しい Entity Data Model を作成します。**</span><span class="sxs-lookup"><span data-stu-id="e55f4-162">**Figure 1 – Creating a new Entity Data Model**</span></span>

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

<span data-ttu-id="e55f4-164">**図 2 – モデル内容のステップを選択**</span><span class="sxs-lookup"><span data-stu-id="e55f4-164">**Figure 2 – Choose Model Contents Step**</span></span>

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

<span data-ttu-id="e55f4-166">**図 3: データ接続の選択**</span><span class="sxs-lookup"><span data-stu-id="e55f4-166">**Figure 3 – Choose Your Data Connection**</span></span>

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

<span data-ttu-id="e55f4-168">**図 4-データベース オブジェクトの選択**</span><span class="sxs-lookup"><span data-stu-id="e55f4-168">**Figure 4 – Choose Your Database Objects**</span></span>

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a><span data-ttu-id="e55f4-170">ADO.NET エンティティ データ モデルの変更</span><span class="sxs-lookup"><span data-stu-id="e55f4-170">Modifying the ADO.NET Entity Data Model</span></span>

<span data-ttu-id="e55f4-171">Entity Data Model を作成した後は、エンティティ デザイナーを利用して、モデルを変更できます (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e55f4-171">After you create an Entity Data Model, you can modify the model by taking advantage of the Entity Designer (see Figure 5).</span></span> <span data-ttu-id="e55f4-172">ソリューション エクスプ ローラー ウィンドウ内で、Models フォルダーに含まれている MoviesDBModel.edmx ファイルをダブルクリックして、いつでも、エンティティ デザイナーを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-172">You can open the Entity Designer at any time by double-clicking the MoviesDBModel.edmx file contained in the Models folder within the Solution Explorer window.</span></span>

<span data-ttu-id="e55f4-173">**5 –、ADO.NET Entity Data Model デザイナーを図します。**</span><span class="sxs-lookup"><span data-stu-id="e55f4-173">**Figure 5 – The ADO.NET Entity Data Model Designer**</span></span>

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

<span data-ttu-id="e55f4-175">たとえば、エンティティ デザイナーを使用して、エンティティ モデルのデータ ウィザードで生成するクラスの名前を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-175">For example, you can use the Entity Designer to change the names of the classes that the Entity Model Data Wizard generates.</span></span> <span data-ttu-id="e55f4-176">ウィザードでは、映画をという名前の新しいデータ アクセス クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-176">The Wizard created a new data access class named Movies.</span></span> <span data-ttu-id="e55f4-177">つまり、ウィザードは、データベース テーブルとまったく同じ名前のクラスしました。</span><span class="sxs-lookup"><span data-stu-id="e55f4-177">In other words, the Wizard gave the class the very same name as the database table.</span></span> <span data-ttu-id="e55f4-178">使用するのでこのクラスを特定のムービー インスタンスを表す、ムービーに映画からクラスの名前する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e55f4-178">Because we will use this class to represent a particular Movie instance, we should rename the class from Movies to Movie.</span></span>

<span data-ttu-id="e55f4-179">エンティティ クラスの名前を変更する場合は、エンティティ デザイナーでクラス名をダブルクリックして、新しい名前を入力します (図 6 参照)。</span><span class="sxs-lookup"><span data-stu-id="e55f4-179">If you want to rename an entity class, you can double-click on the class name in the Entity Designer and enter a new name (see Figure 6).</span></span> <span data-ttu-id="e55f4-180">また、エンティティ デザイナーでエンティティを選択した後、[プロパティ] ウィンドウ内のエンティティの名前を変更できます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-180">Alternatively, you can change the name of an entity in the Properties window after selecting an entity in the Entity Designer.</span></span>

<span data-ttu-id="e55f4-181">**図 6 – エンティティの名前を変更します。**</span><span class="sxs-lookup"><span data-stu-id="e55f4-181">**Figure 6 – Changing an entity name**</span></span>

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

<span data-ttu-id="e55f4-183">保存ボタン (フロッピー ディスクのアイコン) をクリックして変更を行った後に、エンティティ データ モデルを保存してください。</span><span class="sxs-lookup"><span data-stu-id="e55f4-183">Remember to save your Entity Data Model after making a modification by clicking the Save button (the icon of the floppy disk).</span></span> <span data-ttu-id="e55f4-184">バック グラウンドでは、エンティティ デザイナーは、一連の c# クラスを生成します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-184">Behind the scenes, the Entity Designer generates a set of C# classes.</span></span> <span data-ttu-id="e55f4-185">ソリューション エクスプ ローラー ウィンドウから MoviesDBModel.Designer.cs ファイルを開くことで、これらのクラスを表示できます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-185">You can view these classes by opening the MoviesDBModel.Designer.cs file from the Solution Explorer window.</span></span>


<span data-ttu-id="e55f4-186">次に、エンティティ デザイナーを使用するときに、変更が上書きされますので、Designer.cs ファイル内のコードを変更しないでください。</span><span class="sxs-lookup"><span data-stu-id="e55f4-186">Don't modify the code in the Designer.cs file since your changes will be overwritten the next time you use the Entity Designer.</span></span> <span data-ttu-id="e55f4-187">Designer.cs ファイルで定義されたエンティティ クラスの機能を拡張するかどうかは、作成することができます*部分クラス*で個別のファイル。</span><span class="sxs-lookup"><span data-stu-id="e55f4-187">If you want to extend the functionality of the entity classes defined in the Designer.cs file then you can create *partial classes* in separate files.</span></span>


#### <a name="selecting-database-records-with-the-entity-framework"></a><span data-ttu-id="e55f4-188">Entity Framework でのデータベース レコードを選択します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-188">Selecting Database Records with the Entity Framework</span></span>

<span data-ttu-id="e55f4-189">ムービーのレコードの一覧を表示するページを作成し、映画データベース アプリケーションを構築してみましょう。</span><span class="sxs-lookup"><span data-stu-id="e55f4-189">Let's start building our Movie Database application by creating a page that displays a list of movie records.</span></span> <span data-ttu-id="e55f4-190">リスト 1 で、Home コント ローラーは、Index() という名前のアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-190">The Home controller in Listing 1 exposes an action named Index().</span></span> <span data-ttu-id="e55f4-191">Index() アクションに、ムービーのデータベース テーブルからすべてのムービー レコード、Entity Framework を活用してに返します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-191">The Index() action returns all of the movie records from the Movie database table by taking advantage of the Entity Framework.</span></span>

<span data-ttu-id="e55f4-192">**1 – controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e55f4-192">**Listing 1 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

<span data-ttu-id="e55f4-193">リスト 1 で、コント ローラーがコンス トラクターが含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e55f4-193">Notice that the controller in Listing 1 includes a constructor.</span></span> <span data-ttu-id="e55f4-194">コンス トラクターによって初期化という名前のクラスレベル フィールド\_db します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-194">The constructor initializes a class-level field named \_db.</span></span> <span data-ttu-id="e55f4-195">\_Db フィールドは、Microsoft Entity Framework によって生成されたデータベースのエンティティを表します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-195">The \_db field represents the database entities generated by the Microsoft Entity Framework.</span></span> <span data-ttu-id="e55f4-196">\_Db フィールドは、エンティティ デザイナーによって生成された MoviesDBEntities クラスのインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="e55f4-196">The \_db field is an instance of the MoviesDBEntities class that was generated by the Entity Designer.</span></span>


<span data-ttu-id="e55f4-197">TheMoviesDBEntities クラスを Home コント ローラーを使用するためには、MovieEntityApp.Models 名前空間をインポートする必要があります (*MVCProjectName*します。モデルの場合)。</span><span class="sxs-lookup"><span data-stu-id="e55f4-197">In order to use theMoviesDBEntities class in the Home controller, you must import the MovieEntityApp.Models namespace (*MVCProjectName*.Models).</span></span>


<span data-ttu-id="e55f4-198">\_Db フィールドは、映画データベース テーブルからレコードを取得する Index() 操作内で使用します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-198">The \_db field is used within the Index() action to retrieve the records from the Movies database table.</span></span> <span data-ttu-id="e55f4-199">式\_db します。MovieSet では、映画データベース テーブルからすべてのレコードを表します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-199">The expression \_db.MovieSet represents all of the records from the Movies database table.</span></span> <span data-ttu-id="e55f4-200">ムービー オブジェクトのジェネリック コレクションをムービーのセットに変換する ToList() メソッドが使用される (リスト&lt;ムービー&gt;)。</span><span class="sxs-lookup"><span data-stu-id="e55f4-200">The ToList() method is used to convert the set of movies into a generic collection of Movie objects (List&lt;Movie&gt;).</span></span>

<span data-ttu-id="e55f4-201">LINQ to Entities を利用して、ムービーのレコードが取得されます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-201">The movie records are retrieved with the help of LINQ to Entities.</span></span> <span data-ttu-id="e55f4-202">リスト 1 で Index() アクションは LINQ を使用して*メソッド構文*データベース レコードのセットを取得します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-202">The Index() action in Listing 1 uses LINQ *method syntax* to retrieve the set of database records.</span></span> <span data-ttu-id="e55f4-203">LINQ を使用することができる場合は、*クエリ構文*代わりにします。</span><span class="sxs-lookup"><span data-stu-id="e55f4-203">If you prefer, you can use LINQ *query syntax* instead.</span></span> <span data-ttu-id="e55f4-204">次の 2 つのステートメントでは、まったく同じことを行います。</span><span class="sxs-lookup"><span data-stu-id="e55f4-204">The following two statements do the very same thing:</span></span>

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

<span data-ttu-id="e55f4-205">どの LINQ 構文を使用 – メソッド構文またはクエリ構文 – 最も直感的に表示されています。</span><span class="sxs-lookup"><span data-stu-id="e55f4-205">Use whichever LINQ syntax – method syntax or query syntax – that you find most intuitive.</span></span> <span data-ttu-id="e55f4-206">2 つのアプローチの間のパフォーマンスに違いはありません – 唯一の違いはスタイル。</span><span class="sxs-lookup"><span data-stu-id="e55f4-206">There is no difference in performance between the two approaches – the only difference is style.</span></span>

<span data-ttu-id="e55f4-207">リスト 2 でビューを使用すると、ムービーのレコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-207">The view in Listing 2 is used to display the movie records.</span></span>

<span data-ttu-id="e55f4-208">**2 – Views\Home\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e55f4-208">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

<span data-ttu-id="e55f4-209">リスト 2 でビューが含まれています、 **foreach**ループをムービーの各レコードを反復処理し、ムービーのレコードのタイトルとディレクター プロパティの値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-209">The view in Listing 2 contains a **foreach** loop that iterates through each movie record and displays the values of the movie record's Title and Director properties.</span></span> <span data-ttu-id="e55f4-210">各レコードの横にある、編集、削除のリンクが表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e55f4-210">Notice that an Edit and Delete link is displayed next to each record.</span></span> <span data-ttu-id="e55f4-211">ビューの下部にあるさらに、ムービーの追加リンクが表示されます (図 7 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e55f4-211">Furthermore, an Add Movie link appears at the bottom of the view (see Figure 7).</span></span>

<span data-ttu-id="e55f4-212">**図 7 –、インデックス ビュー**</span><span class="sxs-lookup"><span data-stu-id="e55f4-212">**Figure 7 – The Index view**</span></span>

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

<span data-ttu-id="e55f4-214">インデックス ビューは、*ビュー型指定された*します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-214">The Index view is a *typed view*.</span></span> <span data-ttu-id="e55f4-215">インデックス ビューには、&lt;ページ % @ %&gt;ディレクティブ、 *Inherits*ムービー オブジェクトの厳密に型指定のジェネリック List コレクションにモデルのプロパティをキャストする属性 (リスト&lt;ムービー)。</span><span class="sxs-lookup"><span data-stu-id="e55f4-215">The Index view includes a &lt;%@ Page %&gt; directive with an *Inherits* attribute that casts the Model property to a strongly typed generic List collection of Movie objects (List&lt;Movie).</span></span>

## <a name="inserting-database-records-with-the-entity-framework"></a><span data-ttu-id="e55f4-216">Entity Framework でのデータベース レコードを挿入します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-216">Inserting Database Records with the Entity Framework</span></span>

<span data-ttu-id="e55f4-217">Entity Framework を使用すると、データベース テーブルに新しいレコードを挿入しやすきます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-217">You can use the Entity Framework to make it easy to insert new records into a database table.</span></span> <span data-ttu-id="e55f4-218">3 を一覧表示するには、ムービーのデータベース テーブルに新しいレコードを挿入するために使用できる、ホーム コント ローラー クラスに追加された 2 つの新しいアクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e55f4-218">Listing 3 contains two new actions added to the Home controller class that you can use to insert new records into the Movie database table.</span></span>

<span data-ttu-id="e55f4-219">**3 – controllers \homecontroller.cs (メソッドの追加) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e55f4-219">**Listing 3 – Controllers\HomeController.cs (Add methods)**</span></span>

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

<span data-ttu-id="e55f4-220">Add() の最初のアクションでは、ビューを単純に返します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-220">The first Add() action simply returns a view.</span></span> <span data-ttu-id="e55f4-221">ビューには、新しいムービー データベースを追加するフォームが含まれます (図 8 参照) を記録します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-221">The view contains a form for adding a new movie database record (see Figure 8).</span></span> <span data-ttu-id="e55f4-222">フォームを送信するときに、2 番目の Add() アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-222">When you submit the form, the second Add() action is invoked.</span></span>

<span data-ttu-id="e55f4-223">2 番目の Add() アクションは、AcceptVerbs 属性で修飾されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e55f4-223">Notice that the second Add() action is decorated with the AcceptVerbs attribute.</span></span> <span data-ttu-id="e55f4-224">HTTP POST 操作を実行する場合にのみ、この操作を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-224">This action can be invoked only when performing an HTTP POST operation.</span></span> <span data-ttu-id="e55f4-225">つまり、この操作は、HTML フォームを投稿する場合にのみ呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-225">In other words, this action can only be invoked when posting an HTML form.</span></span>

<span data-ttu-id="e55f4-226">2 番目の Add() アクションは、ASP.NET MVC TryUpdateModel() メソッドのヘルプで、Entity Framework Movie クラスの新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-226">The second Add() action creates a new instance of the Entity Framework Movie class with the help of the ASP.NET MVC TryUpdateModel() method.</span></span> <span data-ttu-id="e55f4-227">TryUpdateModel() メソッドは、Add() メソッドに渡される FormCollection 内のフィールドを受け取り、Movie クラスに HTML フォーム フィールドの値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-227">The TryUpdateModel() method takes the fields in the FormCollection passed to the Add() method and assigns the values of these HTML form fields to the Movie class.</span></span>


<span data-ttu-id="e55f4-228">Entity Framework を使用する場合は、tryupdatemodel に渡します"または"UpdateModel メソッドを使用して、エンティティ クラスのプロパティを更新する場合、プロパティの「ホワイト リスト」を指定してください。</span><span class="sxs-lookup"><span data-stu-id="e55f4-228">When using the Entity Framework, you must supply a "white list" of properties when using the TryUpdateModel or UpdateModel methods to update the properties of an entity class.</span></span>


<span data-ttu-id="e55f4-229">次に、Add() アクションは、いくつかの簡単なフォーム検証を実行します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-229">Next, the Add() action performs some simple form validation.</span></span> <span data-ttu-id="e55f4-230">アクションは、タイトルとディレクターの両方のプロパティに値があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-230">The action verifies that both the Title and Director properties have values.</span></span> <span data-ttu-id="e55f4-231">検証エラーがある場合、検証エラー メッセージは、ModelState に追加されます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-231">If there is a validation error, then a validation error message is added to ModelState.</span></span>

<span data-ttu-id="e55f4-232">検証エラーがない場合、新しいムービーのレコードは、Entity Framework を利用して、映画データベース テーブルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-232">If there are no validation errors then a new movie record is added to the Movies database table with the help of the Entity Framework.</span></span> <span data-ttu-id="e55f4-233">新しいレコードは、次の 2 つのコード行をデータベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-233">The new record is added to the database with the following two lines of code:</span></span>

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

<span data-ttu-id="e55f4-234">コードの最初の行では、Entity Framework によって追跡されているムービーのセットに新しいムービー エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-234">The first line of code adds the new Movie entity to the set of movies being tracked by the Entity Framework.</span></span> <span data-ttu-id="e55f4-235">基になるデータベースに追跡されている映画を自由に変更を加え、2 つ目のコード行を保存します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-235">The second line of code saves whatever changes have been made to the Movies being tracked back to the underlying database.</span></span>

<span data-ttu-id="e55f4-236">**図 8-追加のビュー**</span><span class="sxs-lookup"><span data-stu-id="e55f4-236">**Figure 8 – The Add view**</span></span>

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a><span data-ttu-id="e55f4-238">Entity Framework でのデータベース レコードを更新しています</span><span class="sxs-lookup"><span data-stu-id="e55f4-238">Updating Database Records with the Entity Framework</span></span>

<span data-ttu-id="e55f4-239">新しいデータベース レコードを挿入するだけに準拠した方法として、Entity Framework でのデータベース レコードを編集するほぼ同じ方法に従うことができます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-239">You can follow almost the same approach to edit a database record with the Entity Framework as the approach that we just followed to insert a new database record.</span></span> <span data-ttu-id="e55f4-240">4 を一覧表示するには、Edit() という名前の 2 つの新しいコント ローラー アクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e55f4-240">Listing 4 contains two new controller actions named Edit().</span></span> <span data-ttu-id="e55f4-241">Edit() の最初のアクションは、ムービーのレコードを編集するための HTML フォームを返します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-241">The first Edit() action returns an HTML form for editing a movie record.</span></span> <span data-ttu-id="e55f4-242">2 番目の Edit() アクションは、データベースを更新しようとします。</span><span class="sxs-lookup"><span data-stu-id="e55f4-242">The second Edit() action attempts to update the database.</span></span>

<span data-ttu-id="e55f4-243">**4 – controllers \homecontroller.cs (Edit メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e55f4-243">**Listing 4 – Controllers\HomeController.cs (Edit methods)**</span></span>

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

<span data-ttu-id="e55f4-244">2 番目の Edit() アクションは、ムービーの編集中の Id と一致するデータベースからムービー レコードを取得することによって開始します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-244">The second Edit() action starts by retrieving the Movie record from the database that matches the Id of the movie being edited.</span></span> <span data-ttu-id="e55f4-245">エンティティ ステートメントに次の LINQ では、特定の Id と一致する最初のデータベース レコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-245">The following LINQ to Entities statement grabs the first database record that matches a particular Id:</span></span>

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

<span data-ttu-id="e55f4-246">次に、ムービー エンティティのプロパティには HTML フォームのフィールドの値を割り当てる TryUpdateModel() メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-246">Next, the TryUpdateModel() method is used to assign the values of the HTML form fields to the properties of the movie entity.</span></span> <span data-ttu-id="e55f4-247">更新する正確なプロパティを指定するホワイト リストが提供されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e55f4-247">Notice that a white list is supplied to specify the exact properties to update.</span></span>

<span data-ttu-id="e55f4-248">次に、単純な検証を実行して、ムービーのタイトルとディレクターの両方のプロパティに値があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-248">Next, some simple validation is performed to verify that both the Movie Title and Director properties have values.</span></span> <span data-ttu-id="e55f4-249">プロパティのいずれかがない場合、値、ModelState に検証エラー メッセージを追加し、ModelState.IsValid 値 false を返します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-249">If either property is missing a value, then a validation error message is added to ModelState and ModelState.IsValid returns the value false.</span></span>

<span data-ttu-id="e55f4-250">最後に、検証エラーがない場合、基になる映画データベース テーブルで更新されます変更 SaveChanges() メソッドを呼び出すことによって。</span><span class="sxs-lookup"><span data-stu-id="e55f4-250">Finally, if there are no validation errors, then the underlying Movies database table is updated with any changes by calling the SaveChanges() method.</span></span>

<span data-ttu-id="e55f4-251">データベースのレコードを編集するときは、データベースの更新を実行するコント ローラー アクションを編集中のレコードの Id を渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="e55f4-251">When editing database records, you need to pass the Id of the record being edited to the controller action that performs the database update.</span></span> <span data-ttu-id="e55f4-252">それ以外の場合、コント ローラー アクションでは、基になるデータベースを更新するレコードはわかりません。</span><span class="sxs-lookup"><span data-stu-id="e55f4-252">Otherwise, the controller action will not know which record to update in the underlying database.</span></span> <span data-ttu-id="e55f4-253">リストに 5 に含まれている、編集ビューには、編集中のデータベース レコードの Id を表す隠しフォーム フィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e55f4-253">The Edit view, contained in Listing 5, includes a hidden form field that represents the Id of the database record being edited.</span></span>

<span data-ttu-id="e55f4-254">**Listing 5 – Views\Home\Edit.aspx**</span><span class="sxs-lookup"><span data-stu-id="e55f4-254">**Listing 5 – Views\Home\Edit.aspx**</span></span>

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a><span data-ttu-id="e55f4-255">Entity Framework でのデータベース レコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-255">Deleting Database Records with the Entity Framework</span></span>

<span data-ttu-id="e55f4-256">このチュートリアルに取り組む必要があります、最終的なデータベース操作では、データベース レコードを削除しています。</span><span class="sxs-lookup"><span data-stu-id="e55f4-256">The final database operation, which we need to tackle in this tutorial, is deleting database records.</span></span> <span data-ttu-id="e55f4-257">リスト 6 でコント ローラー アクションを使用すると、特定のデータベース レコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-257">You can use the controller action in Listing 6 to delete a particular database record.</span></span>

<span data-ttu-id="e55f4-258">**Listing 6 -- \Controllers\HomeController.cs (Delete action)**</span><span class="sxs-lookup"><span data-stu-id="e55f4-258">**Listing 6 -- \Controllers\HomeController.cs (Delete action)**</span></span>

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

<span data-ttu-id="e55f4-259">Delete() アクションは、まず、Id と一致するエンティティは、アクションに渡されるムービーを取得します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-259">The Delete() action first retrieves the Movie entity that matches the Id passed to the action.</span></span> <span data-ttu-id="e55f4-260">次に、ムービーは、SaveChanges() メソッドを続けて DeleteObject() メソッドを呼び出すことによって、データベースから削除されます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-260">Next, the movie is deleted from the database by calling the DeleteObject() method followed by the SaveChanges() method.</span></span> <span data-ttu-id="e55f4-261">最後に、ユーザーはインデックス ビューにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="e55f4-261">Finally, the user is redirected back to the Index view.</span></span>

## <a name="summary"></a><span data-ttu-id="e55f4-262">まとめ</span><span class="sxs-lookup"><span data-stu-id="e55f4-262">Summary</span></span>

<span data-ttu-id="e55f4-263">このチュートリアルの目的は、ASP.NET MVC と Microsoft Entity Framework を活用して、データベース駆動型 web アプリケーションをビルドする方法をデモしました。</span><span class="sxs-lookup"><span data-stu-id="e55f4-263">The purpose of this tutorial was to demonstrate how you can build database-driven web applications by taking advantage of ASP.NET MVC and the Microsoft Entity Framework.</span></span> <span data-ttu-id="e55f4-264">使用すると、選択、挿入、更新、アプリケーションを構築し、データベース レコードを削除する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="e55f4-264">You learned how to build an application that enables you to select, insert, update, and delete database records.</span></span>

<span data-ttu-id="e55f4-265">まず、Visual Studio 内からのエンティティ データ モデルを生成する、Entity Data Model ウィザードを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-265">First, we discussed how you can use the Entity Data Model Wizard to generate an Entity Data Model from within Visual Studio.</span></span> <span data-ttu-id="e55f4-266">次に、LINQ to Entities を使用して、データベース テーブルから一連のデータベース レコードを取得する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e55f4-266">Next, you learn how to use LINQ to Entities to retrieve a set of database records from a database table.</span></span> <span data-ttu-id="e55f4-267">最後に、Entity Framework を使用して挿入、更新、およびデータベースのレコードを削除しました。</span><span class="sxs-lookup"><span data-stu-id="e55f4-267">Finally, we used the Entity Framework to insert, update, and delete database records.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e55f4-268">次へ</span><span class="sxs-lookup"><span data-stu-id="e55f4-268">Next</span></span>](creating-model-classes-with-linq-to-sql-cs.md)
