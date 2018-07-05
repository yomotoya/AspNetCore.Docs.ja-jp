---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: パラメーター化クエリを使用、SqlDataSource (c#) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、SqlDataSource コントロールで、外観を続行し、パラメーター化クエリを定義する方法について説明します。 パラメーターを指定する両方の宣言しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 2cafcf0938704a02f163c3d1646b7b4a9f38c3aa
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400756"
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>パラメーター化されたクエリと SqlDataSource (c#) を使用
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe)または[PDF のダウンロード](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> このチュートリアルでは、SqlDataSource コントロールで、外観を続行し、パラメーター化クエリを定義する方法について説明します。 パラメーターを宣言およびプログラムで指定することができますな数など、クエリ文字列、セッション状態、その他のコントロール、および複数の場所から取得できます。


## <a name="introduction"></a>はじめに

前のチュートリアルでは、SqlDataSource コントロールを使用して、データベースから直接データを取得する方法を説明しました。 データ ソース構成ウィザードを使用して、選択できるデータベースし、: テーブルまたはビューから返す列の選択カスタム SQL ステートメントを入力しますまたは、ストアド プロシージャを使用します。 テーブルまたはビューから列を選択またはカスタムの SQL ステートメントを入力、SqlDataSource が s を制御するかどうか`SelectCommand`プロパティには、結果として得られる、アドホック SQL が割り当てられている`SELECT`ステートメントと、これは、`SELECT`ステートメントが実行されるときに、SqlDataSource の`Select()`メソッドが呼び出されます (プログラムまたは自動的にデータ Web コントロールから)。

SQL`SELECT`ステートメントが不足して前のチュートリアルのデモで使用される`WHERE`句。 `SELECT`ステートメント、`WHERE`返される結果を制限する句を使用できます。 たとえば、50.00 ドル以上のコストの製品の名前を表示するには、次のクエリを使用しましたでした。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

通常で使用する値を`WHERE`句はクエリ文字列値、セッション変数、または、ページ上の Web コントロールからのユーザー入力などのいくつかの外部ソースによって決定します。 使用するとそのような入力を指定する理想的には、*パラメーター*します。 使用して、Microsoft SQL Server でのパラメーターが表記は`@parameterName`。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

SqlDataSource、両方のパラメーター化クエリをサポートしている`SELECT`ステートメントと`INSERT`、 `UPDATE`、および`DELETE`ステートメント。 さらに、パラメーターの値に自動的にプルできるさまざまなソースからクエリ文字列、セッションの状態 ページで、コントロールと具合またはプログラムによって割り当てられたことができます。 このチュートリアルでは、宣言とプログラミングの両方の値パラメーターを指定する方法と、パラメーター化クエリもを定義する方法がわかります。

> [!NOTE]
> 前のチュートリアルでは、概念的な類似性を示す、SqlDataSource によるチュートリアルでは、まず 46 経由で任意の当社のツールが行われた ObjectDataSource を比較しました。 これらの類似点は、パラメーターに拡張することもできます。 ビジネス ロジック層のメソッドの入力パラメーターにマップされている ObjectDataSource のパラメーター。 SqlDataSource、パラメーターは、SQL クエリ内で直接定義されます。 両方のコントロールのパラメーターのコレクションがある、 `Select()`、 `Insert()`、 `Update()`、および`Delete()`メソッド、および両方には、定義済みのソース (のクエリ文字列値、セッション変数、およびなどから設定されたこれらのパラメーター値を持つことができます) またはプログラムによって割り当てられています。


## <a name="creating-a-parameterized-query"></a>パラメーター クエリの作成

SqlDataSource コントロールのデータ ソースの構成ウィザードでは、データベース レコードを取得するために実行するコマンドを定義するための 3 つの手段を提供します。

- 既存のテーブルまたはビューから列を選択して
- カスタムの SQL ステートメントを入力して、または
- ストアド プロシージャを選択して

既存のテーブルまたはビューのパラメーターからの列を選択するときに、`WHERE`句を追加して指定する必要があります`WHERE`句 ダイアログ ボックス。 カスタム SQL ステートメントを作成するときに、パラメーターを入力できますに直接、`WHERE`句 (を使用して`@parameterName`各パラメーターを示すため)。 A[ストアド プロシージャ](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)の 1 つまたは複数の SQL ステートメントで構成し、これらのステートメントをパラメーター化することができます。 SQL ステートメントで使用されるパラメーター、渡す必要がありますで入力パラメーターとしてストアド プロシージャにします。

パラメーター化クエリを作成する方法によって異なりますので SqlDataSource の`SelectCommand`が指定すると、let を見てすべての 3 つの方法です。 最初に、開く、`ParameterizedQueries.aspx`ページで、`SqlDataSource`フォルダー、SqlDataSource コントロールをツールボックスからデザイナーにドラッグし、設定、`ID`に`Products25BucksAndUnderDataSource`します。 次に、コントロールのスマート タグからのデータ ソースの構成のリンクをクリックします。 使用するデータベースを選択 (`NORTHWINDConnectionString`) [次へ] をクリックします。

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>手順 1: 追加、`WHERE`句のテーブルまたはビューから列を選択するときに

SqlDataSource コントロールをデータベースから返すデータを選択すると、データ ソースの構成ウィザードでは単に既存のテーブルから返したり (図 1 参照) を表示する列を選択することができます。 SQL を作成して、自動的に行う`SELECT`ステートメントで、データベースに送信されるときに SqlDataSource の`Select()`メソッドが呼び出されます。 前のチュートリアルで行ったようにドロップダウン リストから、Products テーブルを選択し、確認、 `ProductID`、 `ProductName`、および`UnitPrice`列。


[![テーブルまたはビューから返す列を選択します。](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**図 1**: テーブルまたはビューからの戻り値に列を選択 ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))。


含める、`WHERE`句、`SELECT`ステートメントでは、をクリックして、`WHERE`ボタンが追加表示されます`WHERE`句 ダイアログ ボックス (図 2 参照)。 によって返される結果を制限するパラメーターを追加する、`SELECT`クエリ、まず、データをフィルター処理する列を選択します。 次に、フィルター処理するために使用する演算子を選択 (=、 &lt;、 &lt;=、&gt;など)。 最後に、クエリ文字列またはセッションの状態からなど、パラメーターの値のソースを選択します。 パラメーターを構成した後では、[追加] ボタンをクリックします。、`SELECT`クエリ。

この例では、let s のみ結果を返す、場所、`UnitPrice`値は、2,500 ドル以下です。 そのため、選択`UnitPrice`列のドロップダウン リストからおよび&lt;= 演算子のドロップダウン リストから。 ($2,500) などのハード コーディングされたパラメーター値を使用する場合、またはパラメーター値は、プログラムで指定する場合は、ソースのドロップダウン リストから [なし] を選択します。 次に、2,500 値のテキスト ボックスで、ハード コーディングされたパラメーターの値を入力し、[追加] ボタンをクリックしてプロセスを完了します。


[![返される結果を制限する、追加 WHERE 句 ダイアログ ボックス](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**図 2**: 追加から返される結果を制限する`WHERE`句 ダイアログ ボックス ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))。


パラメーターを追加した後は、データ ソース構成ウィザードに戻るには、[ok] をクリックします。 `SELECT`ウィザードの下部にあるステートメントが、`WHERE`という名前のパラメーターを含む句`@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> 複数の条件を指定する場合、`WHERE`句の追加から`WHERE`句 ダイアログ ボックスで、ウィザードで結合、`AND`演算子。 含める必要がある場合、`OR`で、`WHERE`句 (など`WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`) を構築する必要があります、`SELECT`カスタム SQL ステートメント 画面でのステートメント。


SqlDataSource (をクリックして次に、し、[完了]) の構成を完了し、し、SqlDataSource s の宣言型マークアップを検査します。 マークアップが含まれています、`<SelectParameters>`コレクションで、パラメーターのソースを綴った、`SelectCommand`します。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

ときに SqlDataSource s`Select()`メソッドが呼び出される、`UnitPrice`パラメーター値 (2,500) に適用されます、`@UnitPrice`パラメーター、`SelectCommand`データベースに送信される前にします。 最終的には 2,500 ドル未満から返される製品だけである、`Products`テーブル。 ページに GridView を追加、これを確認するには、このデータ ソースにバインドし、ブラウザーでページを表示します。 記載されている 2,500 円、小さい図 3 が確認されるこれらの製品のみが表示されます。


[![唯一のこれらの製品より小さいまたは等しい 2,500 ドルが表示されます。](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**図 3**: のみこれらの製品より小さいまたは等しい 2,500 ドルが表示されます ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))。


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>手順 2: カスタム SQL ステートメントにパラメーターを追加します。

カスタム SQL ステートメントを追加する場合は、入力、`WHERE`句に明示的にまたはクエリ ビルダーのフィルター セルで値を指定します。 これを示すためには、秒の価格は、特定のしきい値より小さい GridView でこれらの製品だけを表示することができます。 開始するための TextBox を追加することで、`ParameterizedQueries.aspx`ユーザーから、このしきい値の値を収集するページ。 テキスト ボックス s 設定`ID`プロパティを`MaxPrice`します。 Button Web コントロールを追加し、設定、`Text`プロパティに一致する製品の表示にします。

次に、GridView をページにドラッグし、という名前の新しい SqlDataSource を作成することも、スマート タグから`ProductsFilteredByPriceDataSource`します。 データ ソースの構成ウィザードから続行を指定するカスタム SQL ステートメントまたはストアド プロシージャの画面 (図 4 参照) し、次のクエリを入力します。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

(手動またはクエリ ビルダーによって) をクエリを入力した後、[次へ] をクリックします。


[![パラメーターの値を小さい製品のみを返す](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**図 4**: 返すのみこれらの製品より小さいまたはパラメーターの値に等しい ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))。


クエリには、パラメーターが含まれているため、ウィザードの次の画面は私たちをパラメーター値のソースを要求します。 パラメーターのソースのドロップダウン リストからコントロールを選択し、 `MaxPrice` (TextBox コントロールの`ID`値) ControlID のドロップダウン リストから。 ユーザーに任意のテキストの入力がない場合に使用する、省略可能な既定値を入力することも、`MaxPrice`テキスト ボックス。 しばらくの間には、既定値を入力しません。


[![MaxPrice TextBox の Text プロパティは、パラメーター ソースとして使用します。](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**図 5**:`MaxPrice`テキスト ボックス s`Text`プロパティ パラメーターのソースとして使用されます ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))。


次をクリックして、データ ソースの構成ウィザードを完了し、完了します。 宣言型マークアップ GridView、テキスト ボックス、ボタン、および SqlDataSource を次に示します。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

注意 SqlDataSource s 内にあるパラメーター`<SelectParameters>`セクションは、`ControlParameter`のような追加のプロパティを含む`ControlID`と`PropertyName`。 ときに、SqlDataSource s`Select()`メソッドが呼び出される、 `ControlParameter` Web コントロールの指定したプロパティから値を取得し、対応するパラメーターに割り当てます、`SelectCommand`します。 この例で、`MaxPrice`としてテキスト プロパティが使用される、`@MaxPrice`パラメーターの値。

ブラウザーからこのページを表示する時間がかかります。 まず、ページにアクセスしたとき、またはたびに、`MaxPrice`テキスト ボックスに、GridView でレコードが表示されない値が不足しています。


[![表示されるときに、MaxPrice テキスト ボックスが空のレコードはありません。](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**図 6**: No レコードが表示されるときに、`MaxPrice`テキスト ボックスが空 ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))。


製品が表示されない理由は、既定では、パラメーターの値に空の文字列がデータベースに変換されますので、`NULL`値。 以降の比較`[UnitPrice] <= NULL`評価は常に false の場合、結果は返されません。

5.00 のように、テキスト ボックスに値を入力し、一致する製品の表示 ボタンをクリックします。 、ポストバックの SqlDataSource は GridView、パラメーター ソースの 1 つが変更されたことを通知します。 その結果、GridView が SqlDataSource、5.00 にこれらの製品少ない以下を表示するのに再バインドします。


[![製品のより小さいまたは等しい 5.00 ドルが表示されます。](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**図 7**: 製品より小さいまたは等しい 5.00 ドルが表示されます ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))。


## <a name="initially-displaying-all-products"></a>最初にすべての製品を表示します。

表示したい場合があります、ページが最初に読み込まれたときに表示製品ではなく*すべて*製品です。 すべての製品を一覧表示する方法の 1 つたびに、`MaxPrice`テキスト ボックスが空パラメーター s の既定値をいくつか非常に高い値に設定するのには、1000000 のようなので、Northwind Traders はこれまでがインベントリ単価可能性は低い s を超えています $1,000,000。 ただし、このアプローチは shortsighted であり、その他の状況では機能しない可能性があります。

前のチュートリアル -[宣言パラメーター](../basic-reporting/declarative-parameters-cs.md)と[マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)いますが、同様の問題に直面していました。 あるソリューションは、ビジネス ロジック層でこのロジックを配置することでした。 具体的には、受信した値の検査、BLL および、それが`NULL`または予約されているいくつかの値、呼び出しが返されたすべてのレコードを DAL メソッドにルーティングされました。 パラメーター化されたために使用する SQL ステートメントを実行する DAL メソッドに呼び出しが受信した値が通常のフィルター選択値の場合は、`WHERE`句で指定された値。

残念ながら、アーキテクチャは、SqlDataSource を使用する場合バイパスします。 代わりに、インテリジェントな場合にすべてのレコードを取得する SQL ステートメントをカスタマイズする必要があります、`@MaximumPrice`パラメーターが`NULL`またはいくつか予約済みの値。 この演習では、let s がある、そのため場合、`@MaximumPrice`パラメーターが等しく`-1.0`、し*すべて*レコードが返される (`-1.0`製品が、負の値があるないために、予約済みの値として動作`UnitPrice`値)。 これを実現するには、次の SQL ステートメントを使用できます。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

これは、`WHERE`句を返します*すべて*場合、`@MaximumPrice`パラメーターと等しい`-1.0`します。 パラメーターの値がない場合`-1.0`、これらの製品のみが`UnitPrice`に等しいまたはそれよりも小さい、`@MaximumPrice`パラメーターの値が返されます。 既定値を設定して、`@MaximumPrice`パラメーターを`-1.0`、最初のページの読み込み時 (またはたびに、`MaxPrice`テキスト ボックスが空)、`@MaximumPrice`の値を持ちます`-1.0`とすべての製品が表示されます。


[![これで、すべての製品が表示されるときに、MaxPrice テキスト ボックスが空](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**図 8**: 今すぐすべての製品が表示されるときに、`MaxPrice`テキスト ボックスが空 ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))。


いくつかの注意事項がこの方法に注意してください。 これによって、パラメーター データ型を推論することを最初に、実現、SQL クエリで s の使用状況。 変更した場合、`WHERE`句から`@MaximumPrice = -1.0`に`@MaximumPrice = -1`ランタイムは、パラメーター、整数として扱います。 割り当てるしようとした場合、 `MaxPrice` (5.00) などの 10 進値をテキスト ボックスに、5.00 整数に変換できないため、エラーが発生します。 これを解決するいずれかを使用することを確認する`@MaximumPrice = -1.0`で、`WHERE`句、またはそれ以上がまだ、設定、`ControlParameter`オブジェクト`Type`プロパティを 10 進数。

第二に、追加することで、`OR @MaximumPrice = -1.0`に、`WHERE`句、クエリ エンジン インデックスを使用できないの`UnitPrice`(存在する)、テーブル スキャンになります。 内のレコード数が十分に大きい場合がある場合のパフォーマンスに影響する可能性、`Products`テーブル。 優れたアプローチはストアド プロシージャにこのロジックを移動することで、`IF`ステートメントは実行するか、`SELECT`からクエリを実行、`Products`なしテーブル、`WHERE`句のすべてのレコードが返される必要がある場合、またはそのいずれかが`WHERE`句のみを含み、`UnitPrice`条件、インデックスを使用できるようにします。

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>手順 3: を作成して、パラメーター化されたストアド プロシージャの使用

ストアド プロシージャは、ストアド プロシージャ内で定義されている SQL ステートメントで使用して入力パラメーターのセットを含めることができます。 入力パラメーターを受け取るストアド プロシージャを使用する SqlDataSource を構成するときに、アドホック SQL ステートメントと同じ手法を使用してこれらのパラメーター値を指定できます。

SqlDataSource でストアド プロシージャの使用を示すためには、let s では、Northwind データベースの名前付きで、新しいストアド プロシージャを作成`GetProductsByCategory`、という名前のパラメーターを受け入れる`@CategoryID`し、すべての製品の列を返しますが`CategoryID`列に一致する`@CategoryID`します。 ストアド プロシージャを作成するには、サーバー エクスプ ローラーに移動し、ドリルダウン、`NORTHWND.MDF`データベース。 (サーバー エクスプ ローラーが表示されない場合は、起動に表示 メニューに移動し、サーバー エクスプ ローラーのオプションを選択します。)

`NORTHWND.MDF`データベース、ストアド プロシージャ フォルダーを右クリックし、新しいストアド プロシージャの追加、選択、および、次の構文を入力します。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

保存アイコン (または Ctrl + S)、ストアド プロシージャを保存する をクリックします。 ストアド プロシージャをテストするにはストアド プロシージャ フォルダーを右クリックして実行 を選択します。 これは、ストアド プロシージャのパラメーターが要求されます (`@CategoryID`、このインスタンスで) 後が、結果が表示されます、出力ウィンドウにします。


[![GetProductsByCategory ストアド プロシージャを実行すると、 @CategoryID 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**図 9**:`GetProductsByCategory`ストアド プロシージャを実行すると、 `@CategoryID` 1 の ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))。


このストアド プロシージャを使用して、GridView で飲み物のカテゴリのすべての製品を表示する秒を使用できます。 新しい GridView をページに追加し、という名前の新しい SqlDataSource にバインドする`BeverageProductsDataSource`します。 カスタム SQL ステートメントまたはストアド プロシージャの画面の指定に引き続き、ストアド プロシージャのラジオ ボタンを選択および選択、`GetProductsByCategory`ストアド プロシージャをドロップダウン リストから。


[![選択、GetProductsByCategory ストアド プロシージャをドロップダウン リストから](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**図 10**: 選択、`GetProductsByCategory`ドロップダウン リストからストアド プロシージャ ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))。


ストアド プロシージャは、入力パラメーターを受け取るので (`@CategoryID`)、[次へ] をクリックするとこのパラメーターの値のソースを指定することを求められます。 飲み物`CategoryID`は 1 です。 したがって None パラメーターのソースのドロップダウン リストのままにしてと DefaultValue テキスト ボックスに 1 を入力します。


[![ハード コーディングされた値 1 を使用して、飲料カテゴリ、製品を返す](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**図 11**: Hard-Coded 値 1 を使用して、飲料カテゴリ、製品を返す ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))。


SqlDataSource s、ストアド プロシージャを使用する場合は次の宣言型マークアップ示す`SelectCommand`ストアド プロシージャの名前に設定されて、 [ `SelectCommandType`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx)に設定されている`StoredProcedure`を示します`SelectCommand`アドホック SQL ステートメントではなく、ストアド プロシージャの名前を指定します。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

ブラウザーでページをテストします。 飲み物のカテゴリに属する製品だけが表示されますが、*すべて*以降、製品のフィールドが表示されます、`GetProductsByCategory`ストアド プロシージャが返すすべての列から、`Products`テーブル。 もちろん、制限または GridView の列の編集 ダイアログ ボックスから GridView に表示されるフィールドをカスタマイズすることでした。


[![飲み物のすべてが表示されます。](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**図 12**: 飲み物のすべてが表示されます ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))。


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>手順 4: が SqlDataSource s をプログラムで呼び出す`Select()`ステートメント

例ではこれまでに、前のチュートリアルとこのチュートリアルで確認したところでが GridView に直接、SqlDataSource コントロールがバインドされています。 SqlDataSource コントロールのデータで、ただし、プログラムでアクセスおよびできるコードに列挙されました。 これを検査し、データをクエリする必要があるが、それを表示する必要がないある場合に特に役立ちます。 すべての定型データベースへの接続、コマンドを指定し、結果を取得する ADO.NET コードを記述するのではなく、SqlDataSource 単調このコードを処理させることができます。

SqlDataSource s の操作を説明するために、上司がランダムに選択されたカテゴリおよび関連付けられた製品の名前を表示する web ページの作成要求を使用するに近づくをデータは、プログラムで imagine します。 つまり、ユーザーがこのページにアクセスしたいからカテゴリをランダムに選択する、`Categories`テーブルをカテゴリ名を表示し、そのカテゴリに属する製品を一覧表示します。

これを実現する 2 つ SqlDataSource コントロール 1 つからランダムなカテゴリを取得する必要があります、`Categories`テーブルと製品カテゴリを取得します。 ビルドします。 この手順ではランダムなカテゴリ レコードを取得する SqlDataSource手順 5 がカテゴリの製品を取得する SqlDataSource を作成します。

SqlDataSource を追加することで開始`ParameterizedQueries.aspx`設定とその`ID`に`RandomCategoryDataSource`します。 次の SQL クエリを使用するように構成します。


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()` ランダムな順序で並べ替えたレコードを返します (を参照してください[Using`NEWID()`レコードをランダムに並べ替えを](http://www.sqlteam.com/item.asp?ItemID=8747))。 `SELECT TOP 1` 結果セットから最初のレコードを返します。 まとめるには、このクエリを返します、`CategoryID`と`CategoryName`、ランダムに選択した 1 つのカテゴリの列の値。

S カテゴリを表示する`CategoryName`値、Label Web コントロールをページに追加、設定、`ID`プロパティを`CategoryNameLabel`、クリアとその`Text`プロパティ。 SqlDataSource コントロールからプログラムでデータを取得する必要がありますを呼び出すその`Select()`メソッド。 [ `Select()`メソッド](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx)型の 1 つの入力パラメーターが必要ですが[ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)、返される前に、データをメッセージする方法を指定します。 これは、手順については、並べ替えと、データをフィルター処理を含めることができ、Web コントロールの並べ替えまたは SqlDataSource コントロールからのデータをページングする場合、データによって使用されます。 この例で、返される前に変更する t 必要データのないおで渡すはそのため、`DataSourceSelectArguments.Empty`オブジェクト。

`Select()`メソッドを実装するオブジェクトを返します`IEnumerable`します。 SqlDataSource コントロール秒の値に依存に厳密な型が返される[`DataSourceMode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)します。 いずれかの値をこのプロパティを設定することができます、前のチュートリアルで既に説明した、`DataSet`または`DataReader`します。 場合に設定`DataSet`、`Select()`メソッドを返します。 を[DataView](https://msdn.microsoft.com/library/01s96x0z.aspx)オブジェクト以外に設定した場合`DataReader`、実装するオブジェクトを返す[ `IDataReader`](https://msdn.microsoft.com/library/system.data.idatareader.aspx)します。 以降、 `RandomCategoryDataSource` SqlDataSource がその`DataSourceMode`プロパティに設定`DataSet`(既定)、ここで使用される、DataView オブジェクト。

次のコードからレコードを取得する方法を示しています、 `RandomCategoryDataSource` DataView と SqlDataSource を読み取る方法と、 `CategoryName` DataView の最初の行の列の値。


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]` 最初を返します`DataRowView`DataView にします。 `randomCategoryView[0]["CategoryName"]` 値を返します、`CategoryName`この最初の行の列。 データ ビューでは、弱い型指定があるに注意してください。 特定の列の値を参照するには、文字列 (このケースでは、CategoryName) として、列の名前を渡す必要があります。 図 13 に表示されるメッセージでは、`CategoryNameLabel`ページを表示するときにします。 によって表示される実際のカテゴリ名をランダムに選択もちろん、 `RandomCategoryDataSource` SqlDataSource (ポストバックを含む)、ページにアクセスするたびにします。


[![名前が表示されます、ランダムに選択したカテゴリ s](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**図 13**: ランダムに選択したカテゴリ s の名前が表示されます ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))。


> [!NOTE]
> SqlDataSource コントロール s 場合`DataSourceMode`プロパティ設定した`DataReader`からの戻り値、`Select()`メソッドにキャストする必要がある`IDataReader`します。 読み取り、`CategoryName`のようなコードを使用して、d 最初の行の列の値。


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

SqlDataSource でランダムに選択カテゴリでは、カテゴリの製品を一覧表示する GridView を追加する準備ができたらします。

> [!NOTE]
> カテゴリの名前を表示するラベル Web コントロールを使用してではなく追加 FormView や DetailsView、ページに SqlDataSource にバインドすること。 ただし、SqlDataSource s をプログラムで呼び出す方法について説明することを許可、ラベルを使用して、`Select()`ステートメントとコードでその結果として得られるデータと連携します。


## <a name="step-5-assigning-parameter-values-programmatically"></a>手順 5: プログラムによってパラメーター値を割り当てる

すべての例ハード コーディングされたパラメーター値または定義済みパラメーターのソース (クエリ文字列値をページで、上の Web コントロール) のいずれかから行われたこのチュートリアルではこれまで確認したところを使用します。 ただし、SqlDataSource コントロールのパラメーターがプログラムで設定もできます。 現在の例を完了するには、すべての指定したカテゴリに属する製品を返す SqlDataSource 必要があります。 この SqlDataSource は必要があります、`CategoryID`を設定する必要がある値を持つパラメーターがに基づいて、`CategoryID`によって返される列の値、`RandomCategoryDataSource`で SqlDataSource、`Page_Load`イベント ハンドラー。

GridView をページに追加することで開始し、という名前の新しい SqlDataSource にバインドする`ProductsByCategoryDataSource`します。 手順 3 で行ったようなより、SqlDataSource の構成を呼び出すように、`GetProductsByCategory`ストアド プロシージャ。 None、パラメーター ソースのドロップダウン リストの設定をそのまま使用しますが、この既定値をプログラムで設定しますが、既定値を入力しないでください。


[![パラメーターのソースまたは既定値を指定しません。](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**図 14**: パラメーターのソースまたは既定値を指定しない ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))。


SqlDataSource ウィザードを完了すると、結果として得られる宣言型マークアップは次のようになります。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

割り当てることができます、`DefaultValue`の`CategoryID`パラメーター内でプログラムによって、`Page_Load`イベント ハンドラー。


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

これにより、ページには、ランダムに選択したカテゴリに関連付けられている製品を表示する GridView が含まれます。


[![パラメーターのソースまたは既定値を指定しません。](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**図 15**: パラメーターのソースまたは既定値を指定しない ([フルサイズの画像を表示する をクリックします](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))。


## <a name="summary"></a>まとめ

SqlDataSource では、ページ開発者は、パラメーター値を持つハード コーディングされた、定義済みパラメーターのソースからプルしたり、プログラムによって割り当てられているパラメーター化されたクエリの定義を使用できます。 このチュートリアルでは、アドホック SQL クエリとストアド プロシージャの両方のデータ ソース構成ウィザードからのパラメーター化クエリを作成する方法を説明しました。 については、ハード コーディングされたパラメーターのソース、パラメーターのソースとしての Web コントロールを使用すると、パラメーター値をプログラムで指定することも説明しました。

ように、ObjectDataSource で SqlDataSource 機能も提供します、基になるデータを変更します。 次のチュートリアルで定義する方法に注目します`INSERT`、 `UPDATE`、および`DELETE`SqlDataSource ステートメント。 これらのステートメントが追加されたら、挿入、編集、および GridView、DetailsView、FormView コントロールに固有の機能を削除する、組み込みを利用できます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Scott クライド、Randell Schmidt、Ken Pespisa でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](querying-data-with-the-sqldatasource-control-cs.md)
> [次へ](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
