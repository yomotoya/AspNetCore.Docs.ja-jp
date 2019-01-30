---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'チュートリアル: ASP.NET MVC アプリで EF Database First のビューを生成します。'
description: このチュートリアルでは、ASP.NET のスキャフォールディングを使用して、コント ローラーとビューを生成する重点を置いています。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 7a56c0f9197a99427bcde6103ebc69d245e8ce63
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236420"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="d3c75-103">チュートリアル: ASP.NET MVC アプリで EF Database First のビューを生成します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="d3c75-104">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="d3c75-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="d3c75-105">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="d3c75-106">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="d3c75-107">このチュートリアルでは、ASP.NET のスキャフォールディングを使用して、コント ローラーとビューを生成する重点を置いています。</span><span class="sxs-lookup"><span data-stu-id="d3c75-107">This tutorial focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="d3c75-108">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="d3c75-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d3c75-109">スキャフォールディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-109">Add scaffold</span></span>
> * <span data-ttu-id="d3c75-110">新しいビューにリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-110">Add links to new views</span></span>
> * <span data-ttu-id="d3c75-111">学生のビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-111">Display student views</span></span>
> * <span data-ttu-id="d3c75-112">登録のビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="d3c75-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="d3c75-113">Prerequisite</span></span>

* [<span data-ttu-id="d3c75-114">Web アプリケーションとデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="d3c75-115">スキャフォールディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-115">Add scaffold</span></span>

<span data-ttu-id="d3c75-116">モデル クラスの標準的なデータ操作を提供するコードを生成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="d3c75-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="d3c75-117">コードを追加するには、スキャフォールディングの項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="d3c75-118">型のスキャフォールディングを追加することができます。 多くのオプションがあります。このチュートリアルでスキャフォールディングをコント ローラーと、前のセクションで作成した受講者と登録のモデルに対応するビューが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d3c75-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="d3c75-119">プロジェクトでの一貫性を維持するには、既存に、新しいコント ローラーを追加**コント ローラー**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d3c75-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="d3c75-120">右クリックし、**コント ローラー**フォルダー、および選択**追加** > **スキャフォールディングされた新しい項目**します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="d3c75-121">選択、 **MVC 5 コント ローラーとビュー、Entity Framework を使用して**オプション。</span><span class="sxs-lookup"><span data-stu-id="d3c75-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="d3c75-122">このオプションは、コント ローラーとビューの更新、削除、作成、およびモデルのデータを表示するに生成されます。</span><span class="sxs-lookup"><span data-stu-id="d3c75-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![mvc コント ローラーを追加します。](generating-views/_static/image2.png)

<span data-ttu-id="d3c75-124">選択**学生 (ContosoSite.Models)** モデル クラスを選び、 **ContosoUniversityDataEntities (ContosoSite.Models)** コンテキスト クラスの。</span><span class="sxs-lookup"><span data-stu-id="d3c75-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="d3c75-125">としてコント ローラー名をそのまま**StudentsController**します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="d3c75-126">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3c75-126">Click **Add**.</span></span>

<span data-ttu-id="d3c75-127">エラーが発生した場合は、前のセクションで、プロジェクトをビルドしていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d3c75-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="d3c75-128">そうである場合、プロジェクトのビルドを再試行してくださいし、スキャフォールディングされた項目を再度追加します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="d3c75-129">コード生成プロセスが完了したら後に、表示される新しいコント ローラーとビュー、プロジェクトの**コント ローラー**と**ビュー** > **学生**フォルダー.</span><span class="sxs-lookup"><span data-stu-id="d3c75-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>


<span data-ttu-id="d3c75-130">同じ手順をもう一度、実行しますが、のスキャフォールディングを追加、**登録**クラス。</span><span class="sxs-lookup"><span data-stu-id="d3c75-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="d3c75-131">完了したらがある場合、 **EnrollmentsController.cs**ファイル、および下のフォルダー**ビュー**という名前**登録**を作成、削除、詳細、編集、およびインデックスのビューと。</span><span class="sxs-lookup"><span data-stu-id="d3c75-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="d3c75-132">新しいビューにリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-132">Add links to new views</span></span>

<span data-ttu-id="d3c75-133">受講者と登録のやすく、新しいビューに移動するためのインデックス ビューに、いくつかのハイパーリンクを追加できます。</span><span class="sxs-lookup"><span data-stu-id="d3c75-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="d3c75-134">ファイルを開きます**ビュー** > **ホーム** > *Index.cshtml*、これは、サイトのホーム ページ。</span><span class="sxs-lookup"><span data-stu-id="d3c75-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="d3c75-135">Jumbotron は、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="d3c75-136">ActionLink 方法の場合は、最初のパラメーターは、リンクに表示するテキストです。</span><span class="sxs-lookup"><span data-stu-id="d3c75-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="d3c75-137">2 番目のパラメーターは、アクションと 3 番目のパラメーターは、コント ローラーの名前です。</span><span class="sxs-lookup"><span data-stu-id="d3c75-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="d3c75-138">たとえば、最初のリンクは StudentsController でインデックスの操作を指します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="d3c75-139">実際のハイパーリンクは、これらの値から構成されます。</span><span class="sxs-lookup"><span data-stu-id="d3c75-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="d3c75-140">最初のリンクはユーザーが最終的には、 **Index.cshtml**ファイル内で、**ビュー/学生**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d3c75-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="d3c75-141">学生のビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-141">Display student views</span></span>

<span data-ttu-id="d3c75-142">プロジェクトに正しく追加コードが、受講者の一覧を表示し、編集、作成、またはデータベース内の生徒レコードを削除することができますを確認します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="d3c75-143">右クリックし、**ビュー** > **ホーム** > *Index.cshtml*ファイル、および選択**ブラウザーで表示**します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="d3c75-144">アプリケーションのホーム ページで次のように選択します。**受講者の一覧**します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="d3c75-145">**インデックス** ページで、このデータを変更するには、受講者とのリンクの一覧に注目してください。</span><span class="sxs-lookup"><span data-stu-id="d3c75-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="d3c75-146">選択、**新規作成**リンクし、新しい学生のいくつかの値を提供します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="d3c75-147">クリックして**作成**、し、新しい学生が一覧に追加されます。</span><span class="sxs-lookup"><span data-stu-id="d3c75-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="d3c75-148">戻り、**インデックス**] ページで、[、**編集**リンク、および学生の値の一部を変更します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="d3c75-149">をクリックして**保存**、および学生のレコードが変更されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="d3c75-150">最後に、選択、**削除**リンクし、クリックして、レコードを削除することを確認、**削除**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3c75-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="d3c75-151">すべてのコードを記述することがなく、Student テーブルで、データに対する一般的な操作を実行するビューが追加されました。</span><span class="sxs-lookup"><span data-stu-id="d3c75-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="d3c75-152">お気付きかもしれませんが、フィールドのテキスト ラベルをデータベースのプロパティに基づくこと (など**LastName**) されるとは限りません web ページに表示します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="d3c75-153">たとえばのラベルをしたい場合があります**姓**します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="d3c75-154">チュートリアルの後半では、この表示の問題を修正します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="d3c75-155">登録のビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-155">Display enrollment views</span></span>

<span data-ttu-id="d3c75-156">データベースには、学生と登録のテーブルとコースと登録のテーブル間の一対多リレーションシップの間の一対多リレーションシップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d3c75-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="d3c75-157">登録のためのビューは、これらのリレーションシップを正しく処理します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="d3c75-158">サイトと選択のホーム ページに移動し、**登録の一覧**リンクをクリックし、**新規作成**リンク。</span><span class="sxs-lookup"><span data-stu-id="d3c75-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="d3c75-159">ビューには、新しい登録レコードを作成するためのフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d3c75-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="d3c75-160">具体的には、フォームが含まれていることを確認、 **CourseID**ドロップダウン リストと**StudentID**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="d3c75-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="d3c75-161">関連テーブルからの値の両方が設定されます。</span><span class="sxs-lookup"><span data-stu-id="d3c75-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="d3c75-162">さらに、指定された値の検証が自動的に適用に基づいてフィールドのデータ型にします。</span><span class="sxs-lookup"><span data-stu-id="d3c75-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="d3c75-163">**グレード**互換性のない値を指定しようとする場合、エラー メッセージが表示されるので、番号が必要です。*グレード フィールドは数値でなければなりません。*</span><span class="sxs-lookup"><span data-stu-id="d3c75-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="d3c75-164">自動的に生成されたビューが、データベース内のデータを使用するユーザーを有効にすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="d3c75-165">このシリーズの次のチュートリアルでは、データベースを更新し、web アプリケーションに対応する変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="d3c75-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3c75-166">次の手順</span><span class="sxs-lookup"><span data-stu-id="d3c75-166">Next steps</span></span>

<span data-ttu-id="d3c75-167">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="d3c75-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d3c75-168">スキャフォールディングの追加</span><span class="sxs-lookup"><span data-stu-id="d3c75-168">Added scaffold</span></span>
> * <span data-ttu-id="d3c75-169">新しいビューへのリンクを追加しました</span><span class="sxs-lookup"><span data-stu-id="d3c75-169">Added links to new views</span></span>
> * <span data-ttu-id="d3c75-170">表示されている学生ビュー</span><span class="sxs-lookup"><span data-stu-id="d3c75-170">Displayed student views</span></span>
> * <span data-ttu-id="d3c75-171">表示されている登録ビュー</span><span class="sxs-lookup"><span data-stu-id="d3c75-171">Displayed enrollment views</span></span>

<span data-ttu-id="d3c75-172">データベースを変更する方法については、次のチュートリアルに進んでください。</span><span class="sxs-lookup"><span data-stu-id="d3c75-172">Advance to the next tutorial to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d3c75-173">データベースを変更します。</span><span class="sxs-lookup"><span data-stu-id="d3c75-173">Change the database</span></span>](changing-the-database.md)