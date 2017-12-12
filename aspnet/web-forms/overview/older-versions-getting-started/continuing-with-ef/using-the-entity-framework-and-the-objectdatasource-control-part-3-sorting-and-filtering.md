---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: "Entity Framework 4.0 ObjectDataSource コントロールを使用して、パート 3: 並べ替えとフィルター処理 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、Entity Framework 4.0 チュートリアル シリーズの概要を作成した Contoso 大学 web アプリケーションに基づいています。 私。。。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 4accd3381a66bde1f87f0dc7dd95beeb54fcc6a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Entity Framework 4.0 ObjectDataSource コントロールを使用して、パート 3: 並べ替えとフィルター処理
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズのビルドによって作成される Contoso 大学 web アプリケーションで、 [Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)一連のチュートリアルです。 前のチュートリアルを完了していない場合このチュートリアルの開始点とすることができます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)作成したとします。 こともできます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)一連の完全なチュートリアルで作成します。 チュートリアルについて質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)です。


前のチュートリアルでは、Entity Framework を使用する n 層の web アプリケーションでリポジトリ パターンを実装し、`ObjectDataSource`コントロール。 このチュートリアルでは、並べ替えとフィルター処理を行うし、マスター/詳細シナリオを処理する方法を示します。 次の拡張機能を追加、 *Departments.aspx*ページ。

- 部門名でを選択できるようにするテキスト ボックスです。
- グリッドに表示される各部門のコースの一覧です。
- 列見出しをクリックして並べ替えることです。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>GridView 列の並べ替えに機能を追加します。

開く、 *Departments.aspx*ページし、追加、`SortParameterName="sortExpression"`属性を`ObjectDataSource`という名前のコントロール`DepartmentsObjectDataSource`です。 (を作成した後で、`GetDepartments`をという名前のパラメーターを受け取るメソッド`sortExpression`)。コントロールの開始タグのマークアップには、次の例がようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

追加、`AllowSorting="true"`属性の開始タグを`GridView`コントロール。 コントロールの開始タグのマークアップには、次の例がようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

*Departments.aspx.cs*、呼び出すことによって既定の並べ替え順序を設定、`GridView`コントロールの`Sort`メソッドから、`Page_Load`メソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

ビジネス ロジックのクラスまたはリポジトリ クラスのいずれかで、並べ替えまたはフィルター処理するコードを追加できます。 場合は、ビジネス ロジックのクラスで、並べ替えまたは作業をフィルター処理を行いますデータがデータベースから取得された後、ビジネス ロジックのクラスを使用するため、`IEnumerable`リポジトリから返されるオブジェクト。 並べ替えとフィルター処理、リポジトリ クラス内のコードを追加して LINQ の式より前に行うまたはオブジェクト クエリに変換された場合、`IEnumerable`オブジェクトのコマンドをこれは通常より効率的な処理のため、データベースに渡されます。 このチュートリアルでは並べ替えと、データベースで行われる処理の原因となる方法でフィルター処理を実装します-つまり、リポジトリにします。

