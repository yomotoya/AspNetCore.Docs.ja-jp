---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'チュートリアル: ASP.NET MVC アプリで EF Database First のデータ検証を強化します。'
description: このチュートリアルでは、検証の要件を指定し、書式設定を表示するには、データ モデルへのデータ注釈の追加について説明します。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 85299d70c6cba52c1d40a42edfd429c96318134a
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236485"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="7e2f7-103">チュートリアル: ASP.NET MVC アプリで EF Database First のデータ検証を強化します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="7e2f7-104">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="7e2f7-105">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="7e2f7-106">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="7e2f7-107">このチュートリアルでは、検証の要件を指定し、書式設定を表示するには、データ モデルへのデータ注釈の追加について説明します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="7e2f7-108">[コメント] セクションのユーザーからのフィードバックに基づいて、改良されました。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="7e2f7-109">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7e2f7-110">データ注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-110">Add data annotations</span></span>
> * <span data-ttu-id="7e2f7-111">メタデータ クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e2f7-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="7e2f7-112">Prerequisites</span></span>

* [<span data-ttu-id="7e2f7-113">ビューをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="7e2f7-114">データ注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-114">Add data annotations</span></span>

<span data-ttu-id="7e2f7-115">以前のトピックで説明するように、いくつかのデータ検証規則は、ユーザーの入力に自動的に適用されます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="7e2f7-116">たとえば、グレード プロパティの数値をのみ提供できます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="7e2f7-117">複数のデータ検証規則を指定するには、モデル クラスにデータ注釈を追加できます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="7e2f7-118">これらの注釈は、指定したプロパティの web アプリケーション全体で適用されます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="7e2f7-119">プロパティの表示方法を変更する書式属性を適用することもできます。次のようにテキスト ラベルに使用される値を変更します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="7e2f7-120">このチュートリアルでは、FirstName、LastName、および MiddleName プロパティに指定された値の長さを制限するデータの注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="7e2f7-121">データベースにこれらの値は 50 文字までに制限されていますただし、web アプリケーションでその文字の制限は現在適用されません。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="7e2f7-122">ユーザーがこれらの値のいずれかの 50 を超える文字と、値をデータベースに保存しようとしています。 ページがクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="7e2f7-123">グレードは、0 から 4 までの値に制限します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="7e2f7-124">選択**モデル** > **ContosoModel.edmx** > **ContosoModel.tt**を開くと、 *Student.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="7e2f7-125">クラスには、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="7e2f7-126">開いている*Enrollment.cs*し、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="7e2f7-127">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-127">Build the solution.</span></span>

<span data-ttu-id="7e2f7-128">をクリックして**受講者の一覧**選択**編集**します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="7e2f7-129">50 を超える文字を入力しようとすると、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![エラー メッセージを表示します。](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="7e2f7-131">ホーム ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-131">Go back to the home page.</span></span> <span data-ttu-id="7e2f7-132">をクリックして**登録の一覧**選択**編集**します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="7e2f7-133">4 上のレベルを提供しようとしてください。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="7e2f7-134">このエラーが表示されます。*フィールド グレードは、0 ~ 4 にする必要があります。*</span><span class="sxs-lookup"><span data-stu-id="7e2f7-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="7e2f7-135">メタデータ クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-135">Add metadata classes</span></span>

<span data-ttu-id="7e2f7-136">を変更するデータベースが予期しないときの動作のモデル クラスを直接検証属性を追加します。ただし、データベースを変更する必要がある場合、モデル クラスを再生成する、のモデル クラスに適用した属性がすべて失われます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="7e2f7-137">このアプローチは、非常に非効率的な重要な検証規則を喪失することができます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="7e2f7-138">この問題を回避するには、属性を格納するメタデータ クラスを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="7e2f7-139">メタデータ クラスのモデル クラスを関連付けると、それらの属性は、モデルに適用されます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="7e2f7-140">この方法ですべてのメタデータ クラスに適用されている属性を失うことがなく、モデル クラスを再生成することができます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="7e2f7-141">**モデル**フォルダー、という名前のクラスを追加*Metadata.cs*します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="7e2f7-142">コードに置き換えます*Metadata.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="7e2f7-143">これらのメタデータ クラスには、すべての以前にモデル クラスに適用する必要がある検証属性が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="7e2f7-144">**表示**テキスト ラベルに使用される値を変更する属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="7e2f7-145">ここで、メタデータ クラスでモデル クラスを関連付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="7e2f7-146">**モデル**フォルダー、という名前のクラスを追加*PartialClasses.cs*します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="7e2f7-147">ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="7e2f7-148">各クラスがマークされている通知を`partial`クラス、および各一致する名前空間と名前が自動的に生成するクラスとして。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="7e2f7-149">メタデータ属性を部分クラスに適用すると、データの検証属性が自動的に生成されたクラスに適用されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="7e2f7-150">これらの属性は、メタデータ属性は再生成されません部分クラスに適用されるため、モデル クラスを再生成するときに失われたできません。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="7e2f7-151">自動的に生成されたクラスを再生成するには、開く、 *ContosoModel.edmx*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="7e2f7-152">デザイン画面を選択します右クリックして、もう一度**データベースからモデルを更新**します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="7e2f7-153">データベースを変更していない場合でもこのプロセスは、クラスを再生成します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="7e2f7-154">**更新**] タブで [**テーブル**と**完了**します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="7e2f7-155">保存、 *ContosoModel.edmx*ファイル変更を適用します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="7e2f7-156">開く、 *Student.cs*ファイルまたは*Enrollment.cs*ファイル、および以前に適用するデータの検証属性は、不要になったファイルに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="7e2f7-157">ただし、アプリケーションを実行し、データを入力すると、検証規則が適用されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e2f7-158">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7e2f7-158">Additional resources</span></span>

<span data-ttu-id="7e2f7-159">プロパティとクラスに適用できるデータ検証注釈の一覧については、次を参照してください。 [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-159">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e2f7-160">次の手順</span><span class="sxs-lookup"><span data-stu-id="7e2f7-160">Next steps</span></span>

<span data-ttu-id="7e2f7-161">このチュートリアルでは、次の作業を行いました。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-161">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7e2f7-162">追加されたデータ注釈</span><span class="sxs-lookup"><span data-stu-id="7e2f7-162">Added data annotations</span></span>
> * <span data-ttu-id="7e2f7-163">追加のメタデータ クラス</span><span class="sxs-lookup"><span data-stu-id="7e2f7-163">Added metadata classes</span></span>

<span data-ttu-id="7e2f7-164">Web アプリとデータベースを Azure に発行する方法については、次のチュートリアルに進んでください。</span><span class="sxs-lookup"><span data-stu-id="7e2f7-164">Advance to the next tutorial to learn how to publish the web app and database to Azure.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7e2f7-165">Azure に発行する</span><span class="sxs-lookup"><span data-stu-id="7e2f7-165">Publish to Azure</span></span>](publish-to-azure.md)