---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: ASP.NET MVC - パート 2 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MV で jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84112316a9ace732cb7d75d7cbaeb071c72de822
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a><span data-ttu-id="7a85c-103">ASP.NET MVC - パート 2 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用</span><span class="sxs-lookup"><span data-stu-id="7a85c-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 2</span></span>
====================
<span data-ttu-id="7a85c-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7a85c-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="7a85c-105">このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="adding-an-automatic-datetime-template"></a><span data-ttu-id="7a85c-106">自動の DateTime テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-106">Adding an Automatic DateTime Template</span></span>

<span data-ttu-id="7a85c-107">このチュートリアルの最初の部分では、書式設定を明示的に指定するモデルに属性を追加する方法と、モデルを表示するために使用されるテンプレートを明示的に指定する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="7a85c-107">In the first part of this tutorial, you saw how you can add attributes to the model to explicitly specify formatting, and how you can explicitly specify the template that's used to render the model.</span></span> <span data-ttu-id="7a85c-108">たとえば、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)明示的に次のコード内の属性の書式を指定する、`ReleaseDate`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="7a85c-108">For example, the [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute in the following code explicity specifies the formatting for the `ReleaseDate` property.</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

<span data-ttu-id="7a85c-109">次の例で、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性を使用して、`Date`列挙体は、モデルを表示する日付テンプレートを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-109">In the following example, the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute, using the `Date` enumeration, specifies that the date template should be used to render the model.</span></span> <span data-ttu-id="7a85c-110">プロジェクトで日付テンプレートがない場合は、組み込み日付テンプレートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-110">If there's no date template in your project, the built-in date template is used.</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

