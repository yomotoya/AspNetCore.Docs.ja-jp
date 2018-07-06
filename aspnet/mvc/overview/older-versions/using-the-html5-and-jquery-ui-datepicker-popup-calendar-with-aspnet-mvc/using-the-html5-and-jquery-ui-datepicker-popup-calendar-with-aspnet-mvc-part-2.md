---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: ASP.NET MVC - 第 2 部での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、表示のテンプレートと、ASP.NET MV の jQuery UI datepicker ポップアップ カレンダーを操作する方法の基本を説明しています.
ms.author: aspnetcontent
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 0dc9957bbb13394a4ee225d9966f4004de5714b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839308"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a><span data-ttu-id="8890e-103">ASP.NET MVC - 第 2 部での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用</span><span class="sxs-lookup"><span data-stu-id="8890e-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 2</span></span>
====================
<span data-ttu-id="8890e-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8890e-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="8890e-105">このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。</span><span class="sxs-lookup"><span data-stu-id="8890e-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="adding-an-automatic-datetime-template"></a><span data-ttu-id="8890e-106">自動の DateTime のテンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="8890e-106">Adding an Automatic DateTime Template</span></span>

<span data-ttu-id="8890e-107">このチュートリアルの最初の部分では、書式設定を明示的に指定するモデルに属性を追加する方法と、モデルを表示するために使用するテンプレートを明示的に指定する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="8890e-107">In the first part of this tutorial, you saw how you can add attributes to the model to explicitly specify formatting, and how you can explicitly specify the template that's used to render the model.</span></span> <span data-ttu-id="8890e-108">たとえば、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性を明示的に次のコードでは、書式を指定します、`ReleaseDate`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="8890e-108">For example, the [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute in the following code explicity specifies the formatting for the `ReleaseDate` property.</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

<span data-ttu-id="8890e-109">次の例では、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性を使用して、`Date`列挙型では、モデルを表示する日付テンプレートを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="8890e-109">In the following example, the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute, using the `Date` enumeration, specifies that the date template should be used to render the model.</span></span> <span data-ttu-id="8890e-110">プロジェクトで日付テンプレートがない場合は、組み込みの日付のテンプレートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8890e-110">If there's no date template in your project, the built-in date template is used.</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

