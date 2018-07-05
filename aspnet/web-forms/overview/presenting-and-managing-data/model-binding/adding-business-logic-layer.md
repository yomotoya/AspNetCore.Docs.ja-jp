---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: モデル バインディングと web フォームを使用するプロジェクトに追加のビジネス ロジック層 |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: be69bf56c6a1c5bf601a47e90e4e1c67c48a760f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386501"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="ab4f0-104">モデル バインディングと web フォームを使用するプロジェクトに追加のビジネス ロジック層</span><span class="sxs-lookup"><span data-stu-id="ab4f0-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="ab4f0-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ab4f0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ab4f0-106">このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="ab4f0-107">モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="ab4f0-108">このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="ab4f0-109">このチュートリアルでは、ビジネス ロジック層でモデルのバインディングを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="ab4f0-110">現在のページ以外のオブジェクトを使用するデータ メソッドを呼び出すことを指定する OnCallingDataMethods メンバーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="ab4f0-111">このチュートリアルで作成したプロジェクトで、[以前](retrieving-data.md)系列の部分。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="ab4f0-112">できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト</span><span class="sxs-lookup"><span data-stu-id="ab4f0-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="ab4f0-113">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="ab4f0-114">これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="ab4f0-115">構築します</span><span class="sxs-lookup"><span data-stu-id="ab4f0-115">What you'll build</span></span>

<span data-ttu-id="ab4f0-116">モデル バインドでは、web ページの分離コード ファイル内、または個別のビジネス ロジック クラスで、データ操作のコードを配置することができます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="ab4f0-117">前のチュートリアルは、データ操作のコードの分離コード ファイルを使用する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="ab4f0-118">このアプローチは、小規模なサイトは、大規模なサイトを保守する際に、繰り返しおよびより難解をコーディングする可能性しますが、あります。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="ab4f0-119">プログラムで抽象化レイヤーがないために、分離コード ファイルに含まれているコードをテストする非常に難しいことができます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="ab4f0-120">データ操作のコードを集中管理するには、すべてのデータを操作するためのロジックを含むビジネス ロジック層を作成できます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="ab4f0-121">ビジネス ロジック層は、web ページから呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="ab4f0-122">このチュートリアルでは、すべてにビジネス ロジック層では、前のチュートリアルで作成したコードの移動し、ページからそのコードを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="ab4f0-123">このチュートリアルではあります。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="ab4f0-124">ビジネス ロジック層に分離コード ファイルからコードを移動します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="ab4f0-125">ビジネス ロジック層で、メソッドを呼び出す、データ バインド コントロールを変更します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="ab4f0-126">ビジネス ロジック層を作成します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-126">Create business logic layer</span></span>

<span data-ttu-id="ab4f0-127">ここで、web ページから呼び出されるクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="ab4f0-128">このクラスのメソッドのように、前のチュートリアルで使用する方法と、値プロバイダー属性が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="ab4f0-129">最初に、という名前の新しいフォルダーを追加**BLL**します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-129">First, add a new folder called **BLL**.</span></span>

![フォルダーを追加します。](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="ab4f0-131">という名前の新しいクラスを作成、BLL フォルダーで**SchoolBL.cs**します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="ab4f0-132">すべての分離コード ファイル内に置かれていたデータ操作が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="ab4f0-133">メソッドは、分離コード ファイル内のメソッドとほぼ同じですが、いくつかの変更が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="ab4f0-134">最も重要な変更に注意してがのインスタンス内からコードを実行しているされなく**ページ**クラス。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="ab4f0-135">ページ クラスが含まれています、 **tryupdatemodel に渡します**メソッドと**ModelState**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="ab4f0-136">ビジネス ロジック層にこのコードを移動すると、不要になったこれらのメンバーを呼び出すページ クラスのインスタンスがあります。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="ab4f0-137">この問題を回避するには、追加する必要があります、 **ModelMethodContext** tryupdatemodel に渡しますまたは ModelState にアクセスする任意のメソッドのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="ab4f0-138">Tryupdatemodel に渡しますを呼び出したり、ModelState を取得するには、この ModelMethodContext パラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="ab4f0-139">この新しいパラメーターに対応する web ページで何も変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="ab4f0-140">SchoolBL.cs でコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="ab4f0-141">ビジネス ロジック層からデータを取得する既存のページを修正します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="ab4f0-142">最後に、ビジネス ロジック層を使用する分離コード ファイルにクエリを使用してから、Students.aspx、AddStudent.aspx、Courses.aspx のページが変換されます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="ab4f0-143">学生や AddStudent、コースの分離コード ファイルで削除するか、次のクエリ メソッドをコメントします。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="ab4f0-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="ab4f0-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="ab4f0-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="ab4f0-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="ab4f0-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="ab4f0-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="ab4f0-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="ab4f0-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="ab4f0-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="ab4f0-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="ab4f0-149">ようになりましたが必要ないコード データ操作に関連する分離コード ファイルです。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="ab4f0-150">**OnCallingDataMethods**イベント ハンドラーでは、データ メソッドを使用するオブジェクトを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="ab4f0-151">Students.aspx では、そのイベント ハンドラーの値を追加し、ビジネス ロジック クラスのメソッドの名前に、データのメソッドの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="ab4f0-152">Students.aspx の分離コード ファイルでは、CallingDataMethods イベントのイベント ハンドラーを定義します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="ab4f0-153">このイベント ハンドラーでは、データ操作のビジネス ロジック クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="ab4f0-154">AddStudent.aspx で同様に変更します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="ab4f0-155">Courses.aspx で同様に変更します。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="ab4f0-156">アプリケーションを実行し、以前保持していたとおりに機能のすべてのページことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="ab4f0-157">検証ロジックが正常に動作することもできます。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ab4f0-158">まとめ</span><span class="sxs-lookup"><span data-stu-id="ab4f0-158">Conclusion</span></span>

<span data-ttu-id="ab4f0-159">このチュートリアルでは、データ アクセス層とビジネス ロジック層を使用するアプリケーションを再構造。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="ab4f0-160">データ コントロールがデータ操作の現在のページではないオブジェクトを使用することを指定しました。</span><span class="sxs-lookup"><span data-stu-id="ab4f0-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ab4f0-161">前へ</span><span class="sxs-lookup"><span data-stu-id="ab4f0-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
