---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: モデルの検証の追加 |Microsoft ドキュメント
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 39d1d9d4cb8b11f7ce5a3a85c51f652115d79db7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874549"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="62c4f-104">モデルの検証の追加</span><span class="sxs-lookup"><span data-stu-id="62c4f-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="62c4f-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="62c4f-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="62c4f-106">このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="62c4f-107">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="62c4f-108">検証ロジックをここで追加、`Movie`モデル、およびすることで、検証規則がいつでも作成またはアプリケーションを使用してムービーを編集しようと、ユーザーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-108">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user attempts to create or edit a movie using the application.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="62c4f-109">ドライに保つこと</span><span class="sxs-lookup"><span data-stu-id="62c4f-109">Keeping Things DRY</span></span>

<span data-ttu-id="62c4f-110">ASP.NET MVC の中核となる設計の基本思想の 1 つはドライ (&quot;しないことを繰り返さない&quot;)。</span><span class="sxs-lookup"><span data-stu-id="62c4f-110">One of the core design tenets of ASP.NET MVC is DRY (&quot;Don't Repeat Yourself&quot;).</span></span> <span data-ttu-id="62c4f-111">ASP.NET MVC では、機能や動作を 1 回だけを指定し、アプリケーションのすべての場所で反映されることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="62c4f-111">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an application.</span></span> <span data-ttu-id="62c4f-112">これを記述する必要のあるコードの量を削減と低いエラーが発生しやすいと保守が簡単に記述して、コード。</span><span class="sxs-lookup"><span data-stu-id="62c4f-112">This reduces the amount of code you need to write and makes the code you do write less error prone and easier to maintain.</span></span>

<span data-ttu-id="62c4f-113">ASP.NET MVC と Entity Framework Code First によって提供される検証のサポートは、アクションでドライ原則の優れた例です。</span><span class="sxs-lookup"><span data-stu-id="62c4f-113">The validation support provided by ASP.NET MVC and Entity Framework Code First is a great example of the DRY principle in action.</span></span> <span data-ttu-id="62c4f-114">(モデル クラス) の 1 つの場所に検証規則を宣言によって指定でき、どこからでも、アプリケーションで、規則を適用します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-114">You can declaratively specify validation rules in one place (in the model class) and the rules are enforced everywhere in the application.</span></span>

<span data-ttu-id="62c4f-115">ムービー アプリケーションでのこの検証のサポートを利用を行う方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="62c4f-115">Let's look at how you can take advantage of this validation support in the movie application.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="62c4f-116">ムービー モデルに検証規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-116">Adding Validation Rules to the Movie Model</span></span>

<span data-ttu-id="62c4f-117">いくつかの検証ロジックを追加することから始めます、`Movie`クラスです。</span><span class="sxs-lookup"><span data-stu-id="62c4f-117">You'll begin by adding some validation logic to the `Movie` class.</span></span>

<span data-ttu-id="62c4f-118">*Movie.cs* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-118">Open the *Movie.cs* file.</span></span> <span data-ttu-id="62c4f-119">追加、`using`を参照するファイルの上部にあるステートメント、 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間。</span><span class="sxs-lookup"><span data-stu-id="62c4f-119">Add a `using` statement at the top of the file that references the [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

<span data-ttu-id="62c4f-120">名前空間が含まれていない`System.Web`です。</span><span class="sxs-lookup"><span data-stu-id="62c4f-120">Notice the namespace does not contain `System.Web`.</span></span> <span data-ttu-id="62c4f-121">DataAnnotations は、すべてのクラスまたはプロパティを宣言して適用できる検証属性の組み込みのセットを提供します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-121">DataAnnotations provides a built-in set of validation attributes that you can apply declaratively to any class or property.</span></span>

<span data-ttu-id="62c4f-122">更新できるように、`Movie`組み込み活用するためにクラス[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)、 [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)、および[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)検証属性.</span><span class="sxs-lookup"><span data-stu-id="62c4f-122">Now update the `Movie` class to take advantage of the built-in [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), and [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) validation attributes.</span></span> <span data-ttu-id="62c4f-123">属性を適用する場所の例として、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-123">Use the following code as an example of where to apply the attributes.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

<span data-ttu-id="62c4f-124">アプリケーションを実行し、次の実行時エラーがもう一度表示されます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-124">Run the application and you will again get the following run time error:</span></span>

<span data-ttu-id="62c4f-125">***データベースが作成されたために、'MovieDBContext' コンテキストのバックアップ モデルが変更されました。Code First Migrations を使用して、データベースを更新するを検討してください ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。***</span><span class="sxs-lookup"><span data-stu-id="62c4f-125">***The model backing the 'MovieDBContext' context has changed since the database was created. Consider using Code First Migrations to update the database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***</span></span>

<span data-ttu-id="62c4f-126">スキーマを更新するのに移行を使用します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-126">We will use migrations to update the schema.</span></span> <span data-ttu-id="62c4f-127">ソリューションをビルドし、開きます、**パッケージ マネージャー コンソール**ウィンドウと、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-127">Build the solution, and then open the **Package Manager Console** window and enter the following commands:</span></span>

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

<span data-ttu-id="62c4f-128">このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMIgration`指定された名前のクラスを派生 (*AddDataAnnotationsMig*)、および、`Up`メソッドを更新するコードを確認できますスキーマの制約。</span><span class="sxs-lookup"><span data-stu-id="62c4f-128">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class with the name specified (*AddDataAnnotationsMig*), and in the `Up` method you can see the code that updates the schema constraints.</span></span> <span data-ttu-id="62c4f-129">`Title`と`Genre`フィールドが null 許容では不要になった (つまり、値を入力する必要があります) および`Rating`フィールドが 5 の最大長。</span><span class="sxs-lookup"><span data-stu-id="62c4f-129">The `Title` and `Genre` fields are no longer nullable (that is, you must enter a value) and the `Rating` field has a maximum length of 5.</span></span>

<span data-ttu-id="62c4f-130">検証属性では、適用対象のモデル プロパティに適用する動作を指定します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-130">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="62c4f-131">`Required`属性は、プロパティは値である必要がありますを示します。 このサンプルでは、ムービーが値を持つことが、 `Title`、 `ReleaseDate`、 `Genre`、と`Price`有効にするにはプロパティです。</span><span class="sxs-lookup"><span data-stu-id="62c4f-131">The `Required` attribute indicates that a property must have a value; in this sample, a movie has to have values for the `Title`, `ReleaseDate`, `Genre`, and `Price` properties in order to be valid.</span></span> <span data-ttu-id="62c4f-132">`Range` 属性は、指定した範囲内に値を制限します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-132">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="62c4f-133">`StringLength` 属性では、文字列プロパティの最大長を設定でき、オプションとして最小長も設定できます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-133">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span> <span data-ttu-id="62c4f-134">組み込みの型 (など`decimal, int, float, DateTime`) が既定値を必要し、しない必要があります、`Required`属性。</span><span class="sxs-lookup"><span data-stu-id="62c4f-134">Intrinsic types (such as `decimal, int, float, DateTime`) are required by default and don't need the `Required` attribute.</span></span>

<span data-ttu-id="62c4f-135">コードでは、アプリケーションがデータベースに変更を保存する前に、モデル クラスを指定する検証規則が適用されている最初でいます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-135">Code First ensures that the validation rules you specify on a model class are enforced before the application saves changes in the database.</span></span> <span data-ttu-id="62c4f-136">次のコードが例外をスローするなど、ときに、`SaveChanges`メソッドは、いくつか必要なため`Movie`プロパティ値が不足していると、価格が (0 では、有効な範囲外です)。</span><span class="sxs-lookup"><span data-stu-id="62c4f-136">For example, the code below will throw an exception when the `SaveChanges` method is called, because several required `Movie` property values are missing and the price is zero (which is out of the valid range).</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

<span data-ttu-id="62c4f-137">検証するルールの .NET Framework によって自動的に適用できるように、アプリケーションより堅牢なします。</span><span class="sxs-lookup"><span data-stu-id="62c4f-137">Having validation rules automatically enforced by the .NET Framework helps make your application more robust.</span></span> <span data-ttu-id="62c4f-138">また、ユーザーが何かを検証することを忘れてしまい、データベースに不適切なデータが誤って格納されることもなくなります。</span><span class="sxs-lookup"><span data-stu-id="62c4f-138">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

<span data-ttu-id="62c4f-139">ここでは、完全なコードが、更新されたリスト*Movie.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="62c4f-139">Here's a complete code listing for the updated *Movie.cs* file:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a><span data-ttu-id="62c4f-140">ASP.NET MVC では、UI の検証エラー</span><span class="sxs-lookup"><span data-stu-id="62c4f-140">Validation Error UI in ASP.NET MVC</span></span>

<span data-ttu-id="62c4f-141">アプリケーションを再実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="62c4f-141">Re-run the application and navigate to the */Movies* URL.</span></span>

<span data-ttu-id="62c4f-142">をクリックして、**新規作成**リンクを新しいムービーを追加します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-142">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="62c4f-143">いくつかの無効な値にフォームに記入し、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="62c4f-143">Fill out the form with some invalid values and then click the **Create** button.</span></span>

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="62c4f-145">コンマを使用するロケールを英語以外の jQuery 検証をサポートするために (&quot;、&quot;) する必要があります、小数点の*globalize.js*と特定の*cultures/globalize.cultures.js*ファイル (から[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`です。</span><span class="sxs-lookup"><span data-stu-id="62c4f-145">to support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="62c4f-146">次のコードを操作する Views\Movies\Edit.cshtml ファイルへの変更を示しています、 &quot;FR-FR&quot;カルチャ。</span><span class="sxs-lookup"><span data-stu-id="62c4f-146">The following code shows the modifications to the Views\Movies\Edit.cshtml file to work with the &quot;fr-FR&quot; culture:</span></span>


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

<span data-ttu-id="62c4f-147">フォームが自動的に使用方法赤い枠線の色を無効なデータが含まれ、それぞれの横にある適切な検証エラー メッセージが生成されるテキスト ボックスを強調表示することを確認します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-147">Notice how the form has automatically used a red border color to highlight the text boxes that contain invalid data and has emitted an appropriate validation error message next to each one.</span></span> <span data-ttu-id="62c4f-148">エラーは、(JavaScript と jQuery を使用している) クライアント側とサーバー側 (ユーザーが JavaScript を無効にしている場合) の両方に適用されます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-148">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="62c4f-149">実際の利点でのコードの 1 つの行を変更する必要はありませんでした、`MoviesController`クラスまたは、 *Create.cshtml* UI この検証を有効にするために表示します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-149">A real benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="62c4f-150">このチュートリアルで前に作成したコントローラーとビューにより、`Movie` モデル クラスのプロパティで検証属性を使って指定した検証規則が自動的に取得されます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-150">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified by using validation attributes on the properties of the `Movie` model class.</span></span>

<span data-ttu-id="62c4f-151">プロパティ用お気付き`Title`と`Genre`、フォームを送信するまでに必要な属性が適用されません (ヒット、**作成**ボタンをクリック)、または入力フィールドにテキストを入力し、それを削除します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-151">You might have noticed for the properties `Title` and `Genre`, the required attribute is not enforced until you submit the form (hit the **Create** button), or enter text into the input field and removed it.</span></span> <span data-ttu-id="62c4f-152">フィールドを最初に空 (作成ビューのフィールド) などで必須の属性のみと、他の検証属性を持つ検証をトリガーするには、次を実行できます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-152">For a field which is initially empty (such as the fields on the Create view) and which has only the required attribute and no other validation attributes, you can do the following to trigger validation:</span></span>

1. <span data-ttu-id="62c4f-153">タブのフィールドにします。</span><span class="sxs-lookup"><span data-stu-id="62c4f-153">Tab into the field.</span></span>
2. <span data-ttu-id="62c4f-154">いくつかのテキストを入力します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-154">Enter some text.</span></span>
3. <span data-ttu-id="62c4f-155">タブを終了します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-155">Tab out.</span></span>
4. <span data-ttu-id="62c4f-156">フィールドに戻す タブします。</span><span class="sxs-lookup"><span data-stu-id="62c4f-156">Tab back into the field.</span></span>
5. <span data-ttu-id="62c4f-157">テキストを削除します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-157">Remove the text.</span></span>
6. <span data-ttu-id="62c4f-158">タブを終了します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-158">Tab out.</span></span>

<span data-ttu-id="62c4f-159">上の順序は、[送信] ボタンを押すことがなく必要な検証をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="62c4f-159">The above sequence will trigger the required validation without hitting the submit button.</span></span> <span data-ttu-id="62c4f-160">単に任意のフィールドを入力することがなく送信 ボタンを押すと、クライアント側の検証がトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-160">Simply hitting the submit button without entering any of the fields will trigger client side validation.</span></span> <span data-ttu-id="62c4f-161">クライアント側の検証エラーがなくなるまで、フォーム データはサーバーに送信されません。</span><span class="sxs-lookup"><span data-stu-id="62c4f-161">The form data is not sent to the server until there are no client side validation errors.</span></span> <span data-ttu-id="62c4f-162">これをテストするには、HTTP Post メソッドにブレークポイントを配置するかを使用して、 [fiddler ツール](http://fiddler2.com/fiddler2/)または IE 9 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)です。</span><span class="sxs-lookup"><span data-stu-id="62c4f-162">You can test this by putting a break point in the HTTP Post method or using the [fiddler tool](http://fiddler2.com/fiddler2/) or the IE 9 [F12 developer tools](https://msdn.microsoft.com/ie/aa740478).</span></span>

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a><span data-ttu-id="62c4f-163">作成の表示し、アクション メソッドを作成で発生する検証方法</span><span class="sxs-lookup"><span data-stu-id="62c4f-163">How Validation Occurs in the Create View and Create Action Method</span></span>

<span data-ttu-id="62c4f-164">コントローラーまたはビューのコードを更新しなくても検証 UI が生成する仕組みが気になるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="62c4f-164">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="62c4f-165">[次へ] の一覧が動作を示しています、`Create`内のメソッド、`MovieController`ようなクラスです。</span><span class="sxs-lookup"><span data-stu-id="62c4f-165">The next listing shows what the `Create` methods in the `MovieController` class look like.</span></span> <span data-ttu-id="62c4f-166">このチュートリアルで先ほどの作成から変更されていません。</span><span class="sxs-lookup"><span data-stu-id="62c4f-166">They're unchanged from how you created them earlier in this tutorial.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

<span data-ttu-id="62c4f-167">最初の (HTTP GET の) `Create` アクション メソッドは、初期の作成フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-167">The first (HTTP GET) `Create` action method displays the initial Create form.</span></span> <span data-ttu-id="62c4f-168">2 番目の (`[HttpPost]`) バージョンは、フォームの送信を処理します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-168">The second (`[HttpPost]`) version handles the form post.</span></span> <span data-ttu-id="62c4f-169">2 番目の `Create` メソッド (`HttpPost` バージョン) は、`ModelState.IsValid` を呼び出してムービーに検証エラーがあるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-169">The second `Create` method (The `HttpPost` version) calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="62c4f-170">このメソッドを呼び出すと、オブジェクトに適用されているすべての検証属性が評価されます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-170">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="62c4f-171">オブジェクトに検証エラーがある場合、`Create` メソッドはフォームを再表示します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-171">If the object has validation errors, the `Create` method re-displays the form.</span></span> <span data-ttu-id="62c4f-172">エラーがない場合、メソッドはデータベースに新しいムービーを保存します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-172">If there are no errors, the method saves the new movie in the database.</span></span> <span data-ttu-id="62c4f-173">使用して、このムービーの例では**サーバーには、クライアント側で検出された検証エラーがある場合に、フォームは通知されません、2 つ目** `Create`**メソッドが呼び出されない**です。</span><span class="sxs-lookup"><span data-stu-id="62c4f-173">In our movie example we are using, **the form is not posted to the server when their are validation errors detected on the client side; the second** `Create`**method is never called**.</span></span> <span data-ttu-id="62c4f-174">クライアントの検証が無効になっている場合は、ブラウザーで JavaScript が無効にして HTTP POST`Create`メソッド呼び出し`ModelState.IsValid`をムービーに検証エラーがあるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-174">If you disable JavaScript in your browser, client validation is disabled and the HTTP POST `Create` method calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span>

<span data-ttu-id="62c4f-175">`HttpPost Create` メソッドにブレークポイントを設定し、メソッドが呼び出されないことを確認できます。検証エラーが検出された場合、クライアント側の検証はフォームのデータを送信しません。</span><span class="sxs-lookup"><span data-stu-id="62c4f-175">You can set a break point in the `HttpPost Create` method and verify the method is never called, client side validation will not submit the form data when validation errors are detected.</span></span> <span data-ttu-id="62c4f-176">ブラウザーで JavaScript を無効にすると、エラーのあるフォームが送信され、ブレークポイントがヒットします。</span><span class="sxs-lookup"><span data-stu-id="62c4f-176">If you disable JavaScript in your browser, then submit the form with errors, the break point will be hit.</span></span> <span data-ttu-id="62c4f-177">JavaScript がなくても完全な検証が行われます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-177">You still get full validation without JavaScript.</span></span> <span data-ttu-id="62c4f-178">次の図は、Internet Explorer で JavaScript を無効にする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="62c4f-178">The following image shows how to disable JavaScript in Internet Explorer.</span></span>

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

<span data-ttu-id="62c4f-179">次の図では、FireFox ブラウザーで JavaScript を無効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-179">The following image shows how to disable JavaScript in the FireFox browser.</span></span>

![](adding-validation-to-the-model/_static/image5.png)

<span data-ttu-id="62c4f-180">次の図は、Chrome ブラウザーで JavaScript を無効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-180">The following image shows how to disable JavaScript with the Chrome browser.</span></span>

![](adding-validation-to-the-model/_static/image6.png)

<span data-ttu-id="62c4f-181">以下は、 *Create.cshtml*チュートリアルで先ほどスキャフォールディングされたビュー テンプレート。</span><span class="sxs-lookup"><span data-stu-id="62c4f-181">Below is the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="62c4f-182">これは、前に示した両方のアクション メソッドで、初期フォームの表示と、エラー発生時のフォームの再表示に使われます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-182">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

<span data-ttu-id="62c4f-183">コードの使用方法に注意してください、`Html.EditorFor`を出力するヘルパー、`<input>`要素ごと`Movie`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="62c4f-183">Notice how the code uses an `Html.EditorFor` helper to output the `<input>` element for each `Movie` property.</span></span> <span data-ttu-id="62c4f-184">呼び出しは、このヘルパーの横にある、`Html.ValidationMessageFor`ヘルパー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="62c4f-184">Next to this helper is a call to the `Html.ValidationMessageFor` helper method.</span></span> <span data-ttu-id="62c4f-185">これら 2 つのヘルパー メソッドがコント ローラーによって、ビューに渡されるモデル オブジェクトを操作 (ここで、`Movie`オブジェクト)。</span><span class="sxs-lookup"><span data-stu-id="62c4f-185">These two helper methods work with the model object that's passed by the controller to the view (in this case, a `Movie` object).</span></span> <span data-ttu-id="62c4f-186">これらは、自動的に、モデルと表示のエラー メッセージを適切に指定された検証属性を探します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-186">They automatically look for validation attributes specified on the model and display error messages as appropriate.</span></span>

<span data-ttu-id="62c4f-187">このアプローチのすばらしい点は、コント ローラーもビュー テンプレートの作成を知っているものが適用されている実際の検証規則の情報や、特定のエラー メッセージが表示されてです。</span><span class="sxs-lookup"><span data-stu-id="62c4f-187">What's really nice about this approach is that neither the controller nor the Create view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="62c4f-188">検証規則とエラー文字列は、`Movie` クラスでのみ指定されています。</span><span class="sxs-lookup"><span data-stu-id="62c4f-188">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="62c4f-189">これらの同じ検証規則は、編集ビュー、およびその他のビュー テンプレートを作成、モデルを編集するに自動的に適用されます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-189">These same validation rules are automatically applied to the Edit view and any other views templates you might create that edit your model.</span></span>

<span data-ttu-id="62c4f-190">検証ロジックを後で変更する場合は、これを行う正確に 1 か所でモデルの検証属性を追加することで (この例では、`movie`クラス)。</span><span class="sxs-lookup"><span data-stu-id="62c4f-190">If you want to change the validation logic later, you can do so in exactly one place by adding validation attributes to the model (in this example, the `movie` class).</span></span> <span data-ttu-id="62c4f-191">アプリケーションの異なる部分で規則の適用方法が一貫しない可能性を心配する必要はありません。すべての検証ロジックは 1 か所で定義され、すべての場所で使われます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-191">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="62c4f-192">これにより、コードの簡潔さが保たれ、簡単に維持や更新できます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-192">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="62c4f-193">また、これは DRY 原則に完全に従うことを意味します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-193">And it means that you'll be fully honoring the DRY principle.</span></span>

## <a name="adding-formatting-to-the-movie-model"></a><span data-ttu-id="62c4f-194">書式設定、ムービーのモデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-194">Adding Formatting to the Movie Model</span></span>

<span data-ttu-id="62c4f-195">*Movie.cs* ファイルを開き、`Movie` クラスを調べます。</span><span class="sxs-lookup"><span data-stu-id="62c4f-195">Open the *Movie.cs* file and examine the `Movie` class.</span></span> <span data-ttu-id="62c4f-196">[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間が一連の組み込みの検証属性だけでなく書式属性を提供します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-196">The [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="62c4f-197">既に適用された、 [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)リリース日と価格のフィールドの列挙値。</span><span class="sxs-lookup"><span data-stu-id="62c4f-197">We've already applied a [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="62c4f-198">次のコードは、`ReleaseDate`と`Price`と適切なプロパティ[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="62c4f-198">The following code shows the `ReleaseDate` and `Price` properties with the appropriate [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

<span data-ttu-id="62c4f-199">[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性が検証属性ではありません、HTML をレンダリングする方法をビュー エンジンに通知するために使用します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-199">The [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributes are not validation attributes, they are used to tell the view engine how to render the HTML.</span></span> <span data-ttu-id="62c4f-200">上記の例では、`DataType.Date`属性は、時刻のない日にのみ、としてムービー日付を表示します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-200">In the example above, the `DataType.Date` attribute displays the movie dates as dates only, without time.</span></span> <span data-ttu-id="62c4f-201">たとえば、次[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性は、データの形式を検証しません。</span><span class="sxs-lookup"><span data-stu-id="62c4f-201">For example, the following [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributes don't validate the format of the data:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

<span data-ttu-id="62c4f-202">上に示した属性は、データを書式設定、ビュー エンジンのヒントを提供するだけ (などの属性を提供および&lt;、&gt;の URL をおよび&lt;、href =&quot;mailto:EmailAddress.com&quot; &gt;電子メール アドレス。</span><span class="sxs-lookup"><span data-stu-id="62c4f-202">The attributes listed above only provide hints for the view engine to format the data (and supply attributes such as &lt;a&gt; for URL's and &lt;a href=&quot;mailto:EmailAddress.com&quot;&gt; for email.</span></span> <span data-ttu-id="62c4f-203">使用することができます、[正規表現](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)データの形式を検証する属性。</span><span class="sxs-lookup"><span data-stu-id="62c4f-203">You can use the [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attribute to validate the format of the data.</span></span>

<span data-ttu-id="62c4f-204">その他の方法を使用する、`DataType`属性が明示的に設定する、 [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)値。</span><span class="sxs-lookup"><span data-stu-id="62c4f-204">An alternative approach to using the `DataType` attributes, you could explicitly set a [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) value.</span></span> <span data-ttu-id="62c4f-205">次のコードは、日付の書式設定文字列のリリース日付プロパティを示しています (つまり、 &quot;d&quot;)。</span><span class="sxs-lookup"><span data-stu-id="62c4f-205">The following code shows the release date property with a date format string (namely, &quot;d&quot;).</span></span> <span data-ttu-id="62c4f-206">リリース日の一部として、時間にするないを指定するのにには、これを使用します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-206">You'd use this to specify that you don't want to time as part of the release date.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

<span data-ttu-id="62c4f-207">完全な`Movie`クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-207">The complete `Movie` class is shown below.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

<span data-ttu-id="62c4f-208">アプリケーションを実行しを参照、`Movies`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="62c4f-208">Run the application and browse to the `Movies` controller.</span></span> <span data-ttu-id="62c4f-209">リリース日と価格の形式が適切にします。</span><span class="sxs-lookup"><span data-stu-id="62c4f-209">The release date and price are nicely formatted.</span></span> <span data-ttu-id="62c4f-210">次の図は、リリース日とを使用して価格を示しています。 &quot;FR-FR&quot; 、カルチャとします。</span><span class="sxs-lookup"><span data-stu-id="62c4f-210">The image below shows the release date and price using &quot;fr-FR&quot; as the culture.</span></span>

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

<span data-ttu-id="62c4f-212">次の図は、既定のカルチャ (英語) に表示される同じデータを示します。</span><span class="sxs-lookup"><span data-stu-id="62c4f-212">The image below shows the same data displayed with the default culture (English US).</span></span>

![](adding-validation-to-the-model/_static/image8.png)

<span data-ttu-id="62c4f-213">このシリーズの次のパートでは、アプリケーションを確認し、自動的に生成される `Details` および `Delete` メソッドに対していくつかの改良を行います。</span><span class="sxs-lookup"><span data-stu-id="62c4f-213">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="62c4f-214">[前へ](adding-a-new-field-to-the-movie-model-and-table.md)
> [次へ](examining-the-details-and-delete-methods.md)</span><span class="sxs-lookup"><span data-stu-id="62c4f-214">[Previous](adding-a-new-field-to-the-movie-model-and-table.md)
[Next](examining-the-details-and-delete-methods.md)</span></span>
