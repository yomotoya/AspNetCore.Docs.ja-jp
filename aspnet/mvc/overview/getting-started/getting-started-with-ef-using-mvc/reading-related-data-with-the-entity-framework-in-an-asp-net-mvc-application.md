---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーションに Entity Framework と関連するデータの読み取り |Microsoft ドキュメント
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 06784d8b610856e71eae78b0db2d0253faedb955
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>関連する Entity Framework、ASP.NET MVC アプリケーションでのデータを読み取り
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


前のチュートリアルでは、データの School モデルを完了しました。 このチュートリアルではあります読み取りおよび、関連するデータを表示: つまり、ナビゲーション プロパティに、Entity Framework が読み込まれるデータ。

以下の図は、使用するページを示しています。

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>関連するデータの遅延、Eager、および明示的な読み込み

いくつかの方法が、Entity Framework が、エンティティのナビゲーション プロパティに関連するデータを読み込むことができます。

- *遅延読み込み*。 エンティティが最初に読み込まれるときに、関連データは取得されません。 ただし、ナビゲーション プロパティに初めてアクセスしようとすると、そのナビゲーション プロパティに必要なデータが自動的に取得されます。 これは、結果、データベースに送信される複数のクエリで — と 1 つ、エンティティ自体に関連したエンティティのデータを毎回を取得する必要があります。 `DbContext`クラスには、既定では遅延読み込みができるようにします。 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *一括読み込み*。 エンティティが読み取られるときに、関連データがエンティティと共に取得されます。 これは通常、必要なデータをすべて取得する 1 つの結合クエリになります。 一括読み込みを使用して指定する、`Include`メソッドです。

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *明示的読み込み*。 これは、似ていますが、遅延読み込みを明示的にデータを取得する、関連するコードです。ナビゲーション プロパティにアクセスするときに自動的に発生しません。 エンティティと呼び出し元のオブジェクトの状態マネージャー エントリを取得することによって、関連するデータを手動で読み込む、 [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx)のコレクションのメソッドまたは[Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx)メソッドを保持するプロパティを1 つのエンティティです。 (次の例では、管理者のナビゲーション プロパティをロードする場合は置き換えて`Collection(x => x.Courses)`で`Reference(x => x.Administrator)`)。通常、遅延読み込みを有効にした場合にのみ、明示的な読み込みを使用します。

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

すぐに、プロパティの値を取得する、ので遅延読み込みと明示的な読み込みは両方とも呼ばれます*遅延読み込み*です。

### <a name="performance-considerations"></a>パフォーマンスに関する考慮事項

取得したすべてのエンティティの関連データが必要な場合は、通常、データベースに送信された 1 つのクエリの方が、取得した各エンティティに対する分離したクエリよりも効率的なため、一括読み込みを使用すると、より最適なパフォーマンスが得られます。 たとえば、前述の例では、各部門に 10 個の関連コースがあるとします。 一括読み込みの例になる単なる (結合) を 1 つのクエリと 1 つのラウンド トリップをデータベースにします。 遅延読み込みと明示的な読み込みの例が両方とも 11 個のクエリと 11 個のラウンド トリップでデータベースにします。 データベースとの余分なラウンド トリップは、特に待ち時間が長いときのパフォーマンスに悪影響をもたらします。

その一方で、一部のシナリオで遅延読み込みが効率的です。 一括読み込みには、非常に複雑な結合を生成するのには SQL Server を効率的に処理できない可能性があります。 または、エンティティのセットのサブセットについてのみ、エンティティのナビゲーション プロパティにアクセスする必要がある場合を処理している場合、一括読み込みが必要以上のデータが取得されるため、遅延読み込みが向上します。 パフォーマンスが重要な場合、最適な選択を行うために、両方の方法でパフォーマンスをテストすることをお勧めします。

