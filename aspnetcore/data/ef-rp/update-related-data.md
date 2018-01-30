---
title: "Razor ページの EF コアの関連データ - 8 個の更新します。"
author: rick-anderson
description: "このチュートリアルでは外部キー フィールドとナビゲーション プロパティを更新することによって関連するデータを更新します。"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 5c91c91ab938f3aa4abc55049c54f399469f6163
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>関連するデータの EF コア Razor ページ (8 の 7) の更新

によって[Tom Dykstra](https://github.com/tdykstra)、および[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

このチュートリアルでは、関連するデータの更新を示します。 問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7)です。

次の図は、完了したページの一部を示します。

![コースの編集 ページ](update-related-data/_static/course-edit.png)
![インストラクターの編集 ページ](update-related-data/_static/instructor-edit-courses.png)

確認し、テストを作成および編集コース ページ。 新しいコースを作成します。 その主キー (整数)、名ではなく、部門が選択されます。 新しいコースを編集します。 テストが完了したら、新しいコースを削除します。

## <a name="create-a-base-class-to-share-common-code"></a>一般的なコードを共有する基本クラスを作成します。

コース/作成およびコース/編集のそれぞれのページには、部門名の一覧が必要があります。 作成、 *Pages/Courses/DepartmentNamePageModel.cshtml.cs*作成および編集するページの基本クラス。

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

上記のコードを作成、 [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0)部門名の一覧を格納します。 場合`selectedDepartment`を指定すると、その部門が選択されて、`SelectList`です。

作成および編集ページのモデル クラスが派生`DepartmentNamePageModel`です。

## <a name="customize-the-courses-pages"></a>コースのページをカスタマイズします。

コースの新しいエンティティが作成されると、既存の部署とのリレーションシップが必要です。 コースを作成するときに、部署を追加するには、基本クラス作成および編集するにはには、部門を選択するためのドロップダウン リストが含まれています。 ドロップダウン リストを設定、`Course.DepartmentID`外部キー (FK) プロパティです。 EF コアを使用して、`Course.DepartmentID`を読み込む FK、`Department`ナビゲーション プロパティ。

![コースを作成します。](update-related-data/_static/ddl.png)

次のコードで、モデルの作成 ページを更新します。

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

上のコードでは以下の操作が行われます。

