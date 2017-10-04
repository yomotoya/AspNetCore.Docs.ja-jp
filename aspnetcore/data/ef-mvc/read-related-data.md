---
title: "ASP.NET MVC を持つコアを EF Core - 読み取り関連データ - 10 の 6"
author: tdykstra
description: "このチュートリアルでは、読み取りを関連データ--Entity Framework は、ナビゲーション プロパティに読み込まれるデータを表示します。"
keywords: "ASP.NET Core、Entity Framework Core では、関連するデータの結合します。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 71fec30f-8ea7-4ca8-96e3-d2e26c5be44e
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 778ef976fdbef70684ca26b0c7c548ffcc83ee00
ms.sourcegitcommit: e45f8912ce32b4071bf7e83b8f8315cd8bba3520
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2017
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a>読み取りに関連したデータの ASP.NET Core MVC のチュートリアル (10 の 6) の EF コア

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。

前のチュートリアルでは、データの School モデルを完了しました。 このチュートリアルを読み取るし、関連データ--Entity Framework は、ナビゲーション プロパティに読み込まれるデータを表示します。

次の図で作業をしているページを表示します。

![インデックス ページのコース](read-related-data/_static/courses-index.png)

![講習においてインストラクター インデックス ページ](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Eager、明示的なおよび関連するデータの読み込み遅延

いくつかの方法をオブジェクト リレーショナル マッピング (ORM) ソフトウェアなどは Entity Framework は、エンティティのナビゲーション プロパティに関連するデータを読み込むことができます。

* 一括読み込みします。 エンティティが読み込まれると、それに伴う関連データを取得します。 通常、この結果、すべてのために必要なデータを取得する 1 つの結合クエリ。 使用して Entity Framework のコアで一括読み込みを指定する、`Include`と`ThenInclude`メソッドです。

  ![一括読み込みの例](read-related-data/_static/eager-loading.png)

  一部の個別のクエリでデータを取得するおよび EF「修正」ナビゲーション プロパティです。  つまり、EF は、以前に取得したエンティティのナビゲーション プロパティで属しているとは別に取得したエンティティを自動的に追加します。 関連するデータを取得するクエリでは、使用することができます、`Load`メソッドなど、リストまたはオブジェクトを返すメソッドではなく`ToList`または`Single`です。

  ![別のクエリ例](read-related-data/_static/separate-queries.png)

* 明示的な読み込みします。 エンティティが読み込まれると、関連データが取得されていません。 関連するデータを取得するコードを記述すると、必要なかどうか。 メソッドに別々 にクエリで一括読み込みの場合、複数のクエリ結果の読み込み明示的なデータベースに送信します。 違いは、明示的な読み込みでのコードが指定ロードするナビゲーション プロパティです。 Entity Framework Core 1.1 で使用することができます、`Load`明示的な読み込みを行うメソッドです。 例:

  ![明示的な読み込みの例](read-related-data/_static/explicit-loading.png)

* 遅延読み込みします。 エンティティが読み込まれると、関連データが取得されていません。 ただし、ナビゲーション プロパティにアクセスしようとすると、最初にそのナビゲーション プロパティに必要なデータが自動的に取得します。 クエリは、最初にナビゲーション プロパティからデータを取得しようとするたびにデータベースに送信します。 Entity Framework Core 1.0 は、遅延読み込みをサポートしていません。

### <a name="performance-considerations"></a>パフォーマンスに関する考慮事項

データベースに送信する 1 つのクエリが通常エンティティごとに取得される個別のクエリよりも効率的であるために、多くの場合、一括読み込み関連データが必要なすべてのエンティティを取得する場合に最高のパフォーマンスを提供します。 たとえば、部門ごとに 10 個の関連コースがあるとします。 すべての関連データの一括読み込みになる単なる (結合) を 1 つのクエリと 1 つのラウンド トリップをデータベースにします。 各部門のコースの個別のクエリをデータベースに 11 個のラウンド トリップになります。 データベースへの余分なラウンド トリップは、遅延が多場合に特にパフォーマンスを低下させます。

その一方で、一部のシナリオで別々 にクエリが効率的です。 1 つのクエリ内のすべての関連するデータの一括読み込みには、非常に複雑な結合を生成するのには SQL Server を効率的に処理できない可能性があります。 または、エンティティのナビゲーション プロパティを処理しているエンティティのセットのサブセットについてのみにアクセスする必要がある場合に別々 にクエリ パフォーマンスが向上するため、事前にすべての一括読み込みする必要がありますよりも多くのデータを取得します。 パフォーマンスが重要な場合は、パフォーマンスを最適な選択を行うために両方の方法をテストすることをお勧めします。

## <a name="create-a-courses-page-that-displays-department-name"></a>部門名が表示されるコース ページを作成します。

Course エンティティには、コースに割り当てられている部署の部門エンティティを含むナビゲーション プロパティが含まれています。 コースの一覧で、割り当てられている部門の名前を表示するのには、内にある [部門] エンティティの Name プロパティを取得する必要があります、`Course.Department`ナビゲーション プロパティ。

CoursesController と同じオプションを使用して、Course エンティティ型の名前が付いたコント ローラーを作成、 **Entity Framework を使用して、ビューがある MVC コント ローラー**以前に行っていた生徒コント ローラーのように scaffolder、次の図では:

![コースのコント ローラーを追加します。](read-related-data/_static/add-courses-controller.png)

開いている*CoursesController.cs*を調べると、`Index`メソッドです。 自動のスキャフォールディングがの一括読み込みを指定、`Department`ナビゲーション プロパティを使用して、`Include`メソッドです。

置換、`Index`メソッドを次のコードをより適切な名前を使用する、`IQueryable`コース エンティティを返す (`courses`の代わりに`schoolContext`)。

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

開いている*Views/Courses/Index.cshtml*テンプレート コードを次のコードに置き換えます。 変更が強調表示されます。

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

スキャフォールディングのコードに、次の変更を加えた。

* インデックスからのコースに見出しを変更します。

* 追加、**数**を表示する列、`CourseID`プロパティの値。 既定では、通常はエンドユーザーにとって意味のないために、主キーはスキャフォールディングされました。 ただし、ここでは、主キーは無効と非表示にします。

* 変更、**部門**部門名を表示する列。 コードの表示、`Name`に読み込まれる部門 エンティティのプロパティ、`Department`ナビゲーション プロパティ。

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

アプリの実行を選択して、**コース**部署名を使用してリストを表示するタブです。

![インデックス ページのコース](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>コースおよびまで登録数を示す講習においてインストラクター ページを作成します。

このセクションでは、講習においてインストラクター ページを表示するためにコント ローラーと Instructor エンティティのビューを作成します。

![講習においてインストラクター インデックス ページ](read-related-data/_static/instructors-index.png)

このページは、読み取りし、次のように関連するデータを表示します。

* 講習においてインストラクターの一覧には、OfficeAssignment エンティティから関連するデータが表示されます。 0 または 1 を 1 つのリレーションシップは、インストラクター OfficeAssignment エンティティです。 OfficeAssignment エンティティの一括読み込みを使用します。 前述のように、主テーブルの取得したすべての行に、関連するデータが必要なときに一括読み込みは通常より効率的です。 この場合、表示されているすべてのインストラクターのオフィスの割り当てを表示します。

* ユーザーは、あるインストラクターを選択するときに関連する一連のエンティティが表示されます。 多対多のリレーションシップは、インストラクター コースのエンティティです。 Course エンティティとそれらに関連する部署エンティティの一括読み込みを使用します。 この場合、講師が選択されているためだけのコースを必要なので、別のクエリがより効率的な可能性があります。 ただし、この例は、ナビゲーション プロパティではエンティティ内でナビゲーション プロパティの一括読み込みを使用する方法を示します。

* 選択すると、ユーザー、コース、登録のエンティティ セットから関連するデータが表示されます。 コースと登録のエンティティは、一対多リレーションシップでです。 登録のエンティティとその関連学生エンティティには、別々 にクエリを使用します。

### <a name="create-a-view-model-for-the-instructor-index-view"></a>講師インデックス ビューのビュー モデルを作成します。

講習においてインストラクター ページでは、次の 3 つの異なるテーブルからデータを示します。 そのため、3 つのプロパティを含むテーブルの 1 つのデータを保持している各ビュー モデルを作成します。

*SchoolViewModels*フォルダー作成*InstructorIndexData.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>講師コント ローラーとビューを作成します。

EF 読み取り/書き込みアクションの次の図に示すように、インストラクター コント ローラーを作成します。

![講習においてインストラクター コント ローラーを追加します。](read-related-data/_static/add-instructors-controller.png)

開いている*InstructorsController.cs*を使用して、追加し、ViewModels 名前空間のステートメント。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

インデックスのメソッドを関連するデータの一括読み込みを行うし、ビュー モデルに格納するには、次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

このメソッドは省略可能なルート データを受け取ります (`id`) およびクエリ文字列パラメーター (`courseID`)、選択した講師と選択したコースの ID 値を提供します。 パラメーターがによって提供される、**選択**ハイパーリンクをページ。

コードは、ビュー モデルのインスタンスを作成し、インストラクターの一覧に配置して開始します。 コードでの一括読み込みを指定、`Instructor.OfficeAssignment`と`Instructor.CourseAssignments`ナビゲーション プロパティ。 内で、`CourseAssignments`プロパティ、`Course`プロパティは、読み込まれた場合は、その、`Enrollments`と`Department`プロパティは、アンロード、および各`Enrollment`エンティティ、`Student`プロパティが読み込まれます。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

ビューには、OfficeAssignment エンティティが常に必要とするので、同じクエリでフェッチをより効率的です。 Course エンティティは、web ページにあるインストラクターが選択されているため、ページが表示されるより多くの場合、コースよりも選択されている場合にのみ、1 つのクエリは複数のクエリよりも高くときに必要があります。

コードが繰り返されます`CourseAssignments`と`Course`から 2 つのプロパティが必要なので`Course`です。 最初の文字列`ThenInclude`取得を呼び出して`CourseAssignment.Course`、 `Course.Enrollments`、および`Enrollment.Student`です。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

その時点でコードでは、別`ThenInclude`のナビゲーション プロパティでは`Student`、する必要はありません。 呼び出し元が`Include`経由で始まる`Instructor`を経由するチェーンここでも、この時間を指定する必要があるため、プロパティ`Course.Department`の代わりに`Course.Enrollments`です。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

次のコードは、あるインストラクターが選択されたときに実行します。 講習においてインストラクターにビューのモデルの一覧から選択した講師が取得されます。 ビュー モデルの`Courses`そのインストラクターから Course エンティティとプロパティが読み込まれて、`CourseAssignments`ナビゲーション プロパティ。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

`Where`メソッドのコレクションを返しますが、ここでは、条件は、そのメソッドの結果で返される単一インストラクター エンティティのみを渡されます。 します。 `Single`メソッドに変換し、コレクションにそのエンティティのアクセスできる単一の Instructor エンティティ`CourseAssignments`プロパティです。 `CourseAssignments`プロパティが含まれます`CourseAssignment`をのみ関連エンティティ`Course`エンティティです。

使用する、`Single`メソッドのコレクションがわかっている場合にコレクションを 1 つの項目になります。 渡されるコレクションが空の場合、または複数の項目がある場合、1 つのメソッドは例外をスローします。 代わりに`SingleOrDefault`既定の値を返す (この場合は null) コレクションが空の場合。 ただし、ここではするがまだ例外が発生 (を検索しようとしてから、`Courses`プロパティは null 参照を)、例外メッセージは、問題の原因を示す明確に小さいとします。 呼び出すと、`Single`メソッドを渡すこともできます、Where 句で条件を呼び出す代わりに、`Where`メソッドとは別に。

```csharp
.Single(i => i.ID == id.Value)
```

これは次のコードの代わりに使用します。

```csharp
.Where(I => i.ID == id.Value).Single()
```

次に、コースが選択されている場合は、選択したコースがビューのモデルでコースの一覧から取得されます。 モデルの表示の`Enrollments`コースから登録エンティティとプロパティが読み込まれる`Enrollments`ナビゲーション プロパティ。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>講師インデックス ビューを変更します。

*Views/Instructors/Index.cshtml*、テンプレート コードを次のコードに置き換えます。 変更が強調表示されます。

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

既存のコードに、次の変更を加えた。

* モデル クラスを変更`InstructorIndexData`です。

* ページのタイトルを変更**インデックス**に**講習においてインストラクター**です。

* 追加、 **Office**を表示する列`item.OfficeAssignment.Location`場合にのみ、`item.OfficeAssignment`が null でないです。 (これは、0 または 1 を 1 つのリレーションシップであるため、ある可能性がありますいない関連 OfficeAssignment エンティティです。)

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
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* 新しいラベル付けされるハイパーリンクを追加**選択**各行の他のリンクの直前に選択したインストラクター ID への送信を停止する、`Index`メソッドです。

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

アプリの実行を選択して、**講習においてインストラクター**タブです。関連 OfficeAssignment エンティティが存在しない場合は、関連 OfficeAssignment エンティティと空のテーブル セルの Location プロパティを表示します。

![講習においてインストラクター インデックス ページが何も選択](read-related-data/_static/instructors-index-no-selection.png)

*Views/Instructors/Index.cshtml* (末尾の要素ファイルの) の表に、終了した後のファイルは、次のコードを追加します。 このコードは、あるインストラクターが選択されている場合、インストラクターに関連するコースの一覧を表示します。

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

このコードを読み取り、`Courses`コースの一覧を表示するビューのモデルのプロパティです。 用意されています、**選択**ハイパーリンクを選択したコースの ID を送信する、`Index`アクション メソッド。

ページを更新し、インストラクターを選択します。 これで、選択したインストラクターに割り当てられているコースを表示するグリッドが表示し、する各コースに割り当てられている部署の名前を参照してください。

![講習においてインストラクター インデックス ページ講師が選択されています。](read-related-data/_static/instructors-index-instructor-selected.png)

追加したコード ブロックの後に、次のコードを追加します。 コースが選択されているときにコースに受講登録された者のリストを表示します。

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

このコードは、受講者コースに登録済みの一覧を表示するために、ビュー モデルの登録プロパティを読み取ります。

ページをもう一度更新し、インストラクターを選択します。 登録済みの受講者との成績の一覧を表示するコースを選択します。

![講習においてインストラクター インデックス ページ インストラクターと選択したコース](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>明示的読み込み

講習においてインストラクターの一覧を取得した*InstructorsController.cs*の一括読み込みを指定した、`CourseAssignments`ナビゲーション プロパティ。

まれでは、選択した講師コースの「登録」を参照するユーザーを意図したとします。 その場合は、要求された場合にのみ登録データを読み込むことができます。 明示的な読み込みを行う方法の例を表示するには、置換、`Index`メソッドを次のコードの登録の一括読み込みを削除し、そのプロパティを明示的に読み込みます。 コードの変更が強調表示されます。

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

新しいコードを削除、 *ThenInclude*インストラクター エンティティを取得するコードから登録データのメソッドを呼び出します。 インストラクターとコースが選択されている場合、強調表示されたコードは、選択したコースの登録のエンティティと各登録のための学生エンティティを取得します。

今すぐ講習においてインストラクター インデックス ページに移動して、アプリが表示されますなし ページで、表示される内容に違いが、データの取得方法を変更した実行します。

## <a name="summary"></a>概要

ここで使用した一括読み込み 1 つのクエリを使用して、複数のクエリを使用してナビゲーション プロパティに関連するデータを読み取る。 次のチュートリアルでは、関連するデータを更新する方法を学習します。

>[!div class="step-by-step"]
>[前へ](complex-data-model.md)
>[次へ](update-related-data.md)  
