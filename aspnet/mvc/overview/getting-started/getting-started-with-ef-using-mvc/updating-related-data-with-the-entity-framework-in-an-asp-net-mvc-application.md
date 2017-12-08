---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "ASP.NET MVC アプリケーションに Entity Framework と関連するデータの更新 |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 348940748e3c33ace03d1b8f41615e9814cf6b40
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>ASP.NET MVC アプリケーションに Entity Framework と関連するデータの更新
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。


前のチュートリアルでは、関連するデータが表示されます。このチュートリアルでは関連するデータを更新します。 ほとんどのリレーションシップでこれを外部キー フィールドまたはナビゲーション プロパティのいずれかの更新します。 多対多のリレーションシップで Entity Framework は開始されません結合テーブルを直接追加し、適切なナビゲーション プロパティからエンティティを削除するようにします。

次の図を使って作業するページの一部を表示します。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![コースをインストラクター編集](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>コースを作成および編集ページをカスタマイズします。

コースの新しいエンティティが作成されると、既存の部署とのリレーションシップが必要です。 この作業を容易にスキャフォールディング コードには、コント ローラーのメソッドおよび部門を選択するためのドロップダウン リストを含むビューを作成および編集が含まれています。 ドロップダウン リストを設定、`Course.DepartmentID`外部キーのプロパティを読み込むために、Entity Framework が必要なすべてが、`Department`ナビゲーション プロパティが、適切な`Department`エンティティです。 スキャフォールディングのコードを使用することが、エラー処理を追加し、ドロップダウン リストを並べ替えるには少し変更することもできます。

*CourseController.cs*、4 つの削除`Create`と`Edit`メソッドに次のコードに置き換えます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

次の追加`using`ファイルの先頭にステートメント。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList`メソッド名でソートのすべての部門の一覧を取得、作成、`SelectList`ドロップダウン一覧は、コレクション内のビューに、コレクションを渡すと、`ViewBag`プロパティです。 このメソッドは、省略可能な受け取ります`selectedDepartment`パラメーターをドロップダウン リストが表示される場合に選択される項目を指定する呼び出し元のコードを使用します。 ビューが名前を渡す`DepartmentID`を[DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)とヘルパーのヘルパーに渡し、ファイルの場所を認識し、`ViewBag`オブジェクトに対する、`SelectList`という名前`DepartmentID`です。

`HttpGet` `Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択したアイテムを設定しなくてもメソッド。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit`メソッドは編集されているコースに既に割り当てられている部門の ID に基づいて、選択したアイテムを設定します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost`両方のメソッド`Create`と`Edit`もエラーが発生したページを再表示するときに、選択した項目を設定するコードが含まれます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

このコードにより、いるエラー メッセージを表示する、ページが表示され、ときにどのような部門が選択されている選択されたままとします。

コース ビューは、部門フィールドのドロップダウン リストを持つスキャフォールディングされた既にを使用しない DepartmentID キャプションこのフィールドは、変更、次の強調表示するように、 *Views\Course\Create.cshtml*ファイルの名前をキャプションを変更します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

同じ変更を加える*Views\Course\Edit.cshtml*です。

通常、scaffolder は、キーの値は、データベースによって生成されるとは変更できませんあり、意味のあるユーザーに表示される値ではないために、主キーをスキャフォールディングしません。 コースのエンティティ、scaffolder はテキスト ボックスを`CourseID`を認識するためのフィールド、`DatabaseGeneratedOption.None`属性は、ユーザーができることを意味主キーの値を入力します。 数値が意味のあることを手動で追加する必要がありますので、他のビューで参照を理解していないこと。

*Views\Course\Edit.cshtml*、コースの前に数値フィールドを追加、**タイトル**フィールドです。 主キーであるためにが表示されますが、変更することはできません。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

非表示フィールドが既に存在 (`Html.HiddenFor`ヘルパー) の編集ビューでコース番号。 追加する、 *Html.LabelFor* 、ユーザーがクリックしたときに、ポストされたデータに含まれるコース番号が原因であるために、ヘルパーは隠しフィールドの必要性を排除しない**保存**[編集] ページ。

*Views\Course\Delete.cshtml*と*Views\Course\Details.cshtml*部門名キャプションを"Department"を"Name"からの変更、および前にコース数値フィールドを追加、**タイトル**フィールドです。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

実行、**作成**ページ (コース インデックス ページを表示し、をクリックして**新規作成**) と新しいコースのデータを入力します。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)


              **[作成]**をクリックします。 一覧に追加された新しいコース コース インデックス ページが表示されます。 インデックス ページの一覧で、部門名を示すリレーションシップが正常に確立されているナビゲーション プロパティに由来します。

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

