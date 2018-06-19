---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'ASP.NET MVC で最初の EF データベース: ビューをカスタマイズする |Microsoft ドキュメント'
author: tfitzmac
description: MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867659"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="d9490-104">ASP.NET MVC で最初の EF データベース: ビューをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="d9490-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="d9490-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d9490-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d9490-106">MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="d9490-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="d9490-107">このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d9490-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="d9490-108">生成されたコードは、データベース テーブルの列に対応します。</span><span class="sxs-lookup"><span data-stu-id="d9490-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="d9490-109">系列のこの部分は、プレゼンテーションを強化するために自動的に生成されたビューの変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="d9490-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="d9490-110">登録済みのコースを受講者の詳細に追加します。</span><span class="sxs-lookup"><span data-stu-id="d9490-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="d9490-111">生成されたコードでは、アプリケーションの適切な開始点が、必ずしも行われません、アプリケーションで必要な機能の一部。</span><span class="sxs-lookup"><span data-stu-id="d9490-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="d9490-112">アプリケーションの特定の要件を満たすためにコードをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="d9490-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="d9490-113">現時点では、アプリケーションは、選択したスチューデントの登録済みのコースを表示されません。</span><span class="sxs-lookup"><span data-stu-id="d9490-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="d9490-114">ここでは、各受講者の登録済みのコースを追加、**詳細**受講者を表示します。</span><span class="sxs-lookup"><span data-stu-id="d9490-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="d9490-115">開いている**Students/Details.cshtml**、および、最後の下&lt;/dl&gt;  タブが終了する前に&lt;/div&gt;タグは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="d9490-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="d9490-116">このコードは、選択した受講者に、Enrollment テーブルの各レコードの行を表示するテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d9490-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="d9490-117">**表示**メソッドは、式を表すオブジェクト (modelItem) の HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="d9490-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="d9490-118">表示方法を使用する (単にプロパティの値への埋め込みコード) ではなく、値は、型とその種類のテンプレートに基づく正しく書式設定を確認します。</span><span class="sxs-lookup"><span data-stu-id="d9490-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="d9490-119">この例では各式は、ループの現在のレコードを 1 つのプロパティを返し、値は、プリミティブ型をテキストとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="d9490-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="d9490-120">もう一度、受講者/インデックス ビューを参照して選択**詳細**受講者のいずれか。</span><span class="sxs-lookup"><span data-stu-id="d9490-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="d9490-121">登録済みのコースが含まれています、ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d9490-121">You will see the enrolled courses have been included in the view.</span></span>

![学生の登録を](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="d9490-123">[前へ](changing-the-database.md)
> [次へ](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="d9490-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
