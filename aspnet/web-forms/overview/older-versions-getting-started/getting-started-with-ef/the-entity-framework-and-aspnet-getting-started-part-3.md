---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 3 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 654f3556af5d05ec186e1811421966bbaffd2e21
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889255"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 3
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください[系列内の最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>フィルター処理、並べ替え、およびデータをグループ化

前のチュートリアルでは使用して、`EntityDataSource`コントロールを表示し、データを編集します。 このチュートリアルではあり、フィルター処理する、順序、データをグループ化します。 プロパティを設定してのこれを行うと、`EntityDataSource`コントロール、構文は他のデータ ソース コントロールから異なります。 説明するように、ただし、使用できます、`QueryExtender`をこれらの相違点を最小限にするコントロール。

変更、 *Students.aspx*ページ、受講者をフィルター処理を名前で検索して、名前で並べ替え。 変更することもあります、 *Courses.aspx*名前でコースを検索して選択した部門のコースを表示するページ。 最後に、学生の統計情報を追加、 *About.aspx*ページ。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>プロパティを使用して、EntityDataSource"Where"データをフィルター処理

開く、 *Students.aspx*前のチュートリアルで作成したページ。 現在の構成に従って、`GridView`ページ内のコントロールからのすべての名前を表示する、`People`エンティティ セット。 のみ受講者を表示するただし、見つけることができます を選択して`Person`null 以外の登録の日付が含まれているエンティティ。

切り替える**デザイン**表示し、選択、`EntityDataSource`コントロール。 **プロパティ**ウィンドウで、設定、`Where`プロパティを`it.EnrollmentDate is not null`です。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

使用する構文、`Where`のプロパティ、`EntityDataSource`コントロールは、Entity SQL です。 Entity SQL は、TRANSACT-SQL に似ていますが、データベース オブジェクトではなく、エンティティで使用するためにカスタマイズがします。 式で`it.EnrollmentDate is not null`、単語`it`クエリによって返されるエンティティへの参照を表します。 したがって、`it.EnrollmentDate`を指す、`EnrollmentDate`のプロパティ、`Person`エンティティを`EntityDataSource`制御を返します。

ページを実行します。 これで、学生のリストには、受講者のみが含まれています。 (ここで表示される行がない登録日はありません)。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>注文データを EntityDataSource"OrderBy"プロパティを使用します。

この一覧が最初に表示されるときに、名前の順序である必要があります。 *Students.aspx*  ページで開かれて**デザイン**ビューを使用して、`EntityDataSource`コントロールがまだオンにした場合、**プロパティ**ウィンドウのセット、 **OrderBy**プロパティを`it.LastName`です。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

ページを実行します。 学生のリストは姓で並べ替えが開始されました。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>"Where"プロパティを設定する制御パラメーターを使用します。

その他のデータ ソース コントロールとするパラメーターの値を渡すことができます、`Where`プロパティです。 *Courses.aspx*ページ、チュートリアルのパート 2 で作成した、このメソッドを使用するには、ユーザーがドロップダウン リストから選択する部門に関連付けられているコースを表示することです。

開いている*Courses.aspx*に切り替えると**デザイン**ビュー。 1 秒あたりの追加`EntityDataSource`コントロールをページ、および名前を付けます`CoursesEntityDataSource`です。 接続して、`SchoolEntities`モデル、および選択`Courses`として、 **EntitySetName**値。

**プロパティ** ウィンドウにある省略記号をクリックして、**場所**プロパティ ボックス。 (確認、`CoursesEntityDataSource`コントロールが使用する前に選択されて、**プロパティ**ウィンドウです)。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**式エディター**  ダイアログ ボックスが表示されます。 このダイアログ ボックスで選択**Where を自動的に生成式が指定されたパラメーターに基づく**、クリックして**パラメーターの追加**です。 パラメーターの名前を付けます`DepartmentID`**コントロール**として、**パラメーター ソース**値に設定して、選択**DepartmentsDropDownList**として、 **ControlID**値。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

をクリックして**詳細プロパティの表示**、し、[、**プロパティ**のウィンドウ、**式エディター** ] ダイアログ ボックスで、変更、`Type`プロパティを`Int32`です。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

完了したら、をクリックして**OK**です。

ドロップダウン リストの下に追加、`GridView`コントロールをページし、名前を付けます`CoursesGridView`です。 接続に、`CoursesEntityDataSource`データ ソース コントロールで、をクリックして**スキーマの更新**、 をクリックして**列の編集**、および削除、`DepartmentID`列。 `GridView`コントロールのマークアップが次の例に似ています。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

ユーザーには、ドロップダウン リストで選択した部門が変更された、ときに関連付けられているコースに自動的に変更の一覧が必要です。 これには、ドロップダウン リストを選択して、**プロパティ**ウィンドウのセット、`AutoPostBack`プロパティを`True`です。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

デザイナーの使用が完了したら、これに切り替える**ソース**表示し、置換、`ConnectionString`と`DefaultContainer`のプロパティの名前に、`CoursesEntityDataSource`コントロールを`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`属性。 完了したら、コントロールのマークアップは次の例のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

ページを実行し、ドロップダウン リストを使用して、さまざまな部門を選択します。 選択した部門によって提供されるコースのみが表示されます、`GridView`コントロール。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>プロパティを使用して、EntityDataSource"GroupBy"データをグループ化

Contoso 大学が、に関するページにいくつかの学生本文統計に配置しようとします。 具体的には、登録されていると、日付によって生徒数の内訳を表示しようとします。

開いている*About.aspx*、し、**ソース**ビューで、既存の内容を置き換える、`BodyContent`間を「学生本文統計」を含む制御`h2`タグ。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

見出しの後の追加、`EntityDataSource`を制御し、名前を付けます`StudentStatisticsEntityDataSource`です。 接続に`SchoolEntities`を選択、`People`エンティティ セットにしのままにして、**選択**ボックス、ウィザードをそのままにします。 次のプロパティを設定、**プロパティ**ウィンドウ。

- 受講者のみをフィルターするには、設定、`Where`プロパティを`it.EnrollmentDate is not null`です。
- 登録日で結果をグループ化、設定、`GroupBy`プロパティを`it.EnrollmentDate`です。
- 登録日と生徒の数を選択するには設定、`Select`プロパティを`it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`です。
- 登録日で結果の順序、設定、`OrderBy`プロパティを`it.EnrollmentDate`です。

**ソース**ビューで、置換、`ConnectionString`と`DefaultContainer`とプロパティの名前に、`ContextTypeName`プロパティです。 `EntityDataSource`コントロール マークアップ次の例のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

構文、 `Select`、 `GroupBy`、および`Where`プロパティ以外には、TRANSACT-SQL のようになります、`it`現在のエンティティを指定するキーワードです。

作成する次のマークアップを追加、`GridView`コントロールにデータを表示します。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

登録日ごと生徒の数を示す一覧を表示するページを実行します。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>フィルター処理と順序付けの QueryExtender コントロールの使用

`QueryExtender`コントロールには、マークアップのフィルターおよび並べ替えを指定する方法が用意されています。 構文が使用しているデータベース管理システム (DBMS) に依存しません。 ナビゲーション プロパティを使用する構文が Entity Framework に一意である例外に、Entity Framework の一般的に独立してもできます。

このチュートリアルのこの部分で使用する、`QueryExtender`データのフィルターと順序を制御し、order by フィールドのいずれかにナビゲーション プロパティなります。

(マークアップではなくコードを使用して、によって自動的に生成されるクエリを拡張する場合、`EntityDataSource`制御を行うことができますを処理、`QueryCreated`イベント。 これは、どのように`QueryExtender`コントロールを拡張`EntityDataSource`もクエリを制御します)。

開く、 *Courses.aspx*ページし、前に追加したマークアップの下には、見出し、[検索] ボタンの検索文字列を入力するテキスト ボックスを作成する次のマークアップを挿入し、 `EntityDataSource` にバインドされているコントロール`Courses`エンティティ セット。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

注意して、`EntityDataSource`コントロールの`Include`プロパティに設定されている`Department`です。 データベースで、`Course`テーブルが部門名を含まない; が含まれている、`DepartmentID`外部キー列です。 結合する必要がありますが、データベースを直接、コース データと共に部門名を取得するクエリを実行する場合、`Course`と`Department`テーブル。 設定して、`Include`プロパティを`Department`、Entity Framework が、関連する作業の作業を行う必要がありますを指定する`Department`エンティティを取得するとき、`Course`エンティティです。 `Department`エンティティに保存し、`Department`のナビゲーション プロパティ、`Course`エンティティです。 (既定では、`SchoolEntities`データ モデル デザイナーによって生成されたクラスが関連するデータを取得すると、必要なため、そのクラスに、データ ソース コントロールをバインドした、`Include`プロパティが必要ではありません。 ただし、設定することのパフォーマンスが向上します ページで、それ以外の場合、Entity Framework 実行するようにデータを取得するデータベースへの呼び出しを区切るため、`Course`エンティティと関連する`Department`エンティティです)。

後に、`EntityDataSource`したコントロールを作成する次のマークアップを挿入、作成、`QueryExtender`にバインドされるコントロールを`EntityDataSource`コントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression`要素は、タイトルが一致するテキスト ボックスに入力された値のコースを選択することを指定します。 テキスト ボックスに入力した文字の数と比較されます、ためにのみ、`SearchType`プロパティを指定`StartsWith`です。

`OrderByExpression`要素は、部門名内のコース タイトルは、結果セットを並べ替えられますことを指定します。 部門名を指定する方法に注意してください:`Department.Name`です。 の間の関連付け、`Course`エンティティと`Department`エンティティは、一対一、`Department`ナビゲーション プロパティが含まれています、`Department`エンティティです。 (これが一対多リレーションシップの場合、プロパティはコレクションを格納します。)部門名を取得する必要がありますを指定する、`Name`のプロパティ、`Department`エンティティです。

最後に、追加、`GridView`コースの一覧を表示するコントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

最初の列は、部門名が表示されるテンプレート フィールドです。 データ バインド式を指定`Department.Name`で学習したのと同様、`QueryExtender`コントロール。

ページを実行します。 最初の表示は、部門られ、続いてコースのタイトルの順序ですべてのコースの一覧を示します。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

"M"を入力し、クリックして**検索**にタイトルが"m"(検索はいない大文字と小文字) で始まるすべてのコースを参照してください。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>"Like"演算子を使用してデータをフィルター処理

同様の効果を得ることができます、`QueryExtender`コントロールの`StartsWith`、 `Contains`、および`EndsWith`を使用して型を検索、`Like`の演算子、`EntityDataSource`コントロールの`Where`プロパティです。 このチュートリアルでは、使用する方法が表示されます、`Like`受講者の名前を検索する演算子です。

開いている*Students.aspx*で**ソース**ビュー。 後に、`GridView`コントロールを次のマークアップを追加します。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

このマークアップが経験した前除くに似ていますが、`Where`プロパティの値。 2 番目の部分、`Where`部分文字列検索を定義する式 (`LIKE %FirstMidName% or LIKE %LastName%`)、テキスト ボックスに入力した内容の姓と名の両方を検索します。

ページを実行します。 最初に表示すべて受講者のための既定値、`StudentName`パラメーターは、「%」です。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

テキスト ボックスに"g"の文字を入力し、クリックして**検索**です。 "G"いずれかで、名前を持つ最初または最生徒数の一覧が表示されます。

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

したようになりました表示、更新、フィルター処理、並べ替え、および個々 のテーブルからデータをグループ化します。 次のチュートリアルでは、関連するデータ (マスター/詳細シナリオ) を扱う始めます。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-4.md)