実行、**編集**ページ (コース インデックス ページを表示し、をクリックして**編集**コースで)。

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

ページ上のデータを変更し、クリックして**保存**です。 コース インデックス ページには、更新された一連のデータが表示されます。

## <a name="adding-an-edit-page-for-instructors"></a>講習においてインストラクター編集ページを追加します。

講師レコードを編集するときに、インストラクター オフィス割り当てを更新することにします。 `Instructor`エンティティと 0 または 1 を 1 つの関係には、`OfficeAssignment`エンティティで、次の状況を処理しなければならないことを意味します。

- ユーザーがオフィス割り当てをクリアし、値が最初にあった場合、は、解除し、削除する必要があります、`OfficeAssignment`エンティティです。
- 新規に作成する必要があります office 代入値を入力すると、空か最初、`OfficeAssignment`エンティティです。
- Office の代入式の値を変更すると場合、は、既存の値を変更する必要があります`OfficeAssignment`エンティティです。

開いている*InstructorController.cs*見ると、 `HttpGet` `Edit`メソッド。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

スキャフォールディング コードをここでは、する必要はありません。 データを設定するには、ドロップダウン リストが必要なものは、テキスト ボックス。 このメソッドを次のコードに置き換えます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

このコードは、`ViewBag`ステートメント、関連付けられているため、一括読み込みを追加および`OfficeAssignment`エンティティです。 一括読み込みを行うことはできません、`Find`メソッド、ため、`Where`と`Single`メソッドは、インストラクターを選択する代わりに使用されます。

置換、 `HttpPost` `Edit`メソッドを次のコード。 これは、割り当ての office 更新プログラムを処理します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

参照を`RetryLimitExceededException`が必要です、`using`ステートメントですこれを追加するには、を右クリック`RetryLimitExceededException`、順にクリック**解決** - **System.Data.Entity.Infrastructureを使用して**.

![再試行の例外を解決するには](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

コードは次のとおり

- メソッドの名前を変更`EditPost`署名が、同じため、`HttpGet`メソッド (、`ActionName`属性は、/Edit/URL はまだ使用されていることを指定します)。
- 現在の取得`Instructor`の一括読み込みを使用して、データベースからのエンティティ、`OfficeAssignment`ナビゲーション プロパティ。 これと同じで行った、 `HttpGet` `Edit`メソッドです。
- 更新、取得した`Instructor`モデル バインダーから値を持つエンティティ。 [TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.108).aspx)使用オーバー ロードを使用する*ホワイト リスト*に含めるプロパティです。 こうすれば、過剰な投稿で説明したよう[2 番目のチュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)です。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- オフィスの場所が空白の場合は、設定、`Instructor.OfficeAssignment`プロパティを null にできるように、関連する行で、`OfficeAssignment`テーブルは削除されます。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- データベースに変更を保存します。

*Views\Instructor\Edit.cshtml*の後に、`div`用の要素、 **Hire Date**フィールドに、オフィスの場所を編集するための新しいフィールドを追加します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

ページの実行 (選択、**講習においてインストラクター**  タブをクリックして**編集**インストラクターに)。 変更、**オフィス** をクリック**保存**です。

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>[編集] ページをインストラクターにコースの課題を追加します。

講習においてインストラクターには、コースの任意の数を教えることがあります。 次のスクリーン ショットに示すように、チェック ボックスのグループを使用して、course 割り当てを変更する機能を追加することでインストラクターの編集 ページを拡張します。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

間のリレーションシップ、`Course`と`Instructor`エンティティは多対多の結合テーブルでは、外部キーのプロパティに直接アクセスする必要はありません。 代わりの追加し、削除のエンティティとの間、`Instructor.Courses`ナビゲーション プロパティ。

UI コースを変更することができるインストラクターに割り当てられているチェック ボックスのグループです。 データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているものを選択します。 ユーザーでは、選択したり、コースの割り当てを変更する チェック ボックスをオフにすることができます。 コースの数が非常に大きいと場合、は、他のビューでは、データの表示方法を使用することはおそらくが作成またはリレーションシップを削除するためにナビゲーション プロパティを操作するための同じメソッドを使用するとします。

