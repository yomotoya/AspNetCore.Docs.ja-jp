---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "ASP.NET MVC アプリケーション (10 の 6) で、Entity Framework と関連するデータの更新 |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 2ca76364a2e9a71dc92644bd579345ae3c304a69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>ASP.NET MVC アプリケーション (10 の 6) で、Entity Framework と関連するデータの更新
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。
> 
> > [!NOTE] 
> > 
> > 解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、関連するデータが表示されます。このチュートリアルでは関連するデータを更新します。 ほとんどのリレーションシップするこれを適切な外部キー フィールドを更新します。 多対多リレーションシップの場合、Entity Framework は開始されません結合テーブルを直接ように明示的に追加し、適切なナビゲーション プロパティからエンティティを削除する必要があります。

次の図で作業をしているページを表示します。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>コースを作成および編集ページをカスタマイズします。

コースの新しいエンティティが作成されると、既存の部署とのリレーションシップが必要です。 この作業を容易にスキャフォールディング コードには、コント ローラーのメソッドおよび部門を選択するためのドロップダウン リストを含むビューを作成および編集が含まれています。 ドロップダウン リストを設定、`Course.DepartmentID`外部キーのプロパティを読み込むために、Entity Framework が必要なすべてが、`Department`ナビゲーション プロパティが、適切な`Department`エンティティです。 スキャフォールディングのコードを使用することが、エラー処理を追加し、ドロップダウン リストを並べ替えるには少し変更することもできます。

