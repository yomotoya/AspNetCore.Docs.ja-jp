---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: HTML5 と jQuery UI Datepicker ポップアップ カレンダーを使用して、ASP.NET mvc - パート 4 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、表示のテンプレートと、ASP.NET MV の jQuery UI datepicker ポップアップ カレンダーを操作する方法の基本を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 74bd88408c4d60032a1275aa9f8540cf710dac36
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380909"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a><span data-ttu-id="e68f3-103">ASP.NET MVC - パート 4 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用</span><span class="sxs-lookup"><span data-stu-id="e68f3-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 4</span></span>
====================
<span data-ttu-id="e68f3-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e68f3-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="e68f3-105">このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


### <a name="adding-a-template-for-editing-dates"></a><span data-ttu-id="e68f3-106">日付を編集するためのテンプレートの追加</span><span class="sxs-lookup"><span data-stu-id="e68f3-106">Adding a Template for Editing Dates</span></span>

<span data-ttu-id="e68f3-107">このセクションでは、ASP.NET MVC でマークされているモデルのプロパティを編集するための UI の表示中に適用される日付を編集用のテンプレートを作成します、**日付**の列挙体、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="e68f3-107">In this section you'll create a template for editing dates that will be applied when ASP.NET MVC displays UI for editing model properties that are marked with the **Date** enumeration of the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attibute.</span></span> <span data-ttu-id="e68f3-108">テンプレートは、日付のみを表示します。時刻は表示されません。</span><span class="sxs-lookup"><span data-stu-id="e68f3-108">The template will render only the date; time will not be displayed.</span></span> <span data-ttu-id="e68f3-109">テンプレートで使用し、 [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーの日付を編集する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-109">In the template you'll use the [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) popup calendar to provide a way to edit dates.</span></span>

<span data-ttu-id="e68f3-110">まず、開きます、 *Movie.cs*ファイルを追加、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性を**日付**列挙型を`ReleaseDate`プロパティは、次のコードに示すように。</span><span class="sxs-lookup"><span data-stu-id="e68f3-110">To begin, open the *Movie.cs* file and add the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribute with the **Date** enumeration to the `ReleaseDate` property, as shown in the following code:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

<span data-ttu-id="e68f3-111">このコードにより、`ReleaseDate`両方に時間がテンプレートを表示し、テンプレートを編集せずに表示されるフィールド。</span><span class="sxs-lookup"><span data-stu-id="e68f3-111">This code causes the `ReleaseDate` field to be displayed without the time in both display templates and edit templates.</span></span> <span data-ttu-id="e68f3-112">アプリケーションに含まれる場合、 *date.cshtml*テンプレート、 *views \shared\editortemplates*フォルダーまたは、 *Views\Movies\EditorTemplates*フォルダー、そのテンプレートいずれかを表示するために使用されます`DateTime`プロパティの編集中にします。</span><span class="sxs-lookup"><span data-stu-id="e68f3-112">If your application contains a *date.cshtml* template in the *Views\Shared\EditorTemplates* folder or in the *Views\Movies\EditorTemplates* folder, that template will be used to render any `DateTime` property while editing.</span></span> <span data-ttu-id="e68f3-113">それ以外の場合、組み込みの ASP.NET テンプレートのシステム日付として、プロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-113">Otherwise the built-in ASP.NET templating system will display the property as a date.</span></span>

<span data-ttu-id="e68f3-114">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-114">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="e68f3-115">リリース日の入力フィールドの日付のみが表示されていることを確認する編集リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-115">Select an edit link to verify that the input field for the release date is showing only the date.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

<span data-ttu-id="e68f3-116">**ソリューション エクスプ ローラー**、展開、*ビュー*フォルダー、展開、 *Shared*フォルダー、およびし右クリック、 *views \shared\editortemplates*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="e68f3-116">In **Solution Explorer**, expand the *Views* folder, expand the *Shared* folder, and then right-click the *Views\Shared\EditorTemplates* folder.</span></span>