<span data-ttu-id="7a85c-111">ただし、ASP です。MVC では、型の名前に一致するテンプレートを参照して規則を介した-構成を使用した型の照合を実行できます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-111">However, ASP.MVC can perform type matching using convention-over-configuration, by looking for a template that matches the name of a type.</span></span> <span data-ttu-id="7a85c-112">これにより、属性やコードをまったく使用せず、データを自動的に書式設定テンプレートを作成できます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-112">This lets you create a template that automatically formats data without using any attributes or code at all.</span></span> <span data-ttu-id="7a85c-113">チュートリアルのこの部分の型のモデルのプロパティを自動的に適用するテンプレートを作成するを[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="7a85c-113">For this part of the tutorial, you'll create a template that's automatically applied to model properties of type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).</span></span> <span data-ttu-id="7a85c-114">属性またはその他の構成を使用して、型のすべてのモデルのプロパティを表示するために、テンプレートを使用することを指定する必要はありません[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="7a85c-114">You won't need to use an attribute or other configuration to specify that the template should be used to render all model properties of type [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).</span></span>

<span data-ttu-id="7a85c-115">また、個々 のプロパティまたは個別のフィールドの表示をカスタマイズする方法についても学習します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-115">You'll also learn a way to customize the display of individual properties or even individual fields.</span></span>

<span data-ttu-id="7a85c-116">最初に、既存の書式情報を削除し、アプリケーションで完全な日時を表示してみましょう。</span><span class="sxs-lookup"><span data-stu-id="7a85c-116">To begin, let's remove existing formatting information and display full dates in the application.</span></span>

<span data-ttu-id="7a85c-117">開く、 *Movie.cs*ファイルおよびコメント アウト、`DataType`属性を`ReleaseDate`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="7a85c-117">Open the *Movie.cs* file and comment out the `DataType` attribute on the `ReleaseDate` property:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

<span data-ttu-id="7a85c-118">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-118">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="7a85c-119">注意して、`ReleaseDate`の書式情報を指定しない場合、既定値であるため、日付と時刻、今すぐプロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-119">Notice that the `ReleaseDate` property now displays both the date and time, because that's the default when no formatting information is provided.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a><span data-ttu-id="7a85c-120">新しいテンプレートをテストするための CSS スタイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-120">Adding CSS Styles for Testing New Templates</span></span>

<span data-ttu-id="7a85c-121">日付の書式設定テンプレートを作成する前に、新しいテンプレートに適用できるいくつかの CSS スタイル規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-121">Before you create a template for formatting dates, you'll add a few CSS style rules that you can apply to the new templates.</span></span> <span data-ttu-id="7a85c-122">表示されたページが、新しいテンプレートを使用していることを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-122">These will help you verify that the rendered page is using the new template.</span></span>

<span data-ttu-id="7a85c-123">開く、 *Content\Site.cs*s ファイルおよびファイルの末尾に次の CSS 規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-123">Open the *Content\Site.cs*s file and add the following CSS rules to the bottom of the file:</span></span>

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a><span data-ttu-id="7a85c-124">DateTime の表示テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-124">Adding DateTime Display Templates</span></span>

<span data-ttu-id="7a85c-125">ここで、新しいテンプレートを作成できます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-125">Now you can create the new template.</span></span> <span data-ttu-id="7a85c-126">*Views\Movies*フォルダーを作成、 *DisplayTemplates*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="7a85c-126">In the *Views\Movies* folder, create a *DisplayTemplates* folder.</span></span>

<span data-ttu-id="7a85c-127">*\Shared*フォルダーを作成、 *displaytemplates という*フォルダーおよび*EditorTemplates*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="7a85c-127">In the *Views\Shared* folder, create a *DisplayTemplates* folder and an *EditorTemplates* folder.</span></span>

<span data-ttu-id="7a85c-128">内の表示テンプレート、 *views \shared\displaytemplates*フォルダーは、すべてのコント ローラーによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-128">The display templates in the *Views\Shared\DisplayTemplates* folder will be used by all controllers.</span></span> <span data-ttu-id="7a85c-129">内の表示テンプレート、 *Views\Movie\DisplayTemplates*フォルダーがでのみ使用する、`Movie`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="7a85c-129">The display templates in the *Views\Movie\DisplayTemplates* folder will be used only by the `Movie` controller.</span></span> <span data-ttu-id="7a85c-130">(両方のフォルダー内のテンプレートに同じ名前のテンプレートが表示された場合、 *Views\Movie\DisplayTemplates*フォルダー — より特定のテンプレートは、— ビューによって返されるが優先、`Movie`コント ローラーです)。</span><span class="sxs-lookup"><span data-stu-id="7a85c-130">(If a template with the same name appears in both folders, the template in the *Views\Movie\DisplayTemplates* folder — that is, the more specific template — takes precedence for views returned by the `Movie` controller.)</span></span>

<span data-ttu-id="7a85c-131">**ソリューション エクスプ ローラー**、展開、*ビュー*フォルダーで、展開、 *Shared*フォルダー、およびしを右クリック、 *views \shared\displaytemplates*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="7a85c-131">In **Solution Explorer**, expand the *Views* folder, expand the *Shared* folder, and then right-click the *Views\Shared\DisplayTemplates* folder.</span></span>

<span data-ttu-id="7a85c-132">をクリックして**追加** をクリックし、**ビュー**です。</span><span class="sxs-lookup"><span data-stu-id="7a85c-132">Click **Add** and then click **View**.</span></span> <span data-ttu-id="7a85c-133">**ビューの追加** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-133">The **Add View** dialog box is displayed.</span></span>

<span data-ttu-id="7a85c-134">**ビュー名**ボックスに、入力`DateTime`です。</span><span class="sxs-lookup"><span data-stu-id="7a85c-134">In the **View name** box, type `DateTime`.</span></span> <span data-ttu-id="7a85c-135">(この名前は、型の名前と一致するために使用する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="7a85c-135">(You must use this name in order to match the name of the type.)</span></span>

<span data-ttu-id="7a85c-136">選択、**部分ビューとして作成**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="7a85c-136">Select the **Create as a partial view** check box.</span></span> <span data-ttu-id="7a85c-137">確認して、**レイアウトまたはマスター ページを使用して**と**厳密に型指定されたビューを作成する**チェック ボックスが選択されていません。</span><span class="sxs-lookup"><span data-stu-id="7a85c-137">Make sure that the **Use a layout or master page** and **Create a strongly-typed view** check boxes are not selected.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

<span data-ttu-id="7a85c-138">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7a85c-138">Click **Add**.</span></span> <span data-ttu-id="7a85c-139">A *DateTime.cshtml*でテンプレートを作成、 *views \shared\displaytemplates*です。</span><span class="sxs-lookup"><span data-stu-id="7a85c-139">A *DateTime.cshtml* template is created in the *Views\Shared\DisplayTemplates*.</span></span>

<span data-ttu-id="7a85c-140">次の図は、*ビュー*フォルダー**ソリューション エクスプ ローラー**後、`DateTime`表示およびエディターのテンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-140">The following image shows the *Views* folder in **Solution Explorer** after the `DateTime` display and editor templates are created.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

<span data-ttu-id="7a85c-141">開く、 *Views\Shared\DisplayTemplates\DateTime.cshtml*ファイルし、を使用して、次のマークアップを追加、 [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx)メソッド、プロパティを時刻のない日付として書式設定をします。</span><span class="sxs-lookup"><span data-stu-id="7a85c-141">Open the *Views\Shared\DisplayTemplates\DateTime.cshtml* file and add the following markup, which uses the [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) method to format the property as a date without the time.</span></span> <span data-ttu-id="7a85c-142">(、`{0:d}`形式を短い日付形式を指定します)。</span><span class="sxs-lookup"><span data-stu-id="7a85c-142">(The `{0:d}` format specifies short date format.)</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

<span data-ttu-id="7a85c-143">作成するには、この手順を繰り返して、`DateTime`でテンプレート、 *Views\Movie\DisplayTemplates*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="7a85c-143">Repeat this step to create a `DateTime` template in the *Views\Movie\DisplayTemplates* folder.</span></span> <span data-ttu-id="7a85c-144">次のコードを使用して、 *Views\Movie\DisplayTemplates\DateTime.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7a85c-144">Use the following code in the *Views\Movie\DisplayTemplates\DateTime.cshtml* file.</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

<span data-ttu-id="7a85c-145">`loud-1` CSS クラスが赤い太字で表示する日付。</span><span class="sxs-lookup"><span data-stu-id="7a85c-145">The `loud-1` CSS class causes the date to be display in bold red text.</span></span> <span data-ttu-id="7a85c-146">追加する、`loud-1`この特定のテンプレートが使用されているときに簡単に確認できるように一時的な措置と同様の CSS クラス。</span><span class="sxs-lookup"><span data-stu-id="7a85c-146">You added the `loud-1` CSS class just as a temporary measure so you can easily see when this particular template is being used.</span></span>

<span data-ttu-id="7a85c-147">終了したらが作成され、日付を表示する ASP.NET を使用してテンプレートをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="7a85c-147">What you've done is created and customized templates that ASP.NET will use to display dates.</span></span> <span data-ttu-id="7a85c-148">一般的なテンプレート (で、 *views \shared\displaytemplates*フォルダー) 単純な短い形式の日付が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-148">The more general template (in the *Views\Shared\DisplayTemplates* folder) displays a simple short date.</span></span> <span data-ttu-id="7a85c-149">具体的には、テンプレート、`Movie`コント ローラー (で、 *Views\Movies\DisplayTemplates*フォルダー) 赤い太字のテキストとしても書式設定されている短い日付を表示します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-149">The template that's specifically for the `Movie` controller (in the *Views\Movies\DisplayTemplates* folder) displays a short date that's also formatted as bold red text.</span></span>

<span data-ttu-id="7a85c-150">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-150">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="7a85c-151">ブラウザーでは、アプリケーションのインデックス ビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-151">The browser renders the Index view for the application.</span></span>

<span data-ttu-id="7a85c-152">`ReleaseDate`プロパティは、時刻のない赤い太字の日付を表示するようになりました。こうことを確認する、`DateTime`テンプレート ヘルパーで、 *Views\Movies\DisplayTemplates*経由でフォルダーが選択されている、 `DateTime` 、共有フォルダーにテンプレート ヘルパー (*Views\Shared\DisplayTemplates*)。</span><span class="sxs-lookup"><span data-stu-id="7a85c-152">The `ReleaseDate` property now displays the date in a bold red font without the time.This helps you confirm that the `DateTime` templated helper in the *Views\Movies\DisplayTemplates* folder is selected over the `DateTime` templated helper in the shared folder (*Views\Shared\DisplayTemplates*).</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

<span data-ttu-id="7a85c-153">名前を変更、 *Views\Movies\DisplayTemplates\DateTime.cshtml*ファイルの名前を*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="7a85c-153">Now rename the *Views\Movies\DisplayTemplates\DateTime.cshtml* file to *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.</span></span>

<span data-ttu-id="7a85c-154">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-154">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="7a85c-155">この時間、`ReleaseDate`プロパティには、時間したりせずに赤い太字の日付が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-155">This time the `ReleaseDate` property displays a date without the time and without the bold red font.</span></span> <span data-ttu-id="7a85c-156">これは、データの名前を持つテンプレート型であることを示しています (このケースで`DateTime`) をその型のすべてのモデルのプロパティの表示が自動的に使用します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-156">This illustrates that a template that has the name of the data type (in this case `DateTime`) is automatically used to display all model properties of that type.</span></span> <span data-ttu-id="7a85c-157">名前を変更した後、 *DateTime.cshtml*ファイルの名前を*LoudDateTime.cshtml*、ASP.NET でテンプレートを見つからなくなった、 *Views\Movies\DisplayTemplates*フォルダーで、使用*DateTime.cshtml*テンプレートから、* Views\Movies\Shared\*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="7a85c-157">After you renamed the *DateTime.cshtml* file to *LoudDateTime.cshtml*, ASP.NET no longer found a template in the *Views\Movies\DisplayTemplates* folder, so it used the *DateTime.cshtml* template from the *Views\Movies\Shared\* folder.</span></span>

<span data-ttu-id="7a85c-158">(一致するテンプレートは大文字小文字を区別しない、任意の大文字と小文字テンプレート ファイルの名前を作成した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7a85c-158">(The template matching is case insensitive, so you could have created the template file name with any casing.</span></span> <span data-ttu-id="7a85c-159">たとえば、 *DATETIME.chstml、datetime.cshtml*、および*DaTeTiMe.cshtml*すべて一致は、`DateTime`型です)。</span><span class="sxs-lookup"><span data-stu-id="7a85c-159">For example, *DATETIME.chstml, datetime.cshtml*, and *DaTeTiMe.cshtml* would all match the `DateTime` type.)</span></span>

<span data-ttu-id="7a85c-160">確認する: この時点で、`ReleaseDate`を使用してフィールドが表示されている、 *Views\Movies\DisplayTemplates\DateTime.cshtml*テンプレートでは、短い日付形式を使用してデータが表示されますが、それ以外の場合、特殊な形式は追加されません。</span><span class="sxs-lookup"><span data-stu-id="7a85c-160">To review: at this point, the `ReleaseDate` field is being displayed using the *Views\Movies\DisplayTemplates\DateTime.cshtml* template, which displays the data using a short date format, but otherwise adds no special format.</span></span>

### <a name="using-uihint-to-specify-a-display-template"></a><span data-ttu-id="7a85c-161">UIHint を使用して、表示用のテンプレートを指定するには</span><span class="sxs-lookup"><span data-stu-id="7a85c-161">Using UIHint to Specify a Display Template</span></span>

<span data-ttu-id="7a85c-162">場合は、web アプリケーションがある多く`DateTime`フィールドおよび日付専用の形式で、それらのほとんどすべてを表示する既定では、 *DateTime.cshtml*テンプレートは、有効なアプローチです。</span><span class="sxs-lookup"><span data-stu-id="7a85c-162">If your web application has many `DateTime` fields and by default you want to display all or most of them in date-only format, the *DateTime.cshtml* template is a good approach.</span></span> <span data-ttu-id="7a85c-163">ある場合は、いくつかの日付、完全な日付と時刻を表示するか。</span><span class="sxs-lookup"><span data-stu-id="7a85c-163">But what if you have a few dates where you want to display the full date and time?</span></span> <span data-ttu-id="7a85c-164">問題はありません。</span><span class="sxs-lookup"><span data-stu-id="7a85c-164">No problem.</span></span> <span data-ttu-id="7a85c-165">その他のテンプレートを作成して使用することができます、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)した完全な日付と時刻の書式を指定する属性。</span><span class="sxs-lookup"><span data-stu-id="7a85c-165">You can create an additional template and use the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute to specify formatting for the full date and time.</span></span> <span data-ttu-id="7a85c-166">そのテンプレートを選択的に適用できます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-166">You can then selectively apply that template.</span></span> <span data-ttu-id="7a85c-167">使用することができます、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)モデル レベルで属性がビュー内のテンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-167">You can use the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute at the model level or you can specify the template inside a view.</span></span> <span data-ttu-id="7a85c-168">このセクションで使用する方法が表示されます、`UIHint`選択的に書式を変更する、日時フィールドの一部のインスタンスの属性です。</span><span class="sxs-lookup"><span data-stu-id="7a85c-168">In this section, you'll see how to use the `UIHint` attribute to selectively change the formatting for some instances of date-time fields.</span></span>

<span data-ttu-id="7a85c-169">開く、 *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*ファイルを開き、次に、既存のコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-169">Open the *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* file and replace the existing code with the following:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

<span data-ttu-id="7a85c-170">これを表示する完全な日付と時刻とテキストは、緑、および大規模な CSS クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-170">This causes the full date and time to be displayed and adds the CSS class that makes the text green and large.</span></span>

<span data-ttu-id="7a85c-171">開く、 *Movie.cs*ファイルを追加、 [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)属性を`ReleaseDate`プロパティ、次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="7a85c-171">Open the *Movie.cs* file and add the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute to the `ReleaseDate` property, as shown in the following example:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

<span data-ttu-id="7a85c-172">これは、ことを通知 ASP.NET MVC を表示するとき、`ReleaseDate`プロパティ (具体的とだけ`DateTime`オブジェクト)、それを使用する必要があります、 *LoudDateTime.cshtml*テンプレート。</span><span class="sxs-lookup"><span data-stu-id="7a85c-172">This tells ASP.NET MVC that when it displays the `ReleaseDate` property (specifically, and not just any `DateTime` object), it should use the *LoudDateTime.cshtml* template.</span></span>

<span data-ttu-id="7a85c-173">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-173">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="7a85c-174">注意して、`ReleaseDate`プロパティが表示されます、日付と時刻を大きい緑フォントでします。</span><span class="sxs-lookup"><span data-stu-id="7a85c-174">Notice that the `ReleaseDate` property now displays the date and time in a large green font.</span></span>

<span data-ttu-id="7a85c-175">戻り、`UIHint`属性、 *Movie.cs*ファイルおよびコメント アウトため、 *LoudDateTime.cshtml*テンプレートは使用されません。</span><span class="sxs-lookup"><span data-stu-id="7a85c-175">Return to the `UIHint` attribute in the *Movie.cs* file and comment it out so the *LoudDateTime.cshtml* template won't be used.</span></span> <span data-ttu-id="7a85c-176">アプリケーションをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-176">Run the application again.</span></span> <span data-ttu-id="7a85c-177">リリース日は大きく、緑色は表示されません。</span><span class="sxs-lookup"><span data-stu-id="7a85c-177">The release date is not displayed large and green.</span></span> <span data-ttu-id="7a85c-178">あることを確認、 *Views\Shared\DisplayTemplates\DateTime.cshtml*テンプレートは、インデックスおよび詳細ビューで使用します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-178">This verifies that the *Views\Shared\DisplayTemplates\DateTime.cshtml* template is used in the Index and Details views.</span></span>

<span data-ttu-id="7a85c-179">前述のように、ビューでは、一部のデータの個々 のインスタンスに、テンプレートを適用するテンプレートを適用することもできます。</span><span class="sxs-lookup"><span data-stu-id="7a85c-179">As mentioned earlier, you can also apply a template in a view, which lets you apply the template to an individual instance of some data.</span></span> <span data-ttu-id="7a85c-180">開く、 *Views\Movies\Details.cshtml*ビュー。</span><span class="sxs-lookup"><span data-stu-id="7a85c-180">Open the *Views\Movies\Details.cshtml* view.</span></span> <span data-ttu-id="7a85c-181">追加`"LoudDateTime"`の 2 番目のパラメーターとして、 [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)を呼び出して、`ReleaseDate`フィールドです。</span><span class="sxs-lookup"><span data-stu-id="7a85c-181">Add `"LoudDateTime"` as the second parameter of the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call for the `ReleaseDate` field.</span></span> <span data-ttu-id="7a85c-182">完成したコードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="7a85c-182">The completed code looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

<span data-ttu-id="7a85c-183">これを指定する、`LoudDateTime`テンプレートを使用して、モデルにどのような属性を適用に関係なく、モデルのプロパティを表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7a85c-183">This specifies that the `LoudDateTime` template should be used to display the model property, regardless of what attributes are applied to the model.</span></span>

<span data-ttu-id="7a85c-184">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-184">Press CTRL+F5 to run the application.</span></span>

<span data-ttu-id="7a85c-185">ムービーのインデックス ページを使用していることを確認、 *Views\Shared\DisplayTemplates\DateTime.cshtml*テンプレート (赤い太字) および*Movie\Details*ページを使用して、 *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*テンプレート (大きなおよび緑)。</span><span class="sxs-lookup"><span data-stu-id="7a85c-185">Verify that the movie index page is using the *Views\Shared\DisplayTemplates\DateTime.cshtml* template (red bold) and the *Movie\Details* page is using the *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* template (large and green).</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

<span data-ttu-id="7a85c-186">次のセクションでは、複合型のテンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="7a85c-186">In the next section, you'll create a template for a complex type.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7a85c-187">[前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="7a85c-187">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)</span></span>