*CourseController.cs*、4 つの削除`Edit`と`Create`メソッドに次のコードに置き換えます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList`メソッド名でソートのすべての部門の一覧を取得、作成、`SelectList`ドロップダウン一覧は、コレクション内のビューに、コレクションを渡すと、`ViewBag`プロパティです。 このメソッドは、省略可能な受け取ります`selectedDepartment`パラメーターをドロップダウン リストが表示される場合に選択される項目を指定する呼び出し元のコードを使用します。 ビューが名前を渡す`DepartmentID`に[、`DropDownList`ヘルパー](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)、ヘルパーは、ファイルの場所を認識しています、`ViewBag`オブジェクトに対して、`SelectList`という名前`DepartmentID`です。

`HttpGet` `Create`メソッドの呼び出し、`PopulateDepartmentsDropDownList`新しいコースの部門が確立されていないため、選択したアイテムを設定しなくてもメソッド。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit`メソッドは編集されているコースに既に割り当てられている部門の ID に基づいて、選択したアイテムを設定します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost`両方のメソッド`Create`と`Edit`もエラーが発生したページを再表示するときに、選択した項目を設定するコードが含まれます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

このコードにより、いるエラー メッセージを表示する、ページが表示され、ときにどのような部門が選択されている選択されたままとします。

*Views\Course\Create.cshtml*の強調表示されたコードを追加する前に新しいコース番号フィールドを作成、**タイトル**フィールドです。 主キー フィールドは既定では、スキャフォールディングされた前述の以前のチュートリアルがこの主キー、意味のあるユーザーが、キーの値を入力できるようにするためです。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

*Views\Course\Edit.cshtml*、 *Views\Course\Delete.cshtml*、および*Views\Course\Details.cshtml*、コースの前に数値フィールドを追加、**タイトル**フィールドです。 主キーであるためにが表示されますが、変更することはできません。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

実行、**作成**ページ (コース インデックス ページを表示し、をクリックして**新規作成**) と新しいコースのデータを入力します。

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

**[作成]**をクリックします。 一覧に追加された新しいコース コース インデックス ページが表示されます。 インデックス ページの一覧で、部門名を示すリレーションシップが正常に確立されているナビゲーション プロパティに由来します。

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

実行、**編集**ページ (コース インデックス ページを表示し、をクリックして**編集**コースで)。

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

ページ上のデータを変更し、クリックして**保存**です。 コース インデックス ページには、更新された一連のデータが表示されます。

## <a name="adding-an-edit-page-for-instructors"></a>講習においてインストラクター編集ページを追加します。

講師レコードを編集するときに、インストラクター オフィス割り当てを更新することにします。 `Instructor`エンティティと 0 または 1 を 1 つの関係には、`OfficeAssignment`エンティティで、次の状況を処理しなければならないことを意味します。

- ユーザーがオフィス割り当てをクリアし、値が最初にあった場合、は、解除し、削除する必要があります、`OfficeAssignment`エンティティです。
- 新規に作成する必要があります office 代入値を入力すると、空か最初、`OfficeAssignment`エンティティです。
- Office の代入式の値を変更すると場合、は、既存の値を変更する必要があります`OfficeAssignment`エンティティです。

開いている*InstructorController.cs*見ると、 `HttpGet` `Edit`メソッド。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

スキャフォールディング コードをここでは、する必要はありません。 データを設定するには、ドロップダウン リストが必要なものは、テキスト ボックス。 このメソッドを次のコードに置き換えます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

このコードは、`ViewBag`ステートメント、関連付けられているため、一括読み込みを追加および`OfficeAssignment`エンティティです。 一括読み込みを行うことはできません、`Find`メソッド、ため、`Where`と`Single`メソッドは、インストラクターを選択する代わりに使用されます。

置換、 `HttpPost` `Edit`メソッドを次のコード。 これは、割り当ての office 更新プログラムを処理します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

コードは次のとおり

- 現在の取得`Instructor`の一括読み込みを使用して、データベースからのエンティティ、`OfficeAssignment`ナビゲーション プロパティ。 これと同じで行った、 `HttpGet` `Edit`メソッドです。
- 更新、取得した`Instructor`モデル バインダーから値を持つエンティティ。 [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx)使用オーバー ロードを使用する*ホワイト リスト*に含めるプロパティです。 こうすれば、過剰な投稿で説明したよう[2 番目のチュートリアル](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)です。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- オフィスの場所が空白の場合は、設定、`Instructor.OfficeAssignment`プロパティを null にできるように、関連する行で、`OfficeAssignment`テーブルは削除されます。

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- データベースに変更を保存します。

*Views\Instructor\Edit.cshtml*の後に、`div`用の要素、 **Hire Date**フィールドに、オフィスの場所を編集するための新しいフィールドを追加します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

ページの実行 (選択、**講習においてインストラクター**  タブをクリックして**編集**インストラクターに)。 変更、**オフィス** をクリック**保存**です。

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>[編集] ページをインストラクターにコースの課題を追加します。

講習においてインストラクターには、コースの任意の数を教えることがあります。 次のスクリーン ショットに示すように、チェック ボックスのグループを使用して、course 割り当てを変更する機能を追加することでインストラクターの編集 ページを拡張します。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

間のリレーションシップ、`Course`と`Instructor`エンティティは多対多の結合テーブルに直接アクセスする必要はありません。 追加する代わりに、およびとの間にエンティティを削除するか、`Instructor.Courses`ナビゲーション プロパティ。

UI コースを変更することができるインストラクターに割り当てられているチェック ボックスのグループです。 データベース内のすべてのコースのチェック ボックスが表示され、インストラクターに現在割り当てられているものを選択します。 ユーザーでは、選択したり、コースの割り当てを変更する チェック ボックスをオフにすることができます。 コースの数が非常に大きいと場合、は、他のビューでは、データの表示方法を使用することはおそらくが作成またはリレーションシップを削除するためにナビゲーション プロパティを操作するための同じメソッドを使用するとします。

チェック ボックスの一覧については、ビューにデータを提供するには、ビュー モデル クラスを使用します。 作成*AssignedCourseData.cs*で、 *ViewModels*フォルダーと、既存のコードを次のコードを置換します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

*InstructorController.cs*、置換、 `HttpGet` `Edit`メソッドを次のコード。 変更が強調表示されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

コードでは、追加の一括読み込み、`Courses`ナビゲーション プロパティ、新しいを呼び出すと`PopulateAssignedCourseData` チェック ボックスを配列の使用に関する情報を提供するメソッドを`AssignedCourseData`モデル クラスを表示します。

内のコード、`PopulateAssignedCourseData`メソッドはすべて読み取り`Course`ビューを使用してコースの一覧を読み込むためにエンティティ モデル クラス。 コードが、インストラクターにコースが存在するかどうかをチェック各コースに`Courses`ナビゲーション プロパティ。 コースをインストラクターに割り当てられているかどうかをチェックするときに、効率的な参照を作成するには、インストラクターに割り当てられているコースに配置されます、 [HashSet](https://msdn.microsoft.com/library/bb359438.aspx)コレクション。 `Assigned`プロパティに設定されている`true`コース講師が割り当てられます。 ビューは、このプロパティを使用して、どのチェック ボックスとして表示する選択を確認されます。 最後に、一覧は、ビューに渡される、`ViewBag`プロパティです。

次に、ユーザーがクリックしたときに実行されるコードを追加**保存**です。 置換、 `HttpPost` `Edit`メソッドを次のコードを更新する新しいメソッドを呼び出した、`Courses`のナビゲーション プロパティ、`Instructor`エンティティです。 変更が強調表示されます。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

ビューのコレクションがあるないため`Course`エンティティ、モデル バインダーは自動的に更新、`Courses`ナビゲーション プロパティ。 モデル バインダーを使用して、コースのナビゲーション プロパティを更新するの代わりにすることによって行います新しい`UpdateInstructorCourses`メソッドです。 除外する必要があるため、`Courses`モデル バインドからのプロパティです。 呼び出すコードの変更は必要ありません[TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx)使用しているため、*ホワイト リスト*オーバー ロードと`Courses`は include リストではありません。

場合チェックを行わないボックスを選択した場合、コードで`UpdateInstructorCourses`を初期化、`Courses`ナビゲーション プロパティが空のコレクション。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

コードは、データベース内のすべてのコースをループ処理し、ビューで選択されたものではなくインストラクターに現在割り当てられているものに対しては、各コースを確認します。 効率的な検索を容易に後者の 2 つのコレクションが格納されている`HashSet`オブジェクト。

コースのチェック ボックスが選択されましたが、コースがにないかどうか、`Instructor.Courses`ナビゲーション プロパティ、コースはナビゲーション プロパティで、コレクションに追加します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

コースのチェック ボックスが選択されますが、コースは場合、`Instructor.Courses`コースのナビゲーション プロパティは、ナビゲーション プロパティから削除します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

*Views\Instructor\Edit.cshtml*、追加、**コース**フィールドのチェック ボックスを強調表示されている、次を追加することで配列にコードの直後に、`div`用の要素、 `OfficeAssignment`フィールド:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

このコードでは、次の 3 つの列を含んでいる HTML テーブルを作成します。 各列ではコースの番号とタイトルから構成されるキャプションを続けて チェック ボックスです。 すべてのチェック ボックスをグループとして扱う場合にモデル バインダーを通知する同じ名前 ("selectedCourses") であります。 `value`各チェック ボックスの属性がの値に設定されている`CourseID.`モデル バインダーがコント ローラーで構成される配列を渡しますページがポストされるときに、`CourseID`のチェック ボックスが選択されている値。

適合インストラクターに割り当てられているコースのチェック ボックスが表示される最初と`checked`属性は、それらを (それらのチェックが表示される) を選択します。

コースの割り当てを変更した後にしておくに戻ると、サイトに変更を確認できる、`Index`ページ。 そのため、そのページ内のテーブルに列を追加する必要があります。 使用する必要はありませんここでは、`ViewBag`オブジェクトを表示する情報は既にあるため、`Courses`のナビゲーション プロパティ、`Instructor`モデルとしてそのページへ渡そうとしているエンティティ。

*Views\Instructor\Index.cshtml*、追加、**コース**見出しの直後、 **Office**見出しで、次の例に示すように。

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

オフィスの場所の詳細セルに引き続いてすぐに新しい詳細セルを追加します。

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

実行、**インストラクター インデックス**ページを参照する各インストラクターに割り当てられているコース。

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

をクリックして**編集**の編集 ページを表示するには、あるインストラクターにします。

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

一部のコース割り当てを変更し、クリックして**保存**です。 インデックス ページには、加えた変更が反映されます。

 注: インストラクター コースのデータを編集するには、コースの数に制限がある場合にも動作します。 非常に大きいコレクションを別の UI と更新の別の方法が必要になります。  
 

## <a name="update-the-delete-method"></a>Update、Delete メソッド

講師が削除されたとき (存在する場合) は、office の割り当てのレコードが削除、HttpPost 削除メソッドのコードを変更します。

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


管理者としての部門に割り当てられているインストラクターを削除しようとすると、参照整合性エラーが表示されます。 参照してください[このチュートリアルの現在のバージョン](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)追加のコードを管理者としてインストラクターが割り当てられているその他の部門からインストラクターを自動的に削除されます。

## <a name="summary"></a>まとめ

この概要に関連するデータの操作が完了しました。 これまでにこれらのチュートリアルでは、全範囲の CRUD 操作を実行したが、同時実行の問題に対処していません。 次のチュートリアルは同時実行のトピックを紹介、それを処理するためのオプションについて説明し、同時処理を 1 つのエンティティの種類について記述した CRUD コードを追加します。

最後に他の Entity Framework リソースへのリンクが見つかりません[このシリーズの前回のチュートリアル](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)です。

>[!div class="step-by-step"]
[前へ](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