チェック ボックスの一覧については、ビューにデータを提供するには、ビュー モデル クラスを使用します。 作成*AssignedCourseData.cs*で、 *ViewModels*フォルダーと、既存のコードを次のコードを置換します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

*InstructorController.cs*、置換、 `HttpGet` `Edit`メソッドを次のコード。 変更が強調表示されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

コードでは、追加の一括読み込み、`Courses`ナビゲーション プロパティ、新しいを呼び出すと`PopulateAssignedCourseData` チェック ボックスを配列の使用に関する情報を提供するメソッドを`AssignedCourseData`モデル クラスを表示します。

内のコード、`PopulateAssignedCourseData`メソッドはすべて読み取り`Course`ビューを使用してコースの一覧を読み込むためにエンティティ モデル クラス。 コードが、インストラクターにコースが存在するかどうかをチェック各コースに`Courses`ナビゲーション プロパティ。 コースをインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、 [HashSet](https://msdn.microsoft.com/en-us/library/bb359438.aspx)コレクション。 `Assigned`プロパティに設定されている`true`コース講師が割り当てられます。 ビューは、このプロパティを使用して、どのチェック ボックスとして表示する選択を確認されます。 最後に、一覧は、ビューに渡される、`ViewBag`プロパティです。

次に、ユーザーがクリックしたときに実行されるコードを追加**保存**です。 置換、`EditPost`メソッドを次のコードを更新する新しいメソッドを呼び出した、`Courses`のナビゲーション プロパティ、`Instructor`エンティティです。 変更が強調表示されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

メソッドのシグネチャが異なるようになりました、 `HttpGet` `Edit`メソッド、メソッド名の変更から`EditPost`に`Edit`です。

ビューのコレクションがあるないため`Course`エンティティ、モデル バインダーは自動的に更新、`Courses`ナビゲーション プロパティ。 モデル バインダーを使用して更新するのではなく、`Courses`ナビゲーション プロパティで、新しい実行を`UpdateInstructorCourses`メソッドです。 除外する必要があるため、`Courses`モデル バインドからのプロパティです。 呼び出すコードの変更は必要ありません[TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.98).aspx)使用しているため、*ホワイト リスト*オーバー ロードと`Courses`は include リストではありません。

場合チェックを行わないボックスを選択した場合、コードで`UpdateInstructorCourses`を初期化、`Courses`ナビゲーション プロパティが空のコレクション。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

コードは、データベース内のすべてのコースをループ処理し、ビューで選択されたものではなくインストラクターに現在割り当てられているものに対しては、各コースを確認します。 効率的な検索を容易に後者の 2 つのコレクションが格納されている`HashSet`オブジェクト。

コースのチェック ボックスが選択されましたが、コースがにないかどうか、`Instructor.Courses`ナビゲーション プロパティ、コースはナビゲーション プロパティで、コレクションに追加します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

コースのチェック ボックスが選択されますが、コースは場合、`Instructor.Courses`コースのナビゲーション プロパティは、ナビゲーション プロパティから削除します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

*Views\Instructor\Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを次を追加することで配列にコードの直後に、`div`用の要素、`OfficeAssignment`フィールドと前に、`div`要素を**保存**ボタン。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

貼り付けた後、コード、改行する場合とインデントされないように見えます。 ここでは、ここに表示するように見えるようににすべてを手動で修正します。 インデント完璧なのに、する必要はありませんが、 `@</tr><tr>`、 `@:<td>`、 `@:</td>`、および`@</tr>`行ことはできません。 1 行に示すように、または実行時エラーが表示されます。

このコードでは、次の 3 つの列を含んでいる HTML テーブルを作成します。 各列ではコースの番号とタイトルから構成されるキャプションを続けて チェック ボックスです。 すべてのチェック ボックスをグループとして扱う場合にモデル バインダーを通知する同じ名前 ("selectedCourses") であります。 `value`各チェック ボックスの属性がの値に設定されている`CourseID.`モデル バインダーがコント ローラーで構成される配列を渡しますページがポストされるときに、`CourseID`のチェック ボックスが選択されている値。

適合インストラクターに割り当てられているコースのチェック ボックスが表示される最初と`checked`属性は、それらを (それらのチェックが表示される) を選択します。

