---
title: "ASP.NET MVC を持つコアを EF コア CRUD - 2 10"
author: tdykstra
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/crud
ms.openlocfilehash: a7e0d4ff3d57e42dd7e33ffb5f26f2143520be87
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a><span data-ttu-id="3238a-102">作成、読み取り、更新、および削除の ASP.NET Core MVC のチュートリアル (10 の 2) と EF コア</span><span class="sxs-lookup"><span data-stu-id="3238a-102">Create, Read, Update, and Delete - EF Core with ASP.NET Core MVC tutorial (2 of 10)</span></span>

<span data-ttu-id="3238a-103">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3238a-103">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3238a-104">Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3238a-104">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="3238a-105">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="3238a-105">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="3238a-106">前のチュートリアルを格納し、Entity Framework と SQL Server LocalDB を使用してデータを表示する MVC アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3238a-106">In the previous tutorial, you created an MVC application that stores and displays data using the Entity Framework and SQL Server LocalDB.</span></span> <span data-ttu-id="3238a-107">このチュートリアルでを確認およびカスタマイズしたりする、CRUD (作成、読み取り、更新、削除) MVC のスキャフォールディング自動的に作成するコント ローラーとビューのコード。</span><span class="sxs-lookup"><span data-stu-id="3238a-107">In this tutorial, you'll review and customize the CRUD (create, read, update, delete) code that the MVC scaffolding automatically creates for you in controllers and views.</span></span>

> [!NOTE] 
> <span data-ttu-id="3238a-108">コント ローラーと、データ アクセス層の間で抽象化レイヤーを作成するのには、リポジトリ パターンを実装する一般的なことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3238a-108">It's a common practice to implement the repository pattern in order to create an abstraction layer between your controller and the data access layer.</span></span> <span data-ttu-id="3238a-109">単純なおよび教育自体 Entity Framework を使用する方法に焦点を当ててをこれらのチュートリアルを維持するには、リポジトリを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="3238a-109">To keep these tutorials simple and focused on teaching how to use the Entity Framework itself, they don't use repositories.</span></span> <span data-ttu-id="3238a-110">EF のリポジトリについては、次を参照してください。[このシリーズの前回のチュートリアル](advanced.md)です。</span><span class="sxs-lookup"><span data-stu-id="3238a-110">For information about repositories with EF, see [the last tutorial in this series](advanced.md).</span></span>

<span data-ttu-id="3238a-111">このチュートリアルでは、次の web ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="3238a-111">In this tutorial, you'll work with the following web pages:</span></span>

![学生の詳細 ページ](crud/_static/student-details.png)

![学生の作成 ページ](crud/_static/student-create.png)

![学生の編集 ページ](crud/_static/student-edit.png)