遅延読み込みは、パフォーマンスの問題が発生するコードをマスクできます。 たとえば、eager または明示的な読み込みが指定されていないが、大量のエンティティを処理し、各イテレーションでいくつかのナビゲーション プロパティを使用するコードできない可能性があります非常に効率的な (データベースに多くのラウンド トリップ) が原因です。 内部設置型 SQL server を使用した開発にもを実行するアプリケーションでは、待機時間の増加と遅延読み込みのための Azure SQL データベースに移動すると、パフォーマンスの問題があります。 プロファイルの負荷が現実的なテスト データベース クエリは遅延読み込みが適切なかどうかを判断するに役立ちます。 詳細については、次を参照してください。 [Demystifying Entity Framework 戦略: 関連するデータの読み込み](https://msdn.microsoft.com/magazine/hh205756.aspx)と[SQL Azure へのネットワーク待機時間の削減に Entity Framework を使用して](https://msdn.microsoft.com/magazine/gg309181.aspx)です。

### <a name="disable-lazy-loading-before-serialization"></a>シリアル化する前に遅延読み込みを無効にします。

しない場合は遅延読み込み有効なシリアル化中は、意図したものと非常に多くのデータのクエリを終了できます。 シリアル化は、型のインスタンス上の各プロパティにアクセスして、通常は動作します。 レイジー読み込まれたエンティティをシリアル化し、プロパティへのアクセスは、遅延読み込みをトリガーします。 さらに多くの遅延読み込みとシリアル化が発生する可能性、遅延読み込みのエンティティの各プロパティがシリアル化プロセスにアクセスします。 この長時間応答しない連鎖反応を防ぐためには、レイジー エンティティをシリアル化する前の読み込みをオフにします。

シリアル化できますも困難になる Entity Framework を使用して、プロキシ クラスによって」の説明に従って、[高度なシナリオのチュートリアル](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies)です。

シリアル化の問題を回避する方法の 1 つが、エンティティ オブジェクトの代わりにデータ転送オブジェクト (Dto) をシリアル化のように、 [Entity Framework と Web API を使用して](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md)チュートリアルです。

Dtos の使用を使用しない場合は、遅延読み込みを無効にし、プロキシの問題を回避[プロキシの作成を無効にする](https://msdn.microsoft.com/data/jj592886.aspx)です。

ここでは、その他の[遅延読み込みを無効にする方法は](https://msdn.microsoft.com/data/jj574232):

- 特定のナビゲーション プロパティに対して省略、`virtual`キーワード、プロパティを宣言するときにします。
- すべてのナビゲーション プロパティでは、次のように設定します。`LazyLoadingEnabled`に`false`、コンテキスト クラスのコンス トラクターに次のコードを配置します。 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>コース ページにその表示部門名を作成します。

`Course`エンティティには含むナビゲーション プロパティが含まれています、`Department`コースに割り当てられている部門のエンティティです。 コースの一覧で、割り当てられている部門の名前を表示するのには、取得する必要があります、`Name`プロパティから、`Department`エンティティは、`Course.Department`ナビゲーション プロパティ。

という名前のコント ローラーを作成する`CourseController`(CoursesController されません) の`Course`と同じオプションを使用して、エンティティ型、 **MVC 5 コント ローラー、ビューがある Entity Framework を使用して**の前に行ったscaffolder`Student`コント ローラーは、次の図に示すようにします。

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

開いている*Controllers\CourseController.cs*見ると、`Index`メソッド。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

自動スキャフォールディングでは、`Include` メソッドを使って、`Department` ナビゲーション プロパティに一括読み込みを指定しています。

開いている*Views\Course\Index.cshtml*テンプレート コードを次のコードに置き換えます。 変更が強調表示されています。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

スキャフォールディング コードに、次の変更を行いました。

- 見出しが変更**インデックス**に**コース**です。
- `CourseID` プロパティ値を示す **Number** 列が追加されました。 既定では、通常はエンドユーザーにとって意味のないために、主キーはスキャフォールディングされました。 ただし、このケースでは、主キーは意味があり、表示する必要があります。
- 移動、**部門**を右側に列の見出しを変更します。 Scaffolder が正しく表示する選択、`Name`プロパティから、`Department`エンティティが列見出しにする必要がありますコースのページに**部門**なく**名前**です。

Department 列のスキャフォールディング コードが表示されることを確認、`Name`のプロパティ、`Department`に読み込まれるエンティティ、`Department`ナビゲーション プロパティ。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

ページを実行 (選択、**コース**Contoso 大学のホーム ページのタブ) リスト部署名を表示します。

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>コースおよびまで登録数を示す講習においてインストラクター ページを作成します。

このセクションでをコント ローラーを作成し、表示、`Instructor`講習においてインストラクター ページを表示するためにエンティティ。

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

このページは、次の方法で関連データを読み取って表示します。

- 講習においてインストラクターの一覧から関連するデータが表示されます、`OfficeAssignment`エンティティです。 `Instructor` エンティティと `OfficeAssignment` エンティティは、一対ゼロまたは一対一のリレーションシップです。 一括読み込みを使用して、`OfficeAssignment`エンティティです。 前述のように、通常、一括読み込みは、主テーブルで取得したすべての行の関連データが必要なときにより効率的です。 このケースでは、割り当てられたすべてのインストラクターのオフィスの割り当てを表示する必要があります。
- ユーザーが関連する、インストラクターを選択すると`Course`エンティティが表示されます。 `Instructor` エンティティと `Course` エンティティは多対多リレーションシップです。 一括読み込みを使用して、`Course`エンティティとその関連`Department`エンティティです。 ここでは、選択した講師に対してのみコースする必要があるために、遅延読み込みがより効率的な可能性があります。 ただし、この例では、ナビゲーション プロパティにあるエンティティ内のナビゲーション プロパティに一括読み込みを使用する方法を示します。
- ユーザーは、コースを選択するときに関連するデータを`Enrollments`エンティティ セットが表示されます。 `Course` エンティティと `Enrollment` エンティティは一対多リレーションシップです。 追加の明示的な読み込み`Enrollment`エンティティとその関連`Student`エンティティです。 (明示的な読み込みは必要ありませんので、遅延読み込みが有効になっているが、この明示的な読み込みを行う方法を示しています。)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>講師インデックス ビューのビュー モデルを作成します。

講習においてインストラクター ページは、次の 3 つの異なるテーブルを示しています。 そのため、テーブルの 1 つにデータを保持するごとに、3 つのプロパティを含むビュー モデルを作成します。

*ViewModels*フォルダー作成*InstructorIndexData.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>講師コント ローラーとビューを作成します。

作成、 `InstructorController` (InstructorsController されません)、次の図に示すように、EF 読み取り/書き込みアクションがコント ローラー。

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

開いている*Controllers\InstructorController.cs*を追加し、`using`のステートメント、`ViewModels`名前空間。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

スキャフォールディングのコード、`Index`メソッドに対してのみ、一括読み込みを指定する、`OfficeAssignment`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

置換、`Index`追加を読み込むには、次のコードを持つメソッドに関連したデータとビュー モデルに格納します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

メソッドは省略可能なルート データを受け入れます (`id`) およびクエリ文字列パラメーター (`courseID`) を選択したインストラクターと、選択したコースの ID 値を指定して、ビューにすべての必要なデータを渡します。 パラメーターは、ページの **Select** ハイパーリンクによって指定されます。

このコードは、ビュー モデルのインスタンスを作成し、インストラクターのリストに配置することから始めます。 コードでの一括読み込みを指定、`Instructor.OfficeAssignment`と`Instructor.Courses`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

2 番目`Include`メソッド コース、読み込んでが読み込まれる各コースには一括読み込みの場合、`Course.Department`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

前述のように、一括読み込みは必要ありませんが、パフォーマンスを向上させるために行われます。 ビューが常に必要なため、`OfficeAssignment`エンティティを同じクエリでフェッチする方が効率的です。 `Course` 一括読み込みが、ページが表示されるより多くの場合、コースよりも選択されている場合にのみ、遅延読み込みよりも良いように web ページにあるインストラクターが選択されているときに、エンティティが必要です。

講師 ID を選択した場合は、選択した講師が講習においてインストラクターにビューのモデルの一覧から取得されます。 ビュー モデルの`Courses`でプロパティが読み込まれてから、`Course`そのインストラクターからエンティティ`Courses`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where`メソッドのコレクションを返しますが、条件が 1 つだけでそのメソッドの結果に渡されるここでは`Instructor`返されるエンティティです。 `Single`メソッドが、1 つに、コレクションを変換`Instructor`エンティティで、そのエンティティにアクセスできる`Courses`プロパティです。

使用する、[単一](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx)メソッドのコレクションがわかっている場合にコレクションを 1 つの項目になります。 `Single`に渡されるコレクションが空の場合、または複数の項目がある場合、メソッドが例外をスローします。 代わりに[SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)、既定値が返されます (`null`ここでは)、コレクションが空の場合。 ただし、ここではするがまだ例外が発生 (から検索しようとしています、`Courses`プロパティを`null`参照)、例外メッセージは、問題の原因を示す小さい明確にします。 呼び出すと、`Single`メソッドに渡すことも、`Where`条件呼び出す代わりに、`Where`メソッドとは別に。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

これは次のコードの代わりに使用します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

次に、コースが選択された場合、選択したコースはビュー モデルのコースのリストから取得されます。 モデルの表示の`Enrollments`プロパティが読み込まれ、`Enrollment`コースからエンティティ`Enrollments`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>講師インデックス ビューを変更します。

*Views\Instructor\Index.cshtml*、テンプレート コードを次のコードに置き換えます。 変更が強調表示されています。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

既存のコードに次の変更を行いました。

- モデル クラスが `InstructorIndexData` に変更されました。
- **Index** のページ タイトルが **Instructors** に変更されました。
- 追加、 **Office**を表示する列`item.OfficeAssignment.Location`場合にのみ、`item.OfficeAssignment`が null でないです。 (これは、0 または 1 を 1 つのリレーションシップであるため、できない場合があります、関連する`OfficeAssignment`エンティティです)。 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- 動的に追加するコードを追加しました`class="success"`を`tr`選択した講師の要素。 これにより、設定を使用して、選択した行の背景色、[ブートス トラップ](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)クラスです。 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- 追加された新しい`ActionLink`というラベルの付いた**選択**各行の他のリンクの直前に送信される、選択した講師 ID が、`Index`メソッドです。

アプリケーションの実行を選択して、**講習においてインストラクター**タブです。ページが表示されます、`Location`関連のプロパティ`OfficeAssignment`エンティティと空のテーブル セルがある場合に関連しない`OfficeAssignment`エンティティです。

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

*Views\Instructor\Index.cshtml*の終了後にファイルを`table`(末尾の要素ファイルの)、次のコードを追加します。 このコードでは、インストラクターが選択されたときに、インストラクターに関連するコースのリストを表示します。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

このコードでは、ビュー モデルの `Courses` プロパティを読み取り、コースのリストを表示します。 用意されています、`Select`ハイパーリンクを選択したコースの ID を送信する、`Index`アクション メソッド。

ページを実行し、インストラクターを選択します。 選択したインストラクターに割り当てられたコースを表示するグリッドを表示し、各コースに割り当てられた部門の名前を表示します。

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

追加したコード ブロックの後に、次のコードを追加します。 このコードは、コースを選択したときに、コースに登録されている受講者のリストを表示します。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

このコードを読み取り、`Enrollments`コースに受講者の一覧を表示するために、ビュー モデルのプロパティを登録します。

ページを実行し、インストラクターを選択します。 次に、コースを選択して、登録済みの受講者とその成績のリストを表示します。

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>明示的な読み込みの追加

開いている*InstructorController.cs*見ますが、どのように`Index`メソッド選択コースの「登録」リストを取得。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

一括読み込みを指定した講習においてインストラクターの一覧を取得するときに、`Courses`ナビゲーション プロパティと、`Department`コースごとのプロパティです。 配置する、`Courses`コレクションにおいて、ビュー モデルにアクセスしているので、`Enrollments`そのコレクション内の 1 つのエンティティのナビゲーション プロパティ。 一括読み込みを指定していないため、`Course.Enrollments`ナビゲーション プロパティ、遅延読み込みの結果 ページでそのプロパティからデータが表示されます。

その他の方法でコードを変更することがなく遅延読み込みを無効にした場合、`Enrollments`プロパティはコースが実際にどのくらい登録に関係なく null になります。 その場合、読み込みに、`Enrollments`プロパティ、一括読み込みまたは明示的な読み込みのいずれかを指定する必要があります。 一括読み込みを行う方法は、既に説明しました。 明示的な読み込みの例を表示するのには、置換、`Index`メソッドを次のコードは、明示的に読み込む、`Enrollments`プロパティです。 変更されたコードが強調表示されます。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

選択したを取得したら`Course`エンティティ、新しいコードがそのコースを明示的に読み込みます`Enrollments`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

明示的に読み込む各し、`Enrollment`エンティティの関連`Student`エンティティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

使用することを確認、`Collection`コレクション プロパティを読み込みますが、使用する、1 つのエンティティを保持するプロパティの`Reference`メソッドです。

講師インデックス ページを今すぐ実行して表示されますなし ページで、表示される内容に違いが、データの取得方法を変更しました。

## <a name="summary"></a>まとめ

ナビゲーション プロパティに関連するデータを読み込むすべての 3 つ方法 (レイジー、eager、および明示的な) を使用したようになりました。 次のチュートリアルでは、関連データの更新方法を学習します。

このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。 新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

> [!div class="step-by-step"]
> [前へ](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [次へ](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