<span data-ttu-id="8890e-111">ただし、ASP します。MVC では、規則上の構成を使用して、型の名前に一致するテンプレートを探すことによって型の照合を実行できます。</span><span class="sxs-lookup"><span data-stu-id="8890e-111">However, ASP.MVC can perform type matching using convention-over-configuration, by looking for a template that matches the name of a type.</span></span> <span data-ttu-id="8890e-112">これにより、属性やコードをまったく使用せず、データを自動的に書式設定テンプレートを作成できます。</span><span class="sxs-lookup"><span data-stu-id="8890e-112">This lets you create a template that automatically formats data without using any attributes or code at all.</span></span> <span data-ttu-id="8890e-113">このチュートリアルのこの部分では、型のモデルのプロパティが自動的に適用するテンプレートを作成します[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="8890e-113">For this part of the tutorial, you'll create a template that's automatically applied to model properties of type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).</span></span> <span data-ttu-id="8890e-114">属性またはその他の構成を使用して、型のすべてのモデルのプロパティを表示するために、テンプレートを使用することを指定する必要はありません[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="8890e-114">You won't need to use an attribute or other configuration to specify that the template should be used to render all model properties of type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).</span></span>

<span data-ttu-id="8890e-115">また、個々 のプロパティまたはさらには個々 のフィールドの表示をカスタマイズする方法についても学習します。</span><span class="sxs-lookup"><span data-stu-id="8890e-115">You'll also learn a way to customize the display of individual properties or even individual fields.</span></span>

<span data-ttu-id="8890e-116">を開始するには、既存の書式設定情報を削除し、アプリケーションの完全な日付を表示してみましょう。</span><span class="sxs-lookup"><span data-stu-id="8890e-116">To begin, let's remove existing formatting information and display full dates in the application.</span></span>

<span data-ttu-id="8890e-117">開く、 *Movie.cs*ファイルし、コメント アウト、`DataType`属性を`ReleaseDate`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="8890e-117">Open the *Movie.cs* file and comment out the `DataType` attribute on the `ReleaseDate` property:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

<span data-ttu-id="8890e-118">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8890e-118">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="8890e-119">なお、`ReleaseDate`の書式情報が指定されていないとき、既定であるため、日付と時刻の両方にプロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8890e-119">Notice that the `ReleaseDate` property now displays both the date and time, because that's the default when no formatting information is provided.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a><span data-ttu-id="8890e-120">新しいテンプレートをテストするための CSS スタイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="8890e-120">Adding CSS Styles for Testing New Templates</span></span>

<span data-ttu-id="8890e-121">日付の書式設定テンプレートを作成する前に、新しいテンプレートに適用できるいくつかの CSS スタイル規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="8890e-121">Before you create a template for formatting dates, you'll add a few CSS style rules that you can apply to the new templates.</span></span> <span data-ttu-id="8890e-122">レンダリングされたページで、新しいテンプレートを使用していることを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="8890e-122">These will help you verify that the rendered page is using the new template.</span></span>

<span data-ttu-id="8890e-123">開く、 *Content\Site.cs*s ファイルを開き、次の CSS 規則をファイルの末尾に追加します。</span><span class="sxs-lookup"><span data-stu-id="8890e-123">Open the *Content\Site.cs*s file and add the following CSS rules to the bottom of the file:</span></span>

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a><span data-ttu-id="8890e-124">DateTime の表示のテンプレートの追加</span><span class="sxs-lookup"><span data-stu-id="8890e-124">Adding DateTime Display Templates</span></span>

<span data-ttu-id="8890e-125">これで新しいテンプレートを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="8890e-125">Now you can create the new template.</span></span> <span data-ttu-id="8890e-126">*Views \movies*フォルダーを作成、 *DisplayTemplates*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8890e-126">In the *Views\Movies* folder, create a *DisplayTemplates* folder.</span></span>

<span data-ttu-id="8890e-127">*Views \shared*フォルダーを作成、 *DisplayTemplates*フォルダーと*EditorTemplates*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8890e-127">In the *Views\Shared* folder, create a *DisplayTemplates* folder and an *EditorTemplates* folder.</span></span>

<span data-ttu-id="8890e-128">内の表示テンプレート、 *views \shared\displaytemplates*フォルダーがすべてのコント ローラーによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="8890e-128">The display templates in the *Views\Shared\DisplayTemplates* folder will be used by all controllers.</span></span> <span data-ttu-id="8890e-129">内の表示テンプレート、 *Views\Movie\DisplayTemplates*フォルダーがでのみ使用する、`Movie`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="8890e-129">The display templates in the *Views\Movie\DisplayTemplates* folder will be used only by the `Movie` controller.</span></span> <span data-ttu-id="8890e-130">(両方のフォルダー内のテンプレートで同じ名前のテンプレートが表示された場合、 *Views\Movie\DisplayTemplates*フォルダー-特定のテンプレートは、-ビューによって返されるが優先されます、`Movie`コント ローラー)。</span><span class="sxs-lookup"><span data-stu-id="8890e-130">(If a template with the same name appears in both folders, the template in the *Views\Movie\DisplayTemplates* folder — that is, the more specific template — takes precedence for views returned by the `Movie` controller.)</span></span>

<span data-ttu-id="8890e-131">**ソリューション エクスプ ローラー**、展開、*ビュー*フォルダー、展開、 *Shared*フォルダー、およびし右クリック、 *views \shared\displaytemplates*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8890e-131">In **Solution Explorer**, expand the *Views* folder, expand the *Shared* folder, and then right-click the *Views\Shared\DisplayTemplates* folder.</span></span>

<span data-ttu-id="8890e-132">クリックして**追加** をクリックし、**ビュー**します。</span><span class="sxs-lookup"><span data-stu-id="8890e-132">Click **Add** and then click **View**.</span></span> <span data-ttu-id="8890e-133">**ビューの追加** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8890e-133">The **Add View** dialog box is displayed.</span></span>

<span data-ttu-id="8890e-134">**ビュー名**ボックスに「`DateTime`します。</span><span class="sxs-lookup"><span data-stu-id="8890e-134">In the **View name** box, type `DateTime`.</span></span> <span data-ttu-id="8890e-135">(この名前は、型の名前を一致させるために使用する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="8890e-135">(You must use this name in order to match the name of the type.)</span></span>

<span data-ttu-id="8890e-136">選択、**部分ビューとして作成**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="8890e-136">Select the **Create as a partial view** check box.</span></span> <span data-ttu-id="8890e-137">確認、**レイアウトまたはマスター ページを使用して**と**厳密に型指定されたビューを作成する**チェック ボックスが選択されていません。</span><span class="sxs-lookup"><span data-stu-id="8890e-137">Make sure that the **Use a layout or master page** and **Create a strongly-typed view** check boxes are not selected.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

<span data-ttu-id="8890e-138">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8890e-138">Click **Add**.</span></span> <span data-ttu-id="8890e-139">A *DateTime.cshtml*でテンプレートを作成、 *views \shared\displaytemplates*します。</span><span class="sxs-lookup"><span data-stu-id="8890e-139">A *DateTime.cshtml* template is created in the *Views\Shared\DisplayTemplates*.</span></span>

<span data-ttu-id="8890e-140">次の図は、*ビュー*フォルダー**ソリューション エクスプ ローラー**後、`DateTime`表示し、エディターのテンプレートが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8890e-140">The following image shows the *Views* folder in **Solution Explorer** after the `DateTime` display and editor templates are created.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

<span data-ttu-id="8890e-141">開く、 *Views\Shared\DisplayTemplates\DateTime.cshtml*ファイルを開きを使用して、次のマークアップを追加、 [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx)プロパティの時間を除いた日付として書式設定メソッドです。</span><span class="sxs-lookup"><span data-stu-id="8890e-141">Open the *Views\Shared\DisplayTemplates\DateTime.cshtml* file and add the following markup, which uses the [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) method to format the property as a date without the time.</span></span> <span data-ttu-id="8890e-142">(、`{0:d}`形式は、短い日付形式を指定します)。</span><span class="sxs-lookup"><span data-stu-id="8890e-142">(The `{0:d}` format specifies short date format.)</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

<span data-ttu-id="8890e-143">この手順を繰り返して作成、`DateTime`テンプレート、 *Views\Movie\DisplayTemplates*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8890e-143">Repeat this step to create a `DateTime` template in the *Views\Movie\DisplayTemplates* folder.</span></span> <span data-ttu-id="8890e-144">次のコードを使用して、 *Views\Movie\DisplayTemplates\DateTime.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8890e-144">Use the following code in the *Views\Movie\DisplayTemplates\DateTime.cshtml* file.</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

<span data-ttu-id="8890e-145">`loud-1` CSS クラスがこの日付を赤い太字のテキストで表示します。</span><span class="sxs-lookup"><span data-stu-id="8890e-145">The `loud-1` CSS class causes the date to be display in bold red text.</span></span> <span data-ttu-id="8890e-146">追加した、 `loud-1` CSS クラスは、この特定のテンプレートが使用されているときに簡単に確認できるように、一時的な措置としてだけです。</span><span class="sxs-lookup"><span data-stu-id="8890e-146">You added the `loud-1` CSS class just as a temporary measure so you can easily see when this particular template is being used.</span></span>

<span data-ttu-id="8890e-147">実行したが作成され、日付を表示する ASP.NET を使用してテンプレートをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="8890e-147">What you've done is created and customized templates that ASP.NET will use to display dates.</span></span> <span data-ttu-id="8890e-148">一般的なテンプレート (で、 *views \shared\displaytemplates*フォルダー) 単純な短い形式の日付が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8890e-148">The more general template (in the *Views\Shared\DisplayTemplates* folder) displays a simple short date.</span></span> <span data-ttu-id="8890e-149">具体的には、テンプレート、`Movie`コント ローラー (で、 *Views\Movies\DisplayTemplates*フォルダー) 赤い太字のテキストとしても書式設定された短い形式の日付を表示します。</span><span class="sxs-lookup"><span data-stu-id="8890e-149">The template that's specifically for the `Movie` controller (in the *Views\Movies\DisplayTemplates* folder) displays a short date that's also formatted as bold red text.</span></span>

<span data-ttu-id="8890e-150">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8890e-150">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="8890e-151">ブラウザーでは、アプリケーションのインデックス ビューをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="8890e-151">The browser renders the Index view for the application.</span></span>

<span data-ttu-id="8890e-152">`ReleaseDate`プロパティは、時間を除いた赤い太字の日付を表示するようになりました。こうことを確認する、`DateTime`テンプレート ヘルパーで、 *Views\Movies\DisplayTemplates*経由でフォルダーが選択されている、 `DateTime` 、共有フォルダーにテンプレート化されたヘルパー (*Views\Shared\DisplayTemplates*)。</span><span class="sxs-lookup"><span data-stu-id="8890e-152">The `ReleaseDate` property now displays the date in a bold red font without the time.This helps you confirm that the `DateTime` templated helper in the *Views\Movies\DisplayTemplates* folder is selected over the `DateTime` templated helper in the shared folder (*Views\Shared\DisplayTemplates*).</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

<span data-ttu-id="8890e-153">名前を変更、 *Views\Movies\DisplayTemplates\DateTime.cshtml*ファイルを*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="8890e-153">Now rename the *Views\Movies\DisplayTemplates\DateTime.cshtml* file to *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.</span></span>

<span data-ttu-id="8890e-154">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8890e-154">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="8890e-155">この時間、`ReleaseDate`プロパティは、時間と赤い太字なしで日付を表示します。</span><span class="sxs-lookup"><span data-stu-id="8890e-155">This time the `ReleaseDate` property displays a date without the time and without the bold red font.</span></span> <span data-ttu-id="8890e-156">これは、データの名前を持つテンプレートを入力することを示しています (この場合`DateTime`) に自動的にその型のすべてのモデル プロパティの表示に使用します。</span><span class="sxs-lookup"><span data-stu-id="8890e-156">This illustrates that a template that has the name of the data type (in this case `DateTime`) is automatically used to display all model properties of that type.</span></span> <span data-ttu-id="8890e-157">名前を変更した後、 *DateTime.cshtml*ファイルを*LoudDateTime.cshtml*、ASP.NET でのテンプレートを見つからなくなった、 *Views\Movies\DisplayTemplates*フォルダーを使用して*DateTime.cshtml*テンプレートから、* Views\Movies\Shared\*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8890e-157">After you renamed the *DateTime.cshtml* file to *LoudDateTime.cshtml*, ASP.NET no longer found a template in the *Views\Movies\DisplayTemplates* folder, so it used the *DateTime.cshtml* template from the *Views\Movies\Shared\* folder.</span></span>

<span data-ttu-id="8890e-158">(一致するテンプレートは大文字と小文字、大文字小文字の区別でテンプレート ファイルの名前を作成した可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="8890e-158">(The template matching is case insensitive, so you could have created the template file name with any casing.</span></span> <span data-ttu-id="8890e-159">たとえば、 *DATETIME.chstml、datetime.cshtml*、および*DaTeTiMe.cshtml*はすべてと一致、`DateTime`型です)。</span><span class="sxs-lookup"><span data-stu-id="8890e-159">For example, *DATETIME.chstml, datetime.cshtml*, and *DaTeTiMe.cshtml* would all match the `DateTime` type.)</span></span>

<span data-ttu-id="8890e-160">確認する: この時点で、`ReleaseDate`を使用してフィールドが表示されている、 *Views\Movies\DisplayTemplates\DateTime.cshtml*テンプレートでは、短い日付形式を使用してデータが表示されますが、それ以外の場合、特殊な形式は追加されません。</span><span class="sxs-lookup"><span data-stu-id="8890e-160">To review: at this point, the `ReleaseDate` field is being displayed using the *Views\Movies\DisplayTemplates\DateTime.cshtml* template, which displays the data using a short date format, but otherwise adds no special format.</span></span>

### <a name="using-uihint-to-specify-a-display-template"></a><span data-ttu-id="8890e-161">UIHint を使用して、表示用のテンプレートを指定するには</span><span class="sxs-lookup"><span data-stu-id="8890e-161">Using UIHint to Specify a Display Template</span></span>

<span data-ttu-id="8890e-162">Web アプリケーションに多くの場合`DateTime`フィールドと、既定では日付のみの形式で、それらのほとんどすべてを表示する、 *DateTime.cshtml*テンプレートは、適切なアプローチです。</span><span class="sxs-lookup"><span data-stu-id="8890e-162">If your web application has many `DateTime` fields and by default you want to display all or most of them in date-only format, the *DateTime.cshtml* template is a good approach.</span></span> <span data-ttu-id="8890e-163">しかし、ある場合は、いくつかの日付、完全な日付と時刻を表示するでしょうか。</span><span class="sxs-lookup"><span data-stu-id="8890e-163">But what if you have a few dates where you want to display the full date and time?</span></span> <span data-ttu-id="8890e-164">問題はありません。</span><span class="sxs-lookup"><span data-stu-id="8890e-164">No problem.</span></span> <span data-ttu-id="8890e-165">その他のテンプレートを作成して使用することができます、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)した完全な日付と時刻の書式を指定する属性。</span><span class="sxs-lookup"><span data-stu-id="8890e-165">You can create an additional template and use the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute to specify formatting for the full date and time.</span></span> <span data-ttu-id="8890e-166">そのテンプレートを選択的に適用できます。</span><span class="sxs-lookup"><span data-stu-id="8890e-166">You can then selectively apply that template.</span></span> <span data-ttu-id="8890e-167">使用することができます、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)モデル レベルで属性は、ビュー内でテンプレートを指定できます。</span><span class="sxs-lookup"><span data-stu-id="8890e-167">You can use the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute at the model level or you can specify the template inside a view.</span></span> <span data-ttu-id="8890e-168">このセクションで使用する方法を確認します、`UIHint`選択的に日付/時刻フィールドの一部のインスタンスの書式設定を変更する属性。</span><span class="sxs-lookup"><span data-stu-id="8890e-168">In this section, you'll see how to use the `UIHint` attribute to selectively change the formatting for some instances of date-time fields.</span></span>

