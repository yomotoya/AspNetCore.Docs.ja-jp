---
title: ASP.NET Core での Entity Framework Core を使用した Razor ページ - チュートリアル 1/8
author: rick-anderson
description: Entity Framework Core を使用して Razor ページ アプリを作成する方法について説明します
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/intro
ms.openlocfilehash: 2f6408f2381721c450519818a5973bad0f86ccad
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938408"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="20ae9-103">ASP.NET Core での Entity Framework Core を使用した Razor ページ - チュートリアル 1/8</span><span class="sxs-lookup"><span data-stu-id="20ae9-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="20ae9-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="20ae9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="20ae9-105">Contoso University のサンプル Web アプリでは、Entity Framework (EF) Core を使用して ASP.NET Core Razor Pages アプリを作成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="20ae9-105">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="20ae9-106">このサンプル アプリは架空の Contoso University の Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="20ae9-107">学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="20ae9-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="20ae9-108">このページは、Contoso University のサンプル アプリを作成する方法を説明するチュートリアル シリーズの 1 回目です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="20ae9-109">完成したアプリをダウンロードまたは表示します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="20ae9-110">[ダウンロードの方法はこちらをご覧ください。](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="20ae9-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20ae9-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="20ae9-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20ae9-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20ae9-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20ae9-113">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="20ae9-113">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20ae9-114">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20ae9-114">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20ae9-115">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="20ae9-115">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span></span>

------

<span data-ttu-id="20ae9-116">[Razor ページ](xref:razor-pages/index)に関する知識。</span><span class="sxs-lookup"><span data-stu-id="20ae9-116">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="20ae9-117">Razor ページのプログラミングが初めての場合、このシリーズを始める前に[こちらの入門編](xref:tutorials/razor-pages/razor-pages-start)を完了してください。</span><span class="sxs-lookup"><span data-stu-id="20ae9-117">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="20ae9-118">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="20ae9-118">Troubleshooting</span></span>

<span data-ttu-id="20ae9-119">解決できない問題に遭遇した場合、通常、[完成済みのプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)と自分のコードを比較することで解決策がわかります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-119">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="20ae9-120">ヘルプが必要なときは、[StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) で [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) または [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) について質問を投稿することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="20ae9-120">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="20ae9-121">Contoso University Web アプリ</span><span class="sxs-lookup"><span data-stu-id="20ae9-121">The Contoso University web app</span></span>

<span data-ttu-id="20ae9-122">このチュートリアル シリーズで作成するアプリは、大学向けの基本的な Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-122">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="20ae9-123">ユーザーは学生、講座、講師の情報を見たり、更新したりできます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="20ae9-124">チュートリアルで作成する画面は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-124">Here are a few of the screens created in the tutorial.</span></span>

![Students インデックス ページ](intro/_static/students-index.png)

![Students 編集ページ](intro/_static/student-edit.png)

<span data-ttu-id="20ae9-127">このサイトの UI スタイルは、組み込みのテンプレートによって生成されるものに類似しています。</span><span class="sxs-lookup"><span data-stu-id="20ae9-127">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="20ae9-128">このチュートリアルでは、UI ではなく、EF Core と Razor ページに重点を置きます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-128">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="20ae9-129">ContosoUniversity Razor Pages Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="20ae9-129">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20ae9-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20ae9-130">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="20ae9-131">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="20ae9-132">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="20ae9-133">プロジェクトに **ContosoUniversity** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="20ae9-134">このプロジェクトに *ContosoUniversity* という名前を付けることは重要です。そうすることでコードをコピーしたり貼り付けるときに名前空間が一致します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="20ae9-135">ドロップダウン リストで **[ASP.NET Core 2.1]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-135">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="20ae9-136">前の手順の画像については、「[Razor Web アプリの作成](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="20ae9-136">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).</span></span>
<span data-ttu-id="20ae9-137">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-137">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20ae9-138">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20ae9-138">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="20ae9-139">サイトのスタイルを設定する</span><span class="sxs-lookup"><span data-stu-id="20ae9-139">Set up the site style</span></span>