<span data-ttu-id="e68f3-117">クリックして**追加**、 をクリックし、**ビュー**します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-117">Click **Add**, and then click **View**.</span></span> <span data-ttu-id="e68f3-118">**ビューの追加** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-118">The **Add View** dialog box is displayed.</span></span>

<span data-ttu-id="e68f3-119">**ビュー名**ボックスに「&quot;日付&quot;します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-119">In the **View name** box, type &quot;Date&quot;.</span></span>

<span data-ttu-id="e68f3-120">選択、**部分ビューとして作成**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="e68f3-120">Select the **Create as a partial view** check box.</span></span> <span data-ttu-id="e68f3-121">確認、**レイアウトまたはマスター ページを使用して**と**厳密に型指定されたビューを作成する**チェック ボックスが選択されていません。</span><span class="sxs-lookup"><span data-stu-id="e68f3-121">Make sure that the **Use a layout or master page** and **Create a strongly-typed view** check boxes are not selected.</span></span>

<span data-ttu-id="e68f3-122">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e68f3-122">Click **Add**.</span></span> <span data-ttu-id="e68f3-123">*Views\Shared\EditorTemplates\Date.cshtml*テンプレートが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-123">The *Views\Shared\EditorTemplates\Date.cshtml* template is created.</span></span>

<span data-ttu-id="e68f3-124">次のコードを追加、 *Views\Shared\EditorTemplates\Date.cshtml*テンプレート。</span><span class="sxs-lookup"><span data-stu-id="e68f3-124">Add the following code to the *Views\Shared\EditorTemplates\Date.cshtml* template.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

<span data-ttu-id="e68f3-125">最初の行の宣言モデルを`DateTime`型。</span><span class="sxs-lookup"><span data-stu-id="e68f3-125">The first line declares the model to be a `DateTime` type.</span></span> <span data-ttu-id="e68f3-126">コンパイル時に取得するようにベスト プラクティスですが、編集のモデルの種類を宣言し、テンプレートを表示する必要はありませんが、モデル、ビューに渡されるを確認します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-126">Although you don't need to declare the model type in edit and display templates, it's a best practice so that you get compile-time checking of the model being passed to the view.</span></span> <span data-ttu-id="e68f3-127">(別のメリットはし、Visual Studio でのビューでモデルの IntelliSense を取得すること) です。ASP.NET MVC では、モデルの種類が宣言されていない場合、[動的](https://msdn.microsoft.com/library/dd264741.aspx)を入力し、コンパイル時ではない型チェックします。</span><span class="sxs-lookup"><span data-stu-id="e68f3-127">(Another benefit is that you then get IntelliSense for the model in the view in Visual Studio.) If the model type is not declared, ASP.NET MVC considers it a [dynamic](https://msdn.microsoft.com/library/dd264741.aspx) type and there's no compile-time type checking.</span></span> <span data-ttu-id="e68f3-128">モデルを宣言する場合、`DateTime`型が厳密に型指定します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-128">If you declare the model to be a `DateTime` type, it becomes strongly typed.</span></span>

<span data-ttu-id="e68f3-129">2 番目の行が表示される HTML マークアップをリテラルだけ&quot;日付テンプレートを使用した&quot;日付フィールドの前にします。</span><span class="sxs-lookup"><span data-stu-id="e68f3-129">The second line is just literal HTML markup that displays &quot;Using Date Template&quot; before a date field.</span></span> <span data-ttu-id="e68f3-130">この日付テンプレートが使用されていることを確認するのにこの行を一時的に使用します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-130">You'll use this line temporarily to verify that this date template is being used.</span></span>

