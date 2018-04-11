---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: 挿入、更新、および削除 (VB) に関連付けられているイベントを確認する |Microsoft ドキュメント
author: rick-anderson
description: について確認する前に、実行時に、および挿入後に発生するイベントを使用してこのチュートリアルでは、更新、または ASP.NET データ Web コントロールの操作を削除します。 W...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c6a0ff85567b6e41a62feddc58672f38ad0d75b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>挿入、更新、および削除 (VB) に関連付けられたイベントを確認します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe)または[PDF のダウンロード](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> について確認する前に、実行時に、および挿入後に発生するイベントを使用してこのチュートリアルでは、更新、または ASP.NET データ Web コントロールの操作を削除します。 製品フィールドのサブセットのみを更新する編集のインターフェイスをカスタマイズする方法を会いしましょう。


## <a name="introduction"></a>はじめに

組み込みの挿入、編集、または削除、GridView、DetailsView、またはフォーム ビューのコントロールの機能を使用する場合、エンドユーザーが新しいレコードの追加や更新、または既存のレコードを削除するプロセスを完了すると、さまざまな手順が経過します。 説明したよう、[前のチュートリアル](an-overview-of-inserting-updating-and-deleting-data-vb.md)[編集] ボタンが更新し、[キャンセル] ボタンとテキスト ボックスに BoundFields 有効化置き換え GridView の行を編集する際に、します。 エンドユーザーは、データを更新し、更新プログラムをクリックすると、次の手順は、ポストバック時に実行されます。

1. GridView を設定、ObjectDataSource の`UpdateParameters`で編集されたレコードの一意の識別フィールド (を使用して、`DataKeyNames`プロパティ)、ユーザーが入力した値と共に
2. GridView 呼び出します、ObjectDataSource の`Update()`メソッドで、さらに、基になるオブジェクトに適切なメソッドを呼び出します (`ProductsDAL.UpdateProduct`で、前のチュートリアル)
3. 今すぐ更新された変更が含まれている、基になるデータが GridView にバインドし直す

この一連の手順では、中に多数のイベントを発生させる、カスタム ロジックを追加するイベント ハンドラーを作成することを有効にするために必要な場所です。 たとえば、手順 1. GridView の前に`RowUpdating`イベントが発生します。 いくつかの検証エラーがある場合は、この時点では、更新の要求をキャンセルおできます。 ときに、`Update()`メソッドが呼び出され、ObjectDataSource の`Updating`を追加またはのいずれかの値をカスタマイズする機会を提供するイベントの起動、`UpdateParameters`です。 オブジェクトのメソッドの実行、ObjectDataSource が完了した後、ObjectDataSource の基になる`Updated`イベントが発生します。 イベント ハンドラー、`Updated`イベントが影響を受けた行の数と、例外が発生したかどうかなど、更新操作の詳細を調べることができます。 手順 2. GridView の後に最後に、`RowUpdated`イベントの起動; のこのイベントはイベント ハンドラーを実行だけで、更新操作に関する追加情報を確認します。

図 1 は、GridView を更新するときに、この一連のイベントと手順を示しています。 図 1 のイベント パターンが GridView で更新に固有ではありません。 挿入、更新、または GridView からデータを削除すると、DetailsView、またはフォーム ビューには、同じデータ Web コントロールと、ObjectDataSource の両方の前と後のレベルのイベントのシーケンスが precipitates です。


[![一連の前と後のイベント起動 GridView にデータを更新する場合](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**図 1**: A シリーズの前と後イベントには起動時にデータの更新、GridView ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))


このチュートリアルでは、これらのイベントを使用して、組み込みの挿入を拡張するについて確認、更新、および ASP.NET データの機能を削除すると Web を制御します。 製品フィールドのサブセットのみを更新する編集のインターフェイスをカスタマイズする方法を会いしましょう。

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>手順 1: 更新、製品の`ProductName`と`UnitPrice`フィールド

前のチュートリアル編集インターフェイスで*すべて*製品フィールドされなかった読み取り専用を含める必要があります。 -GridView からフィールドを削除する場合と`QuantityPerUnit`データ Web コントロールのデータ更新が設定されていない場合、ObjectDataSource - `QuantityPerUnit` `UpdateParameters`値。 値は、ObjectDataSource を渡しますし`Nothing`に、`UpdateProduct`ビジネス ロジック層 (BLL) のメソッドは、編集済みのデータベース レコードの変更は`QuantityPerUnit`列を`NULL`値。 同様に場合の必須フィールドなど`ProductName`が削除、更新プログラムが失敗し、編集のインターフェイスから、"*列 'ProductName' が null を許容しない*"例外。 この動作の原因が、ObjectDataSource が呼び出しに構成されていたため、`ProductsBLL`クラスの`UpdateProduct`メソッドの各製品フィールドの入力パラメーターが必要です。 したがって、ObjectDataSource の`UpdateParameters`コレクションにパラメーターが含まれている場合、メソッドのそれぞれのパラメーターを入力します。

データ フィールドのサブセットのみを更新するエンド ユーザーを許可する Web コントロールを提供するかどうかは、設定するかプログラムで欠落している必要があります`UpdateParameters`objectdatasource の値`Updating`イベント ハンドラーの作成や、BLL メソッドを呼び出すフィールドのサブセットのみが必要です。 この後者のアプローチを見てみましょう。

具体的には、表示するページを作成しましょうだけ`ProductName`と`UnitPrice`編集可能な GridView のフィールドです。 この GridView の編集用のインターフェイスは 2 つの表示されているフィールドを更新するユーザーのみを許可`ProductName`と`UnitPrice`です。 か、既存 BLL を使用する ObjectDataSource を作成する必要がありますので、この編集用のインターフェイスは、製品のフィールドのサブセットのみを提供、`UpdateProduct`メソッドが不足している製品フィールドの値設定にプログラムでその`Updating`イベントハンドラー、またはお GridView で定義されているフィールドのサブセットのみを必要とする新しい BLL メソッドを作成する必要があります。 このチュートリアルでは、説明、後者のオプションを使用してし、のオーバー ロードを作成、`UpdateProduct`メソッドは、3 つの入力パラメーターは、1: `productName`、 `unitPrice`、および`productID`:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

などの元の`UpdateProduct`メソッド、このオーバー ロードは、指定したデータベースに製品があるかどうかをチェックして開始`ProductID`です。 返します、場合`False`、製品情報を更新する要求が失敗したことを示します。 それ以外の場合、既存の製品レコードの更新`ProductName`と`UnitPrice`適宜フィールドし、TableAdpater を呼び出すことによって、更新をコミット`Update()`に渡して、メソッド、`ProductsRow`インスタンス。

この追加すると、`ProductsBLL`簡略化された GridView インターフェイスを作成する準備ができましたクラスです。 開く、`DataModificationEvents.aspx`で、`EditInsertDelete`フォルダー GridView をページに追加します。 新しい ObjectDataSource を作成および構成を使用するよう、`ProductsBLL`クラス、`Select()`メソッドのマッピングを`GetProducts`とその`Update()`へのマッピングをメソッド、`UpdateProduct`使用するオーバー ロードでのみ、 `productName`、 `unitPrice`、および`productID`パラメーターを入力します。 図 2 は、ObjectDataSource をマップするときに、データ ソースの作成ウィザードを示します`Update()`メソッドを`ProductsBLL`クラスの新しい`UpdateProduct`メソッドのオーバー ロードします。


[![新しい UpdateProduct オーバー ロードに map ObjectDataSource の Update() メソッド](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**図 2**: マップ ObjectDataSource の`Update()`メソッドは、[新規] を`UpdateProduct`オーバー ロード ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))


この例は、データを編集することはできませんレコード挿入または削除できる必要がありますのみが最初に、のですぐを明示的に示す ObjectDataSource の`Insert()`と`Delete()`メソッドは、のいずれかにマップすることはできません、 `ProductsBLL`クラスのメソッドは挿入と削除のタブに移動し、ドロップダウン リストから選択する ()。


[![挿入および削除のタブのドロップダウン リストから (なし)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**図 3**: (なし) から、ドロップダウン リストを挿入および削除のタブ ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))


このウィザードの完了後、GridView のスマート タグからの編集を有効にするチェック ボックスを確認します。

データ ソースの作成ウィザードおよび GridView にバインドを完了すると、Visual Studio は、宣言の構文の両方のコントロールを作成しました。 ソース ビューの下に表示される、ObjectDataSource の宣言型マークアップの検査を参照してください。


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

ObjectDataSource のマッピングが存在しないため`Insert()`と`Delete()`メソッドがあるない`InsertParameters`または`DeleteParameters`セクションです。 さらに、以降、`Update()`メソッドのマップ先、`UpdateProduct`メソッド オーバー ロードがのみ、次の 3 つの入力パラメーターを受け取り、`UpdateParameters`セクションには 3 つ`Parameter`インスタンス。

なお、ObjectDataSource`OldValuesParameterFormatString`プロパティに設定されている`original_{0}`です。 データ ソースの構成ウィザードを使用してこのプロパティは Visual Studio によって自動的に設定します。 ただし、BLL メソッドは、元に必要ないので`ProductID`で渡される、ObjectDataSource の宣言構文からこのプロパティの割り当てを完全に削除する値。

> [!NOTE]
> だけオフにした場合、`OldValuesParameterFormatString`プロパティ [デザイン] ビューで [プロパティ] ウィンドウからプロパティ値は、宣言の構文では引き続き存在しますが、空の文字列に設定されます。 削除するか完全宣言構文から、または、[プロパティ] ウィンドウからプロパティ値、既定値と`{0}`です。


あるときに、ObjectDataSource のみ`UpdateParameters`の場合、製品の名前、価格、および ID、Visual Studio が追加 BoundField または CheckBoxField GridView での各製品のフィールドです。


[![GridView の各製品のフィールドの BoundField または CheckBoxField を含む](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**図 4**: GridView の各製品のフィールドの BoundField または CheckBoxField を含む ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))


エンドユーザーは、製品を編集、その更新ボタンをクリックすると、GridView はされた読み取り専用フィールドを列挙します。 Objectdatasource の対応するパラメーターの値を設定し、`UpdateParameters`コレクション、ユーザーが入力した値にします。 対応するパラメーターがない場合は、GridView は、コレクションに 1 つを追加します。 そのため、当社 GridView に BoundFields と CheckBoxFields のすべての製品のフィールドが含まれる場合、ObjectDataSource は、最終的に、`UpdateProduct`にもかかわらず、これらのパラメーターのすべてを受け取るオーバー ロードを ObjectDataSource の宣言型マークアップでは、3 つの入力パラメーター (図 5 を参照してください) を指定します。 同様に、一部の読み取り専用ではない組み合わせがある場合製品フィールドの入力パラメーターに対応していない、GridView、`UpdateProduct`過負荷、更新を試みているときに例外が発生します。


[![GridView は ObjectDataSource の UpdateParameters コレクションにパラメーターを追加します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**図 5**: GridView はパラメーターを追加して、ObjectDataSource`UpdateParameters`コレクション ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))


ObjectDataSource を呼び出すことを確認する、`UpdateProduct`で製品の名前、価格、および ID に、だけを受け取るオーバー ロードの編集可能なフィールドを持つように、GridView を制限する必要があります、`ProductName`と`UnitPrice`です。 これには、それら他のフィールドを設定して、その他の BoundFields と CheckBoxFields を削除することによって`ReadOnly`プロパティを`True`、または 2 つの組み合わせ。 このチュートリアルでは単に削除を除くすべての GridView フィールド、`ProductName`と`UnitPrice`BoundFields、その後、GridView の宣言型マークアップのようになります。


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

場合でも、`UpdateProduct`オーバー ロードには次の 3 つの入力パラメーターが必要ですが、当社 GridView に 2 つの BoundFields があるだけです。 これは、ため、`productID`入力パラメーターは、主キーの値との値によって渡された、`DataKeyNames`編集された行のプロパティです。

当社 GridView と共に、`UpdateProduct`過負荷、により、ユーザーは他の製品のフィールドのいずれかを失うことがなく、名前と製品の価格だけを編集します。


[![インターフェイスを使用した製品の名前と価格の編集](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**図 6**: だけ、製品の名前と価格を編集するインターフェイスが ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))


> [!NOTE]
> 前のチュートリアルで既に説明した、s ビューステートする GridView に (既定の動作) が有効になっているがきわめて重要です。 GridView s を設定した場合`EnableViewState`プロパティを`false`、同時実行ユーザーが誤って削除または編集を記録する危険を実行します。 参照してください[警告: 同時実行の問題と ASP.NET 2.0 Gridview/DetailsView/FormViews その編集をサポートするか削除すると、をビュー ステートが無効になっている](http://scottonwriting.net/sowblog/posts/10054.aspx)詳細についてはします。


## <a name="improving-theunitpriceformatting"></a>向上、`UnitPrice`書式設定

図 6 の動作を示す GridView の例の中に、`UnitPrice`フィールドがすべての書式設定されていない、任意の通貨が欠落している価格表示の結果として得られる記号し小数点以下 4 桁がします。 通貨の編集不可能な行の書式設定を適用する設定、 `UnitPrice` BoundField の`DataFormatString`プロパティを`{0:c}`とその`HtmlEncode`プロパティを`False`です。


[![UnitPrice の DataFormatString と HtmlEncode プロパティを適宜設定します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**図 7**: 設定、`UnitPrice`の`DataFormatString`と`HtmlEncode`プロパティに応じて ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))


この変更により、編集不可能な行、価格を通貨形式にします。編集した行ただし、まだ表示通貨記号と小数点以下 4 桁の値。


[![編集できない行は今すぐ形式の通貨値として](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**図 8**:「編集不可能な行がこれ通貨の値として書式設定 ([フルサイズのイメージを表示するには、をクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))


指定された書式設定の指示、`DataFormatString`プロパティは、BoundField の設定で、編集用のインターフェイスに適用できる`ApplyFormatInEditMode`プロパティを`True`(既定値は`False`)。 このプロパティを設定する`True`です。


[![UnitPrice BoundField の ApplyFormatInEditMode プロパティを True に設定します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**図 9**: 設定、 `UnitPrice` BoundField の`ApplyFormatInEditMode`プロパティを`True`([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))


この変更によりの値、`UnitPrice`編集後に表示される行が通貨として書式設定もできます。


[![編集された行の UnitPrice の値は、これで書式設定の通貨として](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**図 10**:「編集行の`UnitPrice`値これ通貨として書式設定 ([フルサイズ イメージを表示するに、をクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))


ただし、$19.00 のスローなど、テキスト ボックス内の通貨記号と製品の更新、`FormatException`です。 GridView が、ObjectDataSource をユーザーが指定した値を代入しようとしたときに`UpdateParameters`コレクションに変換できない、 `UnitPrice` 「ドル 19.00」の文字列を`Decimal`パラメーターに必要な (図 11 を参照してください)。 この問題を解決する gridview のイベント ハンドラーを作成できます`RowUpdating`イベントをユーザーが指定したを解析して`UnitPrice`通貨形式として`Decimal`です。

GridView の`RowUpdating`イベントが、第 2 パラメーターとして型のオブジェクトを受け取ります[GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx)が含まれている、`NewValues`する準備が完了のユーザーが指定した値を保持するプロパティの 1 つとしてディクショナリObjectDataSource に割り当てられている`UpdateParameters`コレクション。 既存の設定を上書きできるお`UnitPrice`値で、 `NewValues` 10 進値を使用して、コレクションの解析でのコードの次の行を含む、通貨の書式を使用して、`RowUpdating`イベント ハンドラー。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

ユーザーが指定した場合、`UnitPrice`値 (「ドル 19.00」) などは、この値がによって計算された 10 進値で上書きされます[Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx)、通貨の値を解析します。 これは正しく、10 進数、通貨記号、コンマ、小数点、およびが発生した場合と解析を使用して、 [NumberStyles 列挙](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx)で、 [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx)名前空間。

図 11 は、ユーザー指定の通貨記号によって発生する両方の問題を示しています。 `UnitPrice`、方法と共に、GridView の`RowUpdating`イベント ハンドラーを使用して、このような入力を正しく解析します。


[![編集された行の UnitPrice の値は、これで書式設定の通貨として](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**図 11**:「編集行の`UnitPrice`値これ通貨として書式設定 ([フルサイズ イメージを表示するに、をクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>手順 2: を禁止します。`NULL UnitPrices`

データベースが使用できるように構成されているときに`NULL`の値が、`Products`テーブルの`UnitPrice`列、おように必要がありますを指定することから、この特定のページを訪問するユーザー、 `NULL` `UnitPrice`値。 つまり、ユーザーが入力に失敗した場合、 `UnitPrice` 、このページで、編集済みの製品が必要となる指定した価格をユーザーに通知するメッセージを表示しデータベースに結果を保存するのではなく、製品の行を編集するときの値します。

`GridViewUpdateEventArgs` GridView にオブジェクトが渡された`RowUpdating`イベント ハンドラーに含まれる、`Cancel`プロパティを場合に、設定`True`、更新プロセスを終了します。 拡張してみましょう、`RowUpdating`イベント ハンドラーを設定する`e.Cancel`に`True`場合は、理由を説明するメッセージを表示して、`UnitPrice`値で、`NewValues`コレクションの値には、`Nothing`です。

という名前のページに、ラベルの Web コントロールを追加することによって開始`MustProvideUnitPriceMessage`です。 指定する、ユーザーが失敗した場合、このラベル コントロールが表示されます、`UnitPrice`製品を更新するときの値します。 ラベルの設定`Text`プロパティを「価格は、製品の提供する必要があります」です。 新しい CSS クラスを作成しました`Styles.css`という`Warning`を次の定義。


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

最後に、設定、ラベルの`CssClass`プロパティを`Warning`です。 この時点で、デザイナーが図 12 に示すように、赤、太字、斜体で警告メッセージ、GridView 上余分な大きなフォント サイズを表示する必要があります。


[![GridView 上のラベルが追加されました](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**図 12**: A ラベルがされて追加上 GridView ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))


既定では、このラベルを非表示、設定によりその`Visible`プロパティを`False`で、`Page_Load`イベントのハンドラー。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

ユーザーを指定せず、製品を更新しようとしたかどうか、`UnitPrice`更新をキャンセルし、警告のラベルを表示します。 GridView の補強`RowUpdating`次のようにイベント ハンドラー。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

ユーザーが、料金を指定せず、製品を保存しようとすると場合、は、更新が取り消され、便利なメッセージが表示されます。 データベース (およびのビジネス ロジック) の中では、 `NULL` `UnitPrice` s、この特定の ASP.NET ページはありません。


[![ユーザーが UnitPrice 空白から出られません。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**図 13**: ユーザーからから出られません A`UnitPrice`空白 ([フルサイズのイメージを表示するには、をクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))。


GridView を使用する方法を説明しましたこれまで`RowUpdating`をプログラムによって、ObjectDataSource に割り当てられているパラメーター値を変更するイベント`UpdateParameters`コレクションにもを取り消すには完全に処理を更新する方法です。 これらの概念は DetailsView およびフォームのコントロールに引き継がれると、挿入および削除にも適用されます。

ObjectDataSource レベルのイベント ハンドラーではこれらのタスクを実行することもその`Inserting`、 `Updating`、および`Deleting`イベント。 これらのイベントは、基になるオブジェクトの関連付けられたメソッドが呼び出される前に発生させるし、入力パラメーターのコレクションを変更または完全の操作をキャンセルする最終機会を提供します。 これら 3 つのイベントのイベント ハンドラーの型のオブジェクトが渡される[ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx)関心のある 2 つのプロパティを持ちます。

- [キャンセル](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx)、これは場合に設定`True`、実行中の操作を取り消します
- [入力パラメーター](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx)のコレクションである`InsertParameters`、 `UpdateParameters`、または`DeleteParameters`かどうかは、イベント ハンドラーによって、 `Inserting`、 `Updating`、または`Deleting`イベント

ObjectDataSource レベルでパラメーター値の操作を示すためには、新しい製品を追加することができます、ページで、DetailsView を含めてみましょう。 この DetailsView は、データベースに簡単に新しい製品を追加するためのインターフェイスを提供する使用されます。 新しい製品に追加してみましょうのみの値を入力するユーザーを許可するときに、一貫性のあるユーザー インターフェイスを保持する、`ProductName`と`UnitPrice`フィールドです。 既定では、いない DetailsView の挿入のインターフェイスに提供されているこれらの値に設定されます、`NULL`データベースの値。 ただし、ObjectDataSource を使用してお`Inserting`間もなくおわかりのように、別の既定値を挿入するイベントです。

## <a name="step-3-providing-an-interface-to-add-new-products"></a>手順 3: 新しい製品を追加するインターフェイスを提供します。

GridView 上のデザイナーには、ツールボックスからクリア DetailsView をドラッグしてその`Height`と`Width`プロパティ、およびバインドのページに、ObjectDataSource に既に存在します。 各製品のフィールドの BoundField または CheckBoxField が追加されます。 この DetailsView を使用して、新しい製品を追加するので、スマート タグから挿入を有効にするオプションをオンにする必要があります。ただし、オプションはありませんこのようなので、ObjectDataSource`Insert()`メソッドがメソッドにマップされていない、`ProductsBLL`クラス (図 3 を参照するデータ ソースを構成するときに、このマッピングを (なし) を設定おリコール)。

ObjectDataSource を構成するのには、ウィザードの起動のスマート タグからデータ ソースの構成 リンクを選択します。 最初の画面では、するには、ObjectDataSource がバインドされている基になるオブジェクトを変更できます。ままに`ProductsBLL`です。 次の画面には、ObjectDataSource のメソッドから、基になるオブジェクトへのマッピングが一覧表示します。 ここに明示的に指定する場合でも、`Insert()`と`Delete()`メソッドを任意のメソッドにマップする必要はありません、INSERT、および DELETE の各タブに移動する場合、マッピングが存在するが表示されます。 これは、ため、`ProductsBLL`の`AddProduct`と`DeleteProduct`メソッドを使用して、`DataObjectMethodAttribute`属性の既定のメソッドがあることを示すために`Insert()`と`Delete()`、それぞれします。 そのため、ObjectDataSource ウィザードは、明示的に指定されたその他の値がない限り、ウィザードを実行するこれら各時間を選択します。

ままにして、`Insert()`メソッドを指す、`AddProduct`メソッドが、もう一度 (なし)、削除 タブのドロップダウン リストを設定します。


[![AddProduct メソッドに、[挿入] タブのドロップダウン リストを設定します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**図 14**: に挿入 タブのドロップダウン リストを設定、`AddProduct`メソッド ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))


[![(なし)、[削除] タブのボックスの一覧を設定します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**図 15**: (None) に、タブの削除のドロップダウン リストの設定 ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))


これらの変更を加えたら、ObjectDataSource の宣言の構文を含むように拡張されます、`InsertParameters`コレクション、次のようにします。


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

再び追加ウィザードを再実行する、`OldValuesParameterFormatString`プロパティです。 既定値に設定することによってこのプロパティをオフにする (`{0}`) または宣言構文からを完全を削除します。

挿入する機能を提供する ObjectDataSource、DetailsView のスマート タグの表示と表示されます、挿入を有効にする チェック ボックスです。デザイナーに戻り、このオプションをオンにします。 次に、サイズを縮小 DetailsView の 2 つの BoundFields のみがある表示されるように`ProductName`と`UnitPrice`-、CommandField とします。 この時点で DetailsView の宣言の構文は、ようになります。


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

図 16 では、この時点で、ブラウザーで表示したときに、このページを示します。 ご覧のように、DetailsView には、(Chai) の最初の製品の価格と名前が一覧表示します。 ここで必要なデータが、ユーザー データベースを新しい製品をすばやく追加するための手段を提供する挿入インターフェイスです。


[![DetailsView は読み取り専用モードで現在レンダリングされています。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**図 16**:「DetailsView は読み取り専用モードで現在レンダリングされている ([フルサイズ イメージを表示するに、をクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))


挿入モードを設定する必要がありますで DetailsView を表示するのには、`DefaultMode`プロパティを`Inserting`です。 これにより、最初にアクセスされるときに、挿入モードで DetailsView をレンダリングされ、新しいレコードを挿入した後に保持することがあります。 図 17 では、このような DetailsView は、新しいレコードを追加するため、クイック インターフェイスを提供します。


[![DetailsView に新しい製品を簡単に追加するためのインターフェイスを提供します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**図 17**: DetailsView のインターフェイスを提供簡単に追加する新しい製品 ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))


ユーザーは、製品名と ("Acme Water"図 17 と同様から 1.99 など) の価格を入力し、挿入をクリックすると、ポストバックに陥りますすると、データベースに追加される新しい製品レコードの挿入のワークフローが開始します。 DetailsView では、その挿入インターフェイスおよび GridView は自動的に再バインドのデータ ソースに、新しい製品を含めるために図 18 に示すようには維持します。


![製品](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**図 18**: 製品"Acme Water"がデータベースに追加されました


図 18 に GridView しないフィールドを表示し、製品 DetailsView インターフェイスから不足しているときに`CategoryID`、 `SupplierID`、`QuantityPerUnit`に割り当てられた`NULL`データベースの値。 これは、次の手順を実行することによって確認できます。

1. Visual Studio でのサーバー エクスプ ローラーに移動します。
2. 展開する、`NORTHWND.MDF`データベース ノード
3. 右クリックし、`Products`データベース テーブルのノード
4. テーブル データの表示を選択します。

これが一覧表示、すべてのレコードで、`Products`テーブル。 図 19 に示すように、すべての新しい製品の列以外の`ProductID`、 `ProductName`、および`UnitPrice`が`NULL`値。


[![NULL 値の割り当てには、製品フィールドに指定されていない DetailsView です。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**図 19**: 割り当てられた、製品フィールドに指定されていない DetailsView`NULL`値 ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))


以外の場合、既定値を提供することがあります`NULL`これらの 1 つ以上の列値は、いずれかため`NULL`最適な既定オプションのないデータベース列自体を許可していないので、または`NULL`s。 これを実現するプログラムで設定できます。 DetailsView のパラメーターの値`InputParameters`コレクション。 この割り当てすることができますか、イベント ハンドラー DetailsView の`ItemInserting`イベントまたは ObjectDataSource の`Inserting`イベント。 既に説明しましたので前と後のレベルのイベントを使用して、データを Web でレベルを制御、ObjectDataSource のイベントをこの時間を使用してを見てみましょう。

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>手順 4: の値を割り当て、`CategoryID`と`SupplierID`パラメーター

このチュートリアルと想像してこのインターフェイスを使用して新しい製品を追加するときに、アプリケーションの必要がありますの割り当てを`CategoryID`と`SupplierID`1 の値。 前述のように、ObjectDataSource が前と後のレベルのイベントのペア発生するデータ変更処理中にします。 ときにその`Insert()`メソッドが呼び出され、最初に、ObjectDataSource を発生させますその`Inserting`イベント、メソッドを呼び出しているその`Insert()`メソッドは、マップされてさせ、最後に、`Inserted`イベント。 `Inserting`イベント ハンドラーにより、私たちは、入力パラメーターを調整または完全の操作をキャンセルする最後の機会を 1 つです。

> [!NOTE]
> 可能性がありますするさせるか、実際のアプリケーション ユーザーが、カテゴリと仕入先を指定するかのいくつかの条件に基づいてこの値を選択またはビジネス ロジック (なく盲目的 ID は 1 を選択) します。 関係なく、プログラムで ObjectDataSource の事前レベルのイベントからの入力パラメーターの値を設定する方法を示します。


ObjectDataSource のイベント ハンドラーを作成するには、しばらく時間かかる`Inserting`イベント。 イベント ハンドラーの 2 番目の入力パラメーター型のオブジェクトであることに注意してください。 `ObjectDataSourceMethodEventArgs`、パラメーター コレクションにアクセスするプロパティを持つ (`InputParameters`) と、操作を取り消すプロパティ (`Cancel`)。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

この時点で、`InputParameters`プロパティを含む、ObjectDataSource `InsertParameters` DetailsView から割り当てられた値を持つコレクション。 これらのパラメーターのいずれかの値を変更するだけで使用:`e.InputParameters("paramName") = value`です。 そのため、設定する、`CategoryID`と`SupplierID`1 の値は、調整、`Inserting`イベント ハンドラーを次のようになります。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

この時間 (など Acme Soda)、新しい製品を追加するときに、`CategoryID`と`SupplierID`新製品の列が 1 に設定されます (図 20 を参照してください)。


[![新しい製品は、CategoryID および SupplierID をある値を 1 に設定](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**図 20**: 新しい製品ようになりましたがあるその`CategoryID`と`SupplierID`値 1 に設定 ([フルサイズのイメージを表示するをクリックして](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))


## <a name="summary"></a>まとめ

中に、編集、挿入、およびプロセスを削除すると、データの Web コントロールと、ObjectDataSource の両方が、前と後のレベルのイベントの数を実行します。 このチュートリアルは、前のレベルのイベントを調査し、これらを使用して、入力パラメーターをカスタマイズしたり、データ変更操作をキャンセル両方のデータの Web コントロールと ObjectDataSource のイベントから完全する方法を説明します。 次のチュートリアルで作成して、後のレベルのイベントのイベント ハンドラーを使用してに紹介します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Jackie Goor および Liz Shulok がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [次へ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