<span data-ttu-id="20ae9-140">変更をいくつか行い、サイトのメニュー、レイアウト、ホーム ページを決めます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-140">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="20ae9-141">*Pages/Shared/_Layout.cshtml* に次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-141">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="20ae9-142">"ContosoUniversity" をすべて "Contoso University" に変更します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-142">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="20ae9-143">これは 3 回出てきます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-143">There are three occurrences.</span></span>

* <span data-ttu-id="20ae9-144">メニュー エントリとして「**Students**」、「**Courses**」、「**Instructors**」、「**Departments**」を追加し、「**Contact**」を削除します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="20ae9-145">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-145">The changes are highlighted.</span></span> <span data-ttu-id="20ae9-146">(マークアップは一部表示されて*いません*。)</span><span class="sxs-lookup"><span data-stu-id="20ae9-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="20ae9-147">*Pages/Index.cshtml* で、ファイルの中身を次のコードに変更し、ASP.NET と MVC に関するテキストをこのアプリに関するテキストに変更します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="20ae9-148">データ モデルを作成する</span><span class="sxs-lookup"><span data-stu-id="20ae9-148">Create the data model</span></span>

<span data-ttu-id="20ae9-149">Contoso University アプリのエンティティ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-149">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="20ae9-150">次の 3 つのエンティティから始めます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-150">Start with the following three entities:</span></span>

![Course、Enrollment、Student からなるデータ モデルの図](intro/_static/data-model-diagram.png)

<span data-ttu-id="20ae9-152">`Student` エンティティと `Enrollment` エンティティの間には一対多の関係があります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-152">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="20ae9-153">`Course` エンティティと `Enrollment` エンティティの間には一対多の関係があります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-153">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="20ae9-154">1 人の学生をさまざまな講座に登録できます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-154">A student can enroll in any number of courses.</span></span> <span data-ttu-id="20ae9-155">1 つの講座に任意の数の学生を登録できます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-155">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="20ae9-156">次のセクションでは、エンティティごとにクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-156">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="20ae9-157">Student エンティティ</span><span class="sxs-lookup"><span data-stu-id="20ae9-157">The Student entity</span></span>

![Student エンティティの図](intro/_static/student-entity.png)

<span data-ttu-id="20ae9-159">*Models* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-159">Create a *Models* folder.</span></span> <span data-ttu-id="20ae9-160">以下のコードを使用して、*Models* フォルダーで、*Student.cs* という名前のクラス ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-160">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="20ae9-161">`ID` プロパティは、このクラスに相当するデータベース (DB) テーブルの主キー列になります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-161">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="20ae9-162">既定では、EF Core は、`ID` または `classnameID` という名前のプロパティを主キーとして解釈します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-162">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="20ae9-163">`classnameID` の `classname` はクラスの名前です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-163">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="20ae9-164">代わりの自動的に認識される主キーは、前の例の `StudentID` です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-164">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="20ae9-165">`Enrollments` プロパティはナビゲーション プロパティです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-165">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="20ae9-166">ナビゲーション プロパティは、このエンティティに関連する他のエンティティにリンクします。</span><span class="sxs-lookup"><span data-stu-id="20ae9-166">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="20ae9-167">この例では、`Student entity` の `Enrollments` プロパティによって、その `Student` に関連するすべての `Enrollment` エンティティが保持されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-167">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="20ae9-168">たとえば、DB 内のある Student 行に 2 つ関連する Enrollment 行がある場合、`Enrollments` ナビゲーション プロパティにその 2 つの `Enrollment` エンティティが含まれます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-168">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="20ae9-169">関連する `Enrollment` 行とは、その学生の主キー値を `StudentID` 列に含む行です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-169">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="20ae9-170">たとえば、ID=1 の学生には、`Enrollment` テーブルに行が 2 つあるとします。</span><span class="sxs-lookup"><span data-stu-id="20ae9-170">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="20ae9-171">その `Enrollment` テーブルには、`StudentID` = 1 の行が 2 つあります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-171">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="20ae9-172">`StudentID` は `Enrollment` テーブルの外部キーであり、`Student` テーブルでその学生を指定します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-172">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="20ae9-173">あるナビゲーション プロパティが複数のエンティティを保持できる場合、そのナビゲーション プロパティは `ICollection<T>` のようなリスト型にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-173">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="20ae9-174">`ICollection<T>` を指定するか、`List<T>` や `HashSet<T>` のような型を指定できます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-174">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="20ae9-175">`ICollection<T>` を使用する場合、EF Core で `HashSet<T>` コレクションが既定で作成されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-175">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="20ae9-176">複数のエンティティを保持するナビゲーション プロパティは、多対多または一対多の関係から発生します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-176">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="20ae9-177">Enrollment エンティティ</span><span class="sxs-lookup"><span data-stu-id="20ae9-177">The Enrollment entity</span></span>