<span data-ttu-id="e68f3-131">次の行が、 [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx)レンダリング ヘルパー、`input`テキスト ボックスにあるフィールドです。</span><span class="sxs-lookup"><span data-stu-id="e68f3-131">The next line is an [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) helper that renders an `input` field that's a text box.</span></span> <span data-ttu-id="e68f3-132">ヘルパーの 3 番目のパラメーターでは、匿名型を使用して、テキスト ボックスのクラスを設定する`datefield`タイプを`date`します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-132">The third parameter for the helper uses an anonymous type to set the class for the text box to `datefield` and the type to `date`.</span></span> <span data-ttu-id="e68f3-133">(ため`class`、予約されている c# を使用する必要があります、`@`文字をエスケープするため、 `class` c# パーサーで属性)。</span><span class="sxs-lookup"><span data-stu-id="e68f3-133">(Because `class` is a reserved in C#, you need to use the `@` character to escape the `class` attribute in the C# parser.)</span></span>

<span data-ttu-id="e68f3-134">`date`型により、HTML5 対応のブラウザーで HTML5 カレンダー コントロールをレンダリングする HTML5 入力型です。</span><span class="sxs-lookup"><span data-stu-id="e68f3-134">The `date` type is an HTML5 input type that enables HTML5-aware browsers to render a HTML5 calendar control.</span></span> <span data-ttu-id="e68f3-135">後で、jQuery の datepicker にフックする何らかの JavaScript を追加します、`Html.TextBox`要素を使用して、`datefield`クラス。</span><span class="sxs-lookup"><span data-stu-id="e68f3-135">Later on you'll add some JavaScript to hook up the jQuery datepicker to the `Html.TextBox` element using the `datefield` class.</span></span>

<span data-ttu-id="e68f3-136">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-136">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="e68f3-137">確認することができます、`ReleaseDate`テンプレートに表示されるため、プロパティを編集ビューには、テンプレートの編集を使用して&quot;日付テンプレートを使用した&quot;直前に、`ReleaseDate`この図のように、テキスト入力ボックス。</span><span class="sxs-lookup"><span data-stu-id="e68f3-137">You can verify that the `ReleaseDate` property in the edit view is using the edit template because the template displays &quot;Using Date Template&quot; just before the `ReleaseDate` text input box, as shown in this image:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

<span data-ttu-id="e68f3-138">ブラウザーでページのソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-138">In your browser, view the source of the page.</span></span> <span data-ttu-id="e68f3-139">(たとえば、ページを右クリックし **ソースの表示**)。次の例を示しています、ページのマークアップのいくつかを示す、`class`と`type`レンダリングされた html 属性。</span><span class="sxs-lookup"><span data-stu-id="e68f3-139">(For example, right-click the page and select **View source**.) The following example shows some of the markup for the page, illustrating the `class` and `type` attributes in the rendered HTML.</span></span>

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

<span data-ttu-id="e68f3-140">戻り、 *Views\Shared\EditorTemplates\Date.cshtml*テンプレートと削除、&quot;日付テンプレートを使用した&quot;マークアップ。</span><span class="sxs-lookup"><span data-stu-id="e68f3-140">Return to the *Views\Shared\EditorTemplates\Date.cshtml* template and remove the &quot;Using Date Template&quot; markup.</span></span> <span data-ttu-id="e68f3-141">これで完成したテンプレートは、ようになります。</span><span class="sxs-lookup"><span data-stu-id="e68f3-141">Now the completed template looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a><span data-ttu-id="e68f3-142">JQuery UI Datepicker ポップアップ カレンダーを追加する NuGet を使用して</span><span class="sxs-lookup"><span data-stu-id="e68f3-142">Adding a jQuery UI Datepicker Popup Calendar using NuGet</span></span>

