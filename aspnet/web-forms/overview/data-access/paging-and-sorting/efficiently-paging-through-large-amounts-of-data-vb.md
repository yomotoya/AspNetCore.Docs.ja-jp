---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: 大量のデータ (VB) を効率的にページング |Microsoft Docs
author: rick-anderson
description: データ プレゼンテーション コントロールの既定ページング オプションは、大量のデータは、その基になるデータ ソース コントロール retriev として使用する場合に適していません.
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b00e18287bdb791a353b7ebd1bbb6cc0ab586b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805507"
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>効率的にページングする大量のデータ (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe)または[PDF のダウンロード](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> データ プレゼンテーション コントロールの既定のページング オプションは適していません大量のデータを使用する場合、データのサブセットのみが表示される場合でも、その基になるデータ ソース コントロールが、すべてのレコードを取得します。 このような状況では、する必要がありますにカスタムにページングします。


## <a name="introduction"></a>はじめに

上記のチュートリアルで説明したように、2 つの方法のいずれかでページングを実装できます。

- **既定のページング**ページングを有効にするオプションをチェックするだけで実装できるデータ Web コントロールのスマート タグただし、データのページを表示するたびに、ObjectDataSource が取得します。*すべて*であっても、レコードの。ただし ページで、それらのサブセットのみが表示されます。
- **カスタム ページング**既定のパフォーマンスが向上、ユーザーから要求されたデータの特定のページに表示する必要のあるデータベースからレコードのみを取得することによってページングしかし、カスタム ページングは、少しの労力を実装するには既定のページングより

実装だけチェックのチェック ボックスがあり、re 容易さが原因で終了です。 既定のページングは、魅力的なオプションです。 その na ve アプローチのすべてのレコードを取得する際では、ことは信じがたいの選択した多数の同時ユーザーに十分な大きさの金額またはサイトのデータをページングするとき。 このような状況では、カスタムに応答性の高いシステムを提供するためにページングにする必要があります。

カスタム ページングの課題は、正確なデータの特定のページに必要なレコード セットを返すクエリを記述できることです。 さいわい、Microsoft SQL Server 2005 を提供します新しいキーワードの順位付けの結果をレコードの適切なサブセットを取得できる効率的にクエリを記述できます。 このチュートリアルでは、この新しい SQL Server 2005 のキーワードを使用して、GridView コントロールにカスタム ページングを実装する方法がわかります。 カスタム ページングのユーザー インターフェイスが既定のページング、1 つのページから、[次へ] を使用するステップ インと同じですが、カスタム ページングでは既定のページングよりも高速の桁数を指定できます。

> [!NOTE]
> カスタム ページングが発生した正確なパフォーマンスの向上は、レコードをポケットベル通知とデータベース サーバー上に配置されている負荷の合計数によって異なります。 このチュートリアルの最後には、カスタム ページング経由で取得したパフォーマンスの利点を紹介するいくつかの大まかなメトリックも取り上げます。


## <a name="step-1-understanding-the-custom-paging-process"></a>手順 1: カスタム ページング プロセスを理解します。

データのページング、ときに、ページに表示される正確なレコードは、要求されているデータのページとページあたりに表示されるレコードの数によって異なります。 たとえば、ことたい 81 の製品内でページを 1 ページあたり 10 個の製品を表示するとします。 最初のページを表示するときに d する製品の 1 ~ 10 です。2 番目のページを表示するときに d 予定 11 から 20 の製品で対象です。

取得する必要があるレコードとページング インターフェイスを表示する方法を指定する 3 つの変数です。

- **開始行インデックス** ページで表示するデータの最初の行のインデックスはページのインデックスを 1 ページに表示するレコードに乗算と 1 つを追加することによって計算このインデックスを作成できます。 たとえば、最初のページ (ページ インデックスが 0) を一度にレコード 10 のページングしているとき、に、開始行インデックスは 0 \* 10 + 1、または 1; 2 番目のページ (ページ インデックスは 1) の行の開始インデックスが 1 \* 10 + 1、または 11。
- **最大行数は**1 ページに表示するレコードの最大数。 この変数は、最後のページのページ サイズよりも返されるレコードの少ない場合があるため最大行数として呼ばれます。 たとえば、1 ページあたり 10 81 の製品レコード間のページング、ときに、9 番目と最後のページは 1 つのレコードがあります。 ページで、ただしは表示されません行の最大値よりも多くのレコード。
- **合計レコード数**を介してページングされるレコードの合計数。 この変数は t の特定のページを取得するレコードを決定する必要があるときに、ページング インターフェイスによって決まります。 たとえば、ポケットベル通知を 81 の製品がある場合は、ページング UI に 9 つのページ番号を表示するページング インターフェイスが認識しています。

既定のページングを使用した開始行インデックス行の最大数はページ サイズでは単にページのインデックスとページ サイズを 1 つの積として計算されます。 既定のページングには、すべてからレコードを取得するためことにより、簡単な作業の開始行インデックス行への移行データを行ごとにインデックスの任意のページを表示するときに、データベースが呼ばれます。 さらに、レコードの総数はので、すぐに使用できる s DataTable (または任意のオブジェクトがデータベースの結果を保持するために使用されている) 内のレコードの数だけです。

開始行のインデックスと行の最大数の変数を指定するには、カスタム ページングの実装ではその後の行の開始インデックス位置にあると、レコード数の最大行数は最大の開始レコードの正確なサブセットを返すこと必要がありますのみです。 カスタム ページングでは、2 つの課題を提供します。

- 行のインデックスに各行を指定した開始行インデックス位置にあるレコードを返すを開始するためにページングされるデータ全体で効率的に関連付けることができる必要があります。
- ポケットベル通知でレコードの合計数を提供する必要があります。

次の 2 つの手順では、これら 2 つの課題に応答するために必要な SQL スクリプトを考察します。 SQL スクリプトだけでなくも DAL と BLL にメソッドを実装する必要あります。

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>手順 2: ポケットベル通知でレコードの合計数を返す

表示されているページのレコードの正確なサブセットを取得する方法を調べる前に、最初からページングされるレコードの合計数を返す方法を見て s を使用できます。 この情報は、ページングのユーザー インターフェイスを適切に構成するために必要です。 使用して、特定の SQL クエリによって返されるレコードの合計数を取得できます、 [ `COUNT`集計関数](https://msdn.microsoft.com/library/ms175997.aspx)します。 たとえば、内のレコードの合計数を決定するため、`Products`テーブル、次のクエリを使用できます。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

この情報を返す、DAL にメソッドを追加して使用できます。 具体的には、DAL メソッドを作成します`TotalNumberOfProducts()`を実行する、`SELECT`前に示したステートメント。

開いて開始、`Northwind.xsd`でデータセットの型指定されたファイル、`App_Code/DAL`フォルダー。 次に、右クリックし、`ProductsTableAdapter`デザイナーでクエリの追加を選択します。 として前のチュートリアルで確認したところこれは、DAL に新しいメソッドを追加することにより、呼び出されると、特定の SQL ステートメントまたはストアド プロシージャを実行します。 同様に、TableAdapter には、前のチュートリアルでのメソッドが、このいずれかのアドホック SQL ステートメントを使用するを選択します。


![アドホック SQL ステートメントを使用します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**図 1**: アドホック SQL ステートメントを使用


次の画面を作成するクエリの種類を指定できます。 このクエリは内のレコードの合計数に 1 つのスカラー値に返すため、`Products`テーブルの選択、`SELECT`単一の値のオプションが返されます。


![1 つの値を返す SELECT ステートメントを使用するクエリを構成します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**図 2**: 単一の値を返す SELECT ステートメントを使用するクエリの構成


使用するクエリの種類を示す後、は、次に、クエリを指定します必要があります。


![製品のクエリから選択 COUNT(*) を使用します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**図 3**: SELECT Count (\*) FROM 製品クエリ


最後に、メソッドの名前を指定します。 使用して、前述の s として`TotalNumberOfProducts`します。


![DAL メソッド TotalNumberOfProducts を名前します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**図 4**: DAL メソッド TotalNumberOfProducts の名前


[完了] をクリックした後、ウィザードが追加されます、 `TotalNumberOfProducts` DAL に対するメソッド。 場合に、SQL クエリの結果、DAL で返すスカラーのメソッドが null 許容型を返す`NULL`します。 この`COUNT`、ただし、常に返されます以外`NULL`値; に関係なく、DAL メソッドが null 許容の整数を返します。

DAL メソッドだけでなく、BLL のメソッドも必要です。 開く、`ProductsBLL`クラス ファイルを追加、 `TotalNumberOfProducts` DAL s 単に呼び出すメソッドを`TotalNumberOfProducts`メソッド。


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

DAL s`TotalNumberOfProducts`メソッドは null 許容の整数を返します。 ただし、私たちを作成しました、`ProductsBLL`クラス s`TotalNumberOfProducts`メソッドの標準的な整数を返すようにします。 そのため、私たちがする必要があります、`ProductsBLL`クラス s`TotalNumberOfProducts`メソッドは、DAL s によって返される null 許容の整数の値の部分を返す`TotalNumberOfProducts`メソッド。 呼び出し`GetValueOrDefault()`場合は null 許容の整数が存在する場合、null 許容の整数の値を返します`null`、ただし、既定の整数値は 0 を返します。

## <a name="step-3-returning-the-precise-subset-of-records"></a>手順 3: レコードの正確なサブセットを返す

DAL で BLL 開始行インデックスをそのまま使用するメソッドを作成する、次のタスクでは、最大行数は変数の前に説明したし、適切なレコードが返されます。 その前に let s は、最初に必要な SQL スクリプトを見ています。 私たちが直面する課題は、行の開始インデックス位置にある (およびレコードの最大レコード数まで) を開始するには、これらのレコードを返すことができるように、を介してページングされる全体の結果内の各行にインデックスを効率的に割り当てるにさせていただく必要があります。

行のインデックスとして機能するデータベース テーブルの列は既に場合でもこれは、チャレンジではありません。 一見考えることも、`Products`テーブル s`ProductID`フィールドはで十分な最初の製品が`ProductID`1、2、2 番目のなど。 ただし、商品を削除するには、このアプローチを戻して、シーケンスは、ギャップが残されます。

2 つ一般的な手法、改ページするデータを効率的に行のインデックスを関連付けるために使用できるように取得するレコードの正確なサブセットがあります。

- **SQL Server 2005 の s を使用して`ROW_NUMBER()`キーワード**から SQL Server 2005 では、新しい、`ROW_NUMBER()`キーワードは、何らかの順序に基づいて返される各レコードを順位付けを関連付けます。 この順位付けは、各行の行のインデックスとして使用できます。
- **テーブル変数を使用し、 `SET ROWCOUNT`**  SQL server [ `SET ROWCOUNT`ステートメント](https://msdn.microsoft.com/library/ms188774.aspx)数の合計レコード終了する前にクエリを処理する必要がありますを指定するために使用できます。[テーブル変数](http://www.sqlteam.com/item.asp?ItemID=9454)akin を表形式のデータを保持できるローカルの T-SQL 変数[一時テーブル](http://www.sqlteam.com/item.asp?ItemID=2029)します。 このアプローチは均等に Microsoft SQL Server 2005 および SQL Server 2000 の両方 (一方、`ROW_NUMBER()`アプローチは、SQL Server 2005 でのみ動作)。  
  
  持つテーブル変数を作成するという考え方です、`IDENTITY`列とデータを介してページングされるテーブルの主キーの列。 テーブル変数は、連続する行のインデックスを関連付けるために、データを介してページングされるテーブルの内容をダンプする次に、(を使用して、`IDENTITY`列)、テーブル内の各レコード。 テーブル変数を設定すると後、`SELECT`のステートメントでテーブル変数では、特定のレコードを取得する基になるテーブルと結合してを実行できます。 `SET ROWCOUNT`インテリジェントにテーブル変数にダンプする必要があるレコードの数を制限するステートメントを使用します。  
  
  このアプローチの効率性が要求されているページ数に基づいてとして、`SET ROWCOUNT`値には、開始行インデックスと行の最大数の値が割り当てられます。 最初のなどのページの最小番号をいくつかのページのデータをページングするとき、この方法は非常に効率的です。 ただし、末尾付近のページを取得するときに、既定のページングのようなパフォーマンスが発生します。

このチュートリアルでは、カスタム ページングを使用して実装する、`ROW_NUMBER()`キーワード。 テーブル変数を使用しての詳細については、`SET ROWCOUNT`方法を参照してください[A 詳細の効率的な方法の大きな結果セットからページング](http://www.4guysfromrolla.com/webtech/042606-1.shtml)します。

`ROW_NUMBER()`キーワードでは、次の構文を使用して特定の順序で返された各レコードに順位付けが関連付けられています。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` 指定された順序に関しては、各レコードのランクを指定する数値を返します。 たとえば、製品ごとの多い順にランクを表示する、最小コストの高い使えば、次のクエリ。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

図 5 は、このクエリに Visual Studio で [クエリ] ウィンドウから実行したときに、秒の結果を示します。 製品は行ごとの価格のランクとの価格で並べ替えられますことに注意してください。


![価格のランクが含まれている各返されるレコード](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**図 5**: の 価格のランクが含まれている各返されるレコード


> [!NOTE]
> `ROW_NUMBER()` 多くの新しい順位付け関数の 1 つは SQL Server 2005 で使用できます。 詳細については詳しい`ROW_NUMBER()`、読み取り、その他の順位付け関数と共に[Microsoft SQL Server 2005 でランク付けされた結果を返す](http://www.4guysfromrolla.com/webtech/010406-1.shtml)します。


指定した結果を順位付けと`ORDER BY`内の列、`OVER`句 (`UnitPrice`、上の例では)、SQL Server は、結果を並べ替える必要があります。 これは、結果には、順序付けられる列経由でクラスター化インデックスがある場合は、クイック操作またはカバリングがあるかどうか、インデックスしますが、それ以外の場合コストが高くすることができます。 十分な大きさのクエリのパフォーマンスを向上させるで使用される結果が順序付けられた列の非クラスター化インデックスを追加することを検討してください。 参照してください[順位付け関数と SQL Server 2005 のパフォーマンス](http://www.sql-server-performance.com/ak_ranking_functions.asp)用にパフォーマンスに関する考慮事項について詳しく説明します。

によって返されるランク付け情報`ROW_NUMBER()`で直接使用することはできません、`WHERE`句。 派生テーブルを使用して、返される、`ROW_NUMBER()`で表示できますし、結果、`WHERE`句。 次のクエリは派生テーブルを使用して、と共に、ProductName および [単価] 列を返すなど、`ROW_NUMBER()`結果、および、使用、`WHERE`句をのみこれらの製品価格ランクを返すの 11 ~ 20。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

少しさらに、この概念を拡張するには、目的の行の開始インデックスと行の最大数の値を指定されたデータの特定のページを取得するには、この方法を利用できます。


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> このチュートリアルで後でわかりますが、 *`StartRowIndex`* によって提供される ObjectDataSource のインデックスは、0 から始まる一方、 `ROW_NUMBER()` SQL Server 2005 によって返される値は 1 から始まるインデックスします。 そのため、`WHERE`句は、これらのレコードを返します場所`PriceRank`より厳密に大きい*`StartRowIndex`* 以下と等しい、 *`StartRowIndex`*  + *`MaximumRows`*.


これまで方法を説明してきた`ROW_NUMBER()`できる、開始行インデックスと行の最大数の値を指定されたデータの特定のページを取得するため、今すぐ必要があります DAL BLL 内のメソッドとしてこのロジックを実装します。

順序を決定しなければ、このクエリを作成するときに、結果が順位付けします。s、製品の名前がアルファベット順で並べ替えることができます。 つまり、このチュートリアルではカスタム ページングの実装ではできませんを並べ替えることができますもよりも、カスタム ページ分けしたレポートを作成できません。 次のチュートリアルでは、見ていく方法、このような機能を提供できます。

前のセクションでは、アドホック SQL ステートメントとして DAL メソッドを作成しました。 残念ながら、T-SQL パーサーのように TableAdapter ウィザードは t によって使用される Visual Studio で、`OVER`で使用される構文、`ROW_NUMBER()`関数。 そのため、ストアド プロシージャとしてこの DAL メソッドを作成する必要があります。 [表示] メニュー (またはヒット Ctrl + Alt + S) からサーバー エクスプ ローラーを選択し、展開、`NORTHWND.MDF`ノード。 新しいストアド プロシージャを追加するストアド プロシージャ ノードを右クリックし、新しいストアド プロシージャの追加を選択 (図 6 参照)。


![新しいストアド プロシージャを製品を使用するページングの追加します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**図 6**: 製品のページングの新しいストアド プロシージャを追加


このストアド プロシージャは、2 つの整数の入力パラメーター - を受け入れる必要があります`@startRowIndex`と`@maximumRows`を使用して、`ROW_NUMBER()`関数に並べ、`ProductName`フィールドに、指定したより大きい行のみを返す`@startRowIndex`と未満または等しい`@startRowIndex`  +  `@maximumRow`秒。 新しいストアド プロシージャに次のスクリプトを入力し、データベースにストアド プロシージャを追加する [保存] アイコンをクリックします。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

ストアド プロシージャを作成すると、少しテストしてみましょう。右クリックし、`GetProductsPaged`ストアド プロシージャのサーバー エクスプ ローラーでの名前および Execute オプションを選択します。 Visual Studio から求められます、入力パラメーターの`@startRowIndex`と`@maximumRow`s (図 7 を参照してください)。 値を変化し、結果を確認します。


![値を入力、@startRowIndexと@maximumRowsパラメーター](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>図 7</strong>: の値を入力、@startRowIndexと@maximumRowsパラメーター


後のパラメーターの値を入力してこれらの選択、結果を出力ウィンドウが表示されます。 図 8 は、10 の両方を渡すときに結果を示しています、`@startRowIndex`と`@maximumRows`パラメーター。


[![レコードに含まれる 2 番目のページ データが返されます](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**図 8**:、レコードに含まれる 2 番目のページ データが返されます ([フルサイズの画像を表示する をクリックします](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))。


これで作成されるストアド プロシージャを作成する準備ができたら、`ProductsTableAdapter`メソッド。 開く、`Northwind.xsd`で右クリックして、型指定されたデータセット、`ProductsTableAdapter`クエリの追加オプションを選択します。 アドホック SQL ステートメントを使用してクエリを作成する代わりには、既存のストアド プロシージャを使用してそれを作成します。


![既存のストアド プロシージャを使用して、DAL メソッドの作成します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**図 9**: 既存のストアド プロシージャを使用して、DAL メソッドの作成


次に、呼び出すストアド プロシージャを選択するように求められます。 選択、`GetProductsPaged`ストアド プロシージャをドロップダウン リストから。


![選択、GetProductsPaged ストアド プロシージャをドロップダウン リストから](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**図 10**: 選択、GetProductsPaged ストアド プロシージャをドロップダウン リストから


次の画面で、要求するデータの種類は、ストアド プロシージャによって返される: 表形式のデータ、1 つの値または値はありません。 以降、`GetProductsPaged`ストアド プロシージャが複数のレコードが返される、表形式のデータを返すことを示します。


![ストアド プロシージャが表形式のデータを返すことを示します](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**図 11**: ストアド プロシージャが表形式のデータを返すことを示します


最後に、作成するメソッドの名前を指定します。 前のチュートリアルとしてください DataTable を返すメソッドの両方の塗りつぶしを使用して DataTable を作成します。 最初のメソッドの名前を付けます`FillPaged`、もう 1 つ`GetProductsPaged`します。


![名前のメソッド FillPaged と GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**図 12**: 名メソッド FillPaged と GetProductsPaged


さらに、製品の特定のページを返す DAL メソッドを作成する必要もあります BLL にこのような機能を提供します。 DAL のメソッドと同様 BLL の GetProductsPaged メソッドは、開始行インデックスと行の最大数を指定するための 2 つの整数入力を受け入れる必要があり、指定した範囲内にあるレコードだけを返す必要があります。 DAL の GetProductsPaged メソッドにダウン呼び出しだけでそのように ProductsBLL クラスで BLL のメソッドを作成します。


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

任意の名前を使用する BLL メソッドの入力パラメーターは、使用する選択が、まもなく、わかります`startRowIndex`と`maximumRows`余分なから起きなくのこのメソッドを使用して、ObjectDataSource を構成するときに作業します。

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>手順 4: カスタム ページングを使用して ObjectDataSource を構成します。

完全なレコードの特定のサブセットにアクセスするための BLL と DAL メソッドを使ってカスタム ページングを使用して、基になるレコードからそのページを制御 GridView を作成する準備が整いました。 開いて開始、`EfficientPaging.aspx`ページで、`PagingAndSorting`フォルダーは、ページに GridView を追加し、新しい ObjectDataSource コントロールを使用するように構成します。 過去のチュートリアルでは、多くの場合がありました ObjectDataSource を使用するように構成、`ProductsBLL`クラスの`GetProducts`メソッド。 今回は、ただし、使用する、`GetProductsPaged`メソッド代わりに、以降、`GetProducts`メソッドを返します。*すべて*データベースでは、製品の一方`GetProductsPaged`レコードの特定のサブセットだけを返します。


![ObjectDataSource ProductsBLL クラスの GetProductsPaged メソッドを使用して構成します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**図 13**: ProductsBLL クラスの GetProductsPaged メソッドを使用して ObjectDataSource を構成します。


以降再読み取り専用の GridView を作成するには、INSERT、UPDATE でメソッドのドロップダウン リストを設定する少しし、(None) にタブを削除します。

次のソースの ObjectDataSource ウィザード要求私たち、`GetProductsPaged`メソッド s`startRowIndex`と`maximumRows`パラメーターの値を入力します。 これらの入力パラメーターは、GridView で、自動的には設定実際には、単純に [なし] に、元のまま、[完了] をクリックします。


![入力パラメーターのソースを None のままにします](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**図 14**: 入力パラメーターのソースを None のままにします


ObjectDataSource ウィザードの完了後は、GridView は各製品のデータ フィールドのも、BoundField または CheckBoxField に含まれます。 自由に、必要に応じて、GridView の外観を調整できます。 のみを表示することを選択したら、 `ProductName`、 `CategoryName`、 `SupplierName`、 `QuantityPerUnit`、および`UnitPrice`BoundFields します。 また、そのスマート タグでページングを有効にするチェック ボックスをオンにページングをサポートするために、GridView を構成します。 これらの変更後に GridView コントロールと ObjectDataSource 宣言型マークアップを次のようになります。


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

ブラウザーを使用してページを閲覧する場合、GridView はありません場所を検索します。


![GridView は表示されません。](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**図 15**: GridView は表示されません


ObjectDataSource は両方の値として 0 を現在使用されているので、GridView が不足しているが、 `GetProductsPaged` `startRowIndex`と`maximumRows`パラメーターを入力します。 そのため、SQL クエリの結果は、レコードを返さないされ、したがって、GridView は表示されません。

これを解決するには、カスタム ページングを使用して ObjectDataSource を構成する必要があります。 これは、次の手順で実行できます。

1. **ObjectDataSource s 設定`EnablePaging`プロパティを`true`** に渡す必要があります、ObjectDataSource にこれを示します、 `SelectMethod` 2 つのパラメーター: 開始行インデックスを指定する 1 つ ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx))、行の最大数を指定する 1 つ ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx))。
2. **ObjectDataSource s を設定`StartRowIndexParameterName`と`MaximumRowsParameterName`プロパティに応じて**、`StartRowIndexParameterName`と`MaximumRowsParameterName`プロパティに渡される入力パラメーターの名前を示すため、`SelectMethod`用カスタム ページングします。 これらのパラメーター名では既定では、`startIndexRow`と`maximumRows`、これは、理由を作成するとき、`GetProductsPaged`メソッドで BLL は、入力パラメーターのこれらの値を使用しました。 BLL s のさまざまなパラメーター名を使用する場合`GetProductsPaged`などのメソッド`startIndex`と`maxRows`しなければならなくなります例 ObjectDataSource s の設定、`StartRowIndexParameterName`と`MaximumRowsParameterName`プロパティに応じて (などの startIndex`StartRowIndexParameterName`およびの maxRows `MaximumRowsParameterName`)。
3. **ObjectDataSource s 設定[`SelectCountMethod`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx)を合計数のレコードされているページを返すメソッドの名前に (`TotalNumberOfProducts`)** を思い出してください、`ProductsBLL`クラスの`TotalNumberOfProducts`メソッドを実行する DAL メソッドを使用して、ページングされるレコードの合計数を返します、`SELECT COUNT(*) FROM Products`クエリ。 この情報は、ObjectDataSource 正しくページング インターフェイスを表示するために必要です。
4. **削除、`startRowIndex`と`maximumRows` `<asp:Parameter>` ObjectDataSource s 宣言型マークアップから要素**ウィザードで ObjectDataSource を構成するときに Visual Studio に自動的に追加されました 2 つ`<asp:Parameter>`要素`GetProductsPaged`メソッドのパラメーターを入力します。 設定して`EnablePaging`に`true`、これらのパラメーターが自動的に渡される; ObjectDataSource は渡すしようとしても宣言構文内で現れる場合*4*パラメーターを`GetProductsPaged`メソッド2 つのパラメーターと、`TotalNumberOfProducts`メソッド。 これらを削除するを忘れた場合`<asp:Parameter>`などのエラー メッセージが届きますブラウザーを使用してページにアクセスすると、要素: *ObjectDataSource 'ObjectDataSource1' は、非ジェネリック メソッドの 'TotalNumberOfProducts' を持つを特定できませんでしたパラメーター: startRowIndex、maximumRows*します。

これらの変更を行った後、次のよう ObjectDataSource s の宣言型構文になります。


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

なお、`EnablePaging`と`SelectCountMethod`プロパティが設定されていると、`<asp:Parameter>`要素が削除されました。 図 16 は、これらの変更が行われた後に、[プロパティ] ウィンドウのスクリーン ショットを示します。


![カスタム ページングを使用するには、ObjectDataSource コントロールを構成します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**図 16**: カスタム ページングを使用するには、ObjectDataSource コントロールを構成します。


これらの変更を行った後は、ブラウザーからこのページを参照してください。 表示されている場合、10 個の製品が表示アルファベット順でします。 一度にデータの 1 つのページをステップに時間がかかります。 エンドユーザーの観点から既定のページングとカスタム ページングの視覚的な違いはありませんが、カスタム ページングをより効率的のページを大量データのように、特定のページに表示する必要があるレコードのみを取得します。


[![製品名、Ordered、データを使用してカスタム ページングのページは、します。](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**図 17**: データ、製品名、Ordered、ページングを使用してのカスタム ページング ([フルサイズの画像を表示する をクリックします](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))。


> [!NOTE]
> カスタムのページングを使用したページは ObjectDataSource s によって返される値をカウント`SelectCountMethod`GridView のビュー ステートに格納されます。 他の変数の GridView、 `PageIndex`、 `EditIndex`、 `SelectedIndex`、`DataKeys`コレクション、および具合に格納されている*コントロールの状態*、GridView 秒の値に関係なく保持する`EnableViewState`プロパティ。 `PageCount`値が永続化される最後のページに移動するリンクを含むページング インターフェイスを使用する場合は、ビュー ステートを使用して、ポストバック間で、GridView ビュー状態が有効にすることが重要です。 (ページ、ページング インターフェイスは直接リンクを最後に含まれていない場合、ビュー ステートが無効にできます。)


最後のページ リンクをクリックすると、ポストバックが発生して、GridView を更新するように指示しますその`PageIndex`プロパティ。 GridView の割り当ての最後のページ リンクをクリックすると、その`PageIndex`プロパティをいずれかの値より小さい、`PageCount`プロパティ。 無効にすると、ビュー ステート、`PageCount`ポストバック間で値が失われることと`PageIndex`代わりに最大の整数値が代入されます。 GridView が乗算することによって開始行インデックスを決定しようとした次に、`PageSize`と`PageCount`プロパティ。 これは、結果、`OverflowException`のため、製品が許可されている整数値の最大サイズを超えています。

## <a name="implement-custom-paging-and-sorting"></a>カスタム ページングを実装および並べ替え

現在のカスタム ページングの実装では、を通じて、データをページングする順序が、作成するときに、静的に指定する必要があります、`GetProductsPaged`ストアド プロシージャ。 ただし、GridView s のスマート タグがページングを有効にするオプションだけでなくで並べ替えを有効にするのチェック ボックスが含まれていることに注意が可能性があります。 残念ながら、現在カスタム ページング実装を GridView に並べ替えのサポートを追加しても、データの現在表示されているページのレコードには並べ替えのみです。 などもページングをサポートし、製品名の降順で、データの最初のページを表示するときに並べ替え、GridView を構成する場合は、ページ 1 で、製品の順序が反転されます。 無視、71 他の製品がアルファベット順に; マーガリン後に、逆のアルファベット順の順序で並べ替えるときに、最初の製品としてマーガリンを示しますこのような図 18 に示すよう最初のページのレコードだけが、並べ替え中と見なされます。


[![並べ替えられた、データにのみ表示されます、現在のページ](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**図 18**: のみ、データ ページに表示される、現在の並べ替え ([フルサイズの画像を表示する をクリックします](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))。


並べ替えのみデータの現在のページに適用されます、BLL s から、データが取得された後、並べ替えが行われているため、`GetProductsPaged`メソッド、およびこのメソッドは、特定のページのレコードを返しますのみです。 正しく並べ替えを実装する必要がある並べ替え式を渡す、`GetProductsPaged`メソッドのデータの特定のページを返す前に、データを適切に付けられるようにします。 これで、次のチュートリアルを実行する方法を見ていきます。

## <a name="implementing-custom-paging-and-deleting"></a>カスタム ページングと削除を実装します。

最後のページから最後のレコードを削除するときを検索、カスタムのページング手法を使用してデータをページング GridView で削除する機能を有効にする GridView が表示されなくなります適切にデクリメントする操作ではなく GridView の`PageIndex`. このバグを再現するには、同様に先ほど作成したチュートリアルでは、削除を有効にします。 81 製品、一度に 10 個の製品をページングしますので、1 つの製品を表示する必要があります (9 ページ) の最後のページに移動します。 この製品を削除します。

最後の製品では、GridView を削除すると*は*8 番目のページに自動的に移動し、既定のページングを使用したこのような機能が発生しました。 カスタムのページングを使用したただし、最後のページでその最終製品を削除した後、GridView から消失する画面完全。 正確な理由*なぜ*これは、このチュートリアルの範囲を超えては少し; を参照してください[カスタム ページングを GridView から最後のページで、最後のレコードを削除する](http://scottonwriting.net/sowblog/posts/7326.aspx)ソースの低レベルの詳細この問題。 要約すると、次の一連の削除 ボタンがクリックされたときに、GridView が行う手順のための s:

1. レコードを削除します。
2. 指定した表示する適切なレコードを取得する`PageIndex`と `PageSize`
3. 確認、 `PageIndex` ; データ ソース内のデータのページ数を超えていない場合は、GridView s を減分は、自動的に`PageIndex`プロパティ
4. データの適切なページを手順 2. で取得したレコードを使用して GridView にバインドします。

そので手順 2、という事実から問題の原因、`PageIndex`は引き続き表示するレコードを取得するときに使用、`PageIndex`の最後のページが唯一のレコードが削除されただけです。 そのため、手順 2. で*ありません*データの最終ページには任意のレコードが含まれていないために、レコードが返されます。 次に、手順 3 で、GridView は実現しています、`PageIndex`プロパティがデータ ソース内のページの総数よりも大きい (経過したら削除最後のページの最後のレコード) をデクリメントしますのでその`PageIndex`プロパティ。 手順 4. で、GridView が自体を手順 2.; で取得されたデータにバインドしようただし、レコードが返されなかった手順 2、したがって結果として空の GridView。 この問題は t サーフェスため、既定のページングを使用した手順 2. で*すべて*レコードがデータ ソースから取得されます。

この問題を解決するのには、2 つがあります。 1 つは、GridView s のイベント ハンドラーを作成する`RowDeleted`だけ削除されたページに表示されていたレコードの数を決定するイベント ハンドラー。 かどうか 1 つのレコードが削除されるだけでレコードが最後の 1 つをされている必要があるし、GridView s をデクリメントする必要があります`PageIndex`します。 もちろん、のみ更新する、`PageIndex`ことで決定削除操作が実際に成功した場合、`e.Exception`プロパティは`null`します。

このアプローチは、更新されるため、`PageIndex`手順 1 の後、手順 2. の前にします。 そのため、手順 2 で、適切なレコード セットが返されます。 これを実現するには、次のようなコードを使用します。


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

別の回避策は、ObjectDataSource s のイベント ハンドラーを作成する`RowDeleted`イベントを設定して、`AffectedRows`プロパティ値の 1 にします。 GridView の更新手順 1. で (ただし、手順 2 でデータを再取得する前に) レコードを削除すると、その`PageIndex`プロパティの場合は、操作の影響を受けた 1 つまたは複数の行。 ただし、 `AffectedRows` ObjectDataSource でプロパティが設定されていないと、そのためこの手順を省略するとします。 この手順を実行する方法の 1 つが手動で設定するには、`AffectedRows`プロパティの場合は、削除操作が正常に完了します。 これは、次のようなコードを使用して実行できます。


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

これらのイベント ハンドラーの両方のコードの分離コード クラスで見つかる、`EfficientPaging.aspx`例。

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>既定およびカスタム ページングのパフォーマンスを比較します。

カスタム ページングは、既定のページングを返しますが、のみ、必要なレコードを取得するため*すべて*表示されている各ページのレコードのカスタム ページングが既定のページングよりも効率的であるが明確な。 しかしよりもはるかに効率的なはカスタム ページングでしょうか。 カスタム ページングを既定のページングを移動して、どのようなパフォーマンスの向上を見なすことができますか。

残念ながら、s がない 1 つのサイズに合ったすべてここに回答します。 パフォーマンスの向上は、さまざまな要因によって異なります、最も顕著な web サーバーとデータベース サーバー間のデータベース サーバーとの通信チャネルにレコードがページングされると、負荷の数をされている 2 つ配置します。 わずか数十個のレコードを含む小さなテーブル向けのパフォーマンスの違いはごくわずかにあります。 大規模なテーブル、数十万行の数千人からのパフォーマンスの違いは、深刻です。

僕の本の記事[SQL Server 2005 で ASP.NET 2.0 でカスタム ページング](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)、これら 2 つのページング手法をデータベース テーブルのページングとの間のパフォーマンスの違いを示すために実行したいくつかのパフォーマンス テストが含まれています50,000 個のレコード。 これらのテストで SQL Server レベルでクエリを実行する時間の両方を検証しました (を使用して[SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) と、ASP.NET ページを使用して[ASP.NET トレース機能](https://msdn.microsoft.com/library/y13fw6we.aspx)します。 注意してこれらのテストが 1 人のアクティブなユーザーには、開発ボックス上で実行およびしたがって科学でないロード パターンの一般的な web サイトを模倣しないにしてください。 関係なく、結果は、十分に大量のデータを使用する場合、既定およびカスタム ページングの実行時間の相対的な違いを示しています。


|  | **平均実行時間 (秒)** | **読み取り** |
| --- | --- | --- |
| **既定の SQL Profiler のページング** | 1.411 | 383 |
| **カスタム ページング SQL Profiler** | 0.002 | 29 |
| **ページングの既定の ASP.NET トレース** | 2.379 | *該当なし* |
| **カスタム ページング ASP.NET トレース** | 0.029 | *該当なし* |


ご覧のように、データの特定のページを取得する平均 354 未満の読み取りに必要なし、短時間で完了します。 ASP.NET ページで、カスタム ページが 1/100 に近いでレンダリングできる<sup>th</sup>かかった時間の既定のページングを使用する場合。 参照してください[筆者の記事](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)コードおよびデータベースと共にこれらの結果の詳細については、独自の環境でこれらのテストを再現するためにダウンロードできます。

## <a name="summary"></a>まとめ

既定のページングはデータ Web コントロールのスマート タグでページングを有効にするチェック ボックス チェックだけを実装するために簡単ですが、パフォーマンスが犠牲にこのようなわかりやすくするため。 ユーザーがデータの任意のページを要求したときに既定のページングと*すべて*レコードが返される場合でも、それらのごく一部だけを表示する可能性があります。 このパフォーマンスのオーバーヘッドを対処するには、ObjectDataSource には、代替ページング オプション カスタム ページングが用意されています。

カスタム ページングが既定の秒のパフォーマンスの問題を表示する必要があるレコードのみを取得することによってページングを改良中に、s カスタム ページングを実装するよりも複雑です。 最初に、正しく (かつ効率的に) にアクセスするレコードの要求の特定のサブセット、クエリを記述する必要があります。 これは、さまざまな方法で実現できます。このチュートリアルで調べる 1 つは、新しい SQL Server 2005 の s を使用する`ROW_NUMBER()`ランク付けする関数の結果、し、返されるだけで結果が順位付けが指定した範囲内にあります。 さらに、ポケットベル通知でレコードの合計数を決定するための手段を追加する必要があります。 これらの DAL、BLL メソッドを作成した後も、合計レコードの数がページングされていると、BLL に、開始行インデックスと行の最大数の値を正しく渡すことができますを確認できるように、ObjectDataSource を構成する必要があります。

カスタム ページングを実装するいくつかの手順では必要でありは既定のページングと同じくらいしない簡単、十分に大量のデータをページングするときカスタム ページングが、必要です。 結果の調査は、カスタム ページング、ASP.NET ページのレンダリング時間から秒を低減でき、1 つ以上桁違いによって、データベース サーバー負荷を軽減することができます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](paging-and-sorting-report-data-vb.md)
> [次へ](sorting-custom-paged-data-vb.md)
