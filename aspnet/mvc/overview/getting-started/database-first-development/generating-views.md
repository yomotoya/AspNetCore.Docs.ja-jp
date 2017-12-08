---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: "ASP.NET MVC で最初の EF データベース: ビューを生成する |Microsoft ドキュメント"
author: tfitzmac
description: "MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 5fccb3c56af0945ec448becff777a3e92dc160d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="7a29d-104">ASP.NET MVC で最初の EF データベース: ビューを生成します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="7a29d-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7a29d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7a29d-106">MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="7a29d-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="7a29d-107">このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="7a29d-108">生成されたコードは、データベース テーブルの列に対応します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="7a29d-109">系列のこの部分は、ASP.NET のスキャフォールディングを使用して、コント ローラーとビューを生成するについて説明します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="7a29d-110">スキャフォールディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-110">Add scaffold</span></span>

<span data-ttu-id="7a29d-111">標準的なデータ操作モデル クラスを提供するコードを生成する準備ができたらです。</span><span class="sxs-lookup"><span data-stu-id="7a29d-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="7a29d-112">コードを追加するには、scaffold 項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="7a29d-113">追加することができます。 スキャフォールディングの型の多くのオプションがあります。このチュートリアルでは、コント ローラーと前のセクションで作成した Student と登録のモデルに対応するビューには、スキャフォールディングが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7a29d-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="7a29d-114">プロジェクト内の一貫性を維持するために、新しいコント ローラーを追加、既存**コント ローラー**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="7a29d-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="7a29d-115">右クリックし、**コント ローラー**フォルダー、および選択**追加**–**スキャフォールディングされた新しい項目**です。</span><span class="sxs-lookup"><span data-stu-id="7a29d-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![スキャフォールディングを追加します。](generating-views/_static/image1.png)

<span data-ttu-id="7a29d-117">選択、 **MVC 5 コント ローラー、ビューがある Entity Framework を使用して**オプション。</span><span class="sxs-lookup"><span data-stu-id="7a29d-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="7a29d-118">このオプションは、コント ローラーとビューの更新、削除、作成、およびモデルにデータを表示するに生成されます。</span><span class="sxs-lookup"><span data-stu-id="7a29d-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![mvc コント ローラーを追加します。](generating-views/_static/image2.png)

<span data-ttu-id="7a29d-120">選択**学生**を選択して、モデル クラス、 **ContosoUniversityEntities**コンテキスト クラスです。</span><span class="sxs-lookup"><span data-stu-id="7a29d-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="7a29d-121">としてコント ローラーの名前をそのまま**StudentsController**、</span><span class="sxs-lookup"><span data-stu-id="7a29d-121">Keep the controller name as **StudentsController**,</span></span>

![コント ローラーを指定します。](generating-views/_static/image3.png)

<span data-ttu-id="7a29d-123">**[追加]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a29d-123">Click **Add**.</span></span>

<span data-ttu-id="7a29d-124">エラーが発生した場合は、前のセクションで、プロジェクトをビルドしていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7a29d-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="7a29d-125">場合は、プロジェクトをビルドして再度スキャフォールディングの項目を追加ください。</span><span class="sxs-lookup"><span data-stu-id="7a29d-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="7a29d-126">コードの生成処理が完了したら、新しいコント ローラーとビューをプロジェクトに表示されます。</span><span class="sxs-lookup"><span data-stu-id="7a29d-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![ビューを表示します。](generating-views/_static/image4.png)

<span data-ttu-id="7a29d-128">同じ手順をもう一度、実行しますが、登録クラス、スキャフォールディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="7a29d-129">完了したらが必要、 **EnrollmentsController.cs**ファイル、および下にあるフォルダー**ビュー**という名前**登録**の作成、削除、詳細、編集とインデックス表示モード。</span><span class="sxs-lookup"><span data-stu-id="7a29d-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![ビューを表示します。](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="7a29d-131">新しいビューへのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-131">Add links to new views</span></span>

<span data-ttu-id="7a29d-132">やすく、新しいビューに移動するための受講者との登録、インデックス ビューに、いくつかのハイパーリンクを追加できます。</span><span class="sxs-lookup"><span data-stu-id="7a29d-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="7a29d-133">ファイルを開く**Views/Home/Index.cshtml**、これは、サイトのホーム ページです。</span><span class="sxs-lookup"><span data-stu-id="7a29d-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="7a29d-134">Jumbotron、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="7a29d-135">ActionLink メソッドの最初のパラメーターは、リンクに表示するテキストです。</span><span class="sxs-lookup"><span data-stu-id="7a29d-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="7a29d-136">2 番目のパラメーターはアクションで、3 番目のパラメーターは、コント ローラーの名前です。</span><span class="sxs-lookup"><span data-stu-id="7a29d-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="7a29d-137">たとえば、最初のリンクは、StudentsController でインデックスの操作を指します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="7a29d-138">実際のハイパーリンクは、これらの値から構成されます。</span><span class="sxs-lookup"><span data-stu-id="7a29d-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="7a29d-139">最初のリンクが最終的にユーザーを受け取り、 **Index.cshtml**ファイルの場所、**ビュー/受講者**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="7a29d-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="7a29d-140">学生のビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-140">Display student views</span></span>