<span data-ttu-id="e68f3-143">このセクションで追加します、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダー日付編集テンプレートにします。</span><span class="sxs-lookup"><span data-stu-id="e68f3-143">In this section you'll add the [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) popup calendar to the date-edit template.</span></span> <span data-ttu-id="e68f3-144">[の jQuery UI](http://jqueryui.com/)ライブラリは、アニメーション、高度な特殊効果、およびカスタマイズ可能なウィジェットのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-144">The [jQuery UI](http://jqueryui.com/) library provides support for animation, advanced effects, and customizable widgets.</span></span> <span data-ttu-id="e68f3-145">これは、jQuery JavaScript ライブラリの上に構築されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-145">It's built on top of the jQuery JavaScript library.</span></span> <span data-ttu-id="e68f3-146">Datepicker ポップアップ カレンダーで簡単かつ自然に文字列を入力する代わりに、予定表を使用して日付を入力します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-146">The datepicker popup calendar makes it easy and natural to enter dates using a calendar instead of entering a string.</span></span> <span data-ttu-id="e68f3-147">ポップアップ カレンダーでは、有効な日付にユーザーによっても制限: 通常のテキストのエントリの日付のように入力するように`2/33/1999`(年 2 月 33rd、1999) が、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーの制限によるものです。</span><span class="sxs-lookup"><span data-stu-id="e68f3-147">The popup calendar also limits users to legal dates — ordinary text entry for a date would let you enter something like `2/33/1999` ( February 33rd, 1999), but the [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) popup calendar won't allow that.</span></span>

<span data-ttu-id="e68f3-148">最初に、jQuery UI ライブラリをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e68f3-148">First, you have to install the jQuery UI libraries.</span></span> <span data-ttu-id="e68f3-149">そのためには、SP1 バージョンの Visual Studio 2010 と Visual Web Developer に含まれているパッケージ マネージャーは、NuGet を使用します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-149">To do that, you'll use NuGet, which is a package manager that's included in SP1 versions of Visual Studio 2010 and Visual Web Developer.</span></span>

<span data-ttu-id="e68f3-150">Visual Web developer でから、**ツール**メニューの **ライブラリ パッケージ マネージャー**選び**NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-150">In Visual Web Developer, from the **Tools** menu, select **Library Package Manager** and then select **Manage NuGet Packages**.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

<span data-ttu-id="e68f3-151">注: 場合、**ツール**メニューが表示されない、**ライブラリ パッケージ マネージャー**コマンド次の手順で NuGet をインストールする必要がある、[をインストールする NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)のページNuGet の web サイト。</span><span class="sxs-lookup"><span data-stu-id="e68f3-151">Note: If the **Tools** menu doesn't display the **Library Package Manager** command, you need to install NuGet by following the instructions on the [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) page of the NuGet website.</span></span>   
  
<span data-ttu-id="e68f3-152">Visual Web Developer ではなく Visual Studio を使用しているかどうか、**ツール**メニューの **ライブラリ パッケージ マネージャー**選び**ライブラリ パッケージ参照の追加**します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-152">If you're using Visual Studio instead of Visual Web Developer, from the **Tools** menu, select **Library Package Manager** and then select **Add Library Package Reference**.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

<span data-ttu-id="e68f3-153">**MVCMovie - NuGet パッケージの管理**ダイアログ ボックスで、をクリックして、**オンライン**左側のタブ、enter &quot;jQuery.UI&quot;検索ボックスにします。</span><span class="sxs-lookup"><span data-stu-id="e68f3-153">In the **MVCMovie - Manage NuGet Packages** dialog box, click the **Online** tab on the left and then enter &quot;jQuery.UI&quot; in the search box.</span></span> <span data-ttu-id="e68f3-154">J を選択**クエリ UI ウィジェット: Datepicker**を選択してから、**インストール**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e68f3-154">Select j **Query UI WIdgets:Datepicker**, then select the **Install** button.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

<span data-ttu-id="e68f3-155">NuGet では、これらのデバッグ バージョンと縮小されたバージョンの jQuery UI のコアと jQuery UI 日付選択カレンダーをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-155">NuGet adds these debug versions and minified versions of jQuery UI Core and the jQuery UI date picker to your project:</span></span>

