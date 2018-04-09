---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: SqlDataSource (VB) によるパラメーター化クエリを使用して |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルは、SqlDataSource コントロールで、外観を続行し、パラメーター化クエリを定義する方法について説明します。 パラメーターを指定する両方 decla しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: a7442ef3bebb2742cc36d695914b745aa2dfa721
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>SqlDataSource (VB) でパラメーター化クエリの使用
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe)または[PDF のダウンロード](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> このチュートリアルは、SqlDataSource コントロールで、外観を続行し、パラメーター化クエリを定義する方法について説明します。 パラメーターは、プログラムでは、両方の宣言でと指定することができます、数など、クエリ文字列、セッション状態、その他のコントロール、および複数の場所からプルできます。


## <a name="introduction"></a>はじめに

前のチュートリアルでは、SqlDataSource コントロールを使用して、データベースから直接データを取得する方法を説明しました。 データ ソースの構成ウィザードを使用して、おでした選択データベースし、いずれかの: テーブルまたはビューから返す列を選択カスタムの SQL ステートメントです。または、ストアド プロシージャを使用します。 テーブルまたはビューから列を選択するか、カスタム SQL ステートメントを入力する、SqlDataSource が s を制御するかどうか`SelectCommand`プロパティには、アドホック sql ステートメントが割り当てられている`SELECT`ステートメントがこの`SELECT`ステートメントが実行すると実行、SqlDataSource の`Select()`メソッドが呼び出されます (プログラムによってまたは自動的に Web コントロールのデータから)。

SQL`SELECT`ステートメントが不足して前のチュートリアル デモで使用される`WHERE`句。 `SELECT`ステートメント、`WHERE`返される結果を制限する句を使用できます。 たとえば、コスト 50.00 ドル以上の製品の名前を表示するには、次のクエリを使用おでした。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

通常で使用する値、`WHERE`句をクエリ文字列値、セッション変数、またはページ上の Web コントロールからのユーザー入力など、いくつかの外部ソースから決定されます。 使用するとこのような入力を指定することをお勧め*パラメーター*です。 使用して、Microsoft SQL Server でのパラメーターが示されて`@parameterName`など。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

SqlDataSource が、両方のパラメーター化クエリをサポートしている`SELECT`ステートメントと`INSERT`、 `UPDATE`、および`DELETE`ステートメントです。 さらに、パラメーター値できますプルされ、自動的にさまざまなソースから、クエリ文字列、セッションの状態 ページで、コントロールとなどまたはプログラムによって割り当てられることができます。 このチュートリアルではパラメーター化クエリもを宣言またはプログラムによって値パラメーターを指定する方法を定義する方法が表示されます。

> [!NOTE]
> 前のチュートリアルでは、概念的な類似性に注意してください、SqlDataSource によるチュートリアルでは、まず 46 経由で任意の当社のツールが行われた ObjectDataSource と比較されます。 こうした類似性は、パラメーターを拡張することもできます。 ビジネス ロジック層内のメソッドの入力パラメーターにマップされた ObjectDataSource のパラメーター。 SqlDataSource、パラメーターが、SQL クエリ内で直接定義されます。 両方のコントロールのパラメーターのコレクションがある、 `Select()`、 `Insert()`、 `Update()`、および`Delete()`メソッド、および両方には、これらのパラメーター値 (のクエリ文字列値、セッション変数、およびに事前に定義されたソースから設定を持つことができます) またはプログラムによって割り当てられます。


## <a name="creating-a-parameterized-query"></a>パラメーター クエリの作成

SqlDataSource コントロールのデータ ソース構成ウィザードでは、データベース レコードを取得するために実行するコマンドを定義するための 3 つの手段を提供します。

- 既存のテーブルまたはビューから列を選択して
- カスタムの SQL ステートメントを入力して、または
- ストアド プロシージャを選択して

既存のテーブルまたはビューのパラメーターからの列を選択するときに、`WHERE`句を追加して指定する必要があります`WHERE`句 ダイアログ ボックス。 カスタム SQL ステートメントを作成するときに、パラメーターを入力できますに直接、`WHERE`句 (を使用して`@parameterName`各パラメーターを示すため)。 A[ストアド プロシージャ](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)の 1 つまたは複数の SQL ステートメントで構成し、これらのステートメントをパラメーター化することができます。 SQL ステートメントで使用されるパラメーターただしに渡すことがで入力パラメーターとしてストアド プロシージャです。

パラメーター化クエリを作成する方法によって異なりますので SqlDataSource の`SelectCommand`が指定すると、使用すると、見てみましょうすべての 3 つの方法です。 最初に開く、 `ParameterizedQueries.aspx`  ページで、`SqlDataSource`フォルダー、SqlDataSource コントロールをツールボックスからデザイナーにドラッグし、設定、`ID`に`Products25BucksAndUnderDataSource`です。 次に、コントロールのスマート タグからのデータ ソースの構成のリンクをクリックします。 使用するデータベースを選択 (`NORTHWINDConnectionString`) し、[次へ] をクリックします。

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>手順 1: 追加、`WHERE`句のテーブルまたはビューから列を選択するときに

SqlDataSource コントロールを持つデータベースから返されるデータを選択すると、データ ソース構成ウィザードにより、単に既存のテーブルから返したり (図 1 を参照してください) を表示する列を選択します。 SQL を作成して、自動的`SELECT`ステートメントで、データベースに送信されるときに、SqlDataSource の`Select()`メソッドが呼び出されます。 前のチュートリアルで行ったように、ドロップダウン リストから、Products テーブルを選択し、確認、 `ProductID`、 `ProductName`、および`UnitPrice`列です。


[![テーブルまたはビューから返す列を選択します。](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**図 1**: テーブルまたはビューからの戻り値に列を選択 ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))


含める、`WHERE`句、`SELECT`ステートメントでは、をクリックして、 `WHERE`  ボタンの追加 を`WHERE`句 ダイアログ ボックス (図 2 を参照してください)。 によって返される結果を制限するパラメーターを追加する、`SELECT`クエリで最初にデータをフィルター処理する列を選択します。 次に、フィルター処理に使用する演算子を選択 (=、 &lt;、 &lt;=、&gt;など)。 クエリ文字列またはセッションの状態からなど、パラメーターの値のソースを最後に、選択します。 パラメーターを構成した後に含める追加 をクリックして、`SELECT`クエリ。

この例では、let s のみ結果を返す、場所、 `UnitPrice` 2,500 ドル未満の値がします。 そのため、選択`UnitPrice`列のドロップダウン リストからおよび&lt;= 演算子のドロップダウン リストからです。 (ドル 2,500) などのパラメーターのハードコード値を使用する場合、またはパラメーター値が、プログラムで指定する場合は、ソースのドロップダウン リストから [なし] を選択します。 次に、2,500 値のテキスト ボックスで、パラメーターのハードコード値を入力し、[追加] ボタンをクリックしてプロセスを完了します。


[![返される結果を制限する、追加 WHERE 句 ダイアログ ボックス](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**図 2**: 追加から返される結果を制限する`WHERE`句 ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))


パラメーターを追加すると、データ ソース構成ウィザードに戻るには [ok] をクリックします。 `SELECT`ウィザードの下部にあるステートメントを含める必要があります今すぐ、`WHERE`という名前のパラメーターを含む句`@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> 複数の条件を指定する場合、`WHERE`句の追加から`WHERE`句 ダイアログ ボックス、ウィザードで結合、`AND`演算子。 含める必要がある場合、`OR`で、`WHERE`句 (など`WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) を作成し、`SELECT`カスタム SQL ステートメント 画面でのステートメント。


