---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Entity Framework 4.0 と ObjectDataSource コントロールを使用して、パート 3: 並べ替えとフィルター処理 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズでは、Entity Framework 4.0 のチュートリアル シリーズの概要を作成した Contoso University web アプリケーションに基づいています。 ここには.
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 71e26dc9a63131842daf4aa1549d761690b723ea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826465"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Entity Framework 4.0 と ObjectDataSource コントロールを使用して、パート 3: 並べ替えとフィルター処理
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズは、Contoso University web アプリケーションによって作成される、 [、Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)チュートリアル シリーズです。 前のチュートリアルを完了していない場合は、このチュートリアルの開始点としてできます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)に、作成します。 できます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完全なチュートリアル シリーズで作成します。 チュートリアルについて質問等がございましたらを投稿できます、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)します。


前のチュートリアルでは、Entity Framework を使用する n 層 web アプリケーションでリポジトリ パターンを実装し、`ObjectDataSource`コントロール。 このチュートリアルでは、並べ替えとフィルター処理を実行してマスター/詳細のシナリオを処理する方法を示します。 次の機能強化を追加します、 *Departments.aspx*ページ。

- 部門名を選択するためのテキスト ボックスです。
- グリッドに表示されている各部門のコースのリスト。
- 列見出しをクリックしてソートする権限です。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>GridView の列の並べ替えに機能を追加します。

開く、 *Departments.aspx*ページし、追加、`SortParameterName="sortExpression"`属性を`ObjectDataSource`という名前のコントロール`DepartmentsObjectDataSource`します。 (後でに作成、`GetDepartments`という名前のパラメーターを受け取るメソッド`sortExpression`)。コントロールの開始タグのマークアップには、次の例では、今すぐに似ています。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

追加、`AllowSorting="true"`属性の開始タグを`GridView`コントロール。 コントロールの開始タグのマークアップには、次の例では、今すぐに似ています。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

*Departments.aspx.cs*、呼び出すことによって、既定の並べ替え順序を設定、`GridView`コントロールの`Sort`からメソッド、`Page_Load`メソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

ビジネス ロジック クラスまたはリポジトリ クラスのいずれかでは、並べ替え、またはフィルター処理するコードを追加できます。 場合に、ビジネス ロジック クラスでは、並べ替えまたはフィルター作業に、データベースからデータが取得された後、ビジネス ロジック クラスを使用するため、`IEnumerable`リポジトリによって返されるオブジェクト。 並べ替えとフィルタ リングのリポジトリ クラスにコードを追加して、LINQ 式の前に行うまたはオブジェクト クエリに変換された場合、`IEnumerable`オブジェクトをコマンドは、これは通常より効率的な処理には、データベースに渡されるされます。 このチュートリアルでは、並べ替えと、データベースで行われる処理を原因となる方法でフィルター処理を実装します-つまり、リポジトリにします。