- <span data-ttu-id="e68f3-156">*jquery.ui.core.js*</span><span class="sxs-lookup"><span data-stu-id="e68f3-156">*jquery.ui.core.js*</span></span>
- <span data-ttu-id="e68f3-157">*jquery.ui.core.min.js*</span><span class="sxs-lookup"><span data-stu-id="e68f3-157">*jquery.ui.core.min.js*</span></span>
- <span data-ttu-id="e68f3-158">*jquery.ui.datepicker.js*</span><span class="sxs-lookup"><span data-stu-id="e68f3-158">*jquery.ui.datepicker.js*</span></span>
- <span data-ttu-id="e68f3-159">*jquery.ui.datepicker.min.js*</span><span class="sxs-lookup"><span data-stu-id="e68f3-159">*jquery.ui.datepicker.min.js*</span></span>

<span data-ttu-id="e68f3-160">注: デバッグ バージョン (せずにファイルを、 *. min.js*拡張機能) は便利です、デバッグが、実稼働サイトで、縮小されたバージョンのみを含めます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-160">Note: The debug versions (the files without the *.min.js* extension) are useful for debugging, but in a production site, you'd include only the minified versions.</span></span>

<span data-ttu-id="e68f3-161">JQuery の日付の選択を実際に使用するには、テンプレートの編集にカレンダー ウィジェットをフックする jQuery スクリプトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e68f3-161">To actually use the jQuery date picker, you need to create a jQuery script that will hook up the calendar widget to the edit template.</span></span> <span data-ttu-id="e68f3-162">**ソリューション エクスプ ローラー**を右クリックし、*スクリプト*フォルダーと選択**追加**、し**新しい項目の**、し**JScriptファイル**します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-162">In **Solution Explorer**, right-click the *Scripts* folder and select **Add**, then **New Item**, and then **JScript File**.</span></span> <span data-ttu-id="e68f3-163">ファイルに名前を*DatePickerReady.js*します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-163">Name the file *DatePickerReady.js*.</span></span>

<span data-ttu-id="e68f3-164">次のコードを追加、 *DatePickerReady.js*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e68f3-164">Add the following code to the *DatePickerReady.js* file:</span></span>

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

<span data-ttu-id="e68f3-165">これが何の簡単な説明がここでは jQuery を使い慣れていない場合は、: 最初の行は、 &quot;jQuery の準備ができて&quot;関数で、ページ内のすべての DOM 要素を読み込んだときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-165">If you're not familiar with jQuery, here's a brief explanation of what this does: the first line is the &quot;jQuery ready&quot; function, which is called when all the DOM elements in a page have loaded.</span></span> <span data-ttu-id="e68f3-166">2 行目は、クラス名を持つすべての DOM 要素を選択します。 `datefield`、を起動し、`datepicker`それぞれの関数。</span><span class="sxs-lookup"><span data-stu-id="e68f3-166">The second line selects all DOM elements that have the class name `datefield`, then invokes the `datepicker` function for each of them.</span></span> <span data-ttu-id="e68f3-167">(追加したことに注意してください、`datefield`クラスを*Views\Shared\EditorTemplates\Date.cshtml*チュートリアルの前半のテンプレートです)。</span><span class="sxs-lookup"><span data-stu-id="e68f3-167">(Remember that you added the `datefield` class to the *Views\Shared\EditorTemplates\Date.cshtml* template earlier in the tutorial.)</span></span>

<span data-ttu-id="e68f3-168">次に、開く、 *views \shared\\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e68f3-168">Next, open the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="e68f3-169">日付の選択を使用できるように、必要なすべてが、次のファイルへの参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e68f3-169">You need to add references to the following files, which are all required so that you can use the date picker:</span></span>

