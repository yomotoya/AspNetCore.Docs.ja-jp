---
title: ASP.NET Core での Entity Framework Core を使用した Razor ページ - チュートリアル 1/8
author: rick-anderson
description: Entity Framework Core を使用して Razor ページ アプリを作成する方法について説明します
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: d7cf4740f31f1e0ae56461efc4c1b3d91238270f
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726018"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="a55e8-103">ASP.NET Core での Entity Framework Core を使用した Razor ページ - チュートリアル 1/8</span><span class="sxs-lookup"><span data-stu-id="a55e8-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="a55e8-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a55e8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a55e8-105">Contoso University のサンプル Web アプリでは、Entity Framework (EF) Core 2.0 と Visual Studio 2017 を使用して ASP.NET Core 2.0 MVC Web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="a55e8-106">このサンプル アプリは架空の Contoso University の Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="a55e8-107">学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a55e8-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="a55e8-108">このページは、Contoso University のサンプル アプリを作成する方法を説明するチュートリアル シリーズの 1 回目です。</span><span class="sxs-lookup"><span data-stu-id="a55e8-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="a55e8-109">完成したアプリをダウンロードまたは表示します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="a55e8-110">[ダウンロードの方法はこちらをご覧ください。](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a55e8-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a55e8-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a55e8-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<span data-ttu-id="a55e8-112">[Razor ページ](xref:mvc/razor-pages/index)に関する知識。</span><span class="sxs-lookup"><span data-stu-id="a55e8-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="a55e8-113">Razor ページのプログラミングが初めての場合、このシリーズを始める前に[こちらの入門編](xref:tutorials/razor-pages/razor-pages-start)を完了してください。</span><span class="sxs-lookup"><span data-stu-id="a55e8-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a55e8-114">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="a55e8-114">Troubleshooting</span></span>