![Enrollment エンティティの図](intro/_static/enrollment-entity.png)

<span data-ttu-id="20ae9-179">以下のコードを使用して、*Models* フォルダーで、*Enrollment.cs* を作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-179">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="20ae9-180">`EnrollmentID` プロパティは主キーです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-180">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="20ae9-181">このエンティティでは、`Student` エンティティのような `ID` ではなく、`classnameID` パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-181">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="20ae9-182">一般的に、開発者は 1 つのパターンを選択し、データ モデル全体でそれを使用します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-182">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="20ae9-183">後のチュートリアルでは、クラス名なしの ID を利用し、データ モデルに継承を簡単に実装します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-183">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="20ae9-184">`Grade` プロパティは `enum` です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-184">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="20ae9-185">`Grade` の型宣言の後の疑問符は、`Grade` プロパティが null 許容であることを示します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-185">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="20ae9-186">null という成績は、0 の成績とは異なります。null は成績がわからないことか、まだ評価されていないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-186">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="20ae9-187">`StudentID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Student` です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-187">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="20ae9-188">`Enrollment` エンティティは 1 つの `Student`エンティティに関連付けられますので、このプロパティには `Student` エンティティが 1 つ含まれます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-188">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="20ae9-189">`Student` エンティティは、複数の `Enrollment` エンティティが含まれる `Student.Enrollments` ナビゲーション プロパティとは異なります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-189">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="20ae9-190">`CourseID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Course` です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-190">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="20ae9-191">`Enrollment` エンティティは 1 つの `Course` エンティティに関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-191">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="20ae9-192">EF Core は、プロパティの名前が `<navigation property name><primary key property name>` であれば、それを外部キーとして解釈します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-192">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="20ae9-193">たとえば、`Student` ナビゲーション プロパティの `StudentID` です。`Student` エンティティの主キーは `ID` であるからです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-193">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="20ae9-194">外部キーのプロパティには `<primary key property name>` という名前を付けることもできます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-194">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="20ae9-195">たとえば、`CourseID` にします。`Course` エンティティの主キーが `CourseID` であるからです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-195">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="20ae9-196">Course エンティティ</span><span class="sxs-lookup"><span data-stu-id="20ae9-196">The Course entity</span></span>

![Course エンティティの図](intro/_static/course-entity.png)

<span data-ttu-id="20ae9-198">以下のコードを使用して、*Models* フォルダーで、*Course.cs* を作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-198">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="20ae9-199">`Enrollments` プロパティはナビゲーション プロパティです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-199">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="20ae9-200">1 つの `Course` エンティティにたくさんの `Enrollment` エンティティを関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-200">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="20ae9-201">`DatabaseGenerated` 属性によって、アプリで主キーを指定できます。DB に生成させるのではありません。</span><span class="sxs-lookup"><span data-stu-id="20ae9-201">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="20ae9-202">Student モデルをスキャホールディングする</span><span class="sxs-lookup"><span data-stu-id="20ae9-202">Scaffold the student model</span></span>