- <span data-ttu-id="e68f3-170">*Content/themes/base/jquery.ui.core.css*</span><span class="sxs-lookup"><span data-stu-id="e68f3-170">*Content/themes/base/jquery.ui.core.css*</span></span>
- <span data-ttu-id="e68f3-171">*Content/themes/base/jquery.ui.datepicker.css*</span><span class="sxs-lookup"><span data-stu-id="e68f3-171">*Content/themes/base/jquery.ui.datepicker.css*</span></span>
- <span data-ttu-id="e68f3-172">*Content/themes/base/jquery.ui.theme.css*</span><span class="sxs-lookup"><span data-stu-id="e68f3-172">*Content/themes/base/jquery.ui.theme.css*</span></span>
- <span data-ttu-id="e68f3-173">*jquery.ui.core.min.js*</span><span class="sxs-lookup"><span data-stu-id="e68f3-173">*jquery.ui.core.min.js*</span></span>
- <span data-ttu-id="e68f3-174">*jquery.ui.datepicker.min.js*</span><span class="sxs-lookup"><span data-stu-id="e68f3-174">*jquery.ui.datepicker.min.js*</span></span>
- <span data-ttu-id="e68f3-175">*DatePickerReady.js*</span><span class="sxs-lookup"><span data-stu-id="e68f3-175">*DatePickerReady.js*</span></span>

<span data-ttu-id="e68f3-176">次の例では、表示、実際のコードの下部に追加する必要があります、`head`内の要素、 *views \shared\\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e68f3-176">The following example shows the actual code that you should add at the bottom of the `head` element in the *Views\Shared\\_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

<span data-ttu-id="e68f3-177">完全な`head`セクションを次に示します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-177">The complete `head` section is shown here:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

<span data-ttu-id="e68f3-178">[URL コンテンツ ヘルパー](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx)メソッド リソース パスは絶対パスに変換します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-178">The [URL content helper](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) method converts the resource path to an absolute path.</span></span> <span data-ttu-id="e68f3-179">使用する必要があります`@URL.Content`アプリケーションが IIS で実行されているときに、これらのリソースを正しく参照します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-179">You must use `@URL.Content` to correctly reference these resources when the application is running on IIS.</span></span>

<span data-ttu-id="e68f3-180">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-180">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="e68f3-181">編集リンクを選択し、挿入ポイントを配置、 **ReleaseDate**フィールド。</span><span class="sxs-lookup"><span data-stu-id="e68f3-181">Select an edit link, then put the insertion point into the **ReleaseDate** field.</span></span> <span data-ttu-id="e68f3-182">JQuery UI ポップアップ カレンダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-182">The jQuery UI popup calendar is displayed.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

<span data-ttu-id="e68f3-183">ほとんどの jQuery コントロールと同様に、datepicker では、広範囲にカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-183">Like most jQuery controls, the datepicker lets you customize it extensively.</span></span> <span data-ttu-id="e68f3-184">については、次を参照してください。 [Visual カスタマイズ: jQuery UI テーマを設計](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme)上、[の jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/)サイト。</span><span class="sxs-lookup"><span data-stu-id="e68f3-184">For information, see [Visual Customization: Designing a jQuery UI theme](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) on the [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) site.</span></span>

### <a name="supporting-the-html5-date-input-control"></a><span data-ttu-id="e68f3-185">HTML5 の日付入力コントロールをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="e68f3-185">Supporting the HTML5 Date Input Control</span></span>

<span data-ttu-id="e68f3-186">多くのブラウザーは HTML5 をサポートは、HTML5 のネイティブの入力などを使用してたい、`date`入力要素、および jQuery UI のカレンダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-186">As more browsers support HTML5, you'll want to use the native HTML5 input, such as the `date` input element, and not use the jQuery UI calendar.</span></span> <span data-ttu-id="e68f3-187">ブラウザーには、それらがサポートしている場合、HTML5 コントロールを自動的に使用するアプリケーションにロジックを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-187">You can add logic to your application to automatically use HTML5 controls if the browser supports them.</span></span> <span data-ttu-id="e68f3-188">これを行うには、置換の内容、 *DatePickerReady.js*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="e68f3-188">To do this, replace the contents of the *DatePickerReady.js* file with the following:</span></span>

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

