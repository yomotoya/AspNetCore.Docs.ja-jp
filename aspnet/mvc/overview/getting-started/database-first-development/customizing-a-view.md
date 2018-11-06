---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First と ASP.NET MVC: ビューのカスタマイズ |Microsoft Docs'
author: Rick-Anderson
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: f66e097d53514ab3842e04cd545ca626c652478a
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021211"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="a5699-104">EF Database First と ASP.NET MVC: ビューのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="a5699-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="a5699-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a5699-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a5699-106">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="a5699-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="a5699-107">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a5699-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="a5699-108">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="a5699-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="a5699-109">シリーズのこの部分は、プレゼンテーションを強化するために自動的に生成されたビューの変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="a5699-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="a5699-110">登録済みのコースを受講者の詳細に追加します。</span><span class="sxs-lookup"><span data-stu-id="a5699-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="a5699-111">生成されたコードにより、アプリケーションの出発点として適していますが、こと必ずしもはできませんのすべてのアプリケーションで必要な機能。</span><span class="sxs-lookup"><span data-stu-id="a5699-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="a5699-112">アプリケーションの特定の要件を満たすためにコードをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="a5699-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="a5699-113">現時点では、アプリケーションは、選択した学生の登録済みのコースを表示されません。</span><span class="sxs-lookup"><span data-stu-id="a5699-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="a5699-114">このセクションには、各学生の登録済みのコースを追加します、**詳細**受講者を表示します。</span><span class="sxs-lookup"><span data-stu-id="a5699-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="a5699-115">開いている**Students/Details.cshtml**、および最後の下&lt;/dl&gt;  タブが終了する前に&lt;/div&gt;タグは、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a5699-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="a5699-116">このコードでは、選択した受講者で、Enrollment テーブルの各レコードの行を表示するテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="a5699-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="a5699-117">**表示**メソッドは、式を表すオブジェクト (modelItem) の HTML をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="a5699-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="a5699-118">Display メソッドを使用する (コードでプロパティ値を単純に埋め込む) ではなく、値はその型と、その型のテンプレートに基づいて正しく書式設定を確認します。</span><span class="sxs-lookup"><span data-stu-id="a5699-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="a5699-119">この例では、各式はループでは、現在のレコードを 1 つのプロパティを返し、値は、プリミティブ型をテキストとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="a5699-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="a5699-120">もう一度/Students インデックス ビューを参照して選択**詳細**受講者の 1 つ。</span><span class="sxs-lookup"><span data-stu-id="a5699-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="a5699-121">表示されますが、ビューには登録済みのコースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a5699-121">You will see the enrolled courses have been included in the view.</span></span>

![登録と学生](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="a5699-123">[前へ](changing-the-database.md)
> [次へ](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="a5699-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
