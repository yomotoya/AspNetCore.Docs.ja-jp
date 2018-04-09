---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: モデル バインディング機能と web フォームと JQuery UI Datepicker の統合 |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="0173d-104">モデル バインディング機能と web フォームと JQuery UI Datepicker の統合</span><span class="sxs-lookup"><span data-stu-id="0173d-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="0173d-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0173d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0173d-106">このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="0173d-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="0173d-107">モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。</span><span class="sxs-lookup"><span data-stu-id="0173d-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="0173d-108">この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。</span><span class="sxs-lookup"><span data-stu-id="0173d-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="0173d-109">このチュートリアルは、JQuery UI を追加する方法を示します[Datepicker ウィジェット](http://jqueryui.com/datepicker/)Web フォーム、および選択した値を持つデータベースを更新するバインドを使用してモデルにします。</span><span class="sxs-lookup"><span data-stu-id="0173d-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="0173d-110">このチュートリアルで作成されたプロジェクトで、[最初](retrieving-data.md)と[2 番目](updating-deleting-and-creating-data.md)シリーズの一部です。</span><span class="sxs-lookup"><span data-stu-id="0173d-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="0173d-111">実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。</span><span class="sxs-lookup"><span data-stu-id="0173d-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="0173d-112">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="0173d-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="0173d-113">これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="0173d-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="0173d-114">新機能のビルド</span><span class="sxs-lookup"><span data-stu-id="0173d-114">What you'll build</span></span>

<span data-ttu-id="0173d-115">このチュートリアルでは、次の手順を。</span><span class="sxs-lookup"><span data-stu-id="0173d-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="0173d-116">プロパティ、スチューデントの登録日を記録するモデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="0173d-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="0173d-117">JQuery UI Datepicker ウィジェットを使用する加入契約日を選択するユーザーを有効にします。</span><span class="sxs-lookup"><span data-stu-id="0173d-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="0173d-118">登録日の検証規則を実施します。</span><span class="sxs-lookup"><span data-stu-id="0173d-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="0173d-119">JQuery UI Datepicker ウィジェットでは、簡単にフィールドをユーザーが操作するときにポップアップ表示されるカレンダーから日付を選択することができます。</span><span class="sxs-lookup"><span data-stu-id="0173d-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="0173d-120">このウィジェットを使用すると、日付を手動で入力よりもユーザーにとって便利です。</span><span class="sxs-lookup"><span data-stu-id="0173d-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="0173d-121">データ操作のモデル バインディングを使用するページに Datepicker ウィジェットを統合するには、少量の追加作業だけが必要です。</span><span class="sxs-lookup"><span data-stu-id="0173d-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="0173d-122">モデルに新しいプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="0173d-122">Add a new property to the model</span></span>

<span data-ttu-id="0173d-123">最初に、追加されます、 **Datetime**プロパティ、受講者をモデル化し、データベースにその変更を移行します。</span><span class="sxs-lookup"><span data-stu-id="0173d-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="0173d-124">開いている**UniversityModels.cs**、学生モデルを強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0173d-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="0173d-125">**RangeAttribute**プロパティの検証規則の実施に含まれます。</span><span class="sxs-lookup"><span data-stu-id="0173d-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="0173d-126">このチュートリアルではものと Contoso 大学が、2013 年 1 月 1 日の設立されました、したがって以前の登録日が無効です。</span><span class="sxs-lookup"><span data-stu-id="0173d-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="0173d-127">パッケージの管理 ウィンドウでコマンドを実行して、移行を追加**追加移行 AddEnrollmentDate**です。</span><span class="sxs-lookup"><span data-stu-id="0173d-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="0173d-128">移行コードが Student テーブルに新しい Datetime 列を追加することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0173d-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="0173d-129">RangeAttribute で指定した値と一致するには、次の強調表示されたコードに示すように、新しい列の既定値を追加します。</span><span class="sxs-lookup"><span data-stu-id="0173d-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="0173d-130">移行ファイルへの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="0173d-130">Save your change to the migration file.</span></span>

<span data-ttu-id="0173d-131">データをもう一度シードする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0173d-131">You do not need to seed the data again.</span></span> <span data-ttu-id="0173d-132">そのため、開く**される Configuration.cs** 、Migrations フォルダー内でコードをコメント アウトまたは削除、**シード**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0173d-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="0173d-133">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="0173d-133">Save and close the file.</span></span>

<span data-ttu-id="0173d-134">これで、コマンドを実行**更新データベース**です。</span><span class="sxs-lookup"><span data-stu-id="0173d-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="0173d-135">列がデータベースに存在するようになりましたし EnrollmentDate の既定値があるすべての既存のレコードのことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0173d-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="0173d-136">登録日のダイナミック コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="0173d-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="0173d-137">表示および登録日を編集するためのコントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="0173d-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="0173d-138">この時点では、テキスト ボックスで値を編集します。</span><span class="sxs-lookup"><span data-stu-id="0173d-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="0173d-139">チュートリアルの後半には、JQuery Datepicker ウィジェットがテキスト ボックスが変わります。</span><span class="sxs-lookup"><span data-stu-id="0173d-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="0173d-140">最初にすることが重要に変更を加える必要はありません、 **AddStudent.aspx**ファイル。</span><span class="sxs-lookup"><span data-stu-id="0173d-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="0173d-141">DynamicEntity コントロールは、新しいプロパティを自動的にレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="0173d-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="0173d-142">開いている**Students.aspx**、し、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0173d-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="0173d-143">アプリケーションを実行し、日付を入力して登録日の値を設定することができますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0173d-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="0173d-144">新しい学生を追加するとき。</span><span class="sxs-lookup"><span data-stu-id="0173d-144">When adding a new student:</span></span>

![日付を設定](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="0173d-146">または、既存の値を編集します。</span><span class="sxs-lookup"><span data-stu-id="0173d-146">Or, editing an existing value:</span></span>

![日付を編集します。](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="0173d-148">日付動作しますが、それを入力すると、カスタマー エクスペリエンスを提供したいができない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0173d-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="0173d-149">次のセクションでは、カレンダーから日付の選択を有効になります。</span><span class="sxs-lookup"><span data-stu-id="0173d-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="0173d-150">JQuery UI を使用する NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="0173d-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="0173d-151">**ジュース UI** NuGet パッケージは、JQuery UI ウィジェットの web アプリケーションに簡単に統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="0173d-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="0173d-152">このパッケージを使用するには、NuGet でインストールします。</span><span class="sxs-lookup"><span data-stu-id="0173d-152">To use this package, install it through NuGet.</span></span>

![ジュース UI を追加します。](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="0173d-154">インストールするジュース UI のバージョンは、アプリケーションで JQuery のバージョンで競合があります。</span><span class="sxs-lookup"><span data-stu-id="0173d-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="0173d-155">このチュートリアルの前に、アプリケーションを実行して再試行してください。</span><span class="sxs-lookup"><span data-stu-id="0173d-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="0173d-156">JavaScript エラーが発生した場合は、JQuery のバージョンを調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0173d-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="0173d-157">スクリプト フォルダー (このチュートリアルの書き込み時にバージョン 1.8.2) に JQuery の想定されるバージョンを追加するか、Site.master、JQuery ファイルへのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="0173d-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="0173d-158">テンプレートをカスタマイズ DateTime を Datepicker ウィジェットが含まれます</span><span class="sxs-lookup"><span data-stu-id="0173d-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="0173d-159">Datetime 値を編集用の動的なデータ テンプレートには、Datepicker ウィジェットを追加します。</span><span class="sxs-lookup"><span data-stu-id="0173d-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="0173d-160">ウィジェットをテンプレートに追加すると、それが自動的とレンダリングされます新しい学生を追加するため、両方の形式で編集受講者をグリッド ビューで。</span><span class="sxs-lookup"><span data-stu-id="0173d-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="0173d-161">開いている**DateTime\_Edit.ascx**、し、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="0173d-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="0173d-162">分離コード ファイルでは、DatePicker の日付の最小と最大値を設定します。</span><span class="sxs-lookup"><span data-stu-id="0173d-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="0173d-163">これらの値を設定してはされないようにする無効な日付に移動します。</span><span class="sxs-lookup"><span data-stu-id="0173d-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="0173d-164">最小値と最大値を取得するには、 **RangeAttribute**が提供されている場合は、DateTime プロパティにします。</span><span class="sxs-lookup"><span data-stu-id="0173d-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="0173d-165">開いている**DateTime\_Edit.ascx.cs**、次の強調表示されているコード ページを追加および\_Load メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0173d-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="0173d-166">Web アプリケーションを実行し、AddStudent のページに移動します。</span><span class="sxs-lookup"><span data-stu-id="0173d-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="0173d-167">フィールドの値を指定し、登録日に、テキスト ボックスをクリックすると、カレンダーが表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0173d-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![日付の選択](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="0173d-169">日付を選択し、をクリックして**挿入**です。</span><span class="sxs-lookup"><span data-stu-id="0173d-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="0173d-170">RangeAttribute がサーバー上の検証を強制します。</span><span class="sxs-lookup"><span data-stu-id="0173d-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="0173d-171">Datepicker で minDate プロパティの設定によって適用することも検証クライアントにします。</span><span class="sxs-lookup"><span data-stu-id="0173d-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="0173d-172">予定表では、minDate の値より前の日付に移動し、ユーザーはことはできません。</span><span class="sxs-lookup"><span data-stu-id="0173d-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="0173d-173">グリッド ビュー内のレコードを編集するときにも、カレンダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0173d-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![GridView で Datepicker](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="0173d-175">まとめ</span><span class="sxs-lookup"><span data-stu-id="0173d-175">Conclusion</span></span>

<span data-ttu-id="0173d-176">このチュートリアルでは、モデル バインディングを使用する web フォームに JQuery ウィジェットを組み込む方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="0173d-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="0173d-177">次の[チュートリアル](using-query-string-values-to-retrieve-data.md)データを選択する場合、クエリ文字列の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="0173d-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0173d-178">[前へ](sorting-paging-and-filtering-data.md)
> [次へ](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="0173d-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
