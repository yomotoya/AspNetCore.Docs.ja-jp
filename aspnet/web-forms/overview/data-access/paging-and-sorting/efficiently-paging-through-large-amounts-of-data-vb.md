---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: 大量のデータ (VB) を効率的にページング |Microsoft ドキュメント
author: rick-anderson
description: 大量のデータ、その基になるデータ ソース コントロール retriev として使用するときに、データのプレゼンテーション コントロールの既定のページング オプションは適していません.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 00057f9bfd9b1c479e500ac591db694388a5d358
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>効率的に大量のデータ (VB) のページング
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe)または[PDF のダウンロード](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> データのプレゼンテーション コントロールの既定のページング オプションは適していません大量のデータを使用するときに、データのサブセットのみが表示される場合でも、その基になるデータ ソース コントロールをすべてのレコードを取得します。 このような状況で必要がありますにカスタムにページングできます。


## <a name="introduction"></a>はじめに

前のチュートリアルで説明したように 2 つの方法のいずれかのページングを実装できます。

- **既定のページング**ページングを有効にするオプションをチェックするだけで実装できるデータ Web コントロールのスマート タグですただし、データのページを表示しているときに、ObjectDataSource を取得*すべて*であっても、レコードの。ページで、それらのサブセットのみが表示されます
- **カスタム ページング**既定のパフォーマンスが向上にユーザーが要求されたデータの特定のページに表示される必要のあるデータベースからレコードだけを取得することによってページングしかし、カスタム ページングは、もう少しの作業を実装するには既定のページングより

実装だけチェックのチェック ボックスがあり、re 簡単操作のため完了です。 既定のページングは、魅力的なオプションです。 その na ve アプローチのすべてのレコードを取得中では、implausible 場合に、多数の同時ユーザーに十分に大量のデータまたはサイトをページングするとき。 このような状況では、応答性の高いシステムを提供するためにページングするカスタムにする必要があります。

カスタム ページングのチャレンジは、データの特定のページに必要なレコードの正確なセットを返すクエリを記述できることです。 幸いにも、Microsoft SQL Server 2005 が提供 new キーワードの順位付け結果を得るするレコードの適切なサブセットを取得できる効率的にクエリを記述することにより、します。 このチュートリアルではこの新しい SQL Server 2005 キーワードを使用して GridView コントロールのカスタム ページングを実装する方法が表示されます。 カスタム ページングのユーザー インターフェイスと同じである 1 つのページから、[次へ] を使用するステップの既定のページングのカスタム ページングがあります数倍の既定のページングよりも高速です。

> [!NOTE]
> カスタム ページングによって行われる正確なパフォーマンスの向上は、を介してページングされるレコードと、データベース サーバー上に配置されている負荷の合計数によって異なります。 このチュートリアルの最後にカスタム ページング経由で取得したパフォーマンスの利点を示すいくつかの大まかなメトリックに紹介します。


## <a name="step-1-understanding-the-custom-paging-process"></a>手順 1: カスタム ページング プロセスの理解

データのページングとページに表示される正確なレコードは、要求されているデータのページとページあたりに表示されるレコードの数によって異なります。 たとえばを想像 81 の製品を複数のページを 1 ページあたり 10 個の製品を表示します。 最初のページを表示するときに d たい製品 1 ~ 10 です。2 番目のページを表示するときに d お関心がある製品 11 から 20 で。

取得する必要があるレコードと、ページング インターフェイスの表示方法を規定する 3 つの変数です。

- **開始行インデックス**] ページで表示するデータの最初の行のインデックス。 この [インデックスことができますがページ インデックスのページごとに表示するレコードを乗算することと、1 つ追加することによって計算されます。 たとえば、最初のページ (ページ インデックスが 0) の一度にレコード 10 のページング、ときに、開始行インデックスは 0 \* 10 + 1、または 1 です (ページ インデックスは 1) 2 番目のページで、開始行はインデックスが 1 \* 10 + 1。、または 11。
- **最大行数は**ページごとに表示するレコードの最大数。 この変数は、最後のページのページ サイズよりも返されるレコードの数が少なく場合があるため最大行数と呼ばれます。 たとえば、1 ページあたり 10 81 製品レコード間のページング、ときに 9 番目と最後のページは 1 つのレコードがあります。 ページで、ただしが現れない行の最大数の値よりも多くのレコードです。
- **合計レコード数**を介してページングされるレコードの合計数。 この変数は t の特定のページを取得するレコードを決定する必要があるときに、ページング インターフェイスは定められています。 たとえば、を介してページングされる 81 の製品がある場合は、ページング インターフェイスとページング UI に 9 つのページ番号を表示する認識します。

既定のページングと行の開始インデックスは、行の最大数は、ページ サイズでは単に ページのインデックスとページのサイズと、1 つの製品として計算されます。 既定のページングを取得、すべてのレコードからので簡単なタスクの開始行のインデックス行に移動し、それによってデータ、行ごとに、インデックスの任意のページを表示するときに、データベースが呼ばれます。 さらに、レコードの総数はので、すぐに使用できる s DataTable (または、どのオブジェクトがデータベースの結果を保持するために使用されている) のレコードの数だけです。

開始行のインデックスと行の最大数の変数を指定するには、カスタム ページングの実装ではその後の行の開始インデックス位置と、レコード数の最大行数は最大の開始レコードの正確なサブセットを返すこと必要がありますのみです。 カスタム ページングには、2 つの課題が用意されています。

- 効率的に行のインデックスを指定した開始行のインデックスにあるレコードを返すお開始できるようにを介してページングされるデータ全体の行ごとに関連付けることができる必要があります。
- を通じてページングされるレコードの合計数を提供する必要があります。

次の 2 つの手順でこれら 2 つの課題に対処するために必要な SQL スクリプトについて確認します。 SQL スクリプトだけでなくも必要になります、DAL と BLL でメソッドを実装します。

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>手順 2: を介してページングされるレコードの合計数を返す

表示されているページのレコードの正確なサブセットを取得する方法を説明する前に、最初からページングされるレコードの合計数を返す方法を見て s を使用できます。 この情報が正しくページングのユーザー インターフェイスを構成するために必要です。 使用して、特定の SQL クエリによって返されるレコードの合計数を取得できます、 [ `COUNT`集計関数](https://msdn.microsoft.com/library/ms175997.aspx)です。 たとえば、内のレコードの合計数を決定するため、`Products`テーブル、次のクエリを使用できます。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

この情報を返す、DAL にメソッドを追加 s を使用できます。 具体的には、DAL メソッドを作成`TotalNumberOfProducts()`を実行する、`SELECT`上に示したステートメントです。

開いて開始、`Northwind.xsd`に型指定されたデータセット ファイル、`App_Code/DAL`フォルダーです。 次を右クリックし、`ProductsTableAdapter`デザイナーでクエリの追加を選択します。 おとして前のチュートリアルで見てきましたこれにより、DAL に新しいメソッドを追加する、呼び出されると、特定の SQL ステートメントまたはストアド プロシージャを実行します。 同様に、TableAdapter には、前のチュートリアルでのメソッドが、このいずれかのアドホック SQL ステートメントを使用してを選択します。


![アドホック SQL ステートメントを使用します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**図 1**: アドホック SQL ステートメントを使用


次の画面を作成するクエリの種類を指定できます。 このクエリは内のレコードの合計数に 1 つのスカラー値に返すので、`Products`テーブルを選択して、`SELECT`単一の値のオプションが返されます。


![単一の値を返す SELECT ステートメントを使用するクエリを構成します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**図 2**: 単一の値を返す SELECT ステートメントを使用するクエリを構成します。


使用するクエリの種類を示す後に、、次に、クエリを指定お必要があります。


![製品のクエリから SELECT COUNT(*) を使用します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**図 3**: SELECT COUNT を使用して (\*) FROM 製品クエリ


最後に、メソッドの名前を指定します。 ここに挙げた、let s としてを使用して`TotalNumberOfProducts`です。


![DAL メソッド TotalNumberOfProducts を名前します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**図 4**: DAL メソッド TotalNumberOfProducts の名前


[完了] をクリックすると、ウィザードが追加されます、 `TotalNumberOfProducts` DAL にメソッドです。 スカラー返す DAL で返し、null 許容型は、SQL クエリの結果は場合に`NULL`です。 当社`COUNT`、ただし、常に返されます以外`NULL`値; に関係なく、DAL メソッドが null 許容の整数を返します。

DAL メソッドだけでなく、BLL 内のメソッドも必要です。 開く、`ProductsBLL`クラス ファイルを追加、 `TotalNumberOfProducts` DAL s 呼び出すだけメソッド`TotalNumberOfProducts`メソッド。


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

DAL s`TotalNumberOfProducts`メソッドが null 許容の整数を返します。 ただし、おを作成しました、`ProductsBLL`クラス s`TotalNumberOfProducts`メソッドことが標準の整数を返すようにします。 したがってが必要、`ProductsBLL`クラス s`TotalNumberOfProducts`メソッドは、DAL s によって返される null 許容の整数の値の部分を返す`TotalNumberOfProducts`メソッドです。 呼び出し`GetValueOrDefault()`nullable 整数がある場合です。 存在する場合に null 許容の整数の値を返します`null`、ただし、既定の整数値は 0 を返します。

## <a name="step-3-returning-the-precise-subset-of-records"></a>手順 3: レコードの正確なサブセットを返す

DAL と行の開始インデックスを受け入れる BLL でメソッドを作成するが、次のタスクと変数の最大行数の前に説明した適切なレコードを返します。 その前に、let s 最初を見て、必要な SQL スクリプト。 私たちが直面する課題は、行の開始インデックス位置 (およびレコードの最大レコード数に達するまで) の開始には、これらのレコードを返すことができますようにを介してページングされる全体の結果の行ごとにインデックスを効率的に割り当てるには時間ができる必要があります。

行のインデックスとして機能するデータベース テーブルの列は既に場合でもこれは、チャレンジではありません。 一見おと考えることも、`Products`表 s`ProductID`フィールドが十分に機能を最初の製品が`ProductID`1、2、2 番目のようにします。 ただし、商品を削除すると、シーケンスは、このアプローチのため取り消されましたギャップが残されます。

これには 2 つ一般的な方法は、データを複数のページを効率的に行のインデックスを関連付けるために使用できるように取得するレコードの正確なサブセットがあります。

- **SQL Server 2005 の s を使用して`ROW_NUMBER()`キーワード**を初めて使用する SQL Server 2005、`ROW_NUMBER()`キーワードは、一部の順序に基づいて返される各レコードを順位付けを関連付けます。 この順位付けは、各行の行のインデックスとして使用できます。
- **テーブル変数を使用して、 `SET ROWCOUNT`**  SQL Server s [ `SET ROWCOUNT`ステートメント](https://msdn.microsoft.com/library/ms188774.aspx)合計レコードの数です終了する前に、クエリを処理する必要がありますを指定するために使用する。[テーブル変数](http://www.sqlteam.com/item.asp?ItemID=9454)akin を表形式のデータを保持できるローカルの T-SQL 変数[一時テーブル](http://www.sqlteam.com/item.asp?ItemID=2029)です。 このアプローチは均等に Microsoft SQL Server 2005 および SQL Server 2000 の両方 (一方、`ROW_NUMBER()`アプローチは、SQL Server 2005 でのみ機能)。  
  
  持つテーブル変数を作成するという考え方です、`IDENTITY`列とデータを介してページングされるテーブルの主キーの列です。 これにより、連続する行のインデックスを関連付け、テーブル変数にデータを介してページングされるテーブルの内容をダンプする次に、(を使用して、`IDENTITY`列)、テーブル内の各レコード。 テーブル変数の読み込みが完了した後、`SELECT`のステートメントに、テーブル変数で、特定のレコードをプルする基になるテーブルと結合を実行できます。 `SET ROWCOUNT`インテリジェントなテーブル変数にダンプする必要があるレコードの数を制限するステートメントを使用します。  
  
  このアプローチの効率性が要求されているページ数に基づくとして、`SET ROWCOUNT`値には、開始行のインデックスと行の最大数の値が割り当てられます。 データのいくつかのページなど、最初のページの番号付きの低をページングするとき、この方法は非常に効率的です。 ただし、末尾付近のページを取得するときに、既定値のページングのようなパフォーマンスが発生します。

このチュートリアルでは、カスタム ページングを使用して実装する、`ROW_NUMBER()`キーワード。 テーブル変数を使用する方法について、`SET ROWCOUNT`手法を参照してください[A 詳細を効率的に大きな結果セットからページング](http://www.4guysfromrolla.com/webtech/042606-1.shtml)です。

`ROW_NUMBER()`キーワードでは、次の構文を使用して、特定の順序付け経由で返された各レコードにランクが関連付けられています。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` 指定された順序に関しては、各レコードのランクを指定する数値を返します。 たとえば、製品ごとの多い順にランクを表示する、最小限のコストを使用できます、次のクエリ。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

図 5 は、このクエリは、Visual Studio で [クエリ] ウィンドウから実行したときに、秒の結果。 行ごとの価格ランクと共に、価格、製品を並べ替えることに注意してください。


![価格のランクが含まれる各返されるレコード](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**図 5**:、価格のランクがの含まれる各返されるレコード


> [!NOTE]
> `ROW_NUMBER()` 多くの新しい順位付け関数の 1 つは SQL Server 2005 で使用できます。 詳細については`ROW_NUMBER()`、他の順位付け関数と共に読み取り[Microsoft SQL Server 2005 で順位付けされた結果を返す](http://www.4guysfromrolla.com/webtech/010406-1.shtml)です。


指定した結果を順位付けと`ORDER BY`内の列、`OVER`句 (`UnitPrice`、上記の例では)、SQL Server は、結果を並べ替える必要があります。 これは、結果されている順に並べ替え、列にクラスター化インデックスがある場合は、クイック操作または、カバリングがあるかどうか、インデックスでもかまいません高額それ以外の場合。 十分な大きさのクエリのパフォーマンスを向上させるには、これによって、結果は並べ 列の非クラスター化インデックスを追加することを検討してください。 参照してください[SQL Server 2005 でのパフォーマンスと順位付け関数](http://www.sql-server-performance.com/ak_ranking_functions.asp)パフォーマンスに関する考慮事項の詳細を把握します。

によって返されるランク付け情報`ROW_NUMBER()`で直接使用することはできません、`WHERE`句。 返すただし、派生テーブルを使用できます、`ROW_NUMBER()`で表示することができますし、結果、`WHERE`句。 たとえば、次のクエリ テーブルを使用して、派生と共に、ProductName および UnitPrice 列を返す、`ROW_NUMBER()`結果、および、使用、`WHERE`のみこれらの製品価格ランクを返すには句が 11 ~ 20。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

少しさらに、この概念を拡張するには、目的の行の開始インデックスと行の最大数の値を指定されたデータの特定のページを取得するには、このアプローチを利用できます。


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> 紹介するように後でこのチュートリアルでは、 *`StartRowIndex`*によって提供される、ObjectDataSource のインデックスは 0 から始まる一方、 `ROW_NUMBER()` SQL Server 2005 によって返される値は 1 から始まるインデックスします。 そのため、`WHERE`句は、これらのレコードを返します場所`PriceRank`がより厳密に大きい*`StartRowIndex`*以下と等しい*`StartRowIndex`*  + *`MaximumRows`*.


これまで方法を説明してきた`ROW_NUMBER()`できます、開始行のインデックスと行の最大数の値を指定されたデータの特定のページを取得するために使用、今すぐいただくために DAL BLL 内のメソッドとしてこのロジックを実装します。

順序を決める必要がありますこのクエリを作成するときに、結果が順位付けです。%s の名前をアルファベット順で製品を並べ替えることができます。 つまり、このチュートリアルではカスタム ページングの実装はできませんを並べ替えることもできますよりも、カスタム ページ分けしたレポートを作成できません。 チュートリアルでは、[次へ]、ただし、会いしましょうこのような機能の提供方法です。

前のセクションでは、アドホック SQL ステートメントとして DAL メソッドを作成しました。 同様に、TableAdapter ウィザードではありません t によって使用される Visual Studio で T-SQL パーサー残念ながら、`OVER`で使用される構文、`ROW_NUMBER()`関数。 そのため、ストアド プロシージャとしてこの DAL メソッドを作成する必要があります。 [表示] メニュー (またはヒット Ctrl + Alt + S) からサーバー エクスプ ローラーを選択し、展開、`NORTHWND.MDF`ノード。 新しいストアド プロシージャを追加、ストアド プロシージャ ノードを右クリックし、新しいストアド プロシージャの追加を選択 (図 6 を参照してください)。


![ページングとは、製品の新しいストアド プロシージャを追加します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**図 6**: ページングとは、製品の新しいストアド プロシージャを追加


このストアド プロシージャは、2 つの整数入力パラメーターを受け入れる必要があります`@startRowIndex`と`@maximumRows`を使用して、`ROW_NUMBER()`関数が並べ、`ProductName`フィールドに、指定したを超える行だけを返す`@startRowIndex`と未満または等しい`@startRowIndex`  +  `@maximumRow` s。 新しいストアド プロシージャに、次のスクリプトを入力し、データベースにストアド プロシージャを追加する [保存] アイコンをクリックします。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

ストアド プロシージャを作成すると、すぐをテストしてみましょう。右クリックし、`GetProductsPaged`ストアド プロシージャが、サーバー エクスプ ローラーでの名前および Execute オプションを選択します。 Visual Studio は、の入力を求め、入力パラメーター`@startRowIndex`と`@maximumRow`s (図 7 を参照してください)。 別の値を再試行し、結果を確認します。


![値を入力して、@startRowIndexと@maximumRowsパラメーター](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>図 7</strong>: の値を入力して、@startRowIndexと@maximumRowsパラメーター


後のパラメーターの値を入力してこれらの選択、結果を出力ウィンドウが表示されます。 図 8 の両方を 10 に渡すときに結果を示しています、`@startRowIndex`と`@maximumRows`パラメーター。


[![返される、レコードに表示されている 2 番目のデータ ページ](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**図 8**:、レコードに表示されている 2 番目のページ データが返されます ([フルサイズのイメージを表示するをクリックして](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


これで作成されるストアド プロシージャ、おを作成する準備ができたら、`ProductsTableAdapter`メソッドです。 開く、`Northwind.xsd`で右クリックして、型指定されたデータセット、`ProductsTableAdapter`クエリの追加オプションを選択します。 アドホック SQL ステートメントを使用してクエリを作成するには、代わりに、既存のストアド プロシージャを使用してそれを作成します。


![既存のストアド プロシージャを使用して、DAL メソッドを作成します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**図 9**: 既存のストアド プロシージャを使用して、DAL メソッドを作成します。


次に、呼び出すストアド プロシージャを選択するよう求められます。 選択、`GetProductsPaged`ドロップダウン リストからストアド プロシージャです。


![選択、GetProductsPaged ドロップダウン リストからストアド プロシージャ](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**図 10**: GetProductsPaged 選択ドロップダウン リストからストアド プロシージャ


次の画面で、要求したデータの種類は、ストアド プロシージャによって返される: 表形式のデータ、1 つの値または値はありません。 以降、`GetProductsPaged`ストアド プロシージャの複数のレコードが返される、表形式のデータが返されることを示すことができます。


![ストアド プロシージャが表形式のデータを返すことを示します](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**図 11**: ストアド プロシージャが表形式のデータを返すことを示します


最後に、作成するメソッドの名前を指定します。 同様に、前のチュートリアルでは、DataTable、両方の塗りつぶしを使用してメソッドを作成してください DataTable を返します。 最初のメソッドの名前を付けます`FillPaged`、2 番目`GetProductsPaged`です。


![名前のメソッド FillPaged と GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**図 12**: 名前メソッド FillPaged と GetProductsPaged


さらに製品の特定のページを返す DAL メソッドを作成するも必要があります、BLL にこのような機能を提供します。 DAL メソッドと同様に BLL の GetProductsPaged メソッドは、開始行のインデックスと行の最大数を指定するための 2 つの整数入力を受け入れる必要があり、指定した範囲内でそれらのレコードだけを返す必要があります。 だけでの呼び出しを DAL の GetProductsPaged メソッドに次のように ProductsBLL クラスでこのような BLL メソッドを作成します。


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

任意の名前を使用することができます BLL のメソッドの入力パラメーターを使用する選択が、間もなく、見ていきます`startRowIndex`と`maximumRows`us 余分なから保存の作業をこのメソッドを使用して、ObjectDataSource を構成するときにします。

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>手順 4: カスタム ページングを使用して、ObjectDataSource を構成します。

完全なレコードの特定のサブセットにアクセスするための BLL と DAL メソッドでは、カスタム ページングを使用して、基になるレコードからそのページ GridView を作成する準備ができたら制御します。 開いて開始、 `EfficientPaging.aspx`  ページで、`PagingAndSorting`フォルダー、GridView、ページを追加、および新しい ObjectDataSource コントロールを使用するように構成します。 過去のチュートリアルでは、多くの場合がありましたを使用するように構成 ObjectDataSource、`ProductsBLL`クラスの`GetProducts`メソッドです。 このとき、ただし、使用する、`GetProductsPaged`メソッド代わりに、以降、`GetProducts`メソッドを返します。*すべて*データベース内の製品の一方`GetProductsPaged`レコードの特定のサブセットだけを返します。


![ObjectDataSource ProductsBLL クラスの GetProductsPaged メソッドを使用して構成します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**図 13**: ProductsBLL クラスの GetProductsPaged メソッドを使用して、ObjectDataSource を構成します。


お再読み取り専用の GridView を作成するには、以降、insert、UPDATE、メソッドのドロップダウン リストを設定して (なし) にタブを削除します。

次に、ObjectDataSource ウィザードの指示に従って us のソースで、`GetProductsPaged`メソッド s`startRowIndex`と`maximumRows`パラメーターの値を入力します。 これらの入力パラメーターは実際に設定されます GridView によって自動的に、するだけでのままにして、ソースを None におよび完了 をクリックします。


![[なし] として入力パラメーターのソースのままにしてください。](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**図 14**: None として入力パラメーターのソースのままにして


ObjectDataSource ウィザードの完了後は、GridView はの各製品のデータ フィールドのも、BoundField または CheckBoxField に含まれます。 自由に適宜 GridView の外観をカスタマイズできます。 I のみを表示することを選択したら、 `ProductName`、 `CategoryName`、 `SupplierName`、 `QuantityPerUnit`、および`UnitPrice`BoundFields です。 また、構成のスマート タグでページングを有効にする チェック ボックスをチェックしてページングをサポートする GridView。 これらの変更後、GridView と ObjectDataSource 宣言型マークアップを次のようになります。


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

ブラウザー ページにアクセスする場合は、GridView がない where 発見可能にします。


![GridView は表示されません。](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**図 15**: GridView は表示されません


GridView は、ObjectDataSource は両方の値として 0 を現在使用されているので、不足している、 `GetProductsPaged` `startRowIndex`と`maximumRows`パラメーターを入力します。 そのため、レコードを返すことが結果の SQL クエリがありませんされ、したがって GridView は表示されません。

この問題を解決するには、カスタム ページングを使用して、ObjectDataSource を構成する必要があります。 これは、次の手順で実行できます。

1. **集合 ObjectDataSource s`EnablePaging`プロパティを`true`**に渡す必要があります、ObjectDataSource にこれを示します、 `SelectMethod` 2 つのパラメーター: 行の開始インデックスを指定する 1 つ ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx))、および行の最大数を指定する 1 つ ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx))。
2. **ObjectDataSource s を設定`StartRowIndexParameterName`と`MaximumRowsParameterName`プロパティに応じて**、`StartRowIndexParameterName`と`MaximumRowsParameterName`プロパティに渡される入力パラメーターの名前を示す、`SelectMethod`用カスタム ページングします。 これらのパラメーター名は、既定では、`startIndexRow`と`maximumRows`、これは、理由を作成するとき、`GetProductsPaged`メソッド、BLL では入力パラメーターのこれらの値を使用しました。 BLL s のさまざまなパラメーター名を使用する場合`GetProductsPaged`などのメソッド`startIndex`と`maxRows`にする必要がありますの例は、ObjectDataSource s を設定、`StartRowIndexParameterName`と`MaximumRowsParameterName`プロパティに応じて (などの startIndex`StartRowIndexParameterName`およびの maxRows `MaximumRowsParameterName`)。
3. **集合 ObjectDataSource s [ `SelectCountMethod`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx)を合計数のレコードされているページを返すメソッドの名前に (`TotalNumberOfProducts`)**ことに注意してください、`ProductsBLL`クラスの`TotalNumberOfProducts`メソッドを実行する DAL メソッドを使用してページングされるレコードの合計数を返します、`SELECT COUNT(*) FROM Products`クエリ。 この情報には、正しくページング インターフェイスを表示するために、ObjectDataSource が必要です。
4. **削除、`startRowIndex`と`maximumRows` `<asp:Parameter>` ObjectDataSource s 宣言型マークアップから要素**ウィザード、ObjectDataSource を構成するには、Visual Studio に自動的に追加 2`<asp:Parameter>`要素`GetProductsPaged`のメソッドの入力パラメーターです。 設定して`EnablePaging`に`true`、これらのパラメーターが自動的に渡されます;、ObjectDataSource を渡そうとして表示される場合も宣言の構文では、 *4*パラメーターを`GetProductsPaged`メソッド2 つのパラメーターと、`TotalNumberOfProducts`メソッドです。 これらを削除するを忘れた場合`<asp:Parameter>`などのエラー メッセージが表示されます、ブラウザーからページを訪問すると、要素: *ObjectDataSource 'ObjectDataSource1' は、非ジェネリック メソッドの 'TotalNumberOfProducts' を持つを特定できませんでしたパラメーター: startRowIndex、maximumRows*です。

これらの変更を加えたら、ObjectDataSource s の宣言構文は次のようになります。


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

なお、`EnablePaging`と`SelectCountMethod`プロパティが設定されてと`<asp:Parameter>`要素が削除されました。 図 16 は、これらの変更が加えられた後に、[プロパティ] ウィンドウのスクリーン ショットを示します。


![カスタム ページングを使用するのには、ObjectDataSource コントロールを構成します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**図 16**: カスタム ページングを使用するのには、ObjectDataSource コントロールを構成します。


これらの変更を行った後は、ブラウザーからこのページを参照してください。 この一覧を表示すると、10 個の製品が表示されますアルファベット順です。 すぐをデータを 1 ページずつ処理します。 既定のページングとカスタム ページングのエンド ユーザーの観点から visual の違いはありませんが、カスタム ページングをより効率的にページ間大量のデータと、特定のページに表示される必要があるレコードのみを取得します。


[![データ、Ordered、製品名では、ページを使用してのカスタム ページング](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**図 17**: データは、製品名、Ordered はページを使用してのカスタム ページング ([フルサイズのイメージを表示するをクリックして](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> ページ数、ObjectDataSource s によって返される値が、カスタム ページングと`SelectCountMethod`GridView のビュー ステートに格納されています。 その他の GridView 変数、 `PageIndex`、 `EditIndex`、 `SelectedIndex`、`DataKeys`コレクション、およびなに格納されている*状態コントロール*、GridView 秒の値に関係なくこれが永続化される`EnableViewState`プロパティ。 以降、`PageCount`値が永続化される最後のページに移動するリンクを含むページング インターフェイスを使用する場合は、ビュー ステートを使用して、ポストバック間での GridView のビュー ステートを有効にする必要があります。 (ページ、ページング インターフェイスは直接のリンクを最後に含まれていない場合、し、ビュー ステートを無効にすることがあります。)


最後のページのリンクをクリックするとポストバックを発生させるし、GridView を更新するように指示その`PageIndex`プロパティです。 GridView を割り当てます、最後のページ リンクをクリックすると場合、その`PageIndex`プロパティのいずれかの値より小さい、`PageCount`プロパティです。 無効にすると、ビュー ステートに、`PageCount`ポストバック間で値が失われると、`PageIndex`最大の整数値を代わりに割り当てられてです。 GridView が乗算することによって開始行のインデックスを決定しようとした次に、`PageSize`と`PageCount`プロパティです。 これは、結果、`OverflowException`製品が許可されている整数型の最大サイズを超えているためです。

## <a name="implement-custom-paging-and-sorting"></a>カスタム ページングの実装と並べ替え

を介して、データをページングする順序を指定する静的に作成するときに、現在のカスタム ページングの実装が必要です、`GetProductsPaged`ストアド プロシージャです。 ただし、場合がありますメモ GridView s のスマート タグにページングを有効にするオプションだけでなく、並べ替えを有効にするチェックが含まれています。 残念ながら、現在カスタム ページング実装を GridView に並べ替えのサポートを追加しても、データの表示されているページ上のレコードには並べ替えのみです。 たとえばもページングをサポートしの降順に並べ替え、製品名によって、データの最初のページを表示するときに並べ替え GridView を構成する場合は、1 ページ目で、製品の順序を逆は。 71 他の製品の後にあるマーガリン、アルファベット以外を無視する逆のアルファベット順に並べ替えるときに、最初の製品としてマーガリンを示しますこのような図 18 に示す最初のページのレコードだけが、並べ替え中と見なされます。


[![並べ替えのみ、データ、現在のページに表示](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**図 18**: のみ、データ ページに表示される、現在の並べ替え ([フルサイズのイメージを表示するをクリックして](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


並べ替えにのみ適用されますデータの現在のページ、BLL s からデータが取得された後に、並べ替えが行われているため`GetProductsPaged`メソッド、およびこのメソッドは、特定のページのレコードのみを返します。 正しく並べ替えを実装する必要がありますに、並べ替え式を渡す、`GetProductsPaged`メソッドのデータは、データの特定のページを返す前に適切に順位付けされることができるようにします。 これを実現する、次のチュートリアルでこれについて扱います。

## <a name="implementing-custom-paging-and-deleting"></a>カスタム ページングと削除を実装します。

考えることが、最後のページから最後のレコードを削除するときにカスタムのページング手法を使用してデータを含むはページングされた GridView の削除機能を有効にする GridView が表示されなくなります適切にデクリメントではなく GridView の`PageIndex`. このバグを再現するには、だけ先ほど作成したチュートリアルの削除を有効にします。 81 製品、一度に 10 個の製品をページングおために 1 つの製品を表示する必要があります (ページ 9)、最後のページに移動します。 この製品を削除します。

削除すると、最後の製品、GridView*必要があります*8 番目のページに自動的に移動し、このような機能が既定のページングと発生します。 カスタム ページングは、ただし、最後のページの最終製品を削除した後 GridView 単に表示されなくなります画面から完全。 正確な理由*理由*ビットは扱いませんが、このチュートリアルは行われるこれを参照してください[カスタム ページング GridView から最後のページには、最後のレコードを削除する](http://scottonwriting.net/sowblog/posts/7326.aspx)詳細については、低レベルのソースについてこの問題。 要約すると、[削除] ボタンがクリックされたときに、GridView によって実行されるステップの次のシーケンスがあるため s:

1. レコードを削除します。
2. 指定した表示する適切なレコードを取得`PageIndex`と `PageSize`
3. 確認、 `PageIndex` GridView s を減分は、自動的に場合、データ ソース内のデータのページの数を超えない`PageIndex`プロパティ
4. 手順 2. で取得されたレコードを使用した GridView に、適切なページのデータをバインドします。

問題に起因して、ファクトにその手順 2、`PageIndex`が表示するレコードを取得するときに使用、`PageIndex`の最後のページが唯一のレコードが削除されただけです。 そのため、手順 2. で*ありません*データの最終ページには不要になったすべてのレコードが含まれているために、レコードが返されます。 次に、手順 3. で GridView ことを認識するその`PageIndex`プロパティは、データ ソース内のページの合計数より大きい (以降お ve 削除最後のページの最後のレコード) およびデクリメントしたがってその`PageIndex`プロパティです。 手順 4. で GridView がそれ自体を; 手順 2 で取得されたデータにバインドしようただし、ステップ 2 でレコードが返されなかった、したがって結果として得られる空 GridView にします。 この問題のない t サーフェスため既定のページングと手順 2. で*すべて*レコードは、データ ソースから取得されます。

これを修正するのには、2 つのオプションがあります。 GridView s のイベント ハンドラーを作成する 1 つは、`RowDeleted`イベント ハンドラーが削除されるだけで、ページに表示されていたレコードの数を決定します。 かどうか 1 つのレコードが削除されるだけで、レコードが最後の 1 つをされている必要があり、GridView s をデクリメントする必要があります`PageIndex`です。 もちろん、のみ更新する、`PageIndex`ことを確認して判断できます、削除操作が実際に成功した場合、`e.Exception`プロパティは`null`します。

このアプローチは更新するので、`PageIndex`手順 1. の後に、手順 2 の前にします。 したがって、手順 2. でレコードの適切なセットが返されます。 これを実現するには、次のようにコードを使用します。


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

別の回避策は、ObjectDataSource s のイベント ハンドラーを作成する`RowDeleted`イベントを設定して、`AffectedRows`プロパティを 1 の値にします。 GridView の更新手順 1. で (ただし、手順 2. でデータを再取得する前に) レコードを削除すると、その`PageIndex`プロパティの場合は、操作の影響を受けた 1 つまたは複数の行。 ただし、 `AffectedRows` ObjectDataSource によってプロパティが設定されていない、したがって、この手順を省略します。 この手順を実行する方法の 1 つが手動で設定するには、`AffectedRows`プロパティの場合は、削除操作が正常に完了します。 これは、次のようにコードを使用して実行できます。


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

分離コード クラスでこれらのイベント ハンドラーの両方のコードが見つかりません、`EfficientPaging.aspx`例です。

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>既定およびカスタム ページングのパフォーマンスを比較します。

カスタム ページングは、既定のページングを返しますがのみ、必要なレコードを取得するため*すべて*表示されている各ページのレコードのカスタム ページングが既定のページングよりも効率的であるは明白です。 しかし、カスタム ページングは、どのくらい効率的ですか? どのようなパフォーマンスの向上は、カスタム ページングを既定のページングを移動して見なすことができますか。

残念ながら、s がない 1 つのサイズが収まるすべてがここで回答します。 パフォーマンス向上は、さまざまな要因によって異なります、web サーバーとデータベース サーバーの間でデータベース サーバーとの通信チャネルの 2 つのレコードを介してページングされると、負荷の数をされている最も顕著配置されます。 わずか 1 ダースのレコードを含む小さなテーブルは、パフォーマンスの差がごくわずかであり可能性があります。 大きなテーブルの場合は、数千 ~ 数十万行のただし、パフォーマンスの違いは深刻です。

、私のアーティクル[SQL Server 2005 での ASP.NET 2.0 では、カスタム ページング](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)、これら 2 つのページング手法を持つデータベース テーブルをページングするときの間のパフォーマンスの違いを示すために実行したいくつかのパフォーマンス テストが含まれていますレコードが 50,000 個です。 これらのテストで SQL Server レベルでクエリを実行する時間の両方について詳しく説明 (を使用して[SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) と ASP.NET ページを使用して、 [ASP.NET のトレース機能](https://msdn.microsoft.com/library/y13fw6we.aspx)します。 注意してくださいことこれらのテスト 1 つのアクティブなユーザーでは、開発ボックス上で実行されたしたがって科学されない一般的な web サイトのロード パターンを模倣されません。 関係なく、結果は、十分に大量のデータを使用する場合、既定およびカスタム ページングの実行時間の相対的な違いを示しています。


|  | **Avg.期間 (秒)** | **読み取り** |
| --- | --- | --- |
| **既定の SQL Profiler のページング** | 1.411 | 383 |
| **カスタム ページング SQL Profiler** | 0.002 | 29 |
| **ページングの既定の ASP.NET トレース** | 2.379 | *該当なし* |
| **カスタム ページング ASP.NET のトレース** | 0.029 | *該当なし* |


わかるように、データの特定のページを取得する平均読み取り以下 354 を必要し、短時間で完了しました。 ASP.NET ページで、カスタム ページができた 1/100 に近いにレンダーされる<sup>th</sup>時間かかったの既定のページングを使用する場合。 参照してください[私の記事](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)コードと、データベースと共にこれらの結果の詳細については、ご使用の環境でこれらのテストを再現するダウンロードできます。

## <a name="summary"></a>まとめ

既定のページングは、データ Web コントロールのスマート タグにだけチェック ページングを有効にする チェック ボックスを実装する簡単ですが、このようなわかりやすくするための欠点はパフォーマンス。 ユーザーがデータの任意のページを要求したときに既定のページングと*すべて*それらのごく一部だけが表示される場合でも、レコードが返されます。 このパフォーマンスのオーバーヘッドを対抗、ObjectDataSource には、代替ページング オプション カスタム ページングが用意されています。

カスタム ページングを向上させる既定の秒のパフォーマンスの問題を表示する必要があるレコードのみを取得することによってページング中に、s がカスタム ページングを実装するさらに複雑です。 最初に、正しく (かつ効率的に) にアクセス要求レコードの特定のサブセット、クエリを書き込む必要があります。 これには、さまざまな方法です。このチュートリアルで確認して、1 つが新しい SQL Server 2005 の s を使用するには`ROW_NUMBER()`ランク付けする関数の結果、し、返すだけで結果が順位付けが指定された範囲内であります。 さらに、を介してページングされるレコードの合計数を決定するための手段を追加する必要があります。 これらの DAL と BLL メソッドを作成するには後もことを確認し合計レコードの数をポケットベル通知を受け取りますが、BLL に、開始行のインデックスと行の最大数の値を渡すことが正しくできますされるように、ObjectDataSource を構成する必要があります。

カスタム ページングを実装するいくつかの手順は必要し、及びませんなどの単純な既定のページングは、カスタム ページングが必要十分に大量のデータをページングするとき。 結果の調査し、表示された、カスタム ページング役立つかについて、ASP.NET ページのレンダリング時間を秒数が 1 つ以上大幅データベース サーバーの負荷を軽減することができます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](paging-and-sorting-report-data-vb.md)
> [次へ](sorting-custom-paged-data-vb.md)