並べ替え機能を追加するには、リポジトリ インターフェイスおよびリポジトリ クラスにも、ビジネス ロジック クラスに新しいメソッドを追加する必要があります。 *ISchoolRepository.cs*ファイルに追加し、新しい`GetDepartments`を受け取るメソッドを`sortExpression`部門の返される一覧の並べ替えに使用されるパラメーター。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression`パラメーターには、並べ替えに使用する列と並べ替えの方向を指定します。

新しいメソッドのコードを追加、 *SchoolRepository.cs*ファイル。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

既存のパラメーターのない変更`GetDepartments`に新しいメソッドを呼び出すメソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

次の新しいメソッドを追加、テスト プロジェクトで*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

並べ替えられた一覧を返すこのメソッドに依存する任意の単体テストを作成する場合は、一覧を返す前に並べ替える必要があります。 しません作成することにテストそのようなこのチュートリアルでは並べ替えられていない部門の一覧を返すことが、メソッド、します。

*SchoolBL.cs*ファイルに、ビジネス ロジック クラスに次の新しいメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

このコードは、リポジトリ メソッドに並べ替えパラメーターを渡します。

実行、 *Departments.aspx*ページ。

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

その列を並べ替えるには任意の列見出しをクリックすることができますようになりました。 列が既に並べ替えられている場合、見出しをクリックすると、並べ替えの方向を反転させます。

## <a name="adding-a-search-box"></a>検索ボックスを追加します。

このセクションでは、検索テキスト ボックスを追加、リンク、`ObjectDataSource`コントロール パラメーターを使用して制御し、フィルター処理をサポートするためにビジネス ロジック クラスにメソッドを追加します。

開く、 *Departments.aspx*ページ、見出しと 1 つ目の間は、次のマークアップを追加および`ObjectDataSource`コントロール。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

`ObjectDataSource`という名前のコントロール`DepartmentsObjectDataSource`次の操作を行います。

- 追加、`SelectParameters`という名前のパラメーター要素`nameSearchString`に入力された値を取得する、`SearchTextBox`コントロール。
- 変更、`SelectMethod`属性に値`GetDepartmentsByName`します。 (作成するメソッドのこの後でします。)

マークアップを`ObjectDataSource`コントロール次の例のようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

*ISchoolRepository.cs*、追加、`GetDepartmentsByName`両方を受け取るメソッド`sortExpression`と`nameSearchString`パラメーター。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

*SchoolRepository.cs*、次の新しいメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

このコードを使用して、`Where`検索文字列が含まれている項目を選択するメソッド。 検索文字列が空の場合は、すべてのレコードが選択されます。 このような 1 つのステートメントでまとめてメソッドの呼び出しを指定する場合は注意 (`Include`、し`OrderBy`、し`Where`)、`Where`メソッドを常に最後にある必要があります。

既存の変更`GetDepartments`を受け取るメソッドを`sortExpression`新しいメソッドを呼び出すパラメーター。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

*MockSchoolRepository.cs*テスト プロジェクトで、次の新しいメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

*SchoolBL.cs*、次の新しいメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

実行、 *Departments.aspx*ページし、選択ロジックが機能するかどうかを確認する検索文字列を入力します。 テキスト ボックスを空のままにして、すべてのレコードが返されることを確認するために検索を再試行してください。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>グリッドの行ごとの詳細の列を追加します。

次に、すべてのグリッドの右側のセルに表示される各部門のコースを表示します。 これを行うには、使用する、入れ子になった`GridView`コントロールとデータ バインドからのデータを`Courses`のナビゲーション プロパティ、`Department`エンティティ。

開いている*Departments.aspx*のマークアップで、`GridView`を制御するハンドラーを指定の`RowDataBound`イベント。 コントロールの開始タグのマークアップには、次の例では、今すぐに似ています。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

新しい追加`TemplateField`要素の後に、`Administrator`テンプレート フィールド。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

このマークアップを作成、入れ子になった`GridView`コース番号とコースのリストのタイトルを表示するコントロールです。 データ バインドがわかるために、データ ソースは指定しません内のコードで、`RowDataBound`ハンドラー。

開いている*Departments.aspx.cs*の次のハンドラーを追加し、`RowDataBound`イベント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

このコードを取得、`Department`イベントの引数からのエンティティに変換します、`Courses`ナビゲーション プロパティを`List`コレクション、および入れ子になったして`GridView`コレクションにします。

開く、 *SchoolRepository.cs*ファイルし、一括読み込みを指定、`Courses`ナビゲーション プロパティを呼び出して、`Include`メソッドで作成したオブジェクトのクエリで、`GetDepartmentsByName`メソッド。 `return`内のステートメント、`GetDepartmentsByName`メソッド例では、次のようになります。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

ページを実行します。 GridView コントロールには、並べ替えとフィルター処理する前に追加する機能、だけでなく、各部門の入れ子になったコースの詳細に表示されます。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

これにより、並べ替え、フィルター処理、およびマスター詳細シナリオの概要が完了します。 次のチュートリアルでは、同時実行を処理する方法を確認します。

> [!div class="step-by-step"]
> [前へ](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
