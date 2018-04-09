---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: ASP.NET MVC で最初の EF データベース データの検証を拡張 |。Microsoft ドキュメント
author: tfitzmac
description: MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="58982-104">ASP.NET MVC で最初の EF データベース データの検証を拡張。</span><span class="sxs-lookup"><span data-stu-id="58982-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="58982-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="58982-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="58982-106">MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="58982-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="58982-107">このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="58982-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="58982-108">生成されたコードは、データベース テーブルの列に対応します。</span><span class="sxs-lookup"><span data-stu-id="58982-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="58982-109">系列のこの部分は、データ注釈検証の要件を指定し、書式設定を表示するには、データ モデルを追加するについて説明します。</span><span class="sxs-lookup"><span data-stu-id="58982-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="58982-110">コメント セクション内のユーザーからのフィードバックに基づいて、改良されました。</span><span class="sxs-lookup"><span data-stu-id="58982-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="58982-111">データ注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="58982-111">Add data annotations</span></span>

<span data-ttu-id="58982-112">以前のトピックで説明したとおり、一部のデータ検証ルールは、ユーザー入力に自動的に適用されます。</span><span class="sxs-lookup"><span data-stu-id="58982-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="58982-113">たとえば、学年プロパティの数値をのみ提供できます。</span><span class="sxs-lookup"><span data-stu-id="58982-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="58982-114">データ検証ルールを指定するには、モデル クラスをデータ注釈を追加できます。</span><span class="sxs-lookup"><span data-stu-id="58982-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="58982-115">これらの注釈は、指定したプロパティの web アプリケーション全体に適用されます。</span><span class="sxs-lookup"><span data-stu-id="58982-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="58982-116">プロパティの表示方法を変更する書式属性を適用することもできます。など、テキスト ラベルに使用する値を変更します。</span><span class="sxs-lookup"><span data-stu-id="58982-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="58982-117">このチュートリアルでは、FirstName、LastName、および MiddleName プロパティに指定された値の長さを制限するデータ注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="58982-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="58982-118">データベースにこれらの値は 50 文字までに制限されます。ただし、web アプリケーションでその文字の制限が現在適用されません。</span><span class="sxs-lookup"><span data-stu-id="58982-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="58982-119">ユーザーは、それらの値のいずれかの 50 個を超える文字を提供する場合、値をデータベースに保存しようとするときに、ページがクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="58982-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="58982-120">値は 0 ~ 4 をグレードを制限することもされます。</span><span class="sxs-lookup"><span data-stu-id="58982-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="58982-121">開く、 **Student.cs**ファイルで、**モデル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="58982-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="58982-122">クラスに次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="58982-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="58982-123">Enrollment.cs では、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="58982-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="58982-124">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="58982-124">Build the solution.</span></span>

<span data-ttu-id="58982-125">編集または受講者を作成するためのページを参照します。</span><span class="sxs-lookup"><span data-stu-id="58982-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="58982-126">50 個を超える文字を入力しようとすると、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="58982-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![エラー メッセージを表示します。](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="58982-128">登録を編集するためページを参照し、4 上のレベルを提供しようとしてください。</span><span class="sxs-lookup"><span data-stu-id="58982-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![グレード範囲エラー](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="58982-130">プロパティとクラスに適用できるデータの検証の注釈の一覧については、次を参照してください。 [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="58982-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="58982-131">メタデータ クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="58982-131">Add metadata classes</span></span>

<span data-ttu-id="58982-132">データベースを変更; されないようにする場合にモデル クラスを直接検証属性を追加ただし場合は、データベースを変更して、モデル クラスを再生成する必要がありますは、すべてのモデル クラスを適用した属性をできなくなります。</span><span class="sxs-lookup"><span data-stu-id="58982-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="58982-133">この方法には、非常に効率がよくないになり、重要な検証規則を失うことができます。</span><span class="sxs-lookup"><span data-stu-id="58982-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="58982-134">この問題を回避するのには、属性を格納するメタデータ クラスを追加できます。</span><span class="sxs-lookup"><span data-stu-id="58982-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="58982-135">メタデータ クラスのモデル クラスを関連付けるときに、これらの属性は、モデルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="58982-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="58982-136">この方法ですべてのメタデータ クラスに適用されている属性を失うことがなく、モデル クラスを再生成することができます。</span><span class="sxs-lookup"><span data-stu-id="58982-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="58982-137">**モデル**フォルダー、という名前のクラスを追加**Metadata.cs**です。</span><span class="sxs-lookup"><span data-stu-id="58982-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![メタデータ クラスを追加します。](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="58982-139">Metadata.cs でコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="58982-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="58982-140">これらのメタデータ クラスには、すべてのモデル クラスを適用した以前の検証属性が含まれます。</span><span class="sxs-lookup"><span data-stu-id="58982-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="58982-141">**表示**テキスト ラベルに使用される値を変更する属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="58982-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="58982-142">ここで、メタデータの各クラスでモデル クラスを関連付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="58982-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="58982-143">**モデル**フォルダー、という名前のクラスを追加**PartialClasses.cs**です。</span><span class="sxs-lookup"><span data-stu-id="58982-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="58982-144">ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="58982-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="58982-145">各クラスとしてマークされていることを確認、`partial`クラス、および各一致する名前空間と名前が自動的に生成するクラスとして。</span><span class="sxs-lookup"><span data-stu-id="58982-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="58982-146">メタデータの属性を部分クラスに適用することでは、データの検証属性が自動的に生成されたクラスに適用されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="58982-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="58982-147">これらの属性は、メタデータの属性が再生成されません部分クラスに適用されるため、モデル クラスを再生成するときに失われたできません。</span><span class="sxs-lookup"><span data-stu-id="58982-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="58982-148">自動的に生成されたクラスを再生成するには、ContosoModel.edmx ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="58982-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="58982-149">デザイン画面と選択をもう一度右クリックし**データベースからモデルを更新**です。</span><span class="sxs-lookup"><span data-stu-id="58982-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="58982-150">データベースを変更していない場合でもこのプロセスは、クラスを再生成します。</span><span class="sxs-lookup"><span data-stu-id="58982-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="58982-151">**更新**] タブで [**テーブル**と**完了**です。</span><span class="sxs-lookup"><span data-stu-id="58982-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![テーブルを更新します。](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="58982-153">変更を適用する ContosoModel.edmx ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="58982-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="58982-154">Student.cs ファイルまたは Enrollment.cs ファイルを開くし、以前に適用するデータの検証属性がファイルには不要になったことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="58982-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="58982-155">ただし、アプリケーションを実行し、検証規則が入力したデータをまだ適用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="58982-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="58982-156">[前へ](customizing-a-view.md)
> [次へ](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="58982-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
