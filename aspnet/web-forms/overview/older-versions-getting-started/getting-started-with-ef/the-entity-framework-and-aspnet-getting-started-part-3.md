---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォームの第 3 部 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 162f93c0249d8fa11d67ea10c68bc2ca8bae7259
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826332"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 3
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>フィルター処理、並べ替え、およびデータをグループ化

前のチュートリアルでは使用して、`EntityDataSource`コントロール データを表示および編集します。 このチュートリアルではありますフィルター、および順は、データをグループ化します。 プロパティを設定してのこれを行うと、`EntityDataSource`コントロール、構文は他のデータ ソース コントロールによって異なります。 わかる、ただし、使用できます、`QueryExtender`これらの相違点を最小限に抑えるコントロール。

変更します、 *Students.aspx*ページ、受講者をフィルター処理を並べ替え、名、および名前で検索します。 変更することもあります、 *Courses.aspx*名前で、選択した部門とコースを検索するためのコースを表示するページ。 最後に、学生の統計情報を追加します、 *About.aspx*ページ。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>プロパティを使用して、EntityDataSource"Where"データをフィルター処理

開く、 *Students.aspx*前のチュートリアルで作成したページです。 構成は、現在、`GridView`ページ内のコントロールからのすべての名前を表示する、`People`エンティティ セット。 、受講者のみを表示するただし、これを選択して`Person`null 以外の登録日を取得するエンティティ。

切り替える**デザイン**表示し、選択、`EntityDataSource`コントロール。 **プロパティ**ウィンドウで、設定、`Where`プロパティを`it.EnrollmentDate is not null`します。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

使用する構文、`Where`のプロパティ、`EntityDataSource`コントロールは、Entity SQL です。 Entity SQL は、TRANSACT-SQL に似ていますが、データベース オブジェクトではなく、エンティティで使用するためにカスタマイズが。 式で`it.EnrollmentDate is not null`、単語`it`クエリによって返されるエンティティへの参照を表します。 そのため、`it.EnrollmentDate`を指す、`EnrollmentDate`のプロパティ、`Person`エンティティを`EntityDataSource`制御を返します。

ページを実行します。 学生のリストには、今すぐ受講者のみが含まれています。 (ここに表示される行がない加入契約日はありません)。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>注文データを EntityDataSource"OrderBy"プロパティを使用します。

この一覧は、最初に表示されるときに、名前の順序であるを場合もあります。 *Students.aspx*で開いているページ**デザイン**ビューを使用して、`EntityDataSource`コントロールが選択したままで、**プロパティ**ウィンドウのセット、 **OrderBy**プロパティを`it.LastName`します。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

ページを実行します。 受講者の一覧にして、姓の順になります。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>コントロールのパラメーターを使用して、"Where"プロパティを設定するには

他のデータ ソース コントロールでのパラメーター値を渡すことができます、`Where`プロパティ。 *Courses.aspx*ページ、チュートリアルのパート 2 で作成したことは、このメソッドを使用して、ユーザーがドロップダウン リストから選択する部門に関連付けられているコースを表示することができます。

開いている*Courses.aspx*に切り替えると**デザイン**ビュー。 1 秒あたりの追加`EntityDataSource`コントロールをページ、および名前を付けます`CoursesEntityDataSource`します。 接続先の`SchoolEntities`モデル、および選択`Courses`として、 **EntitySetName**値。

**プロパティ** ウィンドウで省略記号をクリックして、**場所**プロパティ ボックス。 (確認、`CoursesEntityDataSource`コントロールが使用する前に選択したまま、**プロパティ**ウィンドウ)。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**式エディター**  ダイアログ ボックスが表示されます。 このダイアログ ボックスで選択**Where を自動的に生成式が指定されたパラメーターに基づいて**、順にクリックします**パラメーターの追加**します。 パラメーターの名前`DepartmentID`、**コントロール**として、**パラメーター ソース**値、および選択**DepartmentsDropDownList**として、 **ControlID**値。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

