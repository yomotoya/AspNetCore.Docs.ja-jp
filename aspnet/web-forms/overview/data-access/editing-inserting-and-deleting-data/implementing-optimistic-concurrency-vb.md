---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: オプティミスティック同時実行制御 (VB) を実装する |Microsoft Docs
author: rick-anderson
description: データを編集する複数のユーザーを許可する web アプリケーションの場合、2 人のユーザーは編集、同じデータと同時にリスクがあります。 この tutori にしています.
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 63b5a274103851b4b60c92d5fe46125cc4a1b0be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832967"
---
<a name="implementing-optimistic-concurrency-vb"></a>オプティミスティック同時実行制御 (VB) を実装します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe)または[PDF のダウンロード](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> データを編集する複数のユーザーを許可する web アプリケーションの場合、2 人のユーザーは編集、同じデータと同時にリスクがあります。 このチュートリアルでは、このリスクを処理するために、オプティミスティック同時実行制御を実装します。


## <a name="introduction"></a>はじめに

データを表示するユーザーのみを許可する web アプリケーション、またはデータを変更できるユーザー 1 人のユーザーのみが含まれているは、いずれかに別の変更を誤って上書きする 2 つの同時実行ユーザーの脅威はありません。 複数のユーザー データを更新または削除を許可する web アプリケーション、ただし、可能性があるもう 1 つの同時ユーザーと競合する 1 つのユーザーの変更。 せず、同時実行ポリシーを配置するには、2 人のユーザーが同時に 1 つのレコードを編集時にその変更をコミットしたユーザー最後が上書きされます最初によって行われた変更。

たとえば、こと Jisun と Sam、2 人のユーザーが両方ページにアクセスして、アプリケーションで更新および削除の GridView コントロールを使用して、製品への訪問者を許可されているとします。 両方は、ほぼ同時に、gridview 編集ボタンをクリックします。 Jisun では、製品名を「Chai 紅茶」に変更し、[更新] ボタンをクリックします。 最終的には、`UPDATE`を設定すると、データベースに送信されるステートメント*すべて*の製品の更新可能なフィールド (Jisun では、1 つのフィールドのみ更新される場合でも`ProductName`)。 この時点では、データベースは、「Chai 紅茶」、飲み物、供給業者の風変わりな液体は、この特定の製品のカテゴリの値がします。 ただし、Sam の画面に GridView として表示されます、製品名の編集可能な GridView 行"Chai"。 Jisun の変更がコミットされた後、数秒 Sam は調味料にカテゴリを更新し、更新プログラムをクリックします。 これは、結果、 `UPDATE` "Chai"に、製品名を設定しているデータベースに送信されたステートメント、`CategoryID`対応する飲み物のカテゴリの ID、および具合にします。 Jisun の製品名の変更が上書きされました。 図 1 は、この一連のイベントをグラフィカルに示しています。


[![2 人のユーザーが同時に 1 つのユーザーの変更を上書きするその他のレコードが存在 s が発生する可能性を更新すると](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**図 1**: と 2 人のユーザーを同時に更新レコードが s 潜在的な 1 つのユーザーが他の上書きに対する変更 ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image3.png))。


同様に、2 人のユーザーは、ページにアクセスして、1 人のユーザーはレコードを更新する別のユーザーによって削除されるときに処理する可能性があります。 または、ユーザーがページを読み込むときと削除 ボタンをクリックするの別のユーザーが変更そのレコードの内容。

3 つ[同時実行制御](http://en.wikipedia.org/wiki/Concurrency_control)戦略を使用できます。

- **何もしない**-同時実行ユーザーは、同じレコードを変更するは、(既定の動作) を獲得する最後のコミットをお知らせ
- [**オプティミスティック同時実行制御**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -同時実行は、このような競合が発生しません時間の大半で競合し、ある可能性がありますそのため、場合は、競合が発生した場合、単に通知するユーザーを想定していますが、変更。別のユーザーには、同じデータが変更されたために、保存できません。
- **ペシミスティック同時実行制御**-同時実行の競合は一般的であり別のユーザーの同時実行のアクティビティのための変更が保存されなかったとユーザーがいるを許容しないことを前提としていますそのため、1 つのユーザーは、レコードの更新を起動するときにロック。、他のユーザーを編集したり、ユーザーがその変更をコミットするまでそのレコードの削除を防ぐ

勝利の最後の書き込みを具体的には、お知らせした、すべて、チュートリアルのこれまでとして既定の同時実行の解決方法を使用が。 このチュートリアルではオプティミスティック同時実行制御を実装する方法を説明します。

> [!NOTE]
> このチュートリアル シリーズでのペシミスティック同時実行制御の例に注目しません。 ペシミスティック同時実行制御はなどのロックのため、ほとんど使用されていなければ正しく開放、他のユーザーがデータを更新するを防ぐことができます。 たとえば、ユーザーを編集するためのレコードをロックし、そのロックを解除する前に、その日のまま場合は、その他のユーザーはありません、元のユーザーが指定値を返しの更新が完了するまで、そのレコードを更新できません。 したがって、ペシミスティック同時実行制御が使用されている場合は通常、タイムアウトに達すると、ロックをキャンセルします。 チケット販売 web サイト、短い期間の特定の席の場所をロックする、ユーザーが注文処理を完了するまで、ペシミスティック同時実行制御の例に示します。


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>手順 1: の実装はオプティミスティック同時実行制御方法を見る

オプティミスティック同時実行制御は、更新または削除プロセスを開始するときと同様、更新または削除されるレコードが同じ値があることを確認することによって機能します。 たとえば、編集可能な GridView で [編集] ボタンをクリックすると、レコードの値はデータベースから読み取るありテキスト ボックスや他の Web コントロールに表示されます。 これらの元の値は、GridView で保存されます。 後で、ユーザーは、自分の変更を行います、[更新] ボタンをクリックして、後に元の値と新しい値を送受信するビジネス ロジック層、し、データ アクセス層まで。 データ アクセス層は、ユーザーが編集を開始した元の値は、データベースに引き続き値と同じ場合のみ、レコードを更新する SQL ステートメントを実行する必要があります。 図 2 は、このイベントのシーケンスを示しています。


[![正常に更新または削除、元の値は現在のデータベースの値と等しくする必要があります。](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**図 2**: For Update または成功を元の値必要がありますと等しいデータベースの現在の値を Delete ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image6.png))。


オプティミスティック同時実行制御を実装するためのさまざまな方法はあります (を参照してください[Peter A. 作成](http://peterbromberg.net/)の[Optmistic 同時実行更新ロジック](http://www.eggheadcafe.com/articles/20050719.asp)のさまざまなオプションについて簡単に説明)。 ADO.NET 型指定されたデータセットは、チェック ボックスのチェック マークだけで構成できる 1 つの実装を提供します。 型指定されたデータセット内に TableAdapter は、TableAdapter のオプティミスティック同時実行制御を有効にする`UPDATE`と`DELETE`のすべての元の値の比較を含めるようにステートメントを`WHERE`句。 次`UPDATE`ステートメントでは、たとえば、更新プログラム名と製品の価格データベースの現在の値が、GridView でレコードを更新するときに取得された元の値に等しい場合のみです。 `@ProductName`と`@UnitPrice`パラメーターには、ユーザーが入力した新しい値が含まれて`@original_ProductName`と`@original_UnitPrice`編集ボタンがクリックされたときに、GridView に読み込まれた最初の値が含まれます。


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> これは、`UPDATE`読みやすくするため、ステートメントを簡素化されています。 実際には、`UnitPrice`チェックイン、`WHERE`句は、以降は複雑になります`UnitPrice`含めることができます`NULL`s と確認しているとき`NULL = NULL`常に False を返します (代わりに使用する必要があります`IS NULL`)。


さまざまな基になるだけでなく`UPDATE`ダイレクト メソッドのステートメントでは、オプティミスティック同時実行もその DB のシグネチャを変更して、TableAdapter を構成します。 最初のチュートリアルでは、メッセージの取り消し[*データ アクセス層を作成する*](../introduction/creating-a-data-access-layer-cs.md)、値の入力パラメーターとしての DB のダイレクト メソッドに、スカラーの一覧を受け取るものがあったこと (厳密に型指定された DataRow ではなくまたはDataTable インスタンスの場合)。 オプティミスティック同時実行制御、直接の DB を使用する場合`Update()`と`Delete()`メソッドにも、元の値の入力パラメーターが含まれます。 さらに、BLL バッチを使用するためのコードの更新パターン (、`Update()`をスカラー値ではなく、Datarow とデータ テーブルを受け取るメソッド オーバー ロード) も変更する必要があります。

はなく、既存の拡張よりも DAL の Tableadapter みましょう (これに対応する BLL の変更が必要となる) オプティミスティック同時実行を使用する代わりに新しいデータセットを作成型指定された名前付き`NorthwindOptimisticConcurrency`に追加する`Products`TableAdapter をオプティミスティック同時実行制御を使用します。 次に、作成します、 `ProductsOptimisticConcurrencyBLL` DAL、オプティミスティック同時実行制御をサポートするために適切な変更が含まれているビジネス ロジック層のクラス。 用意した土台を構築すると ASP.NET ページを作成する準備になります。

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>手順 2: は、オプティミスティック同時実行制御をサポートするデータ アクセス層を作成します。

新しい型指定されたデータセットを作成するを右クリックし、`DAL`内のフォルダー、`App_Code`フォルダーという名前の新しいデータセットを追加および`NorthwindOptimisticConcurrency`します。 最初のチュートリアルで説明したように行うのために追加されます新しい TableAdapter を TableAdapter 構成ウィザードを自動的に起動する、型指定されたデータセット。 データベースへの接続 - を使用して Northwind データベースの同じ接続を指定するよう指示最初の画面で、`NORTHWNDConnectionString`設定から`Web.config`します。


[![同じ Northwind データベースへの接続します。](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**図 3**: 同じ Northwind データベースへの接続 ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image9.png))。


次に、データを照会する方法についてよう求められます。 アドホック SQL ステートメントでは、新しいストアド プロシージャ、または既存のストアド プロシージャ。 元の DAL でアドホック SQL クエリを使用するとため、このオプションを使用ここでも。


[![アドホック SQL ステートメントを使用して取得するデータを指定します。](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**図 4**: アドホック SQL ステートメントを使用して取得するデータを指定 ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image12.png))。


次の画面で、製品情報の取得に使用する SQL クエリを入力します。 使用する正確な同じ SQL クエリを使用しましょう、`Products`で TableAdapter を返しますのすべて、元の DAL、`Product`名、製品のサプライヤーとカテゴリ名と列。


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]


[![元の DAL で製品 TableAdapter から同じ SQL クエリを使用します。](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**図 5**: から、同じ SQL クエリを使用して、`Products`元の DAL の TableAdapter ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image15.png))。


次の画面上に移動すると、前に、詳細オプション ボタンをクリックします。 この TableAdapter 採用のオプティミスティック同時実行制御には、「オプティミスティック同時実行制御を使用して、」チェック ボックスにチェックします。


[![当座預金でオプティミスティック同時実行制御を有効にする、&quot;オプティミスティック同時実行制御を使用して、&quot;チェック ボックス](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**図 6**: オプティミスティック同時実行制御 [オプティミスティック同時実行制御を使用する] チェック ボックスをオンを有効にする ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image18.png))。


最後に、TableAdapter に datatable し、; DataTable を返すデータ アクセス パターンを使用することを示しますDB のダイレクト メソッドを作成することも示します。 GetProducts に GetData の戻り値は、メソッド名 DataTable パターンを変更、名前付け規則をミラーリングするように、元の DAL で使用しています。


[![すべてのデータ アクセス パターンを利用する TableAdapter があります。](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**図 7**: TableAdapter 利用すべてのデータ アクセス パターンがある ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image21.png))。


データセット デザイナー ウィザードを完了すると、厳密に型が含まれます`Products`DataTable および TableAdapter。 DataTable の名前を変更する少し`Products`に`ProductsOptimisticConcurrency`DataTable のタイトル バーを右クリックして、コンテキスト メニューから名前の変更を選択して行うことができます。


[![型指定された DataSet に DataTable と TableAdapter が追加されました。](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**図 8**: A DataTable と型指定されたデータセットに追加された TableAdapter ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image24.png))。


間の相違点を確認する、`UPDATE`と`DELETE`間でクエリを実行、 `ProductsOptimisticConcurrency` TableAdapter (オプティミスティック同時実行制御を使用) して (これは) 製品の TableAdapter に TableAdapter をクリックし、[プロパティ] ウィンドウに移動します。 `DeleteCommand`と`UpdateCommand`プロパティの`CommandText`サブプロパティ DAL の更新または削除に関連するメソッドが呼び出されたときに、データベースに送信される実際の SQL 構文を確認します。 `ProductsOptimisticConcurrency` TableAdapter、`DELETE`ステートメントを使用します。


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

一方、`DELETE`元の DAL の製品 TableAdapter のステートメントは、はるかに簡単です。


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

ご覧のとおり、`WHERE`句、`DELETE`オプティミスティック同時実行制御を使用する TableAdapter のステートメントには間の比較が含まれています、`Product`テーブルの既存の列の値と元の値に、GridView や DetailsView (FormView) 最後に作成されました。 以外のすべてのフィールドから`ProductID`、`ProductName`と`Discontinued`が`NULL`値やその他のパラメーター チェックが正しく比較に含まれる`NULL`値、`WHERE`句。

いますしませんが、追加のデータ テーブル オプティミスティック同時実行制御が有効なデータセットにこのチュートリアルでは、として追加、ASP.NET ページは、更新および製品情報の削除のみ提供します。 ただし、私たちを追加する必要は引き続き、`GetProductByProductID(productID)`メソッドを`ProductsOptimisticConcurrency`TableAdapter。

これを行うには、TableAdapter のタイトル バーを右クリックし (領域権利、`Fill`と`GetProducts`メソッド名)、コンテキスト メニューから追加のクエリを選択します。 これにより、TableAdapter クエリの構成ウィザードが起動します。 TableAdapter の初期構成では、作成することと、`GetProductByProductID(productID)`アドホック SQL ステートメントを使用するメソッド (図 4 参照)。 以降、`GetProductByProductID(productID)`メソッドは、特定の製品に関する情報を返します、このクエリがあることを示す、`SELECT`行を返す型のクエリを実行します。


[![クエリの型としてマークする&quot;を行を返す SELECT&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**図 9**: クエリの型としてマークする"`SELECT`行を返す"([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image27.png))。


次の画面を事前に読み込まれた、TableAdapter の既定のクエリで、使用する SQL クエリ求められたら。 句に含める既存のクエリを補強`WHERE ProductID = @ProductID`図 10 に示すようにします。


[![追加、WHERE 句を事前に読み込まれたクエリが特定の製品レコードを返す](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**図 10**: 追加、`WHERE`句を Pre-Loaded クエリが特定の製品レコードを返す ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image30.png))。


最後に、生成されたメソッド名を変更`FillByProductID`と`GetProductByProductID`します。


[![FillByProductID を GetProductByProductID メソッドの名前を変更します。](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**図 11**: メソッドの名前を変更`FillByProductID`と`GetProductByProductID`([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image33.png))。


このウィザードは完了で TableAdapter にデータを取得するための 2 つのメソッド: `GetProducts()`、返された*すべて*製品; と`GetProductByProductID(productID)`、指定された製品が返されます。

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>手順 3: オプティミスティック同時実行制御が有効な DAL のビジネス ロジック層を作成します。

既存`ProductsBLL`クラスには、バッチ更新とダイレクト パターンの DB を使用しての例を示します。 `AddProduct`メソッドと`UpdateProduct`両方オーバー ロードを渡して、バッチ更新パターンを使用して、 `ProductRow` TableAdapter の Update メソッドのインスタンス。 `DeleteProduct`メソッドで呼び出す TableAdapter の DB 直接パターンが使用一方、`Delete(productID)`メソッド。

新しい`ProductsOptimisticConcurrency`TableAdapter、DB のダイレクト メソッドで元の値が渡すもする必要があるようになりました。 たとえば、`Delete`メソッドが 10 個の入力パラメーターが受け取るようになりました元`ProductID`、 `ProductName`、 `SupplierID`、 `CategoryID`、 `QuantityPerUnit`、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、 `ReorderLevel`、。`Discontinued`します。 これら追加入力パラメーターの値を使用して`WHERE`の句、`DELETE`ステートメントだけで指定されたレコードを削除するまで、元のデータベースの現在の値がマップに、データベースに送信します。

Tableadapter のメソッド シグネチャを while`Update`バッチ更新パターンで使用される方法が変更されていない、オリジナルと新しい値を記録するために必要なコードにします。 そのため、当社の既存のオプティミスティック同時実行制御が有効な DAL を使用しようとするのではなく`ProductsBLL`クラスで、新しい DAL を操作するための新しいビジネス ロジック層クラスを作成しましょう。

という名前のクラスを追加`ProductsOptimisticConcurrencyBLL`を`BLL`内のフォルダー、`App_Code`フォルダー。


![ProductsOptimisticConcurrencyBLL クラス BLL フォルダーを追加します。](implementing-optimistic-concurrency-vb/_static/image34.png)

**図 12**: 追加、 `ProductsOptimisticConcurrencyBLL` BLL フォルダーにクラス


次に、次のコードを追加、`ProductsOptimisticConcurrencyBLL`クラス。


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

使用して、注意してください。`NorthwindOptimisticConcurrencyTableAdapters`クラス宣言の先頭上記のステートメント。 `NorthwindOptimisticConcurrencyTableAdapters`名前空間が含まれています、`ProductsOptimisticConcurrencyTableAdapter`クラスで、DAL のメソッドを提供します。 また、クラス宣言の前に見つかります、`System.ComponentModel.DataObject`属性には、Visual Studio、ObjectDataSource ウィザードのドロップダウン リストにこのクラスを含めるように指示します。

`ProductsOptimisticConcurrencyBLL`の`Adapter`プロパティのインスタンスにクイック アクセスを提供、`ProductsOptimisticConcurrencyTableAdapter`クラスし、元の BLL クラスで使用されるパターンに従います (`ProductsBLL`、`CategoriesBLL`など)。 最後に、`GetProducts()`メソッドを呼び出すだけです DAL の`GetProducts()`メソッドを返します、`ProductsOptimisticConcurrencyDataTable`オブジェクトを含む、`ProductsOptimisticConcurrencyRow`データベース内の各製品レコードのインスタンス。

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>オプティミスティック同時実行制御で直接パターンの DB を使用して商品を削除します。

オプティミスティック同時実行制御を使用する DAL に対して DB 直接パターンを使用する場合、メソッドが新しいと、元の値を渡される必要があります。 削除するには、値がない新しい、ために元の値のみを渡す必要があります。 当社の BLL にし、する必要がありますは受け付けてすべて元のパラメーターの入力パラメーターとして。 みましょうが、`DeleteProduct`メソッドで、`ProductsOptimisticConcurrencyBLL`クラスは、DB のダイレクト メソッドを使用します。 これは、このメソッドは、すべての 10 個の製品データ フィールドの入力パラメーターとしてでは、次のコードに示すように、DAL に渡す必要があることを意味します。


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

削除ボタンをクリックしたときに、データベース内の値からの GridView、DetailsView、またはフォーム ビュー) に最後に読み込まれたこれらの値の元の値が異なる場合、`WHERE`句は、任意のデータベース レコードとレコード一致しません影響があります。 そのため、TableAdapter の`Delete`メソッドが返す`0`と BLL の`DeleteProduct`メソッドが返す`false`します。

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>オプティミスティック同時実行制御でバッチ更新パターンを使用して製品の更新

前に述べた、TableAdapter の`Update`バッチ更新パターンのメソッドがオプティミスティック同時実行制御が使用されているかどうかに関係なく同じメソッド シグネチャ。 つまり、`Update`メソッドは、配列 Datarow、DataTable、または型指定されたデータセットの DataRow を想定します。 元の値を指定するための追加の入力パラメーターはありません。 DataTable の追跡元と変更された値をその DataRow(s) のため可能です。 DAL を発行したとき、`UPDATE`ステートメント、`@original_ColumnName`一方、DataRow の元の値を持つパラメーターが設定されます、`@ColumnName`パラメーターには、DataRow の変更後の値が設定されます。

`ProductsBLL`クラス (、元の非オプティミスティック同時実行 DAL を使用) する場合は、バッチ更新パターンを使用して、コードでは、次の一連のイベントを実行します。 製品情報を更新する場合。

1. 現在のデータベース製品情報を読み取り、 `ProductRow` TableAdapter を使用してインスタンス`GetProductByProductID(productID)`メソッド
2. 新しい値を割り当てる、`ProductRow`手順 1. のインスタンス
3. 呼び出す TableAdapter の`Update`に渡して、メソッド、`ProductRow`インスタンス

この一連の手順、ただしはオプティミスティック同時実行制御をサポート正しくされなくなります、`ProductRow`で設定されますつまり、DataRow で使用される元の値に現在存在するものは、データベースから直接ステップ 1 が設定されます、。データベース、および編集のプロセスの開始時の GridView にバインドされていたものされません。 代わりに、使用、オプティミスティック同時実行が有効な DAL、必要がありますを変更する、`UpdateProduct`次の手順を使用するメソッドのオーバー ロードします。

1. 現在のデータベース製品情報を読み取り、 `ProductsOptimisticConcurrencyRow` TableAdapter を使用してインスタンス`GetProductByProductID(productID)`メソッド
2. 割り当てる、*元*値を`ProductsOptimisticConcurrencyRow`手順 1. のインスタンス
3. 呼び出す、`ProductsOptimisticConcurrencyRow`インスタンスの`AcceptChanges()`メソッドは、現在の値が「元」の DataRow を指示します
4. 割り当てる、*新しい*値を`ProductsOptimisticConcurrencyRow`インスタンス
5. 呼び出す TableAdapter の`Update`に渡して、メソッド、`ProductsOptimisticConcurrencyRow`インスタンス

すべてのデータベースの現在の値で指定された製品レコードの読み取りを手順 1。 この手順は不要、`UpdateProduct`を更新するオーバー ロード*すべて*製品列の (これらの値として、上書き手順 2. で)、列の値のサブセットのみとしてに渡される、これらのオーバー ロードに不可欠ですが、入力パラメーターです。 元の値が割り当てられていると、`ProductsOptimisticConcurrencyRow`インスタンス、`AcceptChanges()`メソッドが呼び出された、DataRow の現在の値で使用される元の値としてマークする、`@original_ColumnName`内のパラメーター、`UPDATE`ステートメント。 新しいパラメーターの値が次に、割り当てられている、 `ProductsOptimisticConcurrencyRow` 、最後に、 `Update` DataRow を渡して、メソッドが呼び出されます。

次のコードは、`UpdateProduct`を製品のすべてのデータを受け入れるオーバー ロードの入力パラメーターとしてのフィールドします。 ここでは、表示しない、`ProductsOptimisticConcurrencyBLL`クラスにこのチュートリアルにも含まれています、ダウンロードに含まれる、`UpdateProduct`だけ、製品の名前と価格を入力パラメーターとして受け取るオーバー ロードします。


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>手順 4: は、BLL メソッドに、ASP.NET ページから、元と新しい値を渡す

DAL BLL 完了とは、システムに組み込まれているオプティミスティック同時実行制御ロジックを使用することができる ASP.NET ページを作成します。 具体的には、データ Web コントロール (GridView、DetailsView、またはフォーム ビュー) では、元の値と ObjectDataSource にビジネス ロジック層の両方の値のセットを渡す必要がありますを忘れないでください。 さらに、同時実行制御違反を適切に処理する ASP.NET ページを構成する必要があります。

開いて開始、`OptimisticConcurrency.aspx`ページで、`EditInsertDelete`フォルダーと、デザイナーの設定への GridView の追加、`ID`プロパティを`ProductsGrid`します。 という名前の新しい ObjectDataSource を作成することを選択、GridView のスマート タグから`ProductsOptimisticConcurrencyDataSource`します。 この ObjectDataSource オプティミスティック同時実行制御をサポートする DAL を使用するので、構成を使用するよう、`ProductsOptimisticConcurrencyBLL`オブジェクト。


[![ObjectDataSource 使用 ProductsOptimisticConcurrencyBLL オブジェクトがあります。](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**図 13**: ObjectDataSource 使用している、`ProductsOptimisticConcurrencyBLL`オブジェクト ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image37.png))。


選択、 `GetProducts`、 `UpdateProduct`、および`DeleteProduct`ウィザードで、ドロップダウン リストからメソッド。 UpdateProduct メソッドでは、すべての製品のデータ フィールドを受け入れるオーバー ロードを使用します。

## <a name="configuring-the-objectdatasource-controls-properties"></a>ObjectDataSource コントロールのプロパティを構成します。

ウィザードの完了後に、次のよう ObjectDataSource の宣言型マークアップになります。


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

ご覧のとおり、`DeleteParameters`コレクションに含まれる、`Parameter`で 10 個の入力パラメーターの各インスタンス、`ProductsOptimisticConcurrencyBLL`クラスの`DeleteProduct`メソッド。 同様に、`UpdateParameters`コレクションに含まれる、`Parameter`で入力パラメーターの各インスタンス`UpdateProduct`します。

ObjectDataSource を削除するところのデータの変更を関連するこれらの以前チュートリアル`OldValuesParameterFormatString`プロパティをこの時点であるため、このプロパティは、BLL メソッドが、古い (あるいは元) の値で渡されると、新しい値を期待していることを示します。 さらに、このプロパティの値では、元の値の入力パラメーターの名前を示します。 BLL に元の値で渡される、ため*いない*このプロパティを削除します。

> [!NOTE]
> 値、`OldValuesParameterFormatString`プロパティは、元の値を期待する BLL 内の入力パラメーター名にマップする必要があります。 これらのパラメーターという名前であるため`original_productName`、`original_supplierID`で、おくことができます、`OldValuesParameterFormatString`プロパティの値として`original_{0}`。 かどうか、ただし、BLL メソッドの入力パラメーターがのような名前`old_productName`、`old_supplierID`で、更新する必要があります、`OldValuesParameterFormatString`プロパティを`old_{0}`します。


BLL メソッドに元の値を正しく渡す ObjectDataSource の順序で実行する必要がある最後のプロパティ設定を 1 つがあります。 ObjectDataSource は、 [ConflictDetection プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx)に割り当てること[2 つの値のいずれかの](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -既定値です。BLL メソッドの元の入力パラメーターには、元の値を送信しません
- `CompareAllValues` は、BLL 方法; 元の値を送信オプティミスティック同時実行制御を使用する場合は、このオプションを選択します。

設定する少し、`ConflictDetection`プロパティを`CompareAllValues`します。

## <a name="configuring-the-gridviews-properties-and-fields"></a>GridView のプロパティとパブリック フィールドを構成します。

ObjectDataSource のプロパティが正しく構成されている、GridView の設定に注目してみましょう。 最初に、編集および削除をサポートするために、GridView、たいので、GridView のスマート タグから、編集の有効化および削除を有効にするチェック ボックスをクリックします。 これを [commandfield] が追加されますが`ShowEditButton`と`ShowDeleteButton`に設定されて`true`します。

バインドするとき、 `ProductsOptimisticConcurrencyDataSource` ObjectDataSource、GridView に各製品のデータ フィールドのフィールドが含まれています。 このような GridView を編集できますが、ユーザー エクスペリエンスが許容されるは。 `CategoryID`と`SupplierID`BoundFields がテキスト ボックスとしてレンダリングされます ID 番号として、適切なカテゴリと仕入先を入力する必要はありません。 ありません、数値フィールドおよび製品の名前が指定されているし unit price、在庫数、順序、および並べ替えレベルの値の単位は両方の適切な数値とよりも大きいか等しいことを確認するには、いない検証コントロールの書式設定0 を返します。

説明したように、*編集および挿入インターフェイスに検証コントロールを追加*と*データ変更インターフェイスをカスタマイズ*チュートリアルでは、ユーザー インターフェイスでカスタマイズできますTemplateFields、BoundFields で置き換えます。 この GridView とその編集インターフェイスは次の方法で変更しました。

- 削除、 `ProductID`、 `SupplierName`、および`CategoryName`BoundFields
- 変換、 `ProductName` TemplateField に BoundField RequiredFieldValidation コントロールを追加します。
- 変換、`CategoryID`と`SupplierID`BoundFields TemplateFields に、テキスト ボックスではなく、Dropdownlist を使用して編集インターフェイスを調整します。 これらの TemplateFields' で`ItemTemplates`、`CategoryName`と`SupplierName`データ フィールドが表示されます。
- 変換、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`BoundFields TemplateFields を CompareValidator コントロールを追加します。

前のチュートリアルでこれらのタスクを実行する方法をについて説明しました既に、ため最終的な宣言型構文を一覧表示し、プラクティスとして、実装のままにだけ行います。


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

私たちは非常にも完全に実際の例の近くにします。 ただし、いくつかの微妙な襲ってくると、問題が発生があります。 さらに、いくつかのインターフェイスを同時実行制御違反が発生したときに、ユーザーに警告する必要があります。

> [!NOTE]
> データ Web コントロールで (これは、BLL に渡されますが)、ObjectDataSource に元の値を正しく渡すには、ことが重要ですが、GridView の`EnableViewState`プロパティに設定されて`true`(既定値)。 ビュー ステートを無効にした場合は、ポストバック時に元の値は失われます。


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>ObjectDataSource に適切な元の値を渡す

いくつかの GridView が構成されている方法で問題があります。 場合 ObjectDataSource の`ConflictDetection`プロパティに設定されて`CompareAllValues`(としては渡しませんよ)、ObjectDataSource の`Update()`または`Delete()`メソッドは、GridView、DetailsView、または FormView) によって呼び出される、ObjectDataSource がコピーしようとした場合、GridView の元の値に適切な`Parameter`インスタンス。 このプロセスのグラフィカル表現を図 2 を戻す参照します。

具体的には、GridView の元の値には、毎回、データが GridView にバインドが双方向データ バインド ステートメント内の値が割り当てられます。 そのため、双方向データ バインドを使用してすべての必要な元の値がキャプチャされる変換できる形式で提供されることを不可欠です。

これが重要な理由を表示するには、ブラウザーでページを参照するのにはしばらくかかります。 予想どおり、GridView には、左端の列で、編集、削除ボタンでは、各製品が一覧表示します。


[![製品を GridView に表示されます。](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**図 14**:、製品を GridView に表示される ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image40.png))。


任意の製品の削除 ボタンをクリックした場合、`FormatException`がスローされます。


[![FormatException で、製品の結果を削除しようとしています。](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**図 15**: で、製品の結果を削除しようとして、 `FormatException` ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image43.png))。


`FormatException` ObjectDataSource が元の読み取りしようとしたときに発生`UnitPrice`値。 以降、`ItemTemplate`が、`UnitPrice`を通貨として書式設定 (`<%# Bind("UnitPrice", "{0:C}") %>`) のような 19.95 ドルの通貨記号が含まれています。 `FormatException` ObjectDataSource が、この文字列に変換しようとしています。 ときに発生する`decimal`します。 この問題を回避するためには、さまざまなオプションがあります。

- 通貨の書式設定の削除、`ItemTemplate`します。 つまり、使用する代わりに`<%# Bind("UnitPrice", "{0:C}") %>`、使用するだけで`<%# Bind("UnitPrice") %>`。 これの欠点は、価格の形式が不要になったことです。
- 表示、`UnitPrice`の通貨として書式設定、`ItemTemplate`が使用して、`Eval`これを実現するキーワード。 いることを思い出してください`Eval`一方向のデータ バインドを実行します。 提供する必要があります、`UnitPrice`で双方向データ バインド ステートメントが必要も、元の値の値、 `ItemTemplate`、これに配置できるラベル Web コントロールを持つが、`Visible`プロパティに設定されて`false`します。 次のマークアップを ItemTemplate に使用できます。


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- 通貨の書式設定の削除、`ItemTemplate`を使用して、`<%# Bind("UnitPrice") %>`します。 Gridview の`RowDataBound`ラベル Web コントロールを内のイベント ハンドラー、プログラムでアクセス、`UnitPrice`値が表示され、設定、`Text`プロパティを書式設定されたバージョン。
- ままに、`UnitPrice`を通貨として書式設定します。 Gridview の`RowDeleting`イベント ハンドラーでは、元の既存の置換`UnitPrice`値 (19.95 ドル) を使用して、実際の 10 進値で`Decimal.Parse`します。 ようなものを実現する方法を説明しました、`RowUpdating`内のイベント ハンドラー、 [*処理 BLL - と DAL レベルの例外で、ASP.NET ページ*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)チュートリアル。

2 番目のアプローチを使用した例いるラベル Web を非表示を追加するコントロール`Text`プロパティは、書式設定されていないにバインドされた双方向データ`UnitPrice`値。

この問題を解決するには、製品の削除 ボタンをもう一度クリックしてください。 この時間が表示されます、 `InvalidOperationException` ObjectDataSource が BLL の呼び出しを試行するときに`UpdateProduct`メソッド。


[![ObjectDataSource は、送信する入力パラメーターを持つメソッドを見つけることができません。](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**図 16**: ObjectDataSource は、送信する入力パラメーターを持つメソッドを見つけることができません ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image46.png))。


例外のメッセージを見ると、明確では、ObjectDataSource、BLL を起動する必要のある`DeleteProduct`メソッドが含まれる`original_CategoryName`と`original_SupplierName`パラメーターを入力します。 これは、ため、`ItemTemplate`の`CategoryID`と`SupplierID`TemplateFields が現在の双方向のバインド ステートメントを含めることが、`CategoryName`と`SupplierName`データ フィールド。 代わりに、含める必要があります`Bind`ステートメントと、`CategoryID`と`SupplierID`データ フィールド。 これを実現するには、既存のバインド ステートメントを置き換えます`Eval`ステートメントを追加し、非表示のラベル コントロールが`Text`プロパティにバインドされます、`CategoryID`と`SupplierID`に示すように、双方向データ バインドを使用してデータ フィールド以下に：


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

これらの変更が正常に削除し、製品情報を編集することがようになりました。 手順 5 では、同時実行制御違反が検出されていることを確認する方法を紹介します。 ここでは、数分を更新してみてくださいいて、期待どおりに動作を更新して、1 人のユーザーを削除することを確認するいくつかのレコードを削除しています。

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>手順 5: オプティミスティック同時実行制御のサポートをテストします。

同時実行制御違反が検出された (なく無条件に上書きされないデータの結果として得られる) されていることを確認するためには、このページに 2 つのブラウザー ウィンドウを開く必要があります。 両方のブラウザー インスタンス Chai の編集 ボタンをクリックします。 だけのいずれかで、ブラウザー、「Chai 紅茶」に名前を変更し、[更新] をクリックします。 更新プログラムが成功し、GridView を新しい製品名として「Chai 紅茶」で、編集済み状態に戻す必要があります。

その他のブラウザー ウィンドウ インスタンスでただし、製品名 TextBox であっても、"Chai"。 この 2 番目のブラウザー ウィンドウでは、更新、`UnitPrice`に`25.00`します。 オプティミスティック同時実行制御のサポートがない場合、2 番目のブラウザー インスタンスでの更新 をクリックしては、製品名を変更"Chai"、最初のブラウザー インスタンスによって行われた変更が上書きされます。 採用されているオプティミスティック同時実行制御、ただし、2 番目のブラウザー インスタンスでの更新ボタンをクリックすると結果を[DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx)します。


[![同時実行制御違反が検出されると、DBConcurrencyException がスローされます。](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**図 17**: ときの同時実行制御違反が検出されると、`DBConcurrencyException`がスローされます ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image49.png))。


`DBConcurrencyException` DAL のバッチ更新パターンを利用する際にのみスローされます。 DB 直接パターンが例外を発生させない、影響を受けた行がないことだけを示します。 これを示すためには、編集済みの状態に両方のブラウザー インスタンスの GridView を返します。 次に、最初のブラウザー インスタンスで編集ボタンをクリックし、"Chai"には、「Chai 紅茶」から製品名を変更する更新をクリックします。 2 番目のブラウザー ウィンドウでは、Chai の削除 ボタンをクリックします。

削除をクリックすると、ページがポストバック、GridView 呼び出す ObjectDataSource の`Delete()`メソッド、および、ObjectDataSource 呼び出して、`ProductsOptimisticConcurrencyBLL`クラスの`DeleteProduct`メソッド、元の値を渡します。 元の`ProductName`「Chai 紅茶」現在と一致しません。 これは、2 番目のブラウザー インスタンスを値`ProductName`データベース内の値。 そのため、`DELETE`データベースにレコードがないため、データベースへの発行ステートメントが 0 の行に影響する、`WHERE`句を満たします。 `DeleteProduct`メソッドを返します。 `false` ObjectDataSource のデータが GridView にバインドするとします。

エンドユーザーの観点からフラッシュをスクリーンの原因と 2 番目のブラウザー ウィンドウで Chai 紅茶の削除 ボタンをクリックするとに戻るには、製品は問題がないが、"Chai"(製品名行われた変更が最初のブラウザーでとして記載されているようになりましたインスタンスの場合)。 ユーザーでは、もう一度削除 ボタンがクリックした場合、削除は成功、GridView の元として`ProductName`値 ("Chai") ようになりましたが一致する、データベース内の値。

どちらのこのような場合、ユーザー操作は理想から遠く離れたです。 ユーザーの具体的な説明を表示する明らかにしたくない、`DBConcurrencyException`バッチ更新パターンを使用する場合は例外です。 ユーザー コマンドが失敗しましたが、正確な理由を示す値がありませんでした。 は、DB 直接パターンを使用するときの動作は少し紛らわしいです。

これら 2 つの問題を解決するには、更新または削除の失敗理由を説明するページ ラベル Web コントロールを作成できます。 バッチ更新パターンを確認できるかどうかを`DBConcurrencyException`必要に応じて、警告のラベルを表示する GridView の後のレベルのイベント ハンドラーで例外が発生しました。 DB のダイレクト メソッドは、BLL メソッドの戻り値を確認できます (これは`true`1 つの行が影響を受ける場合`false`それ以外の場合) し、必要に応じて情報メッセージを表示します。

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>手順 6: は情報メッセージを追加して、同時実行制御違反が発生した場合に表示する方法

同時実行制御違反が発生したときに発生した現象は、DAL のバッチ更新または DB 直接パターンが使用されるかどうかに依存します。 このチュートリアルでは、更新および削除するために使用される DB 直接パターンで使用されているバッチ更新パターンでは、両方のパターンを使用します。 開始するには、削除、またはデータを更新しようとしてください。 同時実行制御違反が発生したことを説明するページに 2 つのラベルの Web コントロールを追加してみましょう。 ラベル コントロールの設定`Visible`と`EnableViewState`プロパティ`false`; これにより、それらの特定のページにアクセス where 点を除いて、各ページのアクセス時に非表示にする、`Visible`プロパティ プログラムで`true`します。


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

設定だけでなく、 `Visible`、`EnabledViewState`と`Text`プロパティも設定した、`CssClass`プロパティを`Warning`、大規模な赤、斜体、太字のフォントで表示されるのラベルを停止します。 この CSS`Warning`クラスが定義され、Styles.css に追加されたに戻り、 *、イベントに関連付けられている挿入、更新、および削除の確認*チュートリアル。

これらのラベルを追加すると、Visual Studio のデザイナーがこのよう図 18 になります。


[![2 つのラベル コントロールがページに追加されました](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**図 18**: 2 つのラベル コントロールに追加されたページ ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image52.png))。


場所でこれらのラベルの Web コントロール、私たちは同時実行制御違反が発生した場合、ポイントの適切なラベルの位置を決定する方法を詳しく調べる準備が`Visible`にプロパティを設定することができます`true`、情報メッセージを表示します。

## <a name="handling-concurrency-violations-when-updating"></a>更新するときに、同時実行制御違反を処理

まず、バッチ更新パターンを使用する場合は、同時実行制御違反を処理する方法を見てみましょう。 バッチには、このような違反のパターンの原因を更新するため、`DBConcurrencyException`例外がスローされる、ASP.NET ページを決定するコードを追加する必要があるかどうかを`DBConcurrencyException`更新プロセス中に例外が発生しました。 そのため、レコードの編集を開始するときに、別のユーザーの間で同じデータが変更するため、その変更は保存されませんでしたが説明するユーザーにメッセージを表示する必要があり、それらの更新ボタンがクリックされたときにします。

説明したように、*処理 BLL - と DAL レベルの例外で、ASP.NET ページ*チュートリアルで、このような例外を検出およびデータ Web コントロールの後のレベルのイベント ハンドラーで抑制できます。 そのため、GridView のイベント ハンドラーを作成する必要があります`RowUpdated`場合にチェックするイベントを`DBConcurrencyException`例外がスローされました。 このイベント ハンドラーには、イベント ハンドラーは、以下のコードに示すように更新の処理中に発生したすべての例外への参照が渡されます。


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

`DBConcurrencyException`例外では、このイベント ハンドラーの表示、`UpdateConflictMessage`コントロールのラベルし、例外が処理されたことを示します。 場所でこのコードでは、レコードを更新するときに、同時実行制御違反が発生した場合、ユーザーの変更は失われます、ため、同時に別のユーザーの変更が上書きされるとします。 具体的には、GridView が編集済みの状態に返され、現在のデータベースのデータにバインドします。 これにより、他のユーザーの変更により、以前は表示されませんでした、GridView の行が更新されます。 さらに、`UpdateConflictMessage`ラベル コントロールをユーザーに説明が発生します。 このイベントのシーケンス図 19 の詳細を示します。


[![ユーザーの更新プログラムは、同時実行制御違反の表面に失われます](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**図 19**: A のユーザーの更新プログラムは、同時実行制御違反の表面に失われます ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image55.png))。


> [!NOTE]
> または、GridView 編集済みの状態に返すときではなく残る可能性があります、GridView の編集状態で設定して、`KeepInEditMode`プロパティの渡されたで`GridViewUpdatedEventArgs`オブジェクトを true にします。 この方法で実行する場合、必ず GridView にデータを再バインドする (呼び出すことによってその`DataBind()`メソッド)、その他のユーザーの値が編集インターフェイスに読み込まれるようにします。 このチュートリアルでダウンロード可能なコードが次の 2 行のコードの`RowUpdated`イベント ハンドラーがコメント アウトされています。 同時実行制御違反の後に、次の行のコードに、GridView が編集モードのままのコメントを解除だけです。


## <a name="responding-to-concurrency-violations-when-deleting"></a>削除するときに、同時実行制御違反への応答

DB の直接パターンでは、同時実行制御違反が発生した場合に発生する例外はありません。 代わりに、データベース ステートメントだけが影響を受けないレコードを WHERE 句は、任意のレコードと一致しません。 BLL で作成したデータ変更メソッドのすべては、正確に 1 つのレコードが影響を受けるかどうかを示すブール値を返すように設計されています。 そのため、レコードを削除するときに、同時実行制御違反が発生したかどうかを判断することができますを考察 BLL の戻り値`DeleteProduct`メソッド。

BLL メソッドの戻り値を通じて ObjectDataSource の後のレベルのイベント ハンドラーで調べることができます、`ReturnValue`のプロパティ、`ObjectDataSourceStatusEventArgs`イベント ハンドラーに渡されるオブジェクト。 戻り値を決定する必要があるので、`DeleteProduct`メソッド、ObjectDataSource のイベント ハンドラーを作成する必要があります`Deleted`イベント。 `ReturnValue`プロパティの型は`object`でき、`null`例外が発生したかどうかと、値を返す前に、メソッドは中断されました。 そのため、私たちが最初いることを確認、`ReturnValue`プロパティは`null`ブール値です。 このチェックに合格紹介と仮定すると、`DeleteConflictMessage`ラベル コントロールの場合、`ReturnValue`は`false`します。 これは、次のコードを使用して実行できます。


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

同時実行違反が発生した場合、ユーザーの削除要求が取り消されました。 ページと Delete ボタンをクリックしたときに彼に読み込まれるまでの間には、そのレコードのユーザーに発生した変更を示す GridView が更新されます。 このような違反には、ときに、`DeleteConflictMessage`ラベルが表示されるだけです (図 20 を参照してください) の変更点について説明します。


[![同時実行制御違反が発生した場合、ユーザーの削除が取り消されました](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**図 20**: 同時実行制御違反が発生した場合に削除が取り消されたユーザー s ([フルサイズの画像を表示する をクリックします](implementing-optimistic-concurrency-vb/_static/image58.png))。


## <a name="summary"></a>まとめ

同時実行制御違反の営業案件の存在により、複数の場合、すべてのアプリケーションで同時実行ユーザー データを更新または削除します。 場合の 2 人のユーザーは、最終書き込み"wins"で取得したユーザーと同じデータを同時に更新、ときに、変更内容を変更、他のユーザーの上書き、このような違反は考慮されません。 また、開発者は、オプティミスティックおよびペシミスティック同時実行制御を実装できます。 オプティミスティック同時実行制御は、こと同時実行制御違反が低くてもと単に更新プログラムを禁止または削除コマンドは、同時実行制御違反を構成すると仮定します。 ペシミスティック同時実行制御では、違反は多くの場合、更新または削除コマンドの 1 つのユーザーの拒否するには、同時実行が許容されないと仮定します。 ペシミスティック同時実行制御でレコードを更新するはその他のユーザーを変更またはロックされているレコードの削除を防ぐ、ロックがあります。

.NET で型指定されたデータセットは、オプティミスティック同時実行制御をサポートするための機能を提供します。 具体的には、`UPDATE`と`DELETE`データベースへの発行ステートメントは、すべてのテーブルの列を含む、ときに、update または delete がのみ発生する場合は、ユーザーの元のデータと一致するレコードの現在のデータを積み重ねない必要があります。update または delete を実行します。 オプティミスティック同時実行制御をサポートするために DAL を構成すると、BLL メソッドを更新する必要があります。 さらに、ObjectDataSource データ Web コントロールから元の値を取得して BLL に渡しますように BLL 呼び出す ASP.NET ページを構成する必要があります。

このチュートリアルで説明したようには ASP.NET web アプリケーションでオプティミスティック同時実行制御を実装する DAL と BLL を更新して、ASP.NET ページのサポートを追加することがあります。 この追加作業が、時間と労力を賢明に投資収益率がかどうかは、アプリケーションによって異なります。 同時実行ユーザーが、データの更新がある頻度の低い、または更新して、データが異なることから、その同時実行制御はできません、重要な問題。 あれば、ただし、日常的に複数のユーザーのサイトの動作と同じデータで、同時実行制御は無意識の上書きから 1 つのユーザーの更新や削除を防ぐのに役立ちます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](customizing-the-data-modification-interface-vb.md)
> [次へ](adding-client-side-confirmation-when-deleting-vb.md)
