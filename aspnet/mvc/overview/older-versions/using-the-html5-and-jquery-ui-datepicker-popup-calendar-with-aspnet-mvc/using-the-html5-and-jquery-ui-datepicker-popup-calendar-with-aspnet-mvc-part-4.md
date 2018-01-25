---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: "ASP.NET MVC - パート 4 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MV で jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 32211465adeb1353908daa1014d188b84389e1a7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a><span data-ttu-id="84d00-103">ASP.NET MVC - パート 4 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用</span><span class="sxs-lookup"><span data-stu-id="84d00-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 4</span></span>
====================
<span data-ttu-id="84d00-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="84d00-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="84d00-105">このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。</span><span class="sxs-lookup"><span data-stu-id="84d00-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


### <a name="adding-a-template-for-editing-dates"></a><span data-ttu-id="84d00-106">日付を編集するためのテンプレートの追加</span><span class="sxs-lookup"><span data-stu-id="84d00-106">Adding a Template for Editing Dates</span></span>

<span data-ttu-id="84d00-107">このセクションでは、ASP.NET MVC でマークされているモデルのプロパティを編集するための UI の表示時に適用される日付を編集用のテンプレートを作成します、**日付**の列挙体、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="84d00-107">In this section you'll create a template for editing dates that will be applied when ASP.NET MVC displays UI for editing model properties that are marked with the **Date** enumeration of the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attibute.</span></span> <span data-ttu-id="84d00-108">テンプレートは日付のみを表示します。時刻は表示されません。</span><span class="sxs-lookup"><span data-stu-id="84d00-108">The template will render only the date; time will not be displayed.</span></span> <span data-ttu-id="84d00-109">テンプレートでは、使用、 [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーの日付を編集する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="84d00-109">In the template you'll use the [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) popup calendar to provide a way to edit dates.</span></span>

