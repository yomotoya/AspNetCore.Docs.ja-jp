---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (5/10) で、Entity framework 関連のデータの読み取り |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 750c49572e99c6ab8c84d65e4c18f8da9b5d304c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835247"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>関連データ (5/10) ASP.NET MVC アプリケーションで Entity Framework での読み取り
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。
> 
> > [!NOTE] 
> > 
> > を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。 一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、School データ モデルを完了しました。 このチュートリアルでを読み取るし、関連データを表示、つまり、Entity Framework がナビゲーション プロパティに読み込まれるデータ。

以下の図は、使用するページを示しています。

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>関連データの遅延、意欲、および明示的な読み込み

いくつかの方法が、Entity Framework がエンティティのナビゲーション プロパティに関連するデータを読み込むことができます。

- *遅延読み込み*。 エンティティが最初に読み込まれるときに、関連データは取得されません。 ただし、ナビゲーション プロパティに初めてアクセスしようとすると、そのナビゲーション プロパティに必要なデータが自動的に取得されます。 これは、データベースに送信する複数のクエリ結果: 1 つ、エンティティ自体は、1 つに関連したエンティティのデータを毎回を取得する必要があります。 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *一括読み込み*。 エンティティが読み取られるときに、関連データがエンティティと共に取得されます。 これは通常、必要なデータをすべて取得する 1 つの結合クエリになります。 一括読み込みを使用して指定する、`Include`メソッド。

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *明示的読み込み*。 はコードで、関連するデータを明示的に取得する点を除いて、遅延読み込みと同様にはナビゲーション プロパティにアクセスするときに自動的に発生しません。 エンティティおよび呼び出し元のオブジェクト状態マネージャーのエントリを取得することによって、関連するデータを手動で読み込む、`Collection.Load`のコレクションのメソッドまたは`Reference.Load`メソッドの 1 つのエンティティを保持するプロパティ。 (次の例では、管理者のナビゲーション プロパティをロードする場合は置換が`Collection(x => x.Courses)`で`Reference(x => x.Administrator)`)。

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

すぐに、プロパティの値を取得、ため、遅延読み込みと明示的な読み込みは両方とも呼ばれます*遅延読み込み*します。

一般に、わかっている場合必要関連データのすべてのエンティティを取得した、一括読み込みは、最適なパフォーマンスをデータベースに送信する 1 つのクエリが通常は分離したクエリの各エンティティを取得するよりも効率的であるためです。 たとえば、上記の例では、各部門に 10 個の関連コースとします。 一括読み込みの例になります (結合) を 1 つのクエリだけで、1 つラウンド トリップをデータベースにします。 遅延読み込みと明示的な読み込みの例は両方とも 11 個のクエリと 11 個のラウンド トリップでデータベースに。 データベースとの余分なラウンド トリップは、特に待ち時間が長いときのパフォーマンスに悪影響をもたらします。

その一方で、一部のシナリオで遅延読み込みが効率的です。 一括読み込み SQL Server を効率的に処理できない、生成される非常に複雑な結合が発生する可能性があります。 または、処理している一連のエンティティのサブセットについてのみ、エンティティのナビゲーション プロパティにアクセスする必要がある場合は、一括読み込みでは、必要以上に多くのデータを取得するために、遅延読み込みを優れて可能性があります。 パフォーマンスが重要な場合、最適な選択を行うために、両方の方法でパフォーマンスをテストすることをお勧めします。

通常、遅延読み込みを有効にした場合にのみ明示的な読み込みを使用します。 遅延読み込みをオフにすると、1 つのシナリオでは、シリアル化中にします。 遅延読み込みとシリアル化を混合しないでくださいと読み込みが有効になっている場合はないときに遅延するためのものよりもはるかに多くのデータのクエリを終了することができますに注意してください。 シリアル化は、通常、型のインスタンス上の各プロパティにアクセスする機能します。 プロパティへのアクセスは、遅延読み込みをトリガーし、それらの遅延の読み込まれたエンティティはシリアル化します。 さらに多くの遅延読み込みのシリアル化が発生する可能性、遅延読み込みのエンティティの各プロパティがシリアル化プロセスにアクセスします。 このが長時間応答しない連鎖反応を防ぐためには、遅延エンティティをシリアル化する前の読み込みをオフにします。

データベースのコンテキスト クラスは、既定では、遅延読み込みを実行します。 遅延読み込みを無効にする 2 つの方法はあります。

- 特定のナビゲーション プロパティでは、省略、`virtual`キーワード、プロパティを宣言するときにします。
- すべてのナビゲーション プロパティの設定`LazyLoadingEnabled`に`false`します。 たとえば、コンテキスト クラスのコンス トラクターで、次のコードを配置することができます。 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

