---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (6/10) で Entity Framework で関連データの更新 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e85162f58ed9826132db8bd854914a14709f709d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809434"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>ASP.NET MVC アプリケーション (6/10) で Entity Framework で関連データの更新
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。
> 
> > [!NOTE] 
> > 
> > を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。 一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルには、関連データが表示されます。このチュートリアルでは、関連するデータを更新します。 ほとんどのリレーションシップは、これは適切な外部キー フィールドを更新することで行うことができます。 多対多のリレーションシップで Entity Framework は結合テーブルを直接公開、ため、明示的に追加し、該当するナビゲーション プロパティからエンティティを削除する必要があります。

以下の図は、使用するページを示しています。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Courses の Create ページと Edit ページをカスタマイズする

新しいコース エンティティが作成されると、既存の部門とのリレーションシップが必要になります。 これを容易にするため、スキャフォールディング コードには、コントローラーのメソッドと、部門を選択するためのドロップダウン リストを含む Create ビューと Edit ビューが含まれます。 ドロップダウン リストのセット、`Course.DepartmentID`外部キー プロパティは、Entity Framework は、読み込むために必要なすべて、`Department`ナビゲーション プロパティを適切な`Department`エンティティ。 このスキャフォールディング コードを使用しますが、エラー処理を追加し、ドロップダウン リストを並べ替えるために少し変更します。

*CourseController.cs*、4 つの削除`Edit`と`Create`メソッドし、次のコードに置き換えます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList`メソッドの名前で並べ替えたすべての部門の一覧を取得、作成、`SelectList`のドロップダウン リストでは、コレクション ビューに、コレクションを渡すと、`ViewBag`プロパティ。 このメソッドは、ドロップダウン リストがレンダリングされるときに選択される項目を指定するためのコード呼び出しを許可する、省略可能な `selectedDepartment` パラメーターを受け取ります。 ビューが名前を渡す`DepartmentID`に[、`DropDownList`ヘルパー](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)、ヘルパーがファイルの場所を認識し、`ViewBag`オブジェクト、`SelectList`という名前`DepartmentID`。

`HttpGet` `Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択した項目を設定せずメソッド。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit`メソッドが既に編集中のコースに割り当てられている部門の ID に基づいて、選択した項目を設定します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost`両方のメソッド`Create`と`Edit`も、ページを再表示エラーが発生したときに、選択した項目を設定するコードが含まれます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

このコードによりエラー メッセージを表示、ページが表示されときに、選択した常時されます部門が選択されているようになります。

*Views\Course\Create.cshtml*、作成する前に、新しいコース番号フィールドを強調表示されたコードを追加、**タイトル**フィールド。 前述の以前のチュートリアルでは、主キー フィールドが既定では、スキャフォールディングされませんが、この主キーは、ユーザーは、キーの値を入力できるようにするために、わかりやすい。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

*Views\Course\Edit.cshtml*、 *Views\Course\Delete.cshtml*、および*Views\Course\Details.cshtml*、前にコース番号フィールドを追加、**タイトル**フィールド。 主キーであるためが表示されますが、変更することはできません。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

実行、**作成**ページ (コースのインデックス ページを表示し、をクリックして**新規作成**) 新しいコースのデータを入力します。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

**[作成]** をクリックします。 コースの Index ページには、一覧に追加された新しいコースが表示されます。 Index ページのリストの部門名は、ナビゲーション プロパティから取得され、リレーションシップが正常に確立されていることを示しています。

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

実行、**編集**ページ (コースのインデックス ページを表示し、クリックして**編集**コースで)。

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

ページ上のデータを変更し、**[Save]\(保存\)** をクリックします。 コースの Index ページには、更新されたコース データが表示されます。

## <a name="adding-an-edit-page-for-instructors"></a>Instructors の Edit ページを追加します。

インストラクター レコードを編集するときに、インストラクターのオフィスの割り当ての更新が必要な場合があります。 `Instructor`エンティティと 0 または 1 に 1 つリレーションシップを持つ、`OfficeAssignment`エンティティは、次の状況を処理する必要があります。

- 解除し、削除する場合は、ユーザーがオフィスの割り当てをクリアした値する必要があります、`OfficeAssignment`エンティティ。
- 新規に作成する必要がある場合は、ユーザーがオフィスの割り当ての値を入力し、空か最初、`OfficeAssignment`エンティティ。
- ユーザーは、オフィスの割り当ての値を変更する場合は、既存の値を変更する必要があります`OfficeAssignment`エンティティ。