* `DepartmentNamePageModel` から派生します。
* 使用して`TryUpdateModelAsync`を防ぐために[過剰ポスティング](xref:data/ef-rp/crud#overposting)です。
* 置換`ViewData["DepartmentID"]`で`DepartmentNameSL`(から基底クラス)。

`ViewData["DepartmentID"]`交換が厳密に型指定と`DepartmentNameSL`です。 厳密に型指定されたモデルは、厳密に型指定経由で優先されます。 詳細については、次を参照してください。[弱く型指定されたデータ (ViewData および ViewBag)](xref:mvc/views/overview#VD_VB)です。

### <a name="update-the-courses-create-page"></a>更新のコースを作成 ページ

更新*Pages/Courses/Create.cshtml*次のマークアップ。

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

上記のマークアップでは、次の変更を加えます。

* キャプションを変更**DepartmentID**に**部門**です。
* 置換`"ViewBag.DepartmentID"`で`DepartmentNameSL`(から基底クラス)。
* "選択 Department"オプションを追加します。 この変更は、最初の部署ではなく「部署の選択」を表示します。
* 部門が選択されていない場合は、検証メッセージを追加します。

Razor ページを使用して、[タグ ヘルパーの選択](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

[作成] ページをテストします。 [作成] ページは、部門 ID ではなく、部門名が表示されます。

### <a name="update-the-courses-edit-page"></a>コースの編集 ページを更新します。

次のコード編集ページのモデルを更新します。

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

変更は、モデルの作成 ページで行われたものと似ています。 上記のコードで`PopulateDepartmentsDropDownList`パス部門 ID のドロップダウン リストで指定された部門を選択します。

更新*Pages/Courses/Edit.cshtml*次のマークアップ。

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

上記のマークアップでは、次の変更を加えます。

* コース ID を表示します 一般に、エンティティの主キー (PK) は表示されません。 Pk がユーザーに意味をなしません。 この例では、主キーは、コース数です。
* キャプションを変更**DepartmentID**に**部門**です。
* 置換`"ViewBag.DepartmentID"`で`DepartmentNameSL`(から基底クラス)。
* "選択 Department"オプションを追加します。 この変更は、最初の部署ではなく「部署の選択」を表示します。
* 部門が選択されていない場合は、検証メッセージを追加します。

ページには、非表示フィールドが含まれています (`<input type="hidden">`) コース数。 追加する、`<label>`タグ ヘルパーを`asp-for="Course.CourseID"`隠しフィールドの必要性を排除しません。 `<input type="hidden">`ユーザーがクリックしたときに、ポストされたデータに含まれるコース番号に必要な**保存**です。

更新されたコードをテストします。 作成、編集、およびコースを削除します。

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>詳細を AsNoTracking を追加してページ モデルの削除

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__)追跡が必要でない場合、パフォーマンスを向上させることができます。 追加`AsNoTracking`Delete と詳細ページのモデルにします。 次のコードは、更新の削除 ページのモデルを示しています。

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

更新プログラム、`OnGetAsync`メソッドで、 *Pages/Courses/Details.cshtml.cs*ファイル。

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>削除と詳細のページを変更します。

次のマークアップを削除 Razor ページを更新します。

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

[詳細] ページに同じ変更を行います。

### <a name="test-the-course-pages"></a>コース ページをテストします。

テストを作成、編集、詳細、および削除します。

## <a name="update-the-instructor-pages"></a>講師ページを更新します。

次のセクションでは、インストラクター ページを更新します。

### <a name="add-office-location"></a>オフィスの場所を追加します。

講師レコードを編集するには、インストラクターのオフィス割り当てを更新することがあります。 `Instructor`エンティティと 0 または 1 を 1 つの関係には、`OfficeAssignment`エンティティです。 講師コードを処理する必要があります。

* ユーザーがオフィス割り当てをクリアする場合は、削除、`OfficeAssignment`エンティティです。
* 場合は、ユーザーがオフィス割り当てを入力し、空か、作成、新しい`OfficeAssignment`エンティティです。
* ユーザーは、office の割り当てを変更する場合は、更新、`OfficeAssignment`エンティティです。

次のコードで講習においてインストラクター編集ページのモデルを更新します。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

上のコードでは以下の操作が行われます。

- 現在の取得`Instructor`の一括読み込みを使用して、データベースからのエンティティ、`OfficeAssignment`ナビゲーション プロパティ。
- 更新、取得した`Instructor`モデル バインダーから値を持つエンティティ。 `TryUpdateModel`により、[過剰ポスティング](xref:data/ef-rp/crud#overposting)です。
- オフィスの場所が空白の場合は、設定`Instructor.OfficeAssignment`を null にします。 ときに`Instructor.OfficeAssignment`は null にすると、関連する行で、`OfficeAssignment`テーブルを削除します。

### <a name="update-the-instructor-edit-page"></a>講師の編集 ページを更新します。

更新*Pages/Instructors/Edit.cshtml*オフィスの場所で。

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

講習においてインストラクター オフィスの場所を変更することを確認します。

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>コースの課題をインストラクターの編集 ページに追加します。

講習においてインストラクターには、コースの任意の数を教えることがあります。 このセクションでは、コースの割り当てを変更する機能を追加します。 次の図は、更新されたインストラクターの編集 ページを示しています。

![コースをインストラクターの編集 ページ](update-related-data/_static/instructor-edit-courses.png)

`Course`および`Instructor`は多対多リレーションシップを持ちます。 追加し、リレーションシップを削除、追加または削除を行うエンティティから、`CourseAssignments`エンティティ セットに参加します。

チェック ボックスは、コースに割り当てられている、インストラクターへの変更を有効にします。 データベース内のすべてのコースにチェック ボックスが表示されます。 割り当てられているインストラクター コースがチェックされます。 ユーザーでは、選択したり、コースの割り当てを変更する チェック ボックスをオフにすることができます。 コースの数が非常に大きい: 場合

* おそらく、コースを表示するのに別のユーザー インターフェイスを使用します。
* リレーションシップを作成または削除の結合エンティティを操作するためのメソッドは変更されません。

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>サポートするクラスを追加作成し、インストラクター ページの編集

作成*SchoolViewModels/AssignedCourseData.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData`クラスには、あるインストラクターによって割り当てられたコースのチェック ボックスを作成するデータが含まれています。

作成、 *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*基本クラス。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel`基本クラスで、編集を使用してページ モデルを作成します。 `PopulateAssignedCourseData`すべてを読み込み`Course`を設定するエンティティ`AssignedCourseDataList`です。 各コースに、設定するコードを`CourseID`タイトル、および、インストラクター コースに割り当てられるかどうか。 A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1)効率的な参照を作成するために使用します。

### <a name="instructors-edit-page-model"></a>講習においてインストラクター編集ページのモデル

次のコードで、インストラクター編集ページのモデルを更新します。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

上記のコードでは、office の割り当て変更を処理します。

講師 Razor ビューを更新します。

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Visual Studio でコードを貼り付けるときに改行コードを中断するように変更されます。 Ctrl + Z を 1 回押して、オート フォーマットを元に戻します。 Ctrl + Z は、ここに表示するように表示されるように、改行を修正します。 インデント完璧なのに、する必要はありませんが、 `@</tr><tr>`、 `@:<td>`、 `@:</td>`、および`@:</tr>`行ことはできません。 1 行に示すようにします。 選択された新しいコードのブロックして、Tab キーを押して 3 回、新しいコードと既存のコードの行にします。 投票またはこのバグの状態を確認[で次のリンク](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html)です。

上記のコードでは、次の 3 つの列を含んでいる HTML テーブルを作成します。 各列には、チェック ボックスとコース番号とタイトルを含むキャプション。 すべてのチェック ボックスは、同じ名前 ("selectedCourses") を持ちます。 同じ名前を使用するには、グループとして扱われるようにする、モデル バインダーが通知されます。 各チェック ボックスの値の属性に設定されて`CourseID`です。 モデル バインダーがで構成される配列を渡しますページがポストされるときに、`CourseID`のチェック ボックスだけが選択されている値。

チェック ボックスが最初に表示されているコース インストラクターに割り当てられている属性がチェックされます。

アプリを実行し、更新された講習においてインストラクターの編集 ページをテストします。 一部のコース割り当てを変更します。 インデックス ページには、変更が反映されます。

注: インストラクター コースのデータを編集するのには、ここに採用されているアプローチは、コースの数に制限がある場合にも動作します。 非常に大きいコレクションを別の UI と更新の別の方法になりますより使用可能かつ効率的です。

### <a name="update-the-instructors-create-page"></a>講習においてインストラクター作成ページを更新します。

モデルを更新するインストラクター作成ページを次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

上記のコードがに似ていますが、 *Pages/Instructors/Edit.cshtml.cs*コード。

次のマークアップ、インストラクター作成 Razor ページを更新します。

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

講師の作成 ページをテストします。

## <a name="update-the-delete-page"></a>更新プログラムの削除 ページ

次のコードで Delete ページ モデルを更新します。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

上記のコードでは、次の変更を加えます。

* 一括読み込みを使用して、`CourseAssignments`ナビゲーション プロパティ。 `CourseAssignments`含める必要があるインストラクターが削除されたときに削除されないまたはします。 それらを読み取る必要はありませんを回避するのには、データベースで連鎖削除を構成します。

* 講師が削除されますが、部門の管理者として割り当てられている場合は、その部門からインストラクター割り当てを削除します。

>[!div class="step-by-step"]
[前へ](xref:data/ef-rp/read-related-data)
[次へ](xref:data/ef-rp/concurrency)
