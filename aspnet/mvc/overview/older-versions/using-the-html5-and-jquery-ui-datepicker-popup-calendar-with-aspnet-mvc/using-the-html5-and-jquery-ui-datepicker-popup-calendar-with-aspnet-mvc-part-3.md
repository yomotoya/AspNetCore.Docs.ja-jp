---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: ASP.NET MVC - パート 3 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、表示のテンプレートと、ASP.NET MV の jQuery UI datepicker ポップアップ カレンダーを操作する方法の基本を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 11f9e0a8602e9ee1feda9d0e7d0d319add7c440c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400772"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="7ec10-103">ASP.NET MVC - パート 3 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用</span><span class="sxs-lookup"><span data-stu-id="7ec10-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="7ec10-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7ec10-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="7ec10-105">このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="7ec10-106">複雑な型の使用</span><span class="sxs-lookup"><span data-stu-id="7ec10-106">Working with Complex Types</span></span>

<span data-ttu-id="7ec10-107">このセクションでは、address クラスを作成し、それを表示するテンプレートを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="7ec10-108">*モデル*フォルダーという新しいクラス ファイルを作成*Person.cs* 2 つの型を配置します。`Person`クラスおよび`Address`クラス。</span><span class="sxs-lookup"><span data-stu-id="7ec10-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="7ec10-109">`Person`クラスとして型指定されたプロパティが含まれます`Address`します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="7ec10-110">`Address`型が複合型のような組み込み型のいずれかがいない`int`、 `string`、または`double`します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="7ec10-111">代わりに、いくつかのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="7ec10-111">Instead, it has several properties.</span></span> <span data-ttu-id="7ec10-112">新しいクラスのコードは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="7ec10-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="7ec10-113">`Movie`コント ローラーで、次のコードを追加する`PersonDetail`person のインスタンスを表示するアクション。</span><span class="sxs-lookup"><span data-stu-id="7ec10-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="7ec10-114">次のコードを追加し、`Movie`を設定するコント ローラー、`Person`サンプル データ モデル。</span><span class="sxs-lookup"><span data-stu-id="7ec10-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="7ec10-115">開く、 *Views\Movies\PersonDetail.cshtml*ファイルを開き、次のマークアップを追加、`PersonDetail`ビュー。</span><span class="sxs-lookup"><span data-stu-id="7ec10-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="7ec10-116">Ctrl + f5 キーを押してアプリケーションを実行しに移動します*映画/PersonDetail*します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="7ec10-117">`PersonDetail`ビューが含まれていない、`Address`複合型は、ことがわかります。 このスクリーン ショット。</span><span class="sxs-lookup"><span data-stu-id="7ec10-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="7ec10-118">(アドレスは表示されません。)</span><span class="sxs-lookup"><span data-stu-id="7ec10-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="7ec10-119">`Address`のため、複雑な型では、モデル データは表示されません。</span><span class="sxs-lookup"><span data-stu-id="7ec10-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="7ec10-120">アドレス情報を表示するには、開く、 *Views\Movies\PersonDetail.cshtml*もう一度ファイルを開き、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="7ec10-121">完全なマークアップを`PersonDetail`ビューは次のようになりますようになりました。</span><span class="sxs-lookup"><span data-stu-id="7ec10-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="7ec10-122">アプリケーションを再度実行し、表示、`PersonDetail`ビュー。</span><span class="sxs-lookup"><span data-stu-id="7ec10-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="7ec10-123">アドレス情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7ec10-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="7ec10-124">複合型のテンプレートの作成</span><span class="sxs-lookup"><span data-stu-id="7ec10-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="7ec10-125">このセクションでは、表示するために使用されるテンプレートを作成します、`Address`複合型。</span><span class="sxs-lookup"><span data-stu-id="7ec10-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="7ec10-126">テンプレートを作成する場合、`Address`型、ASP.NET MVC に自動的に使用できるアプリケーションで任意の場所のアドレス モデルの書式を設定します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="7ec10-127">これによりのレンダリングを制御する方法、`Address`アプリケーション内の 1 つの場所からの型。</span><span class="sxs-lookup"><span data-stu-id="7ec10-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="7ec10-128">*Views \shared\displaytemplates*フォルダー、という名前の厳密に型指定された部分ビューを作成する**アドレス**:</span><span class="sxs-lookup"><span data-stu-id="7ec10-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="7ec10-129">クリックして**追加**を開き、新しい*Views\Shared\DisplayTemplates\Address.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7ec10-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="7ec10-130">新しいビューには、次の生成されたマークアップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7ec10-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="7ec10-131">アプリケーションを実行し、表示、`PersonDetail`ビュー。</span><span class="sxs-lookup"><span data-stu-id="7ec10-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="7ec10-132">今回は、`Address`作成したテンプレートを使用して、表示、`Address`複合型の表示は次の次のようになります。</span><span class="sxs-lookup"><span data-stu-id="7ec10-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="7ec10-133">モデルの表示形式とテンプレートを指定する方法は概要:</span><span class="sxs-lookup"><span data-stu-id="7ec10-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="7ec10-134">次の方法を使用して形式またはモデル プロパティのテンプレートを指定できますを見てきました。</span><span class="sxs-lookup"><span data-stu-id="7ec10-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="7ec10-135">適用、`DisplayFormat`属性をモデルのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="7ec10-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="7ec10-136">たとえば、次のコードは、時間を除いた表示される日付をなります。</span><span class="sxs-lookup"><span data-stu-id="7ec10-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="7ec10-137">適用する、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性をモデルとデータ型を指定するプロパティ。</span><span class="sxs-lookup"><span data-stu-id="7ec10-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="7ec10-138">たとえば、次のコードでは、時間を除いた表示される日付がなります。</span><span class="sxs-lookup"><span data-stu-id="7ec10-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="7ec10-139">アプリケーションが含まれている場合、 *date.cshtml*テンプレート、 *views \shared\displaytemplates*フォルダーまたは*Views\Movies\DisplayTemplates*フォルダー、そのテンプレートレンダリングに使用される、`DateTime`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="7ec10-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="7ec10-140">それ以外の場合、組み込みの ASP.NET テンプレート システムでは、日付としてプロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="7ec10-141">表示テンプレートの作成、 *views \shared\displaytemplates*フォルダーまたは*Views\Movies\DisplayTemplates*フォルダーの名前には、書式を設定するデータ型が一致します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="7ec10-142">たとえば、ことを説明する、 *Views\Shared\DisplayTemplates\DateTime.cshtml*表示するために使用された`DateTime`モデルに属性を追加して、マークアップをビューに追加することがなく、モデルのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="7ec10-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="7ec10-143">使用して、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)モデルのプロパティを表示するテンプレートを指定するには、モデルの属性。</span><span class="sxs-lookup"><span data-stu-id="7ec10-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="7ec10-144">表示テンプレート名を明示的に追加する、 [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)ビューで呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="7ec10-145">使用する方法によって異なります、アプリケーションで行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ec10-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="7ec10-146">これらの必要な形式の種類だけを取得する方法を混在させることは珍しくありません。</span><span class="sxs-lookup"><span data-stu-id="7ec10-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="7ec10-147">次のセクションで切り替えて少し変えて、入力する方法をカスタマイズするデータを表示する方法をカスタマイズから移動します。</span><span class="sxs-lookup"><span data-stu-id="7ec10-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="7ec10-148">編集ビューの日付を指定する優れた方法を提供するために、アプリケーションで jQuery datepicker フックします。</span><span class="sxs-lookup"><span data-stu-id="7ec10-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7ec10-149">[前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="7ec10-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
