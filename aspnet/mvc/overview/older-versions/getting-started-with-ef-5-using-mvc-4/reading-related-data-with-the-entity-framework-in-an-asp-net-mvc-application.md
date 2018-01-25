---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "ASP.NET MVC アプリケーション (10 の 5) で、Entity Framework と関連するデータの読み取り |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9093fb90a52b297f173c5cddb6f332d2d1a25135
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>関連する with ASP.NET MVC アプリケーション (10 の 5) で Entity Framework データの読み取り
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。
> 
> > [!NOTE] 
> > 
> > 解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、データの School モデルを完了しました。 このチュートリアルではあります読み取りおよび、関連するデータを表示: つまり、ナビゲーション プロパティに、Entity Framework が読み込まれるデータ。

次の図で作業をしているページを表示します。

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>関連するデータの遅延、Eager、および明示的な読み込み

いくつかの方法が、Entity Framework が、エンティティのナビゲーション プロパティに関連するデータを読み込むことができます。

- *遅延読み込み*です。 エンティティが読み込まれると、関連データが取得されていません。 ただし、ナビゲーション プロパティにアクセスしようとすると、最初にそのナビゲーション プロパティに必要なデータが自動的に取得します。 これは、結果、データベースに送信される複数のクエリで — と 1 つ、エンティティ自体に関連したエンティティのデータを毎回を取得する必要があります。 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *一括読み込み*です。 エンティティが読み込まれると、それに伴う関連データを取得します。 通常、この結果、すべてのために必要なデータを取得する 1 つの結合クエリ。 一括読み込みを使用して指定する、`Include`メソッドです。

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *明示的な読み込み*です。 これは、似ていますが、遅延読み込みを明示的にデータを取得する、関連するコードです。ナビゲーション プロパティにアクセスするときに自動的に発生しません。 エンティティと呼び出し元のオブジェクトの状態マネージャー エントリを取得することによって、関連するデータを手動で読み込む、`Collection.Load`のコレクションのメソッドまたは`Reference.Load`メソッドの 1 つのエンティティを保持するプロパティです。 (次の例では、管理者のナビゲーション プロパティをロードする場合は置き換えて`Collection(x => x.Courses)`で`Reference(x => x.Administrator)`)。

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

すぐに、プロパティの値を取得する、ので遅延読み込みと明示的な読み込みは両方とも呼ばれます*遅延読み込み*です。

一般に、わかっている場合必要関連データのすべてのエンティティを取得した、一括読み込みでは、最高のパフォーマンスをデータベースに送信する 1 つのクエリが通常エンティティごとに取得される個別のクエリよりも効率的であるためです。 たとえば、前述の例では、各部門に 10 個の関連コースがあるとします。 一括読み込みの例になる単なる (結合) を 1 つのクエリと 1 つのラウンド トリップをデータベースにします。 遅延読み込みと明示的な読み込みの例が両方とも 11 個のクエリと 11 個のラウンド トリップでデータベースにします。 データベースへの余分なラウンド トリップは、遅延が多場合に特にパフォーマンスを低下させます。

その一方で、一部のシナリオで遅延読み込みが効率的です。 一括読み込みには、非常に複雑な結合を生成するのには SQL Server を効率的に処理できない可能性があります。 またはエンティティのセットのサブセットについてのみ、エンティティのナビゲーション プロパティにアクセスする必要がある場合を処理している場合、一括読み込みが必要以上のデータが取得されるため、遅延読み込みが向上します。 パフォーマンスが重要な場合は、パフォーマンスを最適な選択を行うために両方の方法をテストすることをお勧めします。

通常、遅延読み込みを有効にした場合にのみ、明示的な読み込みを使用します。 遅延読み込みをオフにすると、1 つのシナリオでは、シリアル化中にします。 遅延読み込みとシリアル化しない、混在させるし、読み込みが有効になっていると遅延を意図したよりも大幅に多いデータを照会するが最終的に注意が必要でない場合。 シリアル化は、型のインスタンス上の各プロパティにアクセスして、通常は動作します。 レイジー読み込まれたエンティティをシリアル化し、プロパティへのアクセスは、遅延読み込みをトリガーします。 さらに多くの遅延読み込みとシリアル化が発生する可能性、遅延読み込みのエンティティの各プロパティがシリアル化プロセスにアクセスします。 この長時間応答しない連鎖反応を防ぐためには、レイジー エンティティをシリアル化する前の読み込みをオフにします。

データベース コンテキスト クラスは、既定では、遅延読み込みを実行します。 これには遅延読み込みを無効にする 2 つの方法があります。