クリックして**詳細プロパティの表示**、し、**プロパティ**のウィンドウ、**式エディター**  ダイアログ ボックスで、変更、`Type`プロパティを`Int32`します。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

完了したら、クリックして**OK**します。

ドロップダウン リストで、以下を追加、`GridView`コントロールをページと名前を付けます`CoursesGridView`します。 接続先の`CoursesEntityDataSource`データ ソース コントロールで、をクリックして**スキーマの更新**、 をクリックして**列の編集**、および削除、`DepartmentID`列。 `GridView`コントロールのマークアップが次の例に似ています。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

ドロップダウン リストで選択した部門を変更するときに自動的に変更の関連付けられているコースの一覧が必要。 これには、ドロップダウン リストを選択して、**プロパティ**ウィンドウのセット、`AutoPostBack`プロパティを`True`します。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

デザイナーの使用が完了したら、これで切り替える**ソース**を表示して、置換、`ConnectionString`と`DefaultContainer`のプロパティの名前、`CoursesEntityDataSource`コントロールを`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`属性。 完了すると、コントロールのマークアップは次の例のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

ページを実行し、ドロップダウン リストを使用して、別の部署を選択します。 選択した部門によって提供されているコースのみが表示されます、`GridView`コントロール。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>データをグループ化 EntityDataSource"GroupBy"プロパティを使用します。

Contoso University がそのについてのページにいくつかの学生本文の統計情報を配置する必要があるとします。 具体的には、登録日付で生徒数の内訳を表示しようとします。

開いている*About.aspx*、し、**ソース**ビューで、既存の内容を置き換える、 `BodyContent` 「学生本文の統計情報」の間でコントロール`h2`タグ。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

見出しの後の追加、`EntityDataSource`を制御し、名前を`StudentStatisticsEntityDataSource`します。 接続先の`SchoolEntities`を選択します、`People`エンティティ セット、およびのままに、**選択**ボックス、ウィザードをそのままにします。 次のプロパティを設定、**プロパティ**ウィンドウ。

- 受講者のみをフィルターするには、設定、`Where`プロパティを`it.EnrollmentDate is not null`します。
- 結果をグループ化、登録日で、設定、`GroupBy`プロパティを`it.EnrollmentDate`します。
- 加入契約日と生徒の数を選択するには、設定、`Select`プロパティを`it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`します。
- 登録日で、結果の順序を設定、`OrderBy`プロパティを`it.EnrollmentDate`します。

**ソース**ビューで、置換、`ConnectionString`と`DefaultContainer`でプロパティの名前、`ContextTypeName`プロパティ。 `EntityDataSource`コントロールのマークアップ例では、次のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

構文、 `Select`、 `GroupBy`、および`Where`プロパティが以外には、TRANSACT-SQL に似ています、`it`現在のエンティティを指定するキーワード。

作成する次のマークアップを追加、`GridView`データを表示するコントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

登録日で受講者の数を示す一覧を表示するページを実行します。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>フィルター処理と並べ替えのによる QueryExtender コントロールを使用します。

`QueryExtender`コントロール マークアップのフィルターおよび並べ替えを指定する方法を提供します。 構文は、使用しているデータベース管理システム (DBMS) に依存しません。 一般に、ナビゲーション プロパティを使用する構文が、Entity Framework には一意である例外、Entity Framework に依存しないこともです。

このチュートリアルのこの部分で使用する、`QueryExtender`フィルターおよび並べ替えのデータへの制御と order by フィールドの 1 つには、ナビゲーション プロパティになります。

(マークアップではなくコードを使用して、によって自動的に生成されるクエリを拡張する場合、`EntityDataSource`制御を行うことができます処理、`QueryCreated`イベント。 これは、どのように`QueryExtender`コントロールを拡張`EntityDataSource`もクエリを制御します)。

