---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 取得と表示を使用してデータ モデルのバインディングと web フォーム |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 8153642762cf7032f335d21c8c67eac9b52ed2f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823924"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="4d8e4-104">取得して、モデル バインディングと web フォームでデータの表示</span><span class="sxs-lookup"><span data-stu-id="4d8e4-104">Retrieving and displaying data with model binding and web forms</span></span>
====================
<span data-ttu-id="4d8e4-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4d8e4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4d8e4-106">このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="4d8e4-107">モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="4d8e4-108">このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="4d8e4-109">モデル バインドのパターンは、任意のデータ アクセス テクノロジと連携します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-109">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="4d8e4-110">このチュートリアルでは、Entity Framework を使用するが、最も使い慣れたデータ アクセス テクノロジを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-110">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="4d8e4-111">GridView、ListView、DetailsView、または FormView コントロールなどのデータ バインド サーバー コントロールから選択すると、更新、削除、およびデータの作成に使用するメソッドの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-111">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="4d8e4-112">このチュートリアルでは、SelectMethod の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-112">In this tutorial, you will specify a value for the SelectMethod.</span></span>
> 
> <span data-ttu-id="4d8e4-113">そのメソッド内では、データを取得するロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-113">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="4d8e4-114">次のチュートリアルでは、UpdateMethod、InsertMethod、DeleteMethod の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-114">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
> 
> <span data-ttu-id="4d8e4-115">できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト</span><span class="sxs-lookup"><span data-stu-id="4d8e4-115">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="4d8e4-116">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-116">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="4d8e4-117">これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-117">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="4d8e4-118">チュートリアルでは、Visual Studio でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-118">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="4d8e4-119">利用できるアプリケーション、インターネット経由でホスティング プロバイダーへのデプロイで。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-119">You can also make the application available over the Internet by deploying it to a hosting provider.</span></span> <span data-ttu-id="4d8e4-120">マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、</span><span class="sxs-lookup"><span data-stu-id="4d8e4-120">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="4d8e4-121">[無料 Azure 試用版アカウント](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-121">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="4d8e4-122">Visual Studio web プロジェクトを Azure App Service Web Apps にデプロイする方法については、次を参照してください。、 [Visual Studio を使用して ASP.NET Web 配置](../../deployment/visual-studio-web-deployment/introduction.md)シリーズ。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-122">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="4d8e4-123">そのチュートリアルでは、Entity Framework Code First Migrations を使用して Azure SQL Database に SQL Server データベースをデプロイする方法も示します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-123">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4d8e4-124">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="4d8e4-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4d8e4-125">Microsoft Visual Studio 2013 または Microsoft Visual Studio Express 2013 for Web</span><span class="sxs-lookup"><span data-stu-id="4d8e4-125">Microsoft Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web</span></span>
>   
> 
> <span data-ttu-id="4d8e4-126">このチュートリアルは Visual Studio 2012 でも機能しますが、ユーザー インターフェイスとプロジェクト テンプレートでは、いくつか違いがあります。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-126">This tutorial also works with Visual Studio 2012 but there will be some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="4d8e4-127">構築します</span><span class="sxs-lookup"><span data-stu-id="4d8e4-127">What you'll build</span></span>

<span data-ttu-id="4d8e4-128">このチュートリアルではあります。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-128">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="4d8e4-129">受講者コースに登録されていると、大学を反映するデータ オブジェクトを構築します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-129">Build data objects that reflect a university with students enrolled in courses</span></span>
2. <span data-ttu-id="4d8e4-130">データベース テーブル オブジェクトを構築します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-130">Build database tables from the objects</span></span>
3. <span data-ttu-id="4d8e4-131">テスト データでデータベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-131">Populate the database with test data</span></span>
4. <span data-ttu-id="4d8e4-132">Web フォームでデータを表示</span><span class="sxs-lookup"><span data-stu-id="4d8e4-132">Display data in a web form</span></span>

## <a name="set-up-project"></a><span data-ttu-id="4d8e4-133">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-133">Set up project</span></span>

<span data-ttu-id="4d8e4-134">Visual Studio 2013 で作成する新しい**ASP.NET Web アプリケーション**と呼ばれる**ContosoUniversityModelBinding**します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-134">In Visual Studio 2013, create a new **ASP.NET Web Application** called **ContosoUniversityModelBinding**.</span></span>

![プロジェクトを作成します。](retrieving-data/_static/image2.png)

<span data-ttu-id="4d8e4-136">Web フォーム テンプレートを選択し、その他の既定のオプションのままにします。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-136">Select the Web Forms template, and leave the other default options.</span></span> <span data-ttu-id="4d8e4-137">プロジェクトをセットアップするには、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-137">Click OK to setup the project.</span></span>

![web フォームを選択します。](retrieving-data/_static/image3.png)

<span data-ttu-id="4d8e4-139">最初に、いくつかのサイトの外観をカスタマイズする小さな変更が作成されます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-139">First, you will make a couple of small changes to customize the appearance of the site.</span></span> <span data-ttu-id="4d8e4-140">開く、 **Site.Master**ファイルを開き、含める My ASP.NET アプリケーションではなく、Contoso University のタイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-140">Open the **Site.Master** file, and change the title to include Contoso University instead of My ASP.NET Application.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

<span data-ttu-id="4d8e4-141">ヘッダー テキストを変更し、**アプリケーション名**に**Contoso University**します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-141">Then, change the header text from **Application name** to **Contoso University**.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

<span data-ttu-id="4d8e4-142">また、Site.Master では、このサイトに関連するページを反映するように、ヘッダーに表示されるナビゲーション リンクを変更します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-142">Also in Site.Master, change the navigation links that appear in the header to reflect the pages that are relevant to this site.</span></span> <span data-ttu-id="4d8e4-143">いずれかの必要はありません、**について**ページまたは**連絡先**ページのでこれらのリンクを削除できます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-143">You will not need either the **About** page or the **Contact** page so those links can be removed.</span></span> <span data-ttu-id="4d8e4-144">というページにリンクする必要が代わりに、**学生**します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-144">Instead, you will need a link to a page called **Students**.</span></span> <span data-ttu-id="4d8e4-145">このページはまだ作成されていません。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-145">This page has not been created yet.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

<span data-ttu-id="4d8e4-146">保存して Site.Master を閉じます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-146">Save and close Site.Master.</span></span>

<span data-ttu-id="4d8e4-147">ここで、生徒のデータを表示するための web フォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-147">Now, you'll create the web form for displaying student data.</span></span> <span data-ttu-id="4d8e4-148">プロジェクトを右クリックし、**追加**、**新しい項目の**。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-148">Right-click your project, and **Add** a **New Item**.</span></span> <span data-ttu-id="4d8e4-149">選択、**マスター ページを使用した Web フォーム**テンプレート、名前を付けます**Students.aspx**します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-149">Select the **Web Form with Master Page** template, and name it **Students.aspx**.</span></span>

![ページを作成します。](retrieving-data/_static/image5.png)

<span data-ttu-id="4d8e4-151">選択**Site.Master**新しい web フォームのマスター ページとして。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-151">Select **Site.Master** as the master page for the new web form.</span></span>

## <a name="create-the-data-models-and-database"></a><span data-ttu-id="4d8e4-152">データ モデルとデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-152">Create the data models and database</span></span>

<span data-ttu-id="4d8e4-153">Code First Migrations を使用すると、オブジェクトと、対応するデータベース テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-153">You will use Code First Migrations to create objects and the corresponding database tables.</span></span> <span data-ttu-id="4d8e4-154">これらのテーブルでは、受講者とそのコースに関する情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-154">These tables will store information about the students and their courses.</span></span>

<span data-ttu-id="4d8e4-155">という名前の新しいクラスを追加、Models フォルダーに**UniversityModels.cs**します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-155">In the Models folder, add a new class named **UniversityModels.cs**.</span></span>

![モデル クラスを作成します。](retrieving-data/_static/image7.png)

<span data-ttu-id="4d8e4-157">このファイルでは、よう SchoolContext、学生、登録、およびコースのクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-157">In this file, define the SchoolContext, Student, Enrollment, and Course classes as follows:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

<span data-ttu-id="4d8e4-158">SchoolContext クラスは、データベース接続とデータの変更を管理する DbContext から派生します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-158">The SchoolContext class derives from DbContext, which manages the database connection and changes in the data.</span></span>

<span data-ttu-id="4d8e4-159">Student クラスで属性に適用されていることを確認、 **FirstName**、 **LastName**、および**年**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-159">In the Student class, notice the attributes that have been applied to the **FirstName**, **LastName**, and **Year** properties.</span></span> <span data-ttu-id="4d8e4-160">これらの属性は、このチュートリアルではデータの検証に使用されます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-160">These attributes will be used for data validation in this tutorial.</span></span> <span data-ttu-id="4d8e4-161">このデモ プロジェクトのコードを簡素化するには、これらのプロパティのみがデータ検証属性でマークされています。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-161">To simplify the code for this demonstration project, only these properties were marked with data-validation attributes.</span></span> <span data-ttu-id="4d8e4-162">実際のプロジェクトでは、登録とコースのクラスのプロパティなど、検証済みのデータが必要なすべてのプロパティに検証属性が適用されます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-162">In a real project, you would apply validation attributes to all properties that need validated data, such as properties in the Enrollment and Course classes.</span></span>

<span data-ttu-id="4d8e4-163">UniversityModels.cs を保存します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-163">Save UniversityModels.cs.</span></span>

<span data-ttu-id="4d8e4-164">Code First Migrations のツールを使用して、これらのクラスに基づいてデータベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-164">You will use the tools for Code First Migrations to set up a database based on these classes.</span></span>

<span data-ttu-id="4d8e4-165">**パッケージ マネージャー コンソール**のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-165">In **Package Manager Console**, run the command:</span></span>  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

<span data-ttu-id="4d8e4-166">移行を有効になっていることを示すメッセージが表示されます、コマンドが正常に完了する場合</span><span class="sxs-lookup"><span data-stu-id="4d8e4-166">If the command completes successfully you will receive a message stating migrations have been enabled,</span></span>

![移行を有効にします。](retrieving-data/_static/image8.png)

<span data-ttu-id="4d8e4-168">新しいファイルの名前に注意してください**Configuration.cs**が作成されました。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-168">Notice that a new file named **Configuration.cs** has been created.</span></span> <span data-ttu-id="4d8e4-169">Visual Studio で、作成後に自動的に、このファイルが開きます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-169">In Visual Studio, this file is automatically opened after it is created.</span></span> <span data-ttu-id="4d8e4-170">構成クラスが含まれています、**シード**メソッド テスト データをデータベース テーブルを事前に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-170">The Configuration class contains a **Seed** method which enables you to pre-populate the database tables with test data.</span></span>

<span data-ttu-id="4d8e4-171">Seed メソッドを次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-171">Add the following code to the Seed method.</span></span> <span data-ttu-id="4d8e4-172">追加する必要があります、**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**名前空間。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-172">You'll need to add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

<span data-ttu-id="4d8e4-173">Configuration.cs を保存します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-173">Save Configuration.cs.</span></span>

<span data-ttu-id="4d8e4-174">パッケージ マネージャー コンソールで、コマンドを実行`add-migration initial`します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-174">In the Package Manager Console, run the command `add-migration initial`.</span></span>

<span data-ttu-id="4d8e4-175">コマンドを実行して、`update-database`します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-175">Then, run the command `update-database`.</span></span>

<span data-ttu-id="4d8e4-176">このコマンドを実行するときに、例外が発生した場合は、Seed メソッド内の値から StudentID、CourseID の値があるさまざまなことができます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-176">If you receive an exception when running this command, it is possible that the values for StudentID and CourseID have varied from the values in the Seed method.</span></span> <span data-ttu-id="4d8e4-177">データベースでそれらのテーブルを開き、StudentID、CourseID の既存の値を検索します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-177">Open those tables in the database and find existing values for StudentID and CourseID.</span></span> <span data-ttu-id="4d8e4-178">登録のテーブルのシード処理のコードでは、これらの値を追加します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-178">Add those values in the code for seeding the Enrollments table.</span></span>

<span data-ttu-id="4d8e4-179">データベース ファイルが追加されていますが、プロジェクトが現在非表示。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-179">The database file has been added, but it is currently hidden in the project.</span></span> <span data-ttu-id="4d8e4-180">クリックして**すべてのファイル**ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-180">Click **Show All Files** to display the file.</span></span>

![すべてのファイルを表示します。](retrieving-data/_static/image10.png)

<span data-ttu-id="4d8e4-182">アプリが表示されます、.mdf ファイルに注意してください。\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-182">Notice the .mdf file now appears in the App\_Data folder.</span></span>

![データベース ファイル](retrieving-data/_static/image12.png)

<span data-ttu-id="4d8e4-184">.Mdf ファイルをダブルクリックし、サーバー エクスプ ローラーを開きます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-184">Double-click the .mdf file and open the Server Explorer.</span></span> <span data-ttu-id="4d8e4-185">これで、テーブルは存在し、データが設定されます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-185">The tables now exist and are populated with data.</span></span>

![データベース テーブル](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a><span data-ttu-id="4d8e4-187">受講者と関連テーブルからデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-187">Display data from Students and related tables</span></span>

<span data-ttu-id="4d8e4-188">データベース内のデータとそのデータを取得し、web ページに表示する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-188">With data in the database, you are now ready to retrieve that data and display it in a web page.</span></span> <span data-ttu-id="4d8e4-189">使用する、 **GridView**列と行のデータを表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-189">You will use a **GridView** control to display the data in columns and rows.</span></span>

<span data-ttu-id="4d8e4-190">Students.aspx を開き、検索、 **MainContent**プレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-190">Open Students.aspx, and locate the **MainContent** placeholder.</span></span> <span data-ttu-id="4d8e4-191">プレース ホルダーを追加、 **GridView**コントロールを次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-191">Within that placeholder, add a **GridView** control that includes the following code.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

<span data-ttu-id="4d8e4-192">いくつかのことを確認するには、このマークアップ コードで重要な概念があります。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-192">There are a couple of important concepts in this markup code for you to notice.</span></span> <span data-ttu-id="4d8e4-193">まずの値が設定されていることを確認、 **SelectMethod** GridView 要素プロパティ。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-193">First, notice that a value is set for the **SelectMethod** property in the GridView element.</span></span> <span data-ttu-id="4d8e4-194">この値は、GridView のデータを取得するために使用されるメソッドの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-194">This value specifies the name of the method that is used for retrieving data for the GridView.</span></span> <span data-ttu-id="4d8e4-195">このメソッドは、次の手順で作成します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-195">You will create this method in the next step.</span></span> <span data-ttu-id="4d8e4-196">次に、注意、 **ItemType**を先ほど作成した学生クラスにプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-196">Second, notice that the **ItemType** property is set to the Student class that you created earlier.</span></span> <span data-ttu-id="4d8e4-197">この値を設定するには、マークアップのコードでは、そのクラスのプロパティを参照できます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-197">By setting this value, you can refer to properties of that class in the markup code.</span></span> <span data-ttu-id="4d8e4-198">たとえば、Student クラスには、登録をという名前のコレクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-198">For example, the Student class contains a collection named Enrollments.</span></span> <span data-ttu-id="4d8e4-199">使用することができます**Item.Enrollments**をそのコレクションを取得し、LINQ 構文を使用して、各学生の登録済みのクレジットの合計を取得します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-199">You can use **Item.Enrollments** to retrieve that collection and then use LINQ syntax to retrieve the sum of enrolled credits for each student.</span></span>

<span data-ttu-id="4d8e4-200">分離コード ファイルで指定されているメソッドを追加する必要があります、 **SelectMethod**値。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-200">In the code-behind file, you need to add the method that is specified for the **SelectMethod** value.</span></span> <span data-ttu-id="4d8e4-201">開いている**Students.aspx.cs**、追加**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**と**System.Data.Entity**名前空間。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-201">Open **Students.aspx.cs**, and add **using** statements for the **ContosoUniversityModelBinding.Models** and **System.Data.Entity** namespaces.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

<span data-ttu-id="4d8e4-202">次に、次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-202">Then, add the following method.</span></span> <span data-ttu-id="4d8e4-203">このメソッドの名前が、SelectMethod の指定した名前と一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-203">Notice that the name of this method matches the name you provided for SelectMethod.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

<span data-ttu-id="4d8e4-204">**Include**句は、このクエリのパフォーマンスが向上しますが、クエリの動作に必須ではありません。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-204">The **Include** clause improves the performance of this query but is not essential for the query to work.</span></span> <span data-ttu-id="4d8e4-205">Include 句を指定せず、データを遅延読み込みは、データベースに別のクエリを毎回関連データの取得を送信する必要がありますを使用して取得ができます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-205">Without the Include clause, the data would be retrieved using lazy loading, which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="4d8e4-206">Include 句を指定して、データベースの 1 つのクエリですべての関連データを取得することを意味する一括読み込みを使用してデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-206">By providing the Include clause, the data is retrieved using eager loading, which means all of the related data is retrieved through a single query of the database.</span></span> <span data-ttu-id="4d8e4-207">関連データのほとんどができない場合に使用される、一括読み込みはより多くのデータが取得されるので、効率が低下します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-207">In cases where most of the related data is not be used, eager loading can be less efficient because more data is retrieved.</span></span> <span data-ttu-id="4d8e4-208">ただし、ここでは、一括読み込みは、最高のパフォーマンス レコードごとに、関連するデータが表示されるためです。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-208">However, in this case, eager loading provides the best performance because the related data is displayed for each record.</span></span>

<span data-ttu-id="4d8e4-209">データ関連の読み込み時にパフォーマンスに関する考慮事項の詳細については、「セクションを参照してください。**非同期 (lazy)、Eager、および明示的な読み込みの関連データ**で、 [ASP で Entity Framework に関連するデータの読み取り.NET MVC アプリケーション](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)トピック。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-209">For more information about performance consideration when loading related data, see the section titled **Lazy, Eager, and Explicit Loading of Related Data** in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) topic.</span></span>

<span data-ttu-id="4d8e4-210">既定では、データは、キーとしてマークされているプロパティの値で並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-210">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="4d8e4-211">別の値の並べ替えを指定する OrderBy 句を追加できます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-211">You can add an OrderBy clause to specify a different value for sorting.</span></span> <span data-ttu-id="4d8e4-212">この例での並べ替え、既定の学生 Id プロパティに使用します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-212">In this example, the default StudentID property is used for sorting.</span></span> <span data-ttu-id="4d8e4-213">[並べ替え、ページング、およびデータのフィルター処理](sorting-paging-and-filtering-data.md)トピックでは、並べ替え列を選択するユーザーにによりします。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-213">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) topic, you will enable the user to select a column for sorting.</span></span>

<span data-ttu-id="4d8e4-214">Web アプリケーションを実行し、Students ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-214">Run your web application, and navigate to the Students page.</span></span> <span data-ttu-id="4d8e4-215">Students のページには、次の生徒の情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-215">The Students page displays the following student information.</span></span>

![データを表示します。](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="4d8e4-217">モデル バインド メソッドの自動生成</span><span class="sxs-lookup"><span data-stu-id="4d8e4-217">Automatic generation of model binding methods</span></span>

<span data-ttu-id="4d8e4-218">このチュートリアル シリーズを通して、チュートリアルからプロジェクトに、コードを単にコピーできます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-218">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="4d8e4-219">ただし、このアプローチの欠点の 1 つは、モデル バインド メソッドのコードを自動的に生成する Visual Studio によって提供される機能の対応はならないことにします。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-219">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="4d8e4-220">独自のプロジェクトで作業して、ときに自動コード生成は時間とヘルプができるように操作を実装する方法を把握するを節約できます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-220">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="4d8e4-221">このセクションでは、自動コード生成の機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-221">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="4d8e4-222">ここでは情報提供のみし、プロジェクトに実装する必要があるコードが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-222">This section is only informational and does not contain any code you need to implement in your project.</span></span>

<span data-ttu-id="4d8e4-223">値を設定するときに、 **SelectMethod**、 **UpdateMethod**、 **InsertMethod**、または**DeleteMethod**マークアップのコードでのプロパティ選択することができます、**新しいメソッドの作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-223">When setting a value for the **SelectMethod**, **UpdateMethod**, **InsertMethod**, or **DeleteMethod** properties in the markup code, you can select the **Create New Method** option.</span></span>

![新しいメソッドを作成します。](retrieving-data/_static/image18.png)

<span data-ttu-id="4d8e4-225">Visual Studio では、適切なシグネチャを持つ、分離コードでメソッドを作成するだけでなく、操作を実行する際の実装コードも生成されます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-225">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to assist you with performing the operation.</span></span> <span data-ttu-id="4d8e4-226">最初に設定した場合、 **ItemType**操作のため、生成されたコードの自動コード生成機能を使用する前にプロパティがその型を使用します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-226">If you first set the **ItemType** property before using the automatic code generation feature, the generated code will use that type for the operations.</span></span> <span data-ttu-id="4d8e4-227">たとえば、UpdateMethod プロパティを設定するときに、次のコードが自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-227">For example, when setting the UpdateMethod property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="4d8e4-228">ここでも、上記のコードは、プロジェクトに追加する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-228">Again, the above code does not need to be added to your project.</span></span> <span data-ttu-id="4d8e4-229">次のチュートリアルでは、更新、削除、および新しいデータを追加するためのメソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-229">In the next tutorial, you will implement methods for updating, deleting and adding new data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4d8e4-230">まとめ</span><span class="sxs-lookup"><span data-stu-id="4d8e4-230">Conclusion</span></span>

<span data-ttu-id="4d8e4-231">このチュートリアルでは、データ モデル クラスを作成し、それらのクラスからデータベースを生成します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-231">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="4d8e4-232">テスト データには、データベース テーブルを入力します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-232">You filled the database tables with test data.</span></span> <span data-ttu-id="4d8e4-233">モデル バインド、データベースからデータを取得するために使用して GridView にデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-233">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="4d8e4-234">次の[チュートリアル](updating-deleting-and-creating-data.md)このシリーズでは有効にすると、更新、削除、およびデータを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d8e4-234">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you will enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4d8e4-235">次へ</span><span class="sxs-lookup"><span data-stu-id="4d8e4-235">Next</span></span>](updating-deleting-and-creating-data.md)