<span data-ttu-id="8890e-169">開く、 *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*ファイルを開き、次の既存のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8890e-169">Open the *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* file and replace the existing code with the following:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

<span data-ttu-id="8890e-170">これによって、表示される完全な日付と時刻、緑、および大きなテキストは、CSS クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="8890e-170">This causes the full date and time to be displayed and adds the CSS class that makes the text green and large.</span></span>

<span data-ttu-id="8890e-171">開く、 *Movie.cs*追加ファイルを開き、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)属性を`ReleaseDate`プロパティは、次の例に示すように。</span><span class="sxs-lookup"><span data-stu-id="8890e-171">Open the *Movie.cs* file and add the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute to the `ReleaseDate` property, as shown in the following example:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

<span data-ttu-id="8890e-172">これは、ASP.NET MVC に伝えますを表示するとき、`ReleaseDate`プロパティ (具体的とだけ`DateTime`オブジェクト)、それを使用する必要があります、 *LoudDateTime.cshtml*テンプレート。</span><span class="sxs-lookup"><span data-stu-id="8890e-172">This tells ASP.NET MVC that when it displays the `ReleaseDate` property (specifically, and not just any `DateTime` object), it should use the *LoudDateTime.cshtml* template.</span></span>

<span data-ttu-id="8890e-173">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8890e-173">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="8890e-174">なお、`ReleaseDate`プロパティは日付と時刻を今すぐ表示すると大きい緑色のフォントでします。</span><span class="sxs-lookup"><span data-stu-id="8890e-174">Notice that the `ReleaseDate` property now displays the date and time in a large green font.</span></span>

