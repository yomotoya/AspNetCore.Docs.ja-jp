---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: 検証 (VB) モデルを追加する |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 86058530aa00ecbc00aeebc6ed7b5cf019fdad72
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872924"
---
<a name="adding-validation-to-the-model-vb"></a><span data-ttu-id="735a5-103">検証 (VB) モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="735a5-103">Adding Validation to the Model (VB)</span></span>
====================
<span data-ttu-id="735a5-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="735a5-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="735a5-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="735a5-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="735a5-106">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="735a5-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="735a5-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="735a5-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="735a5-108">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="735a5-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="735a5-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="735a5-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="735a5-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="735a5-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="735a5-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="735a5-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="735a5-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="735a5-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="735a5-113">VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="735a5-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="735a5-114">[バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="735a5-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="735a5-115">C# を使用する場合に切り替え、 [c# バージョン](../cs/adding-validation-to-the-model.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="735a5-115">If you prefer C#, switch to the [C# version](../cs/adding-validation-to-the-model.md) of this tutorial.</span></span>


<span data-ttu-id="735a5-116">検証ロジックをここで追加、`Movie`モデル、およびすることで、検証規則がいつでも作成またはアプリケーションを使用してムービーを編集しようと、ユーザーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="735a5-116">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user attempts to create or edit a movie using the application.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="735a5-117">ドライに保つこと</span><span class="sxs-lookup"><span data-stu-id="735a5-117">Keeping Things DRY</span></span>

<span data-ttu-id="735a5-118">ASP.NET MVC の中核となる設計の基本思想の 1 つは、ドライ (「しないことを繰り返さない」) です。</span><span class="sxs-lookup"><span data-stu-id="735a5-118">One of the core design tenets of ASP.NET MVC is DRY ("Don't Repeat Yourself").</span></span> <span data-ttu-id="735a5-119">ASP.NET MVC では、機能や動作を 1 回だけを指定し、アプリケーションのすべての場所で反映されることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="735a5-119">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an application.</span></span> <span data-ttu-id="735a5-120">これは、量が減り、コードを記述する必要があります、コードが維持するために非常に簡単に記述する操作を行います。</span><span class="sxs-lookup"><span data-stu-id="735a5-120">This reduces the amount of code you need to write and makes the code you do write much easier to maintain.</span></span>

<span data-ttu-id="735a5-121">ASP.NET MVC と Entity Framework Code First によって提供される検証のサポートは、アクションでドライ原則の優れた例です。</span><span class="sxs-lookup"><span data-stu-id="735a5-121">The validation support provided by ASP.NET MVC and Entity Framework Code First is a great example of the DRY principle in action.</span></span> <span data-ttu-id="735a5-122">(モデル クラス) の 1 つの場所に検証規則を宣言によって指定でき、その後、それらのルールは、アプリケーションで everywhere 執行します。</span><span class="sxs-lookup"><span data-stu-id="735a5-122">You can declaratively specify validation rules in one place (in the model class) and then those rules are enforced everywhere in the application.</span></span>

<span data-ttu-id="735a5-123">ムービー アプリケーションでのこの検証のサポートを利用を行う方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="735a5-123">Let's look at how you can take advantage of this validation support in the movie application.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="735a5-124">ムービー モデルに検証規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="735a5-124">Adding Validation Rules to the Movie Model</span></span>

<span data-ttu-id="735a5-125">いくつかの検証ロジックを追加することから始めます、`Movie`クラスです。</span><span class="sxs-lookup"><span data-stu-id="735a5-125">You'll begin by adding some validation logic to the `Movie` class.</span></span>