SqlDataSource (クリックして次に、し、[完了]) の構成を完了し、SqlDataSource s の宣言型マークアップを検査します。 マークアップが含まれています、`<SelectParameters>`コレクションで、ソースでは、パラメーターを詳しく、`SelectCommand`です。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

ときに、SqlDataSource s`Select()`メソッドが呼び出され、`UnitPrice`パラメーター値 (2,500) に適用されます、`@UnitPrice`内のパラメーター、`SelectCommand`データベースに送信される前にします。 最終的な結果から返される 2,500 ドル未満の製品だけであるは、`Products`テーブル。 、これを確認 GridView のページに、このデータ ソースにバインドを追加し、ブラウザーからページを表示します。 図 3 の確認としての要素が 2,500 円、小さいが表示されている製品のみが表示されます。


[![これらの製品より小さいまたは 2,500 円に等しいかどうかのみが表示されます。](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**図 3**: のみこれらの製品より小さいか、2,500 円に等しいかどうかが表示されます ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>手順 2: カスタム SQL ステートメントにパラメーターを追加します。

カスタム SQL ステートメントを追加する場合は、入力、`WHERE`句に明示的にかクエリ ビルダーのフィルターのセルに値を指定します。 これを示すためには、価格が特定のしきい値よりも GridView にこれらの製品だけを表示する秒を使用できます。 テキスト ボックスを追加して、開始、`ParameterizedQueries.aspx`ユーザーからこのしきい値の値を収集するページ。 集合 TextBox s`ID`プロパティを`MaxPrice`です。 Button Web コントロールを追加し、設定、`Text`プロパティに一致する製品の表示にします。

次に、GridView をページにドラッグして、スマート タグをという名前の新しい SqlDataSource を作成する`ProductsFilteredByPriceDataSource`です。 データ ソース構成ウィザードから続行を指定するカスタム SQL ステートメントまたはストアド プロシージャの画面 (図 4 を参照) と、次のクエリを入力します。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

(手動でまたはクエリ ビルダーから) は、クエリを入力すると、[次へ] をクリックします。


[![パラメーターの値以下でなければ製品のみを返す](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**図 4**: 返すのみこれらの製品より小さいか、パラメーター値に等しいかどうか ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))


クエリには、パラメーターが含まれているため、ウィザードの次の画面は、パラメーター値のソースに us を求めます。 パラメーターのソースのドロップダウン リストからコントロールを選択し、 `MaxPrice` (TextBox コントロールの`ID`値) ControlID ドロップダウン リストからです。 ユーザーに任意のテキストの入力がない場合に使用するための省略可能な既定値を入力することも、 `MaxPrice`  テキスト ボックス。 当面の既定値を入力できません。


[![テキストのプロパティは、パラメーターのソースとして使用 ボックスに MaxPrice s](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**図 5**: `MaxPrice` TextBox s`Text`プロパティは、パラメーターのソースとして使用 ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))


次をクリックして、データ ソースの構成ウィザードを完了し、完了します。 宣言型マークアップ GridView、 テキスト ボックス、ボタン、および SqlDataSource を次に示します。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

注意してください SqlDataSource s 内のパラメーター`<SelectParameters>`セクションは、`ControlParameter`のような追加のプロパティを含む`ControlID`と`PropertyName`です。 ときに、SqlDataSource s`Select()`メソッドが呼び出され、 `ControlParameter` Web コントロールの指定したプロパティの値を取得し、それに対応するパラメーターに、`SelectCommand`です。 この例では、`MaxPrice`として s Text プロパティが使用される、`@MaxPrice`パラメーターの値。

ブラウザーからこのページを表示する分ほどをかかります。 最初のページにアクセスしたとき、またはたびに、 `MaxPrice`  テキスト ボックスが GridView でレコードが表示されない値がありません。


[![レコードは、表示されるときに、MaxPrice テキスト ボックスが空でないです。](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**図 6**: いいえレコードが表示されるときに、`MaxPrice`テキスト ボックスが空 ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))


製品が表示されない理由は、既定では、パラメーターの値に空の文字列がデータベースに変換されたため`NULL`値。 以降の比較`[UnitPrice] <= NULL`評価は常に False と結果は返されません。

5.00 と同様に、テキスト ボックスに値を入力し、一致する製品の表示 ボタンをクリックします。 ポストバックでは、SqlDataSource は GridView のパラメーターのソースの 1 つが変更を通知します。 その結果、GridView は、$5.00 をこれらの製品小さい以下を表示すること、SqlDataSource を再バインドします。


[![製品より小さいか、$5.00 を等しいかどうかが表示されます。](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**図 7**: 製品より小さいか、$5.00 を等しいかどうかが表示されます ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))


## <a name="initially-displaying-all-products"></a>すべての製品を最初に表示します。

表示いない製品ページが最初に読み込まれたときではなく表示したい場合があります*すべて*製品です。 すべての製品を一覧表示する方法の 1 つされるたびに、 `MaxPrice`  テキスト ボックスが空になんらかの非常に高い値パラメーターの既定値を設定することです 1000000 と同様に以降 s 珍しいこと Northwind Traders はこれまでがインベントリ単価を超えています $1,000,000 をします。 ただし、このアプローチは shortsighted は、他の状況で動作しない可能性があります。

-前のチュートリアルで[宣言型のパラメーター](../basic-reporting/declarative-parameters-vb.md)と[マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)おは同様の問題に直面していました。 あるソリューションは、ビジネス ロジック層にこのロジックを記述するでした。 具体的には、BLL が受信した値を調べるおよび、それが`NULL`または予約されているいくつかの値、返されたすべてのレコードを DAL メソッドへの呼び出しが送信されました。 パラメーター化されたために使用する SQL ステートメントを実行する DAL メソッドに呼び出しを試みました受信した値が通常のフィルター選択値である場合は、`WHERE`句で指定された値。

残念ながら、アーキテクチャが SqlDataSource を使用する場合はバイパスされます。 代わりに、インテリジェントな場合にすべてのレコードを取得する SQL ステートメントをカスタマイズする必要があります、`@MaximumPrice`パラメーターは`NULL`またはいくつかの予約済みの値。 この演習では、let s があるそのようにする場合、`@MaximumPrice`パラメーターと等しい`-1.0`、し、*すべて*レコードが返される (`-1.0`製品が、負の値があるないために、予約済みの値としては機能`UnitPrice`値)。 これを実現するには、次の SQL ステートメントを使用できます。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

これは、`WHERE`句を返します*すべて*場合、`@MaximumPrice`パラメーターと等しい`-1.0`です。 パラメーター値がない場合`-1.0`、製品のみが`UnitPrice`と同じかそれよりも少ない、`@MaximumPrice`パラメーターの値が返されます。 既定値を設定して、`@MaximumPrice`パラメーターを`-1.0`、最初のページ読み込みの (またはたびに、 `MaxPrice`  テキスト ボックスが空)、`@MaximumPrice`の値を持つ`-1.0`とすべての製品が表示されます。


[![これで、すべての製品表示されるときに、MaxPrice テキスト ボックスが空](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**図 8**: 現在のすべての製品が表示されるときに、`MaxPrice`テキスト ボックスが空 ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))


これは、いくつかの注意事項がこの方法に注意してください。 最初に、これによって、パラメーター %s のデータ型が推論されることに注意して、SQL クエリで s 使用します。 変更した場合、`WHERE`から句`@MaximumPrice = -1.0`に`@MaximumPrice = -1`ランタイムは、整数として、パラメーターを処理します。 割り当てるしようとする場合、 `MaxPrice` (5.00) のような 10 進数値をテキスト ボックスに、エラーが発生 5.00 整数に変換できないためです。 これを解決するには、いずれかの確認を使用すること`@MaximumPrice = -1.0`で、`WHERE`句、またはそれ以上がまだ、設定、`ControlParameter`オブジェクトの`Type`プロパティを 10 進数。

追加することで、次に、`OR @MaximumPrice = -1.0`を`WHERE`句、クエリ エンジンは、インデックスを使用できないで`UnitPrice`(存在する)、これにより、テーブル スキャンの結果として得られます。 影響する可能性がパフォーマンスのレコード数が十分な大きさがある場合、`Products`テーブル。 優れた方法はストアド プロシージャにこのロジックを移動することで、`IF`ステートメントは実行するか、`SELECT`からクエリ、`Products`なしテーブル、`WHERE`句のすべてのレコードが返される必要がある場合、またはそのいずれかが`WHERE`句のみを含み、`UnitPrice`条件、インデックスを使用できるようにします。

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>手順 3: を作成して、パラメーター化されたストアド プロシージャを使用します。

ストアド プロシージャは、ストアド プロシージャ内で定義されている SQL ステートメントで使用できる、入力パラメーターのセットを含めることができます。 入力パラメーターを受け取るストアド プロシージャを使用して SqlDataSource を構成する場合と同様に、アドホック SQL ステートメントと同じ手法を使用してこれらのパラメーター値を指定できます。

Let s が Northwind データベースの名前付きで新しいストアド プロシージャを作成、SqlDataSource でストアド プロシージャの使用を示すためには、 `GetProductsByCategory`、という名前のパラメーターを受け入れる`@CategoryID`し、すべての製品の列を返しますが`CategoryID`列に一致する`@CategoryID`です。 ストアド プロシージャを作成する、サーバー エクスプ ローラーに移動し、ドリルダウン、`NORTHWND.MDF`データベース。 サーバー エクスプ ローラーが表示されない場合は、したり (表示 メニューに移動し、サーバー エクスプ ローラーのオプションを選択します。)

`NORTHWND.MDF`データベース、Stored Procedures フォルダーを右クリックし、新しいストアド プロシージャの追加、選択、および、次の構文を入力します。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

保存 アイコン (または Ctrl + S) ストアド プロシージャを保存する をクリックします。 Stored Procedures フォルダーを右クリックして実行 を選択して、ストアド プロシージャをテストできます。 これはの入力を求め、s のストアド プロシージャのパラメーター (`@CategoryID`がこのインスタンスで)、どれ結果を表示するのには、出力ウィンドウの後にします。


[![GetProductsByCategory ストアド プロシージャを実行すると、 @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**図 9**:`GetProductsByCategory`ストアド プロシージャを実行すると、 `@CategoryID` 1 ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))


このストアド プロシージャを使用して GridView に、飲料カテゴリのすべての製品を表示する秒を使用できます。 新しい GridView をページに追加し、という名前の新しい SqlDataSource にバインド`BeverageProductsDataSource`です。 カスタム SQL ステートメントまたはストアド プロシージャの画面の指定を続行し、ストアド プロシージャのラジオ ボタンを選択し、、`GetProductsByCategory`ドロップダウン リストからストアド プロシージャです。


[![選択、GetProductsByCategory ドロップダウン リストからストアド プロシージャ](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**図 10**: 選択、`GetProductsByCategory`ドロップダウン リストからストアド プロシージャ ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))


ストアド プロシージャは、入力パラメーターを受け入れるため (`@CategoryID`)、このパラメーターの値のソースを指定することを求める [次へ] をクリックします。 飲み物`CategoryID`1 に設定されてため None パラメーターのソースのドロップダウン リストのままにして、DefaultValue ボックスに 1 を入力します。


[![1 のハードコード値を使用して、飲料カテゴリの製品を返す](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**図 11**: 1 の Hard-Coded 値を使用して、飲料カテゴリの製品を返す ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))


SqlDataSource s、ストアド プロシージャを使用する場合は次の宣言型マークアップ示すとして`SelectCommand`プロパティが、ストアド プロシージャの名前に設定され、 [ `SelectCommandType`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx)に設定されている`StoredProcedure`ことを示す`SelectCommand`アドホック SQL ステートメントではなく、ストアド プロシージャの名前を指定します。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

ブラウザーでページをテストします。 飲み物のカテゴリに属する製品だけが表示されますが、*すべて*以降、製品のフィールドの表示、`GetProductsByCategory`ストアド プロシージャが返すすべての列から、`Products`テーブル。 もちろん、制限したり、GridView 列の編集 ダイアログ ボックスから GridView に表示されるフィールドをカスタマイズお可能性があります。


[![飲み物のすべてが表示されます。](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**図 12**: 飲み物のすべてが表示されます ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>手順 4: が SqlDataSource s をプログラムで呼び出す`Select()`ステートメント

例は前のチュートリアルとこのチュートリアルでこれまで見てきましたが、GridView に直接 SqlDataSource コントロールをバインドします。 SqlDataSource コントロールのデータただし、プログラムでアクセスしてコードで列挙します。 検査し、データをクエリする必要がありますが、それを表示する必要がないある場合は特に便利にできます。 すべてのデータベースへの接続、コマンドを指定し、結果を取得して ADO.NET コード定型句を書き込むのではなくこの単調コード処理 SqlDataSource をさせることができます。

SqlDataSource s の扱いを説明するためにデータ プログラムでは、想像してください、上司は、ランダムに選択したカテゴリと関連付けられた製品の名前を表示する web ページを作成する要求に近づきます。 つまり、ユーザーがこのページにアクセスしたいからカテゴリをランダムに選択、`Categories`テーブルをカテゴリ名を表示し、そのカテゴリに属している製品の一覧です。

これを実現する 2 つ SqlDataSource コントロール 1 からランダムなカテゴリを取得する必要があります、`Categories`テーブルと製品カテゴリを取得します。 このステップでは; でランダムなカテゴリ レコードを取得する SqlDataSource ビルドします手順 5 はカテゴリの製品を取得する SqlDataSource を作成します。

SqlDataSource を追加することによって開始`ParameterizedQueries.aspx`設定とその`ID`に`RandomCategoryDataSource`です。 次の SQL クエリを使用するように構成します。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()` ランダムな順序で並べ替えられたレコードを返します (を参照してください[Using`NEWID()`ランダムにレコードの並べ替えに](http://www.sqlteam.com/item.asp?ItemID=8747))。 `SELECT TOP 1` 結果セットから最初のレコードを返します。 まとめるには、このクエリを返します、`CategoryID`と`CategoryName`、ランダムに選択された 1 つのカテゴリの列の値。

カテゴリ s を表示する`CategoryName`値 Label Web コントロールをページに追加設定その`ID`プロパティを`CategoryNameLabel`、クリアとその`Text`プロパティ。 SqlDataSource コントロールからプログラムでデータを取得する必要がありますを呼び出す、`Select()`メソッドです。 [ `Select()`メソッド](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx)型の単一の入力パラメーターが必要ですが[ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)、返される前に、データをメッセージする方法を指定します。 これにより、並べ替えと、データをフィルター処理に指示を含めることができ、Web コントロールの並べ替えまたは SqlDataSource コントロールからのデータを取得するときにデータで使用されています。 この例では、お返される前に変更する t 必要データはなく、そのために渡されます、`DataSourceSelectArguments.Empty`オブジェクト。

`Select()`メソッドを実装するオブジェクトを返します`IEnumerable`です。 SqlDataSource コントロール秒の値に依存に返される正確な型[`DataSourceMode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)です。 このプロパティをいずれかの値に設定できる前のチュートリアルで既に説明した、`DataSet`または`DataReader`です。 場合に設定`DataSet`、`Select()`メソッドを返します、 [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx)オブジェクト以外に設定した場合`DataReader`を実装するオブジェクトを返します[ `IDataReader`](https://msdn.microsoft.com/library/system.data.idatareader.aspx)です。 以降、 `RandomCategoryDataSource` SqlDataSource がその`DataSourceMode`プロパティに設定`DataSet`(既定)、おを操作し、DataView オブジェクト。

次のコードからレコードを取得する方法を示しています、 `RandomCategoryDataSource` DataView として SqlDataSource を読み取る方法だけでなく、 `CategoryName` DataView の最初の行の列の値。


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)` 最初を返します`DataRowView`DataView でします。 `randomCategoryView(0)("CategoryName")` 値を返します、`CategoryName`この最初の行に列です。 DataView は、厳密に型は注意してください。 特定の列の値を参照するには、列の名前 (このケースでは CategoryName) を文字列として渡す必要があります。 図 13 に表示されるメッセージを示しています、`CategoryNameLabel`ページを表示するときにします。 によって表示される実際のカテゴリ名をランダムに選択もちろん、 `RandomCategoryDataSource` SqlDataSource (ポストバックを含む)、ページへのアクセスにします。


[![名前が表示されるカテゴリをランダムに選択されている s](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**図 13**: 名前が表示される、ランダムに選択したカテゴリ s ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))


> [!NOTE]
> SqlDataSource コントロール s 場合`DataSourceMode`プロパティに設定された`DataReader`からの戻り値、`Select()`メソッドにキャストする必要がある`IDataReader`です。 読み取り、 `CategoryName` d は最初の行の列の値のようなコードを使用します。


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

SqlDataSource でランダムに選択するカテゴリでは、カテゴリの製品を一覧表示する GridView を追加する準備ができたらします。

> [!NOTE]
> カテゴリの名前を表示するには、ラベルの Web コントロールを使用してではなくおでしたが FormView または DetailsView に追加 ページで、SqlDataSource にバインドすること。 ただし、SqlDataSource s をプログラムで呼び出す方法を調査することを許可、ラベルを使用して、`Select()`ステートメントとコードの結果として得られるそのデータを操作します。


## <a name="step-5-assigning-parameter-values-programmatically"></a>手順 5: プログラムによってパラメーター値を割り当てる

すべての例は、パラメーターのハードコード値または定義済みパラメーターのソース (クエリ文字列値、ページ上の Web コントロール) のいずれかから行われた、このチュートリアルでこれまで見てきましたが使用します。 ただし、SqlDataSource コントロールのパラメーターを設定することもプログラムでします。 完了するには、現在の使用例をすべての指定したカテゴリに属する製品を返す SqlDataSource 必要があります。 この SqlDataSource されますが、`CategoryID`を設定する必要がある値を持つパラメーターに基づいて、`CategoryID`によって返される列の値、`RandomCategoryDataSource`で SqlDataSource、`Page_Load`イベント ハンドラー。

GridView をページに追加することで開始し、という名前の新しい SqlDataSource にバインド`ProductsByCategoryDataSource`です。 かなりよう手順 3. を呼び出すようにの構成、SqlDataSource、`GetProductsByCategory`ストアド プロシージャです。 ままにして、パラメーター ソース ドロップダウン リストを [なし] が、プログラムでこの既定値は設定、既定値を入力してくださいはありません。


[![パラメーターのソースまたは既定値を指定しません。](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**図 14**: パラメーターのソースまたは既定値を指定しない ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))


SqlDataSource ウィザードを完了すると、結果として得られる宣言型マークアップを次のようになります。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

私たちを割り当てることができます、`DefaultValue`の`CategoryID`パラメーターをプログラム的に、`Page_Load`イベントのハンドラー。


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

この追加すると、ページには、ランダムに選択したカテゴリに関連付けられている製品を表示する GridView が含まれます。


[![パラメーターのソースまたは既定値を指定しません。](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**図 15**: パラメーターのソースまたは既定値を指定しない ([フルサイズのイメージを表示するをクリックして](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))


## <a name="summary"></a>まとめ

SqlDataSource 開発者はページをパラメーター値を持つハード コーディングされた、定義済みパラメーターのソースから取得したりプログラムによって割り当てられているパラメーター化クエリを定義できます。 このチュートリアルでは、アドホック SQL クエリとストアド プロシージャの両方のデータ ソース構成ウィザードからのパラメーター化クエリを作成する方法を説明しました。 についても説明しましたパラメーターのハードコード ソース、パラメーターのソースとしての Web コントロールを使用して、プログラムで、パラメーターの値を指定します。

ように、ObjectDataSource SqlDataSource も提供しています、基になるデータを変更する機能。 次のチュートリアルでは紹介を定義する方法で`INSERT`、 `UPDATE`、および`DELETE`SqlDataSource によるステートメントです。 これらのステートメントが追加されると、挿入、編集、および、GridView、DetailsView、およびフォーム ビュー コントロールに固有の機能を削除すると、組み込みを利用できます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Scott クライド、Randell Schmidt Ken Pespisa がされていました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](querying-data-with-the-sqldatasource-control-vb.md)
> [次へ](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
