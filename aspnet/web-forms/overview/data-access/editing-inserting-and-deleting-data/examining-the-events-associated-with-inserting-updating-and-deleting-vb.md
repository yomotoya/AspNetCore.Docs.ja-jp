---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: 挿入、更新、および削除 (VB) に関連付けられているイベントを調べる |Microsoft Docs
author: rick-anderson
description: 検証する前に、実行時に、および挿入後に発生するイベントを使用してこのチュートリアルでは、更新、または ASP.NET データ Web コントロールの操作を削除します。 W..
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 483ff296bb6fcda14c224c085fc87209bb548700
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387911"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>挿入、更新、および削除 (VB) に関連付けられているイベントを調べる
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe)または[PDF のダウンロード](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> 検証する前に、実行時に、および挿入後に発生するイベントを使用してこのチュートリアルでは、更新、または ASP.NET データ Web コントロールの操作を削除します。 製品のフィールドのサブセットのみを更新する編集インターフェイスをカスタマイズする方法についても説明します。


## <a name="introduction"></a>はじめに

を、組み込みの挿入、編集、または削除 GridView、DetailsView、または FormView コントロールの機能を使用する場合、エンドユーザーが新しいレコードを追加または更新または既存のレコードを削除するプロセスを完了すると、さまざまな手順が発生します。 説明したよう、[前のチュートリアル](an-overview-of-inserting-updating-and-deleting-data-vb.md)[編集] ボタンが更新し、[キャンセル] ボタンとテキスト ボックスに BoundFields 有効化置き換え GridView の行を編集する際に、します。 エンド ユーザーは、データを更新し、更新プログラムをクリックすると、次の手順は、ポストバック時に実行されます。

1. GridView に設定します、ObjectDataSource の`UpdateParameters`編集されたレコードの一意の識別フィールドに (を使用して、`DataKeyNames`プロパティ)、ユーザーが入力した値と共に
2. GridView 呼び出す、ObjectDataSource の`Update()`メソッドで、基になるオブジェクトで適切なメソッドが呼び出されます (`ProductsDAL.UpdateProduct`で、前のチュートリアル)
3. 今すぐ最新の変更が含まれている、基になるデータが GridView にバインドします。

この一連の手順では、中にイベント数を起動して、カスタム ロジックを追加するイベント ハンドラーを作成することを有効にするために必要な場合。 手順 1 まで、GridView の前に、`RowUpdating`イベントが発生します。 いくつかの検証エラーがある場合は、この時点では、更新要求をキャンセルことができます。 ときに、`Update()`メソッドが呼び出されると、ObjectDataSource の`Updating`を追加またはのいずれかの値をカスタマイズする機会を提供するイベントの起動、`UpdateParameters`します。 ObjectDataSource の基になるオブジェクトのメソッドが完了したら実行すると、ObjectDataSource の`Updated`イベントが発生します。 イベント ハンドラー、`Updated`イベントが影響を受けた行の数と、例外が発生したかどうかなど、更新操作の詳細を調べることができます。 手順 2 まで、GridView の後に最後に、`RowUpdated`イベントが発生します。 このイベントのイベント ハンドラー実行だけで、更新操作に関する追加情報を確認します。

図 1 GridView を更新するときに、この一連のイベントと手順を示しています。 図 1 イベント パターンは、GridView を使用する更新に固有ではありません。 挿入、更新、または、GridView からデータを削除、DetailsView、またはフォーム ビューには、同じデータ Web コントロールと ObjectDataSource の両方の前と後のレベルのイベントのシーケンスが precipitates します。


[![一連の前と後イベントには炎を GridView にデータを更新するときに](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**図 1**: A シリーズの前と後のイベント起動時にデータの更新 GridView ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))。


このチュートリアルでは、これらのイベントを使用して、組み込みの挿入を拡張するについて説明しますで更新、および ASP.NET データの機能を削除する Web を制御します。 製品のフィールドのサブセットのみを更新する編集インターフェイスをカスタマイズする方法についても説明します。

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>手順 1: 更新、製品の`ProductName`と`UnitPrice`フィールド

前のチュートリアルから編集インターフェイスで*すべて*製品フィールドされなかった読み取り専用に指定する必要があります。 GridView - からフィールドを削除する場合と答えて`QuantityPerUnit`データ Web コントロールのデータを更新が設定されていない場合、ObjectDataSource の - `QuantityPerUnit` `UpdateParameters`値。 値は、ObjectDataSource を渡しますし`Nothing`に、`UpdateProduct`ビジネス ロジック層 (BLL) のメソッドは、変更、編集済みのデータベース レコードの`QuantityPerUnit`列を`NULL`値。 同様に、必須フィールドの場合など`ProductName`が削除されると編集のインターフェイスから、更新が失敗、"*列 'ProductName' が null を許容しない*"例外。 この動作の理由が呼び出す、ObjectDataSource が構成されているため、`ProductsBLL`クラスの`UpdateProduct`メソッドで、各製品のフィールドの入力パラメーターが必要です。 そのため、ObjectDataSource の`UpdateParameters`コレクションにパラメーターが含まれている場合、メソッドのそれぞれのパラメーターを入力します。

データ フィールドのサブセットのみを更新するには、エンドユーザーは、Web コントロールを提供するかどうかは、設定するかプログラムで不足している必要があります`UpdateParameters`ObjectDataSource の内の値`Updating`イベント ハンドラーまたは作成し、BLL メソッドを呼び出すフィールドのサブセットのみが必要です。 この後者のアプローチを見てみましょう。

具体的には、表示するページを作成しましょうだけ`ProductName`と`UnitPrice`編集可能な GridView のフィールド。 この GridView の編集インターフェイスは、2 つ表示されるフィールドを更新するユーザーのみを許可`ProductName`と`UnitPrice`します。 か、既存 BLL を使用する、ObjectDataSource を作成する必要がありますので、この編集インターフェイスは、製品のフィールドのサブセットを提供するだけ、`UpdateProduct`メソッドが不足している製品フィールドの値が設定でプログラムを使用してその`Updating`イベントハンドラー、または GridView で定義されているフィールドのサブセットのみが必要とする新しい BLL メソッドを作成する必要があります。 このチュートリアルでは、後者のオプションを使用してのオーバー ロードを作成、`UpdateProduct`メソッドは、3 つの入力パラメーターでは、1: `productName`、 `unitPrice`、および`productID`:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

などの元の`UpdateProduct`メソッド、このオーバー ロードは、指定したデータベースに製品があるかどうかをチェックして開始`ProductID`します。 返されたそうでない場合は`False`、製品情報を更新する要求が失敗したことを示します。 それ以外の場合、既存の製品レコードの更新`ProductName`と`UnitPrice`適宜フィールドし、TableAdpater を呼び出すことによって、更新をコミット`Update()`に渡して、メソッド、`ProductsRow`インスタンス。

これにより、`ProductsBLL`簡略化された GridView インターフェイスを作成する準備ができましたクラス。 開く、`DataModificationEvents.aspx`で、`EditInsertDelete`フォルダーと、ページに GridView を追加します。 新しい ObjectDataSource を作成および使用するように構成、`ProductsBLL`クラスとその`Select()`メソッドのマッピングを`GetProducts`とその`Update()`へのマッピングをメソッド、`UpdateProduct`でのみを受け取るオーバー ロード、 `productName`、 `unitPrice`、および`productID`パラメーターを入力します。 図 2 は、ObjectDataSource をマッピングするときに、データ ソースの作成ウィザードを示しています`Update()`メソッドを`ProductsBLL`クラスの新しい`UpdateProduct`メソッドのオーバー ロードします。


[![ObjectDataSource の Update() メソッドを新しい UpdateProduct オーバー ロードにマップします。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**図 2**: マップ ObjectDataSource の`Update()`メソッドを新規`UpdateProduct`オーバー ロード ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))。


この例では、機能、データを編集するがないレコード挿入または削除が必要のみが最初には、ために少し明示的に示す ObjectDataSource の`Insert()`と`Delete()`メソッドは、のいずれかにマップすることはできません、 `ProductsBLL`クラスのメソッドを挿入および削除の各タブに移動し、ドロップダウン リストから選択 (なし)。


[![挿入および削除のタブのドロップダウン リストから (なし)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**図 3**: (なし) がドロップダウン リストから挿入および削除のタブを選択 ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))。


このウィザードの完了後に、GridView のスマート タグの編集を有効にするチェック ボックスをオンします。

GridView にバインドするデータ ソースの作成ウィザードの完了すると、Visual Studio の両方のコントロール宣言構文を作成しました。 次のとおりですが、ObjectDataSource の宣言型マークアップを検査する、ソース ビューに移動します。


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

ObjectDataSource のマッピングが存在しないため`Insert()`と`Delete()`メソッドがあるない`InsertParameters`または`DeleteParameters`のセクションでは。 さらに、以降、`Update()`をメソッドにマップされて、`UpdateProduct`だけ 3 つの入力パラメーターを受け取るメソッド オーバー ロード、`UpdateParameters`セクションには 3 つ`Parameter`インスタンス。

なお、ObjectDataSource の`OldValuesParameterFormatString`プロパティに設定されて`original_{0}`します。 このプロパティは、データ ソースの構成ウィザードを使用して Visual Studio によって自動的に設定されます。 ただし、BLL メソッドは、元の予定がないため`ProductID`で渡される、ObjectDataSource の宣言構文からこのプロパティの割り当てを完全に削除する値。

> [!NOTE]
> だけオフにした場合、`OldValuesParameterFormatString`プロパティ [デザイン] ビューで [プロパティ] ウィンドウからプロパティ値の宣言の構文ではそのままが空の文字列に設定されます。 削除するか完全宣言の構文から、または、[プロパティ] ウィンドウからプロパティ値を既定の`{0}`します。


ObjectDataSource はのみに`UpdateParameters`製品の名前、価格、および ID、Visual Studio が追加 BoundField または CheckBoxField GridView で各製品のフィールド。


[![GridView の各製品のフィールドの BoundField または CheckBoxField を含む](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**図 4**: GridView を BoundField または CheckBoxField の各製品のフィールドの格納 ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))。


エンド ユーザーでは、製品の編集、その更新ボタンをクリックすると、GridView は読み取り専用でしたそれらのフィールドを列挙します。 ObjectDataSource ので、対応するパラメーターの値を設定し、`UpdateParameters`ユーザーが入力した値のコレクション。 対応するパラメーターがない場合、GridView はコレクションに追加します。 そのため、当社の GridView に BoundFields と CheckBoxFields のすべての製品のフィールドが含まれる場合、ObjectDataSource は、最終的に、`UpdateProduct`という事実に関係なく、これらのパラメーターのすべてを受け取るオーバー ロードを ObjectDataSource の宣言型マークアップでは、3 つの入力パラメーター (図 5 参照) を指定します。 同様に、読み取り専用ではないの組み合わせがある場合製品フィールドの入力パラメーターに対応しない GridView、`UpdateProduct`オーバー ロードすると、更新を試みているときに、例外が発生します。


[![GridView は ObjectDataSource の UpdateParameters コレクションにパラメーターを追加します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**図 5**:、GridView は追加パラメーターを ObjectDataSource の`UpdateParameters`コレクション ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))。


ObjectDataSource が呼び出すことを確認する、`UpdateProduct`で製品の名前、価格、および ID だけを受け取るオーバー ロードの編集可能なフィールドに、GridView を制限する必要があります、`ProductName`と`UnitPrice`します。 これは、その他のフィールドを設定して、その他の BoundFields と CheckBoxFields を削除することで実現できます`ReadOnly`プロパティを`True`、または 2 つの組み合わせ。 このチュートリアルでは単に削除を除くすべての GridView フィールド、`ProductName`と`UnitPrice`BoundFields、その後、GridView の宣言型マークアップのようになります。


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

場合でも、`UpdateProduct`オーバー ロードには 3 つの入力パラメーターが必要ですが、のみ、GridView に 2 つの BoundFields があります。 これは、ため、`productID`入力パラメーターが主キーの値との値によって渡された、`DataKeyNames`編集された行のプロパティ。

当社の GridView と共に、`UpdateProduct`オーバー ロードのユーザーを他の製品のフィールドのいずれかを失うことがなく、名前と製品の価格だけを編集できます。


[![インターフェイスを使用した製品の名前と価格の編集](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**図 6**: だけ、製品の名前と価格の編集インターフェイスが ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))。


> [!NOTE]
> 前のチュートリアルで既に説明したは、s ビュー状態である GridView が (既定の動作) を有効にすることがきわめて重要です。 GridView 秒に設定した場合`EnableViewState`プロパティを`false`、同時実行ユーザーが誤って削除または編集を記録するというリスクを実行します。 参照してください[警告: 同時実行の問題を ASP.NET 2.0 Gridview と DetailsView/FormViews そのサポートを編集または削除およびをビュー ステートが無効になっている](http://scottonwriting.net/sowblog/posts/10054.aspx)詳細についてはします。


## <a name="improving-theunitpriceformatting"></a>向上、`UnitPrice`書式設定

図 6 は、GridView の例の中に、`UnitPrice`フィールドがすべての書式設定されていない、記号し小数点以下 4 桁が任意の通貨が不足している価格表示にします。 通貨が編集可能な行の書式設定を適用する設定、 `UnitPrice` BoundField の`DataFormatString`プロパティを`{0:c}`とその`HtmlEncode`プロパティを`False`します。


[![UnitPrice の DataFormatString と HtmlEncode プロパティを適宜設定します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**図 7**: 設定、`UnitPrice`の`DataFormatString`と`HtmlEncode`プロパティに応じて ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))。


この変更により、行が編集可能な料金は、通貨形式に、;編集された行、ただし、引き続き表示されます、通貨記号と小数点以下 4 桁の値。


[![非が編集可能な行で書式化を通貨値として](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**図 8**: The 以外が編集可能な行はこれで通貨の値として書式設定 ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))。


指定された書式設定の指示、`DataFormatString`プロパティは、BoundField を設定して編集インターフェイスに適用できます`ApplyFormatInEditMode`プロパティを`True`(既定値は`False`)。 このプロパティを設定する少し`True`します。


[![UnitPrice BoundField の ApplyFormatInEditMode プロパティを True に設定します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**図 9**: 設定、 `UnitPrice` BoundField の`ApplyFormatInEditMode`プロパティを`True`([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))。


この変更では、値を`UnitPrice`編集後に表示される行が通貨として書式設定もできます。


[![編集された行の UnitPrice の値は今すぐ形式を通貨として](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**図 10**: The 編集行の`UnitPrice`値が通貨として書式設定ようになりました ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))。


ただし、$19.00 のスローなどの通貨記号をテキスト ボックスに製品を更新、`FormatException`します。 GridView が ObjectDataSource のユーザーが指定した値を代入しようとしたときに`UpdateParameters`コレクションに変換できない、 `UnitPrice` 「19.00$」を文字列に、`Decimal`パラメーターに必要な (図 11 を参照してください)。 これを解決するには、GridView のイベント ハンドラーを生み出すことができます`RowUpdating`イベントしてそれをユーザーが指定した解析`UnitPrice`通貨形式として`Decimal`します。

GridView の`RowUpdating`イベントが 2 番目のパラメーターとして型のオブジェクトを受け取ります[GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx)が含まれています、`NewValues`準備のユーザーが指定した値を保持するプロパティの 1 つとしてディクショナリObjectDataSource に割り当てられている`UpdateParameters`コレクション。 既存の上書きできる`UnitPrice`値、 `NewValues` 10 進値を使用して、コレクション内のコードの次の行で、通貨形式を使用して解析、`RowUpdating`イベント ハンドラー。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

ユーザーが指定した場合、`UnitPrice`値 (「$19.00」) などは、この値はによって計算された 10 進値で上書きされます[Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx)、通貨値を解析します。 これは、通貨記号、コンマ、小数点、および、すべてが発生した場合、10 進数が正しく解析され、使用されます、 [NumberStyles 列挙](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx)で、 [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx)名前空間。

図 11 は、ユーザーが指定した通貨記号の原因となった問題を両方`UnitPrice`、方法と共に、GridView の`RowUpdating`イベント ハンドラーを使用して、このような入力を正しく解析します。


[![編集された行の UnitPrice の値は今すぐ形式を通貨として](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**図 11**: The 編集行の`UnitPrice`値が通貨として書式設定ようになりました ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))。


## <a name="step-2-prohibitingnull-unitprices"></a>手順 2: を禁止します。`NULL UnitPrices`

データベースが許可するように構成されて`NULL`値、`Products`テーブルの`UnitPrice`する可能性がありますを指定することから、この特定のページを訪問するユーザーを防ぐために、列、 `NULL` `UnitPrice`値。 つまり、ユーザーが入力に失敗した場合、`UnitPrice`結果、このページを編集した製品が必要な指定された価格をユーザーに通知するメッセージを表示するデータベースに保存するのではなく、製品の行を編集するときの値します。

`GridViewUpdateEventArgs`に GridView のオブジェクトが渡される`RowUpdating`イベント ハンドラーが含まれています、`Cancel`プロパティを場合に、設定`True`、更新プロセスを終了します。 拡張、`RowUpdating`イベント ハンドラーを設定する`e.Cancel`に`True`場合、その理由を説明するメッセージを表示して、`UnitPrice`値、`NewValues`コレクションの値を持つ`Nothing`。

ラベルの Web コントロールをという名前のページに追加することで開始`MustProvideUnitPriceMessage`します。 ユーザーが指定する場合、このラベル コントロールが表示されます、`UnitPrice`製品を更新するときの値します。 ラベルの設定`Text`プロパティを「価格は、製品の提供する必要があります」します。 新しい CSS クラスを作成した`Styles.css`という`Warning`を次の定義。


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

最後に、ラベルを設定`CssClass`プロパティを`Warning`します。 この時点で、デザイナーが図 12 に示すように、警告メッセージの赤い太字、斜体、GridView、上記の超大規模なフォント サイズを表示する必要があります。


[![GridView の上にラベルが追加されました](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**図 12**: A ラベルがされている追加の上、GridView ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))。


既定では、このラベルを非表示に、設定、`Visible`プロパティを`False`で、`Page_Load`イベント ハンドラー。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

ユーザーが指定せず、製品を更新しようとしたかどうか、`UnitPrice`更新をキャンセルし、警告のラベルを表示します。 GridView の補強`RowUpdating`次のようにイベント ハンドラー。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

価格を指定せず、製品を保存しようとすると場合、は、更新は取り消され、便利なメッセージが表示されます。 データベース (およびのビジネス ロジック) の中では、 `NULL` `UnitPrice` s、この特定の ASP.NET ページはありません。


[![ユーザーは、UnitPrice 空白のままことはできません。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**図 13**: ユーザーはから離れることはできません、`UnitPrice`空白 ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))。


GridView を使用する方法を学習しましたところ`RowUpdating`ObjectDataSource に割り当てられているパラメーター値をプログラムで変更するイベント`UpdateParameters`コレクションにもキャンセルするには完全に処理を更新する方法。 これらの概念は、DetailsView コントロールと FormView コントロールに引き継がれることと、挿入および削除にも適用されます。

これらのタスクは、ObjectDataSource レベルのイベント ハンドラーを使用して行うこともできます、 `Inserting`、 `Updating`、および`Deleting`イベント。 これらのイベントは、基になるオブジェクトの関連付けられたメソッドが呼び出される前に起動し、入力パラメーターのコレクションを変更またはも、最初から、操作をキャンセルする最後の機会を提供します。 これら 3 つのイベントのイベント ハンドラーが型のオブジェクトに渡される[ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx)関心のある 2 つのプロパティを持ちます。

- [キャンセル](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx)であり場合に、設定`True`、実行中の操作をキャンセル
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx)のコレクションである`InsertParameters`、 `UpdateParameters`、または`DeleteParameters`かどうかは、イベント ハンドラーによって、 `Inserting`、 `Updating`、または`Deleting`イベント

ObjectDataSource のレベルでパラメーター値の操作を示すためには、新しい製品を追加するユーザーがこのページで、DetailsView を含めてみましょう。 この DetailsView を迅速に、データベースに新しい製品を追加するためのインターフェイスを提供する使用されます。 のみの値を入力するユーザーを許可すてみましょう新しい製品を追加するときに、一貫性のあるユーザー インターフェイスを保持する、`ProductName`と`UnitPrice`フィールド。 既定では、DetailsView の挿入インターフェイスに提供されているこれらの値に設定されます、`NULL`データベースの値。 ただし、ObjectDataSource を使用して`Inserting`すぐ表示されるように、別の既定値を挿入するイベントです。

## <a name="step-3-providing-an-interface-to-add-new-products"></a>手順 3: 新しい製品を追加するインターフェイスを提供します。

クリアします、DetailsView を GridView、上のデザイナーには、ツールボックスからドラッグその`Height`と`Width`プロパティ、およびページに既に存在します ObjectDataSource にバインドします。 各製品のフィールドの BoundField または CheckBoxField が追加されます。 この DetailsView を使用して、新しい製品を追加するので、スマート タグから挿入を有効にするオプションをオンにする必要があります。ただし、オプションはありませんこのようなので、ObjectDataSource の`Insert()`メソッドがメソッドにマップされていない、`ProductsBLL`クラス (図 3 を参照するデータ ソースを構成するときに、このマッピングを (なし) を設定することを思い出してください)。

ObjectDataSource で構成するには、ウィザードを起動、スマート タグからデータ ソースの構成のリンクを選択します。 最初の画面では、ObjectDataSource が; にバインドされている基になるオブジェクトを変更できます。設定します`ProductsBLL`します。 次の画面には、ObjectDataSource のメソッドから、基になるオブジェクトへのマッピングが一覧表示します。 明示的に指定する場合でも、`Insert()`と`Delete()`メソッドが存在するメソッドをマップする必要ありません、INSERT および DELETE の各タブに移動する場合、マッピングが存在するが表示されます。 ため、これは、`ProductsBLL`の`AddProduct`と`DeleteProduct`メソッドを使用して、`DataObjectMethodAttribute`属性を既定のメソッドをしていることを示す`Insert()`と`Delete()`、それぞれします。 そのため、ObjectDataSource ウィザードは、これら各明示的に指定されたその他の値がない限り、ウィザードを実行する時刻を選択します。

ままに、`Insert()`メソッドを指す、`AddProduct`メソッドが、もう一度 (なし)、削除 タブのドロップダウン リストを設定します。


[![AddProduct メソッドに挿入 タブのドロップダウン リストを設定します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**図 14**: 挿入 タブのドロップダウン リストを設定、`AddProduct`メソッド ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))。


[![(なし)、[削除] タブのボックスの一覧を設定します。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**図 15**: (None) に、削除タブのドロップダウン リストの設定 ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))。


これらの変更を加えたら、ObjectDataSource の宣言型構文を含むように拡張は、`InsertParameters`次に示す、コレクション。


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

再び追加ウィザードを再実行、`OldValuesParameterFormatString`プロパティ。 既定値に設定することでこのプロパティをオフにする少し (`{0}`) または宣言型構文から全体を削除します。

挿入機能を提供する ObjectDataSource、DetailsView のスマート タグにはこれで含まれます挿入を有効にする チェック ボックスです。デザイナーに戻り、このオプションをオンにします。 2 つの BoundFields - のみがある表示されるため、DetailsView を次に、切り詰める`ProductName`と`UnitPrice`- および CommandField します。 この時点でよう DetailsView の宣言型構文になります。


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

図 16 では、この時点で、ブラウザーで表示した場合は、このページを示します。 ご覧のとおり、最初の製品 (Chai) の価格と名前、DetailsView が一覧表示します。 ただし、ここで必要なは、ユーザー データベースを新しい製品をすばやく追加するための手段を提供する挿入のインターフェイスです。


[![DetailsView は現在読み取り専用モードでレンダリング](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**図 16**:、DetailsView は現在読み取り専用モードでレンダリング ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))。


挿入モードを設定する必要があります。 そのである DetailsView を表示するには、`DefaultMode`プロパティを`Inserting`します。 これは、最初にアクセスすると、挿入モードである DetailsView をレンダリングし、新しいレコードを挿入した後に保持することがあります。 図 17 に示す、このような DetailsView では、新しいレコードを追加するためのクイック インターフェイスが提供します。


[![DetailsView 新しい製品を簡単に追加するためのインターフェイスを提供します](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**図 17**: DetailsView インターフェイスを提供します、簡単に追加の新しい製品 ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))。


ユーザーは、製品名と価格 ("Acme Water"図 17 のようにから 1.99 など) 入力すると、挿入をクリックすると、ポストバック後し、データベースに追加される新しい製品レコードで、挿入のワークフローを開始します。 DetailsView は、その挿入インターフェイスと GridView が自動的にバインドし、データ ソースに新しい製品を含めるために図 18 に示すように維持します。


![製品](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**図 18**: 製品"Acme Water"がデータベースに追加されました


図 18 GridView 表示されない、DetailsView インターフェイスから不足している製品のフィールド間`CategoryID`、 `SupplierID`、`QuantityPerUnit`に割り当てられている`NULL`データベースの値。 これは、次の手順を実行することによって確認できます。

1. Visual Studio でサーバー エクスプ ローラーに移動します。
2. 展開、`NORTHWND.MDF`データベース ノード
3. 右クリックし、`Products`データベース テーブルのノード
4. テーブル データの表示を選択します。

これが一覧表示内のレコードはすべて、`Products`テーブル。 図 19 に示すよう、すべての新しい製品の列以外の`ProductID`、 `ProductName`、および`UnitPrice`が`NULL`値。


[![NULL 値を割り当てるには、製品フィールドに指定されていない、DetailsView です。](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**図 19**: 割り当てられている、製品フィールドに指定されていない、DetailsView`NULL`値 ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))。


以外の既定値を指定することがあります`NULL`これらの 1 つ以上の列値か`NULL`は最適な既定のオプションはありません、データベース列自体ができないため、または`NULL`s。 これを実現する、DetailsView のパラメーターの値を設定しますできるプログラムで`InputParameters`コレクション。 この割り当てができますか、イベント ハンドラー、DetailsView の`ItemInserting`イベントまたは ObjectDataSource の`Inserting`イベント。 既に説明しましたので Web データの前処理および投稿レベルのイベントを使用してレベルを制御、ObjectDataSource のイベントをこの時間を使用してを見てみましょう。

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>手順 4: に値を割り当てて、`CategoryID`と`SupplierID`パラメーター

このチュートリアルでは想像するこのインターフェイスを使用して新しい製品を追加するときに、このアプリケーションに割り当てる必要があります、`CategoryID`と`SupplierID`1 の値。 前述のように、ObjectDataSource が前と後のレベルのイベントのペアを起動するデータ変更の処理中にします。 ときにその`Insert()`メソッドが呼び出される、ObjectDataSource がその`Inserting`イベント、メソッドを呼び出しているその`Insert()`メソッドにマップされていて、最後に発生させます、`Inserted`イベント。 `Inserting`イベント ハンドラーにより私たちは、入力パラメーターを調整またはも、最初から、操作をキャンセルする最後の機会を 1 つ。

> [!NOTE]
> させるかにすることは実際のアプリケーションで、ユーザーが category と supplier の指定、またはいくつかの条件に基づいてこの値を選択またはビジネス ロジック (なく無条件、ID が 1 を選択)。 関係なく、プログラムで ObjectDataSource の事前のレベルのイベントからの入力パラメーターの値を設定する方法についても示します。


ObjectDataSource のイベント ハンドラーを作成する少し`Inserting`イベント。 イベント ハンドラーの 2 番目の入力パラメーター型のオブジェクトであることに注意してください。 `ObjectDataSourceMethodEventArgs`、パラメーター コレクションにアクセスするプロパティを持つ (`InputParameters`) と、操作をキャンセルするプロパティ (`Cancel`)。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

この時点で、`InputParameters`プロパティには、ObjectDataSource にはが含まれています。 `InsertParameters` 、DetailsView から割り当てられた値を持つコレクション。 これらのパラメーターのいずれかの値を変更する使用:`e.InputParameters("paramName") = value`します。 そのため、設定する、`CategoryID`と`SupplierID`1 の値は、調整、`Inserting`イベント ハンドラーを次のようになります。


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

この (など、Acme Soda)、新しい製品を追加したときに、`CategoryID`と`SupplierID`新製品の列が 1 に設定されます (図 20 を参照してください)。


[![新製品は、CategoryID と SupplierID をある値を 1 に設定](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**図 20**: 新しい製品ようになりましたがあるその`CategoryID`と`SupplierID`値が 1 に設定 ([フルサイズの画像を表示する をクリックします](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))。


## <a name="summary"></a>まとめ

中に、編集、挿入、およびプロセスを削除する、データ Web コントロールと ObjectDataSource の両方が、前と後のレベルのイベント数を実行します。 このチュートリアルでは事前レベルのイベントを調査し、これらを使用する入力パラメーターをカスタマイズしたり、データ変更操作をキャンセルするデータ Web コントロールと ObjectDataSource のイベントから完全両方の方法を説明しました。 次のチュートリアルで作成および投稿レベルのイベントのイベント ハンドラーを使用して紹介します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Jackie Goor と Liz Shulok でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [次へ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