開く、 *Courses.aspx*ページ、および前に追加したマークアップの下には、見出し、検索文字列を検索ボタンを入力するためのテキスト ボックスを作成する次のマークアップを挿入し、 `EntityDataSource` にバインドされたコントロール`Courses`エンティティ セット。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

なお、`EntityDataSource`コントロールの`Include`プロパティに設定されて`Department`します。 データベースで、`Course`テーブルに部署名が含まれていません。 含まれている、`DepartmentID`外部キー列。 参加する必要がありますが、データベースを直接、コースのデータと共に部門名を取得するクエリを実行する場合、`Course`と`Department`テーブル。 設定して、`Include`プロパティを`Department`、Entity Framework が、関連の作業を行う必要がありますを指定する`Department`エンティティを取得するとき、`Course`エンティティ。 `Department`エンティティが格納し、`Department`のナビゲーション プロパティ、`Course`エンティティ。 (既定で、`SchoolEntities`ことが必要であり、データ ソース コントロールを設定するため、そのクラスにバインドした場合、データのモデル デザイナーによって生成されたクラスに関連するデータを取得します、`Include`プロパティが必要ではありません。 ただし、設定するとパフォーマンスが向上、ページの Entity Framework は、データを取得するデータベースへの呼び出しを分離しますそれ以外の場合、`Course`エンティティと関連`Department`エンティティです。)。

後に、`EntityDataSource`したコントロールを作成する次のマークアップを挿入、作成、`QueryExtender`コントロールにバインドされている`EntityDataSource`コントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression`要素は、コースのタイトルが一致するテキスト ボックスに入力された値を選択することを指定します。 ため、多くの文字は、テキスト ボックスに入力は比較する場合のみ、`SearchType`プロパティを指定します`StartsWith`します。

`OrderByExpression`要素は、結果セットは、部門名に含まれるコース タイトルで並べ替えられますことを指定します。 部門名を指定する方法に注意してください:`Department.Name`します。 の間の関連付け、`Course`エンティティと`Department`エンティティは、一対一、`Department`ナビゲーション プロパティが含まれています、`Department`エンティティ。 (これが、一対多リレーションシップで場合は、プロパティはコレクション格納が)。部門名を取得することを指定する必要があります、`Name`のプロパティ、`Department`エンティティ。

最後に、追加、`GridView`コースのリストを表示するコントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

最初の列は、部門名を表示するテンプレートのフィールドです。 データ バインディング式を指定します`Department.Name`で見たのと同様、`QueryExtender`コントロール。

ページを実行します。 最初の表示では、部門とコースのタイトルの順序ですべてのコースの一覧が表示されます。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

"M"を入力し、クリックして**検索**にタイトルが"m"(検索はいない大文字と小文字) で始まるすべてのコースをご覧ください。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>"Like"演算子を使用してデータをフィルター処理

同様の効果を実現することができます、`QueryExtender`コントロールの`StartsWith`、 `Contains`、および`EndsWith`を使用して型を検索、`Like`演算子、`EntityDataSource`コントロールの`Where`プロパティ。 このチュートリアルでは、使用する方法が紹介、`Like`受講者名で検索する演算子。

開いている*Students.aspx*で**ソース**ビュー。 後に、`GridView`コントロールを次のマークアップを追加します。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

このマークアップは経験した前述以外に似ています、`Where`プロパティの値。 2 番目の部分、`Where`式の部分文字列検索を定義します (`LIKE %FirstMidName% or LIKE %LastName%`) テキスト ボックスに入力した内容の最初と最後の名前を検索します。

ページを実行します。 最初にすべての表示、受講者の既定値であるため、`StudentName`パラメーターは、「%」です。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

テキスト ボックスに文字"g"を入力し、クリックして**検索**します。 姓または名のいずれかの"g"がある学生の一覧を表示します。

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

ようになりました表示、更新、フィルター処理、ordered、して個々 のテーブルからデータをグループ化します。 次のチュートリアルでは、関連データ (マスター/詳細シナリオ) を操作する始めます。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-4.md)