![学生の削除 ページ](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a><span data-ttu-id="3238a-116">[詳細] ページをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="3238a-116">Customize the Details page</span></span>

<span data-ttu-id="3238a-117">スキャフォールディング コード受講者インデックス ページを省略して、`Enrollments`プロパティ、そのプロパティには、コレクションが格納されているためです。</span><span class="sxs-lookup"><span data-stu-id="3238a-117">The scaffolded code for the Students Index page left out the `Enrollments` property, because that property holds a collection.</span></span> <span data-ttu-id="3238a-118">**詳細** ページで、HTML テーブルのコレクションの内容を表示します。</span><span class="sxs-lookup"><span data-stu-id="3238a-118">In the **Details** page, you'll display the contents of the collection in an HTML table.</span></span>

<span data-ttu-id="3238a-119">*Controllers/StudentsController.cs*、詳細については、アクション メソッドの表示は、 `SingleOrDefaultAsync` 、1 つを取得する方法を`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="3238a-119">In *Controllers/StudentsController.cs*, the action method for the Details view uses the `SingleOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="3238a-120">呼び出すコードを追加`Include`です。</span><span class="sxs-lookup"><span data-stu-id="3238a-120">Add code that calls `Include`.</span></span> <span data-ttu-id="3238a-121">`ThenInclude`、および`AsNoTracking`メソッドは、次の強調表示されたコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="3238a-121">`ThenInclude`,  and `AsNoTracking` methods, as shown in the following highlighted code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="3238a-122">`Include`と`ThenInclude`メソッドで発生する読み込みコンテキスト、`Student.Enrollments`ナビゲーション プロパティ、および各登録内で、`Enrollment.Course`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="3238a-122">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span>  <span data-ttu-id="3238a-123">これらのメソッドの詳細を学習、[関連データを読み取る](read-related-data.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="3238a-123">You'll learn more about these methods in the [reading related data](read-related-data.md) tutorial.</span></span>

<span data-ttu-id="3238a-124">`AsNoTracking`メソッドで返されるエンティティを現在のコンテキストの有効期間中に更新されませんのシナリオでパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="3238a-124">The `AsNoTracking` method improves performance in scenarios where the entities returned won't be updated in the current context's lifetime.</span></span> <span data-ttu-id="3238a-125">詳しくは`AsNoTracking`このチュートリアルの最後。</span><span class="sxs-lookup"><span data-stu-id="3238a-125">You'll learn more about `AsNoTracking` at the end of this tutorial.</span></span>

### <a name="route-data"></a><span data-ttu-id="3238a-126">ルート データ</span><span class="sxs-lookup"><span data-stu-id="3238a-126">Route data</span></span>

<span data-ttu-id="3238a-127">渡されるキーの値、`Details`メソッドに由来*データ ルーティング*です。</span><span class="sxs-lookup"><span data-stu-id="3238a-127">The key value that's passed to the `Details` method comes from *route data*.</span></span> <span data-ttu-id="3238a-128">ルート データは、URL のセグメント内のモデル バインダーにあるデータです。</span><span class="sxs-lookup"><span data-stu-id="3238a-128">Route data is data that the model binder found in a segment of the URL.</span></span> <span data-ttu-id="3238a-129">たとえば、既定のルートは、コント ローラー、アクション、および id のセグメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="3238a-129">For example, the default route specifies controller, action, and id segments:</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

<span data-ttu-id="3238a-130">次の URL で既定のルートは、コント ローラー、アクションとしてインデックスと、id として 1 とインストラクターをマップします。これらは、ルート データの値です。</span><span class="sxs-lookup"><span data-stu-id="3238a-130">In the following URL, the default route maps Instructor as the controller, Index as the action, and 1 as the id; these are route data values.</span></span>

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

<span data-ttu-id="3238a-131">URL の最後の部分 ("? courseID 2021 を =") のクエリ文字列値です。</span><span class="sxs-lookup"><span data-stu-id="3238a-131">The last part of the URL ("?courseID=2021") is a query string value.</span></span> <span data-ttu-id="3238a-132">モデル バインダーは、ID 値を渡すことも、`Details`メソッド`id`パラメーター クエリ文字列の値として渡す場合。</span><span class="sxs-lookup"><span data-stu-id="3238a-132">The model binder will also pass the ID value to the `Details` method `id` parameter if you pass it as a query string value:</span></span>

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

<span data-ttu-id="3238a-133">インデックス ページにハイパーリンクの Url は、Razor ビューでタグ ヘルパーのステートメントによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-133">In the Index page, hyperlink URLs are created by tag helper statements in the Razor view.</span></span> <span data-ttu-id="3238a-134">次の Razor コードで、`id`ためパラメーターに既定のルートと一致する`id`ルート データを追加します。</span><span class="sxs-lookup"><span data-stu-id="3238a-134">In the following Razor code, the `id` parameter matches the default route, so `id` is added to the route data.</span></span>

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

<span data-ttu-id="3238a-135">これにより、次の HTML が生成されます。 ときに`item.ID`は 6。</span><span class="sxs-lookup"><span data-stu-id="3238a-135">This generates the following HTML when `item.ID` is 6:</span></span>

```html
<a href="/Students/Edit/6">Edit</a>
```

<span data-ttu-id="3238a-136">次の Razor コードで`studentID`クエリ文字列として追加されたために、既定のルートのパラメーターと一致しません。</span><span class="sxs-lookup"><span data-stu-id="3238a-136">In the following Razor code, `studentID` doesn't match a parameter in the default route, so it's added as a query string.</span></span>

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

<span data-ttu-id="3238a-137">これにより、次の HTML が生成されます。 ときに`item.ID`は 6。</span><span class="sxs-lookup"><span data-stu-id="3238a-137">This generates the following HTML when `item.ID` is 6:</span></span>

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

<span data-ttu-id="3238a-138">タグ ヘルパーの詳細については、次を参照してください。 [ASP.NET Core でタグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="3238a-138">For more information about tag helpers, see [Tag helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="add-enrollments-to-the-details-view"></a><span data-ttu-id="3238a-139">詳細ビューに登録を追加します。</span><span class="sxs-lookup"><span data-stu-id="3238a-139">Add enrollments to the Details view</span></span>

<span data-ttu-id="3238a-140">開いている*Views/Students/Details.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="3238a-140">Open *Views/Students/Details.cshtml*.</span></span> <span data-ttu-id="3238a-141">使用して、各フィールドが表示される`DisplayNameFor`と`DisplayFor`ヘルパーを次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="3238a-141">Each field is displayed using `DisplayNameFor` and `DisplayFor` helpers, as shown in the following example:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

<span data-ttu-id="3238a-142">最後のフィールド後と終わりの前にすぐに`</dl>`タグは、「登録」一覧を表示する次コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="3238a-142">After the last field and immediately before the closing `</dl>` tag, add the following code to display a list of enrollments:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

<span data-ttu-id="3238a-143">コードを貼り付けた後にコードのインデントが正しくない場合は、修正して CTRL-D-K キーを押します。</span><span class="sxs-lookup"><span data-stu-id="3238a-143">If code indentation is wrong after you paste the code, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="3238a-144">このコードは、内のエンティティをループ処理、`Enrollments`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="3238a-144">This code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="3238a-145">各登録の場合は、コースのタイトルとグレードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-145">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="3238a-146">格納されているコース エンティティからコース タイトルが取得される、`Course`登録エンティティのナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="3238a-146">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="3238a-147">アプリを実行する、選択、**受講者**タブをクリックし、をクリックして、**詳細**受講者をリンクします。</span><span class="sxs-lookup"><span data-stu-id="3238a-147">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="3238a-148">選択した受講者コースとグレードの一覧を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3238a-148">You see the list of courses and grades for the selected student:</span></span>

![学生の詳細 ページ](crud/_static/student-details.png)

## <a name="update-the-create-page"></a><span data-ttu-id="3238a-150">更新プログラムの作成 ページ</span><span class="sxs-lookup"><span data-stu-id="3238a-150">Update the Create page</span></span>

<span data-ttu-id="3238a-151">*StudentsController.cs*、変更、HttpPost `Create` 、try-catch ブロックを追加してからの ID を削除することによって、`Bind`属性。</span><span class="sxs-lookup"><span data-stu-id="3238a-151">In *StudentsController.cs*, modify the HttpPost `Create` method by adding a try-catch block and removing ID from the `Bind` attribute.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

<span data-ttu-id="3238a-152">このコードは、セットされ、データベースに変更を保存、受講者エンティティに ASP.NET MVC モデル バインダーによって作成された学生エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="3238a-152">This code adds the Student entity created by the ASP.NET MVC model binder to the Students entity set and then saves the changes to the database.</span></span> <span data-ttu-id="3238a-153">(モデル バインダーのフォームから送信されたデータを操作するが容易 ASP.NET MVC 機能を指す。 モデル バインダーは、CLR 型にポストされたフォーム値を変換し、パラメーター内のアクション メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="3238a-153">(Model binder refers to the ASP.NET MVC functionality that makes it easier for you to work with data submitted by a form; a model binder converts posted form values to CLR types and passes them to the action method in parameters.</span></span> <span data-ttu-id="3238a-154">ここでは、モデル バインダーをインスタンス化学生エンティティのフォーム コレクションからプロパティ値を使用している。)</span><span class="sxs-lookup"><span data-stu-id="3238a-154">In this case, the model binder instantiates a Student entity for you using property values from the Form collection.)</span></span>

<span data-ttu-id="3238a-155">削除する`ID`から、`Bind`属性の ID が SQL Server は、行が挿入されると自動的に設定されますが、主キーの値であるためです。</span><span class="sxs-lookup"><span data-stu-id="3238a-155">You removed `ID` from the `Bind` attribute because ID is the primary key value which SQL Server will set automatically when the row is inserted.</span></span> <span data-ttu-id="3238a-156">ユーザーからの入力には、ID 値を設定しません。</span><span class="sxs-lookup"><span data-stu-id="3238a-156">Input from the user doesn't set the ID value.</span></span>

<span data-ttu-id="3238a-157">以外の場合、`Bind`属性、try-catch ブロックがスキャフォールディング コードに加えたのみ変更します。</span><span class="sxs-lookup"><span data-stu-id="3238a-157">Other than the `Bind` attribute, the try-catch block is the only change you've made to the scaffolded code.</span></span> <span data-ttu-id="3238a-158">派生した例外`DbUpdateException`は、変更の保存中にキャッチされ、汎用的なエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-158">If an exception that derives from `DbUpdateException` is caught while the changes are being saved, a generic error message is displayed.</span></span> <span data-ttu-id="3238a-159">`DbUpdateException`例外は場合もありますが原因でプログラミング エラーではなく、アプリケーションを外部の何かため、ユーザーが再試行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3238a-159">`DbUpdateException` exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again.</span></span> <span data-ttu-id="3238a-160">このサンプルでは実装されていませんが実稼働品質アプリケーションは、例外を記録します。</span><span class="sxs-lookup"><span data-stu-id="3238a-160">Although not implemented in this sample, a production quality application would log the exception.</span></span> <span data-ttu-id="3238a-161">詳細については、次を参照してください。、**について理解を深めるログ**」の「[監視と遠隔測定 (実際のクラウド アプリのビルドと Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)です。</span><span class="sxs-lookup"><span data-stu-id="3238a-161">For more information, see the **Log for insight** section in [Monitoring and Telemetry (Building Real-World Cloud Apps with Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).</span></span>

<span data-ttu-id="3238a-162">`ValidateAntiForgeryToken`属性、クロスサイト リクエスト フォージェリ (CSRF) 攻撃の防止に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="3238a-162">The `ValidateAntiForgeryToken` attribute helps prevent cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="3238a-163">トークンは自動的に挿入され別に表示、 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)フォームがユーザーによって送信されるときに含まれるとします。</span><span class="sxs-lookup"><span data-stu-id="3238a-163">The token is automatically injected into the view by the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) and is included when the form is submitted by the user.</span></span> <span data-ttu-id="3238a-164">トークンを検証、`ValidateAntiForgeryToken`属性。</span><span class="sxs-lookup"><span data-stu-id="3238a-164">The token is validated by the `ValidateAntiForgeryToken` attribute.</span></span> <span data-ttu-id="3238a-165">CSRF の詳細については、次を参照してください。[対策要求の偽造](../../security/anti-request-forgery.md)です。</span><span class="sxs-lookup"><span data-stu-id="3238a-165">For more information about CSRF, see [Anti-Request Forgery](../../security/anti-request-forgery.md).</span></span>

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a><span data-ttu-id="3238a-166">Overposting に関するセキュリティに関する注意</span><span class="sxs-lookup"><span data-stu-id="3238a-166">Security note about overposting</span></span>

<span data-ttu-id="3238a-167">`Bind`上のスキャフォールディング コードを含む属性、`Create`メソッドは overposting から保護する方法の 1 つのシナリオを作成します。</span><span class="sxs-lookup"><span data-stu-id="3238a-167">The `Bind` attribute that the scaffolded code includes on the `Create` method is one way to protect against overposting in create scenarios.</span></span> <span data-ttu-id="3238a-168">たとえば、受講者エンティティが含まれています、`Secret`プロパティを設定するには、この web ページをたくないです。</span><span class="sxs-lookup"><span data-stu-id="3238a-168">For example, suppose the Student entity includes a `Secret` property that you don't want this web page to set.</span></span>

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

<span data-ttu-id="3238a-169">存在しない場合でも、 `Secret` web ページで、ハッカー フィールドでした、Fiddler などのツールを使用してまたは投稿に、一部の JavaScript を書き込み、`Secret`値を作成します。</span><span class="sxs-lookup"><span data-stu-id="3238a-169">Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="3238a-170">なし、`Bind`学生のインスタンスでは、モデル バインダーの作成時にモデル バインダーが使用されるフィールドを制限する属性がピックアップされる`Secret`値の形式し、学生エンティティ インスタンスの作成に使用します。</span><span class="sxs-lookup"><span data-stu-id="3238a-170">Without the `Bind` attribute limiting the fields that the model binder uses when it creates a Student instance, the model binder would pick up that `Secret` form value and use it to create the Student entity instance.</span></span> <span data-ttu-id="3238a-171">値の指定されたハッカーし、`Secret`フォーム フィールドが、データベースで更新されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-171">Then whatever value the hacker specified for the `Secret` form field would be updated in your database.</span></span> <span data-ttu-id="3238a-172">次の図は、Fiddler ツールを追加する、`Secret`ポストされたフォーム値へのフィールド (に値"OverPost") です。</span><span class="sxs-lookup"><span data-stu-id="3238a-172">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler シークレット フィールドの追加](crud/_static/fiddler.png)

<span data-ttu-id="3238a-174">"OverPost"は、正常に追加する値、`Secret`プロパティの挿入された行を web ページが、そのプロパティを設定できることを意図したものことはありませんが。</span><span class="sxs-lookup"><span data-stu-id="3238a-174">The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to set that property.</span></span>

<span data-ttu-id="3238a-175">まず、データベースからエンティティを読み取り、呼び出すことで、編集のシナリオで overposting を防ぐことができます`TryUpdateModel`、明示的に許可されているプロパティ リストに渡します。</span><span class="sxs-lookup"><span data-stu-id="3238a-175">You can prevent overposting in edit scenarios by reading the entity from the database first and then calling `TryUpdateModel`, passing in an explicit allowed properties list.</span></span> <span data-ttu-id="3238a-176">これらのチュートリアルで使用する方法です。</span><span class="sxs-lookup"><span data-stu-id="3238a-176">That's the method used in these tutorials.</span></span>

<span data-ttu-id="3238a-177">多くの開発者に好ま overposting を防ぐために別の方法では、モデル バインディングでエンティティ クラスではなく、ビュー モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="3238a-177">An alternative way to prevent overposting that's preferred by many developers is to use view models rather than entity classes with model binding.</span></span> <span data-ttu-id="3238a-178">ビュー モデルで更新するプロパティのみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3238a-178">Include only the properties you want to update in the view model.</span></span> <span data-ttu-id="3238a-179">MVC モデル バインダーが終了すると、モデル プロパティの表示を必要に応じて AutoMapper などのツールを使用して、エンティティ インスタンスにコピーします。</span><span class="sxs-lookup"><span data-stu-id="3238a-179">Once the MVC model binder has finished, copy the view model properties to the entity instance, optionally using a tool such as AutoMapper.</span></span> <span data-ttu-id="3238a-180">使用して`_context.Entry`にその状態を設定するエンティティのインスタンスで`Unchanged`、し、設定`Property("PropertyName").IsModified`ビュー モデルに含まれる各エンティティ プロパティを true に設定します。</span><span class="sxs-lookup"><span data-stu-id="3238a-180">Use `_context.Entry` on the entity instance to set its state to `Unchanged`, and then set `Property("PropertyName").IsModified` to true on each entity property that's included in the view model.</span></span> <span data-ttu-id="3238a-181">このメソッドは両方のでは、編集し、シナリオを作成します。</span><span class="sxs-lookup"><span data-stu-id="3238a-181">This method works in both edit and create scenarios.</span></span>

### <a name="test-the-create-page"></a><span data-ttu-id="3238a-182">テストの作成 ページ</span><span class="sxs-lookup"><span data-stu-id="3238a-182">Test the Create page</span></span>

<span data-ttu-id="3238a-183">内のコード*Views/Students/Create.cshtml*を使用して`label`、 `input`、および`span`の検証メッセージ タグ ヘルパーの各フィールドにします。</span><span class="sxs-lookup"><span data-stu-id="3238a-183">The code in *Views/Students/Create.cshtml* uses `label`, `input`, and `span` (for validation messages) tag helpers for each field.</span></span>

<span data-ttu-id="3238a-184">アプリを実行する、選択、**受講者**タブをクリックし、をクリックして**新規作成**です。</span><span class="sxs-lookup"><span data-stu-id="3238a-184">Run the app, select the **Students** tab, and click **Create New**.</span></span>

<span data-ttu-id="3238a-185">名前と日付を入力します。</span><span class="sxs-lookup"><span data-stu-id="3238a-185">Enter names and a date.</span></span> <span data-ttu-id="3238a-186">お使いのブラウザーを実行できる場合は、無効な日付を入力してください。</span><span class="sxs-lookup"><span data-stu-id="3238a-186">Try entering an invalid date if your browser lets you do that.</span></span> <span data-ttu-id="3238a-187">(一部のブラウザーを強制する日付の選択を使用します。)をクリックして**作成**エラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="3238a-187">(Some browsers force you to use a date picker.) Then click **Create** to see the error message.</span></span>

![日付の妥当性確認エラー](crud/_static/date-error.png)

<span data-ttu-id="3238a-189">これは、既定では取得するサーバー側の検証後のチュートリアルもクライアント側の検証のコードを生成する属性を追加する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-189">This is server-side validation that you get by default; in a later tutorial you'll see how to add attributes that will generate code for client-side validation also.</span></span> <span data-ttu-id="3238a-190">次の強調表示されたコードで、モデルの検証チェックを示しています、`Create`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3238a-190">The following highlighted code shows the model validation check in the `Create` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

<span data-ttu-id="3238a-191">有効な値に日付を変更し、をクリックして**作成**に表示される新しい学生を表示する、**インデックス**ページ。</span><span class="sxs-lookup"><span data-stu-id="3238a-191">Change the date to a valid value and click **Create** to see the new student appear in the **Index** page.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="3238a-192">更新プログラムの編集 ページ</span><span class="sxs-lookup"><span data-stu-id="3238a-192">Update the Edit page</span></span>

<span data-ttu-id="3238a-193">*StudentController.cs*、HttpGet`Edit`メソッド (ことがなく 1 つ、`HttpPost`属性) を使用して、`SingleOrDefaultAsync`で学習したように、選択した学生エンティティを取得する方法を`Details`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3238a-193">In *StudentController.cs*, the HttpGet `Edit` method (the one without the `HttpPost` attribute) uses the `SingleOrDefaultAsync` method to retrieve the selected Student entity, as you saw in the `Details` method.</span></span> <span data-ttu-id="3238a-194">この方法を変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3238a-194">You don't need to change this method.</span></span>

### <a name="recommended-httppost-edit-code-read-and-update"></a><span data-ttu-id="3238a-195">HttpPost 編集コードをお勧めします読み取りと更新。</span><span class="sxs-lookup"><span data-stu-id="3238a-195">Recommended HttpPost Edit code: Read and update</span></span>

<span data-ttu-id="3238a-196">HttpPost 編集アクション メソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3238a-196">Replace the HttpPost Edit action method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

<span data-ttu-id="3238a-197">これらの変更は、overposting を防ぐためにセキュリティのベスト プラクティスを実装します。</span><span class="sxs-lookup"><span data-stu-id="3238a-197">These changes implement a security best practice to prevent overposting.</span></span> <span data-ttu-id="3238a-198">生成された scaffolder、`Bind`属性し、エンティティのセットにモデル バインダーによって作成されたエンティティを追加、`Modified`フラグ。</span><span class="sxs-lookup"><span data-stu-id="3238a-198">The scaffolder generated a `Bind` attribute and added the entity created by the model binder to the entity set with a `Modified` flag.</span></span> <span data-ttu-id="3238a-199">に、コードが多くのシナリオで推奨されていないこと、`Bind`属性に表示されていないフィールド内の既存のデータをクリアして、`Include`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="3238a-199">That code isn't recommended for many scenarios because the `Bind` attribute clears out any pre-existing data in fields not listed in the `Include` parameter.</span></span>

<span data-ttu-id="3238a-200">新しいコードを読み取り、既存のエンティティと呼び出し`TryUpdateModel`取得されたエンティティのフィールドを更新する[ポストされたフォーム データでのユーザー入力に基づいて](xref:mvc/models/model-binding#how-model-binding-works)です。</span><span class="sxs-lookup"><span data-stu-id="3238a-200">The new code reads the existing entity and calls `TryUpdateModel` to update fields in the retrieved entity [based on user input in the posted form data](xref:mvc/models/model-binding#how-model-binding-works).</span></span> <span data-ttu-id="3238a-201">Entity Framework の自動変更のセットを追跡、`Modified`フォーム入力によって変更されたフィールドのフラグ。</span><span class="sxs-lookup"><span data-stu-id="3238a-201">The Entity Framework's automatic change tracking sets the `Modified` flag on the fields that are changed by form input.</span></span> <span data-ttu-id="3238a-202">ときに、`SaveChanges`メソッドが呼び出されると、Entity Framework は、データベースの行を更新する SQL ステートメントを作成します。</span><span class="sxs-lookup"><span data-stu-id="3238a-202">When the `SaveChanges` method is called, the Entity Framework creates SQL statements to update the database row.</span></span> <span data-ttu-id="3238a-203">同時実行の競合は無視され、ユーザーによって更新されたテーブルの列のみが、データベースで更新されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-203">Concurrency conflicts are ignored, and only the table columns that were updated by the user are updated in the database.</span></span> <span data-ttu-id="3238a-204">(後のチュートリアルは、同時実行の競合を処理する方法を示しています)。</span><span class="sxs-lookup"><span data-stu-id="3238a-204">(A later tutorial shows how to handle concurrency conflicts.)</span></span>

<span data-ttu-id="3238a-205">過剰ポスティング、によって更新可能にフィールドを防ぐために、ベスト プラクティスとして、**編集**ページは、ホワイト リストに登録で、`TryUpdateModel`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="3238a-205">As a best practice to prevent overposting, the fields that you want to be updateable by the **Edit** page are whitelisted in the `TryUpdateModel` parameters.</span></span> <span data-ttu-id="3238a-206">(前に、パラメーター リストのフィールドの一覧は空の文字列はフォーム フィールド名を使用するプレフィックスです。)現在保護している、余分なフィールドはありませんが、後で、データ モデルにフィールドを追加する場合、自動的に保護されていることに明示的に追加するには、ここまでにバインドするモデル バインダーを使用するフィールドを一覧表示することを確認します。</span><span class="sxs-lookup"><span data-stu-id="3238a-206">(The empty string preceding the list of fields in the parameter list is for a prefix to use with the form fields names.) Currently there are no extra fields that you're protecting, but listing the fields that you want the model binder to bind ensures that if you add fields to the data model in the future, they're automatically protected until you explicitly add them here.</span></span>

<span data-ttu-id="3238a-207">HttpPost のメソッド シグネチャのこれらの変更の結果として`Edit`メソッドは、HttpGet と同じ`Edit`メソッドです。 したがって、メソッドを名前変更した`EditPost`です。</span><span class="sxs-lookup"><span data-stu-id="3238a-207">As a result of these changes, the method signature of the HttpPost `Edit` method is the same as the HttpGet `Edit` method; therefore you've renamed the method `EditPost`.</span></span>

### <a name="alternative-httppost-edit-code-create-and-attach"></a><span data-ttu-id="3238a-208">代替 HttpPost 編集コード: を作成し、アタッチ</span><span class="sxs-lookup"><span data-stu-id="3238a-208">Alternative HttpPost Edit code: Create and attach</span></span>

<span data-ttu-id="3238a-209">推奨される HttpPost 編集コードでは、変更された列のみが更新され、モデル バインディングに含まれる必要のないプロパティのデータを保持をによりします。</span><span class="sxs-lookup"><span data-stu-id="3238a-209">The recommended HttpPost edit code ensures that only changed columns get updated and preserves data in properties that you don't want included for model binding.</span></span> <span data-ttu-id="3238a-210">ただし、読み取り優先のアプローチには、追加のデータベースの読み取り、および同時実行の競合を処理するためのより複雑なコードで発生することができますが必要です。</span><span class="sxs-lookup"><span data-stu-id="3238a-210">However, the read-first approach requires an extra database read, and can result in more complex code for handling concurrency conflicts.</span></span> <span data-ttu-id="3238a-211">代わりには、EF コンテキストにモデル バインダーによって作成されたエンティティをアタッチし、修正としてマークするにです。</span><span class="sxs-lookup"><span data-stu-id="3238a-211">An alternative is to attach an entity created by the model binder to the EF context and mark it as modified.</span></span> <span data-ttu-id="3238a-212">(プロジェクトを更新しない、次のコードでは、それがのみ表示を省略可能な方法を示しています)。</span><span class="sxs-lookup"><span data-stu-id="3238a-212">(Don't update your project with this code, it's only shown to illustrate an optional approach.)</span></span> 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

<span data-ttu-id="3238a-213">Web ページの UI は、エンティティのすべてのフィールドが含まれています、それらを更新できます、このアプローチを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3238a-213">You can use this approach when the web page UI includes all of the fields in the entity and can update any of them.</span></span>

<span data-ttu-id="3238a-214">スキャフォールディングのコードが作成およびアタッチによる方法を使用しますが、のみキャッチ`DbUpdateConcurrencyException`404 エラー コードを例外とを返します。</span><span class="sxs-lookup"><span data-stu-id="3238a-214">The scaffolded code uses the create-and-attach approach but only catches `DbUpdateConcurrencyException` exceptions and returns 404 error codes.</span></span>  <span data-ttu-id="3238a-215">例では、すべてデータベース更新の例外をキャッチし、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-215">The example shown catches any database update exception and displays an error message.</span></span>

### <a name="entity-states"></a><span data-ttu-id="3238a-216">エンティティの状態</span><span class="sxs-lookup"><span data-stu-id="3238a-216">Entity States</span></span>

<span data-ttu-id="3238a-217">メモリ内のエンティティは、データベース内の対応する行との同期と、この情報を呼び出すときの動作を決定するかどうかの追跡データベース コンテキスト、`SaveChanges`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3238a-217">The database context keeps track of whether entities in memory are in sync with their corresponding rows in the database, and this information determines what happens when you call the `SaveChanges` method.</span></span> <span data-ttu-id="3238a-218">新しいエンティティを渡す場合など、`Add`メソッドに設定されているエンティティの状態を`Added`です。</span><span class="sxs-lookup"><span data-stu-id="3238a-218">For example, when you pass a new entity to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="3238a-219">その後呼び出す、`SaveChanges`メソッド、データベース コンテキストは、SQL の INSERT コマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="3238a-219">Then when you call the `SaveChanges` method, the database context issues a SQL INSERT command.</span></span>

<span data-ttu-id="3238a-220">次の状態のいずれかでエンティティがあります。</span><span class="sxs-lookup"><span data-stu-id="3238a-220">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="3238a-221">`Added`。</span><span class="sxs-lookup"><span data-stu-id="3238a-221">`Added`.</span></span> <span data-ttu-id="3238a-222">エンティティは、データベースにまだ存在しません。</span><span class="sxs-lookup"><span data-stu-id="3238a-222">The entity doesn't yet exist in the database.</span></span> <span data-ttu-id="3238a-223">`SaveChanges`メソッドは、INSERT ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="3238a-223">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="3238a-224">`Unchanged`。</span><span class="sxs-lookup"><span data-stu-id="3238a-224">`Unchanged`.</span></span> <span data-ttu-id="3238a-225">何も行う必要がありますでこのエンティティと、`SaveChanges`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3238a-225">Nothing needs to be done with this entity by the `SaveChanges` method.</span></span> <span data-ttu-id="3238a-226">データベースからエンティティを読み取るときに、エンティティは、この状態を持つ開始します。</span><span class="sxs-lookup"><span data-stu-id="3238a-226">When you read an entity from the database, the entity starts out with this status.</span></span>

* <span data-ttu-id="3238a-227">`Modified`。</span><span class="sxs-lookup"><span data-stu-id="3238a-227">`Modified`.</span></span> <span data-ttu-id="3238a-228">一部またはすべてのエンティティのプロパティの値が変更されています。</span><span class="sxs-lookup"><span data-stu-id="3238a-228">Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="3238a-229">`SaveChanges`メソッドは、UPDATE ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="3238a-229">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="3238a-230">`Deleted`。</span><span class="sxs-lookup"><span data-stu-id="3238a-230">`Deleted`.</span></span> <span data-ttu-id="3238a-231">エンティティは、削除のマークされています。</span><span class="sxs-lookup"><span data-stu-id="3238a-231">The entity has been marked for deletion.</span></span> <span data-ttu-id="3238a-232">`SaveChanges`メソッドは、DELETE ステートメントを発行します。</span><span class="sxs-lookup"><span data-stu-id="3238a-232">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="3238a-233">`Detached`。</span><span class="sxs-lookup"><span data-stu-id="3238a-233">`Detached`.</span></span> <span data-ttu-id="3238a-234">エンティティは、データベース コンテキストによって追跡されているはありません。</span><span class="sxs-lookup"><span data-stu-id="3238a-234">The entity isn't being tracked by the database context.</span></span>

<span data-ttu-id="3238a-235">デスクトップ アプリケーションでは、通常状態の変更から自動的に設定します。</span><span class="sxs-lookup"><span data-stu-id="3238a-235">In a desktop application, state changes are typically set automatically.</span></span> <span data-ttu-id="3238a-236">エンティティを読み取るし、そのプロパティ値の一部を変更します。</span><span class="sxs-lookup"><span data-stu-id="3238a-236">You read an entity and make changes to some of its property values.</span></span> <span data-ttu-id="3238a-237">これにより、そのエンティティの状態に自動的に変更する`Modified`です。</span><span class="sxs-lookup"><span data-stu-id="3238a-237">This causes its entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="3238a-238">その後呼び出す`SaveChanges`、Entity Framework を変更して実際のプロパティのみを更新する SQL の UPDATE ステートメントが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-238">Then when you call `SaveChanges`, the Entity Framework generates a SQL UPDATE statement that updates only the actual properties that you changed.</span></span>

<span data-ttu-id="3238a-239">Web アプリで、`DbContext`エンティティと、ページが表示されたら、そのデータを編集することが破棄される表示を最初に読み込みます。</span><span class="sxs-lookup"><span data-stu-id="3238a-239">In a web app, the `DbContext` that initially reads an entity and displays its data to be edited is disposed after a page is rendered.</span></span> <span data-ttu-id="3238a-240">ときに、HttpPost`Edit`アクション メソッドが呼び出される、新しい web 要求が行われ、の新しいインスタンスを持つ、`DbContext`です。</span><span class="sxs-lookup"><span data-stu-id="3238a-240">When the HttpPost `Edit` action method is called,  a new web request is made and you have a new instance of the `DbContext`.</span></span> <span data-ttu-id="3238a-241">その新しいコンテキスト内のエンティティを再読み込みする場合は、デスクトップの処理をシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="3238a-241">If you re-read the entity in that new context, you simulate desktop processing.</span></span>

<span data-ttu-id="3238a-242">余分な読み取り操作を行うにはたくない場合は、モデル バインダーによって作成されたエンティティ オブジェクトを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3238a-242">But if you don't want to do the extra read operation, you have to use the entity object created by the model binder.</span></span>  <span data-ttu-id="3238a-243">これを行う最も簡単な方法では、前に示した代替 HttpPost 編集コードのように、modified、エンティティの状態を設定します。</span><span class="sxs-lookup"><span data-stu-id="3238a-243">The simplest way to do this is to set the entity state to Modified as is done in the alternative HttpPost Edit code shown earlier.</span></span> <span data-ttu-id="3238a-244">その後呼び出す`SaveChanges`、Entity Framework がコンテキストに変更するプロパティを特定する方法があるないために、データベースの行のすべての列を更新します。</span><span class="sxs-lookup"><span data-stu-id="3238a-244">Then when you call `SaveChanges`, the Entity Framework updates all columns of the database row, because the context has no way to know which properties you changed.</span></span>

<span data-ttu-id="3238a-245">読み取り-優先のアプローチを避けたいユーザーが実際に変更されたフィールドのみを更新する SQL UPDATE ステートメントをする場合、コードは複雑です。</span><span class="sxs-lookup"><span data-stu-id="3238a-245">If you want to avoid the read-first approach, but you also want the SQL UPDATE statement to update only the fields that the user actually changed, the code is more complex.</span></span> <span data-ttu-id="3238a-246">何らかの方法で元の値を保存する必要が (など、非表示フィールドを使用して) ことにより、使用可能な場合に、HttpPost`Edit`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-246">You have to save the original values in some way (such as by using hidden fields) so that they're available when the HttpPost `Edit` method is called.</span></span> <span data-ttu-id="3238a-247">呼び出し元の値を使用して学生エンティティを作成してから、`Attach`メソッドをその元のバージョン、エンティティのエンティティの値を新しい値に更新およびを呼び出す`SaveChanges`です。</span><span class="sxs-lookup"><span data-stu-id="3238a-247">Then you can create a Student entity using the original values, call the `Attach` method with that original version of the entity, update the entity's values to the new values, and then call `SaveChanges`.</span></span>

### <a name="test-the-edit-page"></a><span data-ttu-id="3238a-248">テストの編集 ページ</span><span class="sxs-lookup"><span data-stu-id="3238a-248">Test the Edit page</span></span>

<span data-ttu-id="3238a-249">アプリを実行する、選択、**受講者** タブのをクリックして、**編集**ハイパーリンクです。</span><span class="sxs-lookup"><span data-stu-id="3238a-249">Run the app, select the **Students** tab, then click an **Edit** hyperlink.</span></span>

![[編集] ページの受講者](crud/_static/student-edit.png)

<span data-ttu-id="3238a-251">クリックして、データの一部を変更**保存**です。</span><span class="sxs-lookup"><span data-stu-id="3238a-251">Change some of the data and click **Save**.</span></span> <span data-ttu-id="3238a-252">**インデックス**ページが開かれ、変更されたデータを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3238a-252">The **Index** page opens and you see the changed data.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="3238a-253">更新プログラムの削除 ページ</span><span class="sxs-lookup"><span data-stu-id="3238a-253">Update the Delete page</span></span>

<span data-ttu-id="3238a-254">*StudentController.cs*、テンプレート コード、HttpGet を`Delete`メソッドを使用、`SingleOrDefaultAsync`詳細ビューと編集方法に説明したとおりに、選択した学生エンティティを取得する方法です。</span><span class="sxs-lookup"><span data-stu-id="3238a-254">In *StudentController.cs*, the template code for the HttpGet `Delete` method uses the `SingleOrDefaultAsync` method to retrieve the selected Student entity, as you saw in the Details and Edit methods.</span></span> <span data-ttu-id="3238a-255">ただし、実装するカスタム エラー メッセージへの呼び出し`SaveChanges`失敗した場合、このメソッドとその対応するビューには、一部の機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="3238a-255">However, to implement a custom error message when the call to `SaveChanges` fails, you'll add some functionality to this method and its corresponding view.</span></span>

<span data-ttu-id="3238a-256">更新プログラムの操作を作成すると、削除操作は次の 2 つのアクション メソッドが必要です。</span><span class="sxs-lookup"><span data-stu-id="3238a-256">As you saw for update and create operations, delete operations require two action methods.</span></span> <span data-ttu-id="3238a-257">GET 要求に応答して呼び出されるメソッドは、ユーザーを承認または削除操作をキャンセルする機会を提供するビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="3238a-257">The method that's called in response to a GET request displays a view that gives the user a chance to approve or cancel the delete operation.</span></span> <span data-ttu-id="3238a-258">ユーザーを承認すると、POST 要求が作成されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-258">If the user approves it, a POST request is created.</span></span> <span data-ttu-id="3238a-259">このような場合、HttpPost`Delete`メソッドが呼び出され、そのメソッドが実際には、削除操作を実行し、します。</span><span class="sxs-lookup"><span data-stu-id="3238a-259">When that happens, the HttpPost `Delete` method is called and then that method actually performs the delete operation.</span></span>

<span data-ttu-id="3238a-260">場合、try-catch ブロックに追加、HttpPost`Delete`データベースが更新されたときに発生したエラーを処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="3238a-260">You'll add a try-catch block to the HttpPost `Delete` method to handle any errors that might occur when the database is updated.</span></span> <span data-ttu-id="3238a-261">エラーが発生する場合、HttpPost Delete メソッドは、エラーが発生したことを示すパラメーターを渡す、HttpGet Delete メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3238a-261">If an error occurs, the HttpPost Delete method calls the HttpGet Delete method, passing it a parameter that indicates that an error has occurred.</span></span> <span data-ttu-id="3238a-262">HttpGet Delete メソッドには、確認ページを取り消すか、もう一度実行する機会をユーザーに提供のエラー メッセージとし、再表示されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-262">The HttpGet Delete method then redisplays the confirmation page along with the error message, giving the user an opportunity to cancel or try again.</span></span>

<span data-ttu-id="3238a-263">HttpGet を置き換える`Delete`アクション メソッドを次のコードは、エラー報告を管理します。</span><span class="sxs-lookup"><span data-stu-id="3238a-263">Replace the HttpGet `Delete` action method with the following code, which manages error reporting.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

<span data-ttu-id="3238a-264">このコードは、変更を保存する障害の発生後、メソッドが呼び出されたかどうかを示す省略可能なパラメーターを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="3238a-264">This code accepts an optional parameter that indicates whether the method was called after a failure to save changes.</span></span> <span data-ttu-id="3238a-265">このパラメーターが false 場合に、HttpGet`Delete`以前失敗せずにメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-265">This parameter is false when the HttpGet `Delete` method is called without a previous failure.</span></span> <span data-ttu-id="3238a-266">これが呼び出されたとき、HttpPost によって`Delete`データベース更新エラーへの応答でメソッドをパラメーターが true であり、エラー メッセージは、ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-266">When it's called by the HttpPost `Delete` method in response to a database update error, the parameter is true and an error message is passed to the view.</span></span>

### <a name="the-read-first-approach-to-httppost-delete"></a><span data-ttu-id="3238a-267">HttpPost Delete に読み取り優先のアプローチ</span><span class="sxs-lookup"><span data-stu-id="3238a-267">The read-first approach to HttpPost Delete</span></span>

<span data-ttu-id="3238a-268">HttpPost を置き換える`Delete`アクション メソッド (名前付き`DeleteConfirmed`) を次のコードを実際の削除操作を実行し、データベース更新エラーをキャッチします。</span><span class="sxs-lookup"><span data-stu-id="3238a-268">Replace the HttpPost `Delete` action method (named `DeleteConfirmed`) with the following code, which performs the actual delete operation and catches any database update errors.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

<span data-ttu-id="3238a-269">このコードは、選択したエンティティを取得し、呼び出し、`Remove`エンティティの状態を設定するメソッドを`Deleted`です。</span><span class="sxs-lookup"><span data-stu-id="3238a-269">This code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="3238a-270">ときに`SaveChanges`が呼び出されると、SQL の DELETE コマンドを生成します。</span><span class="sxs-lookup"><span data-stu-id="3238a-270">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span>

### <a name="the-create-and-attach-approach-to-httppost-delete"></a><span data-ttu-id="3238a-271">HttpPost の削除 を作成およびアタッチによる方法</span><span class="sxs-lookup"><span data-stu-id="3238a-271">The create-and-attach approach to HttpPost Delete</span></span>

<span data-ttu-id="3238a-272">大規模なアプリケーションのパフォーマンスの向上が、優先順位の場合、ことで回避できます不必要な SQL クエリのみを使用して学生エンティティをインスタンス化してキーの値と、エンティティの状態に設定し、`Deleted`です。</span><span class="sxs-lookup"><span data-stu-id="3238a-272">If improving performance in a high-volume application is a priority, you could avoid an unnecessary SQL query by instantiating a Student entity using only the primary key value and then setting the entity state to `Deleted`.</span></span> <span data-ttu-id="3238a-273">エンティティを削除するのには、Entity Framework が必要なすべてです。</span><span class="sxs-lookup"><span data-stu-id="3238a-273">That's all that the Entity Framework needs in order to delete the entity.</span></span> <span data-ttu-id="3238a-274">(プロジェクトにこのコードを配置しないです。 ここで、代替手段を説明するためにのみ)</span><span class="sxs-lookup"><span data-stu-id="3238a-274">(Don't put this code in your project; it's here just to illustrate an alternative.)</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

<span data-ttu-id="3238a-275">エンティティには関連するデータも削除する必要がありますが場合、は、その連鎖削除がデータベースで構成されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="3238a-275">If the entity has related data that should also be deleted, make sure that cascade delete is configured in the database.</span></span> <span data-ttu-id="3238a-276">エンティティの削除に対するこのアプローチで EF が付かないかもしれませんを削除する関連エンティティが存在します。</span><span class="sxs-lookup"><span data-stu-id="3238a-276">With this approach to entity deletion, EF might not realize there are related entities to be deleted.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="3238a-277">削除ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="3238a-277">Update the Delete view</span></span>

<span data-ttu-id="3238a-278">*Views/Student/Delete.cshtml*、次の例に示すように h2 見出しと h3 見出しの間でのエラー メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="3238a-278">In *Views/Student/Delete.cshtml*, add an error message between the h2 heading and the h3 heading, as shown in the following example:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

<span data-ttu-id="3238a-279">アプリを実行する、選択、**受講者**タブをクリックし、をクリックして、**削除**ハイパーリンク。</span><span class="sxs-lookup"><span data-stu-id="3238a-279">Run the app, select the **Students** tab, and click a **Delete** hyperlink:</span></span>

![削除の確認 ページ](crud/_static/student-delete.png)

<span data-ttu-id="3238a-281">をクリックして**削除**です。</span><span class="sxs-lookup"><span data-stu-id="3238a-281">Click **Delete**.</span></span> <span data-ttu-id="3238a-282">削除された学生ないインデックス ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-282">The Index page is displayed without the deleted student.</span></span> <span data-ttu-id="3238a-283">(同時実行のチュートリアルではアクション内のコードの処理エラーの例を参照します)。</span><span class="sxs-lookup"><span data-stu-id="3238a-283">(You'll see an example of the error handling code in action in the concurrency tutorial.)</span></span>

## <a name="closing-database-connections"></a><span data-ttu-id="3238a-284">データベース接続を閉じる</span><span class="sxs-lookup"><span data-stu-id="3238a-284">Closing database connections</span></span>

<span data-ttu-id="3238a-285">データベース接続を保持するリソースを解放するには、コンテキスト インスタンス破棄する必要ができるだけ早くが完了したこととします。</span><span class="sxs-lookup"><span data-stu-id="3238a-285">To free up the resources that a database connection holds, the context instance must be disposed as soon as possible when you are done with it.</span></span> <span data-ttu-id="3238a-286">組み込みの ASP.NET Core[依存性の注入](../../fundamentals/dependency-injection.md)をそのタスクの行われます。</span><span class="sxs-lookup"><span data-stu-id="3238a-286">The ASP.NET Core built-in [dependency injection](../../fundamentals/dependency-injection.md) takes care of that task for you.</span></span>

<span data-ttu-id="3238a-287">*Startup.cs*を呼び出す、 [AddDbContext 拡張メソッド](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs)をプロビジョニング、 `DbContext` ASP.NET DI コンテナー内のクラスです。</span><span class="sxs-lookup"><span data-stu-id="3238a-287">In *Startup.cs*, you call the [AddDbContext extension method](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) to provision the `DbContext` class in the ASP.NET DI container.</span></span> <span data-ttu-id="3238a-288">メソッドでは、サービスの有効期間を設定`Scoped`既定です。</span><span class="sxs-lookup"><span data-stu-id="3238a-288">That method sets the service lifetime to `Scoped` by default.</span></span> <span data-ttu-id="3238a-289">`Scoped`コンテキスト オブジェクトの有効期間は、web 要求の存続期間と一致することを意味し、`Dispose`メソッドは、web 要求の最後に自動的に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-289">`Scoped` means the context object lifetime coincides with the web request life time, and the `Dispose` method will be called automatically at the end of the web request.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="3238a-290">トランザクションの処理</span><span class="sxs-lookup"><span data-stu-id="3238a-290">Handling Transactions</span></span>

<span data-ttu-id="3238a-291">既定では、Entity Framework では、トランザクションが暗黙的に実装しています。</span><span class="sxs-lookup"><span data-stu-id="3238a-291">By default the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="3238a-292">ここで複数の行またはテーブルを変更してを呼び出すシナリオで`SaveChanges`、Entity Framework が自動的に作成するすべての変更成功または失敗することを確認します。</span><span class="sxs-lookup"><span data-stu-id="3238a-292">In scenarios where you make changes to multiple rows or tables and then call `SaveChanges`, the Entity Framework automatically makes sure that either all of your changes succeed or they all fail.</span></span> <span data-ttu-id="3238a-293">いくつかの変更を最初に完了して、エラーが発生し、それらの変更は自動的にロールバックします。</span><span class="sxs-lookup"><span data-stu-id="3238a-293">If some changes are done first and then an error happens, those changes are automatically rolled back.</span></span> <span data-ttu-id="3238a-294">必要な複数を制御する--など--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクション](https://docs.microsoft.com/ef/core/saving/transactions)です。</span><span class="sxs-lookup"><span data-stu-id="3238a-294">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="no-tracking-queries"></a><span data-ttu-id="3238a-295">No 追跡クエリ</span><span class="sxs-lookup"><span data-stu-id="3238a-295">No-tracking queries</span></span>

<span data-ttu-id="3238a-296">データベース コンテキストは、テーブルの行を取得し、それらを表すエンティティ オブジェクトを作成、ときに既定では、追跡データベース内にメモリ内のエンティティが同期されているかどうか。</span><span class="sxs-lookup"><span data-stu-id="3238a-296">When a database context retrieves table rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="3238a-297">メモリ内のデータはキャッシュとして機能し、エンティティを更新するときに使用します。</span><span class="sxs-lookup"><span data-stu-id="3238a-297">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="3238a-298">このキャッシュではありません多くの場合、web アプリケーションで必要なコンテキストをインスタンス化は通常短時間 (新しい 1 つが作成され、破棄要求ごとに) とコンテキストを読み取るそのエンティティが再度使用される前に、通常、エンティティが破棄されています。</span><span class="sxs-lookup"><span data-stu-id="3238a-298">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="3238a-299">呼び出してメモリ内のエンティティ オブジェクトの追跡を無効にすることができます、`AsNoTracking`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3238a-299">You can disable tracking of entity objects in memory by calling the `AsNoTracking` method.</span></span> <span data-ttu-id="3238a-300">実行する一般的なシナリオを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="3238a-300">Typical scenarios in which you might want to do that include the following:</span></span>

* <span data-ttu-id="3238a-301">コンテキストの有効期間中に、エンティティを更新する必要はありません必要し、しないに EF[別々 のクエリによって取得されたエンティティのナビゲーション プロパティを自動的に読み込む](read-related-data.md)します。</span><span class="sxs-lookup"><span data-stu-id="3238a-301">During the context lifetime you don't need to update any entities, and you don't need EF to [automatically load navigation properties with  entities retrieved by separate queries](read-related-data.md).</span></span> <span data-ttu-id="3238a-302">頻繁にこれらの条件がコント ローラーの HttpGet アクション メソッドで満たされます。</span><span class="sxs-lookup"><span data-stu-id="3238a-302">Frequently these conditions are met in a controller's HttpGet action methods.</span></span>

* <span data-ttu-id="3238a-303">大量のデータを取得するクエリを実行しているし、返されるデータの小さな部分だけが更新されます。</span><span class="sxs-lookup"><span data-stu-id="3238a-303">You are running a query that retrieves a large volume of data, and only a small portion of the returned data will be updated.</span></span> <span data-ttu-id="3238a-304">大規模なクエリでは、追跡をオフにして、後で更新する必要があるいくつかのエンティティのクエリを実行する方が効率的がある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3238a-304">It may be more efficient to turn off tracking for the large query, and run a query later for the few entities that need to be updated.</span></span>

* <span data-ttu-id="3238a-305">更新するためにエンティティをアタッチするが、別の目的のため、同じエンティティを取得する前。</span><span class="sxs-lookup"><span data-stu-id="3238a-305">You want to attach an entity in order to update it, but earlier you retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="3238a-306">エンティティは、データベースのコンテキストによって既に追跡されているが、ために、変更するエンティティをアタッチできません。</span><span class="sxs-lookup"><span data-stu-id="3238a-306">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="3238a-307">この状況に対処する方法の 1 つが呼び出されて`AsNoTracking`以前のクエリにします。</span><span class="sxs-lookup"><span data-stu-id="3238a-307">One way to handle this situation is to call `AsNoTracking` on the earlier query.</span></span>

<span data-ttu-id="3238a-308">詳細については、次を参照してください。 [vs を追跡します。No 追跡](https://docs.microsoft.com/ef/core/querying/tracking)です。</span><span class="sxs-lookup"><span data-stu-id="3238a-308">For more information, see [Tracking vs. No-Tracking](https://docs.microsoft.com/ef/core/querying/tracking).</span></span>

## <a name="summary"></a><span data-ttu-id="3238a-309">まとめ</span><span class="sxs-lookup"><span data-stu-id="3238a-309">Summary</span></span>

<span data-ttu-id="3238a-310">学生のエンティティの単純な CRUD 操作を実行するページの完全なセットがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="3238a-310">You now have a complete set of pages that perform simple CRUD operations for Student entities.</span></span> <span data-ttu-id="3238a-311">次のチュートリアルでは、機能を拡張します、**インデックス**並べ替え、フィルター処理、およびページングを追加することによってページ。</span><span class="sxs-lookup"><span data-stu-id="3238a-311">In the next tutorial you'll expand the functionality of the **Index** page by adding sorting, filtering, and paging.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3238a-312">[前へ](intro.md)
[次へ](sort-filter-page.md)</span><span class="sxs-lookup"><span data-stu-id="3238a-312">[Previous](intro.md)
[Next](sort-filter-page.md)</span></span>  
