---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF Database First と ASP.NET MVC: ビューの生成 |Microsoft Docs'
author: tfitzmac
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: aspnetcontent
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 9c85b330ac415d8c73d58b31a5108197b754669f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835329"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="d5074-104">EF Database First と ASP.NET MVC: ビューを生成します。</span><span class="sxs-lookup"><span data-stu-id="d5074-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="d5074-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d5074-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d5074-106">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="d5074-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="d5074-107">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d5074-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="d5074-108">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="d5074-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="d5074-109">シリーズのこの部分は、ASP.NET スキャフォールディングを使用して、コント ローラーとビューを生成するについて説明します。</span><span class="sxs-lookup"><span data-stu-id="d5074-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="d5074-110">スキャフォールディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="d5074-110">Add scaffold</span></span>

<span data-ttu-id="d5074-111">モデル クラスの標準的なデータ操作を提供するコードを生成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="d5074-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="d5074-112">コードを追加するには、スキャフォールディングの項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="d5074-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="d5074-113">型のスキャフォールディングを追加することができます。 多くのオプションがあります。このチュートリアルでスキャフォールディングをコント ローラーと、前のセクションで作成した受講者と登録のモデルに対応するビューが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d5074-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="d5074-114">プロジェクトでの一貫性を維持するには、既存に、新しいコント ローラーを追加**コント ローラー**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d5074-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="d5074-115">右クリックし、**コント ローラー**フォルダー、および選択**追加**–**スキャフォールディングされた新しい項目**します。</span><span class="sxs-lookup"><span data-stu-id="d5074-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![スキャフォールディングを追加します。](generating-views/_static/image1.png)

<span data-ttu-id="d5074-117">選択、 **MVC 5 コント ローラーとビュー、Entity Framework を使用して**オプション。</span><span class="sxs-lookup"><span data-stu-id="d5074-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="d5074-118">このオプションは、コント ローラーとビューの更新、削除、作成、およびモデルのデータを表示するに生成されます。</span><span class="sxs-lookup"><span data-stu-id="d5074-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![mvc コント ローラーを追加します。](generating-views/_static/image2.png)

<span data-ttu-id="d5074-120">選択**学生**を選択して、モデルのクラス、 **ContosoUniversityEntities**コンテキスト クラス。</span><span class="sxs-lookup"><span data-stu-id="d5074-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="d5074-121">としてコント ローラー名をそのまま**StudentsController**、</span><span class="sxs-lookup"><span data-stu-id="d5074-121">Keep the controller name as **StudentsController**,</span></span>

![コント ローラーを指定します。](generating-views/_static/image3.png)

<span data-ttu-id="d5074-123">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d5074-123">Click **Add**.</span></span>

<span data-ttu-id="d5074-124">エラーが発生した場合は、前のセクションで、プロジェクトをビルドしていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d5074-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="d5074-125">そうである場合、プロジェクトのビルドを再試行してくださいし、スキャフォールディングされた項目を再度追加します。</span><span class="sxs-lookup"><span data-stu-id="d5074-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="d5074-126">コード生成プロセスが完了したら、新しいコント ローラーとビューをプロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d5074-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![ビューを表示します。](generating-views/_static/image4.png)