<span data-ttu-id="e68f3-189">このスクリプトの最初の行では、Modernizr を使用して、HTML5 の日付の入力がサポートされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-189">The first line of this script uses Modernizr to verify that HTML5 date input is supported.</span></span> <span data-ttu-id="e68f3-190">サポートされていない場合の jQuery UI 日付選択カレンダーをフックする代わりにします。</span><span class="sxs-lookup"><span data-stu-id="e68f3-190">If it's not supported, the jQuery UI date picker is hooked up instead.</span></span> <span data-ttu-id="e68f3-191">([Modernizr](http://www.modernizr.com/docs/)は HTML5 および css3 用のネイティブ実装の可用性を検出するオープン ソース JavaScript ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="e68f3-191">([Modernizr](http://www.modernizr.com/docs/) is an open-source JavaScript library that detects the availability of native implementations of HTML5 and CSS3.</span></span> <span data-ttu-id="e68f3-192">Modernizr に含まれる新しい ASP.NET MVC プロジェクトを作成する。)</span><span class="sxs-lookup"><span data-stu-id="e68f3-192">Modernizr is included in any new ASP.NET MVC projects that you create.)</span></span>

<span data-ttu-id="e68f3-193">この変更を行った後に、Opera 11 などの HTML5 をサポートするブラウザーを使用してテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-193">After you've made this change, you can test it by using a browser that supports HTML5, such as Opera 11.</span></span> <span data-ttu-id="e68f3-194">HTML5 と互換性のあるブラウザーを使用してアプリケーションを実行し、ムービー エントリを編集します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-194">Run the application using an HTML5-compatible browser and edit a movie entry.</span></span> <span data-ttu-id="e68f3-195">HTML5 の日付コントロールは、jQuery UI ポップアップ カレンダーの代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-195">The HTML5 date control is used instead of the jQuery UI popup calendar:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

<span data-ttu-id="e68f3-196">新しいバージョンのブラウザーは HTML5 を段階的実装は、ために、ここでは適切なアプローチでは、さまざまな HTML5 のサポートに対応する web サイトにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-196">Because new versions of browsers are implementing HTML5 incrementally, a good approach for now is to add code to your website that accommodates a wide variety of HTML5 support.</span></span> <span data-ttu-id="e68f3-197">たとえばより堅牢さ*DatePickerReady.js*スクリプトが部分的にのみ、HTML5 の日付コントロールをサポートするサイトのサポート ブラウザーできる次に示します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-197">For example, a more robust *DatePickerReady.js* script is shown below that lets your site support browsers that only partially support the HTML5 date control.</span></span>

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

<span data-ttu-id="e68f3-198">このスクリプトは、HTML5 を選択します。`input`型の要素`date`HTML5 の日付コントロールを完全にサポートしません。</span><span class="sxs-lookup"><span data-stu-id="e68f3-198">This script selects HTML5 `input` elements of type `date` that don't fully support the HTML5 date control.</span></span> <span data-ttu-id="e68f3-199">これらの要素の jQuery UI ポップアップ カレンダーをフックし、変更が、`type`属性を`date`に`text`します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-199">For those elements, it hooks up the jQuery UI popup calendar and then changes the `type` attribute from `date` to `text`.</span></span> <span data-ttu-id="e68f3-200">変更することで、`type`から属性`date`に`text`、部分的な HTML5 日付のサポートが削除されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-200">By changing the `type` attribute from `date` to `text`, partial HTML5 date support is eliminated.</span></span> <span data-ttu-id="e68f3-201">堅牢*DatePickerReady.js*にスクリプトがある[JSFIDDLE](http://jsfiddle.net/XSTK8/15/)します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-201">An even more robust *DatePickerReady.js* script can be found at [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).</span></span>

### <a name="adding-nullable-dates-to-the-templates"></a><span data-ttu-id="e68f3-202">Null 許容型日付をテンプレートに追加します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-202">Adding Nullable Dates to the Templates</span></span>

