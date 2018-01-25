---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: "検証 (c#)、モデルを追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: "コント ローラーの作成"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 6bce4a5d889f548cb1faec15842310703d7077b8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-to-the-model-c"></a><span data-ttu-id="aee2f-103">検証 (c#)、モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-103">Adding Validation to the Model (C#)</span></span>
====================
<span data-ttu-id="aee2f-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="aee2f-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="aee2f-105">このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="aee2f-106">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="aee2f-107">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="aee2f-108">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="aee2f-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="aee2f-109">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="aee2f-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="aee2f-110">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="aee2f-111">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="aee2f-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="aee2f-112">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="aee2f-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="aee2f-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="aee2f-114">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="aee2f-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="aee2f-115">C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="aee2f-116">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="aee2f-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="aee2f-117">Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="aee2f-118">検証ロジックをここで追加、`Movie`モデル、およびすることで、検証規則がいつでも作成またはアプリケーションを使用してムービーを編集しようと、ユーザーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-118">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user attempts to create or edit a movie using the application.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="aee2f-119">ドライに保つこと</span><span class="sxs-lookup"><span data-stu-id="aee2f-119">Keeping Things DRY</span></span>

<span data-ttu-id="aee2f-120">ASP.NET MVC の中核となる設計の基本思想の 1 つは、ドライ (「しないことを繰り返さない」) です。</span><span class="sxs-lookup"><span data-stu-id="aee2f-120">One of the core design tenets of ASP.NET MVC is DRY ("Don't Repeat Yourself").</span></span> <span data-ttu-id="aee2f-121">ASP.NET MVC では、機能や動作を 1 回だけを指定し、アプリケーションのすべての場所で反映されることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="aee2f-121">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an application.</span></span> <span data-ttu-id="aee2f-122">これは、量が減り、コードを記述する必要があります、コードが維持するために非常に簡単に記述する操作を行います。</span><span class="sxs-lookup"><span data-stu-id="aee2f-122">This reduces the amount of code you need to write and makes the code you do write much easier to maintain.</span></span>

<span data-ttu-id="aee2f-123">ASP.NET MVC と Entity Framework Code First によって提供される検証のサポートは、アクションでドライ原則の優れた例です。</span><span class="sxs-lookup"><span data-stu-id="aee2f-123">The validation support provided by ASP.NET MVC and Entity Framework Code First is a great example of the DRY principle in action.</span></span> <span data-ttu-id="aee2f-124">(モデル クラス) の 1 つの場所に検証規則を宣言によって指定でき、その後、それらのルールは、アプリケーションで everywhere 執行します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-124">You can declaratively specify validation rules in one place (in the model class) and then those rules are enforced everywhere in the application.</span></span>

<span data-ttu-id="aee2f-125">ムービー アプリケーションでのこの検証のサポートを利用を行う方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="aee2f-125">Let's look at how you can take advantage of this validation support in the movie application.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="aee2f-126">ムービー モデルに検証規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-126">Adding Validation Rules to the Movie Model</span></span>

<span data-ttu-id="aee2f-127">いくつかの検証ロジックを追加することから始めます、`Movie`クラスです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-127">You'll begin by adding some validation logic to the `Movie` class.</span></span>

<span data-ttu-id="aee2f-128">*Movie.cs* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-128">Open the *Movie.cs* file.</span></span> <span data-ttu-id="aee2f-129">追加、`using`を参照するファイルの上部にあるステートメント、 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。</span><span class="sxs-lookup"><span data-stu-id="aee2f-129">Add a `using` statement at the top of the file that references the [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

<span data-ttu-id="aee2f-130">名前空間は、.NET Framework の一部です。</span><span class="sxs-lookup"><span data-stu-id="aee2f-130">The namespace is part of the .NET Framework.</span></span> <span data-ttu-id="aee2f-131">すべてのクラスまたはプロパティを宣言して適用できる検証属性の組み込みのセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-131">It provides a built-in set of validation attributes that you can apply declaratively to any class or property.</span></span>

<span data-ttu-id="aee2f-132">更新できるように、`Movie`組み込み活用するためにクラス[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)、 [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)、および[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)検証属性.</span><span class="sxs-lookup"><span data-stu-id="aee2f-132">Now update the `Movie` class to take advantage of the built-in [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), and [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) validation attributes.</span></span> <span data-ttu-id="aee2f-133">属性を適用する場所の例として、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-133">Use the following code as an example of where to apply the attributes.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

<span data-ttu-id="aee2f-134">検証属性では、適用対象のモデル プロパティに適用する動作を指定します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-134">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="aee2f-135">`Required`属性は、プロパティは値である必要がありますを示します。 このサンプルでは、ムービーが値を持つことが、 `Title`、 `ReleaseDate`、 `Genre`、と`Price`有効にするにはプロパティです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-135">The `Required` attribute indicates that a property must have a value; in this sample, a movie has to have values for the `Title`, `ReleaseDate`, `Genre`, and `Price` properties in order to be valid.</span></span> <span data-ttu-id="aee2f-136">`Range` 属性は、指定した範囲内に値を制限します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-136">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="aee2f-137">`StringLength` 属性では、文字列プロパティの最大長を設定でき、オプションとして最小長も設定できます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-137">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span>

<span data-ttu-id="aee2f-138">コードでは、アプリケーションがデータベースに変更を保存する前に、モデル クラスを指定する検証規則が適用されている最初でいます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-138">Code First ensures that the validation rules you specify on a model class are enforced before the application saves changes in the database.</span></span> <span data-ttu-id="aee2f-139">次のコードが例外をスローするなど、ときに、`SaveChanges`メソッドは、いくつか必要なため`Movie`プロパティ値が不足していると、価格が (0 では、有効な範囲外です)。</span><span class="sxs-lookup"><span data-stu-id="aee2f-139">For example, the code below will throw an exception when the `SaveChanges` method is called, because several required `Movie` property values are missing and the price is zero (which is out of the valid range).</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

<span data-ttu-id="aee2f-140">検証するルールの .NET Framework によって自動的に適用できるように、アプリケーションより堅牢なします。</span><span class="sxs-lookup"><span data-stu-id="aee2f-140">Having validation rules automatically enforced by the .NET Framework helps make your application more robust.</span></span> <span data-ttu-id="aee2f-141">また、ユーザーが何かを検証することを忘れてしまい、データベースに不適切なデータが誤って格納されることもなくなります。</span><span class="sxs-lookup"><span data-stu-id="aee2f-141">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

<span data-ttu-id="aee2f-142">ここでは、完全なコードが、更新されたリスト*Movie.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="aee2f-142">Here's a complete code listing for the updated *Movie.cs* file:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a><span data-ttu-id="aee2f-143">ASP.NET MVC では、UI の検証エラー</span><span class="sxs-lookup"><span data-stu-id="aee2f-143">Validation Error UI in ASP.NET MVC</span></span>

<span data-ttu-id="aee2f-144">アプリケーションを再実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="aee2f-144">Re-run the application and navigate to the */Movies* URL.</span></span>

<span data-ttu-id="aee2f-145">をクリックして、**作成の映画**リンクを新しいムービーを追加します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-145">Click the **Create Movie** link to add a new movie.</span></span> <span data-ttu-id="aee2f-146">いくつかの無効な値にフォームに記入し、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="aee2f-146">Fill out the form with some invalid values and then click the **Create** button.</span></span>

<span data-ttu-id="aee2f-147">[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aee2f-147">[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)</span></span>

<span data-ttu-id="aee2f-148">フォームが自動的に使用方法、背景色を無効なデータが含まれ、それぞれの横にある適切な検証エラー メッセージが生成されるテキスト ボックスを強調表示することを確認します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-148">Notice how the form has automatically used a background color to highlight the text boxes that contain invalid data and has emitted an appropriate validation error message next to each one.</span></span> <span data-ttu-id="aee2f-149">エラー メッセージは、注釈を付けるときに指定したエラーの文字列を検索、`Movie`クラスです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-149">The error messages match the error strings you specified when you annotated the `Movie` class.</span></span> <span data-ttu-id="aee2f-150">(ユーザーは、JavaScript が無効になっている) 場合に、エラーはクライアント側の JavaScript を使用して) とサーバー側の両方が適用されます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-150">The errors are enforced both client-side (using JavaScript) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="aee2f-151">実際の利点でのコードの 1 つの行を変更する必要はありませんでした、`MoviesController`クラスまたは、 *Create.cshtml* UI この検証を有効にするために表示します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-151">A real benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="aee2f-152">コント ローラーと自動的にこのチュートリアルで先ほど作成したビューの選択、検証規則は、属性の使用を指定したことを`Movie`モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="aee2f-152">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified using attributes on the `Movie` model class.</span></span>

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a><span data-ttu-id="aee2f-153">作成の表示し、アクション メソッドを作成で発生する検証方法</span><span class="sxs-lookup"><span data-stu-id="aee2f-153">How Validation Occurs in the Create View and Create Action Method</span></span>

<span data-ttu-id="aee2f-154">コントローラーまたはビューのコードを更新しなくても検証 UI が生成する仕組みが気になるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="aee2f-154">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="aee2f-155">[次へ] の一覧が動作を示しています、`Create`内のメソッド、`MovieController`ようなクラスです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-155">The next listing shows what the `Create` methods in the `MovieController` class look like.</span></span> <span data-ttu-id="aee2f-156">このチュートリアルで先ほどの作成から変更されていません。</span><span class="sxs-lookup"><span data-stu-id="aee2f-156">They're unchanged from how you created them earlier in this tutorial.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

<span data-ttu-id="aee2f-157">最初のアクション メソッドには、初期作成フォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-157">The first action method displays the initial Create form.</span></span> <span data-ttu-id="aee2f-158">2 つ目は、フォーム ポストを処理します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-158">The second handles the form post.</span></span> <span data-ttu-id="aee2f-159">2 番目`Create`メソッド呼び出し`ModelState.IsValid`をムービーに検証エラーがあるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-159">The second `Create` method calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="aee2f-160">このメソッドを呼び出すと、オブジェクトに適用されているすべての検証属性が評価されます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-160">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="aee2f-161">オブジェクトに検証エラーがある場合、`Create`メソッドには、フォームが再表示します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-161">If the object has validation errors, the `Create` method redisplays the form.</span></span> <span data-ttu-id="aee2f-162">エラーがない場合、メソッドはデータベースに新しいムービーを保存します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-162">If there are no errors, the method saves the new movie in the database.</span></span>

<span data-ttu-id="aee2f-163">以下は、 *Create.cshtml*チュートリアルで先ほどスキャフォールディングされたビュー テンプレート。</span><span class="sxs-lookup"><span data-stu-id="aee2f-163">Below is the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="aee2f-164">これは、前に示した両方のアクション メソッドで、初期フォームの表示と、エラー発生時のフォームの再表示に使われます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-164">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

<span data-ttu-id="aee2f-165">コードの使用方法に注意してください、`Html.EditorFor`を出力するヘルパー、`<input>`要素ごと`Movie`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-165">Notice how the code uses an `Html.EditorFor` helper to output the `<input>` element for each `Movie` property.</span></span> <span data-ttu-id="aee2f-166">呼び出しは、このヘルパーの横にある、`Html.ValidationMessageFor`ヘルパー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-166">Next to this helper is a call to the `Html.ValidationMessageFor` helper method.</span></span> <span data-ttu-id="aee2f-167">これら 2 つのヘルパー メソッドがコント ローラーによって、ビューに渡されるモデル オブジェクトを操作 (ここで、`Movie`オブジェクト)。</span><span class="sxs-lookup"><span data-stu-id="aee2f-167">These two helper methods work with the model object that's passed by the controller to the view (in this case, a `Movie` object).</span></span> <span data-ttu-id="aee2f-168">これらは、自動的に、モデルと表示のエラー メッセージを適切に指定された検証属性を探します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-168">They automatically look for validation attributes specified on the model and display error messages as appropriate.</span></span>

<span data-ttu-id="aee2f-169">このアプローチのすばらしい点は、コント ローラーもビュー テンプレートの作成を知っているものが適用されている実際の検証規則の情報や、特定のエラー メッセージが表示されてです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-169">What's really nice about this approach is that neither the controller nor the Create view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="aee2f-170">検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。</span><span class="sxs-lookup"><span data-stu-id="aee2f-170">The validation rules and the error strings are specified only in the `Movie` class.</span></span>

<span data-ttu-id="aee2f-171">検証ロジックを後で変更する場合は、これを行う 1 つの場所にします。</span><span class="sxs-lookup"><span data-stu-id="aee2f-171">If you want to change the validation logic later, you can do so in exactly one place.</span></span> <span data-ttu-id="aee2f-172">アプリケーションの異なる部分で規則の適用方法が一貫しない可能性を心配する必要はありません。すべての検証ロジックは 1 か所で定義され、すべての場所で使われます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-172">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="aee2f-173">これにより、コードの簡潔さが保たれ、簡単に維持や更新できます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-173">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="aee2f-174">すると、いることを意味して完全性を記念するためです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-174">And it means that that you'll be fully honoring the DRY principle.</span></span>

## <a name="adding-formatting-to-the-movie-model"></a><span data-ttu-id="aee2f-175">書式設定、ムービーのモデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-175">Adding Formatting to the Movie Model</span></span>

<span data-ttu-id="aee2f-176">*Movie.cs* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="aee2f-176">Open the *Movie.cs* file.</span></span> <span data-ttu-id="aee2f-177">[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間が一連の組み込みの検証属性だけでなく書式属性を提供します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-177">The [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="aee2f-178">適用する、 [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性と[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)リリース日と価格のフィールドの列挙値。</span><span class="sxs-lookup"><span data-stu-id="aee2f-178">You'll apply the [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute and a [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="aee2f-179">次のコードは、`ReleaseDate`と`Price`と適切なプロパティ[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="aee2f-179">The following code shows the `ReleaseDate` and `Price` properties with the appropriate [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

<span data-ttu-id="aee2f-180">代わりに、明示的に設定できます、 [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)値。</span><span class="sxs-lookup"><span data-stu-id="aee2f-180">Alternatively, you could explicitly set a [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) value.</span></span> <span data-ttu-id="aee2f-181">次のコードは、日付の書式設定文字列のリリース日付プロパティを示しています (つまり、"d") です。</span><span class="sxs-lookup"><span data-stu-id="aee2f-181">The following code shows the release date property with a date format string (namely, "d").</span></span> <span data-ttu-id="aee2f-182">リリース日の一部として、時間にするないを指定するのにには、これを使用します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-182">You'd use this to specify that you don't want to time as part of the release date.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

<span data-ttu-id="aee2f-183">次のコードの形式、`Price`通貨としてのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="aee2f-183">The following code formats the `Price` property as currency.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

<span data-ttu-id="aee2f-184">完全な`Movie`クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="aee2f-184">The complete `Movie` class is shown below.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

<span data-ttu-id="aee2f-185">アプリケーションを実行しを参照、`Movies`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="aee2f-185">Run the application and browse to the `Movies` controller.</span></span>

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

<span data-ttu-id="aee2f-187">このシリーズの次のパートでは、アプリケーションを確認し、自動的に生成される `Details` および `Delete` メソッドに対していくつかの改良を行います。</span><span class="sxs-lookup"><span data-stu-id="aee2f-187">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="aee2f-188">[前へ](adding-a-new-field.md)
[次へ](improving-the-details-and-delete-methods.md)</span><span class="sxs-lookup"><span data-stu-id="aee2f-188">[Previous](adding-a-new-field.md)
[Next](improving-the-details-and-delete-methods.md)</span></span>
