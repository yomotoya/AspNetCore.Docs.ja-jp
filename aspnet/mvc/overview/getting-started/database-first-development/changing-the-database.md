---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'チュートリアル: ASP.NET MVC アプリで EF Database First のデータベースを変更します。'
description: このチュートリアルでは、データベース構造に更新を行うと、web アプリケーション全体で変更を伝達に重点を置いています。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667636"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="4b395-103">チュートリアル: ASP.NET MVC アプリで EF Database First のデータベースを変更します。</span><span class="sxs-lookup"><span data-stu-id="4b395-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="4b395-104">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="4b395-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="4b395-105">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4b395-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="4b395-106">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="4b395-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="4b395-107">このチュートリアルでは、データベース構造に更新を行うと、web アプリケーション全体で変更を伝達に重点を置いています。</span><span class="sxs-lookup"><span data-stu-id="4b395-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="4b395-108">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="4b395-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4b395-109">列の追加</span><span class="sxs-lookup"><span data-stu-id="4b395-109">Add a column</span></span>
> * <span data-ttu-id="4b395-110">ビューにプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="4b395-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b395-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="4b395-111">Prerequisites</span></span>

* [<span data-ttu-id="4b395-112">ビューの生成</span><span class="sxs-lookup"><span data-stu-id="4b395-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="4b395-113">列の追加</span><span class="sxs-lookup"><span data-stu-id="4b395-113">Add a column</span></span>

<span data-ttu-id="4b395-114">データベースのテーブルの構造を更新する場合、データ モデル、ビュー、およびコント ローラーに、変更が反映されることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b395-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="4b395-115">このチュートリアルでは、生徒のミドル ネームを記録する Student テーブルに新しい列を追加します。</span><span class="sxs-lookup"><span data-stu-id="4b395-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="4b395-116">この列を追加するには、データベース プロジェクトを開き、Student.sql ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="4b395-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="4b395-117">という名前の列を追加、デザイナーまたは T-SQL コードのどちらかを通じて**MiddleName** nvarchar (50) で NULL 値を許容します。</span><span class="sxs-lookup"><span data-stu-id="4b395-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="4b395-118">この変更をデータベース プロジェクト (または f5 キー) を起動して、ローカル データベースに配置します。</span><span class="sxs-lookup"><span data-stu-id="4b395-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="4b395-119">新しいフィールドは、テーブルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="4b395-119">The new field is added to the table.</span></span> <span data-ttu-id="4b395-120">表示されない場合、SQL Server オブジェクト エクスプ ローラーで、ウィンドウで [更新] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="4b395-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![新しい列を表示します。](changing-the-database/_static/image2.png)

<span data-ttu-id="4b395-122">新しい列、データベース テーブルに存在しますが、データ モデル クラス内に現在存在しません。</span><span class="sxs-lookup"><span data-stu-id="4b395-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="4b395-123">新しい列に含めるモデルを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4b395-123">You must update the model to include your new column.</span></span> <span data-ttu-id="4b395-124">**モデル**フォルダーを開き、 **ContosoModel.edmx**モデル ダイアグラムを表示するファイル。</span><span class="sxs-lookup"><span data-stu-id="4b395-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="4b395-125">Student モデルに MiddleName プロパティが含まれていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4b395-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="4b395-126">デザイン画面で任意の場所を右クリックし、選択**データベースからモデルを更新**します。</span><span class="sxs-lookup"><span data-stu-id="4b395-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="4b395-127">更新ウィザードで選択、**更新**タブを選び**テーブル** > **dbo** > **学生**します。</span><span class="sxs-lookup"><span data-stu-id="4b395-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="4b395-128">**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="4b395-128">Click **Finish**.</span></span>

<span data-ttu-id="4b395-129">データベース ダイアグラムには、新しい更新プロセスが完了すると、 **MiddleName**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="4b395-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="4b395-130">保存、 **ContosoModel.edmx**ファイル。</span><span class="sxs-lookup"><span data-stu-id="4b395-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="4b395-131">新しいプロパティに反映されるは、このファイルを保存する必要があります、 **Student.cs**クラス。</span><span class="sxs-lookup"><span data-stu-id="4b395-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="4b395-132">データベースとモデルを更新したようになりました。</span><span class="sxs-lookup"><span data-stu-id="4b395-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="4b395-133">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="4b395-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="4b395-134">ビューにプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="4b395-134">Add the property to the views</span></span>

<span data-ttu-id="4b395-135">残念ながら、ビューは引き続き、新しいプロパティを含んでいません。</span><span class="sxs-lookup"><span data-stu-id="4b395-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="4b395-136">ビューを更新するには、2 つのオプションがあります - もう一度、Student クラスのスキャフォールディングを追加することで、ビューを再生成するかまたは、既存のビューに新しいプロパティを手動で追加することができます。</span><span class="sxs-lookup"><span data-stu-id="4b395-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="4b395-137">このチュートリアルでは、スキャフォールディングを追加するもう一度自動的に生成されたビューをカスタマイズした変更を行っていないためです。</span><span class="sxs-lookup"><span data-stu-id="4b395-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="4b395-138">ビューを変更し、それらの変更が失われるしたくない場合、プロパティを手動で追加を検討する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4b395-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="4b395-139">ビューは再作成させるには、削除、**学生**の下のフォルダー**ビュー**、および削除、 **StudentsController**します。</span><span class="sxs-lookup"><span data-stu-id="4b395-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="4b395-140">右クリックし、**コント ローラー**フォルダーのスキャフォールディングを追加し、**学生**モデル。</span><span class="sxs-lookup"><span data-stu-id="4b395-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="4b395-141">コント ローラーの名前をもう一度、 **StudentsController**します。</span><span class="sxs-lookup"><span data-stu-id="4b395-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="4b395-142">**[追加]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="4b395-142">Select **Add**.</span></span>

<span data-ttu-id="4b395-143">ソリューションをもう一度ビルドします。</span><span class="sxs-lookup"><span data-stu-id="4b395-143">Build the solution again.</span></span> <span data-ttu-id="4b395-144">ビューには、MiddleName プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4b395-144">The views now contain the MiddleName property.</span></span>

![ミドル ネームを表示します。](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="4b395-146">次の手順</span><span class="sxs-lookup"><span data-stu-id="4b395-146">Next steps</span></span>

<span data-ttu-id="4b395-147">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="4b395-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4b395-148">列の追加</span><span class="sxs-lookup"><span data-stu-id="4b395-148">Added a column</span></span>
> * <span data-ttu-id="4b395-149">ビューにプロパティを追加</span><span class="sxs-lookup"><span data-stu-id="4b395-149">Added the property to the views</span></span>

<span data-ttu-id="4b395-150">学生のレコードの詳細を表示するためのビューをカスタマイズする方法については、次のチュートリアルに進んでください。</span><span class="sxs-lookup"><span data-stu-id="4b395-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4b395-151">ビューをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="4b395-151">Customize a view</span></span>](customizing-a-view.md)