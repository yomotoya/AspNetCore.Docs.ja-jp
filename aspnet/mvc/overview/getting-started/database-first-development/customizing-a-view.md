---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'チュートリアル: ASP.NET MVC アプリで EF Database First のビューをカスタマイズします。'
description: このチュートリアルでは、プレゼンテーションを強化するために自動的に生成されたビューの変更について説明します。
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236498"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="6259e-103">チュートリアル: ASP.NET MVC アプリで EF Database First のビューをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="6259e-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="6259e-104">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="6259e-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="6259e-105">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6259e-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="6259e-106">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="6259e-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="6259e-107">このチュートリアルでは、プレゼンテーションを強化するために自動的に生成されたビューの変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="6259e-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="6259e-108">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="6259e-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6259e-109">コースを受講者の詳細ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="6259e-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="6259e-110">コースがページに追加されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6259e-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6259e-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="6259e-111">Prerequisites</span></span>

* [<span data-ttu-id="6259e-112">データベースを変更します。</span><span class="sxs-lookup"><span data-stu-id="6259e-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="6259e-113">コースを受講者の詳細に追加します。</span><span class="sxs-lookup"><span data-stu-id="6259e-113">Add courses to student detail</span></span>

<span data-ttu-id="6259e-114">生成されたコードにより、アプリケーションの出発点として適していますが、こと必ずしもはできませんのすべてのアプリケーションで必要な機能。</span><span class="sxs-lookup"><span data-stu-id="6259e-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="6259e-115">アプリケーションの特定の要件を満たすためにコードをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="6259e-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="6259e-116">現時点では、アプリケーションは、選択した学生の登録済みのコースを表示されません。</span><span class="sxs-lookup"><span data-stu-id="6259e-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="6259e-117">このセクションには、各学生の登録済みのコースを追加します、**詳細**受講者を表示します。</span><span class="sxs-lookup"><span data-stu-id="6259e-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="6259e-118">開いている**ビュー** > **学生** > *Details.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="6259e-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="6259e-119">最後の下&lt;/dl&gt;タグが終了する前に&lt;/div&gt;タグは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="6259e-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="6259e-120">このコードでは、選択した受講者で、Enrollment テーブルの各レコードの行を表示するテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="6259e-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="6259e-121">**表示**メソッドは、式を表すオブジェクト (modelItem) の HTML をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="6259e-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="6259e-122">Display メソッドを使用する (コードでプロパティ値を単純に埋め込む) ではなく、値はその型と、その型のテンプレートに基づいて正しく書式設定を確認します。</span><span class="sxs-lookup"><span data-stu-id="6259e-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="6259e-123">この例では、各式はループでは、現在のレコードを 1 つのプロパティを返し、値は、プリミティブ型をテキストとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="6259e-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="6259e-124">コースが追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="6259e-124">Confirm courses are added</span></span>

<span data-ttu-id="6259e-125">ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="6259e-125">Run the solution.</span></span> <span data-ttu-id="6259e-126">をクリックして**受講者の一覧**選択と**詳細**受講者の 1 つ。</span><span class="sxs-lookup"><span data-stu-id="6259e-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="6259e-127">表示されますが、ビューには登録済みのコースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6259e-127">You will see the enrolled courses have been included in the view.</span></span>

![登録と学生](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="6259e-129">次の手順</span><span class="sxs-lookup"><span data-stu-id="6259e-129">Next steps</span></span>
<span data-ttu-id="6259e-130">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="6259e-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6259e-131">学生の詳細ページに追加されたコース</span><span class="sxs-lookup"><span data-stu-id="6259e-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="6259e-132">コースがページに追加されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="6259e-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="6259e-133">検証の要件を指定し、書式設定を表示するデータ注釈を追加する方法については、次のチュートリアルに進んでください。</span><span class="sxs-lookup"><span data-stu-id="6259e-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6259e-134">データ検証を強化します。</span><span class="sxs-lookup"><span data-stu-id="6259e-134">Enhance data validation</span></span>](enhancing-data-validation.md)