<span data-ttu-id="a55e8-115">解決できない問題に遭遇した場合、通常、[完成したステージ](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)と自分のコードを比較することで解決策がわかります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span></span> <span data-ttu-id="a55e8-116">一般的なエラーとその解決方法の一覧については、[このシリーズの最後のチュートリアルにあるトラブルシューティングのセクション](xref:data/ef-mvc/advanced#common-errors)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a55e8-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="a55e8-117">そこで必要な答えが見つからない場合、[StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) で [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) または [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) に関する質問を投稿できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="a55e8-118">このチュートリアル シリーズは、前のチュートリアルで行った内容を基に構築されています。</span><span class="sxs-lookup"><span data-stu-id="a55e8-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="a55e8-119">チュートリアルが完了したら、毎回、プロジェクトのコピーを保存するようお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a55e8-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="a55e8-120">問題に遭遇したとき、前のチュートリアルから始めることができます。最初まで戻る必要がありません。</span><span class="sxs-lookup"><span data-stu-id="a55e8-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="a55e8-121">あるいは[完了したステージ](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)をダウンロードし、それを利用してやり直すこともできます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="a55e8-122">Contoso University Web アプリ</span><span class="sxs-lookup"><span data-stu-id="a55e8-122">The Contoso University web app</span></span>

<span data-ttu-id="a55e8-123">このチュートリアル シリーズで作成するアプリは、大学向けの基本的な Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="a55e8-124">ユーザーは学生、講座、講師の情報を見たり、更新したりできます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="a55e8-125">チュートリアルで作成する画面は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-125">Here are a few of the screens created in the tutorial.</span></span>

![Students インデックス ページ](intro/_static/students-index.png)

![Students 編集ページ](intro/_static/student-edit.png)

<span data-ttu-id="a55e8-128">このサイトの UI スタイルは、組み込みのテンプレートによって生成されるものに類似しています。</span><span class="sxs-lookup"><span data-stu-id="a55e8-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="a55e8-129">このチュートリアルでは、UI ではなく、EF Core と Razor ページに重点を置きます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="a55e8-130">Razor ページ Web アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="a55e8-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="a55e8-131">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a55e8-132">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="a55e8-133">プロジェクトに **ContosoUniversity** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="a55e8-134">このプロジェクトに *ContosoUniversity* という名前を付けることは重要です。そうすることでコードをコピーしたり貼り付けるときに名前空間が一致します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="a55e8-135">![新しい ASP.NET Core Web アプリケーション](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="a55e8-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="a55e8-136">ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="a55e8-137">![Web アプリケーション (Razor ページ)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="a55e8-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="a55e8-138">**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="a55e8-139">サイトのスタイルを設定する</span><span class="sxs-lookup"><span data-stu-id="a55e8-139">Set up the site style</span></span>

<span data-ttu-id="a55e8-140">変更をいくつか行い、サイトのメニュー、レイアウト、ホーム ページを決めます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="a55e8-141">*Pages/_Layout.cshtml* を開き、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="a55e8-142">"ContosoUniversity" をすべて "Contoso University" に変更します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="a55e8-143">これは 3 回出てきます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-143">There are three occurrences.</span></span>

* <span data-ttu-id="a55e8-144">メニュー エントリとして「**Students**」、「**Courses**」、「**Instructors**」、「**Departments**」を追加し、「**Contact**」を削除します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="a55e8-145">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-145">The changes are highlighted.</span></span> <span data-ttu-id="a55e8-146">(マークアップは一部表示されて*いません*。)</span><span class="sxs-lookup"><span data-stu-id="a55e8-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="a55e8-147">*Pages/Index.cshtml* で、ファイルの中身を次のコードに変更し、ASP.NET と MVC に関するテキストをこのアプリに関するテキストに変更します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="a55e8-148">Ctrl キーを押しながら F5 キーを押してプロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="a55e8-149">ホーム ページには、次のチュートリアルで作成されるタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Contoso University のホーム ページ](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="a55e8-151">データ モデルを作成する</span><span class="sxs-lookup"><span data-stu-id="a55e8-151">Create the data model</span></span>

<span data-ttu-id="a55e8-152">Contoso University アプリのエンティティ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="a55e8-153">次の 3 つのエンティティから始めます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-153">Start with the following three entities:</span></span>

![Course、Enrollment、Student からなるデータ モデルの図](intro/_static/data-model-diagram.png)

<span data-ttu-id="a55e8-155">`Student` エンティティと `Enrollment` エンティティの間には一対多の関係があります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="a55e8-156">`Course` エンティティと `Enrollment` エンティティの間には一対多の関係があります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="a55e8-157">1 人の学生をさまざまな講座に登録できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="a55e8-158">1 つの講座に任意の数の学生を登録できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="a55e8-159">次のセクションでは、エンティティごとにクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="a55e8-160">Student エンティティ</span><span class="sxs-lookup"><span data-stu-id="a55e8-160">The Student entity</span></span>

![Student エンティティの図](intro/_static/student-entity.png)

<span data-ttu-id="a55e8-162">*Models* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-162">Create a *Models* folder.</span></span> <span data-ttu-id="a55e8-163">以下のコードを使用して、*Models* フォルダーで、*Student.cs* という名前のクラス ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="a55e8-164">`ID` プロパティは、このクラスに相当するデータベース (DB) テーブルの主キー列になります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="a55e8-165">既定では、EF Core は、`ID` または `classnameID` という名前のプロパティを主キーとして解釈します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="a55e8-166">`classnameID` で、`classname` は、前述の `Student` の例のとおりクラス名です。</span><span class="sxs-lookup"><span data-stu-id="a55e8-166">In `classnameID`, `classname` is the name of the class, such as `Student` in the preceding example.</span></span>

<span data-ttu-id="a55e8-167">`Enrollments` プロパティはナビゲーション プロパティです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-167">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="a55e8-168">ナビゲーション プロパティは、このエンティティに関連する他のエンティティにリンクします。</span><span class="sxs-lookup"><span data-stu-id="a55e8-168">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="a55e8-169">この例では、`Student entity` の `Enrollments` プロパティによって、その `Student` に関連するすべての `Enrollment` エンティティが保持されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-169">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="a55e8-170">たとえば、DB 内のある Student 行に 2 つ関連する Enrollment 行がある場合、`Enrollments` ナビゲーション プロパティにその 2 つの `Enrollment` エンティティが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-170">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="a55e8-171">関連する `Enrollment` 行とは、その学生の主キー値を `StudentID` 列に含む行です。</span><span class="sxs-lookup"><span data-stu-id="a55e8-171">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="a55e8-172">たとえば、ID=1 の学生には、`Enrollment` テーブルに行が 2 つあるとします。</span><span class="sxs-lookup"><span data-stu-id="a55e8-172">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="a55e8-173">その `Enrollment` テーブルには、`StudentID` = 1 の行が 2 つあります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-173">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="a55e8-174">`StudentID` は `Enrollment` テーブルの外部キーであり、`Student` テーブルでその学生を指定します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-174">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="a55e8-175">あるナビゲーション プロパティが複数のエンティティを保持できる場合、そのナビゲーション プロパティは `ICollection<T>` のようなリスト型にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-175">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="a55e8-176">`ICollection<T>` を指定するか、`List<T>` や `HashSet<T>` のような型を指定できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-176">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="a55e8-177">`ICollection<T>` を使用する場合、EF Core で `HashSet<T>` コレクションが既定で作成されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-177">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="a55e8-178">複数のエンティティを保持するナビゲーション プロパティは、多対多または一対多の関係から発生します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-178">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="a55e8-179">Enrollment エンティティ</span><span class="sxs-lookup"><span data-stu-id="a55e8-179">The Enrollment entity</span></span>

![Enrollment エンティティの図](intro/_static/enrollment-entity.png)

<span data-ttu-id="a55e8-181">以下のコードを使用して、*Models* フォルダーで、*Enrollment.cs* を作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-181">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="a55e8-182">`EnrollmentID` プロパティは主キーです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-182">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="a55e8-183">このエンティティでは、`Student` エンティティのような `ID` ではなく、`classnameID` パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-183">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="a55e8-184">一般的に、開発者は 1 つのパターンを選択し、データ モデル全体でそれを使用します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-184">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="a55e8-185">後のチュートリアルでは、クラス名なしの ID を利用し、データ モデルに継承を簡単に実装します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-185">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="a55e8-186">`Grade` プロパティは `enum` です。</span><span class="sxs-lookup"><span data-stu-id="a55e8-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="a55e8-187">`Grade` の型宣言の後の疑問符は、`Grade` プロパティが null 許容であることを示します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="a55e8-188">null という成績は、0 の成績とは異なります。null は成績がわからないことか、まだ評価されていないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="a55e8-189">`StudentID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Student` です。</span><span class="sxs-lookup"><span data-stu-id="a55e8-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="a55e8-190">`Enrollment` エンティティは 1 つの `Student`エンティティに関連付けられますので、このプロパティには `Student` エンティティが 1 つ含まれます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-190">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="a55e8-191">`Student` エンティティは、複数の `Enrollment` エンティティが含まれる `Student.Enrollments` ナビゲーション プロパティとは異なります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-191">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="a55e8-192">`CourseID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Course` です。</span><span class="sxs-lookup"><span data-stu-id="a55e8-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="a55e8-193">`Enrollment` エンティティは 1 つの `Course` エンティティに関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="a55e8-194">EF Core は、プロパティの名前が `<navigation property name><primary key property name>` であれば、それを外部キーとして解釈します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-194">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="a55e8-195">たとえば、`Student` ナビゲーション プロパティの `StudentID` です。`Student` エンティティの主キーは `ID` であるからです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-195">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="a55e8-196">外部キーのプロパティには `<primary key property name>` という名前を付けることもできます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-196">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="a55e8-197">たとえば、`CourseID` にします。`Course` エンティティの主キーが `CourseID` であるからです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-197">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="a55e8-198">Course エンティティ</span><span class="sxs-lookup"><span data-stu-id="a55e8-198">The Course entity</span></span>

![Course エンティティの図](intro/_static/course-entity.png)

<span data-ttu-id="a55e8-200">以下のコードを使用して、*Models* フォルダーで、*Course.cs* を作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-200">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="a55e8-201">`Enrollments` プロパティはナビゲーション プロパティです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="a55e8-202">1 つの `Course` エンティティにたくさんの `Enrollment` エンティティを関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="a55e8-203">`DatabaseGenerated` 属性によって、アプリで主キーを指定できます。DB に生成させるのではありません。</span><span class="sxs-lookup"><span data-stu-id="a55e8-203">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="a55e8-204">SchoolContext DB コンテキストを作成する</span><span class="sxs-lookup"><span data-stu-id="a55e8-204">Create the SchoolContext DB context</span></span>

<span data-ttu-id="a55e8-205">所与のデータ モデルの EF Core 機能を調整するメイン クラスは、DB コンテキスト クラスです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-205">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="a55e8-206">データ コンテキストは `Microsoft.EntityFrameworkCore.DbContext` から派生します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-206">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="a55e8-207">データ コンテキストによって、データ モデルに含めるエンティティが指定されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-207">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="a55e8-208">このプロジェクトでは、クラスに `SchoolContext` という名前が付けられています。</span><span class="sxs-lookup"><span data-stu-id="a55e8-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="a55e8-209">プロジェクト フォルダーで、*Data* という名前のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="a55e8-210">*Data* フォルダーで、次のコードで *SchoolContext.cs* を作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-210">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="a55e8-211">このコードによって、エンティティ セットごとに `DbSet` プロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="a55e8-212">EF Core 用語で:</span><span class="sxs-lookup"><span data-stu-id="a55e8-212">In EF Core terminology:</span></span>

* <span data-ttu-id="a55e8-213">エンティティ セットは通常、DB テーブルに対応します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-213">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="a55e8-214">エンティティはテーブル内の行に対応します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-214">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a55e8-215">`DbSet<Enrollment>` と `DbSet<Course>` は省略できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-215">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="a55e8-216">EF Core にはそれらが暗黙的に含まれます。`Student` エンティティが `Enrollment` エンティティを参照し、`Enrollment` エンティティが `Course` エンティティを参照するためです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-216">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="a55e8-217">このチュートリアルでは、`SchoolContext` に `DbSet<Enrollment>` と `DbSet<Course>` を残します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-217">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="a55e8-218">DB が作成されると、EF Core によって、`DbSet` プロパティと同じ名前を持つテーブルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-218">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="a55e8-219">コレクションのプロパティ名は通常、複数形になります (Student ではなく Students)。</span><span class="sxs-lookup"><span data-stu-id="a55e8-219">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="a55e8-220">テーブル名を複数形にするかどうかについては、開発者の間で意見が分かれます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-220">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="a55e8-221">このチュートリアル シリーズでは、DbContext に単数形のテーブル名を指定して既定の動作をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="a55e8-221">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="a55e8-222">単数形のテーブル名を指定するには、次の強調表示されているコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-222">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="a55e8-223">依存関係挿入にコンテキストを登録する</span><span class="sxs-lookup"><span data-stu-id="a55e8-223">Register the context with dependency injection</span></span>

<span data-ttu-id="a55e8-224">ASP.NET Core には、[依存関係挿入](xref:fundamentals/dependency-injection)が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a55e8-224">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a55e8-225">サービス (EF Core DB コンテキストなど) は、アプリケーションの起動時に依存関係挿入に登録されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-225">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a55e8-226">これらのサービスを必要とするコンポーネント (Razor ページなど) には、コンストラクターのパラメーターを介してこれらのサービスが指定されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-226">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a55e8-227">db コンテキスト インスタンスを取得するコンストラクター コードは、チュートリアルの後半で示します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-227">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a55e8-228">`SchoolContext` をサービスとして登録するには、*Startup.cs* を開き、強調表示されている行を `ConfigureServices` メソッドに追加します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-228">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="a55e8-229">`DbContextOptionsBuilder` オブジェクトでメソッドが呼び出され、接続文字列の名前がコンテキストに渡されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-229">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="a55e8-230">ローカル開発の場合、[ASP.NET Core 構成システム](xref:fundamentals/configuration/index)が *appsettings.json* ファイルから接続文字列を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-230">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="a55e8-231">`using` ステートメントを名前空間 `ContosoUniversity.Data` と `Microsoft.EntityFrameworkCore` に追加します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-231">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="a55e8-232">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="a55e8-232">Build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="a55e8-233">*appsettings.json* ファイルを開き、次のコードのように接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-233">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="a55e8-234">先の接続文字列では、[SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) が停止しないように `ConnectRetryCount=0` を使用します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-234">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="a55e8-235">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="a55e8-235">SQL Server Express LocalDB</span></span>

<span data-ttu-id="a55e8-236">この接続文字列によって SQL Server LocalDB DB が指定されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-236">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="a55e8-237">LocalDB は SQL Server Express データベース エンジンの軽量版であり、実稼働ではなく、アプリの開発を意図して設計されています。</span><span class="sxs-lookup"><span data-stu-id="a55e8-237">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="a55e8-238">LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。</span><span class="sxs-lookup"><span data-stu-id="a55e8-238">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="a55e8-239">既定では、LocalDB は `C:/Users/<user>` ディレクトリに *.mdf* DB ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-239">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="a55e8-240">テスト データで DB を初期化するコードを追加する</span><span class="sxs-lookup"><span data-stu-id="a55e8-240">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="a55e8-241">EF Core によって空の DB が作成されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-241">EF Core creates an empty DB.</span></span> <span data-ttu-id="a55e8-242">このセクションでは、それにテスト データを入力するために *Seed* メソッドを記述します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-242">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="a55e8-243">*Data* フォルダーで、*DbInitializer.cs* という名前の新しいクラス ファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-243">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="a55e8-244">このコードは、DB に学生が存在するかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-244">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="a55e8-245">DB に学生が存在しない場合、DB にテスト データが入力されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-245">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="a55e8-246">`List<T>` コレクションではなく配列にテスト データを読み込み、パフォーマンスを最適化します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="a55e8-247">`EnsureCreated` メソッドは、DB のコンテキストに合わせて DB を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-247">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="a55e8-248">DB が存在する場合、`EnsureCreated` は DB を変更することなく戻ってきます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-248">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="a55e8-249">*Program.cs* で、次を実行するように `Main` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-249">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="a55e8-250">依存関係挿入コンテナーから DB コンテキスト インスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-250">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="a55e8-251">seed メソッドを呼び出し、コンテキストを渡します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="a55e8-252">seed メソッドが完了したら、コンテキストを破棄します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-252">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="a55e8-253">次は、更新された *Program.cs* ファイルのコードです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-253">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="a55e8-254">初めてアプリを実行すると、DB が作成され、テスト データが入力されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-254">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="a55e8-255">データ モデルが更新されたら、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="a55e8-255">When the data model is updated:</span></span>
* <span data-ttu-id="a55e8-256">DB を削除します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-256">Delete the DB.</span></span>
* <span data-ttu-id="a55e8-257">seed メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-257">Update the seed method.</span></span>
* <span data-ttu-id="a55e8-258">アプリを実行します。データが入力された新しい DB が作成されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-258">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="a55e8-259">後のチュートリアルでは、データ モデルが変わったとき、DB を削除して作り直すのではなく、DB を更新します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-259">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="a55e8-260">スキャフォールディング ツールを追加する</span><span class="sxs-lookup"><span data-stu-id="a55e8-260">Add scaffold tooling</span></span>

<span data-ttu-id="a55e8-261">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用し、Visual Studio Web コード生成パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-261">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="a55e8-262">スキャフォールディング エンジンを実行するには、このパッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="a55e8-262">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="a55e8-263">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-263">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="a55e8-264">パッケージ マネージャー コンソール (PMC) で、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-264">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="a55e8-265">先のコマンドでは、\*.csproj ファイルに NuGet パッケージが追加されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-265">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="a55e8-266">モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="a55e8-266">Scaffold the model</span></span>

* <span data-ttu-id="a55e8-267">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a55e8-268">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-268">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet tool install --global dotnet-aspnet-codegenerator --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

<span data-ttu-id="a55e8-269">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="a55e8-269">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="a55e8-270">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-270">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="a55e8-271">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="a55e8-271">Build the project.</span></span> <span data-ttu-id="a55e8-272">ビルドにより、次のようなエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-272">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="a55e8-273">`_context.Student` を `_context.Students` にグローバルに変更します (つまり、"s" を `Student` に追加します)。</span><span class="sxs-lookup"><span data-stu-id="a55e8-273">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="a55e8-274">7 回の出現が見つかり、更新されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-274">7 occurrences are found and updated.</span></span> <span data-ttu-id="a55e8-275">次のリリースで[このバグ](https://github.com/aspnet/Scaffolding/issues/633)を解決する予定です。</span><span class="sxs-lookup"><span data-stu-id="a55e8-275">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="a55e8-276">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="a55e8-276">Test the app</span></span>

<span data-ttu-id="a55e8-277">アプリを実行し、**[Students]\(学生\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-277">Run the app and select the **Students** link.</span></span> <span data-ttu-id="a55e8-278">ブラウザーの幅にもよりますが、**[Students]\(学生\)** リンクはページの一番上に表示されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-278">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="a55e8-279">**[Students]\(学生\)** リンクが表示されない場合、右上隅にあるナビゲーション アイコンをクリックしてください。</span><span class="sxs-lookup"><span data-stu-id="a55e8-279">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Contoso University のホーム ページ (ウィンドウ幅が狭いとき)](intro/_static/home-page-narrow.png)

<span data-ttu-id="a55e8-281">**[Create]\(作成\)**、**[Edit]\(編集\)**、**[Details]\(詳細\)** リンクをテストします。</span><span class="sxs-lookup"><span data-stu-id="a55e8-281">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="a55e8-282">DB を表示する</span><span class="sxs-lookup"><span data-stu-id="a55e8-282">View the DB</span></span>

<span data-ttu-id="a55e8-283">アプリが起動すると、`DbInitializer.Initialize` は `EnsureCreated` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-283">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="a55e8-284">`EnsureCreated` は DB の存在を検出し、必要に応じて DB を作成します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-284">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="a55e8-285">DB に学生が入っていない場合、`Initialize` メソッドによって学生が追加されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-285">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="a55e8-286">Visual Studio の **[表示]** メニューから **SQL Server オブジェクト エクスプローラー** (SSOX) を開きます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-286">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="a55e8-287">SSOX で、**(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a55e8-287">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="a55e8-288">**[Tables]\(テーブル\)** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-288">Expand the **Tables** node.</span></span>

<span data-ttu-id="a55e8-289">**[Student]\(学生\)** テーブルを右クリックし、**[View Data]\(データの表示\)** をクリックすると、作成された列とテーブルに挿入された行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-289">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="a55e8-290"><em>.mdf</em> ファイルと <em>.ldf</em> DB ファイルは <em>C:\Users\\<yourusername></em> フォルダーにあります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-290">The <em>.mdf</em> and <em>.ldf</em> DB files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="a55e8-291">アプリの起動時に `EnsureCreated` が呼び出され、次のワークフローが可能になります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-291">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="a55e8-292">DB を削除します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-292">Delete the DB.</span></span>
* <span data-ttu-id="a55e8-293">DB スキーマを変更します (たとえば、`EmailAddress` フィールドを追加します)。</span><span class="sxs-lookup"><span data-stu-id="a55e8-293">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="a55e8-294">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-294">Run the app.</span></span>

<span data-ttu-id="a55e8-295">`EnsureCreated` によって、`EmailAddress` 列を含む DB が作成されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-295">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="a55e8-296">規約</span><span class="sxs-lookup"><span data-stu-id="a55e8-296">Conventions</span></span>

<span data-ttu-id="a55e8-297">規約を利用することや EF Core で想定が行われることで、EF Core が完全な DB を作成するために記述しなければならないコードの量は最小限に抑えられます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-297">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="a55e8-298">`DbSet` プロパティの名前がテーブル名として使用されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-298">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="a55e8-299">`DbSet` プロパティによって参照されないエンティティについては、エンティティ クラス名がテーブル名として使用されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-299">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="a55e8-300">列名には、エンティティ プロパティ名が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-300">Entity property names are used for column names.</span></span>

* <span data-ttu-id="a55e8-301">ID または classnameID という名前が付けられているエンティティ プロパティは主キーのプロパティとして認識されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-301">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="a55e8-302">*<navigation property name><primary key property name>* という名前が付いている場合、プロパティは外部キー プロパティとして解釈されます。たとえば、`Student` ナビゲーション プロパティの `StudentID` です。`Student` エンティティの主キーが `ID` であるためです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-302">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="a55e8-303">外部キーには「*<primary key property name>*」という名前を付けることができます。たとえば、`EnrollmentID` です。`Enrollment` エンティティの主キーが `EnrollmentID` であるためです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-303">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="a55e8-304">規約の動作はオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-304">Conventional behavior can be overridden.</span></span> <span data-ttu-id="a55e8-305">たとえば、このチュートリアルで先に示したように、テーブル名を明示的に指定できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-305">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="a55e8-306">列名を明示的に設定できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-306">The column names can be explicitly set.</span></span> <span data-ttu-id="a55e8-307">主キーと外部キーを明示的に設定できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-307">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="a55e8-308">非同期コード</span><span class="sxs-lookup"><span data-stu-id="a55e8-308">Asynchronous code</span></span>

<span data-ttu-id="a55e8-309">ASP.NET Core と EF Core では、非同期プログラミングが既定のモードです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-309">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="a55e8-310">Web サーバーでは、利用できるスレッド数に限りがあります。負荷が高い状況では、利用できるスレッドが全部使われる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-310">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="a55e8-311">その場合、スレッドが解放されるまでサーバーは新しい要求を処理できません。</span><span class="sxs-lookup"><span data-stu-id="a55e8-311">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="a55e8-312">同期コードの場合、たくさんのスレッドが関連付けられていても、I/O の完了を待っているため、実際には何の作業も行っていないということがあります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-312">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="a55e8-313">非同期コードの場合、あるプロセスが I/O の完了を待っているとき、そのスレッドは解放され、サーバーによって他の要求の処理に利用できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-313">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="a55e8-314">結果として、非同期コードの場合、サーバー リソースをより効率的に利用できます。サーバーは、より多くのトラフィックを遅延なく処理できます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-314">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="a55e8-315">非同期コードが実行時に発生させるオーバーヘッドは少量です。</span><span class="sxs-lookup"><span data-stu-id="a55e8-315">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="a55e8-316">トラフィックが少ない場合、パフォーマンスに与える影響は無視して構わない程度です。トラフィックが多い場合、相当なパフォーマンス改善が見込まれます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-316">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="a55e8-317">次のコードでは、キーワード `async`、戻り値 `Task<T>`、キーワード `await`、メソッド `ToListAsync` によりコードの実行が非同期になります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-317">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="a55e8-318">キーワード `async` は次のことをコンパイラに指示します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-318">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="a55e8-319">メソッド本文の一部にコールバックを生成する。</span><span class="sxs-lookup"><span data-stu-id="a55e8-319">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="a55e8-320">返された [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) オブジェクトを自動作成する。</span><span class="sxs-lookup"><span data-stu-id="a55e8-320">Automatically create the [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="a55e8-321">詳細については、[Task の戻り値の型](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)に関する記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a55e8-321">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="a55e8-322">暗黙の戻り値の型 `Task` は進行中の作業を表します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-322">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="a55e8-323">キーワード `await` により、コンパイラはメソッドを 2 つに分割します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-323">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="a55e8-324">最初の部分は、非同期で開始される操作で終わります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-324">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="a55e8-325">2 つ目の部分は、操作の完了時に呼び出されるコールバック メソッドに入ります。</span><span class="sxs-lookup"><span data-stu-id="a55e8-325">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="a55e8-326">`ToListAsync` は、`ToList` 拡張メソッドの非同期バージョンです。</span><span class="sxs-lookup"><span data-stu-id="a55e8-326">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="a55e8-327">EF Core を利用する非同期コードの記述で注意すべき点:</span><span class="sxs-lookup"><span data-stu-id="a55e8-327">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="a55e8-328">クエリやコマンドを DB に送信するステートメントのみが非同期で実行されます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-328">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="a55e8-329">これには、`ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync`、`SaveChangesAsync` が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a55e8-329">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="a55e8-330">`var students = context.Students.Where(s => s.LastName == "Davolio")` など、`IQueryable` を変更するだけのステートメントは含まれません。</span><span class="sxs-lookup"><span data-stu-id="a55e8-330">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="a55e8-331">EF Core コンテキストはスレッド セーフではありません。複数の操作を並列実行しないでください。</span><span class="sxs-lookup"><span data-stu-id="a55e8-331">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="a55e8-332">非同期コードのパフォーマンス上の利点を最大限に活用するには、クエリをデータベースに送信させる EF Core メソッドを (ページングなどのための) ライブラリ パッケージで呼び出す場合、そのライブラリ パッケージで非同期が利用されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-332">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="a55e8-333">.NET の非同期プログラミングについては、「[非同期の概要](/dotnet/articles/standard/async)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a55e8-333">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="a55e8-334">次のチュートリアルでは、基本的な CRUD (作成、読み取り、更新、削除) の操作について説明します。</span><span class="sxs-lookup"><span data-stu-id="a55e8-334">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a55e8-335">次へ</span><span class="sxs-lookup"><span data-stu-id="a55e8-335">Next</span></span>](xref:data/ef-rp/crud)
