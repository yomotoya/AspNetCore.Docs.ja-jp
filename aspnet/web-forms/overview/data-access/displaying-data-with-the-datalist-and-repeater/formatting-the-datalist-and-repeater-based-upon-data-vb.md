---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: "DataList とリピータの書式設定データ (VB) に基づいて |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルではおをステップ実行ことで、いずれかで書式設定関数を使用して、DataList およびリピータ コントロールの外観を書式設定方法の例を示します."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 48e0f2bad8c048e943ec2a3ce72cc0f7ca4d34d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>DataList とデータ (VB) に基づいてリピータの書式設定
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe)または[PDF のダウンロード](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> このチュートリアルではおをステップ実行おで、テンプレート内で書式設定関数を使用するか、データ バインドされたイベントを処理することによって、DataList およびリピータ コントロールの外観を書式設定方法の例を示します。


## <a name="introduction"></a>はじめに

前のチュートリアルで説明したとおり、DataList は外観に影響を与えるのスタイル関連プロパティの数を提供します。 具体的には、DataList s に CSS クラスの既定値を割り当てる方法を説明しました`HeaderStyle`、 `ItemStyle`、 `AlternatingItemStyle`、および`SelectedItemStyle`プロパティです。 これら 4 つのプロパティに加えて、DataList などが含まれるその他のスタイル関連プロパティの数`Font`、 `ForeColor`、 `BackColor`、および`BorderWidth`、いくつかの例です。 リピータ コントロールにスタイルに関連するプロパティが含まれていません。 このようなスタイルの設定は、リピータのテンプレート内のマークアップ内で直接行う必要があります。

多くの場合、ただし、データの書式設定方法によって異なります、データ自体。 たとえば、製品を一覧表示するときにお可能性があります情報を表示する、製品薄い灰色の色では廃止されました、または強調表示したい場合があります、`UnitsInStock`値の場合は 0 です。 前のチュートリアルで説明したとおり、GridView、DetailsView、およびフォーム ビューには、そのデータに基づいて外観の書式を設定する 2 つの異なる方法があります。

- **`DataBound`イベント**適切なイベント ハンドラーを作成`DataBound`データが各項目にバインドされた後に発生するイベント (が GridView の`RowDataBound`イベント; DataList とリピータは、 `ItemDataBound`イベント)。 イベントのハンドラー、バインドされたデータだけを検証することができ、行われた決定を書式設定します。 この手法を調べるお、[カスタム書式指定ベース時にデータ](../custom-formatting/custom-formatting-based-upon-data-vb.md)チュートリアルです。
- **テンプレート内の関数の書式設定**TemplateFields、DetailsView または GridView コントロールまたはフォーム ビュー コントロール内にテンプレートを使用する場合、ASP.NET ページの分離コード クラス、ビジネス ロジック層、またはそのいずれかを書式設定関数を付け加えることが可能web アプリケーションからアクセス可能なその他のクラス ライブラリです。 この書式設定関数は、入力パラメーターの任意の数を受け入れることができますが、テンプレートに表示するために HTML を返す必要があります。 書式設定関数を最初に検査された、 [GridView コントロールを使用して TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)チュートリアルです。

両方のこれらの手法を書式指定は、DataList およびリピータ コントロールで使用できます。 このチュートリアルでは両方のコントロールの両方の手法の使用例の手順をおします。

## <a name="using-theitemdataboundevent-handler"></a>使用して、`ItemDataBound`イベント ハンドラー

データは、バインド時、DataList s のコントロールにデータをプログラムによって割り当てまたはデータ ソース コントロールから`DataSource`プロパティと呼び出し元の`DataBind()`メソッド、DataList の`DataBinding`イベントを発生させる、列挙、データ ソース各データ レコードは、DataList にバインドします。 DataList の作成、データ ソース内の各レコードに対して、 [ `DataListItem` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalistitem.aspx)オブジェクトを現在のレコードにバインドします。 このプロセス中には、DataList は、2 つのイベントを発生します。

- **`ItemCreated`**後に起動、`DataListItem`が作成されました
- **`ItemDataBound`**現在のレコードにバインドされた後に発生します`DataListItem`

次の手順では、DataList コントロール用のデータのバインディング プロセスを示します。

1. DataList s [ `DataBinding`イベント](https://msdn.microsoft.com/en-us/library/system.web.ui.control.databinding.aspx)発生
2. データは、DataList へのバインドします。  
  
 データ ソース内の各レコード 

    1. 作成、`DataListItem`オブジェクト
    2. Fire、 [ `ItemCreated`イベント](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. レコードにバインドします`DataListItem`
    4. Fire、 [ `ItemDataBound`イベント](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. 追加、`DataListItem`を`Items`コレクション

Repeater コントロールにデータをバインドするときに進行状況に合わせて、正確な同じ一連の手順を通じてします。 代わりにする唯一の違いは`DataListItem`リピータを使用して作成されるインスタンス、 [ `RepeaterItem` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s。

> [!NOTE]
> 絶好のリーダーは、DataList とリピータが GridView がデータにバインドされている場合とデータにバインドするまでに経過する手順のシーケンスとの間のわずかな異常を気付きかもしれません。 データ バインド プロセスの末尾に GridView を発生させます、`DataBound`イベントです。 ただし、このようなイベントのある DataList もリピータ コントロール。 これは、前と後のレベルのイベント ハンドラーのパターンが一般的になる必要がある前に、ASP.NET の 1.x 時間帯に戻り、DataList およびリピータ コントロールが作成されているためです。


データに基づく書式設定するための 1 つのオプションは、GridView のイベント ハンドラーを作成するように、`ItemDataBound`イベント。 このイベント ハンドラーがだけにバインドされているデータを検査すると、`DataListItem`または`RepeaterItem`し、必要に応じて、コントロールの書式に影響します。

DataList コントロール用の書式設定の変更を使用して、アイテム全体を実装することができます、`DataListItem`など、標準のスタイル関連プロパティ、 `Font`、 `ForeColor`、 `BackColor`、`CssClass`のようにします。 DataList のテンプレート内の特定の Web コントロールの書式に影響を与えるには、プログラムでアクセスし、それらの Web コントロールのスタイルを変更する必要があります。 このチェックインを実行する方法を説明しました、*カスタム書式指定ベース時にデータ*チュートリアルです。 Repeater コントロールと同様に、`RepeaterItem`クラスのスタイル関連プロパティがありませんそのため、すべてのスタイルに関連する変更を、`RepeaterItem`で、`ItemDataBound`イベント ハンドラーは、プログラムでアクセスして、内の Web コントロールを更新して行う必要があります。テンプレートです。

以降、 `ItemDataBound` DataList とリピータがほぼ同じですが、この例では、DataList の使用に焦点を当てますの手法を書式設定します。

## <a name="step-1-displaying-product-information-in-the-datalist"></a>手順 1: DataList で製品情報を表示します。

書式設定と心配、前に let s はまず、DataList を使用して製品情報を表示するページを作成します。 [前のチュートリアル](displaying-data-with-the-datalist-and-repeater-controls-vb.md)DataList を作成したが`ItemTemplate`各製品の名前、カテゴリ、供給業者、単位、および価格ごとの数が表示されます。 このチュートリアルではこの機能は、ここを繰り返して s を使用できます。 これを実現する、DataList とその ObjectDataSource 最初から再作成できますか、または前のチュートリアルで作成したページからそれらのコントロール経由でコピーすることができます (`Basics.aspx`) このチュートリアルでは、ページに貼り付けます (`Formatting.aspx`)。

DataList および ObjectDataSource の機能がレプリケートされた後`Basics.aspx`に`Formatting.aspx`、DataList s を変更するのにはしばらく時間かかる`ID`プロパティから`DataList1`にわかりやすい`ItemDataBoundFormattingExample`です。 次に、DataList をブラウザーで表示します。 図 1 に示す各製品の唯一の書式設定の違いは、背景色が交互です。


[![DataList コントロールでは、製品が一覧表示されます。](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**図 1**: DataList コントロールでは、製品が一覧表示されます ([フルサイズのイメージを表示するをクリックして](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


このチュートリアルでは、s ドル 2,000 未満の価格の任意の製品はその名の両方があり、unit price 黄色が強調表示されているように、DataList を書式設定を使用できます。

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>手順 2: プログラムによって、ItemDataBound イベント ハンドラー内のデータの値の決定

2,000 円が下の価格の製品だけがあるため適用されるカスタム書式設定、に各製品の価格を決定することは必要があります。 DataList にデータをバインドするときに DataList のデータ ソース内のレコードの列挙し、各レコードを作成、`DataListItem`をデータ ソースのレコードのバインド、インスタンス、`DataListItem`です。 特定のレコード秒後に、現在にデータがバインドされている`DataListItem`オブジェクト、DataList の`ItemDataBound`イベントが発生します。 現在のデータ値を確認するには、このイベントのイベント ハンドラーを作成できます`DataListItem`、それらの値に基づいて、書式設定を変更と、必要です。

作成、 `ItemDataBound` DataList のイベントし、次のコードを追加します。


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

概念と DataList s のセマンティクスを while`ItemDataBound`イベント ハンドラーは、GridView s が使用されるものと同じ`RowDataBound`内のイベント ハンドラー、*カスタム書式指定ベース時にデータ*チュートリアルでは、構文は異なります微妙に。 ときに、`ItemDataBound`イベントの起動、`DataListItem`だけを使用して対応するイベント ハンドラーに渡されるデータにバインド`e.Item`(の代わりに`e.Row`GridView s と同様、`RowDataBound`イベント ハンドラー)。 DataList s`ItemDataBound`に対してイベント ハンドラーが発生*各*行のヘッダー行、ページ フッター行、および行の区切り記号を含む、DataList に追加します。 ただし、製品の情報は、データ行にはバインドのみです。 そのためを使用する場合、`ItemDataBound`イベント データを検査するのには、DataList にバインドする必要がありますまずいることを確認するで、データ項目はします。 これには、チェックして、 `DataListItem` s [ `ItemType`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx)のいずれかである[8 つの値は次](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

両方`Item`と`AlternatingItem``DataListItem`の構成 DataList のデータ項目。 作業すると仮定した場合は、`Item`または`AlternatingItem`、実際にアクセスして`ProductsRow`現在バインドされているインスタンス`DataListItem`です。 `DataListItem` S [ `DataItem`プロパティ](https://msdn.microsoft.com/en-us/system.web.ui.webcontrols.datalistitem.dataitem.aspx)への参照が含まれています、`DataRowView`オブジェクト、`Row`プロパティは、実際に参照を提供`ProductsRow`オブジェクト。

次に、確認、`ProductsRow`インスタンスの`UnitPrice`プロパティです。 Products テーブル s 以降`UnitPrice`フィールドでは、`NULL`へのアクセスを試みる前に、値、`UnitPrice`プロパティは最初を確認するかどうかは、`NULL`値を使用して、`IsUnitPriceNull()`メソッドです。 場合、`UnitPrice`値がない`NULL`、その後、チェックかどうかを 2,000 ドル未満の秒。 2,000 円で実際に下にある場合、し、カスタム書式を適用する必要があります。

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>手順 3: 製品の名前と価格を強調表示

製品の価格が 2,000 ドル未満であることがわかった後は、名前と価格を強調表示します。 これを実現する必要がありますまずプログラムで参照内のラベル コントロール、`ItemTemplate`製品の名前と価格を表示します。 次に、黄色の背景を表示するようにする必要があります。 ラベルを直接変更してこの書式情報を適用できる`BackColor`プロパティ (`LabelID.BackColor = Color.Yellow`); 理想的には、カスケード スタイル シートからは、すべてのディスプレイに関連する問題を表す必要があります。 実際で定義されている目的の書式を提供するスタイル シートがすでにある`Styles.css`  -  `AffordablePriceEmphasis`、これが作成されで説明した、*カスタム書式指定ベース時にデータ*チュートリアルです。

書式設定を適用する 2 つの Label Web コントロールを設定するだけ`CssClass`プロパティ`AffordablePriceEmphasis`次のコードに示すように、します。


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

`ItemDataBound`イベント ハンドラーが完了しましたが、再アクセス、`Formatting.aspx`ブラウザーのページです。 図 2 に示すように、両方の名前と価格が強調表示されます、2,000 円の下で、価格がこれらの製品があります。


[![これらの製品よりも小さい 2,000 円が強調表示されます。](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**図 2**: これらの製品よりも小さい 2,000 円が強調表示されます ([フルサイズのイメージを表示するをクリックして](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> DataList は HTML としてレンダリングされますので`<table>`、その`DataListItem`インスタンス全体の項目に特定のスタイルを適用する設定できるスタイル関連のプロパティがあります。 強調表示したい場合など、*全体*の価格が 2,000 ドル未満の場合は黄項目、でしたが置き換えましたラベルを参照するコードを設定し、`CssClass`次のコード行を持つプロパティ: `e.Item.CssClass = "AffordablePriceEmphasis"`(図 3 を参照してください。)


`RepeaterItem` S ただし、don t Repeater コントロールを構成するは、このようなスタイル レベルのプロパティを提供します。 図 2 で行ったのと同じように、リピータのテンプレート内の Web コントロールにスタイル プロパティのアプリケーションがそのため、必要リピータに、カスタム書式します。


[![製品 2,000 円の全体の製品品目が強調表示されます。](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**図 3**: 製品 2,000 円の下の 全体の製品品目が強調表示されます ([フルサイズのイメージを表示するをクリックして](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>テンプレート内の書式設定関数を使用します。

*GridView コントロールを使用して TemplateFields* GridView の行にバインドされているチュートリアル GridView TemplateField 内で書式設定関数を使用してデータに基づくカスタム書式を適用する方法を説明しました。 書式設定関数は、テンプレートから呼び出すことができますし、その場所に出力する HTML を返す方法です。 書式設定機能が ASP.NET ページの分離コード クラス内に存在できますまたはにクラス ファイルに集中できます、`App_Code`フォルダーまたは別のクラス ライブラリ プロジェクト。 ASP.NET ページの分離コード クラス外の書式設定関数の移動は、または他の ASP.NET web アプリケーションで複数の ASP.NET ページで、同じ書式設定関数を使用するように計画する場合に最適です。

書式設定関数を示すためには、let s がある場合、秒の製品名の横にある [生産中止] のテキストを記載する製品情報、s が廃止されました。 また、価格の強調表示された黄色場合のある let s、2,000 ドル未満の秒 (行ったように、`ItemDataBound`イベント ハンドラーの例) 以外の場合は、価格が 2,000 円または高い、使用すると、s に実際の価格が表示されませんが見積もり価格の代わりに、テキストをしてください。 図 4 は、適用されるこれらの書式設定規則を一覧表示する製品のスクリーン ショットを示します。


[![高価な製品は、価格は見積もり価格を呼び出してくださいをテキストに置き換えられます。](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**図 4**: 高価な製品は、価格は見積もり価格を呼び出してくださいをテキストに置き換えられます ([フルサイズのイメージを表示するには、をクリックして](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))。


## <a name="step-1-create-the-formatting-functions"></a>手順 1: が書式設定関数を作成します。

この例で 2 つの書式設定関数、別および必要な場合は、[生産中止]、テキストと共に、製品名を表示する必要がある場合、いずれかの強調表示された価格を表示する、2,000 円、または、テキスト、それ以外の場合見積もり価格を呼び出してくださいを下回っています。 S、ASP.NET ページの分離コード クラスでこれらの関数を作成し、名前を付けるように`DisplayProductNameAndDiscontinuedStatus`と`DisplayPrice`です。 両方のメソッドを文字列として表示するために HTML を返す必要があるし、マークする必要はどちらも`Protected`(または`Public`) ASP.NET ページの宣言構文の部分から起動するためにします。 これら 2 つのメソッドのコードが次に示します。


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

注意してください、`DisplayProductNameAndDiscontinuedStatus`メソッドの値を受け入れる、`productName`と`discontinued`データ フィールドに対するスカラー値は、一方、`DisplayPrice`メソッドを受け入れる、`ProductsRow`インスタンス (ではなく、`unitPrice`スカラー値)。 どちらの方法を割り当てます。ただし、データベースを含めることができるスカラー値を持つかどうか、書式設定関数が動作して`NULL`値 (など`UnitPrice`以外の場合はどちらも`ProductName`も`Discontinued`許可`NULL`値)、特別な注意が必要で、これらの処理スカラーの入力。

具体的には、入力パラメーター型でなければなりません`Object`受信した値がある可能性がありますので、`DBNull`必要なデータ型ではなくインスタンス。 受信した値がデータベースであるかどうかを判断するさらに、チェックを作成する必要があります`NULL`値。 つまり、必要な場合、 `DisplayPrice` d は、スカラー値として、価格を受け入れるようにメソッドが次のコードを使用する必要があります。


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

注意してください、`unitPrice`入力パラメーターの型は`Object`場合を確認するために、条件付きステートメントが変更されたことと`unitPrice`は`DBNull`か。 さらに、以降、`unitPrice`入力パラメーターとして渡される、 `Object`、10 進値をキャストする必要があります。

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>手順 2: DataList の ItemTemplate から、書式設定関数の呼び出し

ASP.NET ページの分離コード クラスに追加され、書式設定関数ではこれらの書式指定 DataList s から関数を呼び出す`ItemTemplate`です。 テンプレートから書式設定関数を呼び出すには、データ バインド構文内に関数呼び出しを配置します。


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

DataList s で`ItemTemplate`、 `ProductNameLabel` Label Web コントロールを割り当てることによって、s の製品名を表示する現在その`Text`プロパティ結果の`<%# Eval("ProductName") %>`します。 ために必要な場合は、名前と [生産中止]、テキストを表示、割り当てる代わりにするように宣言の構文を更新、`Text`プロパティ値の`DisplayProductNameAndDiscontinuedStatus`メソッドです。 使用して、提供が中止された値と製品の名前で渡す必要がありますこれを行うときに、`Eval("columnName")`構文です。 `Eval`型の値を返します`Object`、ですが、`DisplayProductNameAndDiscontinuedStatus`メソッド型の入力パラメーターが必要ですが`String`と`Boolean`です。 そのため、おによって返される値をキャストする必要があります、`Eval`次のように必要な入力パラメーターの型、メソッド。


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

価格を表示するだけで設定できます、`UnitPriceLabel`ラベル s`Text`プロパティによって返される値を`DisplayPrice`メソッド、お s の製品名を表示するため、テキストを [廃止] と同様です。 ただしで渡す代わりに、`UnitPrice`スカラーの入力パラメーターとしては、代わりに渡す全体で`ProductsRow`インスタンス。


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

同じ場所で書式設定関数を呼び出し、ブラウザーで作業の進行状況を表示するのにはしばらくかかる場合します。 画面のようになります図 5 [生産中止]、テキストなどの提供が中止された製品とし、ください見積もり価格の呼び出しのテキストと、これらの製品の価格を持つドル 2,000 以上のコストが置き換えします。


[![高価な製品は、価格は見積もり価格を呼び出してくださいをテキストに置き換えられます。](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**図 5**: 高価な製品は、価格は見積もり価格を呼び出してくださいをテキストに置き換えられます ([フルサイズのイメージを表示するには、をクリックして](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))。


## <a name="summary"></a>概要

データに基づいた DataList または Repeater コントロールの内容を書式設定行えます 2 つの手法を使用しています。 最初の手法がのイベント ハンドラーを作成するには、`ItemDataBound`を新しいデータ ソース内の各レコードがバインドされていると発生するイベント`DataListItem`または`RepeaterItem`です。 `ItemDataBound`イベント ハンドラーを現在のアイテム データを検証してテンプレートのか、内容に適用できる書式設定し、 `DataListItem` s、全体の項目自体にします。

代わりに、カスタム書式設定は、関数の書式設定を通じて実現できます。 書式設定関数は、DataList から呼び出すことができるメソッドまたはその場所に出力する HTML を返すリピータのテンプレートです。 多くの場合、書式設定関数によって返される HTML は、現在の項目にバインドされている値によって決定されます。 これらの値は、スカラー値または項目にバインドされているオブジェクト全体を渡すことによって、書式設定関数に渡すことができます (など、`ProductsRow`インスタンス)。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Yaakov Ellis、ものです。 Schmidt、および Liz Shulok でした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
[次へ](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
