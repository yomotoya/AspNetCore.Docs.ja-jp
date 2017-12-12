---
title: "EF Core - と razor ページの読み取り関連データは、8 6"
author: rick-anderson
description: "このチュートリアルでは、読み取りおよびでナビゲーション プロパティに、Entity Framework が読み込まれるデータは、関連するデータを表示します。"
keywords: "ASP.NET Core、Entity Framework Core では、関連するデータの結合します。"
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ba9b17ecdcb605d39117d03230b1db37e8e4d0dd
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2017
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>読み取りに関連したデータの EF コア Razor ページ (8 の 6)

によって[Tom Dykstra](https://github.com/tdykstra)、 [Jon P Smith](https://twitter.com/thereformedprog)、および[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

このチュートリアルでは、関連するデータが読み取られ、表示されます。 関連するデータは、EF コアがナビゲーションのプロパティに読み込まれるデータです。

問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related)です。

次の図は、このチュートリアルの完成したページを表示します。

![インデックス ページのコース](read-related-data/_static/courses-index.png)

![講習においてインストラクター インデックス ページ](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Eager、明示的なおよび関連するデータの読み込み遅延

いくつかの方法が EF コアが、エンティティのナビゲーション プロパティに関連するデータを読み込むことができます。

* [一括読み込み](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading)です。 一括読み込みは、エンティティの 1 つの種類のクエリも関連エンティティを読み込むときにです。 エンティティが読み込まれると、その関連データが取得されます。 通常、この結果、すべてのために必要なデータを取得する 1 つの結合クエリ。 EF Core では、一括読み込みの種類によっては複数のクエリを発行します。 複数のクエリを発行することができます効率的である場合は、EF6 で一部のクエリの場合よりも 1 つのクエリがあった場合。 一括読み込みを指定した、`Include`と`ThenInclude`メソッドです。

 ![一括読み込みの例](read-related-data/_static/eager-loading.png)
 
 一括読み込みでは、コレクション nvavigation が含まれるときに、複数のクエリが送信されます。

 * メインのクエリに対して 1 つのクエリ 
 * 「エッジ」ロード ツリー内にコレクションごとに 1 つのクエリ。

* 使用してクエリを区切る`Load`: データは、個別のクエリで取得できるまたは EF コア「修正プログラム」ナビゲーション プロパティ。 EF コアは、ナビゲーション プロパティを自動的に入力「を修正」を意味します。 使用してクエリを区切る`Load`明示的な一括読み込みよりを読み込むようにします。

 ![別のクエリ例](read-related-data/_static/separate-queries.png)

 注: EF コアをコンテキストのインスタンスに以前に読み込まれたその他のエンティティにナビゲーション プロパティを自動的に修正します。 ナビゲーション プロパティのデータが場合でも*いない*いくつかの場合は、プロパティを作成して明示的に含まれる、または以前に読み込まれたすべての関連するエンティティ。

* [明示的な読み込み](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading)です。 エンティティが読み込まれると、関連データが取得されていません。 必要なときに、関連するデータを取得するコードを記述する必要があります。 個別のクエリで明示的な読み込み、DB に送信される複数のクエリ結果になります。 明示的な読み込みでは、コードは、読み込まれるナビゲーションのプロパティを指定します。 使用して、`Load`明示的な読み込みを行うメソッドです。 例:

 ![明示的な読み込みの例](read-related-data/_static/explicit-loading.png)

* [遅延読み込み](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading)です。 [EF コアは現在遅延読み込みをサポートしていません](https://github.com/aspnet/EntityFrameworkCore/issues/3797)です。 エンティティが読み込まれると、関連データが取得されていません。 ナビゲーション プロパティがアクセスすると、最初にそのナビゲーション プロパティに必要なデータは自動的に取得されます。 クエリは、毎回、ナビゲーション プロパティへのアクセスを初めて DB に送信されます。

* `Select`演算子が必要な関連するデータのみを読み込みます。

## <a name="create-a-courses-page-that-displays-department-name"></a>部門名が表示されるコース ページを作成します。

Course エンティティには含むナビゲーション プロパティが含まれています、`Department`エンティティです。 `Department`エンティティには、このコースに割り当てられている部門が含まれています。

コースの一覧で、割り当てられている部門の名前を表示。

* 取得、`Name`プロパティから、`Department`エンティティです。
* `Department`エンティティの取得元、`Course.Department`ナビゲーション プロパティ。

![ourse です。部門](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Scaffold コース モデル

* Visual Studio を終了します。
* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* 次のコマンドを実行します。

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

上記のコマンド スキャフォールディング、`Course`モデル。 Visual Studio でプロジェクトを開きます。

プロジェクトをビルドします。 ビルドには、次のようなエラーが生成されます。

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 グローバルに変更する`_context.Course`に`_context.Courses`(つまり、"s"を追加する`Course`)。 7 回の出現が検出され、更新します。

開いている*Pages/Courses/Index.cshtml.cs*を調べると、`OnGetAsync`メソッドです。 スキャフォールディング エンジンは、指定の一括読み込み、`Department`ナビゲーション プロパティ。 `Include`メソッドは、一括読み込みを指定します。

アプリの実行を選択して、**コース**リンクします。 Department 列を表示、 `DepartmentID`、実用的ではありません。

`OnGetAsync` メソッドを次のコードで更新します。

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

上記のコードを追加`AsNoTracking`です。 `AsNoTracking`返されるエンティティは追跡されませんのでパフォーマンスが向上します。 現在のコンテキストでは更新されませんので、エンティティは追跡されません。

更新*Views/Courses/Index.cshtml*次の強調表示されているマークアップ。

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

スキャフォールディングのコードに、次のように変更されました。

* インデックスからのコースに見出しを変更します。
* 追加、**数**を表示する列、`CourseID`プロパティの値。 既定では、通常はエンドユーザーにとって意味のないために、主キーはスキャフォールディングされました。 ただし、ここでは、主キーは無効です。
* 変更、**部門**部門名を表示する列。 コードが表示されます、`Name`のプロパティ、`Department`に読み込まれるエンティティ、`Department`ナビゲーション プロパティ。

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

アプリの実行を選択して、**コース**部署名を使用してリストを表示するタブです。

![インデックス ページのコース](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>関連する Select とデータの読み込み

`OnGetAsync`メソッドに関連するデータを読み込みます、`Include`メソッド。

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select`演算子が必要な関連するデータのみを読み込みます。 単一のアイテムのように、 `Department.Name` SQL INNER JOIN を使用します。 コレクションの 別のデータベースのアクセスを使用しますが、です。`Include` コレクションに対して演算子です。

次のコードを持つ関連データの読み込み、`Select`メソッド。

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

参照してください[IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml)と[IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)完全な例です。

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>コースおよびまで登録数を示す講習においてインストラクター ページを作成します。

このセクションでは、インストラクター ページが作成されます。

<a name="IP"></a>
![講習においてインストラクター インデックス ページ](read-related-data/_static/instructors-index.png)

このページは、読み取りし、次のように関連するデータを表示します。

* 講習においてインストラクターの一覧から関連するデータが表示されます、`OfficeAssignment`エンティティ (前のイメージでの Office)。 `Instructor`と`OfficeAssignment`エンティティに 0 または 1 を 1 つの関係。 一括読み込みのため、`OfficeAssignment`エンティティです。 一括読み込みは、関連のデータを表示する必要がある場合に通常より効率的です。 この場合、オフィスの割り当て、講習においてインストラクターが表示されます。
* ユーザーが関連するインストラクター (潔助前のイメージで) を選択すると`Course`エンティティが表示されます。 `Instructor`と`Course`エンティティに多対多の関係。 Eager を読み込み、`Course`エンティティとその関連`Department`エンティティを使用します。 ここでは、選択した講師のコースのみが必要なために、別々 にクエリがより効率的な可能性があります。 この例では、ナビゲーション プロパティのナビゲーション プロパティに含まれるエンティティの一括読み込みを使用する方法を示します。
* データに関連するユーザーのコース (前のイメージでは、化学) を選択すると、`Enrollments`エンティティが表示されます。 前のイメージで受講者名とクラスが表示されます。 `Course`と`Enrollment`エンティティに一対多の関係。

### <a name="create-a-view-model-for-the-instructor-index-view"></a>講師インデックス ビューのビュー モデルを作成します。

講習においてインストラクター ページでは、次の 3 つの異なるテーブルからデータを示します。 3 つのテーブルを表す 3 つのエンティティを含むビュー モデルが作成されます。

*SchoolViewModels*フォルダー作成*InstructorIndexData.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Scaffold インストラクター モデル

* Visual Studio を終了します。
* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* 次のコマンドを実行します。

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

上記のコマンド スキャフォールディング、`Instructor`モデル。 Visual Studio でプロジェクトを開きます。

プロジェクトをビルドします。 ビルドには、エラーが生成されます。

グローバルに変更する`_context.Instructor`に`_context.Instructors`(つまり、"s"を追加する`Instructor`)。 7 回の出現が検出され、更新します。

アプリケーションを実行し、インストラクターのページに移動します。

置き換える*Pages/Instructors/Index.cshtml.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

`OnGetAsync`メソッドは、選択した講師の ID を省略可能なルート データを受け付けます。

クエリを調べて、 *Pages/Instructors/Index.cshtml*ページ。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

2 つのクエリが含まれています。

* `OfficeAssignment`: に表示され、[講習においてインストラクター ビュー](#IP)です。
* `CourseAssignments`: これは、学習のコースが表示されます。


### <a name="update-the-instructors-index-page"></a>講習においてインストラクター インデックス ページを更新します。

更新*Pages/Instructors/Index.cshtml*次のマークアップ。

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

上記のマークアップでは、次の変更を加えます。

* 更新プログラム、`page`からディレクティブ`@page`に`@page "{id:int?}"`です。 `"{id:int?}"`ルート テンプレートです。 ルート テンプレートでは、ルート データを URL にクエリ文字列を整数値を変更します。 たとえばをクリックすると、**選択**ページ ディレクティブは、次のように URL を生成する際、インストラクターへのリンク。

    `http://localhost:1234/Instructors?id=2`

    ページ ディレクティブの場合は`@page "{id:int?}"`、以前の url:

    `http://localhost:1234/Instructors/2`

* ページ タイトルが**講習においてインストラクター**です。
* 追加、 **Office**を表示する列`item.OfficeAssignment.Location`場合にのみ、`item.OfficeAssignment`が null でないです。 これは、0 または 1 を 1 つのリレーションシップであるため、ある可能性がありますいない関連 OfficeAssignment エンティティです。

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* 追加、**コース**コースを表示する列が各インストラクターを学習します。 参照してください[行の明示的な移行`@:`](xref:mvc/views/razor#explicit-line-transition-with-)この razor 構文の詳細についてです。

* 動的に追加するコードを追加しました`class="success"`を`tr`選択した講師の要素。 これは、ブートス トラップのクラスを使用して、選択した行の背景色を設定します。

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* 新しいラベル付けされるハイパーリンクを追加**選択**です。 このリンクを送信する ID を選択したインストラクター、`Index`メソッドおよび背景色を設定します。

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

アプリの実行を選択して、**講習においてインストラクター**タブです。ページが表示されます、 `Location` (office) から、関連する`OfficeAssignment`エンティティです。 場合 OfficeAssignment' は null、空のテーブル セルが表示されます。

![講習においてインストラクター インデックス ページが何も選択](read-related-data/_static/instructors-index-no-selection.png)

をクリックして、**選択**リンクします。 行のスタイル変更します。

### <a name="add-courses-taught-by-selected-instructor"></a>選択した講師でコースを追加します。

更新プログラム、`OnGetAsync`メソッド*Pages/Instructors/Index.cshtml.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

更新されたクエリを確認してください。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

上記のクエリを追加、`Department`エンティティです。

講師が選択されているときに、次のコードが実行 (`id != null`)。 講習においてインストラクターにビューのモデルの一覧から選択した講師が取得されます。 ビュー モデルの`Courses`プロパティが読み込まれ、`Course`そのインストラクターからエンティティ`CourseAssignments`ナビゲーション プロパティ。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where`メソッドがコレクションを返します。 上記の「`Where`メソッドを 1 つだけ`Instructor`エンティティが返されます。 `Single`メソッドが、1 つに、コレクションを変換`Instructor`エンティティです。 `Instructor`エンティティへのアクセスを提供する、`CourseAssignments`プロパティです。 `CourseAssignments`関連するへのアクセスを提供`Course`エンティティです。

![講師-コース m:M](complex-data-model/_static/courseassignment.png)

`Single`メソッドは、コレクションは、1 つの項目と、コレクションを使用します。 `Single`コレクションが空の場合、または複数の項目がある場合、メソッドが例外をスローします。 代わりに`SingleOrDefault`既定の値を返す (この場合は null) コレクションが空の場合。 使用して`SingleOrDefault`空のコレクションに対して。

* 例外が発生 (から検索しようとして、 `Courses` null 参照のプロパティ)。
* 例外メッセージは、問題の原因と明確に小さいを示します。

次のコードには追加、ビュー モデルの`Enrollments`コースが選択されているプロパティ。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

末尾に次のマークアップを追加、 *Pages/Courses/Index.cshtml* Razor ページ。

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

上記のマークアップには、あるインストラクターが選択されている場合、インストラクターに関連するコースの一覧が表示されます。

アプリをテストします。 をクリックして、**選択**講習においてインストラクター ページ上のリンク。

![講習においてインストラクター インデックス ページ講師が選択されています。](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>学生のデータを表示します。

このセクションでは、選択したコースを受講者データを表示するアプリが更新されます。

クエリでは、更新、`OnGetAsync`メソッド*Pages/Instructors/Index.cshtml.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

更新*Pages/Instructors/Index.cshtml*です。 ファイルの末尾に次のマークアップを追加します。

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

上記のマークアップは、選択したこのコースで登録されている学生のリストを表示します。

ページを更新し、インストラクターを選択します。 登録済みの受講者との成績の一覧を表示するコースを選択します。

![講習においてインストラクター インデックス ページ インストラクターと選択したコース](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>1 つを使用します。

`Single`メソッドを渡すことができます、`Where`条件呼び出す代わりに、`Where`メソッドとは別に。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

前述の`Single`アプローチも利点は使用する場合より`Where`です。 一部の開発者が必要に応じて、`Single`スタイルに近づきます。

## <a name="explicit-loading"></a>明示的読み込み

現在のコードを指定の一括読み込み`Enrollments`と`Students`:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

ユーザーはほとんどコースに登録を表示するとします。 その場合は、最適化は要求された場合のみ登録データを読み込むことです。 このセクションで、`OnGetAsync`の明示的な読み込みを使用する更新`Enrollments`と`Students`です。

更新プログラム、`OnGetAsync`を次のコード。

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

上記のコードを削除、 *ThenInclude*の登録と学生データ メソッドを呼び出します。 コースを選択すると、強調表示されたコードを取得します。

* `Enrollment`選択コースのエンティティです。
* `Student`各エンティティ`Enrollment`です。

前述のコードをコメントに注意してください。`.AsNoTracking()`です。 追跡対象のエンティティのナビゲーション プロパティが明示的に読み込まれただけことができます。

アプリをテストします。 ユーザーの観点から、アプリの動作は同じです、以前のバージョンです。

次のチュートリアルでは、関連するデータを更新する方法を示します。

>[!div class="step-by-step"]
>[前へ](xref:data/ef-rp/complex-data-model)
>[次へ](xref:data/ef-rp/update-related-data)