<span data-ttu-id="8890e-175">戻り、`UIHint`属性、 *Movie.cs*ファイルし、コメント アウト、 *LoudDateTime.cshtml*テンプレートを使用しません。</span><span class="sxs-lookup"><span data-stu-id="8890e-175">Return to the `UIHint` attribute in the *Movie.cs* file and comment it out so the *LoudDateTime.cshtml* template won't be used.</span></span> <span data-ttu-id="8890e-176">アプリケーションをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="8890e-176">Run the application again.</span></span> <span data-ttu-id="8890e-177">リリース日は、大規模で緑色は表示されません。</span><span class="sxs-lookup"><span data-stu-id="8890e-177">The release date is not displayed large and green.</span></span> <span data-ttu-id="8890e-178">これにより、 *Views\Shared\DisplayTemplates\DateTime.cshtml* Index と Details ビューでテンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="8890e-178">This verifies that the *Views\Shared\DisplayTemplates\DateTime.cshtml* template is used in the Index and Details views.</span></span>

<span data-ttu-id="8890e-179">前述のように、ビューをいくつかのデータの個々 のインスタンスにテンプレートを適用するためのテンプレートを適用することもできます。</span><span class="sxs-lookup"><span data-stu-id="8890e-179">As mentioned earlier, you can also apply a template in a view, which lets you apply the template to an individual instance of some data.</span></span> <span data-ttu-id="8890e-180">開く、 *Views\Movies\Details.cshtml*ビュー。</span><span class="sxs-lookup"><span data-stu-id="8890e-180">Open the *Views\Movies\Details.cshtml* view.</span></span> <span data-ttu-id="8890e-181">追加`"LoudDateTime"`の 2 番目のパラメーターとして、 [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)を呼び出し、`ReleaseDate`フィールド。</span><span class="sxs-lookup"><span data-stu-id="8890e-181">Add `"LoudDateTime"` as the second parameter of the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call for the `ReleaseDate` field.</span></span> <span data-ttu-id="8890e-182">完成したコードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8890e-182">The completed code looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

<span data-ttu-id="8890e-183">これは、ことを指定します、`LoudDateTime`モデルにどのような属性が適用に関係なく、モデルのプロパティを表示するテンプレートを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8890e-183">This specifies that the `LoudDateTime` template should be used to display the model property, regardless of what attributes are applied to the model.</span></span>

<span data-ttu-id="8890e-184">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8890e-184">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="8890e-185">ムービーのインデックス ページを使用していることを確認、 *Views\Shared\DisplayTemplates\DateTime.cshtml*テンプレート (赤い太字)、 *Movie\Details*ページを使用して、 *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*テンプレート (大きなおよび緑)。</span><span class="sxs-lookup"><span data-stu-id="8890e-185">Verify that the movie index page is using the *Views\Shared\DisplayTemplates\DateTime.cshtml* template (red bold) and the *Movie\Details* page is using the *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* template (large and green).</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

<span data-ttu-id="8890e-186">次のセクションでは、複合型のテンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="8890e-186">In the next section, you'll create a template for a complex type.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8890e-187">[前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="8890e-187">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)</span></span>