<span data-ttu-id="84d00-110">開始する、開く、 *Movie.cs*ファイルを追加、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性が、**日付**列挙型を`ReleaseDate`プロパティ、次のコードに示すように。</span><span class="sxs-lookup"><span data-stu-id="84d00-110">To begin, open the *Movie.cs* file and add the [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribute with the **Date** enumeration to the `ReleaseDate` property, as shown in the following code:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

<span data-ttu-id="84d00-111">このコードにより、`ReleaseDate`両方で時間がテンプレートを表示し、テンプレートを編集せずに表示されるフィールドです。</span><span class="sxs-lookup"><span data-stu-id="84d00-111">This code causes the `ReleaseDate` field to be displayed without the time in both display templates and edit templates.</span></span> <span data-ttu-id="84d00-112">アプリケーションが含まれている場合、 *date.cshtml*でテンプレート、 *views \shared\editortemplates*フォルダーまたは、 *Views\Movies\EditorTemplates*フォルダー、そのテンプレートいずれかを表示するために使用されます`DateTime`プロパティの編集中にします。</span><span class="sxs-lookup"><span data-stu-id="84d00-112">If your application contains a *date.cshtml* template in the *Views\Shared\EditorTemplates* folder or in the *Views\Movies\EditorTemplates* folder, that template will be used to render any `DateTime` property while editing.</span></span> <span data-ttu-id="84d00-113">それ以外の場合、組み込みの ASP.NET テンプレート システム日付として、プロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="84d00-113">Otherwise the built-in ASP.NET templating system will display the property as a date.</span></span>

<span data-ttu-id="84d00-114">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="84d00-114">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="84d00-115">リリース日の入力フィールドで、日付のみが表示されていることを確認する編集リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="84d00-115">Select an edit link to verify that the input field for the release date is showing only the date.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

<span data-ttu-id="84d00-116">**ソリューション エクスプ ローラー**、展開、*ビュー*フォルダーで、展開、 *Shared*フォルダー、およびしを右クリック、 *views \shared\editortemplates*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="84d00-116">In **Solution Explorer**, expand the *Views* folder, expand the *Shared* folder, and then right-click the *Views\Shared\EditorTemplates* folder.</span></span>

<span data-ttu-id="84d00-117">をクリックして**追加**、クリックして**ビュー**です。</span><span class="sxs-lookup"><span data-stu-id="84d00-117">Click **Add**, and then click **View**.</span></span> <span data-ttu-id="84d00-118">**ビューの追加** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="84d00-118">The **Add View** dialog box is displayed.</span></span>

<span data-ttu-id="84d00-119">**ビュー名**ボックスに、入力&quot;日付&quot;です。</span><span class="sxs-lookup"><span data-stu-id="84d00-119">In the **View name** box, type &quot;Date&quot;.</span></span>

<span data-ttu-id="84d00-120">選択、**部分ビューとして作成**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="84d00-120">Select the **Create as a partial view** check box.</span></span> <span data-ttu-id="84d00-121">確認して、**レイアウトまたはマスター ページを使用して**と**厳密に型指定されたビューを作成する**チェック ボックスが選択されていません。</span><span class="sxs-lookup"><span data-stu-id="84d00-121">Make sure that the **Use a layout or master page** and **Create a strongly-typed view** check boxes are not selected.</span></span>

<span data-ttu-id="84d00-122">**[追加]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="84d00-122">Click **Add**.</span></span> <span data-ttu-id="84d00-123">*Views\Shared\EditorTemplates\Date.cshtml*テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="84d00-123">The *Views\Shared\EditorTemplates\Date.cshtml* template is created.</span></span>

<span data-ttu-id="84d00-124">次のコードを追加、 *Views\Shared\EditorTemplates\Date.cshtml*テンプレート。</span><span class="sxs-lookup"><span data-stu-id="84d00-124">Add the following code to the *Views\Shared\EditorTemplates\Date.cshtml* template.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

<span data-ttu-id="84d00-125">最初の行を宣言するモデル、`DateTime`型です。</span><span class="sxs-lookup"><span data-stu-id="84d00-125">The first line declares the model to be a `DateTime` type.</span></span> <span data-ttu-id="84d00-126">編集のモデルの種類を宣言し、テンプレートを表示する必要はありませんは、ベスト プラクティス コンパイル時間を取得すること、モデル、ビューに渡されるを確認します。</span><span class="sxs-lookup"><span data-stu-id="84d00-126">Although you don't need to declare the model type in edit and display templates, it's a best practice so that you get compile-time checking of the model being passed to the view.</span></span> <span data-ttu-id="84d00-127">(別のメリットはし、Visual Studio でのビューでモデルの IntelliSense を利用することです。)モデルの種類が宣言されていない場合は、ASP.NET MVC であると判断、[動的](https://msdn.microsoft.com/library/dd264741.aspx)を入力し、コンパイルが存在しない型チェックします。</span><span class="sxs-lookup"><span data-stu-id="84d00-127">(Another benefit is that you then get IntelliSense for the model in the view in Visual Studio.) If the model type is not declared, ASP.NET MVC considers it a [dynamic](https://msdn.microsoft.com/library/dd264741.aspx) type and there's no compile-time type checking.</span></span> <span data-ttu-id="84d00-128">あるモデルを宣言する場合、`DateTime`型が厳密に型指定します。</span><span class="sxs-lookup"><span data-stu-id="84d00-128">If you declare the model to be a `DateTime` type, it becomes strongly typed.</span></span>

<span data-ttu-id="84d00-129">2 番目の行が表示される HTML マークアップをリテラルだけ&quot;日付テンプレートを使用した&quot;日付フィールドの前にします。</span><span class="sxs-lookup"><span data-stu-id="84d00-129">The second line is just literal HTML markup that displays &quot;Using Date Template&quot; before a date field.</span></span> <span data-ttu-id="84d00-130">この行は、この日付テンプレートを使用していることを確認するのに一時的に使用します。</span><span class="sxs-lookup"><span data-stu-id="84d00-130">You'll use this line temporarily to verify that this date template is being used.</span></span>

<span data-ttu-id="84d00-131">次の行は、 [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx)レンダリング ヘルパーに`input`テキスト ボックスにあるフィールドです。</span><span class="sxs-lookup"><span data-stu-id="84d00-131">The next line is an [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) helper that renders an `input` field that's a text box.</span></span> <span data-ttu-id="84d00-132">ヘルパーの 3 番目のパラメーターは、匿名型を使用して、テキスト ボックスのクラスを設定を`datefield`、種類を`date`です。</span><span class="sxs-lookup"><span data-stu-id="84d00-132">The third parameter for the helper uses an anonymous type to set the class for the text box to `datefield` and the type to `date`.</span></span> <span data-ttu-id="84d00-133">(ため`class`、予約されている、C# の場合は、使用する必要があります、`@`エスケープ文字、 `class` c# パーサーでの属性です)。</span><span class="sxs-lookup"><span data-stu-id="84d00-133">(Because `class` is a reserved in C#, you need to use the `@` character to escape the `class` attribute in the C# parser.)</span></span>

<span data-ttu-id="84d00-134">`date`型 HTML5 に対応したブラウザーは HTML5 カレンダー コントロールをレンダリングする HTML5 入力型です。</span><span class="sxs-lookup"><span data-stu-id="84d00-134">The `date` type is an HTML5 input type that enables HTML5-aware browsers to render a HTML5 calendar control.</span></span> <span data-ttu-id="84d00-135">後でに jQuery datepicker をフックするために一部の JavaScript を追加、`Html.TextBox`要素を使用して、`datefield`クラスです。</span><span class="sxs-lookup"><span data-stu-id="84d00-135">Later on you'll add some JavaScript to hook up the jQuery datepicker to the `Html.TextBox` element using the `datefield` class.</span></span>

<span data-ttu-id="84d00-136">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="84d00-136">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="84d00-137">確認することができます、`ReleaseDate`テンプレートが表示されるため、プロパティを編集ビューでは、テンプレートの編集を使用して&quot;日付テンプレートを使用する&quot;直前に、`ReleaseDate`テキストの入力ボックスで、この図のようにします。</span><span class="sxs-lookup"><span data-stu-id="84d00-137">You can verify that the `ReleaseDate` property in the edit view is using the edit template because the template displays &quot;Using Date Template&quot; just before the `ReleaseDate` text input box, as shown in this image:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

<span data-ttu-id="84d00-138">ブラウザーでは、ページのソースを表示します。</span><span class="sxs-lookup"><span data-stu-id="84d00-138">In your browser, view the source of the page.</span></span> <span data-ttu-id="84d00-139">(たとえば、ページを右クリックし **ソースの表示**)。次、ページのマークアップのいくつかの例を示す、`class`と`type`でレンダリングされた HTML 属性。</span><span class="sxs-lookup"><span data-stu-id="84d00-139">(For example, right-click the page and select **View source**.) The following example shows some of the markup for the page, illustrating the `class` and `type` attributes in the rendered HTML.</span></span>

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

<span data-ttu-id="84d00-140">戻り、 *Views\Shared\EditorTemplates\Date.cshtml*テンプレートと削除、&quot;日付テンプレートを使用した&quot;マークアップ。</span><span class="sxs-lookup"><span data-stu-id="84d00-140">Return to the *Views\Shared\EditorTemplates\Date.cshtml* template and remove the &quot;Using Date Template&quot; markup.</span></span> <span data-ttu-id="84d00-141">これで完了したテンプレートは次のよう。</span><span class="sxs-lookup"><span data-stu-id="84d00-141">Now the completed template looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a><span data-ttu-id="84d00-142">JQuery UI Datepicker ポップアップ カレンダーを追加する NuGet を使用します。</span><span class="sxs-lookup"><span data-stu-id="84d00-142">Adding a jQuery UI Datepicker Popup Calendar using NuGet</span></span>

<span data-ttu-id="84d00-143">このセクションでは追加、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダー日付編集テンプレートにします。</span><span class="sxs-lookup"><span data-stu-id="84d00-143">In this section you'll add the [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) popup calendar to the date-edit template.</span></span> <span data-ttu-id="84d00-144">[JQuery UI](http://jqueryui.com/)ライブラリは、アニメーション、高度な特殊効果、およびカスタマイズ可能なウィジェットのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="84d00-144">The [jQuery UI](http://jqueryui.com/) library provides support for animation, advanced effects, and customizable widgets.</span></span> <span data-ttu-id="84d00-145">これは、jQuery JavaScript ライブラリの上に構築されます。</span><span class="sxs-lookup"><span data-stu-id="84d00-145">It's built on top of the jQuery JavaScript library.</span></span> <span data-ttu-id="84d00-146">Datepicker ポップアップ カレンダー、簡単かつ自然文字列を入力する代わりに、予定表を使用して日付を入力できます。</span><span class="sxs-lookup"><span data-stu-id="84d00-146">The datepicker popup calendar makes it easy and natural to enter dates using a calendar instead of entering a string.</span></span> <span data-ttu-id="84d00-147">ポップアップ カレンダーも有効な日付にユーザーを制限: 日付の通常のテキストのエントリには、ように入力することが`2/33/1999`(February 33rd、1999)、ですが、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーことを許可しません。</span><span class="sxs-lookup"><span data-stu-id="84d00-147">The popup calendar also limits users to legal dates — ordinary text entry for a date would let you enter something like `2/33/1999` ( February 33rd, 1999), but the [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) popup calendar won't allow that.</span></span>

<span data-ttu-id="84d00-148">最初に、jQuery UI ライブラリをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="84d00-148">First, you have to install the jQuery UI libraries.</span></span> <span data-ttu-id="84d00-149">実行するには、SP1 バージョンの Visual Studio 2010 と Visual Web Developer に含まれるパッケージ マネージャーは、NuGet を使用します。</span><span class="sxs-lookup"><span data-stu-id="84d00-149">To do that, you'll use NuGet, which is a package manager that's included in SP1 versions of Visual Studio 2010 and Visual Web Developer.</span></span>

<span data-ttu-id="84d00-150">Visual Web Developer から、**ツール**メニューの **ライブラリ パッケージ マネージャー**し、 **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="84d00-150">In Visual Web Developer, from the **Tools** menu, select **Library Package Manager** and then select **Manage NuGet Packages**.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

<span data-ttu-id="84d00-151">メモ: 場合、**ツール**メニューが表示されない、**ライブラリ パッケージ マネージャー**コマンドを使用して手順に従って、NuGet をインストールする必要があります、 [NuGet のインストール](http://docs.nuget.org/docs/start-here/installing-nuget)のページNuGet の web サイトです。</span><span class="sxs-lookup"><span data-stu-id="84d00-151">Note: If the **Tools** menu doesn't display the **Library Package Manager** command, you need to install NuGet by following the instructions on the [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) page of the NuGet website.</span></span>   
  
<span data-ttu-id="84d00-152">Visual Web Developer ではなく Visual Studio を使用するかどうか、**ツール**メニューの **ライブラリ パッケージ マネージャー**し、**ライブラリ パッケージ参照の追加**です。</span><span class="sxs-lookup"><span data-stu-id="84d00-152">If you're using Visual Studio instead of Visual Web Developer, from the **Tools** menu, select **Library Package Manager** and then select **Add Library Package Reference**.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

<span data-ttu-id="84d00-153">**MVCMovie - NuGet パッケージの管理**ダイアログ ボックスで、をクリックして、**オンライン** タブの左側にし、入力&quot;jQuery.UI&quot;検索ボックスにします。</span><span class="sxs-lookup"><span data-stu-id="84d00-153">In the **MVCMovie - Manage NuGet Packages** dialog box, click the **Online** tab on the left and then enter &quot;jQuery.UI&quot; in the search box.</span></span> <span data-ttu-id="84d00-154">J を選択**クエリ UI ウィジェット: Datepicker**クリックし、**インストール**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="84d00-154">Select j **Query UI WIdgets:Datepicker**, then select the **Install** button.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

<span data-ttu-id="84d00-155">NuGet では、これらのデバッグ バージョンと jQuery UI コアと jQuery UI datepicker の縮小されたバージョンがプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="84d00-155">NuGet adds these debug versions and minified versions of jQuery UI Core and the jQuery UI date picker to your project:</span></span>

- <span data-ttu-id="84d00-156">*jquery.ui.core.js*</span><span class="sxs-lookup"><span data-stu-id="84d00-156">*jquery.ui.core.js*</span></span>
- <span data-ttu-id="84d00-157">*jquery.ui.core.min.js*</span><span class="sxs-lookup"><span data-stu-id="84d00-157">*jquery.ui.core.min.js*</span></span>
- <span data-ttu-id="84d00-158">*jquery.ui.datepicker.js*</span><span class="sxs-lookup"><span data-stu-id="84d00-158">*jquery.ui.datepicker.js*</span></span>
- <span data-ttu-id="84d00-159">*jquery.ui.datepicker.min.js*</span><span class="sxs-lookup"><span data-stu-id="84d00-159">*jquery.ui.datepicker.min.js*</span></span>

<span data-ttu-id="84d00-160">注: デバッグ バージョン (せずにファイルを、 *. min.js*拡張機能) は便利ですをデバッグする場合は、実稼働サイトで、縮小されたバージョンのみを含めます。</span><span class="sxs-lookup"><span data-stu-id="84d00-160">Note: The debug versions (the files without the *.min.js* extension) are useful for debugging, but in a production site, you'd include only the minified versions.</span></span>

<span data-ttu-id="84d00-161">JQuery 日付選択カレンダーを実際に使用するのには、テンプレートの編集にカレンダー ウィジェットをフックするため、jQuery スクリプトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84d00-161">To actually use the jQuery date picker, you need to create a jQuery script that will hook up the calendar widget to the edit template.</span></span> <span data-ttu-id="84d00-162">**ソリューション エクスプ ローラー**を右クリックし、*スクリプト*フォルダーを選択**追加**、し**新しい項目の**、し**JScriptファイル**です。</span><span class="sxs-lookup"><span data-stu-id="84d00-162">In **Solution Explorer**, right-click the *Scripts* folder and select **Add**, then **New Item**, and then **JScript File**.</span></span> <span data-ttu-id="84d00-163">ファイルの名前を付けます*DatePickerReady.js*です。</span><span class="sxs-lookup"><span data-stu-id="84d00-163">Name the file *DatePickerReady.js*.</span></span>

<span data-ttu-id="84d00-164">次のコードを追加、 *DatePickerReady.js*ファイル。</span><span class="sxs-lookup"><span data-stu-id="84d00-164">Add the following code to the *DatePickerReady.js* file:</span></span>

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

<span data-ttu-id="84d00-165">これが何を簡単に説明がここでは jQuery 慣れていない場合は、: 最初の行は、 &quot;jQuery の準備ができて&quot;ページ内のすべての DOM 要素が読み込まれるときに呼び出される関数。</span><span class="sxs-lookup"><span data-stu-id="84d00-165">If you're not familiar with jQuery, here's a brief explanation of what this does: the first line is the &quot;jQuery ready&quot; function, which is called when all the DOM elements in a page have loaded.</span></span> <span data-ttu-id="84d00-166">2 番目の行をクラス名を持つすべての DOM 要素の選択`datefield`が呼び出され、`datepicker`それぞれの関数。</span><span class="sxs-lookup"><span data-stu-id="84d00-166">The second line selects all DOM elements that have the class name `datefield`, then invokes the `datepicker` function for each of them.</span></span> <span data-ttu-id="84d00-167">(追加したことに注意してください、`datefield`クラスを*Views\Shared\EditorTemplates\Date.cshtml*チュートリアルの前半のテンプレートです)。</span><span class="sxs-lookup"><span data-stu-id="84d00-167">(Remember that you added the `datefield` class to the *Views\Shared\EditorTemplates\Date.cshtml* template earlier in the tutorial.)</span></span>

<span data-ttu-id="84d00-168">次に、開く、 *\shared\\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="84d00-168">Next, open the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="84d00-169">日付選択カレンダーを使用できるように、必要なすべてが次のファイルへの参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="84d00-169">You need to add references to the following files, which are all required so that you can use the date picker:</span></span>

- <span data-ttu-id="84d00-170">*Content/themes/base/jquery.ui.core.css*</span><span class="sxs-lookup"><span data-stu-id="84d00-170">*Content/themes/base/jquery.ui.core.css*</span></span>
- <span data-ttu-id="84d00-171">*Content/themes/base/jquery.ui.datepicker.css*</span><span class="sxs-lookup"><span data-stu-id="84d00-171">*Content/themes/base/jquery.ui.datepicker.css*</span></span>
- <span data-ttu-id="84d00-172">*Content/themes/base/jquery.ui.theme.css*</span><span class="sxs-lookup"><span data-stu-id="84d00-172">*Content/themes/base/jquery.ui.theme.css*</span></span>
- <span data-ttu-id="84d00-173">*jquery.ui.core.min.js*</span><span class="sxs-lookup"><span data-stu-id="84d00-173">*jquery.ui.core.min.js*</span></span>
- <span data-ttu-id="84d00-174">*jquery.ui.datepicker.min.js*</span><span class="sxs-lookup"><span data-stu-id="84d00-174">*jquery.ui.datepicker.min.js*</span></span>
- <span data-ttu-id="84d00-175">*DatePickerReady.js*</span><span class="sxs-lookup"><span data-stu-id="84d00-175">*DatePickerReady.js*</span></span>

<span data-ttu-id="84d00-176">次の例を示しています、実際のコードの下部に追加する必要があります、`head`内の要素、 *\shared\\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="84d00-176">The following example shows the actual code that you should add at the bottom of the `head` element in the *Views\Shared\\_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

<span data-ttu-id="84d00-177">完全な`head`セクションを示します。</span><span class="sxs-lookup"><span data-stu-id="84d00-177">The complete `head` section is shown here:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

<span data-ttu-id="84d00-178">[URL コンテンツ ヘルパー](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx)メソッド リソース パスは絶対パスに変換します。</span><span class="sxs-lookup"><span data-stu-id="84d00-178">The [URL content helper](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) method converts the resource path to an absolute path.</span></span> <span data-ttu-id="84d00-179">使用する必要があります`@URL.Content`アプリケーションが IIS で実行されているときに、これらのリソースを正しく参照します。</span><span class="sxs-lookup"><span data-stu-id="84d00-179">You must use `@URL.Content` to correctly reference these resources when the application is running on IIS.</span></span>

<span data-ttu-id="84d00-180">Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="84d00-180">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="84d00-181">編集リンクを選択しにカーソルを置きます、 **ReleaseDate**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="84d00-181">Select an edit link, then put the insertion point into the **ReleaseDate** field.</span></span> <span data-ttu-id="84d00-182">JQuery UI ポップアップ カレンダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="84d00-182">The jQuery UI popup calendar is displayed.</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

<span data-ttu-id="84d00-183">ほとんどの jQuery コントロールと同様に、datepicker では、広範囲にカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="84d00-183">Like most jQuery controls, the datepicker lets you customize it extensively.</span></span> <span data-ttu-id="84d00-184">については、次を参照してください。[ビジュアル カスタマイズ: jQuery UI のテーマを設計](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme)上、 [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/)サイトです。</span><span class="sxs-lookup"><span data-stu-id="84d00-184">For information, see [Visual Customization: Designing a jQuery UI theme](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) on the [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) site.</span></span>

### <a name="supporting-the-html5-date-input-control"></a><span data-ttu-id="84d00-185">HTML5 の日付の入力コントロールをサポートします。</span><span class="sxs-lookup"><span data-stu-id="84d00-185">Supporting the HTML5 Date Input Control</span></span>

<span data-ttu-id="84d00-186">多くのブラウザーでは、HTML5 をサポートを使用する入力の場合など、ネイティブに HTML5、`date`入力要素、および jQuery UI カレンダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="84d00-186">As more browsers support HTML5, you'll want to use the native HTML5 input, such as the `date` input element, and not use the jQuery UI calendar.</span></span> <span data-ttu-id="84d00-187">ブラウザーでは、それらがサポートする場合に自動的に HTML5 コントロールを使用するアプリケーションにロジックを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="84d00-187">You can add logic to your application to automatically use HTML5 controls if the browser supports them.</span></span> <span data-ttu-id="84d00-188">これを行うには、内容を置き換える、 *DatePickerReady.js*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="84d00-188">To do this, replace the contents of the *DatePickerReady.js* file with the following:</span></span>

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

<span data-ttu-id="84d00-189">このスクリプトの最初の行では、Modernizr を使用して、HTML5 日付の入力がサポートされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="84d00-189">The first line of this script uses Modernizr to verify that HTML5 date input is supported.</span></span> <span data-ttu-id="84d00-190">サポートされていない場合は、jQuery UI datepicker をフックする代わりにします。</span><span class="sxs-lookup"><span data-stu-id="84d00-190">If it's not supported, the jQuery UI date picker is hooked up instead.</span></span> <span data-ttu-id="84d00-191">([Modernizr](http://www.modernizr.com/docs/) HTML5 および css3 用のネイティブ実装の可用性を検出するオープン ソースの JavaScript ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="84d00-191">([Modernizr](http://www.modernizr.com/docs/) is an open-source JavaScript library that detects the availability of native implementations of HTML5 and CSS3.</span></span> <span data-ttu-id="84d00-192">Modernizr は含まれて作成した新しい ASP.NET MVC プロジェクトに)。</span><span class="sxs-lookup"><span data-stu-id="84d00-192">Modernizr is included in any new ASP.NET MVC projects that you create.)</span></span>

<span data-ttu-id="84d00-193">この変更を行った後に、元の Opera 11 などの HTML5 をサポートするブラウザーを使用してテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="84d00-193">After you've made this change, you can test it by using a browser that supports HTML5, such as Opera 11.</span></span> <span data-ttu-id="84d00-194">HTML5 と互換性のあるブラウザーを使用してアプリケーションを実行し、ムービーのエントリを編集します。</span><span class="sxs-lookup"><span data-stu-id="84d00-194">Run the application using an HTML5-compatible browser and edit a movie entry.</span></span> <span data-ttu-id="84d00-195">日付の HTML5 コントロールは jQuery UI ポップアップ カレンダーの代わりに使用されます。</span><span class="sxs-lookup"><span data-stu-id="84d00-195">The HTML5 date control is used instead of the jQuery UI popup calendar:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

<span data-ttu-id="84d00-196">ブラウザーの新しいバージョンを実装している HTML5 増分ためここでは有効なアプローチでは、さまざまな HTML5 のサポートに対応する web サイトにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="84d00-196">Because new versions of browsers are implementing HTML5 incrementally, a good approach for now is to add code to your website that accommodates a wide variety of HTML5 support.</span></span> <span data-ttu-id="84d00-197">より堅牢な*DatePickerReady.js*スクリプトが部分的にしか HTML5 日付コントロールをサポートする、サイトのサポート ブラウザーできるようにする次に示します。</span><span class="sxs-lookup"><span data-stu-id="84d00-197">For example, a more robust *DatePickerReady.js* script is shown below that lets your site support browsers that only partially support the HTML5 date control.</span></span>

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

<span data-ttu-id="84d00-198">このスクリプトを選択 HTML5`input`型の要素`date`HTML5 日付コントロールを完全にサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="84d00-198">This script selects HTML5 `input` elements of type `date` that don't fully support the HTML5 date control.</span></span> <span data-ttu-id="84d00-199">これらの要素の jQuery UI ポップアップ カレンダーをフックし、変更が、`type`属性から`date`に`text`です。</span><span class="sxs-lookup"><span data-stu-id="84d00-199">For those elements, it hooks up the jQuery UI popup calendar and then changes the `type` attribute from `date` to `text`.</span></span> <span data-ttu-id="84d00-200">変更することによって、`type`属性から`date`に`text`、部分的な HTML5 の日付のサポートが削除されました。</span><span class="sxs-lookup"><span data-stu-id="84d00-200">By changing the `type` attribute from `date` to `text`, partial HTML5 date support is eliminated.</span></span> <span data-ttu-id="84d00-201">さらに高い堅牢な*DatePickerReady.js*にスクリプトがある[JSFIDDLE](http://jsfiddle.net/XSTK8/15/)です。</span><span class="sxs-lookup"><span data-stu-id="84d00-201">An even more robust *DatePickerReady.js* script can be found at [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).</span></span>

### <a name="adding-nullable-dates-to-the-templates"></a><span data-ttu-id="84d00-202">Null 許容型日付を追加するのには、テンプレート</span><span class="sxs-lookup"><span data-stu-id="84d00-202">Adding Nullable Dates to the Templates</span></span>

<span data-ttu-id="84d00-203">日付に既存のテンプレートのいずれかを使用して、null の日付を渡す場合は、実行時エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="84d00-203">If you use one of the existing date templates and pass a null date, you'll get a run-time error.</span></span> <span data-ttu-id="84d00-204">日付テンプレートをより堅牢にするために、null 値を処理することを変更します。</span><span class="sxs-lookup"><span data-stu-id="84d00-204">To make the date templates more robust, you'll change them to handle null values.</span></span> <span data-ttu-id="84d00-205">Null 許容型日付をサポートするためにコードを変更します、 *Views\Shared\DisplayTemplates\DateTime.cshtml*以下。</span><span class="sxs-lookup"><span data-stu-id="84d00-205">To support nullable dates, change the code in the *Views\Shared\DisplayTemplates\DateTime.cshtml* to the following:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

<span data-ttu-id="84d00-206">コードが、モデルの場合、空の文字列を返します**null**です。</span><span class="sxs-lookup"><span data-stu-id="84d00-206">The code returns an empty string when the model is **null**.</span></span>

<span data-ttu-id="84d00-207">コードを変更、 *Views\Shared\EditorTemplates\Date.cshtml*には、次のファイル。</span><span class="sxs-lookup"><span data-stu-id="84d00-207">Change the code in the *Views\Shared\EditorTemplates\Date.cshtml* file to the following:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

<span data-ttu-id="84d00-208">このコードを実行すると、モデルが null でない場合、モデルの`DateTime`値を使用します。</span><span class="sxs-lookup"><span data-stu-id="84d00-208">When this code runs, if the model is not null, the model's `DateTime` value is used.</span></span> <span data-ttu-id="84d00-209">モデルが null の場合、現在の日付が使用されます。</span><span class="sxs-lookup"><span data-stu-id="84d00-209">If the model is null, the current date is used instead.</span></span>

### <a name="wrapup"></a><span data-ttu-id="84d00-210">(コース完了)</span><span class="sxs-lookup"><span data-stu-id="84d00-210">Wrapup</span></span>

<span data-ttu-id="84d00-211">このチュートリアルでは、ASP.NET のテンプレート化されたヘルパーの基本をカバーされてし、ASP.NET MVC アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="84d00-211">This tutorial has covered the basics of ASP.NET templated helpers and shows you how to use the jQuery UI datepicker popup calendar in an ASP.NET MVC application.</span></span> <span data-ttu-id="84d00-212">詳細については、これらのリソースを試してください。</span><span class="sxs-lookup"><span data-stu-id="84d00-212">For more information, try these resources:</span></span>

- <span data-ttu-id="84d00-213">ローカリゼーションのについては、Rajeesh のブログを参照してください。 [ASP.NET MVC で JQueryUI Datepicker](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/)です。</span><span class="sxs-lookup"><span data-stu-id="84d00-213">For information on localization, see Rajeesh's blog [JQueryUI Datepicker in ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).</span></span>
- <span data-ttu-id="84d00-214">JQuery UI については、次を参照してください。 [jQuery UI](http://docs.jquery.com/UI)です。</span><span class="sxs-lookup"><span data-stu-id="84d00-214">For information about jQuery UI, see [jQuery UI](http://docs.jquery.com/UI).</span></span>
- <span data-ttu-id="84d00-215">Datepicker コントロールをローカライズする方法については、次を参照してください。 [UI/Datepicker/ローカライズ](http://docs.jquery.com/UI/Datepicker/Localization)です。</span><span class="sxs-lookup"><span data-stu-id="84d00-215">For information about how to localize the datepicker control, see [UI/Datepicker/Localization](http://docs.jquery.com/UI/Datepicker/Localization).</span></span>
- <span data-ttu-id="84d00-216">ASP.NET MVC テンプレートの詳細についてを参照してください Brad Wilson のブログ シリーズ[ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)です。</span><span class="sxs-lookup"><span data-stu-id="84d00-216">For more information about the ASP.NET MVC templates, see Brad Wilson's blog series on [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html).</span></span> <span data-ttu-id="84d00-217">系列は、ASP.NET MVC 2 用に作成された、マテリアルは依然として、現在のバージョンの ASP.NET MVC に適用します。</span><span class="sxs-lookup"><span data-stu-id="84d00-217">Although the series was written for ASP.NET MVC 2, the material still applies for the current version of ASP.NET MVC.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="84d00-218">前へ</span><span class="sxs-lookup"><span data-stu-id="84d00-218">Previous</span></span>](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