<span data-ttu-id="20ae9-203">このセクションでは、Student モデルがスキャフォールディングされます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-203">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="20ae9-204">つまり、スキャフォールディング ツールにより、Student モデルの作成、読み取り、更新、削除の (CRUD) 操作用のページが生成されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-204">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="20ae9-205">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="20ae9-205">Build the project.</span></span>
* <span data-ttu-id="20ae9-206">*Pages/Students* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-206">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20ae9-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20ae9-207">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="20ae9-208">**ソリューション エクスプローラー**で、*[Pages/Students]* フォルダーを右クリックし、**[追加]** > **[スキャフォールディングされた新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-208">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="20ae9-209">**[スキャフォールディングを追加]** ダイアログで、**[Razor Pages using Entity Framework (CRUD)]\(Entity Framework を使用する Razor Pages (CRUD)\)** > **[追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-209">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="20ae9-210">**[Add Razor Pages using Entity Framework (CRUD)]\(Entity Framework を使用して Razor Pages (CRUD) を追加する\)** ダイアログを完了します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-210">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="20ae9-211">**[モデル クラス]** ドロップダウンで、**[Student (ContosoUniversity.Models)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-211">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="20ae9-212">**[データ コンテキスト クラス]** 行で **+** (+) 記号を選択し、生成された名前を **ContosoUniversity.Models.SchoolContext** に変更します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-212">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="20ae9-213">**[データ コンテキスト クラス]** ドロップダウンで、**[ContosoUniversity.Models.SchoolContext]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-213">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="20ae9-214">**[追加]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-214">Select **Add**.</span></span>

![CRUD ダイアログ](intro/_static/s1.png)

<span data-ttu-id="20ae9-216">前の手順に問題がある場合は、「[ムービー モデルのスキャフォールディング](xref:tutorials/razor-pages/model#scaffold-the-movie-model)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="20ae9-216">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20ae9-217">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="20ae9-217">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20ae9-218">次のコマンドを実行して、Student モデルをスキャホールディングします。</span><span class="sxs-lookup"><span data-stu-id="20ae9-218">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

<span data-ttu-id="20ae9-219">スキャフォールディングのプロセスが作成され、次のファイルが変更されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-219">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="20ae9-220">作成されたファイル</span><span class="sxs-lookup"><span data-stu-id="20ae9-220">Files created</span></span>

* <span data-ttu-id="20ae9-221">*Pages/Students* 作成、削除、詳細、編集、インデックス。</span><span class="sxs-lookup"><span data-stu-id="20ae9-221">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="20ae9-222">*Data/ContosoUniversityContext.cs*</span><span class="sxs-lookup"><span data-stu-id="20ae9-222">*Data/ContosoUniversityContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="20ae9-223">更新されたファイル</span><span class="sxs-lookup"><span data-stu-id="20ae9-223">Files updates</span></span>

* <span data-ttu-id="20ae9-224">*Startup.cs*: このファイルに対しての変更を次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-224">*Startup.cs* : Changes to this file in are detailed the next section.</span></span>
* <span data-ttu-id="20ae9-225">*appsettings.json*: ローカル データベースへの接続に使用される接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-225">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="20ae9-226">依存関係挿入に登録されるコンテキストを調べる</span><span class="sxs-lookup"><span data-stu-id="20ae9-226">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="20ae9-227">ASP.NET Core には、[依存関係挿入](xref:fundamentals/dependency-injection)が組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="20ae9-227">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="20ae9-228">サービス (EF Core DB コンテキストなど) は、アプリケーションの起動時に依存関係挿入に登録されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-228">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="20ae9-229">これらのサービスを必要とするコンポーネント (Razor ページなど) には、コンストラクターのパラメーターを介してこれらのサービスが指定されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-229">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="20ae9-230">db コンテキスト インスタンスを取得するコンストラクター コードは、チュートリアルの後半で示します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-230">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="20ae9-231">スキャフォールディング ツールが自動的に DB コンテキストを作成し、依存関係挿入コンテナーと共に登録します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-231">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="20ae9-232">*Startup.cs* の `ConfigureServices` メソッドを調べます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-232">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="20ae9-233">強調表示された行は、スキャフォールダーによって追加されました。</span><span class="sxs-lookup"><span data-stu-id="20ae9-233">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="20ae9-234">[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) オブジェクトでメソッドが呼び出され、接続文字列の名前がコンテキストに渡されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-234">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="20ae9-235">ローカル開発の場合、[ASP.NET Core 構成システム](xref:fundamentals/configuration/index)が *appsettings.json* ファイルから接続文字列を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-235">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="20ae9-236">main を更新する</span><span class="sxs-lookup"><span data-stu-id="20ae9-236">Update main</span></span>

<span data-ttu-id="20ae9-237">*Program.cs* で、次を実行するように `Main` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-237">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="20ae9-238">依存関係挿入コンテナーから DB コンテキスト インスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-238">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="20ae9-239">[EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-239">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="20ae9-240">`EnsureCreated` メソッドが完了したら、コンテキストを破棄します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-240">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="20ae9-241">次は、更新された *Program.cs* ファイルのコードです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-241">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="20ae9-242">`EnsureCreated` で、コンテキストのデータベースが存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-242">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="20ae9-243">存在する場合、アクションは何も実行されません。</span><span class="sxs-lookup"><span data-stu-id="20ae9-243">If it exists, no action is taken.</span></span> <span data-ttu-id="20ae9-244">存在しない場合は、データベースとそのすべてのスキーマが作成されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-244">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="20ae9-245">`EnsureCreated` は、データベースの作成に移行を使用しません。</span><span class="sxs-lookup"><span data-stu-id="20ae9-245">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="20ae9-246">`EnsureCreated` で作成されたデータベースは、後で移行を使用して更新することはできません。</span><span class="sxs-lookup"><span data-stu-id="20ae9-246">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="20ae9-247">アプリの起動時に `EnsureCreated` が呼び出され、次のワークフローが可能になります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-247">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="20ae9-248">DB を削除します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-248">Delete the DB.</span></span>
* <span data-ttu-id="20ae9-249">DB スキーマを変更します (たとえば、`EmailAddress` フィールドを追加します)。</span><span class="sxs-lookup"><span data-stu-id="20ae9-249">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="20ae9-250">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-250">Run the app.</span></span>
* <span data-ttu-id="20ae9-251">`EnsureCreated` によって、`EmailAddress` 列を含む DB が作成されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-251">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="20ae9-252">`EnsureCreated` は、スキーマが急速に進化している開発の早期に便利です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-252">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="20ae9-253">このチュートリアルの後半では、DB は削除され、移行が使用されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-253">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="20ae9-254">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="20ae9-254">Test the app</span></span>

<span data-ttu-id="20ae9-255">アプリを実行し、Cookie ポリシーに同意します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-255">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="20ae9-256">このアプリで個人情報が保持されることはありません。</span><span class="sxs-lookup"><span data-stu-id="20ae9-256">This app doesn't keep personal information.</span></span> <span data-ttu-id="20ae9-257">Cookie ポリシーについては、[EU の一般的なデータ保護規制 (GDPR) のサポート](xref:security/gdpr)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="20ae9-257">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="20ae9-258">**[Students]** リンクを選択し、**[新規作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-258">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="20ae9-259">[編集]、[詳細]、および [削除] の各リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="20ae9-259">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="20ae9-260">SchoolContext DB コンテキストを確認する</span><span class="sxs-lookup"><span data-stu-id="20ae9-260">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="20ae9-261">所与のデータ モデルの EF Core 機能を調整するメイン クラスは、DB コンテキスト クラスです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-261">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="20ae9-262">データ コンテキストは [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) から派生されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-262">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="20ae9-263">データ コンテキストによって、データ モデルに含めるエンティティが指定されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-263">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="20ae9-264">このプロジェクトでは、クラスに `SchoolContext` という名前が付けられています。</span><span class="sxs-lookup"><span data-stu-id="20ae9-264">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="20ae9-265">次のコードを使用して *SchoolContext.cs* を更新します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-265">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="20ae9-266">強調表示されたコードで、各エンティティ セットの [DbSet\< TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) プロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-266">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="20ae9-267">EF Core 用語で:</span><span class="sxs-lookup"><span data-stu-id="20ae9-267">In EF Core terminology:</span></span>

* <span data-ttu-id="20ae9-268">エンティティ セットは通常、DB テーブルに対応します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-268">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="20ae9-269">エンティティはテーブル内の行に対応します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-269">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="20ae9-270">`DbSet<Enrollment>` と `DbSet<Course>` は省略可能です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-270">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="20ae9-271">EF Core にはそれらが暗黙的に含まれます。`Student` エンティティが `Enrollment` エンティティを参照し、`Enrollment` エンティティが `Course` エンティティを参照するためです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-271">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="20ae9-272">このチュートリアルでは、`SchoolContext` に `DbSet<Enrollment>` と `DbSet<Course>` を残します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-272">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="20ae9-273">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="20ae9-273">SQL Server Express LocalDB</span></span>

<span data-ttu-id="20ae9-274">この接続文字列によって [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb) が指定されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-274">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="20ae9-275">LocalDB は SQL Server Express データベース エンジンの軽量版であり、実稼働ではなく、アプリの開発を意図して設計されています。</span><span class="sxs-lookup"><span data-stu-id="20ae9-275">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="20ae9-276">LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。</span><span class="sxs-lookup"><span data-stu-id="20ae9-276">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="20ae9-277">既定では、LocalDB は `C:/Users/<user>` ディレクトリに *.mdf* DB ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-277">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="20ae9-278">テスト データで DB を初期化するコードを追加する</span><span class="sxs-lookup"><span data-stu-id="20ae9-278">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="20ae9-279">EF Core によって空の DB が作成されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-279">EF Core creates an empty DB.</span></span> <span data-ttu-id="20ae9-280">このセクションでは、それにテスト データを入力するために `Initialize` メソッドを記述します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-280">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="20ae9-281">*Data* フォルダーで、*DbInitializer.cs* という名前の新しいクラス ファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-281">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="20ae9-282">このコードは、DB に学生が存在するかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-282">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="20ae9-283">DB に学生が存在しない場合、テスト データを使用して DB が初期化されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-283">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="20ae9-284">`List<T>` コレクションではなく配列にテスト データを読み込み、パフォーマンスを最適化します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-284">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="20ae9-285">`EnsureCreated` メソッドは、DB のコンテキストに合わせて DB を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-285">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="20ae9-286">DB が存在する場合、`EnsureCreated` は DB を変更することなく戻ってきます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-286">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="20ae9-287">*Program.cs* で、`Initialize` を呼び出すように `Main` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-287">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="20ae9-288">学生のレコードをすべて削除し、アプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-288">Delete any student records and restart the app.</span></span> <span data-ttu-id="20ae9-289">DB が初期化されていない場合は、ブレーク ポイントを `Initialize` に設定して問題を診断します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-289">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="20ae9-290">DB を表示する</span><span class="sxs-lookup"><span data-stu-id="20ae9-290">View the DB</span></span>

<span data-ttu-id="20ae9-291">Visual Studio の **[表示]** メニューから **SQL Server オブジェクト エクスプローラー** (SSOX) を開きます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-291">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="20ae9-292">SSOX で、**(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="20ae9-292">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="20ae9-293">**[Tables]\(テーブル\)** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-293">Expand the **Tables** node.</span></span>

<span data-ttu-id="20ae9-294">**[Student]\(学生\)** テーブルを右クリックし、**[View Data]\(データの表示\)** をクリックすると、作成された列とテーブルに挿入された行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-294">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="20ae9-295">非同期コード</span><span class="sxs-lookup"><span data-stu-id="20ae9-295">Asynchronous code</span></span>

<span data-ttu-id="20ae9-296">ASP.NET Core と EF Core では、非同期プログラミングが既定のモードです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-296">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="20ae9-297">Web サーバーでは、利用できるスレッド数に限りがあります。負荷が高い状況では、利用できるスレッドが全部使われる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-297">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="20ae9-298">その場合、スレッドが解放されるまでサーバーは新しい要求を処理できません。</span><span class="sxs-lookup"><span data-stu-id="20ae9-298">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="20ae9-299">同期コードの場合、たくさんのスレッドが関連付けられていても、I/O の完了を待っているため、実際には何の作業も行っていないということがあります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-299">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="20ae9-300">非同期コードの場合、あるプロセスが I/O の完了を待っているとき、そのスレッドは解放され、サーバーによって他の要求の処理に利用できます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-300">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="20ae9-301">結果として、非同期コードの場合、サーバー リソースをより効率的に利用できます。サーバーは、より多くのトラフィックを遅延なく処理できます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-301">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="20ae9-302">非同期コードが実行時に発生させるオーバーヘッドは少量です。</span><span class="sxs-lookup"><span data-stu-id="20ae9-302">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="20ae9-303">トラフィックが少ない場合、パフォーマンスに与える影響は無視して構わない程度です。トラフィックが多い場合、相当なパフォーマンス改善が見込まれます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-303">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="20ae9-304">次のコードでは、キーワード [async](/dotnet/csharp/language-reference/keywords/async)、戻り値 `Task<T>`、キーワード `await`、メソッド `ToListAsync` によりコードの実行が非同期になります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-304">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="20ae9-305">キーワード `async` は次のことをコンパイラに指示します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-305">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="20ae9-306">メソッド本文の一部にコールバックを生成する。</span><span class="sxs-lookup"><span data-stu-id="20ae9-306">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="20ae9-307">返された [Task](/dotnet/api/system.threading.tasks.task) オブジェクトを自動作成する。</span><span class="sxs-lookup"><span data-stu-id="20ae9-307">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="20ae9-308">詳細については、[Task の戻り値の型](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)に関する記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="20ae9-308">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="20ae9-309">暗黙の戻り値の型 `Task` は進行中の作業を表します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-309">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="20ae9-310">キーワード `await` により、コンパイラはメソッドを 2 つに分割します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-310">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="20ae9-311">最初の部分は、非同期で開始される操作で終わります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-311">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="20ae9-312">2 つ目の部分は、操作の完了時に呼び出されるコールバック メソッドに入ります。</span><span class="sxs-lookup"><span data-stu-id="20ae9-312">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="20ae9-313">`ToListAsync` は、`ToList` 拡張メソッドの非同期バージョンです。</span><span class="sxs-lookup"><span data-stu-id="20ae9-313">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="20ae9-314">EF Core を利用する非同期コードの記述で注意すべき点:</span><span class="sxs-lookup"><span data-stu-id="20ae9-314">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="20ae9-315">クエリやコマンドを DB に送信するステートメントのみが非同期で実行されます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-315">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="20ae9-316">これには、`ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync`、`SaveChangesAsync` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="20ae9-316">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="20ae9-317">`var students = context.Students.Where(s => s.LastName == "Davolio")` など、`IQueryable` を変更するだけのステートメントは含まれません。</span><span class="sxs-lookup"><span data-stu-id="20ae9-317">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="20ae9-318">EF Core コンテキストはスレッド セーフではありません。複数の操作を並列実行しないでください。</span><span class="sxs-lookup"><span data-stu-id="20ae9-318">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="20ae9-319">非同期コードのパフォーマンス上の利点を最大限に活用するには、クエリをデータベースに送信させる EF Core メソッドを (ページングなどのための) ライブラリ パッケージで呼び出す場合、そのライブラリ パッケージで非同期が利用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-319">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="20ae9-320">.NET での非同期プログラミングの詳細については、「[非同期の概要](/dotnet/articles/standard/async)」と「[Async および Await を使用した非同期プログラミング (C#)](/dotnet/csharp/programming-guide/concepts/async/)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="20ae9-320">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="20ae9-321">次のチュートリアルでは、基本的な CRUD (作成、読み取り、更新、削除) の操作について説明します。</span><span class="sxs-lookup"><span data-stu-id="20ae9-321">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="20ae9-322">次へ</span><span class="sxs-lookup"><span data-stu-id="20ae9-322">Next</span></span>](xref:data/ef-rp/crud)