<span data-ttu-id="d5074-128">ここでも、同じ手順を実行しますが、登録クラスのスキャフォールディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="d5074-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="d5074-129">完了したらが必要、 **EnrollmentsController.cs**ファイル、および下にあるフォルダー**ビュー**という名前**登録**で作成、削除、詳細、編集、およびインデックス表示モード。</span><span class="sxs-lookup"><span data-stu-id="d5074-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![ビューを表示します。](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="d5074-131">新しいビューにリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="d5074-131">Add links to new views</span></span>

<span data-ttu-id="d5074-132">受講者と登録のやすく、新しいビューに移動するためのインデックス ビューに、いくつかのハイパーリンクを追加できます。</span><span class="sxs-lookup"><span data-stu-id="d5074-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="d5074-133">ファイルを開きます**Views/Home/Index.cshtml**、これは、サイトのホーム ページ。</span><span class="sxs-lookup"><span data-stu-id="d5074-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="d5074-134">Jumbotron は、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d5074-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="d5074-135">ActionLink 方法の場合は、最初のパラメーターは、リンクに表示するテキストです。</span><span class="sxs-lookup"><span data-stu-id="d5074-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="d5074-136">2 番目のパラメーターは、アクションと 3 番目のパラメーターは、コント ローラーの名前です。</span><span class="sxs-lookup"><span data-stu-id="d5074-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="d5074-137">たとえば、最初のリンクは StudentsController でインデックスの操作を指します。</span><span class="sxs-lookup"><span data-stu-id="d5074-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="d5074-138">実際のハイパーリンクは、これらの値から構成されます。</span><span class="sxs-lookup"><span data-stu-id="d5074-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="d5074-139">最初のリンクはユーザーが最終的には、 **Index.cshtml**ファイル内で、**ビュー/学生**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d5074-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="d5074-140">学生のビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="d5074-140">Display student views</span></span>

<span data-ttu-id="d5074-141">プロジェクトに正しく追加コードが、受講者の一覧を表示し、編集、作成、またはデータベース内の生徒レコードを削除することができますを確認します。</span><span class="sxs-lookup"><span data-stu-id="d5074-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="d5074-142">右クリックし、 **Views/Home/Index.cshtml**ファイル、および選択**ブラウザーで表示**します。</span><span class="sxs-lookup"><span data-stu-id="d5074-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="d5074-143">このページで、学生の一覧については、リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d5074-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="d5074-144">このページでは、このデータを変更するには、受講者とのリンクの一覧に注目してください。</span><span class="sxs-lookup"><span data-stu-id="d5074-144">On this page, notice the list of the students and links to modify this data.</span></span>

![受講者の一覧](generating-views/_static/image7.png)

<span data-ttu-id="d5074-146">をクリックして、**新規作成**リンクし、新しい学生のいくつかの値を提供します。</span><span class="sxs-lookup"><span data-stu-id="d5074-146">Click the **Create New** link and provide some values for a new student.</span></span>

![新しい学生を作成します。](generating-views/_static/image8.png)

<span data-ttu-id="d5074-148">クリックして**作成**、し、新しい学生が一覧に追加されます。</span><span class="sxs-lookup"><span data-stu-id="d5074-148">Click **Create**, and notice the new student is added to your list.</span></span>

![新しい学生の一覧](generating-views/_static/image9.png)

<span data-ttu-id="d5074-150">選択、**編集**リンク、および学生の値の一部を変更します。</span><span class="sxs-lookup"><span data-stu-id="d5074-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![学生を編集します。](generating-views/_static/image10.png)

<span data-ttu-id="d5074-152">をクリックして**保存**、および学生のレコードが変更されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d5074-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="d5074-153">最後に、選択、**削除**リンクし、クリックして、レコードを削除することを確認、**削除**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d5074-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![学生を削除します。](generating-views/_static/image11.png)

<span data-ttu-id="d5074-155">すべてのコードを記述することがなく、Student テーブルで、データに対する一般的な操作を実行するビューが追加されました。</span><span class="sxs-lookup"><span data-stu-id="d5074-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="d5074-156">お気付きかもしれませんが、フィールドのテキスト ラベルをデータベースのプロパティに基づくこと (など**LastName**) されるとは限りません web ページに表示します。</span><span class="sxs-lookup"><span data-stu-id="d5074-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="d5074-157">たとえばのラベルをしたい場合があります**姓**します。</span><span class="sxs-lookup"><span data-stu-id="d5074-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="d5074-158">チュートリアルの後半では、この表示の問題を修正します。</span><span class="sxs-lookup"><span data-stu-id="d5074-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="d5074-159">登録のビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="d5074-159">Display enrollment views</span></span>

<span data-ttu-id="d5074-160">データベースには、学生と登録のテーブルとコースと登録のテーブル間の一対多リレーションシップの間の一対多リレーションシップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d5074-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="d5074-161">登録のためのビューは、これらのリレーションシップを正しく処理します。</span><span class="sxs-lookup"><span data-stu-id="d5074-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="d5074-162">サイトと選択のホーム ページに移動し、**登録の一覧**リンクをクリックし、**新規作成**リンク。</span><span class="sxs-lookup"><span data-stu-id="d5074-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="d5074-163">ビューには、新しい登録レコードを作成するためのフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d5074-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="d5074-164">具体的には、フォームに関連するテーブルからの値で設定された 2 つのドロップダウン リストが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d5074-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![登録を作成します。](generating-views/_static/image12.png)

<span data-ttu-id="d5074-166">さらに、指定された値の検証が自動的に適用に基づいてフィールドのデータ型にします。</span><span class="sxs-lookup"><span data-stu-id="d5074-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="d5074-167">グレードでは、互換性のない値を指定しようとする場合、エラー メッセージが表示されるように、番号が必要です。</span><span class="sxs-lookup"><span data-stu-id="d5074-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![検証メッセージ](generating-views/_static/image13.png)

<span data-ttu-id="d5074-169">自動的に生成されたビューが、データベース内のデータを使用するユーザーを有効にすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d5074-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="d5074-170">このシリーズの次のチュートリアルでは、データベースを更新し、web アプリケーションに対応する変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="d5074-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d5074-171">[前へ](creating-the-web-application.md)
> [次へ](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="d5074-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
