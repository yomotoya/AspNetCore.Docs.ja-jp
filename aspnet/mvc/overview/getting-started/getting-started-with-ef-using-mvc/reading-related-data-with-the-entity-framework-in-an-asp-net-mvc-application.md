---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'チュートリアル: ASP.NET MVC アプリでの EF で関連データを読み取り'
description: このチュートリアルでを読み取るし、関連データを表示、つまり、Entity Framework がナビゲーション プロパティに読み込まれるデータ。
author: tdykstra
ms.author: riande
ms.date: 01/17/2019
ms.topic: tutorial
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8660a75655b801364cce7c4b59847c5c00562a27
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396208"
---
# <a name="tutorial-read-related-data-with-ef-in-an-aspnet-mvc-app"></a>チュートリアル: ASP.NET MVC アプリでの EF で関連データを読み取り

前のチュートリアルでは、School データ モデルを完了しました。 このチュートリアルでを読み取るし、関連データを表示、つまり、Entity Framework がナビゲーション プロパティに読み込まれるデータ。

以下の図は、使用するページを示しています。

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 関連データを読み込む方法について説明します
> * Courses ページを作成します。
> * Instructors ページを作成します。

## <a name="prerequisites"></a>必須コンポーネント

* [複雑なデータ モデルを作成します。](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="learn-how-to-load-related-data"></a>関連データを読み込む方法について説明します

いくつかの方法が、Entity Framework がエンティティのナビゲーション プロパティに関連するデータを読み込むことができます。

- *遅延読み込み*。 エンティティが最初に読み込まれるときに、関連データは取得されません。 ただし、ナビゲーション プロパティに初めてアクセスしようとすると、そのナビゲーション プロパティに必要なデータが自動的に取得されます。 これは、データベースに送信する複数のクエリ結果: 1 つ、エンティティ自体は、1 つに関連したエンティティのデータを毎回を取得する必要があります。 `DbContext`クラスは、既定で遅延読み込みを使用できます。

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *一括読み込み*。 エンティティが読み取られるときに、関連データがエンティティと共に取得されます。 これは通常、必要なデータをすべて取得する 1 つの結合クエリになります。 一括読み込みを使用して指定する、`Include`メソッド。

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *明示的読み込み*。 はコードで、関連するデータを明示的に取得する点を除いて、遅延読み込みと同様にはナビゲーション プロパティにアクセスするときに自動的に発生しません。 エンティティおよび呼び出し元のオブジェクト状態マネージャーのエントリを取得することによって、関連するデータを手動で読み込む、 [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx)のコレクションのメソッドまたは[Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx)を保持するプロパティのメソッド、1 つのエンティティ。 (次の例では、管理者のナビゲーション プロパティをロードする場合は置換が`Collection(x => x.Courses)`で`Reference(x => x.Administrator)`)。通常、遅延読み込みを有効にした場合にのみ明示的な読み込みを使用します。

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

すぐに、プロパティの値を取得、ため、遅延読み込みと明示的な読み込みは両方とも呼ばれます*遅延読み込み*します。

### <a name="performance-considerations"></a>パフォーマンスに関する考慮事項

取得したすべてのエンティティの関連データが必要な場合は、通常、データベースに送信された 1 つのクエリの方が、取得した各エンティティに対する分離したクエリよりも効率的なため、一括読み込みを使用すると、より最適なパフォーマンスが得られます。 たとえば、上記の例では、各部門に 10 個の関連コースとします。 一括読み込みの例になります (結合) を 1 つのクエリだけで、1 つラウンド トリップをデータベースにします。 遅延読み込みと明示的な読み込みの例は両方とも 11 個のクエリと 11 個のラウンド トリップでデータベースに。 データベースとの余分なラウンド トリップは、特に待ち時間が長いときのパフォーマンスに悪影響をもたらします。

その一方で、一部のシナリオで遅延読み込みが効率的です。 一括読み込み SQL Server を効率的に処理できない、生成される非常に複雑な結合が発生する可能性があります。 または、処理しているエンティティのセットのサブセットについてのみ、エンティティのナビゲーション プロパティにアクセスする必要がある場合は、一括読み込みでは、必要以上に多くのデータを取得するために、遅延読み込みを優れて可能性があります。 パフォーマンスが重要な場合、最適な選択を行うために、両方の方法でパフォーマンスをテストすることをお勧めします。

遅延読み込みは、パフォーマンスの問題が発生するコードをマスクできます。 たとえば、eager または明示的な読み込みが指定されていないが、大量のエンティティを処理し、各イテレーションでいくつかのナビゲーション プロパティを使用するコードできない可能性があります非常に効率的な (データベースに多くのラウンド トリップ) が原因です。 オンプレミスの SQL server を使用した開発にも実行するアプリケーションを待機時間の増加と遅延読み込みのための Azure SQL Database に移動すると、パフォーマンスの問題があります。 現実的なテスト負荷をデータベースにクエリをプロファイリング遅延読み込みが適切なかどうかを判断する際に役立ちます。 詳細については、次を参照してください。 [Entity Framework 手法の分かりやすい解説。関連データの読み込み](https://msdn.microsoft.com/magazine/hh205756.aspx)と[Entity Framework を使用して SQL Azure へのネットワーク待機時間を減らす](https://msdn.microsoft.com/magazine/gg309181.aspx)します。

### <a name="disable-lazy-loading-before-serialization"></a>シリアル化する前に遅延読み込みを無効にします。

ままにすると遅延読み込みが有効なシリアル化中に、意図したよりもはるかに多くのデータのクエリを終了できます。 シリアル化は、通常、型のインスタンス上の各プロパティにアクセスする機能します。 プロパティへのアクセスは、遅延読み込みをトリガーし、それらの遅延の読み込まれたエンティティはシリアル化します。 さらに多くの遅延読み込みのシリアル化が発生する可能性、遅延読み込みのエンティティの各プロパティがシリアル化プロセスにアクセスします。 このが長時間応答しない連鎖反応を防ぐためには、遅延エンティティをシリアル化する前の読み込みをオフにします。

シリアル化できますもによって煩雑になる Entity Framework が使用するプロキシ クラスで説明したように、[高度なシナリオ チュートリアル](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies)します。

シリアル化の問題を回避する方法の 1 つは、ように、エンティティ オブジェクトではなく、データ転送オブジェクト (Dto) のシリアル化には、 [Entity Framework で Web API を使用して](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md)チュートリアル。

Dto を使用していない場合は、遅延読み込みを無効にして、プロキシの問題を回避[プロキシの作成を無効にする](https://msdn.microsoft.com/data/jj592886.aspx)します。

ここでは、その他[遅延読み込みを無効にする](https://msdn.microsoft.com/data/jj574232):

- 特定のナビゲーション プロパティでは、省略、`virtual`キーワード、プロパティを宣言するときにします。
- すべてのナビゲーション プロパティの設定`LazyLoadingEnabled`に`false`、コンテキスト クラスのコンス トラクターに次のコードを配置します。

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page"></a>Courses ページを作成します。

`Course`エンティティには含むナビゲーション プロパティが含まれています、`Department`部門に割り当てられているコースのエンティティ。 コースのリストでは、割り当てられた部門の名前を表示するには、取得する必要があります、`Name`プロパティから、`Department`エンティティ内にある、`Course.Department`ナビゲーション プロパティ。

という名前のコント ローラーを作成する`CourseController`(CoursesController されません) の`Course`と同じオプションを使用して、エンティティ型、 **MVC 5 コント ローラーとビュー、Entity Framework を使用して**の前に行ったscaffolder`Student`コント ローラー。

| 設定 | [値] |
| ------- | ----- |
| モデル クラス | 選択**コース (ContosoUniversity.Models)** します。 |
| データ コンテキスト クラス | 選択**SchoolContext (ContosoUniversity.DAL)** します。 |
| コント ローラー名 | 入力*CourseController*します。 ここでも、いない*CoursesController*で、 *s*します。 選択した**コース (ContosoUniversity.Models)**、**コント ローラー名**値が自動的に設定されます。 値を変更する必要があります。 |

その他の既定値のままにし、コント ローラーを追加します。

開いている*Controllers\CourseController.cs*を見て、`Index`メソッド。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

自動スキャフォールディングでは、`Include` メソッドを使って、`Department` ナビゲーション プロパティに一括読み込みを指定しています。

開いている*Views\Course\Index.cshtml*テンプレート コードを次のコードに置き換えます。 変更が強調表示されています。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

スキャフォールディング コードに、次の変更を行いました。

- 変更から見出し**インデックス**に**コース**します。
- `CourseID` プロパティ値を示す **Number** 列が追加されました。 既定では、主キーは、通常はエンドユーザーにとって意味のないため、スキャフォールディングされません。 ただし、このケースでは、主キーは意味があり、表示する必要があります。
- 移動、**部門**を右側に列見出しを変更します。 スキャフォルダーは正しく表示する選択、`Name`プロパティから、 `Department` 、エンティティが列見出しにする必要がありますコース ページでここ**部門**なく**名前**します。

[部署] 列でのスキャフォールディングされたコードが表示されます、`Name`のプロパティ、`Department`に読み込まれるエンティティ、`Department`ナビゲーション プロパティ。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

ページの実行 (選択、**コース**Contoso University のホーム ページのタブ) 部門名の一覧を表示します。

## <a name="create-an-instructors-page"></a>Instructors ページを作成します。

このセクションでをコント ローラーを作成し、表示、 `Instructor` Instructors ページを表示するためにエンティティ。 このページは、次の方法で関連データを読み取って表示します。

- インストラクターのリストから関連するデータが表示されます、`OfficeAssignment`エンティティ。 `Instructor` エンティティと `OfficeAssignment` エンティティは、一対ゼロまたは一対一のリレーションシップです。 一括読み込みを使用して、`OfficeAssignment`エンティティ。 前述のように、通常、一括読み込みは、主テーブルで取得したすべての行の関連データが必要なときにより効率的です。 このケースでは、割り当てられたすべてのインストラクターのオフィスの割り当てを表示する必要があります。
- ユーザーが関連する、インストラクターを選択すると`Course`エンティティが表示されます。 `Instructor` エンティティと `Course` エンティティは多対多リレーションシップです。 一括読み込みを使用して、`Course`エンティティとその関連`Department`エンティティ。 この場合、遅延読み込みをより効率的なためのみ、選択したインストラクターのコースを必要があります。 ただし、この例では、ナビゲーション プロパティにあるエンティティ内のナビゲーション プロパティに一括読み込みを使用する方法を示します。
- ユーザーは、コースを選択するときに関連するデータから、`Enrollments`エンティティ セットが表示されます。 `Course` エンティティと `Enrollment` エンティティは一対多リレーションシップです。 明示的な読み込みを追加します`Enrollment`エンティティとその関連`Student`エンティティ。 (明示的な読み込み必要はありませんので、遅延読み込みが有効になっているが、これは明示的な読み込みを行う方法を示しています。)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Instructor インデックス ビューのビュー モデルを作成します。

Instructors ページには、3 つのテーブルが表示されます。 そのため、テーブルの 1 つにデータを保持するごとに、3 つのプロパティを含むビュー モデルを作成します。

*ViewModels*フォルダー作成*InstructorIndexData.cs*既存のコードを次のコードに置き換えます。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Instructor コント ローラーとビューを作成します。

作成、 `InstructorController` (InstructorsController されません) で EF の読み取り/書き込みアクションのコント ローラー。

| 設定 | [値] |
| ------- | ----- |
| モデル クラス | 選択**インストラクター (ContosoUniversity.Models)** します。 |
| データ コンテキスト クラス | 選択**SchoolContext (ContosoUniversity.DAL)** します。 |
| コント ローラー名 | 入力*InstructorController*します。 ここでも、いない*InstructorsController*で、 *s*します。 選択した**コース (ContosoUniversity.Models)**、**コント ローラー名**値が自動的に設定されます。 値を変更する必要があります。 |

その他の既定値のままにし、コント ローラーを追加します。

開いている*Controllers\InstructorController.cs*を追加し、`using`のステートメント、`ViewModels`名前空間。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

スキャフォールディングされたコードで、`Index`メソッドは、一括読み込みをのみを指定します、`OfficeAssignment`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

置換、`Index`メソッドを次のコードを追加で読み込む関連データとビュー モデルに配置します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

メソッドは省略可能なルート データを受け取ります (`id`) と、クエリ文字列パラメーター (`courseID`) を選択したインストラクターと選択したコースの ID 値を指定して、すべての必要なデータをビューに渡します。 パラメーターは、ページの **Select** ハイパーリンクによって指定されます。

このコードは、ビュー モデルのインスタンスを作成し、インストラクターのリストに配置することから始めます。 コードでの一括読み込みを指定、 `Instructor.OfficeAssignment` 、`Instructor.Courses`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

2 番目の`Include`メソッドは、コースを読み込み一括読み込みでは、読み込まれる各コースに対し、`Course.Department`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

前述のように、一括読み込みは必要ありませんが、パフォーマンスを向上させるために実行されます。 ビューが常に必要なため、`OfficeAssignment`エンティティ、同じクエリでフェッチする方が効率的です。 `Course` 一括読み込みはよりも選択されているコースで、ページがより多くの場合、表示された場合にのみ、遅延読み込みよりも優れていますので、web ページで、インストラクターが選択されたときに、エンティティが必要です。

インストラクターの ID が選択されている場合は、選択したインストラクターがビュー モデルのインストラクターのリストから取得されます。 ビュー モデルの`Courses`プロパティは、そこに、`Course`エンティティからそのインストラクターの`Courses`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where`メソッドは、コレクションを返しますが、1 つだけでそのメソッドの結果に渡された、条件の例ではこの`Instructor`返されるエンティティです。 `Single`メソッドは、1 つに、コレクションを変換します`Instructor`エンティティで、そのエンティティにアクセスできる`Courses`プロパティ。

使用する、[単一](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx)コレクションがわかっている場合、コレクションのメソッドが 1 つの項目が必要があります。 `Single`メソッドに渡されたコレクションが空の場合、または 1 つ以上の項目がある場合に例外をスローします。 代替策は[SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)、既定値を返す (`null`ここで) コレクションが空の場合。 ただし、ここではまだにより、例外 (検索しようとしてから、`Courses`プロパティを`null`参照)、例外メッセージが明確に示されない問題の原因とします。 呼び出すと、`Single`メソッドに渡すことも、`Where`呼び出す代わりに、条件、`Where`メソッドとは別に。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

これは次のコードの代わりに使用します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

次に、コースが選択された場合、選択したコースはビュー モデルのコースのリストから取得されます。 ビュー モデルの`Enrollments`プロパティが読み込まれ、`Enrollment`エンティティからそのコースの`Enrollments`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Instructor インデックス ビューを変更します。

*Views\Instructor\Index.cshtml*、テンプレート コードを次のコードに置き換えます。 変更が強調表示されています。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

既存のコードに次の変更を行いました。

- モデル クラスが `InstructorIndexData` に変更されました。
- **Index** のページ タイトルが **Instructors** に変更されました。
- 追加、 **Office**を表示する列`item.OfficeAssignment.Location`場合にのみ`item.OfficeAssignment`が null でないです。 (これは 0 または 1 に 1 つのリレーションシップであるためがない場合、関連する`OfficeAssignment`エンティティです)。

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- 動的に追加する追加コード`class="success"`を`tr`選択したインストラクターの要素。 これにより、設定を使用して、選択した行の背景色、[ブートス トラップ](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)クラス。

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- 新しい追加`ActionLink`というラベルの付いた**選択**これにより、各行の他のリンクの直前に送信される、選択したインストラクターの ID、`Index`メソッド。

アプリケーションを実行し、選択、 **Instructors**タブ。ページが表示されます、`Location`関連のプロパティ`OfficeAssignment`エンティティと空のテーブル セルがある場合に関連しない`OfficeAssignment`エンティティ。

*Views\Instructor\Index.cshtml*終了後にファイルを`table`(ファイルの末尾) にある要素は、次のコードを追加します。 このコードでは、インストラクターが選択されたときに、インストラクターに関連するコースのリストを表示します。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

このコードでは、ビュー モデルの `Courses` プロパティを読み取り、コースのリストを表示します。 用意されています、`Select`ハイパーリンクを選択したコースの ID を送信する、`Index`アクション メソッド。

ページを実行し、インストラクターを選択します。 選択したインストラクターに割り当てられたコースを表示するグリッドを表示し、各コースに割り当てられた部門の名前を表示します。

追加したコード ブロックの後に、次のコードを追加します。 このコードは、コースを選択したときに、コースに登録されている受講者のリストを表示します。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

このコードを読み取り、`Enrollments`コースに受講者の一覧を表示するには、ビュー モデルのプロパティを登録します。

ページを実行し、インストラクターを選択します。 次に、コースを選択して、登録済みの受講者とその成績のリストを表示します。

### <a name="adding-explicit-loading"></a>明示的読み込みを追加します。

開いている*InstructorController.cs*方法を確認`Index`メソッドは、選択したコースの登録の一覧を取得します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

一括読み込みを指定したインストラクターのリストを取得したときに、`Courses`ナビゲーション プロパティと、`Department`各コースのプロパティ。 配置する、`Courses`コレクション モデルでは、ビュー、およびアクセスするようになりました、`Enrollments`そのコレクション内の 1 つのエンティティからのナビゲーション プロパティ。 一括読み込みを指定していないため、`Course.Enrollments`プロパティからデータがナビゲーション プロパティでは、遅延読み込みの結果としてページに表示します。

他の方法でコードを変更することがなく遅延読み込みを無効にした場合、`Enrollments`プロパティは、コースが実際にいくつの登録に関係なく null になります。 読み込む場合、`Enrollments`プロパティ、一括読み込みと明示的な読み込みのいずれかを指定する必要があります。 一括読み込みを行う方法は、既に説明しました。 明示的な読み込みの例を確認するためには、置換、`Index`メソッドを次のコードを明示的に読み込みます、`Enrollments`プロパティ。 コードの変更が強調表示されます。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

選択した取得した後に`Course`エンティティ、新しいコードは、そのコースを明示的に読み込みます`Enrollments`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

明示的に読み込む各し`Enrollment`エンティティの関連`Student`エンティティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

使用すること、`Collection`コレクション プロパティを読み込みますが、使用する 1 つのエンティティを保持するプロパティの場合、`Reference`メソッド。

Instructor インデックス ページを今すぐ実行してわかりますなし ページで、表示される内容の違い、データを取得する方法を変更しているにもかかわらずです。

## <a name="additional-resources"></a>その他の技術情報

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 関連するデータを読み込む方法を学習しました。
> * Courses ページを作成します。
> * Instructors ページを作成

関連データを更新する方法については、次の記事に進んでください。

> [!div class="nextstepaction"]
> [関連データの更新](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)