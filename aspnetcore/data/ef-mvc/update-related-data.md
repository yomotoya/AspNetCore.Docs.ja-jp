---
title: "ASP.NET MVC を持つコアを EF Core - 更新プログラムに関連したデータ - 10 の 7"
author: tdykstra
description: "このチュートリアルでは外部キー フィールドとナビゲーション プロパティを更新することによって関連するデータを更新します。"
keywords: "ASP.NET Core、Entity Framework Core では、関連するデータの結合します。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 67bd162b-bfb7-4750-9e7f-705228b5288c
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: daf6dd8024863e02e40ad002a0a7da388f5a2ec7
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a>関連するデータの EF コア ASP.NET Core MVC のチュートリアル (10 の 7) での更新

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。

前のチュートリアルでは、関連するデータが表示されます。このチュートリアルでは外部キー フィールドとナビゲーション プロパティを更新することによって関連するデータを更新します。

次の図を使って作業するページの一部を表示します。

![コースの編集 ページ](update-related-data/_static/course-edit.png)

![講師の編集 ページ](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>コースを作成および編集ページをカスタマイズします。

コースの新しいエンティティが作成されると、既存の部署とのリレーションシップが必要です。 この作業を容易にスキャフォールディング コードには、コント ローラーのメソッドおよび部門を選択するためのドロップダウン リストを含むビューを作成および編集が含まれています。 ドロップダウン リストを設定、`Course.DepartmentID`外部キーのプロパティを読み込むために、Entity Framework が必要なすべてが、`Department`適切な部門 エンティティのナビゲーション プロパティ。 スキャフォールディングのコードを使用することが、エラー処理を追加し、ドロップダウン リストを並べ替えるには少し変更することもできます。

*CoursesController.cs*、4 つの作成および編集する方法を削除し、次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

後に、 `Edit` HttpPost メソッドをドロップダウン リストの部署の情報を読み込む新しいメソッドを作成します。

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

`PopulateDepartmentsDropDownList`メソッド名でソートのすべての部門の一覧を取得、作成、`SelectList`ドロップダウン一覧は、コレクション内のビューに、コレクションを渡すと`ViewBag`です。 このメソッドは、省略可能な受け取ります`selectedDepartment`パラメーターをドロップダウン リストが表示される場合に選択される項目を指定する呼び出し元のコードを使用します。 ビューが名前"DepartmentID"に渡す、`<select>`とヘルパーのタグ ヘルパーに渡し、ファイルの場所を知っているし、`ViewBag`オブジェクトに対する、 `SelectList` "DepartmentID"という名前です。

HttpGet`Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択したアイテムを設定しなくてもメソッド。

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

HttpGet`Edit`メソッドは編集されているコースに既に割り当てられている部門の ID に基づいて、選択したアイテムを設定します。

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

両方の HttpPost メソッド`Create`と`Edit`もエラーが発生したページを再表示するときに、選択した項目を設定するコードが含まれます。 これにより、エラー メッセージを表示する、ページが表示され、ときにどのような部門が選択されている選択されたままです。

### <a name="add-asnotracking-to-details-and-delete-methods"></a>追加します。詳細を AsNoTracking および Delete メソッド

コースの詳細と Delete のページのパフォーマンスを最適化するには追加`AsNoTracking`で呼び出し、`Details`と HttpGet`Delete`メソッドです。

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>コースのビューを変更します。

*Views/Courses/Create.cshtml*、"Select Department"オプションを追加、**部門**ドロップダウン リストで、変更のキャプションの変更**DepartmentID**に**部門**、検証メッセージを追加します。

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

*Views/Courses/Edit.cshtml*、部門フィールドだけで行ったの同じ変更を加える*Create.cshtml*です。

また、 *Views/Courses/Edit.cshtml*、コースの前に数値フィールドを追加、**タイトル**フィールドです。 コース番号は、主キーであるためが表示されますが、変更することはできません。

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

非表示フィールドが既に存在 (`<input type="hidden">`) の編集ビューでコース番号。 追加する、 `<label>` 、ユーザーがクリックしたときに、ポストされたデータに含まれるコース番号が原因であるために、タグ ヘルパーは隠しフィールドの必要性を排除しない**保存**上、**編集**ページ。

*Views/Courses/Delete.cshtml*上部にある一連の数値フィールドを追加および部署名を部門 ID を変更します。

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

*Views/Courses/Details.cshtml*、だけで実行したの同じ変更を加えます*Delete.cshtml*です。

### <a name="test-the-course-pages"></a>コース ページをテストします。

アプリを実行する、選択、**コース** タブで、をクリックして**新規作成**、新しいコースのデータを入力。

![コースの作成 ページ](update-related-data/_static/course-create.png)


              **[作成]**をクリックします。 一覧に追加された新しいコース コース インデックス ページが表示されます。 インデックス ページの一覧で、部門名を示すリレーションシップが正常に確立されているナビゲーション プロパティに由来します。

をクリックして**編集**コース インデックス ページ内のコースをします。

![コースの編集 ページ](update-related-data/_static/course-edit.png)

ページ上のデータを変更し、クリックして**保存**です。 コース インデックス ページには、更新された一連のデータが表示されます。

## <a name="add-an-edit-page-for-instructors"></a>講習においてインストラクター編集ページを追加します。

講師レコードを編集するときに、インストラクター オフィス割り当てを更新することにします。 Instructor エンティティには、OfficeAssignment エンティティが、コードには、次の状況を処理することを意味する 0 または 1 を 1 つのリレーションシップがあります。

* 場合は、ユーザーがオフィス割り当てをクリアし、値が最初にあった、OfficeAssignment エンティティを削除します。

* ユーザーが office 代入値を入力し、空か最初場合、は、新しい OfficeAssignment エンティティを作成します。

* ユーザーは、office の代入式の値を変更する場合は、OfficeAssignment の既存のエンティティの値を変更します。

### <a name="update-the-instructors-controller"></a>講習においてインストラクター コント ローラーを更新します。

*InstructorsController.cs*、コード、HttpGet を変更します`Edit`メソッドがロード Instructor エンティティのため`OfficeAssignment`ナビゲーション プロパティと呼び出し`AsNoTracking`:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

置換、HttpPost `Edit` office 割り当ての更新を処理する次のコードにメソッド。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

コードは次のとおり

-  メソッドの名前を変更`EditPost`署名は、ここでは、HttpGet と同じため`Edit`メソッド (、`ActionName`属性を指定する、 `/Edit/` URL はまだ使用)。

-  一括読み込みのデータベースを使用して、現在 Instructor エンティティを取得、`OfficeAssignment`ナビゲーション プロパティ。 これは、HttpGet で行ったのと同じ`Edit`メソッドです。

-  モデル バインダーから値を取得した Instructor エンティティを更新します。 `TryUpdateModel`オーバー ロードでは、ホワイト リストに追加するプロパティです。 こうすれば、過剰な投稿で説明するよう、 [2 番目のチュートリアル](crud.md)です。

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   オフィスの場所が空白の場合は、null OfficeAssignment の表に、関連する行が削除されるように Instructor.OfficeAssignment プロパティを設定します。

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- データベースに変更を保存します。

### <a name="update-the-instructor-edit-view"></a>更新プログラム、インストラクター ビューの編集

*Views/Instructors/Edit.cshtml*、新しいフィールドを追加する前に最後に、オフィスの場所を編集、**保存**ボタンをクリックします。

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

アプリを実行する、選択、**講習においてインストラクター**  タブをクリックして**編集**インストラクターにします。 変更、**オフィス** をクリック**保存**です。

![講師の編集 ページ](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>コースの課題をインストラクターの編集 ページに追加します。

講習においてインストラクターには、コースの任意の数を教えることがあります。 次のスクリーン ショットに示すように、チェック ボックスのグループを使用して、course 割り当てを変更する機能を追加することでインストラクターの編集 ページを拡張します。

![コースをインストラクターの編集 ページ](update-related-data/_static/instructor-edit-courses.png)

コース、インストラクター エンティティ間のリレーションシップは、多対多です。 追加し、リレーションシップを削除、追加し、CourseAssignments 結合エンティティ セットからエンティティを削除します。

UI コースを変更することができるインストラクターに割り当てられているチェック ボックスのグループです。 データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているものを選択します。 ユーザーでは、選択したり、コースの割り当てを変更する チェック ボックスをオフにすることができます。 コースの数が非常に大きい場合は、他のビューでは、データの表示方法を使用することはおそらくはメソッドを使用する、同じ結合エンティティを操作するためのリレーションシップを作成または削除します。

### <a name="update-the-instructors-controller"></a>講習においてインストラクター コント ローラーを更新します。

チェック ボックスの一覧については、ビューにデータを提供するには、ビュー モデル クラスを使用します。

作成*AssignedCourseData.cs*で、 *SchoolViewModels*フォルダーと、既存のコードを次のコードを置換します。

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

*InstructorsController.cs*、置換、HttpGet`Edit`メソッドを次のコード。 変更が強調表示されます。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

コードでは、追加の一括読み込み、`Courses`ナビゲーション プロパティ、新しいを呼び出すと`PopulateAssignedCourseData` チェック ボックスを配列の使用に関する情報を提供するメソッドを`AssignedCourseData`モデル クラスを表示します。

内のコード、`PopulateAssignedCourseData`メソッドをビュー モデル クラスを使用してコースの一覧を読み込むためにすべてのコース エンティティを読み取ります。 コードが、インストラクターにコースが存在するかどうかをチェック各コースに`Courses`ナビゲーション プロパティ。 コースをインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、`HashSet`コレクション。 `Assigned`プロパティに設定されているコースの場合は true、インストラクターに割り当てられます。 ビューは、このプロパティを使用して、どのチェック ボックスとして表示する選択を確認されます。 最後に、一覧は、ビューに渡される`ViewData`です。

次に、ユーザーがクリックしたときに実行されるコードを追加**保存**です。 置換、`EditPost`メソッドに次のコードし、更新する新しいメソッドを追加、 `Courses` Instructor エンティティのナビゲーション プロパティ。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

メソッドのシグネチャは、HttpGet を異なるようになりました`Edit`メソッド、メソッド名の変更から`EditPost`に`Edit`です。

モデル バインダーを自動的に更新できないビューでは、コース エンティティのコレクションがあるないため、`CourseAssignments`ナビゲーション プロパティ。 モデル バインダーを使用して更新するのではなく、`CourseAssignments`ナビゲーション プロパティ、新しいすれば`UpdateInstructorCourses`メソッドです。 除外する必要があるため、`CourseAssignments`モデル バインドからのプロパティです。 これを呼び出すコードの変更が必要としない`TryUpdateModel`ホワイト リストをオーバー ロードを使用しているためと`CourseAssignments`は include リストではありません。

チェックを行わないボックスを選択した場合のコード場合、`UpdateInstructorCourses`を初期化、`CourseAssignments`ナビゲーション プロパティが空のコレクションを返します。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

コードは、データベース内のすべてのコースをループ処理し、ビューで選択されたものではなくインストラクターに現在割り当てられているものに対しては、各コースを確認します。 効率的な検索を容易に後者の 2 つのコレクションが格納されている`HashSet`オブジェクト。

コースのチェック ボックスが選択されましたが、コースがにないかどうか、`Instructor.CourseAssignments`ナビゲーション プロパティ、コースはナビゲーション プロパティで、コレクションに追加します。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

コースのチェック ボックスが選択されますが、コースは場合、`Instructor.CourseAssignments`コースのナビゲーション プロパティは、ナビゲーション プロパティから削除します。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>講師ビューを更新します。

*Views/Instructors/Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを次を追加することで配列にコードの直後に、`div`用の要素、 **Office**フィールドとの前に、`div`要素を**保存**ボタンをクリックします。

<a id="notepad"></a>
> [!NOTE] 
> Visual Studio でコードを貼り付けるときに、改行は、コードを中断するように変更されます。  Ctrl + Z を 1 回押して、オート フォーマットを元に戻します。  ここで分かるように表示されるよう、改行は解決します。 インデント完璧なのに、する必要はありませんが、 `@</tr><tr>`、 `@:<td>`、 `@:</td>`、および`@:</tr>`行ことはできません。 1 行に示すように、または実行時エラーが表示されます。 選択された新しいコードのブロックして、Tab キーを押して 3 回、新しいコードと既存のコードの行にします。

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

このコードでは、次の 3 つの列を含んでいる HTML テーブルを作成します。 各列ではコースの番号とタイトルから構成されるキャプションを続けて チェック ボックスです。 すべてのチェック ボックスをグループとして扱う場合にモデル バインダーを通知する同じ名前 ("selectedCourses") であります。 各チェック ボックスの値の属性がの値に設定`CourseID`です。 モデル バインダーがコント ローラーで構成される配列を渡しますページがポストされるときに、`CourseID`のチェック ボックスが選択されている値。

チェック ボックスが最初に表示されると、適合インストラクターに割り当てられているコースのチェックが属性、選択を (それらのチェックが表示される) にあります。

アプリを実行する、選択、**講習においてインストラクター**タブをクリックし、をクリックして**編集**を表示するには、あるインストラクターに、**編集**ページ。

![コースをインストラクターの編集 ページ](update-related-data/_static/instructor-edit-courses.png)

一部のコース割り当てを変更し、[保存] をクリックします。 インデックス ページには、加えた変更が反映されます。

> [!NOTE] 
> 講師コースのデータを編集するのには、ここに採用されているアプローチは、コースの数に制限がある場合にも動作します。 非常に大きいコレクションを別の UI と更新の別の方法が必要になります。

## <a name="update-the-delete-page"></a>更新プログラムの削除 ページ

*InstructorsController.cs*、削除、`DeleteConfirmed`代わりに、次のコードにメソッドおよび挿入します。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

このコードは、次の変更を加えます。

* 読み込みが eager、`CourseAssignments`ナビゲーション プロパティ。  これを含めることも EF 認識しないので関連`CourseAssignment`エンティティし、削除されません。  通すしなくても済むようにここでする可能性があります連鎖削除データベースを構成します。

* 講師が削除されますが、部門の管理者として割り当てられている場合は、その部門からインストラクター割り当てを削除します。

## <a name="add-office-location-and-courses-to-the-create-page"></a>オフィスの場所およびコースを作成 ページに追加します。

*InstructorsController.cs*、HttpPost、HttpGet を削除`Create`メソッド、代わりに、次のコードを追加します。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

このコードはの表示に似ています、`Edit`コースは最初にありません以外の方法を選択します。 HttpGet`Create`メソッドの呼び出し、`PopulateAssignedCourseData`メソッド コースが選択されている場合がある可能性がありますのでない注文の空のコレクションを提供する、 `foreach` (それ以外の場合、コードの表示は、例外をスロー null 参照)、ビューでループします。

HttpPost`Create`メソッドを選択した各コースは追加、`CourseAssignments`ナビゲーション プロパティの検証エラーをチェックして、データベースに新しいインストラクターを追加する前にします。 行ったコース選択内容を自動的に復元 (例については、無効な日付をキーとしたユーザー)、モデル エラーがあるし、エラー メッセージで、ページが表示され、モデル エラーがある場合でも、コースが追加されます。

コースを追加できるようにすることを確認、`CourseAssignments`ナビゲーション プロパティが空のコレクションとしてプロパティを初期化する必要があります。

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

コント ローラーのコードでこれを行う代わりに、としてでしたで行うこと、インストラクター モデルを自動的に作成、コレクションが存在しない場合、次の例に示すようにプロパティ get アクセス操作子を変更することで。

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

変更する場合、`CourseAssignments`プロパティこの方法では、コント ローラーの明示的なプロパティの初期化コードを削除することができます。

*Views/Instructor/Create.cshtml*オフィスの場所テキスト ボックスを追加、および送信ボタンの前にコースのチェック ボックスをします。 編集ページの場合、[貼り付けをすると、Visual Studio が、コードを再フォーマットする場合、書式設定を修正](#notepad)です。

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

アプリケーションを実行して、インストラクターを作成してテストします。 

## <a name="handling-transactions"></a>トランザクションの処理

説明に従って、 [CRUD チュートリアル](crud.md)、Entity Framework は、トランザクションを暗黙的に実装します。 必要な複数を制御する--など--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクション](https://docs.microsoft.com/ef/core/saving/transactions)です。

## <a name="summary"></a>概要

関連するデータの操作の概要が完了しました。 次のチュートリアルでは、同時実行の競合を処理する方法が表示されます。

>[!div class="step-by-step"]
[前へ](read-related-data.md)
[次へ](concurrency.md)  
