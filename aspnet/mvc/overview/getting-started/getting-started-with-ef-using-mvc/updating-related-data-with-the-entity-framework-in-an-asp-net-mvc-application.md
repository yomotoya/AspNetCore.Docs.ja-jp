---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'チュートリアル: ASP.NET MVC アプリで EF で関連データを更新します。'
description: このチュートリアルでは、関連するデータを更新します。 ほとんどのリレーションシップは、これは外部キー フィールドまたはナビゲーション プロパティを更新することで行うことができます。
author: tdykstra
ms.author: riande
ms.date: 01/19/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1ef4242ff3bd1dd86f4d58bd04ba08e8b90fdaa4
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248278"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>ASP.NET MVC アプリケーションで Entity Framework で関連データの更新
====================


# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a>チュートリアル: ASP.NET MVC アプリで EF で関連データを更新します。

前のチュートリアルでは、関連するデータが表示されます。 このチュートリアルでは、関連するデータを更新します。 ほとんどのリレーションシップは、これは外部キー フィールドまたはナビゲーション プロパティを更新することで行うことができます。 多対多のリレーションシップで Entity Framework は結合テーブルを直接公開を追加し、該当するナビゲーション プロパティからエンティティを削除するようにします。

以下の図は、使用するページの一部を示しています。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![コースで講師の編集](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * Courses ページをカスタマイズします。
> * Instructors ページに office を追加します。
> * Instructors ページにコースを追加します。
> * DeleteConfirmed を更新します。
> * オフィスの場所とコースを Create ページに追加する

## <a name="prerequisites"></a>必須コンポーネント

* [関連データの読み取り](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a>Courses ページをカスタマイズします。

新しいコース エンティティが作成されると、既存の部門とのリレーションシップが必要になります。 これを容易にするため、スキャフォールディング コードには、コントローラーのメソッドと、部門を選択するためのドロップダウン リストを含む Create ビューと Edit ビューが含まれます。 ドロップダウン リストのセット、`Course.DepartmentID`外部キー プロパティは、Entity Framework は、読み込むために必要なすべて、`Department`ナビゲーション プロパティを適切な`Department`エンティティ。 このスキャフォールディング コードを使用しますが、エラー処理を追加し、ドロップダウン リストを並べ替えるために少し変更します。

*CourseController.cs*、4 つの削除`Create`と`Edit`メソッドし、次のコードに置き換えます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

次の追加`using`ファイルの先頭にあるステートメント。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList`メソッドの名前で並べ替えたすべての部門の一覧を取得、作成、`SelectList`のドロップダウン リストでは、コレクション ビューに、コレクションを渡すと、`ViewBag`プロパティ。 このメソッドは、ドロップダウン リストがレンダリングされるときに選択される項目を指定するためのコード呼び出しを許可する、省略可能な `selectedDepartment` パラメーターを受け取ります。 ビューが名前を渡す`DepartmentID`を[DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper、およびヘルパー ファイルの場所を認識し、`ViewBag`オブジェクト、`SelectList`という名前`DepartmentID`します。

`HttpGet` `Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択した項目を設定せずメソッド。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit`メソッドが既に編集中のコースに割り当てられている部門の ID に基づいて、選択した項目を設定します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost`両方のメソッド`Create`と`Edit`も、ページを再表示エラーが発生したときに、選択した項目を設定するコードが含まれます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

このコードによりエラー メッセージを表示、ページが表示されときに、選択した常時されます部門が選択されているようになります。

Course ビューは、department フィールドのドロップダウン リストで既にスキャフォールディングされたが、次の強調表示されていることを変更するため、このフィールドでは、DepartmentID キャプションをたくない、 *Views\Course\Create.cshtml*ファイルキャプションを変更します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

同じ変更を行う*Views\Course\Edit.cshtml*します。

通常、スキャフォールダーの主キー スキャフォールディングは、キー値データベースによって生成される変更ことはできず、ユーザーに表示する意味のある値はありません。 Course エンティティの場合、スキャフォルダーは、テキスト ボックスは含ま、`CourseID`フィールドを認識するため、`DatabaseGeneratedOption.None`属性は、ユーザーができることを意味主キーの値を入力します。 ただし、番号がわかりやすいためにする手動で追加する必要があるため、他のビューで表示することを理解していません。

*Views\Course\Edit.cshtml*、前にコース番号フィールドを追加、**タイトル**フィールド。 主キーであるためが表示されますが、変更することはできません。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

非表示フィールドが既に存在 (`Html.HiddenFor`ヘルパー) の編集ビューで、コース番号。 追加、 *Html.LabelFor* 、ユーザーがクリックしたときに、ポストされたデータに含まれるコース番号が原因であるために、ヘルパーは、隠しフィールドの必要性を排除しない**保存**[編集] ページ。

*Views\Course\Delete.cshtml*と*Views\Course\Details.cshtml*"Department"に"Name"から、部門名キャプションを変更して、前にコース番号フィールドを追加、**タイトル**フィールド。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

実行、**作成**ページ (コースのインデックス ページを表示し、をクリックして**新規作成**) 新しいコースのデータを入力します。

| [値] | 設定 |
| ----- | ------- |
| 数値 | 入力*1000*します。 |
| タイトル | 入力*代数*します。 |
| 謝辞 | 入力*4*します。 |
|Department | 選択**数学**します。 |

**[作成]** をクリックします。 コースの Index ページには、一覧に追加された新しいコースが表示されます。 Index ページのリストの部門名は、ナビゲーション プロパティから取得され、リレーションシップが正常に確立されていることを示しています。

実行、**編集**ページ (コースのインデックス ページを表示し、クリックして**編集**コースで)。

ページ上のデータを変更し、**[Save]\(保存\)** をクリックします。 コースの Index ページには、更新されたコース データが表示されます。

## <a name="add-office-to-instructors-page"></a>Instructors ページに office を追加します。

インストラクター レコードを編集するときに、インストラクターのオフィスの割り当ての更新が必要な場合があります。 `Instructor`エンティティと 0 または 1 に 1 つリレーションシップを持つ、`OfficeAssignment`エンティティは、次の状況を処理する必要があります。

- 解除し、削除する場合は、ユーザーがオフィスの割り当てをクリアした値する必要があります、`OfficeAssignment`エンティティ。
- 新規に作成する必要がある場合は、ユーザーがオフィスの割り当ての値を入力し、空か最初、`OfficeAssignment`エンティティ。
- ユーザーは、オフィスの割り当ての値を変更する場合は、既存の値を変更する必要があります`OfficeAssignment`エンティティ。

開いている*InstructorController.cs*を見て、 `HttpGet` `Edit`メソッド。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

スキャフォールディングされたコードをここでは、対象はありません。 データの設定には、ドロップダウン リストがテキスト ボックスは、必要なものです。 このメソッドを次のコードに置き換えます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

このコードを削除、`ViewBag`ステートメントと、一括読み込みを関連付けられている追加`OfficeAssignment`エンティティ。 一括読み込みを実行することはできません、`Find`メソッド、そのため、`Where`と`Single`メソッドは、インストラクターを選択する代わりに使用されます。

置換、 `HttpPost` `Edit`メソッドを次のコード。 これは、オフィスの割り当ての更新を処理します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

参照を`RetryLimitExceededException`が必要です、`using`ステートメントの追加-マウス カーソルを置く;`RetryLimitExceededException`します。 次のようなメッセージが表示されます。![ 例外メッセージを再試行してください。](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)


選択**potentital 修正内容を表示**、し**System.Data.Entity.Infrastructure を使用します。**

![再試行の例外を解決するには](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

このコードは次のことを行います。

- メソッド名を変更`EditPost`署名と同じであるため、`HttpGet`メソッド (、`ActionName`属性は、/edit/URL がまだ使用されていることを指定します)。
- `OfficeAssignment` ナビゲーション プロパティの一括読み込みを使用して、現在の `Instructor` エンティティをデータベースから取得します。 これで行ったのと同じ、 `HttpGet` `Edit`メソッド。
- モデル バインダーからの値を使用して、取得した `Instructor` エンティティを更新します。 [Tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用するオーバー ロードを使用する*ホワイト リスト*プロパティを追加します。 これにより、過剰ポスティングで説明したよう[2 番目のチュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)します。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- オフィスの場所が空白の場合は、設定、`Instructor.OfficeAssignment`プロパティを null ように、関連する行で、`OfficeAssignment`テーブルは削除されます。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- データベースへの変更を保存します。

*Views\Instructor\Edit.cshtml*後に、`div`の要素、 **Hire Date**フィールドで、オフィスの場所を編集するための新しいフィールドを追加します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

ページの実行 (選択、 **Instructors**  タブをクリックして**編集**インストラクターで)。 **[Office Location]\(オフィスの場所\)** を変更し、**[Save]\(保存\)** をクリックします。

## <a name="add-courses-to-instructors-page"></a>Instructors ページにコースを追加します。

インストラクターは、任意の数のコースを担当する場合があります。 チェック ボックスのグループを使用して、コースの割り当てを変更する機能を追加して、Instructor/edit ページを拡張します。

間のリレーションシップ、`Course`と`Instructor`エンティティは多対多。 つまり、結合テーブル内にある外部キー プロパティに直接アクセスする必要はありません。 代わりに、追加し、エンティティを削除して、`Instructor.Courses`ナビゲーション プロパティ。

インストラクターに割り当てられるコースを変更できるようにする UI は、チェック ボックスのグループです。 データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているコースが選択されます。 ユーザーは、チェック ボックスをオンまたはオフにしてコースの割り当てを変更できます。 コースの数が非常に多い場合、ビューでのデータの表示のさまざまなメソッドを使用することが考えられますが、作成またはリレーションシップを削除するにはナビゲーション プロパティを操作するのと同じ方法を使用するとします。

チェック ボックスのリストのためにデータをビューに提供するには、ビュー モデル クラスを使用します。 作成*AssignedCourseData.cs*で、 *ViewModels*フォルダーと、既存のコードを次のコードを置換します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

*InstructorController.cs*、置換、 `HttpGet` `Edit`メソッドを次のコード。 変更が強調表示されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

このコードは、`Courses` ナビゲーション プロパティに一括読み込みを追加し、新しい `PopulateAssignedCourseData` メソッドを呼び出して、`AssignedCourseData` ビュー モデル クラスを使用してチェック ボックス配列に情報を提供します。

内のコード、`PopulateAssignedCourseData`メソッドを読み取り、すべて`Course`ビューを使用したコースのリストを読み込むためにエンティティ モデル クラス。 各コースに対し、コードはそのコースがインストラクターの `Courses` ナビゲーション プロパティ内に存在しているかどうかをチェックします。 コースがインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、 [HashSet](https://msdn.microsoft.com/library/bb359438.aspx)コレクション。 `Assigned`プロパティに設定されて`true`コースの講師が割り当てられます。 ビューは、このプロパティを使用して、どのチェック ボックスを選択済みとして表示する必要があるかを判断します。 最後に、一覧がビューに渡される、`ViewBag`プロパティ。

次に、ユーザーが **[Save]\(保存\)** をクリックしたときに実行されるコードを追加します。 置換、`EditPost`メソッドを次のコードは、更新プログラムの新しいメソッドを呼び出し、`Courses`のナビゲーション プロパティ、`Instructor`エンティティ。 変更が強調表示されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

メソッド シグネチャが異なるようになりました、 `HttpGet` `Edit`メソッド、メソッド名の変更から`EditPost`に`Edit`します。

ビューのコレクションを持たないため`Course`エンティティ、モデル バインダーを更新できません自動的に、`Courses`ナビゲーション プロパティ。 モデル バインダーを使用して更新するのではなく、`Courses`ナビゲーション プロパティでは、新しい行います`UpdateInstructorCourses`メソッド。 そのため、モデル バインドから `Courses` プロパティを除外する必要があります。 これを呼び出すコードの変更が必要としない[tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)使用しているため、*ホワイト リスト登録*オーバー ロードと`Courses`インクルード一覧に含まれていません。

場合、ボックスが選択されていないチェック、コードでは、`UpdateInstructorCourses`を初期化します、`Courses`空のコレクションでのナビゲーション プロパティ。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

その後コードは、データベース内のすべてのコースをループ処理し、各コースを現在インストラクターに割り当てられているコースとビューで選択されているコースを比較してチェックします。 検索を効率化するため、最後の 2 つのコレクションが `HashSet` オブジェクトに格納されます。

コースのチェック ボックスが選択されたが、そのコースが `Instructor.Courses`ナビゲーション プロパティにない場合、そのコースがナビゲーション プロパティ内のコレクションに追加されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

コースのチェック ボックスが選択さていないが、そのコースが `Instructor.Courses`ナビゲーション プロパティにある場合、そのコースがナビゲーション プロパティから削除されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

*Views\Instructor\Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを次を追加することで配列をコードの直後に、`div`の要素、`OfficeAssignment`フィールドと前に、`div`の要素、**保存**ボタン。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

貼り付けた後、コード、改行の場合とインデントしないたいしてここで、次のように、ここにすべてを手動で修正します。 インデントは完璧である必要はありませんが、`@</tr><tr>`、`@:<td>`、`@:</td>`、および `@</tr>` の行は、示されているようにそれぞれ 1 行にする必要があります。そうしないと、ランタイム エラーが発生します。

このコードは、3 つの列を含む HTML テーブルを作成します。 各列には、チェック ボックスとその後に続くキャプションがあります。キャプションは、コース番号とタイトルから構成されます。 チェック ボックスはすべて、同じ名前 ("selectedCourses") は、グループとして扱う場合にモデル バインダーに通知があります。 `value`各チェック ボックスの属性の値に設定されて`CourseID.`で構成されるコント ローラーにモデル バインダーが配列を渡しますページが投稿されたときに、`CourseID`チェック ボックスが選択されている値。

インストラクターに割り当てられるコースのチェック ボックスが最初に表示されると、`checked`属性は、(オンになった状態を表示します) それらを選択します。

コースの割り当てを変更した後に、サイトが返されるときに、変更を確認できる必要あります、`Index`ページ。 そのため、そのページ内のテーブルに列を追加する必要があります。 ここで使用する必要はありません、`ViewBag`オブジェクトを表示する情報は既にあるため、`Courses`のナビゲーション プロパティ、`Instructor`モデルとしてページに渡しているエンティティ。

*Views\Instructor\Index.cshtml*、追加、**コース**直後に、次の見出し、 **Office**見出しで、次の例に示すように。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

オフィスの場所の詳細セルの直後に続く新しい詳細セルを追加します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

実行、 **Instructor インデックス**ページに各インストラクターに割り当てられているコースをご覧ください。

クリックして**編集**の講師が Edit ページを参照してください。

一部のコース割り当てを変更し、クリックして**保存**します。 行った変更が Index ページに反映されます。

 メモ:ここで採用インストラクター コース データを編集するのには、コースの数に制限がある場合にも動作します。 非常に大きいコレクションの場合、別の UI と別の更新方法が必要になる場合があります。

## <a name="update-deleteconfirmed"></a>DeleteConfirmed を更新します。

*InstructorController.cs*、削除、`DeleteConfirmed`代わりに、次のコード メソッドおよび挿入します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

このコードは、次の変更です。

- インストラクターが任意の部門の管理者として割り当てられている場合は、その部門から、インストラクターの割り当てを削除します。 このコードがないが部門の管理者として割り当てられている講師を削除しようとする場合参照整合性エラーが発生したとします。

このコードは、複数の部門の管理者として割り当てられている 1 つの講師のシナリオを処理しません。 最後のチュートリアルでは、そのシナリオが発生しないようにコードを追加します。

## <a name="add-office-location-and-courses-to-the-create-page"></a>オフィスの場所とコースを Create ページに追加する

*InstructorController.cs*、削除、`HttpGet`と`HttpPost``Create`メソッド、代わりに次のコードを追加。


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

このコードは、確認したものの Edit メソッドの最初にコースが選択されていない点が似ています。 `HttpGet` `Create`メソッドの呼び出し、`PopulateAssignedCourseData`メソッド コースが選択されていますがある可能性があるためではなく注文の空のコレクションを提供する、 `foreach` (それ以外の場合、コードの表示は、null 参照例外をスローするビューでのループ).

メソッドの HttpPost の作成では、テンプレート コードを検証エラーをチェックし、データベースに新しいインストラクターを追加する前に、Courses ナビゲーション プロパティを選択した各コースを追加します。 モデル エラーがある場合でも、コースが追加されたように (たとえば、ユーザーが無効な日付キーになります) をモデル エラーがある場合に加えられたコースの選択が自動的に復元するとエラー メッセージで、ページが表示され、ようにします。

コースを `Courses` ナビゲーション プロパティに追加できるようにするには、プロパティを空のコレクションとして初期化する必要があることに注意してください。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

コントローラー コードでこれを行うための別の方法として、Instructor モデルでこれを行うことができます。このためには、プロパティ ゲッターを変更して、コレクションが存在しない場合に自動的に作成するようにします。次の例に示します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Courses` プロパティをこの方法で変更する場合、コントローラー内の明示的なプロパティの初期化コードを削除することができます。

*Views\Instructor\Create.cshtml*、オフィスの場所は、テキスト ボックスを追加し、雇用日フィールドの後と前にコースのチェック ボックス、**送信**ボタンをクリックします。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

コードを貼り付けた後は、改行とインデント設定を編集 ページの前と同じ修正します。

ページの作成を実行し、インストラクターを追加します。

<a id="transactions"></a>

## <a name="handling-transactions"></a>トランザクションの処理

説明したように、[基本 CRUD 機能チュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)、既定では、Entity Framework に暗黙的にはトランザクションを実装します。 必要があります詳細に制御が - たとえば、--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクション操作](https://msdn.microsoft.com/data/dn456843)msdn です。

## <a name="get-the-code"></a>コードを取得する

[完成したプロジェクトをダウンロードします。](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>その他の技術情報

その他の Entity Framework リソースへのリンクが記載[ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。

## <a name="next-step"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * カスタマイズされた courses ページ
> * Instructors ページに office を追加しました
> * Instructors ページに追加されたコース
> * 更新された DeleteConfirmed
> * 追加されたオフィスの場所とコースを作成 ページ

非同期のプログラミング モデルを実装する方法については、次の記事に進んでください。
> [!div class="nextstepaction"]
> [非同期プログラミング モデル](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