並べ替え機能を追加するには、リポジトリ インターフェイスおよびリポジトリ クラスもビジネス ロジックのクラスに新しいメソッドを追加する必要があります。 *ISchoolRepository.cs*ファイルに追加し、新しい`GetDepartments`を受け取るメソッド、`sortExpression`返される部門の一覧の並べ替えに使用されるパラメーター。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression`パラメーターには、並べ替えに使用する列と並べ替えの方向を指定します。

新しいメソッドのコードを追加、 *SchoolRepository.cs*ファイル。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

既存のパラメーターなしの変更`GetDepartments`に新しいメソッドを呼び出すメソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

テスト プロジェクト内に次の新しいメソッドを追加*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

並べ替えられた一覧を返すこのメソッドに依存する他の単体テストを作成する場合を返す前にリストを並べ替える必要があります。 しないに作成するテストのようこのチュートリアルでは、メソッドは、部門の並べ替えられていないリストを返すだけできるようです。

*SchoolBL.cs*ファイルにビジネス ロジックのクラスに次の新しいメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

このコードは、リポジトリのメソッドに並べ替えのパラメーターを渡します。

実行、 *Departments.aspx*ページ。

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

今すぐその列で並べ替えを行う任意の列見出しをクリックできます。 列が既に並べ替えられて場合、並べ替えの方向を反転の見出しをクリックします。

## <a name="adding-a-search-box"></a>検索ボックスを追加します。

このセクションではあります検索テキスト ボックスを追加、リンク、`ObjectDataSource`コントロール パラメーターの使用を制御し、フィルター処理をサポートするためにビジネス ロジックのクラスにメソッドを追加します。

開く、 *Departments.aspx*  ページ、見出しと、最初の間で、次のマークアップを追加して`ObjectDataSource`コントロール。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

`ObjectDataSource`という名前のコントロール`DepartmentsObjectDataSource`次の操作を行います。

- 追加、`SelectParameters`という名前のパラメーターの要素`nameSearchString`に入力された値を取得する、`SearchTextBox`コントロール。
- 変更、`SelectMethod`属性に値`GetDepartmentsByName`です。 (ありますメソッドを作成するこの後でします。)

マークアップ、`ObjectDataSource`コントロール次の例のようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

*ISchoolRepository.cs*、追加、`GetDepartmentsByName`を両方を受け取るメソッド`sortExpression`と`nameSearchString`パラメーター。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

*SchoolRepository.cs*、次の新しいメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

このコードを使用して、`Where`メソッドを検索文字列が含まれる項目を選択します。 検索文字列が空の場合は、すべてのレコードが選択されます。 メソッドは次のように 1 つのステートメントで一緒に呼び出すかを指定するときに注意してください (`Include`、し`OrderBy`、し`Where`) では、`Where`メソッドを常に最後にある必要があります。

既存の変更`GetDepartments`を受け取るメソッド、`sortExpression`新しいメソッドを呼び出すパラメーター。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

*MockSchoolRepository.cs*テスト プロジェクトで、次の新しいメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

*SchoolBL.cs*、次の新しいメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

実行、 *Departments.aspx*ページし、選択するロジックが動作するかどうかを確認する検索文字列を入力します。 テキスト ボックスを空のままにして、すべてのレコードが返されるかどうかを確認するために検索を再試行してください。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>グリッドの行ごとに詳細列を追加します。

次に、すべてのグリッドの右側のセルに表示される各部門のコースを表示します。 これを行うには、使用する、入れ子になった`GridView`コントロールとデータ バインドからのデータを`Courses`のナビゲーション プロパティ、`Department`エンティティです。

開いている*Departments.aspx*およびのマークアップで、`GridView`を制御するハンドラーを指定の`RowDataBound`イベント。 コントロールの開始タグのマークアップには、次の例がようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

新しい`TemplateField`要素の後に、`Administrator`テンプレート フィールド。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

このマークアップを作成、入れ子になった`GridView`コース番号とタイトルのコースの一覧を表示するコントロール。 データ バインドがわかるために、データ ソースは指定しません内のコードで、`RowDataBound`ハンドラー。

開いている*Departments.aspx.cs*の次のハンドラーを追加し、`RowDataBound`イベント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

このコードを取得、`Department`イベントの引数からのエンティティに変換、`Courses`へのナビゲーション プロパティ、`List`コレクション、および入れ子になったして`GridView`をコレクションにします。

開く、 *SchoolRepository.cs*ファイルし、の一括読み込みの指定、`Courses`ナビゲーション プロパティを呼び出して、`Include`メソッドで作成したオブジェクトのクエリで、`GetDepartmentsByName`メソッドです。 `return`内のステートメント、`GetDepartmentsByName`メソッド次の例のようになります。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

ページを実行します。 だけでなく、並べ替えとフィルター処理前に追加した機能、GridView コントロールは今すぐ部門ごとに入れ子になったコースの詳細を示します。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

これは、並べ替え、フィルター、およびマスター/詳細シナリオの概要を完了します。 次のチュートリアルでは、同時実行の処理方法が表示されます。

>[!div class="step-by-step"]
[前へ](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[次へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