<span data-ttu-id="e68f3-203">日付の既存のテンプレートのいずれかを使用して、null の日付を渡すと、実行時エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-203">If you use one of the existing date templates and pass a null date, you'll get a run-time error.</span></span> <span data-ttu-id="e68f3-204">日付テンプレートをより堅牢にするために、null 値を処理する際に変更します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-204">To make the date templates more robust, you'll change them to handle null values.</span></span> <span data-ttu-id="e68f3-205">Null 許容型日付をサポートするために、コードを変更、 *Views\Shared\DisplayTemplates\DateTime.cshtml*次。</span><span class="sxs-lookup"><span data-stu-id="e68f3-205">To support nullable dates, change the code in the *Views\Shared\DisplayTemplates\DateTime.cshtml* to the following:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

<span data-ttu-id="e68f3-206">コードが、モデルの場合、空の文字列を返します**null**します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-206">The code returns an empty string when the model is **null**.</span></span>

<span data-ttu-id="e68f3-207">コードを変更、 *Views\Shared\EditorTemplates\Date.cshtml*次のファイル。</span><span class="sxs-lookup"><span data-stu-id="e68f3-207">Change the code in the *Views\Shared\EditorTemplates\Date.cshtml* file to the following:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

<span data-ttu-id="e68f3-208">このコードを実行すると、モデルが null でない場合、モデルの`DateTime`値が使用されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-208">When this code runs, if the model is not null, the model's `DateTime` value is used.</span></span> <span data-ttu-id="e68f3-209">モデルが null の場合は、現在の日付が代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-209">If the model is null, the current date is used instead.</span></span>

### <a name="wrapup"></a><span data-ttu-id="e68f3-210">(コース完了)</span><span class="sxs-lookup"><span data-stu-id="e68f3-210">Wrapup</span></span>

<span data-ttu-id="e68f3-211">このチュートリアルでは、ASP.NET テンプレート化されたヘルパーの基礎を取り上げ、ASP.NET MVC アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-211">This tutorial has covered the basics of ASP.NET templated helpers and shows you how to use the jQuery UI datepicker popup calendar in an ASP.NET MVC application.</span></span> <span data-ttu-id="e68f3-212">詳細については、これらのリソースにしてください。</span><span class="sxs-lookup"><span data-stu-id="e68f3-212">For more information, try these resources:</span></span>

- <span data-ttu-id="e68f3-213">ローカライズについては、Rajeesh のブログを参照してください。 [ASP.NET MVC で JQueryUI Datepicker](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/)します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-213">For information on localization, see Rajeesh's blog [JQueryUI Datepicker in ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).</span></span>
- <span data-ttu-id="e68f3-214">JQuery UI の詳細については、次を参照してください。[の jQuery UI](http://docs.jquery.com/UI)します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-214">For information about jQuery UI, see [jQuery UI](http://docs.jquery.com/UI).</span></span>
- <span data-ttu-id="e68f3-215">Datepicker コントロールをローカライズする方法については、次を参照してください。 [UI/Datepicker/ローカライズ](http://docs.jquery.com/UI/Datepicker/Localization)します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-215">For information about how to localize the datepicker control, see [UI/Datepicker/Localization](http://docs.jquery.com/UI/Datepicker/Localization).</span></span>
- <span data-ttu-id="e68f3-216">ASP.NET MVC テンプレートの詳細については、Brad Wilson のブログ シリーズを参照してください。 [ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)します。</span><span class="sxs-lookup"><span data-stu-id="e68f3-216">For more information about the ASP.NET MVC templates, see Brad Wilson's blog series on [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html).</span></span> <span data-ttu-id="e68f3-217">ASP.NET MVC 2 の系列が記述されていますが、ASP.NET MVC の現在のバージョンについても、マテリアルは適用されます。</span><span class="sxs-lookup"><span data-stu-id="e68f3-217">Although the series was written for ASP.NET MVC 2, the material still applies for the current version of ASP.NET MVC.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e68f3-218">前へ</span><span class="sxs-lookup"><span data-stu-id="e68f3-218">Previous</span></span>](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
