---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 取得と表示を使用してデータ モデルのバインディングと web フォーム |Microsoft Docs
author: Rick-Anderson
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396286"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="983ab-104">取得して、モデル バインディングと web フォームでデータの表示</span><span class="sxs-lookup"><span data-stu-id="983ab-104">Retrieving and displaying data with model binding and web forms</span></span>
====================

> <span data-ttu-id="983ab-105">このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="983ab-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="983ab-106">モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="983ab-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="983ab-107">このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。</span><span class="sxs-lookup"><span data-stu-id="983ab-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
>  <span data-ttu-id="983ab-108">モデル バインドのパターンは、任意のデータ アクセス テクノロジと連携します。</span><span class="sxs-lookup"><span data-stu-id="983ab-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="983ab-109">このチュートリアルでは、Entity Framework を使用するが、最も使い慣れたデータ アクセス テクノロジを使用できます。</span><span class="sxs-lookup"><span data-stu-id="983ab-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="983ab-110">GridView、ListView、DetailsView、または FormView コントロールなどのデータ バインド サーバー コントロールから選択すると、更新、削除、およびデータの作成に使用するメソッドの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="983ab-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="983ab-111">このチュートリアルでは、SelectMethod の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="983ab-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="983ab-112">そのメソッド内では、データを取得するロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="983ab-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="983ab-113">次のチュートリアルでは、UpdateMethod、InsertMethod、DeleteMethod の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="983ab-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="983ab-114">できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)で完全なプロジェクトC#または Visual Basic です。</span><span class="sxs-lookup"><span data-stu-id="983ab-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="983ab-115">ダウンロード可能なコードは、Visual Studio 2012 以降のバージョンとは動作します。</span><span class="sxs-lookup"><span data-stu-id="983ab-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="983ab-116">これは、このチュートリアルで示すように、Visual Studio 2017 のテンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="983ab-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="983ab-117">チュートリアルでは、Visual Studio でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="983ab-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="983ab-118">ホスティング プロバイダーにアプリケーションを展開し、インターネット経由で使用できるようにもできます。</span><span class="sxs-lookup"><span data-stu-id="983ab-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="983ab-119">マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、</span><span class="sxs-lookup"><span data-stu-id="983ab-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="983ab-120">[無料 Azure 試用版アカウント](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)します。</span><span class="sxs-lookup"><span data-stu-id="983ab-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="983ab-121">Visual Studio web プロジェクトを Azure App Service Web Apps にデプロイする方法については、次を参照してください。、 [Visual Studio を使用して ASP.NET Web 配置](../../deployment/visual-studio-web-deployment/introduction.md)シリーズ。</span><span class="sxs-lookup"><span data-stu-id="983ab-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="983ab-122">そのチュートリアルでは、Entity Framework Code First Migrations を使用して Azure SQL Database に SQL Server データベースをデプロイする方法も示します。</span><span class="sxs-lookup"><span data-stu-id="983ab-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="983ab-123">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="983ab-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="983ab-124">Microsoft Visual Studio 2017 または Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="983ab-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="983ab-125">このチュートリアルは Visual Studio 2012 と Visual Studio 2013 も機能しますはユーザー インターフェイスとプロジェクト テンプレートでは、いくつか違いがあります。</span><span class="sxs-lookup"><span data-stu-id="983ab-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="983ab-126">構築します</span><span class="sxs-lookup"><span data-stu-id="983ab-126">What you'll build</span></span>

<span data-ttu-id="983ab-127">このチュートリアルではあります。</span><span class="sxs-lookup"><span data-stu-id="983ab-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="983ab-128">受講者コースに登録されていると、大学を反映するデータ オブジェクトを構築します。</span><span class="sxs-lookup"><span data-stu-id="983ab-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="983ab-129">データベース テーブル オブジェクトを構築します。</span><span class="sxs-lookup"><span data-stu-id="983ab-129">Build database tables from the objects</span></span>
* <span data-ttu-id="983ab-130">テスト データでデータベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="983ab-130">Populate the database with test data</span></span>
* <span data-ttu-id="983ab-131">Web フォームでデータを表示</span><span class="sxs-lookup"><span data-stu-id="983ab-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="983ab-132">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="983ab-132">Create the project</span></span>

1. <span data-ttu-id="983ab-133">Visual Studio 2017 では、作成、 **ASP.NET Web アプリケーション (.NET Framework)** という名前のプロジェクト**ContosoUniversityModelBinding**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![プロジェクトを作成します。](retrieving-data/_static/image19.png)