- 特定のナビゲーション プロパティに対して省略、`virtual`キーワード、プロパティを宣言するときにします。
- すべてのナビゲーション プロパティでは、次のように設定します。`LazyLoadingEnabled`に`false`です。 たとえば、コンテキスト クラスのコンス トラクターで、次のコードを配置することができます。 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

遅延読み込みは、パフォーマンスの問題が発生するコードをマスクできます。 たとえば、eager または明示的な読み込みが指定されていないが、大量のエンティティを処理し、各イテレーションでいくつかのナビゲーション プロパティを使用するコードできない可能性があります非常に効率的な (データベースに多くのラウンド トリップ) が原因です。 内部設置型 SQL server を使用した開発にもを実行するアプリケーションでは、待機時間の増加と遅延読み込みのための Azure SQL データベースに移動すると、パフォーマンスの問題があります。 プロファイルの負荷が現実的なテスト データベース クエリは遅延読み込みが適切なかどうかを判断するに役立ちます。 詳細については、次を参照してください。 [Demystifying Entity Framework 戦略: 関連するデータの読み込み](https://msdn.microsoft.com/magazine/hh205756.aspx)と[SQL Azure へのネットワーク待機時間の削減に Entity Framework を使用して](https://msdn.microsoft.com/magazine/gg309181.aspx)です。

## <a name="create-a-courses-index-page-that-displays-department-name"></a>その表示部門名コース インデックス ページを作成します。

`Course`エンティティには含むナビゲーション プロパティが含まれています、`Department`コースに割り当てられている部門のエンティティです。 コースの一覧で、割り当てられている部門の名前を表示するのには、取得する必要があります、`Name`プロパティから、`Department`エンティティは、`Course.Department`ナビゲーション プロパティ。

という名前のコント ローラーを作成する`CourseController`の`Course`の前に行った同じオプションを使用して、エンティティ型、`Student`コント ローラーは、次の図に示すように (を除く、イメージとは異なり、コンテキスト クラスは、DAL 名前空間、いないモデル名前空間):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

開いている*Controllers\CourseController.cs*見ると、`Index`メソッド。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

自動のスキャフォールディングがの一括読み込みを指定、`Department`ナビゲーション プロパティを使用して、`Include`メソッドです。

開いている*Views\Course\Index.cshtml*し、既存のコードを次のコードに置き換えます。 変更が強調表示されます。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

スキャフォールディングのコードに、次の変更を加えた。

- 見出しが変更**インデックス**に**コース**です。
- 行へのリンクを左に移動します。
- 見出しの下の列を追加**数**を示す、`CourseID`プロパティの値。 (既定では、主キーはスキャフォールディングされた通常はエンドユーザーにとって意味のないためです。 ただし、ここでは、主キーは意味のある、非表示にする。)
- 最後の列見出しが変更**DepartmentID** (への外部キーの名前、`Department`エンティティ) に**部門**です。

最後の列のスキャフォールディング コードが表示されることを確認、`Name`のプロパティ、`Department`に読み込まれるエンティティ、`Department`ナビゲーション プロパティ。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

ページを実行 (選択、**コース**Contoso 大学のホーム ページのタブ) リスト部署名を表示します。

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>コースおよびまで登録数を示す講習においてインストラクター インデックス ページを作成します。

このセクションでをコント ローラーを作成し、表示、`Instructor`講習においてインストラクター インデックス ページを表示するためにエンティティ。

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

このページは、読み取りし、次のように関連するデータを表示します。

- 講習においてインストラクターの一覧から関連するデータが表示されます、`OfficeAssignment`エンティティです。 `Instructor`と`OfficeAssignment`エンティティに 0 または 1 を 1 つの関係。 一括読み込みを使用して、`OfficeAssignment`エンティティです。 前述のように、主テーブルの取得したすべての行に、関連するデータが必要なときに一括読み込みは通常より効率的です。 この場合、表示されているすべてのインストラクターのオフィスの割り当てを表示します。
- ユーザーが関連する、インストラクターを選択すると`Course`エンティティが表示されます。 `Instructor`と`Course`エンティティに多対多の関係。 一括読み込みを使用して、`Course`エンティティとその関連`Department`エンティティです。 ここでは、選択した講師に対してのみコースする必要があるために、遅延読み込みがより効率的な可能性があります。 ただし、この例は、ナビゲーション プロパティではエンティティ内でナビゲーション プロパティの一括読み込みを使用する方法を示します。
- ユーザーは、コースを選択するときに関連するデータを`Enrollments`エンティティ セットが表示されます。 `Course`と`Enrollment`エンティティに一対多の関係。 追加の明示的な読み込み`Enrollment`エンティティとその関連`Student`エンティティです。 (明示的な読み込みは必要ありませんので、遅延読み込みが有効になっているが、この明示的な読み込みを行う方法を示しています。)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>講師インデックス ビューのビュー モデルを作成します。

講師インデックス ページには、次の 3 つの異なるテーブルが表示されます。 そのため、3 つのプロパティを含むテーブルの 1 つのデータを保持している各ビュー モデルを作成します。

*ViewModels*フォルダー作成*InstructorIndexData.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>選択した行のスタイルを追加します。

選択した行をマークするには、別の背景色が必要です。 この UI のスタイルを提供するには、セクションに次の強調表示されたコードを追加`/* info and errors */`で*Content\Site.css*、次のようにします。

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>講師コント ローラーとビューの作成

作成、`InstructorController`次の図に示すように、コント ローラー。

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

開いている*Controllers\InstructorController.cs*を追加し、`using`のステートメント、`ViewModels`名前空間。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

スキャフォールディングのコード、`Index`メソッドに対してのみ、一括読み込みを指定する、`OfficeAssignment`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

置換、`Index`追加を読み込むには、次のコードを持つメソッドに関連したデータとビュー モデルに格納します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

メソッドは省略可能なルート データを受け入れます (`id`) およびクエリ文字列パラメーター (`courseID`) を選択したインストラクターと、選択したコースの ID 値を指定して、ビューにすべての必要なデータを渡します。 パラメーターがによって提供される、**選択**ハイパーリンクをページ。

> [!TIP] 
> 
> **ルート データ**
> 
> ルート データは、ルーティング テーブルに指定された URL セグメント内のモデル バインダーにあるデータです。 たとえば、既定のルートを指定して`controller`、 `action`、および`id`セグメント。
> 
> routes.MapRoute(  
>  名前:"Default"、  
>  url: "{controller}/{action}/{id}",  
>  既定値: new {コント ローラー = アクションでは「ホーム」="Index"id = UrlParameter.Optional}  
> );
> 
> 既定のルートでは、次の URL では、マップ`Instructor`として、 `controller`、`Index`として、 `action` 1 を`id`です。 これらは、ルート データの値。
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID 2021 ="クエリ文字列値です。 モデル バインダーは、合格した場合でも機能、`id`クエリ文字列値として。
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Url がによって作成された`ActionLink`Razor ビュー内のステートメント。 次のコードで、`id`ためパラメーターに既定のルートと一致する`id`ルート データを追加します。
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> 次のコードに`courseID`クエリ文字列として追加されたために、既定のルートのパラメーターと一致しません。
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


コードは、ビュー モデルのインスタンスを作成し、インストラクターの一覧に配置して開始します。 コードでの一括読み込みを指定、`Instructor.OfficeAssignment`と`Instructor.Courses`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

2 番目`Include`メソッド コース、読み込んでが読み込まれる各コースには一括読み込みの場合、`Course.Department`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

前述のように、一括読み込みは必要ありませんが、パフォーマンスを向上させるために行われます。 ビューが常に必要なため、`OfficeAssignment`エンティティを同じクエリでフェッチする方が効率的です。 `Course`一括読み込みが、ページが表示されるより多くの場合、コースよりも選択されている場合にのみ、遅延読み込みよりも良いように web ページにあるインストラクターが選択されているときに、エンティティが必要です。

講師 ID を選択した場合は、選択した講師が講習においてインストラクターにビューのモデルの一覧から取得されます。 ビュー モデルの`Courses`でプロパティが読み込まれてから、`Course`そのインストラクターからエンティティ`Courses`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where`メソッドのコレクションを返しますが、条件が 1 つだけでそのメソッドの結果に渡されるここでは`Instructor`返されるエンティティです。 `Single`メソッドが、1 つに、コレクションを変換`Instructor`エンティティで、そのエンティティにアクセスできる`Courses`プロパティです。

使用する、[単一](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx)メソッドのコレクションがわかっている場合にコレクションを 1 つの項目になります。 `Single`に渡されるコレクションが空の場合、または複数の項目がある場合、メソッドが例外をスローします。 代わりに[SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)、既定値が返されます (`null`ここでは)、コレクションが空の場合。 ただし、ここではするがまだ例外が発生 (から検索しようとしています、`Courses`プロパティを`null`参照)、例外メッセージは、問題の原因を示す小さい明確にします。 呼び出すと、`Single`メソッドに渡すことも、`Where`条件呼び出す代わりに、`Where`メソッドとは別に。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

これは次のコードの代わりに使用します。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

次に、コースが選択されている場合は、選択したコースがビューのモデルでコースの一覧から取得されます。 モデルの表示の`Enrollments`プロパティが読み込まれ、`Enrollment`コースからエンティティ`Enrollments`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>講師インデックス ビューを変更します。

*Views\Instructor\Index.cshtml*、既存のコードを次のコードに置き換えます。 変更が強調表示されます。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

既存のコードに、次の変更を加えた。

- モデル クラスを変更`InstructorIndexData`です。
- ページのタイトルを変更**インデックス**に**講習においてインストラクター**です。
- 行のリンクの列を左に移動します。
- 削除、 **FullName**列です。
- 追加、 **Office**を表示する列`item.OfficeAssignment.Location`場合にのみ、`item.OfficeAssignment`が null でないです。 (これは、0 または 1 を 1 つのリレーションシップであるため、できない場合があります、関連する`OfficeAssignment`エンティティです)。 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- 動的に追加するコードを追加しました`class="selectedrow"`を`tr`選択した講師の要素。 これは、先ほど作成した CSS クラスを使用して、選択した行の背景色を設定します。 (、`valign`属性が役に立ちます次のチュートリアルでは、テーブルに複数行の列を追加するとします)。 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- 追加された新しい`ActionLink`というラベルの付いた**選択**各行の他のリンクの直前に送信される、選択した講師 ID が、`Index`メソッドです。

アプリケーションの実行を選択して、**講習においてインストラクター**タブです。ページが表示されます、`Location`関連のプロパティ`OfficeAssignment`エンティティと空のテーブル セルがある場合に関連しない`OfficeAssignment`エンティティです。

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

*Views\Instructor\Index.cshtml*の終了後にファイルを`table`(最後のファイル)、要素は、次の強調表示されたコードを追加します。 これには、あるインストラクターが選択されている場合、インストラクターに関連するコースの一覧が表示されます。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

このコードを読み取り、`Courses`コースの一覧を表示するビューのモデルのプロパティです。 用意されています、`Select`ハイパーリンクを選択したコースの ID を送信する、`Index`アクション メソッド。

> [!NOTE]
> *.Css*ファイルがブラウザーによってキャッシュされます。 アプリケーションを実行するときに、変更が見つからない場合は、ハード更新 (CTRL キーを押しながらクリックして、**更新** ボタン、または、ctrl キーを押しながら F5 キーを押します)。


ページを実行し、インストラクターを選択します。 これで、選択したインストラクターに割り当てられているコースを表示するグリッドが表示し、する各コースに割り当てられている部署の名前を参照してください。

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

追加したコード ブロックの後に、次のコードを追加します。 コースが選択されているときにコースに受講登録された者のリストを表示します。

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

このコードを読み取り、`Enrollments`コースに受講者の一覧を表示するために、ビュー モデルのプロパティを登録します。

ページを実行し、インストラクターを選択します。 登録済みの受講者との成績の一覧を表示するコースを選択します。

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>明示的な読み込みの追加

開いている*InstructorController.cs*見ますが、どのように`Index`メソッド選択コースの「登録」リストを取得。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

一括読み込みを指定した講習においてインストラクターの一覧を取得するときに、`Courses`ナビゲーション プロパティと、`Department`コースごとのプロパティです。 配置する、`Courses`コレクションにおいて、ビュー モデルにアクセスしているので、`Enrollments`そのコレクション内の 1 つのエンティティのナビゲーション プロパティ。 一括読み込みを指定していないため、`Course.Enrollments`ナビゲーション プロパティ、遅延読み込みの結果 ページでそのプロパティからデータが表示されます。

その他の方法でコードを変更することがなく遅延読み込みを無効にした場合、`Enrollments`プロパティはコースが実際にどのくらい登録に関係なく null になります。 その場合、読み込みに、`Enrollments`プロパティ、一括読み込みまたは明示的な読み込みのいずれかを指定する必要があります。 一括読み込みを行う方法は、既に説明しました。 明示的な読み込みの例を表示するのには、置換、`Index`メソッドを次のコードは、明示的に読み込む、`Enrollments`プロパティです。 変更されたコードが強調表示されます。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

選択したを取得したら`Course`エンティティ、新しいコードがそのコースを明示的に読み込みます`Enrollments`ナビゲーション プロパティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

明示的に読み込む各し、`Enrollment`エンティティの関連`Student`エンティティ。

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

使用することを確認、`Collection`コレクション プロパティを読み込みますが、使用する、1 つのエンティティを保持するプロパティの`Reference`メソッドです。 インストラクター インデックス ページを今すぐ実行することができ、わかりますなし ページで、表示される内容に違いが、データの取得方法を変更しました。

## <a name="summary"></a>まとめ

ナビゲーション プロパティに関連するデータを読み込むすべての 3 つ方法 (レイジー、eager、および明示的な) を使用したようになりました。 次のチュートリアルでは、関連するデータを更新する方法を学習します。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。

>[!div class="step-by-step"]
[前へ](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[次へ](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