<span data-ttu-id="735a5-126">開く、 *Movie.vb*ファイル。</span><span class="sxs-lookup"><span data-stu-id="735a5-126">Open the *Movie.vb* file.</span></span> <span data-ttu-id="735a5-127">追加、`Imports`を参照するファイルの上部にあるステートメント、 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。</span><span class="sxs-lookup"><span data-stu-id="735a5-127">Add a `Imports` statement at the top of the file that references the [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace:</span></span>

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

<span data-ttu-id="735a5-128">名前空間は、.NET Framework の一部です。</span><span class="sxs-lookup"><span data-stu-id="735a5-128">The namespace is part of the .NET Framework.</span></span> <span data-ttu-id="735a5-129">すべてのクラスまたはプロパティを宣言して適用できる検証属性の組み込みのセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="735a5-129">It provides a built-in set of validation attributes that you can apply declaratively to any class or property.</span></span>

<span data-ttu-id="735a5-130">更新できるように、`Movie`組み込み活用するためにクラス[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)、 [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)、および[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)検証属性.</span><span class="sxs-lookup"><span data-stu-id="735a5-130">Now update the `Movie` class to take advantage of the built-in [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), and [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) validation attributes.</span></span> <span data-ttu-id="735a5-131">属性を適用する場所の例として、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="735a5-131">Use the following code as an example of where to apply the attributes.</span></span>

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

<span data-ttu-id="735a5-132">検証属性では、適用対象のモデル プロパティに適用する動作を指定します。</span><span class="sxs-lookup"><span data-stu-id="735a5-132">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="735a5-133">`Required`属性は、プロパティは値である必要がありますを示します。 このサンプルでは、ムービーが値を持つことが、 `Title`、 `ReleaseDate`、 `Genre`、と`Price`有効にするにはプロパティです。</span><span class="sxs-lookup"><span data-stu-id="735a5-133">The `Required` attribute indicates that a property must have a value; in this sample, a movie has to have values for the `Title`, `ReleaseDate`, `Genre`, and `Price` properties in order to be valid.</span></span> <span data-ttu-id="735a5-134">`Range` 属性は、指定した範囲内に値を制限します。</span><span class="sxs-lookup"><span data-stu-id="735a5-134">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="735a5-135">`StringLength` 属性では、文字列プロパティの最大長を設定でき、オプションとして最小長も設定できます。</span><span class="sxs-lookup"><span data-stu-id="735a5-135">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span>

<span data-ttu-id="735a5-136">コードでは、アプリケーションがデータベースに変更を保存する前に、モデル クラスを指定する検証規則が適用されている最初でいます。</span><span class="sxs-lookup"><span data-stu-id="735a5-136">Code First ensures that the validation rules you specify on a model class are enforced before the application saves changes in the database.</span></span> <span data-ttu-id="735a5-137">次のコードが例外をスローするなど、ときに、`SaveChanges`メソッドは、いくつか必要なため`Movie`プロパティ値が不足していると、価格が (0 では、有効な範囲外です)。</span><span class="sxs-lookup"><span data-stu-id="735a5-137">For example, the code below will throw an exception when the `SaveChanges` method is called, because several required `Movie` property values are missing and the price is zero (which is out of the valid range).</span></span>

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

<span data-ttu-id="735a5-138">検証するルールの .NET Framework によって自動的に適用できるように、アプリケーションより堅牢なします。</span><span class="sxs-lookup"><span data-stu-id="735a5-138">Having validation rules automatically enforced by the .NET Framework helps make your application more robust.</span></span> <span data-ttu-id="735a5-139">また、ユーザーが何かを検証することを忘れてしまい、データベースに不適切なデータが誤って格納されることもなくなります。</span><span class="sxs-lookup"><span data-stu-id="735a5-139">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

<span data-ttu-id="735a5-140">ここでは、完全なコードが、更新されたリスト*Movie.vb*ファイル。</span><span class="sxs-lookup"><span data-stu-id="735a5-140">Here's a complete code listing for the updated *Movie.vb* file:</span></span>

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a><span data-ttu-id="735a5-141">ASP.NET MVC では、UI の検証エラー</span><span class="sxs-lookup"><span data-stu-id="735a5-141">Validation Error UI in ASP.NET MVC</span></span>

<span data-ttu-id="735a5-142">アプリケーションを再実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="735a5-142">Re-run the application and navigate to the */Movies* URL.</span></span>

<span data-ttu-id="735a5-143">をクリックして、**作成の映画**リンクを新しいムービーを追加します。</span><span class="sxs-lookup"><span data-stu-id="735a5-143">Click the **Create Movie** link to add a new movie.</span></span> <span data-ttu-id="735a5-144">いくつかの無効な値にフォームに記入し、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="735a5-144">Fill out the form with some invalid values and then click the **Create** button.</span></span>

<span data-ttu-id="735a5-145">[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="735a5-145">[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)</span></span>

<span data-ttu-id="735a5-146">フォームが自動的に使用方法、背景色を無効なデータが含まれ、それぞれの横にある適切な検証エラー メッセージが生成されるテキスト ボックスを強調表示することを確認します。</span><span class="sxs-lookup"><span data-stu-id="735a5-146">Notice how the form has automatically used a background color to highlight the text boxes that contain invalid data and has emitted an appropriate validation error message next to each one.</span></span> <span data-ttu-id="735a5-147">エラー メッセージは、注釈を付けるときに指定したエラーの文字列を検索、`Movie`クラスです。</span><span class="sxs-lookup"><span data-stu-id="735a5-147">The error messages match the error strings you specified when you annotated the `Movie` class.</span></span> <span data-ttu-id="735a5-148">(ユーザーは、JavaScript が無効になっている) 場合に、エラーはクライアント側の JavaScript を使用して) とサーバー側の両方が適用されます。</span><span class="sxs-lookup"><span data-stu-id="735a5-148">The errors are enforced both client-side (using JavaScript) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="735a5-149">実際の利点でのコードの 1 つの行を変更する必要はありませんでした、`MoviesController`クラスまたは、 *Create.vbhtml* UI この検証を有効にするために表示します。</span><span class="sxs-lookup"><span data-stu-id="735a5-149">A real benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.vbhtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="735a5-150">コント ローラーと自動的にこのチュートリアルで先ほど作成したビューの選択、検証規則は、属性の使用を指定したことを`Movie`モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="735a5-150">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified using attributes on the `Movie` model class.</span></span>

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a><span data-ttu-id="735a5-151">作成の表示し、アクション メソッドを作成で発生する検証方法</span><span class="sxs-lookup"><span data-stu-id="735a5-151">How Validation Occurs in the Create View and Create Action Method</span></span>

<span data-ttu-id="735a5-152">コントローラーまたはビューのコードを更新しなくても検証 UI が生成する仕組みが気になるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="735a5-152">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="735a5-153">[次へ] の一覧が動作を示しています、`Create`内のメソッド、`MovieController`ようなクラスです。</span><span class="sxs-lookup"><span data-stu-id="735a5-153">The next listing shows what the `Create` methods in the `MovieController` class look like.</span></span> <span data-ttu-id="735a5-154">このチュートリアルで先ほどの作成から変更されていません。</span><span class="sxs-lookup"><span data-stu-id="735a5-154">They're unchanged from how you created them earlier in this tutorial.</span></span>

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

<span data-ttu-id="735a5-155">最初のアクション メソッドには、初期作成フォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="735a5-155">The first action method displays the initial Create form.</span></span> <span data-ttu-id="735a5-156">2 つ目は、フォーム ポストを処理します。</span><span class="sxs-lookup"><span data-stu-id="735a5-156">The second handles the form post.</span></span> <span data-ttu-id="735a5-157">2 番目`Create`メソッド呼び出し`ModelState.IsValid`をムービーに検証エラーがあるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="735a5-157">The second `Create` method calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="735a5-158">このメソッドを呼び出すと、オブジェクトに適用されているすべての検証属性が評価されます。</span><span class="sxs-lookup"><span data-stu-id="735a5-158">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="735a5-159">オブジェクトに検証エラーがある場合、`Create`メソッドには、フォームが再表示します。</span><span class="sxs-lookup"><span data-stu-id="735a5-159">If the object has validation errors, the `Create` method redisplays the form.</span></span> <span data-ttu-id="735a5-160">エラーがない場合、メソッドはデータベースに新しいムービーを保存します。</span><span class="sxs-lookup"><span data-stu-id="735a5-160">If there are no errors, the method saves the new movie in the database.</span></span>

<span data-ttu-id="735a5-161">以下は、 *Create.vbhtml*チュートリアルで先ほどスキャフォールディングされたビュー テンプレート。</span><span class="sxs-lookup"><span data-stu-id="735a5-161">Below is the *Create.vbhtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="735a5-162">これは、前に示した両方のアクション メソッドで、初期フォームの表示と、エラー発生時のフォームの再表示に使われます。</span><span class="sxs-lookup"><span data-stu-id="735a5-162">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

<span data-ttu-id="735a5-163">コードの使用方法に注意してください、`Html.EditorFor`を出力するヘルパー、`<input>`要素ごと`Movie`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="735a5-163">Notice how the code uses an `Html.EditorFor` helper to output the `<input>` element for each `Movie` property.</span></span> <span data-ttu-id="735a5-164">呼び出しは、このヘルパーの横にある、`Html.ValidationMessageFor`ヘルパー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="735a5-164">Next to this helper is a call to the `Html.ValidationMessageFor` helper method.</span></span> <span data-ttu-id="735a5-165">これら 2 つのヘルパー メソッドがコント ローラーによって、ビューに渡されるモデル オブジェクトを操作 (ここで、`Movie`オブジェクト)。</span><span class="sxs-lookup"><span data-stu-id="735a5-165">These two helper methods work with the model object that's passed by the controller to the view (in this case, a `Movie` object).</span></span> <span data-ttu-id="735a5-166">これらは、自動的に、モデルと表示のエラー メッセージを適切に指定された検証属性を探します。</span><span class="sxs-lookup"><span data-stu-id="735a5-166">They automatically look for validation attributes specified on the model and display error messages as appropriate.</span></span>

<span data-ttu-id="735a5-167">このアプローチのすばらしい点は、コント ローラーもビュー テンプレートの作成を知っているものが適用されている実際の検証規則の情報や、特定のエラー メッセージが表示されてです。</span><span class="sxs-lookup"><span data-stu-id="735a5-167">What's really nice about this approach is that neither the controller nor the Create view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="735a5-168">検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。</span><span class="sxs-lookup"><span data-stu-id="735a5-168">The validation rules and the error strings are specified only in the `Movie` class.</span></span>

<span data-ttu-id="735a5-169">検証ロジックを後で変更する場合は、これを行う 1 つの場所にします。</span><span class="sxs-lookup"><span data-stu-id="735a5-169">If you want to change the validation logic later, you can do so in exactly one place.</span></span> <span data-ttu-id="735a5-170">アプリケーションの異なる部分で規則の適用方法が一貫しない可能性を心配する必要はありません。すべての検証ロジックは 1 か所で定義され、すべての場所で使われます。</span><span class="sxs-lookup"><span data-stu-id="735a5-170">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="735a5-171">これにより、コードの簡潔さが保たれ、簡単に維持や更新できます。</span><span class="sxs-lookup"><span data-stu-id="735a5-171">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="735a5-172">また、これは DRY 原則に完全に従うことを意味します。</span><span class="sxs-lookup"><span data-stu-id="735a5-172">And it means that you'll be fully honoring the DRY principle.</span></span>

## <a name="adding-formatting-to-the-movie-model"></a><span data-ttu-id="735a5-173">書式設定、ムービーのモデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="735a5-173">Adding Formatting to the Movie Model</span></span>

<span data-ttu-id="735a5-174">開く、 *Movie.vb*ファイル。</span><span class="sxs-lookup"><span data-stu-id="735a5-174">Open the *Movie.vb* file.</span></span> <span data-ttu-id="735a5-175">[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間が一連の組み込みの検証属性だけでなく書式属性を提供します。</span><span class="sxs-lookup"><span data-stu-id="735a5-175">The [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="735a5-176">適用する、 [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性と[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)リリース日と価格のフィールドの列挙値。</span><span class="sxs-lookup"><span data-stu-id="735a5-176">You'll apply the [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute and a [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="735a5-177">次のコードは、`ReleaseDate`と`Price`と適切なプロパティ[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="735a5-177">The following code shows the `ReleaseDate` and `Price` properties with the appropriate [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute.</span></span>

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

<span data-ttu-id="735a5-178">代わりに、明示的に設定できます、 [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)値。</span><span class="sxs-lookup"><span data-stu-id="735a5-178">Alternatively, you could explicitly set a [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) value.</span></span> <span data-ttu-id="735a5-179">次のコードは、日付の書式設定文字列のリリース日付プロパティを示しています (つまり、"d") です。</span><span class="sxs-lookup"><span data-stu-id="735a5-179">The following code shows the release date property with a date format string (namely, "d").</span></span> <span data-ttu-id="735a5-180">リリース日の一部として、時間にするないを指定するのにには、これを使用します。</span><span class="sxs-lookup"><span data-stu-id="735a5-180">You'd use this to specify that you don't want to time as part of the release date.</span></span>

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

<span data-ttu-id="735a5-181">次のコードの形式、`Price`通貨としてのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="735a5-181">The following code formats the `Price` property as currency.</span></span>

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

<span data-ttu-id="735a5-182">完全な`Movie`クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="735a5-182">The complete `Movie` class is shown below.</span></span>

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

<span data-ttu-id="735a5-183">アプリケーションを実行しを参照、`Movies`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="735a5-183">Run the application and browse to the `Movies` controller.</span></span>

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

<span data-ttu-id="735a5-185">シリーズの次の部分で、アプリケーションを確認し、いくつかの改善を自動的に生成されたおを`Details`と`Delete`メソッド.</span><span class="sxs-lookup"><span data-stu-id="735a5-185">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods..</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="735a5-186">[前へ](adding-a-new-field.md)
> [次へ](improving-the-details-and-delete-methods.md)</span><span class="sxs-lookup"><span data-stu-id="735a5-186">[Previous](adding-a-new-field.md)
[Next](improving-the-details-and-delete-methods.md)</span></span>