コースの割り当てを変更した後にしておくに戻ると、サイトに変更を確認できる、`Index`ページ。 そのため、そのページ内のテーブルに列を追加する必要があります。 使用する必要はありませんここでは、`ViewBag`オブジェクトを表示する情報は既にあるため、`Courses`のナビゲーション プロパティ、`Instructor`モデルとしてそのページへ渡そうとしているエンティティ。

*Views\Instructor\Index.cshtml*、追加、**コース**見出しの直後、 **Office**見出しで、次の例に示すように。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

オフィスの場所の詳細セルに引き続いてすぐに新しい詳細セルを追加します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

実行、**インストラクター インデックス**ページを参照する各インストラクターに割り当てられているコース。

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

をクリックして**編集**の編集 ページを表示するには、あるインストラクターにします。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

一部のコース割り当てを変更し、クリックして**保存**です。 インデックス ページには、加えた変更が反映されます。

 注: インストラクター コースのデータを編集するのには、ここに採用されているアプローチは、コースの数に制限がある場合にも動作します。 非常に大きいコレクションを別の UI と更新の別の方法が必要になります。  
 

## <a name="update-the-deleteconfirmed-method"></a>Update DeleteConfirmed メソッド

*InstructorController.cs*、削除、`DeleteConfirmed`代わりに、次のコードにメソッドおよび挿入します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

このコードは、次の変更を加え。

- 講師がその他の部門の管理者として割り当てられている場合は、その部門からインストラクター割り当てを削除します。 このコードがない場合、取得した参照整合性エラー部門の管理者として割り当てられている講師を削除しようとしています。 します。

このコードは、複数の部門の管理者として割り当てられている 1 つのインストラクターのシナリオを処理しません。 前回のチュートリアルでは、そのシナリオが発生しないようにコードを追加します。

## <a name="add-office-location-and-courses-to-the-create-page"></a>オフィスの場所およびコースを作成 ページに追加します。

*InstructorController.cs*、削除、`HttpGet`と`HttpPost``Create`メソッド、代わりに、次のコードを追加します。


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

このコードは、新機能を確認しました編集メソッドの最初にコースが選択されていないする点を除いてに似ています。 `HttpGet` `Create`メソッドの呼び出し、`PopulateAssignedCourseData`メソッド コースが選択されている場合がある可能性がありますのでない注文の空のコレクションを提供する、 `foreach` (それ以外の場合、コードの表示は、例外をスロー null 参照ビューでループ).

HttpPost 作成メソッドでは、テンプレート コードの検証エラーをチェックし、データベースに新しいインストラクターを追加する前に、コースのナビゲーション プロパティを選択した各コースを追加します。 モデル エラーがある場合でも、コースが追加されたようにモデル エラー (例については、無効な日付をキーとしたユーザー) があるときに行ったコース選択内容を自動的に復元すると、エラー メッセージで、ページが表示され、ようにします。

コースを追加できるようにすることを確認、`Courses`ナビゲーション プロパティが空のコレクションとしてプロパティを初期化する必要があります。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

コント ローラーのコードでこれを行う代わりに、としてでしたで行うこと、インストラクター モデルを自動的に作成、コレクションが存在しない場合、次の例に示すようにプロパティ get アクセス操作子を変更することで。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

変更する場合、`Courses`プロパティこの方法では、コント ローラーの明示的なプロパティの初期化コードを削除することができます。

*Views\Instructor\Create.cshtml*オフィスの場所テキスト ボックスを追加、および、雇用日フィールドの後と前に、チェック ボックスをコース、**送信**ボタンをクリックします。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

コードを貼り付けると後、は、改行してインデントを編集 ページの前に行ったように修正できます。

ページの作成を実行し、インストラクターを追加します。

![講師がコースを作成します。](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>トランザクションの処理

説明に従って、[基本的な CRUD 機能チュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)既定では、Entity Framework に暗黙的に実装するトランザクション。 必要な複数を制御する--など--トランザクションでは Entity Framework の外部で実行する操作を追加する場合のシナリオを参照してください[トランザクションを使用](https://msdn.microsoft.com/en-US/data/dn456843)msdn です。

## <a name="summary"></a>概要

この概要に関連するデータの操作が完了しました。 これまでにこれらのチュートリアルは、同期 I/O を実行するコードで作業しました。 非同期のコードを実装することによって、web サーバーのリソースをより効率的に使用するアプリケーションを行い、それが次のチュートリアルで実行されます。

このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。 新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。

その他の Entity Framework リソースへのリンクは含まれて[ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

>[!div class="step-by-step"]
[前へ](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[次へ](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