開いている*InstructorController.cs*を見て、 `HttpGet` `Edit`メソッド。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

スキャフォールディングされたコードをここでは、対象はありません。 データの設定には、ドロップダウン リストがテキスト ボックスは、必要なものです。 このメソッドを次のコードに置き換えます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

このコードを削除、`ViewBag`ステートメントと、一括読み込みを関連付けられている追加`OfficeAssignment`エンティティ。 一括読み込みを実行することはできません、`Find`メソッド、そのため、`Where`と`Single`メソッドは、インストラクターを選択する代わりに使用されます。

置換、 `HttpPost` `Edit`メソッドを次のコード。 これは、オフィスの割り当ての更新を処理します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

このコードは次のことを行います。

- `OfficeAssignment` ナビゲーション プロパティの一括読み込みを使用して、現在の `Instructor` エンティティをデータベースから取得します。 これで行ったのと同じ、 `HttpGet` `Edit`メソッド。
- モデル バインダーからの値を使用して、取得した `Instructor` エンティティを更新します。 [Tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用するオーバー ロードを使用する*ホワイト リスト*プロパティを追加します。 これにより、過剰ポスティングで説明したよう[2 番目のチュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)します。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- オフィスの場所が空白の場合は、設定、`Instructor.OfficeAssignment`プロパティを null ように、関連する行で、`OfficeAssignment`テーブルは削除されます。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- データベースへの変更を保存します。

*Views\Instructor\Edit.cshtml*後に、`div`の要素、 **Hire Date**フィールドで、オフィスの場所を編集するための新しいフィールドを追加します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

ページの実行 (選択、 **Instructors**  タブをクリックして**編集**インストラクターで)。 **[Office Location]\(オフィスの場所\)** を変更し、**[Save]\(保存\)** をクリックします。

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>[編集] ページの講師にコースの割り当てを追加します。

インストラクターは、任意の数のコースを担当する場合があります。 次のスクリーン ショットに示すように、チェック ボックスのグループを使用して、コースの割り当てを変更する機能を追加して、Instructor/Edit ページを拡張します。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

間のリレーションシップ、`Course`と`Instructor`エンティティは多対多。 つまり、結合テーブルに直接アクセスする必要はありません。 追加する代わりに、およびとの間にエンティティを削除するか、`Instructor.Courses`ナビゲーション プロパティ。

インストラクターに割り当てられるコースを変更できるようにする UI は、チェック ボックスのグループです。 データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているコースが選択されます。 ユーザーは、チェック ボックスをオンまたはオフにしてコースの割り当てを変更できます。 コースの数が非常に多い場合、ビューでのデータの表示のさまざまなメソッドを使用することが考えられますが、作成またはリレーションシップを削除するにはナビゲーション プロパティを操作するのと同じ方法を使用するとします。

チェック ボックスのリストのためにデータをビューに提供するには、ビュー モデル クラスを使用します。 作成*AssignedCourseData.cs*で、 *ViewModels*フォルダーと、既存のコードを次のコードを置換します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

*InstructorController.cs*、置換、 `HttpGet` `Edit`メソッドを次のコード。 変更が強調表示されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

このコードは、`Courses` ナビゲーション プロパティに一括読み込みを追加し、新しい `PopulateAssignedCourseData` メソッドを呼び出して、`AssignedCourseData` ビュー モデル クラスを使用してチェック ボックス配列に情報を提供します。

内のコード、`PopulateAssignedCourseData`メソッドを読み取り、すべて`Course`ビューを使用したコースのリストを読み込むためにエンティティ モデル クラス。 各コースに対し、コードはそのコースがインストラクターの `Courses` ナビゲーション プロパティ内に存在しているかどうかをチェックします。 コースがインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、 [HashSet](https://msdn.microsoft.com/library/bb359438.aspx)コレクション。 `Assigned`プロパティに設定されて`true`コースの講師が割り当てられます。 ビューは、このプロパティを使用して、どのチェック ボックスを選択済みとして表示する必要があるかを判断します。 最後に、一覧がビューに渡される、`ViewBag`プロパティ。

次に、ユーザーが **[Save]\(保存\)** をクリックしたときに実行されるコードを追加します。 置換、 `HttpPost` `Edit`メソッドを次のコードは、更新プログラムの新しいメソッドを呼び出し、`Courses`のナビゲーション プロパティ、`Instructor`エンティティ。 変更が強調表示されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

ビューのコレクションを持たないため`Course`エンティティ、モデル バインダーを更新できません自動的に、`Courses`ナビゲーション プロパティ。 モデル バインダーを使用して、コースのナビゲーション プロパティを更新するではなく行います新しい`UpdateInstructorCourses`メソッド。 そのため、モデル バインドから `Courses` プロパティを除外する必要があります。 これを呼び出すコードの変更が必要としない[tryupdatemodel に渡します](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)使用しているため、*ホワイト リスト登録*オーバー ロードと`Courses`インクルード一覧に含まれていません。

場合、ボックスが選択されていないチェック、コードでは、`UpdateInstructorCourses`を初期化します、`Courses`空のコレクションでのナビゲーション プロパティ。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

その後コードは、データベース内のすべてのコースをループ処理し、各コースを現在インストラクターに割り当てられているコースとビューで選択されているコースを比較してチェックします。 検索を効率化するため、最後の 2 つのコレクションが `HashSet` オブジェクトに格納されます。

コースのチェック ボックスが選択されたが、そのコースが `Instructor.Courses`ナビゲーション プロパティにない場合、そのコースがナビゲーション プロパティ内のコレクションに追加されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

コースのチェック ボックスが選択さていないが、そのコースが `Instructor.Courses`ナビゲーション プロパティにある場合、そのコースがナビゲーション プロパティから削除されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

*Views\Instructor\Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを強調表示されている、次を追加することで配列をコードの直後に、`div`の要素、 `OfficeAssignment`フィールド:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

このコードは、3 つの列を含む HTML テーブルを作成します。 各列には、チェック ボックスとその後に続くキャプションがあります。キャプションは、コース番号とタイトルから構成されます。 チェック ボックスはすべて、同じ名前 ("selectedCourses") は、グループとして扱う場合にモデル バインダーに通知があります。 `value`各チェック ボックスの属性の値に設定されて`CourseID.`で構成されるコント ローラーにモデル バインダーが配列を渡しますページが投稿されたときに、`CourseID`チェック ボックスが選択されている値。

インストラクターに割り当てられるコースのチェック ボックスが最初に表示されると、`checked`属性は、(オンになった状態を表示します) それらを選択します。

コースの割り当てを変更した後に、サイトが返されるときに、変更を確認できる必要あります、`Index`ページ。 そのため、そのページ内のテーブルに列を追加する必要があります。 ここで使用する必要はありません、`ViewBag`オブジェクトを表示する情報は既にあるため、`Courses`のナビゲーション プロパティ、`Instructor`モデルとしてページに渡しているエンティティ。

*Views\Instructor\Index.cshtml*、追加、**コース**直後に、次の見出し、 **Office**見出しで、次の例に示すように。

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

オフィスの場所の詳細セルの直後に続く新しい詳細セルを追加します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

実行、 **Instructor インデックス**ページに各インストラクターに割り当てられているコースをご覧ください。

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

クリックして**編集**の講師が Edit ページを参照してください。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

一部のコース割り当てを変更し、クリックして**保存**します。 行った変更が Index ページに反映されます。

 注: インストラクター コース データを編集するには、コースの数に制限がある場合にも動作します。 非常に大きいコレクションの場合、別の UI と別の更新方法が必要になる場合があります。  
 

## <a name="update-the-delete-method"></a>Update、Delete メソッド

インストラクターが削除されたときに (ある場合) は、office の割り当てのレコードが削除されるので、HttpPost Delete メソッドのコードを変更します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


管理者として、学科に割り当てられている講師を削除しようとすると、参照整合性エラーが表示されます。 参照してください[このチュートリアルの現在のバージョン](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)追加のコードを instructor を管理者として、インストラクターが割り当てられている任意の部門から自動的に削除されます。

## <a name="summary"></a>まとめ

この概要に関連するデータの操作が完了しました。 これまでにこれらのチュートリアルでは、完全な範囲の CRUD 操作を実行したが、同時実行の問題に対処していません。 次のチュートリアルは同時実行のトピックを紹介、それを処理するためのオプションについて説明し、同時実行処理を 1 つのエンティティの種類について既に記述した CRUD コードを追加します。

最後に、その他の Entity Framework リソースへのリンクが見つかります[このシリーズの最終チュートリアル](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)します。

> [!div class="step-by-step"]
> [前へ](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