遅延読み込みは、パフォーマンスの問題が発生するコードをマスクできます。 たとえば、eager または明示的な読み込みが指定されていないが、大量のエンティティを処理し、各イテレーションでいくつかのナビゲーション プロパティを使用するコードできない可能性があります非常に効率的な (データベースに多くのラウンド トリップ) が原因です。 オンプレミスの SQL server を使用した開発にも実行するアプリケーションを待機時間の増加と遅延読み込みのための Azure SQL Database に移動すると、パフォーマンスの問題があります。 現実的なテスト負荷をデータベースにクエリをプロファイリング遅延読み込みが適切なかどうかを判断する際に役立ちます。 詳細については、次を参照してください。 [Entity Framework 手法の分かりやすい解説: 関連データの読み込み](https://msdn.microsoft.com/magazine/hh205756.aspx)と[SQL Azure へのネットワーク待ち時間の短縮に Entity Framework を使用して](https://msdn.microsoft.com/magazine/gg309181.aspx)します。

## <a name="create-a-courses-index-page-that-displays-department-name"></a>コースのインデックス ページを作成するには、その表示部門名

`Course`エンティティには含むナビゲーション プロパティが含まれています、`Department`部門に割り当てられているコースのエンティティ。 コースのリストでは、割り当てられた部門の名前を表示するには、取得する必要があります、`Name`プロパティから、`Department`エンティティ内にある、`Course.Department`ナビゲーション プロパティ。

という名前のコント ローラーを作成する`CourseController`の`Course`前に実行したのと同じオプションを使用して、エンティティ型、`Student`コント ローラーで、次の図に示すように (ただし、イメージとは異なり、コンテキスト クラスは DAL 名前空間にはいないモデル名前空間):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

開いている*Controllers\CourseController.cs*を見て、`Index`メソッド。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

自動スキャフォールディングでは、`Include` メソッドを使って、`Department` ナビゲーション プロパティに一括読み込みを指定しています。

開いている*Views\Course\Index.cshtml*既存のコードを次のコードに置き換えます。 変更が強調表示されています。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

スキャフォールディング コードに、次の変更を行いました。

- 変更から見出し**インデックス**に**コース**します。
- 行へのリンクを左に移動します。
- 見出しの下の列が追加された**数**を示す、`CourseID`プロパティの値。 (既定では、主キー スキャフォールディングされませんので、通常はエンドユーザーにとって意味のないです。 ただし、ここでは、主キーは意味のある、非表示にする。)
- 最後の列見出しが変更された**DepartmentID** (への外部キーの名前、`Department`エンティティ) に**部門**します。

最後の列にスキャフォールディングされたコードが表示されます、`Name`のプロパティ、`Department`に読み込まれるエンティティ、`Department`ナビゲーション プロパティ。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

ページの実行 (選択、**コース**Contoso University のホーム ページのタブ) 部門名の一覧を表示します。

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>コースと登録を示す Instructors インデックス ページを作成します。

このセクションでをコント ローラーを作成し、表示、 `Instructor` Instructors/index ページを表示するためにエンティティ。

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

このページは、次の方法で関連データを読み取って表示します。

- インストラクターのリストから関連するデータが表示されます、`OfficeAssignment`エンティティ。 `Instructor` エンティティと `OfficeAssignment` エンティティは、一対ゼロまたは一対一のリレーションシップです。 一括読み込みを使用して、`OfficeAssignment`エンティティ。 前述のように、通常、一括読み込みは、主テーブルで取得したすべての行の関連データが必要なときにより効率的です。 このケースでは、割り当てられたすべてのインストラクターのオフィスの割り当てを表示する必要があります。
- ユーザーが関連する、インストラクターを選択すると`Course`エンティティが表示されます。 `Instructor` エンティティと `Course` エンティティは多対多リレーションシップです。 一括読み込みを使用して、`Course`エンティティとその関連`Department`エンティティ。 この場合、遅延読み込みをより効率的なためのみ、選択したインストラクターのコースを必要があります。 ただし、この例では、ナビゲーション プロパティにあるエンティティ内のナビゲーション プロパティに一括読み込みを使用する方法を示します。
- ユーザーは、コースを選択するときに関連するデータから、`Enrollments`エンティティ セットが表示されます。 `Course` エンティティと `Enrollment` エンティティは一対多リレーションシップです。 明示的な読み込みを追加します`Enrollment`エンティティとその関連`Student`エンティティ。 (明示的な読み込み必要はありませんので、遅延読み込みが有効になっているが、これは明示的な読み込みを行う方法を示しています。)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Instructor インデックス ビューのビュー モデルを作成します。

Instructor インデックス ページには、3 つのテーブルが表示されます。 そのため、テーブルの 1 つにデータを保持するごとに、3 つのプロパティを含むビュー モデルを作成します。

*ViewModels*フォルダー作成*InstructorIndexData.cs*既存のコードを次のコードに置き換えます。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>選択した行のスタイルの追加

選択した行をマークするには、別の背景色が必要です。 セクションに次の強調表示されたコードを追加してこの UI のスタイルを指定、`/* info and errors */`で*Content\Site.css*以下に示すように。

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Instructor コント ローラーとビューの作成

作成、`InstructorController`次の図に示すようにコント ローラー。

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

開いている*Controllers\InstructorController.cs*を追加し、`using`のステートメント、`ViewModels`名前空間。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

スキャフォールディングされたコードで、`Index`メソッドは、一括読み込みをのみを指定します、`OfficeAssignment`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

置換、`Index`メソッドを次のコードを追加で読み込む関連データとビュー モデルに配置します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

メソッドは省略可能なルート データを受け取ります (`id`) と、クエリ文字列パラメーター (`courseID`) を選択したインストラクターと選択したコースの ID 値を指定して、すべての必要なデータをビューに渡します。 パラメーターは、ページの **Select** ハイパーリンクによって指定されます。

> [!TIP]
> 
> **ルート データ**
> 
> ルート データは、モデル バインダーがルーティング テーブルで指定された URL セグメントであるデータです。 たとえば、既定のルートを指定します`controller`、 `action`、および`id`セグメント。
> 
> routes.MapRoute(  
>  名前:"Default"、  
>  url:"{controller}/{action}/{id}"、  
>  既定値: 新しい {コント ローラー アクションを"Home"= ="Index"id = UrlParameter.Optional}  
> );
> 
> 次の URL で既定のルートをマップ`Instructor`として、 `controller`、`Index`として、 `action` 1 を`id`。 これらは、ルート データの値。
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021"クエリ文字列値です。 モデル バインダーは、渡した場合でも機能、`id`クエリ文字列の値として。
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Url がによって作成された`ActionLink`Razor ビュー内のステートメント。 次のコードで、`id`のでパラメーターに既定のルートと一致する`id`ルート データに追加されます。
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> 次のコードで`courseID`既定のルートのパラメーターに一致しないため、クエリ文字列として追加されます。
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


このコードは、ビュー モデルのインスタンスを作成し、インストラクターのリストに配置することから始めます。 コードでの一括読み込みを指定、 `Instructor.OfficeAssignment` 、`Instructor.Courses`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

2 番目の`Include`メソッドは、コースを読み込み一括読み込みでは、読み込まれる各コースに対し、`Course.Department`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

前述のように、一括読み込みは必要ありませんが、パフォーマンスを向上させるために実行されます。 ビューが常に必要なため、`OfficeAssignment`エンティティ、同じクエリでフェッチする方が効率的です。 `Course` 一括読み込みはよりも選択されているコースで、ページがより多くの場合、表示された場合にのみ、遅延読み込みよりも優れていますので、web ページで、インストラクターが選択されたときに、エンティティが必要です。

インストラクターの ID が選択されている場合は、選択したインストラクターがビュー モデルのインストラクターのリストから取得されます。 ビュー モデルの`Courses`プロパティは、そこに、`Course`エンティティからそのインストラクターの`Courses`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where`メソッドは、コレクションを返しますが、1 つだけでそのメソッドの結果に渡された、条件の例ではこの`Instructor`返されるエンティティです。 `Single`メソッドは、1 つに、コレクションを変換します`Instructor`エンティティで、そのエンティティにアクセスできる`Courses`プロパティ。

使用する、[単一](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx)コレクションがわかっている場合、コレクションのメソッドが 1 つの項目が必要があります。 `Single`メソッドに渡されたコレクションが空の場合、または 1 つ以上の項目がある場合に例外をスローします。 代替策は[SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)、既定値を返す (`null`ここで) コレクションが空の場合。 ただし、ここではまだにより、例外 (検索しようとしてから、`Courses`プロパティを`null`参照)、例外メッセージが明確に示されない問題の原因とします。 呼び出すと、`Single`メソッドに渡すことも、`Where`呼び出す代わりに、条件、`Where`メソッドとは別に。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

これは次のコードの代わりに使用します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

次に、コースが選択された場合、選択したコースはビュー モデルのコースのリストから取得されます。 ビュー モデルの`Enrollments`プロパティが読み込まれ、`Enrollment`エンティティからそのコースの`Enrollments`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Instructor インデックス ビューの変更

*Views\Instructor\Index.cshtml*、既存のコードを次のコードに置き換えます。 変更が強調表示されています。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

既存のコードに次の変更を行いました。

- モデル クラスが `InstructorIndexData` に変更されました。
- **Index** のページ タイトルが **Instructors** に変更されました。
- 行のリンク列を左に移動します。
- 削除、 **FullName**列。
- 追加、 **Office**を表示する列`item.OfficeAssignment.Location`場合にのみ`item.OfficeAssignment`が null でないです。 (これは 0 または 1 に 1 つのリレーションシップであるためがない場合、関連する`OfficeAssignment`エンティティです)。 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- 動的に追加する追加コード`class="selectedrow"`を`tr`選択したインストラクターの要素。 これには、先ほど作成した CSS クラスを使用して、選択した行の背景色を設定します。 (、`valign`テーブルに複数の行の列を追加するときに、属性が、次のチュートリアルで役に立ちますにします)。 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- 新しい追加`ActionLink`というラベルの付いた**選択**これにより、各行の他のリンクの直前に送信される、選択したインストラクターの ID、`Index`メソッド。

アプリケーションを実行し、選択、 **Instructors**タブ。ページが表示されます、`Location`関連のプロパティ`OfficeAssignment`エンティティと空のテーブル セルがある場合に関連しない`OfficeAssignment`エンティティ。

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

*Views\Instructor\Index.cshtml*終了後にファイルを`table`(ファイルの末尾) にある要素は、次の強調表示されたコードを追加します。 これには、インストラクターが選択されたときに、インストラクターに関連するコースのリストが表示されます。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

このコードでは、ビュー モデルの `Courses` プロパティを読み取り、コースのリストを表示します。 用意されています、`Select`ハイパーリンクを選択したコースの ID を送信する、`Index`アクション メソッド。

> [!NOTE]
> *.Css*ファイルは、ブラウザーによってキャッシュされます。 アプリケーションを実行するときに、変更が見つからない場合は、ハードの更新 (CTRL キーを押しながらクリックすると、**更新** ボタン、または、ctrl キーを押しながら f5 キーを押します)。


ページを実行し、インストラクターを選択します。 選択したインストラクターに割り当てられたコースを表示するグリッドを表示し、各コースに割り当てられた部門の名前を表示します。

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

追加したコード ブロックの後に、次のコードを追加します。 このコードは、コースを選択したときに、コースに登録されている受講者のリストを表示します。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

このコードを読み取り、`Enrollments`コースに受講者の一覧を表示するには、ビュー モデルのプロパティを登録します。

ページを実行し、インストラクターを選択します。 次に、コースを選択して、登録済みの受講者とその成績のリストを表示します。

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>明示的読み込みを追加します。

開いている*InstructorController.cs*方法を確認`Index`メソッドは、選択したコースの登録の一覧を取得します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

一括読み込みを指定したインストラクターのリストを取得したときに、`Courses`ナビゲーション プロパティと、`Department`各コースのプロパティ。 配置する、`Courses`コレクション モデルでは、ビュー、およびアクセスするようになりました、`Enrollments`そのコレクション内の 1 つのエンティティからのナビゲーション プロパティ。 一括読み込みを指定していないため、`Course.Enrollments`プロパティからデータがナビゲーション プロパティでは、遅延読み込みの結果としてページに表示します。

他の方法でコードを変更することがなく遅延読み込みを無効にした場合、`Enrollments`プロパティは、コースが実際にいくつの登録に関係なく null になります。 読み込む場合、`Enrollments`プロパティ、一括読み込みと明示的な読み込みのいずれかを指定する必要があります。 一括読み込みを行う方法は、既に説明しました。 明示的な読み込みの例を確認するためには、置換、`Index`メソッドを次のコードを明示的に読み込みます、`Enrollments`プロパティ。 コードの変更が強調表示されます。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

選択した取得した後に`Course`エンティティ、新しいコードは、そのコースを明示的に読み込みます`Enrollments`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

明示的に読み込む各し`Enrollment`エンティティの関連`Student`エンティティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

使用すること、`Collection`コレクション プロパティを読み込みますが、使用する 1 つのエンティティを保持するプロパティの場合、`Reference`メソッド。 Instructor インデックス ページを今すぐ実行することができ、わかりますなし ページで、表示される内容の違い、データを取得する方法を変更しているにもかかわらずです。

## <a name="summary"></a>まとめ

ナビゲーション プロパティに関連するデータを読み込むすべての 3 つ方法 (レイジー、意欲、および明示的な) を使用したようになりました。 次のチュートリアルでは、関連データの更新方法を学習します。

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [前へ](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [次へ](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