2. <span data-ttu-id="983ab-135">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="983ab-135">Select **OK**.</span></span> <span data-ttu-id="983ab-136">テンプレートを選択するダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="983ab-136">The dialog box to select a template appears.</span></span>

   ![web フォームを選択します。](retrieving-data/_static/image3.png)

3. <span data-ttu-id="983ab-138">選択、 **Web フォーム**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="983ab-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="983ab-139">必要に応じて、変更への認証**個々 のユーザー アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="983ab-140">**[OK]** をクリックして、プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="983ab-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="983ab-141">サイトの外観を変更します。</span><span class="sxs-lookup"><span data-stu-id="983ab-141">Modify site appearance</span></span>

   <span data-ttu-id="983ab-142">サイトの外観をカスタマイズするいくつかの変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="983ab-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="983ab-143">Site.Master ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="983ab-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="983ab-144">表示するタイトルを変更する**Contoso University**なく**My ASP.NET Application**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="983ab-145">ヘッダー テキストを変更する**アプリケーション名**に**Contoso University**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="983ab-146">適切なサイトへのナビゲーション ヘッダーのリンクを変更します。</span><span class="sxs-lookup"><span data-stu-id="983ab-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="983ab-147">リンクを削除**について**と**連絡先**し、代わりに、リンク、**学生**ページで、作成されます。</span><span class="sxs-lookup"><span data-stu-id="983ab-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="983ab-148">Site.Master を保存します。</span><span class="sxs-lookup"><span data-stu-id="983ab-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="983ab-149">受講者データを表示する web フォームを追加します。</span><span class="sxs-lookup"><span data-stu-id="983ab-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="983ab-150">**ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加**し**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="983ab-151">**新しい項目の追加**ダイアログ ボックスで、**マスター ページを使用した Web フォーム**テンプレートと名前を付けます**Students.aspx**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![ページを作成します。](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="983ab-153">**[追加]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="983ab-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="983ab-154">Web フォームのマスター ページで、選択**Site.Master**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="983ab-155">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="983ab-155">Select **OK**.</span></span>
   

## <a name="add-the-data-model"></a><span data-ttu-id="983ab-156">データ モデルを追加する</span><span class="sxs-lookup"><span data-stu-id="983ab-156">Add the data model</span></span>

<span data-ttu-id="983ab-157">**モデル**フォルダー、という名前のクラスを追加**UniversityModels.cs**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="983ab-158">右クリックして**モデル**を選択します**追加**、し**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="983ab-159">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="983ab-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="983ab-160">左側のナビゲーション メニューから選択**コード**、し**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![モデル クラスを作成します。](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="983ab-162">クラスの名前**UniversityModels.cs**選択**追加**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="983ab-163">このファイルで定義、 `SchoolContext`、 `Student`、`Enrollment`と`Course`クラスの次のようにします。</span><span class="sxs-lookup"><span data-stu-id="983ab-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="983ab-164">`SchoolContext`クラスから派生`DbContext`、データベース接続の管理し、データに変更します。</span><span class="sxs-lookup"><span data-stu-id="983ab-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="983ab-165">`Student`クラス、適用される属性に注意してください、 `FirstName`、 `LastName`、および`Year`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="983ab-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="983ab-166">このチュートリアルは、データ検証用のこれらの属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="983ab-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="983ab-167">コードを簡略化するのには、これらのプロパティのみがデータ検証属性でマークされます。</span><span class="sxs-lookup"><span data-stu-id="983ab-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="983ab-168">実際のプロジェクトでは、検証を必要とするすべてのプロパティに検証属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="983ab-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="983ab-169">UniversityModels.cs を保存します。</span><span class="sxs-lookup"><span data-stu-id="983ab-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="983ab-170">クラスに基づくデータベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="983ab-170">Set up the database based on classes</span></span>

<span data-ttu-id="983ab-171">このチュートリアルでは[Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/)オブジェクトを作成し、データベース テーブル。</span><span class="sxs-lookup"><span data-stu-id="983ab-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="983ab-172">これらのテーブルでは、受講者とそのコースに関する情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="983ab-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="983ab-173">選択**ツール** > **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="983ab-174">**パッケージ マネージャー コンソール**、このコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="983ab-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="983ab-175">コマンドが正常に完了すると、移行を有効になっていることを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="983ab-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![移行を有効にします。](retrieving-data/_static/image8.png)

      <span data-ttu-id="983ab-177">という名前のファイルに注意してください*Configuration.cs*が作成されました。</span><span class="sxs-lookup"><span data-stu-id="983ab-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="983ab-178">`Configuration`クラスには、`Seed`メソッドは、事前にテスト データ、データベース テーブルに設定できます。</span><span class="sxs-lookup"><span data-stu-id="983ab-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="983ab-179">データベースを事前設定します。</span><span class="sxs-lookup"><span data-stu-id="983ab-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="983ab-180">Configuration.cs を開きます。</span><span class="sxs-lookup"><span data-stu-id="983ab-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="983ab-181">`Seed` メソッドに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="983ab-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="983ab-182">また、追加、`using`のステートメント、`ContosoUniversityModelBinding. Models`名前空間。</span><span class="sxs-lookup"><span data-stu-id="983ab-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="983ab-183">Configuration.cs を保存します。</span><span class="sxs-lookup"><span data-stu-id="983ab-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="983ab-184">パッケージ マネージャー コンソールで、コマンドを実行**追加移行の初期**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="983ab-185">コマンドを実行**更新データベース**します。</span><span class="sxs-lookup"><span data-stu-id="983ab-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="983ab-186">このコマンドを実行するときに例外が発生した場合、`StudentID`と`CourseID`値が異なる可能性があります、`Seed`メソッドの値。</span><span class="sxs-lookup"><span data-stu-id="983ab-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="983ab-187">これらのデータベース テーブルを開きの既存の値を検索`StudentID`と`CourseID`します。</span><span class="sxs-lookup"><span data-stu-id="983ab-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="983ab-188">これらの値をシード処理のコードを追加、`Enrollments`テーブル。</span><span class="sxs-lookup"><span data-stu-id="983ab-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="983ab-189">GridView コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="983ab-189">Add a GridView control</span></span>

<span data-ttu-id="983ab-190">設定されているデータベースのデータで、そのデータを取得し表示する準備ができましたがようになりました。</span><span class="sxs-lookup"><span data-stu-id="983ab-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="983ab-191">Students.aspx を開きます。</span><span class="sxs-lookup"><span data-stu-id="983ab-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="983ab-192">検索、`MainContent`プレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="983ab-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="983ab-193">プレース ホルダーを追加、 **GridView**コントロールをこのコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="983ab-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="983ab-194">点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="983ab-194">Things to note:</span></span>
   * <span data-ttu-id="983ab-195">設定値は、通知、 `SelectMethod` GridView 要素プロパティ。</span><span class="sxs-lookup"><span data-stu-id="983ab-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="983ab-196">この値は、次の手順で作成するは、GridView データの取得に使用する方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="983ab-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="983ab-197">`ItemType`プロパティに設定されて、`Student`クラスの前に作成します。</span><span class="sxs-lookup"><span data-stu-id="983ab-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="983ab-198">この設定では、マークアップでクラスのプロパティを参照できます。</span><span class="sxs-lookup"><span data-stu-id="983ab-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="983ab-199">たとえば、`Student`クラスという名前のコレクションには`Enrollments`します。</span><span class="sxs-lookup"><span data-stu-id="983ab-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="983ab-200">使用することができます`Item.Enrollments`そのコレクションを取得し、使用する[LINQ 構文](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq)各受講者を取得するクレジットの合計を登録します。</span><span class="sxs-lookup"><span data-stu-id="983ab-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="983ab-201">Students.aspx を保存します。</span><span class="sxs-lookup"><span data-stu-id="983ab-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="983ab-202">データを取得するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="983ab-202">Add code to retrieve data</span></span>

   <span data-ttu-id="983ab-203">Students.aspx 分離コード ファイルで指定されたメソッドを追加、`SelectMethod`値。</span><span class="sxs-lookup"><span data-stu-id="983ab-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="983ab-204">Students.aspx.cs を開きます。</span><span class="sxs-lookup"><span data-stu-id="983ab-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="983ab-205">追加`using`のステートメント、`ContosoUniversityModelBinding. Models`と`System.Data.Entity`名前空間。</span><span class="sxs-lookup"><span data-stu-id="983ab-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="983ab-206">指定したメソッドを追加`SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="983ab-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="983ab-207">`Include`句は、クエリのパフォーマンスを向上しますは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="983ab-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="983ab-208">なし、`Include`を使用して句では、データを取得[*遅延読み込み*](https://en.wikipedia.org/wiki/Lazy_loading)データの取得に関連するたびにデータベースに個別のクエリを送信する処理も行われます。</span><span class="sxs-lookup"><span data-stu-id="983ab-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="983ab-209">`Include`を使用して句では、データを取得*一括読み込み*、つまり、1 つのデータベース クエリには、すべての関連データを取得します。</span><span class="sxs-lookup"><span data-stu-id="983ab-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="983ab-210">関連するデータが使用されていない場合より多くのデータが取得されるため、一括読み込みは効率が低下します。</span><span class="sxs-lookup"><span data-stu-id="983ab-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="983ab-211">ただし、この場合、一括読み込みでは、最高のパフォーマンス レコードごとに、関連するデータが表示されるためです。</span><span class="sxs-lookup"><span data-stu-id="983ab-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="983ab-212">データ関連の読み込み時にパフォーマンスに関する考慮事項の詳細についてを参照してください、**非同期 (lazy)、Eager、および明示的な読み込みの関連データ**セクション、 [Entity Framework では、ASP.NET で関連データの読み取りMVC アプリケーション](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)記事。</span><span class="sxs-lookup"><span data-stu-id="983ab-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="983ab-213">既定では、データは、キーとしてマークされているプロパティの値で並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="983ab-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="983ab-214">追加することができます、`OrderBy`句を異なる並べ替え値を指定します。</span><span class="sxs-lookup"><span data-stu-id="983ab-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="983ab-215">この例では、既定では`StudentID`プロパティの並べ替えに使用します。</span><span class="sxs-lookup"><span data-stu-id="983ab-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="983ab-216">[並べ替え、ページング、およびデータのフィルター処理](sorting-paging-and-filtering-data.md)記事では、ユーザーを有効にすると、並べ替えの列を選択します。</span><span class="sxs-lookup"><span data-stu-id="983ab-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="983ab-217">Students.aspx.cs を保存します。</span><span class="sxs-lookup"><span data-stu-id="983ab-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="983ab-218">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="983ab-218">Run your application</span></span> 

<span data-ttu-id="983ab-219">Web アプリケーションを実行 (**F5**) に移動し、**学生**ページで、次の表示。</span><span class="sxs-lookup"><span data-stu-id="983ab-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![データを表示します。](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="983ab-221">モデル バインド メソッドの自動生成</span><span class="sxs-lookup"><span data-stu-id="983ab-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="983ab-222">このチュートリアル シリーズを通して、チュートリアルからプロジェクトに、コードを単にコピーできます。</span><span class="sxs-lookup"><span data-stu-id="983ab-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="983ab-223">ただし、このアプローチの欠点の 1 つは、モデル バインド メソッドのコードを自動的に生成する Visual Studio によって提供される機能の対応はならないことにします。</span><span class="sxs-lookup"><span data-stu-id="983ab-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="983ab-224">独自のプロジェクトで作業して、ときに自動コード生成は時間とヘルプができるように操作を実装する方法を把握するを節約できます。</span><span class="sxs-lookup"><span data-stu-id="983ab-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="983ab-225">このセクションでは、自動コード生成の機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="983ab-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="983ab-226">ここでは情報提供のみし、プロジェクトに実装する必要があるコードが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="983ab-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="983ab-227">値を設定するときに、 `SelectMethod`、 `UpdateMethod`、 `InsertMethod`、または`DeleteMethod`マークアップ コード内のプロパティ を選択できます、**新しいメソッドの作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="983ab-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![メソッドを作成します。](retrieving-data/_static/image18.png)

<span data-ttu-id="983ab-229">Visual Studio では、適切なシグネチャを持つ、分離コードでメソッドを作成するだけでなく、操作を実行する実装コードも生成されます。</span><span class="sxs-lookup"><span data-stu-id="983ab-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="983ab-230">最初に設定した場合、`ItemType`自動コード生成を使用する前にプロパティの機能、生成されたコードを使用して入力の操作。</span><span class="sxs-lookup"><span data-stu-id="983ab-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="983ab-231">たとえば、設定するときに、`UpdateMethod`プロパティでは、次のコードが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="983ab-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="983ab-232">ここでも、このコードをプロジェクトに追加する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="983ab-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="983ab-233">次のチュートリアルでは、更新、削除、および新しいデータを追加するためのメソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="983ab-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="983ab-234">まとめ</span><span class="sxs-lookup"><span data-stu-id="983ab-234">Summary</span></span>

<span data-ttu-id="983ab-235">このチュートリアルでは、データ モデル クラスを作成し、それらのクラスからデータベースを生成します。</span><span class="sxs-lookup"><span data-stu-id="983ab-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="983ab-236">テスト データには、データベース テーブルを入力します。</span><span class="sxs-lookup"><span data-stu-id="983ab-236">You filled the database tables with test data.</span></span> <span data-ttu-id="983ab-237">モデル バインド、データベースからデータを取得するために使用して GridView にデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="983ab-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="983ab-238">次の[チュートリアル](updating-deleting-and-creating-data.md)でこのシリーズでは、更新、削除、およびデータの作成を有効にします。</span><span class="sxs-lookup"><span data-stu-id="983ab-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="983ab-239">次へ</span><span class="sxs-lookup"><span data-stu-id="983ab-239">Next</span></span>](updating-deleting-and-creating-data.md)