<span data-ttu-id="7a29d-141">プロジェクトに正しく追加コードが、生徒数の一覧を表示し、編集、作成、またはデータベースの学生レコードを削除することができますを確認します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="7a29d-142">右クリックし、 **Views/Home/Index.cshtml**ファイル、および選択した**ブラウザーで表示**です。</span><span class="sxs-lookup"><span data-stu-id="7a29d-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="7a29d-143">このページで、学生のリストへのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a29d-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="7a29d-144">このページで、このデータを変更するには、受講者とのリンクの一覧を確認します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-144">On this page, notice the list of the students and links to modify this data.</span></span>

![学生のリスト](generating-views/_static/image7.png)

<span data-ttu-id="7a29d-146">クリックして、**新規作成**リンクし、新しい学生のいくつかの値を提供します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-146">Click the **Create New** link and provide some values for a new student.</span></span>

![新しい学生を作成します。](generating-views/_static/image8.png)

<span data-ttu-id="7a29d-148">をクリックして**作成**、し、リストに新しい学生が追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-148">Click **Create**, and notice the new student is added to your list.</span></span>

![新しい学生を使用してリスト](generating-views/_static/image9.png)

<span data-ttu-id="7a29d-150">選択、**編集**リンク、および、受講者の値の一部を変更します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![学生を編集します。](generating-views/_static/image10.png)

<span data-ttu-id="7a29d-152">をクリックして**保存**、し学生のレコードが変更されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="7a29d-153">最後に、選択、**削除**リンクしをクリックして、レコードを削除することを確認、**削除**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a29d-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![学生を削除します。](generating-views/_static/image11.png)

<span data-ttu-id="7a29d-155">すべてのコードを記述することがなく、Student テーブルにデータの一般的な操作を実行するビューを追加しました。</span><span class="sxs-lookup"><span data-stu-id="7a29d-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="7a29d-156">お気付きフィールドのテキスト ラベルがデータベースのプロパティに基づくこと (など**LastName**) が指定されて必ずしも web ページに表示します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="7a29d-157">たとえば、ことをお勧めするラベル**姓**です。</span><span class="sxs-lookup"><span data-stu-id="7a29d-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="7a29d-158">このチュートリアルで後ほど、この表示の問題を修正します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="7a29d-159">登録のビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-159">Display enrollment views</span></span>

<span data-ttu-id="7a29d-160">データベースには、Student と登録との間でコースと登録のテーブル間の一対多リレーションシップの一対多リレーションシップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7a29d-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="7a29d-161">登録用のビューは、これらのリレーションシップを正しく処理されます。</span><span class="sxs-lookup"><span data-stu-id="7a29d-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="7a29d-162">サイトと選択のホーム ページに移動、**まで登録数の一覧**リンクし、**新規作成**リンクします。</span><span class="sxs-lookup"><span data-stu-id="7a29d-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="7a29d-163">ビューには、新しい登録レコードを作成するためのフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7a29d-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="7a29d-164">具体的には、フォームに関連するテーブルからの値のみが含まれている 2 つのドロップダウン リストが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![登録を作成します。](generating-views/_static/image12.png)

<span data-ttu-id="7a29d-166">さらに、指定された値の検証が自動的にに基づいて適用、フィールドのデータ型。</span><span class="sxs-lookup"><span data-stu-id="7a29d-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="7a29d-167">グレードでは、互換性のない値を指定しようとする場合、エラー メッセージが表示されるように、番号が必要です。</span><span class="sxs-lookup"><span data-stu-id="7a29d-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![検証メッセージ](generating-views/_static/image13.png)

<span data-ttu-id="7a29d-169">自動的に生成されたビューが、データベース内のデータをユーザーが操作を有効にすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7a29d-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="7a29d-170">このシリーズの次のチュートリアルでは、データベースを更新し、web アプリケーションで対応する変更を行います。</span><span class="sxs-lookup"><span data-stu-id="7a29d-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7a29d-171">[前へ](creating-the-web-application.md)
[次へ](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="7a29d-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
