---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: ASP.NET MVC - パート 3 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MV で jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: fd1ae746f4f134b779c7eee50cf6c840bbb7068e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="78e3e-103">ASP.NET MVC - パート 3 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用</span><span class="sxs-lookup"><span data-stu-id="78e3e-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="78e3e-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="78e3e-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="78e3e-105">このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="78e3e-106">複合型の使用</span><span class="sxs-lookup"><span data-stu-id="78e3e-106">Working with Complex Types</span></span>

<span data-ttu-id="78e3e-107">このセクションでは、アドレス クラスを作成し、表示するテンプレートを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="78e3e-108">*モデル*フォルダー、という新しいクラス ファイルを作成*Person.cs*を配置する 2 つの種類:`Person`クラスおよび`Address`クラスです。</span><span class="sxs-lookup"><span data-stu-id="78e3e-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="78e3e-109">`Person`クラスとして型指定されたプロパティに含まれる`Address`です。</span><span class="sxs-lookup"><span data-stu-id="78e3e-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="78e3e-110">`Address`型は複合型、つまりと同様に、組み込み型のいずれでもない`int`、 `string`、または`double`です。</span><span class="sxs-lookup"><span data-stu-id="78e3e-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="78e3e-111">代わりに、いくつかのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="78e3e-111">Instead, it has several properties.</span></span> <span data-ttu-id="78e3e-112">新しいクラスのコードは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="78e3e-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="78e3e-113">`Movie`コント ローラーで、次のコードを追加する`PersonDetail`ユーザー インスタンスを表示するアクション。</span><span class="sxs-lookup"><span data-stu-id="78e3e-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="78e3e-114">次のコードを追加、`Movie`を設定するコント ローラー、`Person`サンプル データが含まれるモデル。</span><span class="sxs-lookup"><span data-stu-id="78e3e-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="78e3e-115">開く、 *Views\Movies\PersonDetail.cshtml*ファイルし、次のマークアップを追加、`PersonDetail`ビュー。</span><span class="sxs-lookup"><span data-stu-id="78e3e-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="78e3e-116">Ctrl キーを押しながら f5 キーを押してアプリケーションを実行しに移動*映画/PersonDetail*です。</span><span class="sxs-lookup"><span data-stu-id="78e3e-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="78e3e-117">`PersonDetail`ビューが含まれていない、`Address`複合型は、することができますを参照してくださいこのスクリーン ショット。</span><span class="sxs-lookup"><span data-stu-id="78e3e-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="78e3e-118">(アドレスは表示されません。)</span><span class="sxs-lookup"><span data-stu-id="78e3e-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="78e3e-119">`Address`複合型になっているために、モデル データは表示されません。</span><span class="sxs-lookup"><span data-stu-id="78e3e-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="78e3e-120">アドレス情報を表示、開く、 *Views\Movies\PersonDetail.cshtml*ファイルに追加され、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="78e3e-121">完全なマークアップを`PersonDetail`次のように表示されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="78e3e-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="78e3e-122">アプリケーションを再実行し、表示、`PersonDetail`ビュー。</span><span class="sxs-lookup"><span data-stu-id="78e3e-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="78e3e-123">アドレス情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="78e3e-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="78e3e-124">複合型のテンプレートの作成</span><span class="sxs-lookup"><span data-stu-id="78e3e-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="78e3e-125">このセクションの内容を表示するために使用されるテンプレートを作成、`Address`複合型。</span><span class="sxs-lookup"><span data-stu-id="78e3e-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="78e3e-126">用のテンプレートを作成する場合、`Address`型、ASP.NET MVC に自動的にそのフォーマットに使用できるアプリケーションで任意の場所のアドレス モデル。</span><span class="sxs-lookup"><span data-stu-id="78e3e-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="78e3e-127">こうことのレンダリングを制御する方法、`Address`アプリケーション内の 1 つの場所からの型。</span><span class="sxs-lookup"><span data-stu-id="78e3e-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="78e3e-128">*Views \shared\displaytemplates*フォルダー、という名前の厳密に型指定された部分ビューを作成する**アドレス**:</span><span class="sxs-lookup"><span data-stu-id="78e3e-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="78e3e-129">をクリックして**追加**を開き、新しい*Views\Shared\DisplayTemplates\Address.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="78e3e-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="78e3e-130">新しいビューには、次の生成されたマークアップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="78e3e-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="78e3e-131">アプリケーションを実行し、表示、`PersonDetail`ビュー。</span><span class="sxs-lookup"><span data-stu-id="78e3e-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="78e3e-132">このとき、`Address`先ほど作成したテンプレートを使用して、表示、`Address`表示が次のようになるよう、複合型。</span><span class="sxs-lookup"><span data-stu-id="78e3e-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="78e3e-133">モデルの表示形式およびテンプレートを指定する方法は概要:</span><span class="sxs-lookup"><span data-stu-id="78e3e-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="78e3e-134">次のアプローチを使用して形式またはモデル プロパティのテンプレートを指定できますを見てきました。</span><span class="sxs-lookup"><span data-stu-id="78e3e-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="78e3e-135">適用する、`DisplayFormat`属性をモデル内のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="78e3e-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="78e3e-136">たとえば、次のコードでは、時間を除いた表示される日付が発生します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="78e3e-137">適用する、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性をモデルとデータ型の指定では、プロパティにします。</span><span class="sxs-lookup"><span data-stu-id="78e3e-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="78e3e-138">たとえば、次のコードが、日付、時刻なしで表示します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="78e3e-139">アプリケーションが含まれている場合、 *date.cshtml*でテンプレート、 *views \shared\displaytemplates*フォルダーまたは*Views\Movies\DisplayTemplates*フォルダー、そのテンプレートレンダリングに使用される、`DateTime`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="78e3e-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="78e3e-140">それ以外の場合、組み込みの ASP.NET テンプレート システムは、日付としてプロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="78e3e-141">画面テンプレートを作成する、 *views \shared\displaytemplates*フォルダーまたは*Views\Movies\DisplayTemplates*フォルダーの名前には、書式を設定するデータ型が一致します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="78e3e-142">たとえば、ことを説明する、 *Views\Shared\DisplayTemplates\DateTime.cshtml*表示するために使用された`DateTime`モデルに属性を追加してビューをビューにすべてのマークアップを追加することがなく、モデルのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="78e3e-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="78e3e-143">使用して、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)モデル プロパティを表示するテンプレートを指定するには、モデルの属性です。</span><span class="sxs-lookup"><span data-stu-id="78e3e-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="78e3e-144">明示的に追加するテンプレート名を表示、 [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)ビューで呼び出します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="78e3e-145">使用するアプローチは、アプリケーションで実行する必要がありますに依存します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="78e3e-146">必要な書式設定の種類だけを取得する方法を混在させることは珍しくことはできません。</span><span class="sxs-lookup"><span data-stu-id="78e3e-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="78e3e-147">次のセクションでは少し変えて、入力する方法をカスタマイズするデータを表示する方法をカスタマイズから移動します。</span><span class="sxs-lookup"><span data-stu-id="78e3e-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="78e3e-148">日付を指定する便利な方法を提供するためにアプリケーションの編集ビューに jQuery datepicker フックされます。</span><span class="sxs-lookup"><span data-stu-id="78e3e-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="78e3e-149">[前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="78e3e-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
