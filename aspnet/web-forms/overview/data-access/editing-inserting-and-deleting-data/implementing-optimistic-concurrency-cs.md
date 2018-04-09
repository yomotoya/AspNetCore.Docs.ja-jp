---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: オプティミスティック同時実行制御 (c#) を実装する |Microsoft ドキュメント
author: rick-anderson
description: 複数のユーザーがデータを編集できるように、web アプリケーションを 2 人のユーザーは編集が同じデータ、同時にリスクがあります。 この tutori にしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: 27441ea9343055b3139468036fc6f201c77667e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="implementing-optimistic-concurrency-c"></a>オプティミスティック同時実行制御 (c#) を実装します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe)または[PDF のダウンロード](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> 複数のユーザーがデータを編集できるように、web アプリケーションを 2 人のユーザーは編集が同じデータ、同時にリスクがあります。 このチュートリアルではこのリスクを処理するオプティミスティック同時実行制御を実装します。


## <a name="introduction"></a>はじめに

データを表示するユーザーのみを許可する web アプリケーションやデータを変更できるユーザー 1 人のユーザーのみが含まれているは、次の 2 つの同時実行ユーザーが 1 つに相互の変更を上書きしてしまうの脅威はありません。 複数のユーザーがデータ更新または削除を許可する web アプリケーションがありますが別の同時実行ユーザーと競合する 1 つのユーザーの変更が発生する可能性です。 なし、同時実行ポリシーを配置するには、2 人のユーザーが同時に 1 つのレコードを編集時に自分が加えた変更をコミットしたユーザー最後よりも優先されます、最初に行われた変更します。

たとえば、こと Jisun と Sam、2 人のユーザーが両方のページにアクセス更新および削除の GridView コントロールを使用して、製品への訪問者を許可されているアプリケーションでは想像してください。 両方は、同時期 GridView [編集] ボタンをクリックします。 Jisun では、製品名を「チャイ」に変更し、[更新] ボタンをクリックします。 最終的には、`UPDATE`を設定すると、データベースに送信されるステートメント*すべて*製品の更新可能なフィールドの (Jisun では、1 つのフィールドのみ更新される場合でも`ProductName`)。 この時点では、データベースは、「チャイ、」飲料、仕入先な液体は、この特定の製品のカテゴリ値がします。 ただし、Sam の画面の GridView であっても、製品名の編集可能な GridView 行"Chai"として。 Jisun の変更がコミットされた後、数秒 Sam は調味料にカテゴリを更新し、更新プログラムをクリックします。 これは、結果、 `UPDATE` "Chai、"製品名を設定しているデータベースに送信されたステートメント、`CategoryID`対応する飲料カテゴリ ID というようにします。 Jisun の製品名の変更が上書きされました。 図 1 は、視覚的に、この一連のイベントを示しています。


[![2 人のユーザーがレコードを同時に更新があります s れる可能性がある 1 つのユーザーの変更を他の s を上書き](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**図 1**: ときに 2 つのユーザーに同時に更新、レコードが潜在的な 1 つのユーザーの変更を上書きする他のため ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image3.png))


同様に、2 人のユーザーは、ページにアクセスは、1 人のユーザーが別のユーザーによって削除されるときにレコードの更新中必要があります。 または、間、ユーザーがページを読み込むときに、[削除] ボタンをクリックすると、別のユーザーが変更レコードの内容。

3 つ[同時実行制御](http://en.wikipedia.org/wiki/Concurrency_control)使用できる方法。

- **何もしない**-使用の同時実行ユーザーが同じレコードを変更する場合は、win (既定の動作)、最後のコミット
- [**オプティミスティック同時実行制御**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -をもありますが同時実行は、このような競合が発生しません時間の大部分で競合し、そのため、場合は、競合が発生した場合、単にユーザーに通知を想定する、変更。別のユーザーには、同じデータが変更されたために、保存できません。
- **ペシミスティック同時実行制御**の同時実行の競合は一般的であり別のユーザーの同時処理のため、変更が保存されなかった述べるユーザーがいますを許容しないことを前提としていますそのため、1 つのユーザーは、レコードを更新するを起動するときにロック。、編集したり、ユーザーが、その変更がコミットされるまで、そのレコードを削除すると、他のユーザーの回避

すべてチュートリアルのこれまでが使用される既定の同時実行の解決方法 - つまり、win の最後の書き込みできるようにしました。 このチュートリアルではオプティミスティック同時実行制御を実装する方法について確認します。

> [!NOTE]
> ペシミスティック同時実行の例では、このチュートリアルのシリーズ見てはありません。 ペシミスティック同時実行制御はなどのロックのためはほとんど使用しない場合正しく開放、他のユーザーがデータを更新するを防ぐことができます。 たとえば、ユーザーが編集するためのレコードをロックし、ロックの解除する前に、その日のままにし、他のユーザーされませんを元のユーザーが取得しての更新を完了するまで、そのレコードを更新できません。 したがって、ペシミスティック同時実行制御が使用されている場合、一般的がありますタイムアウトに達すると、キャンセル、ロックが。 チケット販売 web サイト、短い期間の特定の座席の場所をロックする、ユーザーは、注文プロセスを完了するまでは、ペシミスティック同時実行制御の例を示します。


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>手順 1: が実装されているオプティミスティック同時実行制御方法を見て、

オプティミスティック同時実行制御は、更新または削除されたレコードの同じ値は、更新または削除プロセスが開始されたときと同様にあることを確認することによって機能します。 たとえば、編集可能な GridView [編集] ボタンをクリックすると、レコードの値をデータベースからの読み取りし、がテキスト ボックスおよびその他の Web コントロールに表示されます。 元の値が GridView で保存されます。 後で、ユーザーは、自分が加えた変更は、[更新] ボタンをクリックして、元の値と新しい値は送信ビジネス ロジック層とデータ アクセス層までされます。 データ アクセス層には、ユーザーが編集を開始した元の値が、データベースにまだ値を同一の場合のみ、レコードを更新すること SQL ステートメントを実行する必要があります。 図 2 は、このイベントのシーケンスを示しています。


[![正常に更新または削除は、元の値を現在のデータベースの値と同じにする必要があります。](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**図 2**: For Update または Delete の成功を元の値必要がある値に等しく、現在データベースに ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image6.png))


オプティミスティック同時実行制御を実装するためのさまざまな方法があります (を参照してください[Peter A. 作成](http://peterbromberg.net/)の[Optmistic 同時実行更新ロジック](http://www.eggheadcafe.com/articles/20050719.asp)いくつかのオプションについて簡単に説明用)。 ADO.NET の型指定されたデータセットは、チェック ボックスのティックだけで構成できます。 1 つの実装を提供します。 型指定されたデータセットに TableAdapter を拡張、TableAdapter のオプティミスティック同時実行制御を有効にする`UPDATE`と`DELETE`のすべての元の値の比較を含めるようにステートメントを`WHERE`句。 次`UPDATE`ステートメントでは、たとえば、更新プログラム名と製品の価格データベースの現在の値が GridView でレコードを更新するときに取得された元の値に等しい場合のみです。 `@ProductName`と`@UnitPrice`パラメーターに対し、ユーザーが入力した新しい値を含める`@original_ProductName`と`@original_UnitPrice`もともと読み込まれた GridView [編集] ボタンがクリックしてされたときに値が含まれます。


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> これは、`UPDATE`読みやすくするため、ステートメントを簡略化されています。 実際には、`UnitPrice`で確認してください、`WHERE`句はからさらに複雑になります`UnitPrice`含めることができる`NULL`s および確認しているとき`NULL = NULL`常に False を返します (代わりに使用する必要があります`IS NULL`)。


使用して、別の基になるだけでなく`UPDATE`メソッドの直接ステートメントでは、オプティミスティック同時実行も変更されます、DB の署名を使用する TableAdapter を構成します。 最初のチュートリアルでは、メッセージの取り消し[*データ アクセス レイヤーを作成する*](../introduction/creating-a-data-access-layer-cs.md)、値の入力パラメーターとしての DB 直接メソッドがされたスカラーの一覧を指定するもの (厳密に型指定された DataRow ではなくまたはDataTable のインスタンスの場合)。 オプティミスティック同時実行制御、直接の DB を使用するときに`Update()`と`Delete()`メソッドにも、元の値の入力パラメーターが含まれます。 BLL バッチを使用するためのコードがさらに、パターンを更新 (、`Update()`をスカラー値ではなく、Datarow とデータ テーブルを受け取るメソッド オーバー ロード) も変更する必要があります。

はなく既存拡張よりも DAL の Tableadapter みましょうオプティミスティック同時実行性 (BLL に対応するための変更が必要となる) を使用する代わりに新しいデータセットを作成型指定された名前付き`NorthwindOptimisticConcurrency`、追加するには`Products`TableAdapter をオプティミスティック同時実行制御を使用します。 次に、ここでは作成、 `ProductsOptimisticConcurrencyBLL` DAL、オプティミスティック同時実行制御をサポートするために適切な変更がビジネス ロジック層クラスです。 この基礎を配置すると後 ASP.NET ページを作成する準備完了になります。

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>手順 2: は、オプティミスティック同時実行制御をサポートする、データ アクセス層の作成

新しい型指定されたデータセットを作成するを右クリックし、`DAL`内のフォルダー、`App_Code`フォルダーという名前の新しいデータセットを追加および`NorthwindOptimisticConcurrency`です。 最初のチュートリアルで説明したとおり、そのために追加されます新しい TableAdapter、TableAdapter 構成ウィザードを自動的に起動する、型指定されたデータセット。 最初の画面で、データベースへの接続を使用して Northwind データベースの同じへの接続を指定するように求めおしている、`NORTHWNDConnectionString`設定から`Web.config`です。


[![同じ Northwind データベースへの接続します。](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**図 3**: 同じ Northwind データベースへの接続 ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image9.png))


次に、データを照会する方法に関してよう求められます。 アドホック SQL ステートメントでは、新しいストアド プロシージャ、または既存のストアド プロシージャです。 元の DAL でアドホック SQL クエリを使用した後は、このオプションをここで使用も。


[![アドホック SQL ステートメントを使用して取得するデータを指定します。](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**図 4**: アドホック SQL ステートメントを使用して取得するデータを指定 ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image12.png))


次の画面を使用して製品情報を取得する SQL クエリを入力します。 みましょうに使用される正確な同じ SQL クエリを使用して、`Products`のすべてを返します、元の DAL から TableAdapter、`Product`製品の仕入先とカテゴリ名と共に列。


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![元の DAL で製品 TableAdapter から同じ SQL クエリを使用します。](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**図 5**: から同じ SQL クエリを使用して、 `Products` DAL に元の TableAdapter ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image15.png))


次の画面上に移動すると、前に、詳細オプション ボタンをクリックします。 この TableAdapter 採用オプティミスティック同時実行制御には、単に"オプティミスティック同時実行制御を使う チェック ボックスを確認します。


[![当座預金でオプティミスティック同時実行制御を有効にする、&quot;オプティミスティック同時実行制御を使用して&quot; チェック ボックス](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**図 6**: オプティミスティック同時実行制御を使用するオプティミスティック同時実行制御 チェック ボックスをオンを有効にする ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image18.png))


最後に、TableAdapter が、データ アクセス パターンは、DataTable にデータし、; DataTable を返すの両方を使用することを示しますDB の直接メソッドを作成することも示します。 名前付け規則をミラー化するために、元の DAL で使用、GetProducts に GetData の戻り値は、メソッド名、DataTable パターンを変更します。


[![すべてのデータ アクセス パターンを利用して、TableAdapter があります。](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**図 7**: TableAdapter 利用してすべてのデータ アクセス パターンがある ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image21.png))


ウィザードを完了すると、データセット デザイナーが含まれます、厳密に型指定された`Products`DataTable および TableAdapter。 DataTable の名前を変更するには、しばらく時間かかる`Products`に`ProductsOptimisticConcurrency`DataTable のタイトル バーを右クリックして、コンテキスト メニューの [名前の変更] を選択して行うことができます。


[![型指定された DataSet に DataTable と TableAdapter が追加されました](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**図 8**: DataTable と型指定されたデータセットに TableAdapter が追加されました ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image24.png))


間の相違点を表示する、`UPDATE`と`DELETE`間でクエリを実行、 `ProductsOptimisticConcurrency` TableAdapter (をオプティミスティック同時実行制御を使用) と、製品 TableAdapter (をしない)、TableAdapter をクリックし、[プロパティ] ウィンドウに移動します。 `DeleteCommand`と`UpdateCommand`プロパティの`CommandText`サブプロパティ DAL の更新または削除に関連するメソッドが呼び出されたときに、データベースに送信される実際の SQL 構文を確認できます。 `ProductsOptimisticConcurrency` TableAdapter、`DELETE`ステートメントを使用します。


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

一方、`DELETE`元の DAL の製品 TableAdapter のステートメントがはるかに簡単。


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

わかります、`WHERE`句、`DELETE`オプティミスティック同時実行制御を使用して、TableAdapter のステートメントには、それぞれの間の比較が含まれています、`Product`テーブルの既存の列の値と元の値にはGridView (DetailsView または FormView) が最後に作成します。 以外のすべてのフィールドから`ProductID`、`ProductName`と`Discontinued`が`NULL`値、追加のパラメーターおよびチェックとおり正確に比較する`NULL`の値が、`WHERE`句。

おされませんを追加する、追加のデータ テーブル、オプティミスティック同時実行制御が有効なデータセットこのチュートリアルでは、ように、ASP.NET ページだけは、更新および製品情報を削除します。 ただし、行う必要がありますを追加する、`GetProductByProductID(productID)`メソッドを`ProductsOptimisticConcurrency`TableAdapter。

これを実現する TableAdapter のタイトル バーを右クリックし (上の領域の右側、`Fill`と`GetProducts`メソッド名) とクエリの追加、コンテキスト メニューをクリックします。 これにより、TableAdapter クエリの構成ウィザードが起動します。 作成することを選択、TableAdapter の初期構成を使用すると、`GetProductByProductID(productID)`アドホック SQL ステートメントを使用するメソッド (図 4 を参照してください)。 以降、`GetProductByProductID(productID)`メソッドは、特定の製品に関する情報を返します、次のクエリがあることを示す、`SELECT`行を返しますの種類のクエリを実行します。


[![クエリの種類としてのマークを付ける、&quot;選択行が返されます&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**図 9**: クエリ タイプとしてマークする"`SELECT`行を返す"([フルサイズのイメージを表示する をクリックします](implementing-optimistic-concurrency-cs/_static/image27.png))。


次の画面で使用するには、TableAdapter の既定のクエリが読み込み済みで、SQL クエリのように求められます。 句に含める既存のクエリを補強`WHERE ProductID = @ProductID`図 10 に示すように、します。


[![追加、WHERE 句をあらかじめ読み込まれたクエリが特定の製品レコードを返す](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**図 10**: 追加、 `WHERE` Pre-Loaded クエリで返される特定の製品レコードに句 ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image30.png))


最後に、生成されたメソッド名を変更`FillByProductID`と`GetProductByProductID`です。


[![FillByProductID および GetProductByProductID メソッドの名前を変更します。](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**図 11**: メソッドの名前を変更`FillByProductID`と`GetProductByProductID`([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image33.png))


このウィザードを使用して完全な TableAdapter が含まれていますデータを取得するための 2 つの方法には:`GetProducts()`を返す*すべて*製品; と`GetProductByProductID(productID)`、指定した製品が返されます。

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>手順 3: オプティミスティック同時実行対応 DAL のビジネス ロジック層を作成します。

既存`ProductsBLL`クラスには、バッチ更新と DB 直接パターンを使用する例です。 `AddProduct`メソッドおよび`UpdateProduct`両方オーバー ロードに渡す、バッチ更新パターンを使用する、 `ProductRow` TableAdapter の Update メソッドのインスタンス。 `DeleteProduct`メソッド、その一方で、パターンを使用して、DB 直接、呼び出す TableAdapter の`Delete(productID)`メソッドです。

新しい`ProductsOptimisticConcurrency`TableAdapter、DB の直接メソッドで元の値が渡すもする必要があるようになりました。 たとえば、`Delete`今すぐメソッドでは 10 個の入力パラメーターが必要です元`ProductID`、 `ProductName`、 `SupplierID`、 `CategoryID`、 `QuantityPerUnit`、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、 `ReorderLevel`、。および`Discontinued`です。 これらの追加入力パラメーターの値が使用されます`WHERE`の句、`DELETE`ステートメントの場合は、データベースの現在の値が元の最大マップは、指定されたレコードを削除するだけで、データベースに送信します。

Tableadapter のメソッドのシグネチャを while`Update`バッチ更新パターンで使用する方法を変更していない、オリジナルと新しい値を記録するために必要なコードにします。 したがって、既存のオプティミスティック同時実行対応 DAL を使用しようとするのではなく`ProductsBLL`クラス、新しい、DAL を使用するための新しいビジネス ロジック層クラスを作成してみましょう。

という名前のクラスを追加`ProductsOptimisticConcurrencyBLL`を`BLL`内のフォルダー、`App_Code`フォルダーです。


![ProductsOptimisticConcurrencyBLL クラス BLL フォルダーを追加します。](implementing-optimistic-concurrency-cs/_static/image34.png)

**図 12**: 追加、 `ProductsOptimisticConcurrencyBLL` BLL フォルダーにクラス


次に、次のコードを追加、`ProductsOptimisticConcurrencyBLL`クラス。


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

使用して、注意してください`NorthwindOptimisticConcurrencyTableAdapters`クラス宣言の開始前のステートメント。 `NorthwindOptimisticConcurrencyTableAdapters`名前空間を含む、`ProductsOptimisticConcurrencyTableAdapter`クラスで、DAL のメソッドを提供します。 また、クラス宣言の前にことがわかります、`System.ComponentModel.DataObject`属性は、ObjectDataSource ウィザードのドロップダウン リストにこのクラスを追加する Visual Studio に指示します。

`ProductsOptimisticConcurrencyBLL`の`Adapter`プロパティのインスタンスへのクイック アクセスを提供する、`ProductsOptimisticConcurrencyTableAdapter`クラス、および元 BLL クラスで使用されているパターンに従います (`ProductsBLL`、`CategoriesBLL`など)。 最後に、`GetProducts()`メソッドを呼び出すだけに、DAL の`GetProducts()`メソッドを返します、`ProductsOptimisticConcurrencyDataTable`オブジェクトが設定、`ProductsOptimisticConcurrencyRow`データベース内の各製品レコードのインスタンス。

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>オプティミスティック同時実行制御と DB 直接パターンを使用して商品を削除します。

オプティミスティック同時実行制御を使用する DAL に対して DB 直接パターンを使用する場合メソッドが、新しいおよび元の値を渡される必要があります。 削除すると、値がない新しい、そのために、元の値のみを渡す必要があります。 当社 BLL で次に、お必要がありますすべてそのまま使用元のパラメーターの入力パラメーターとして。 ここが、`DeleteProduct`メソッドで、`ProductsOptimisticConcurrencyBLL`クラス DB 直接メソッドを使用します。 これは、このメソッドをすべての 10 個の製品データ フィールドの入力パラメーターとしてでは、次のコードに示すように、DAL にこれらを渡す必要があることを意味します。


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

元の値の GridView (または DetailsView または FormView) に読み込まれた最後それらの値には、ユーザーは、[削除] ボタンをクリックすると、データベース内の値とは異なります場合、`WHERE`句が任意のデータベース レコードおよびレコードがないと一致しません。影響があります。 そのため、TableAdapter の`Delete`メソッドが返す`0`と BLL の`DeleteProduct`メソッドが返す`false`です。

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>オプティミスティック同時実行制御をバッチ更新パターンを使用して製品の更新

述べたとおり、TableAdapter の`Update`バッチ更新パターンのメソッドは、オプティミスティック同時実行制御が採用されているかどうかに関係なく同じメソッド シグネチャを持ちます。 つまり、`Update`メソッドで DataRow、Datarow、DataTable、または型指定されたデータセットの配列を受け取ります。 元の値を指定するための追加の入力パラメーターがありません。 これには、DataTable の追跡元と変更された値をその DataRow(s) のためです。 DAL が発行したとき、`UPDATE`ステートメントでは、`@original_ColumnName`一方は DataRow の元の値、パラメーターが設定、 `@ColumnName` DataRow の変更された値はパラメーターが入力されます。

`ProductsBLL`クラス (、元の非オプティミスティック同時実行 DAL を使用)、バッチ更新パターンを使用して、コード実行の次の一連のイベントの製品情報を更新する場合。

1. 現在のデータベース製品情報を読み取り、 `ProductRow` TableAdapter を使用してインスタンス`GetProductByProductID(productID)`メソッド
2. 新しい値を割り当てる、`ProductRow`手順 1. のインスタンス
3. 呼び出す TableAdapter の`Update`に渡して、メソッド、`ProductRow`インスタンス

この一連の手順、ただしはオプティミスティック同時実行制御をサポート正しくされません、`ProductRow`の設定、DataRow で使用される元の値に現在存在するものがあり、データベースから直接ステップ 1 が設定されます、データベース、および編集のプロセスの開始時の GridView にバインドされているされません。 代わりに、使用して、オプティミスティック同時実行対応 DAL、ときに必要がありますを変更する、`UpdateProduct`次の手順を使用するメソッドのオーバー ロードします。

1. 現在のデータベース製品情報を読み取り、 `ProductsOptimisticConcurrencyRow` TableAdapter を使用してインスタンス`GetProductByProductID(productID)`メソッド
2. 割り当てる、*元*値を`ProductsOptimisticConcurrencyRow`手順 1. のインスタンス
3. 呼び出す、`ProductsOptimisticConcurrencyRow`インスタンスの`AcceptChanges()`メソッドで、現在の値が「元」の DataRow に指示
4. 割り当てる、*新しい*値を`ProductsOptimisticConcurrencyRow`インスタンス
5. 呼び出す TableAdapter の`Update`に渡して、メソッド、`ProductsOptimisticConcurrencyRow`インスタンス

指定された製品レコードのすべてのデータベースの現在の値の読み取りを手順 1 です。 この手順は不要、`UpdateProduct`を更新するオーバー ロード*すべて*製品列の (これらの値としては、上書き手順 2. で)、ここで、列の値のサブセットのみで渡され、これらのオーバー ロードに不可欠ですが、入力パラメーターです。 元の値に割り当てられた後、`ProductsOptimisticConcurrencyRow`インスタンス、 `AcceptChanges()` DataRow の現在の値で使用するのには、元の値としてマークするメソッドを呼び出す、`@original_ColumnName`内のパラメーター、`UPDATE`ステートメントです。 新しいパラメーター値が次に、割り当てられた、 `ProductsOptimisticConcurrencyRow` 、さらに、`Update`メソッドが呼び出され、DataRow に渡します。

次のコードは、`UpdateProduct`製品のすべてのデータを受け入れるオーバー ロードの入力パラメーターとしてのフィールドです。 ここでは、表示されません中に、`ProductsOptimisticConcurrencyBLL`もこのチュートリアルが含まれて、ダウンロードに含まれているクラス、`UpdateProduct`だけ、製品の名前と価格を入力パラメーターとして受け取るオーバー ロードします。


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>手順 4: 元と新しい値が ASP.NET ページからメソッドを渡し、BLL

DAL BLL 完了とは、システムに組み込まれているオプティミスティック同時実行制御ロジックを使用する ASP.NET ページを作成します。 具体的には、Web コントロール (GridView、DetailsView、またはフォーム ビュー) のデータは、元の値と、ObjectDataSource をビジネス ロジック層の両方の値のセットを渡す必要があることを忘れないでください。 さらに、ASP.NET ページは同時実行制御違反を適切に処理するように構成する必要があります。

開いて開始、 `OptimisticConcurrency.aspx`  ページで、`EditInsertDelete`フォルダーと、デザイナーの設定への GridView の追加、`ID`プロパティを`ProductsGrid`です。 という名前の新しい ObjectDataSource を作成することを選択、GridView のスマート タグから`ProductsOptimisticConcurrencyDataSource`です。 この ObjectDataSource オプティミスティック同時実行制御をサポートする DAL を使用するので、構成を使用するよう、`ProductsOptimisticConcurrencyBLL`オブジェクト。


[![ObjectDataSource 使用 ProductsOptimisticConcurrencyBLL オブジェクトがあります。](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**図 13**: ObjectDataSource に使用されている、`ProductsOptimisticConcurrencyBLL`オブジェクト ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image37.png))


選択、 `GetProducts`、 `UpdateProduct`、および`DeleteProduct`ウィザードでのドロップダウン リストからメソッドです。 UpdateProduct メソッドのすべての製品のデータ フィールドを受け入れるオーバー ロードを使用します。

## <a name="configuring-the-objectdatasource-controls-properties"></a>ObjectDataSource コントロールのプロパティを構成します。

ウィザードを完了すると、ObjectDataSource の宣言型マークアップは、次のようになります。


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

ご覧のように、`DeleteParameters`コレクションが含まれています、`Parameter`で 10 個の入力パラメーターの各インスタンス、`ProductsOptimisticConcurrencyBLL`クラスの`DeleteProduct`メソッドです。 同様に、`UpdateParameters`コレクションが含まれています、`Parameter`で、入力パラメーターの各インスタンス`UpdateProduct`です。

ObjectDataSource のチュートリアルについては、以前データの変更に関係する、削除お`OldValuesParameterFormatString`この時点で、このプロパティは、BLL メソッドが、古い (または元) 値で渡されるだけでなく、新しい値を受け取ることを示すためのプロパティです。 さらに、このプロパティの値では、元の値の入力パラメーターの名前を示します。 BLL に元の値で渡している、ため*いない*このプロパティを削除します。

> [!NOTE]
> 値、`OldValuesParameterFormatString`プロパティは、元の値を期待する BLL 内の入力パラメーター名にマップする必要があります。 これらのパラメーターという名前を付けてため`original_productName`、`original_supplierID`など、おくことができます、`OldValuesParameterFormatString`プロパティの値として`original_{0}`です。 、ただし、BLL メソッドの入力パラメーターが持っているかどうかのような名前`old_productName`、`old_supplierID`など、更新する必要があります、`OldValuesParameterFormatString`プロパティを`old_{0}`です。


BLL メソッドに元の値を正しく渡す ObjectDataSource の順序で実行する必要がある 1 つの最後のプロパティ設定があります。 ObjectDataSource が、 [ConflictDetection プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx)に割り当て可能な[2 つの値のいずれかの](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -既定値です。BLL メソッドの元の入力パラメーターには、元の値を送信しません
- `CompareAllValues` は、BLL メソッドには、元の値を送信オプティミスティック同時実行制御を使用する場合は、このオプションを選択します。

設定に少し時間を取って、`ConflictDetection`プロパティを`CompareAllValues`です。

## <a name="configuring-the-gridviews-properties-and-fields"></a>GridView のプロパティおよびフィールドを構成します。

ObjectDataSource のプロパティが正しく構成されている、みましょう GridView を設定します。 最初に、編集および削除をサポートする GridView ので、GridView のスマート タグから編集を有効にして削除を有効にするチェック ボックスをクリックします。 これにより、CommandField が追加されますが`ShowEditButton`と`ShowDeleteButton`に設定されて`true`です。

バインドするとき、 `ProductsOptimisticConcurrencyDataSource` ObjectDataSource、GridView の各製品のデータ フィールドのフィールドが含まれています。 このような GridView を編集できますが、ユーザー エクスペリエンスは、決して許容可能です。 `CategoryID`と`SupplierID`BoundFields がテキスト ボックスとしてレンダリングされます ID 番号として、適切なカテゴリと仕入先を入力する必要はありません。 行われません、数値フィールドおよび製品の名前が指定されているしこと単価、在庫数、順序、および並べ替えレベルの値の単位は両方の適切な数値とより大きいか等しいことを確認するには、なし検証コントロールの書式設定0 を返します。

説明したよう、*検証コントロールを編集および挿入するインターフェイスを追加する*と*データ変更インターフェイスのカスタマイズ*チュートリアルについては、ユーザー インターフェイスで、カスタマイズできますTemplateFields、BoundFields に置き換えています。 この GridView とその編集インターフェイスを変更しました、次の方法で。

- 削除、 `ProductID`、 `SupplierName`、および`CategoryName`BoundFields
- 変換、 `ProductName` TemplateField に BoundField RequiredFieldValidation コントロールを追加します。
- 変換、`CategoryID`と`SupplierID`BoundFields TemplateFields にテキスト ボックスではなく DropDownLists 使用する編集のインターフェイスを調整します。 これら TemplateFields' で`ItemTemplates`、`CategoryName`と`SupplierName`データ フィールドが表示されます。
- 変換、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`BoundFields TemplateFields および CompareValidator コントロールを追加します。

前のチュートリアルでこれらのタスクを実行する方法を既にについて説明しました、ため、最終的な宣言型の構文は、ここに一覧表示し、プラクティスとして、実装のままにだけ行います。


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

私たちは非常に完全に実際の例を持つの近くにします。 ただし、につれてし、問題が発生するいくつかの微妙ながあります。 さらに、同時実行制御違反が発生したときに、ユーザーに警告するいくつかのインターフェイスも必要があります。

> [!NOTE]
> Web コントロールのデータで (これは、BLL に渡されて) ObjectDataSource に元の値を正しく渡すには、ことが重要を GridView の`EnableViewState`プロパティに設定されている`true`(既定)。 ビュー ステートを無効にした場合、元の値がポストバック時に失われます。


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>ObjectDataSource を正しいの元の値を渡す

GridView の構成方法に関する問題のいくつかあります。 場合、ObjectDataSource`ConflictDetection`プロパティに設定されている`CompareAllValues`(としては、マイクロソフト) ときに、ObjectDataSource の`Update()`または`Delete()`GridView (または DetailsView または FormView) によって呼び出されるメソッド、コピー、ObjectDataSource を試みると、GridView の元の値に、適切な`Parameter`インスタンス。 参照図 2 のこのプロセスのグラフィック表示します。

具体的には、GridView の元の値には、毎回データが GridView にバインドが双方向データ バインド ステートメント内の値が割り当てられます。 そのため、これが双方向データ バインドを使用してすべての必要な元の値がキャプチャされると、変換可能な形式で提供されることに不可欠です。

これが重要な理由を表示するには、ブラウザーでのページにアクセスする時点を取得します。 予想どおり、左端の列の編集および削除ボタンでは、各製品が GridView に一覧表示します。


[![製品が GridView で一覧表示されます。](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**図 14**: GridView に、製品が表示されます ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image40.png))


他の製品の削除 ボタンをクリックした場合、`FormatException`がスローされます。


[![FormatException 製品結果を削除しようとしています。](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**図 15**: での製品の結果を削除しようとして、 `FormatException` ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image43.png))


`FormatException` 、ObjectDataSource が元の読み取りしようとしたときに発生`UnitPrice`値。 以降、`ItemTemplate`が、`UnitPrice`通貨として書式設定 (`<%# Bind("UnitPrice", "{0:C}") %>`)、$19.95 などの通貨記号が含まれています。 `FormatException` 、ObjectDataSource が、この文字列に変換しようとしています。 ときに発生する`decimal`です。 この問題を回避するためには、いくつかのオプションがあります。

- 通貨の書式設定を削除、`ItemTemplate`です。 つまり、使用する代わりに`<%# Bind("UnitPrice", "{0:C}") %>`、単に使用`<%# Bind("UnitPrice") %>`です。 以下のコマンドは、価格の形式が不要になったことです。
- 表示、`UnitPrice`の通貨として書式設定、`ItemTemplate`が使用して、`Eval`これを実現するキーワードです。 注意してください`Eval`一方向のデータ バインドを実行します。 提供する必要があります、`UnitPrice`双方向データ バインド ステートメントでは、まだ必要がありますので、元の値の値、 `ItemTemplate`、この配置できるラベル Web コントロールであるが、`Visible`プロパティに設定されている`false`です。 ItemTemplate に次のマークアップを使用できます。


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- 通貨の書式設定を削除、`ItemTemplate`を使用して、`<%# Bind("UnitPrice") %>`です。 Gridview の`RowDataBound`Label Web が制御をイベント ハンドラー、プログラムでアクセス、`UnitPrice`値が表示され、設定、`Text`プロパティを書式設定されたバージョンです。
- ままにして、`UnitPrice`通貨として書式設定します。 Gridview の`RowDeleting`イベント ハンドラー、元の既存の置換`UnitPrice`を使用して、実際の 10 進値を持つ値 ($19.95)`Decimal.Parse`です。 ようなものを実現する方法を説明しました、`RowUpdating`内のイベント ハンドラー、 [*処理 BLL-DAL レベルでの例外および ASP.NET ページ*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)チュートリアルです。

2 番目の方法で移動すると選択した例のいる非表示のラベル Web を追加するコントロール`Text`プロパティが双方向にデータ バインド、書式設定されていない`UnitPrice`値。

この問題を解決する方法、製品の削除 ボタンをもう一度クリックしてください。 この時間が表示されます、 `InvalidOperationException` 、ObjectDataSource が BLL を起動しようとしたときに`UpdateProduct`メソッドです。


[![ObjectDataSource が送信する必要がある入力パラメーターを持つメソッドを見つけることができません。](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**図 16**: ObjectDataSource が送信する必要がある入力パラメーターを持つメソッドを見つけることができません ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image46.png))


例外のメッセージを見ると、明確では、ObjectDataSource を呼び出し、BLL が要求している`DeleteProduct`を含むメソッド`original_CategoryName`と`original_SupplierName`パラメーターを入力します。 これは、ため、`ItemTemplate`の s、`CategoryID`と`SupplierID`TemplateFields には現在を持つ双方向のバインド ステートメントが含まれて、`CategoryName`と`SupplierName`データ フィールドです。 代わりを含める必要があります`Bind`ステートメントと、`CategoryID`と`SupplierID`データ フィールドです。 これを実現するには、既存のバインド ステートメントを置き換えます`Eval`ステートメント、し、追加の非表示のラベル コントロール`Text`プロパティにバインドされます、`CategoryID`と`SupplierID`ように、双方向データ バインドを使用してデータ フィールド以下に：


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

これらの変更が正常に削除し、製品情報を編集することがようになりました。 手順 5 では、同時実行制御違反が検出されていることを確認する方法を紹介します。 ここでは、数分後に更新してみてください行い期待どおりに動作を更新し、単一のユーザーを削除することを確認するいくつかのレコードを削除します。

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>手順 5: オプティミスティック同時実行サポートのテスト

同時実行制御違反が検出された (ではなくデータが上書きされる無条件で結果として得られる) していることを確認するためには、このページに次の 2 つのブラウザー ウィンドウを開く必要があります。 両方のブラウザー インスタンス Chai の編集 ボタンをクリックします。 次に、1 つだけのブラウザーで「チャイ」に名前を変更して更新 をクリックします。 更新プログラムは、成功し、GridView を新しい製品名として「チャイ」で、編集済み状態に戻す必要があります。

その他のブラウザー ウィンドウ インスタンスで、ただし、製品名 テキスト ボックスであっても、"Chai"。 この 2 つ目のブラウザー ウィンドウを更新、`UnitPrice`に`25.00`です。 オプティミスティック同時実行サポートのない 2 つ目のブラウザー インスタンスでの更新 をクリックして変更、製品名戻る"Chai"、最初のブラウザー インスタンスによって行われた変更を上書きします。 採用されているオプティミスティック同時実行を使って、ただし、2 番目のブラウザー インスタンスで、[更新] ボタンをクリックすると結果で、 [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx)です。


[![同時実行制御違反が検出されると、DBConcurrencyException がスローされます。](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**図 17**: ときに、同時実行制御違反が検出されると、`DBConcurrencyException`がスローされます ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image49.png))


`DBConcurrencyException` DAL のバッチ更新パターンが使用率が低い場合にスローされるだけです。 DB 直接パターンが例外を発生させない、影響を受けた行がないことだけを示します。 これを示すためには、編集済みの状態に両方のブラウザー インスタンスの GridView を返します。 次に、最初のブラウザー インスタンスで 編集 ボタンをクリックして"Chai"に戻す「チャイ」から、製品名の変更し、更新 をクリックします。 2 番目のブラウザー ウィンドウでは、Chai の削除 ボタンをクリックします。

Delete をクリックすると、ページがポストバック、GridView 呼び出します ObjectDataSource の`Delete()`メソッド、および、ObjectDataSource 呼び出し`ProductsOptimisticConcurrencyBLL`クラスの`DeleteProduct`メソッド、元の値を渡します。 元の`ProductName`「チャイ」現在と一致しません。 これは、2 番目のブラウザー インスタンスの値`ProductName`データベース内の値。 したがって、`DELETE`データベース内のレコードが存在しないため、データベースへの発行ステートメントが 0 の行に影響する、`WHERE`句を満たします。 `DeleteProduct`メソッドを返します。 `false` ObjectDataSource のデータが GridView にバインドし直すとします。

エンドユーザーの視点からフラッシュする画面の原因となったチャイ 2 つ目のブラウザー ウィンドウの [削除] ボタンをクリックするに戻るには、製品がまだ残っている"Chai"(製品名の変更、最初のブラウザーによって行われたとして記載されているようになりましたインスタンス)。 ユーザー [削除] ボタンをもう一度クリックすると、削除が正常に終了 GridView の元として`ProductName`値 ("Chai") ようになりましたが一致する、データベース内の値。

このような場合の両方では、ユーザー操作は、理想かけ離れていることです。 ユーザーの具体的な詳細を表示する明確にしたくありません、`DBConcurrencyException`バッチ更新パターンを使用する場合は例外です。 され、ユーザー コマンドが失敗しましたが、正確な理由を示す値がありませんでした。 と DB 直接パターンを使用したときの動作が多少複雑になる場合は。

これら 2 つの問題を解決するには更新または削除に失敗しました理由にについてを説明するページ上のラベルの Web コントロールを作成できます。 バッチ更新パターンを判断できるかどうか、`DBConcurrencyException`必要に応じて、警告のラベルを表示する GridView の後のレベルのイベント ハンドラーで例外が発生しました。 直接 DB メソッドの BLL メソッドの戻り値を確認できます (これは`true`1 つの行が影響を受けた場合`false`それ以外の場合) し、必要に応じて情報メッセージを表示します。

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>手順 6: は情報メッセージを追加して、同時実行制御違反が発生した場合に表示する方法

同時実行制御違反が発生した場合の動作は、DAL のバッチ更新または DB 直接パターンが使用されるかどうかによって変わります。 このチュートリアルでは、更新および、DB 直接使用するパターンを削除するために使用されているバッチ更新パターンと、両方のパターンを使用します。 、開始するには、削除またはデータを更新しようとするときに、同時実行制御違反が発生したことを説明するページに 2 つの Label Web コントロールを追加してみましょう。 ラベル コントロールの設定`Visible`と`EnableViewState`プロパティを`false`です。 これにより、それらの特定のページにアクセス where 点を除いて、各ページのアクセス時に非表示にすること、`Visible`プロパティ プログラムで`true`です。


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

設定に加えて、 `Visible`、 `EnabledViewState`、および`Text`プロパティも設定しました、`CssClass`プロパティを`Warning`、それが原因で、ラベルを大きな、赤、斜体、太字のフォントで表示されるのです。 この CSS`Warning`クラスが定義され、Styles.css に追加されたに戻り、 *、イベントに関連付けられている挿入、更新、および削除を確認する*チュートリアルです。

これらのラベルを追加すると、Visual Studio のデザイナーは図 18 にようになります。


[![2 つのラベル コントロールがページに追加されました](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**図 18**: 2 つのラベル コントロールに追加されたページ ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image52.png))


これらのラベル Web 所定の制御、私たちは同時実行制御違反が発生すると、箇所、適切なラベルを決定する方法を確認する準備が`Visible`プロパティに設定することができます`true`、情報メッセージを表示します。

## <a name="handling-concurrency-violations-when-updating"></a>更新するときに、同時実行制御違反を処理

まず、バッチ更新パターンを使用する場合は、同時実行制御違反を処理する方法を見てみましょう。 バッチには、このような違反のパターンの原因を更新するため、`DBConcurrencyException`例外をスローするコードを決定する、ASP.NET ページを追加する必要があるかどうか、`DBConcurrencyException`更新プロセス中に例外が発生しました。 開始レコードの編集時に、別のユーザーの間で同じデータが変更するため、その変更は保存されませんでした、ユーザーについてのみ説明メッセージが表示する必要がありますので、お場合、し、[更新] ボタンがクリックされたときにします。

示したように、*処理 BLL-DAL レベルでの例外および ASP.NET ページ*チュートリアルで、このような例外を検出し、データ Web コントロールのポスト レベルのイベント ハンドラーで抑制します。 したがって、gridview のイベント ハンドラーを作成する必要があります`RowUpdated`場合をチェックするイベント、`DBConcurrencyException`例外がスローされました。 このイベント ハンドラーには、イベント ハンドラーのコードの下に示すように、更新プロセス中に発生したすべての例外への参照が渡されます。


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

Face の`DBConcurrencyException`の例外を除き、このイベント ハンドラーの表示、`UpdateConflictMessage`コントロールにラベルを付けるし、例外が処理されたことを示します。 配置でこのコードでは、レコードを更新するときに、同時実行制御違反が発生したとき、ユーザーの変更は失われます、ので、同時に別のユーザーの変更を上書きするとします。 具体的には、GridView が編集済みの状態に返され、現在のデータベースのデータにバインドします。 これにより、以前非表示が、その他のユーザーの変更を GridView の行が更新されます。 さらに、`UpdateConflictMessage`ラベル コントロールをユーザーに説明が発生します。 このイベントのシーケンスは、図 19 で詳しく説明します。


[![ユーザーの更新プログラムは、同時実行制御違反の表面に失われます](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**図 19**: A のユーザーの更新プログラムは、同時実行制御違反の表面に失われます ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> または、GridView 編集済みの状態に返すときではなくパラメーターは省略できます GridView の編集状態を設定して、`KeepInEditMode`の渡されたプロパティ`GridViewUpdatedEventArgs`オブジェクトを true にします。 この方法を採用する場合、必ず GridView にデータを再バインドする (呼び出すことによってその`DataBind()`メソッド) 編集インターフェイスに、その他のユーザーの値を読み込むようにします。 このチュートリアルをダウンロードのコード内のコードの 2 つの行には、`RowUpdated`コメント アウトされているイベント ハンドラーです。 単に 同時実行制御違反後に編集モードのままに次の行が GridView のコードのコメントを解除します。


## <a name="responding-to-concurrency-violations-when-deleting"></a>削除するときに、同時実行制御違反への応答

DB の直接パターンでは、同時実行制御違反が発生した場合に発生する例外はありません。 代わりに、データベース ステートメントだけに影響しないレコード、WHERE 句は、任意のレコードと一致しません。 BLL で作成したデータ変更メソッドの一部は、正確に 1 つのレコードが影響を受けるかどうかを示すブール値を返すように設計されています。 そのため、レコードを削除するときに同時実行制御違反が発生したかどうか、おできます調べます BLL の戻り値`DeleteProduct`メソッドです。

を通じて ObjectDataSource の後のレベルのイベント ハンドラーで BLL メソッドの戻り値を調べることができます、`ReturnValue`のプロパティ、`ObjectDataSourceStatusEventArgs`イベント ハンドラーに渡されたオブジェクト。 戻り値の決定に注目しているため、`DeleteProduct`メソッド、ObjectDataSource のイベント ハンドラーを作成する必要があります`Deleted`イベント。 `ReturnValue`プロパティの型は`object`でき、`null`かどうかに例外が発生し、値を返す前に、メソッドは中断されました。 そのため、お必要がありますまずいることを確認、`ReturnValue`プロパティは使用されません`null`ブール値です。 表示は、このチェックに合格したと仮定した場合、`DeleteConflictMessage`ラベル コントロールの場合、`ReturnValue`は`false`します。 これは、次のコードを使用して実行できます。


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

同時実行制御違反を発生した場合に、ユーザーの削除要求が取り消されました。 GridView が更新されたページと [削除] ボタンをクリックして彼に読み込まれるまでの間にそのレコードのユーザーに発生した変更を表示します。 このような違反には、ときに、`DeleteConflictMessage`ラベルが表示される、何が起こった (図 20 を参照してください) を説明します。


[![同時実行制御違反が発生した場合、ユーザーの削除が取り消されました](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**図 20**: 同時実行制御違反の発生時に削除が取り消されるユーザー s ([フルサイズのイメージを表示するをクリックして](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>まとめ

同時実行制御違反するための方法が複数、許可されたすべてのアプリケーションに存在同時実行ユーザー データを更新または削除します。 場合は、2 人のユーザーはだれでも最終書き込み"wins"内の取得、同じデータを同時に更新時に上書きして、その他のユーザーの変更に対して、このような違反は考慮されません。 また、開発者は、オプティミスティックかペシミスティック同時実行制御を実装できます。 オプティミスティック同時実行制御には、こと同時実行制御違反が頻繁とだけで更新は許可されていませんか delete コマンドは、同時実行違反を構成するが想定しています。 ペシミスティック同時実行制御では、同時実行違反とは、頻繁に行われる 1 つのユーザーの拒否する更新か delete コマンドが許されないと仮定します。 ペシミスティック同時実行制御でレコードを更新する必要がありますので、ロックのロック中に、レコードの削除の変更または他のユーザーを防ぐします。

.NET の型指定されたデータセットは、オプティミスティック同時実行制御をサポートするための機能を提供します。 具体的には、`UPDATE`と`DELETE`データベースへの発行ステートメントを含むすべてのテーブルの列で、ときに備わり、update または delete がのみ発生する場合は、レコードの現在のデータで、元のデータがユーザーと一致する必要があります。update または delete を実行します。 オプティミスティック同時実行制御をサポートするために DAL を構成すると、BLL メソッドを更新する必要があります。 さらに、BLL を呼び出す ASP.NET ページは、ObjectDataSource、データの Web コントロールから元の値を取得し、それら BLL になるよう構成する必要があります。

このチュートリアルで説明したとおり、ASP.NET web アプリケーションでオプティミスティック同時実行制御を実装するを DAL と BLL を更新し、ASP.NET ページ内のサポートを追加します。 この追加作業が、時間と労力の観点からへの投資をあるかどうかは、アプリケーションによって異なります。 同時実行ユーザーは、データの更新頻度の低いがある場合は、データを更新して互いに異なって場合、同時実行制御問題ではないキー。 あれば、ただし、日常的に複数のユーザー、同じデータの操作は、サイトで、同時実行制御は無意識の上書きから 1 つのユーザーの更新または削除を防ぐのに役立ちます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](customizing-the-data-modification-interface-cs.md)
> [次へ](adding-client-side-confirmation-when-deleting-cs.md)
