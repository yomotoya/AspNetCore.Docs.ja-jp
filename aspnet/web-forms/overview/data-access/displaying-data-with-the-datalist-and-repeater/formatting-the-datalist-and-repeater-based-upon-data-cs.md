---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: (C#) のデータに基づいて DataList と Repeater を書式設定 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは手順を説明しますと書式設定関数を使用して、DataList と Repeater コントロールの外観の形式を設定する方法の例を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: bc595064de2910dc25077d0241ef9a150fdd4cd1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368341"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>DataList と Repeater データ (c#) に基づいて書式設定
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe)または[PDF のダウンロード](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> このチュートリアルで、テンプレート内で書式設定関数を使用して、またはデータ バインド イベントを処理することによって、DataList と Repeater コントロールの外観の形式を設定する方法の例を手順説明します。


## <a name="introduction"></a>はじめに

前のチュートリアルで説明したように、DataList は、さまざまな外観に影響を与えるスタイル関連プロパティを提供します。 具体的には、DataList s に CSS クラスの既定値を割り当てる方法を説明しました`HeaderStyle`、 `ItemStyle`、 `AlternatingItemStyle`、および`SelectedItemStyle`プロパティ。 これら 4 つのプロパティに加えて、DataList にはなどが含まれます、多数他のスタイル関連プロパティには`Font`、 `ForeColor`、 `BackColor`、および`BorderWidth`があります。 Repeater コントロールでは、任意のスタイル関連プロパティは含まれません。 このようなスタイルの設定は、Repeater のテンプレートのマークアップ内で直接行う必要があります。

多くの場合、ただし、データの書式設定方法によって異なります、データ自体。 たとえば、製品の一覧を表示するときにする可能性は、中止または強調したい場合に、明るい灰色のフォントの色で製品情報を表示、`UnitsInStock`値の場合は 0。 前のチュートリアルで説明したように、GridView、DetailsView と FormView データ データに基づいて外観は書式設定に 2 つの点があります。

- **`DataBound`イベント**、適切なイベント ハンドラーを作成`DataBound`データが各項目にバインドされた後に発生するイベント (が GridView に、`RowDataBound`イベント; DataList と Repeater は、 `ItemDataBound`イベント)。 イベントのハンドラー、バインドされたデータだけを検証することができ、行われた意思決定を書式設定します。 この方法で調べる、[カスタム書式設定時にデータ](../custom-formatting/custom-formatting-based-upon-data-cs.md)チュートリアル。
- **テンプレート内の関数の書式設定**を DetailsView または GridView コントロールまたはテンプレート FormView コントロールで TemplateFields を使用する場合、書式設定関数 ASP.NET ページの分離コード クラス、ビジネス ロジック層、またはそのいずれかに追加できますweb アプリケーションからアクセスできるその他のクラス ライブラリです。 この書式設定関数は、入力パラメーターの任意の数を受け入れることができますが、テンプレートに表示するために HTML を返す必要があります。 書式設定関数がで解説した最初の[GridView コントロールで TemplateFields を使用して](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)チュートリアル。

これらの手法を書式設定の両方は DataList と Repeater コントロールで使用できます。 このチュートリアルでは、両方のコントロールの両方の手法を使用する例を手順説明します。

## <a name="using-theitemdataboundevent-handler"></a>使用して、`ItemDataBound`イベント ハンドラー

データは、DataList にバインドされるデータ ソース コントロールからまたは s のコントロールにデータをプログラムで割り当てることで場合`DataSource`プロパティと呼び出し元の`DataBind()`メソッド、DataList の`DataBinding`イベントが発生、列挙、データ ソースデータの各レコードは、DataList にバインドされます。 DataList の各データ ソースのレコードの作成、 [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx)オブジェクトを現在のレコードにバインドします。 このプロセス中には、DataList は、2 つのイベントを発生させます。

- **`ItemCreated`** 後に起動、`DataListItem`が作成されました
- **`ItemDataBound`** 現在のレコードにバインドされた後に発生します `DataListItem`

次の手順では、DataList コントロール用のデータ バインド プロセスを説明します。

1. DataList s [ `DataBinding`イベント](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx)起動
2. データは、DataList にバインドされます。  
  
   データ ソース内の各レコード 

    1. 作成、`DataListItem`オブジェクト
    2. 火災、 [ `ItemCreated`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. レコードにバインドします `DataListItem`
    4. 火災、 [ `ItemDataBound`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. 追加、`DataListItem`を`Items`コレクション

Repeater コントロールにデータをバインドする場合は、正確な同じ一連の手順に従って進行します。 唯一の違いは、の代わりに`DataListItem`Repeater を使用して作成されるインスタンス、 [ `RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)秒。

> [!NOTE]
> 鋭い読者は、一連のデータにバインドする GridView と DataList と Repeater とデータにバインドされたときに発生する手順の間のわずかな異常で気付いたかもしれません。 GridView を発生させるデータ バインド プロセスの末尾に、`DataBound`イベントです。 ただし、このようなイベントのある DataList でも、Repeater コントロール。 前と後のレベルのイベント ハンドラーのパターンの普及が前に、ASP.NET 1.x タイム フレームに戻り、DataList と Repeater コントロールが作成されたためにです。


データに基づく書式設定するための 1 つのオプションは、GridView のイベント ハンドラーを作成するように、`ItemDataBound`イベント。 このイベント ハンドラーにバインドされていただけデータを検査すると、`DataListItem`または`RepeaterItem`し、必要に応じて、コントロールの書式に影響します。

DataList コントロールの書式設定の変更項目全体の実装を使用して、`DataListItem`を含む、標準のスタイル関連プロパティを`Font`、 `ForeColor`、 `BackColor`、`CssClass`など。 DataList のテンプレート内の特定の Web コントロールの書式設定に影響を与えるには、プログラムからアクセスして、それらの Web コントロールのスタイルを変更する必要があります。 このチェックインを実行する方法を説明しました、*カスタム書式設定時にデータ*チュートリアル。 などの Repeater コントロール、`RepeaterItem`クラスのスタイル関連プロパティがありませんにそのため、すべてのスタイル関連の変更が行われた、`RepeaterItem`で、`ItemDataBound`へのアクセスと、Web コントロール内の更新プログラムを使用してイベント ハンドラーを実行する必要があります。テンプレートです。

以降、 `ItemDataBound` DataList と Repeater は事実上同じで、この例では、DataList を使用して集中する手法の書式を設定します。

## <a name="step-1-displaying-product-information-in-the-datalist"></a>手順 1: DataList で製品情報を表示します。

書式設定に関する気にし、前に let s はまず、DataList を使用して、製品情報を表示するページを作成します。 [前のチュートリアル](displaying-data-with-the-datalist-and-repeater-controls-cs.md)DataList を作成しましたが`ItemTemplate`各製品の名前、カテゴリ、供給業者、単位、および価格ごとの数が表示されます。 このチュートリアルでは、ここで、この機能を繰り返して s を使用できます。 これを実現する、DataList、およびその ObjectDataSource を最初から再作成できますか、またはそれらのコントロールを前のチュートリアルで作成したページからコピーできます (`Basics.aspx`) このチュートリアルでは、ページに貼り付けます (`Formatting.aspx`)。

DataList コントロールと ObjectDataSource 機能をレプリケートしているときと`Basics.aspx`に`Formatting.aspx`、DataList s を変更する少し`ID`プロパティから`DataList1`にわかりやすい`ItemDataBoundFormattingExample`します。 次に、ブラウザーで、DataList を表示します。 図 1 は、各製品の唯一の書式設定の違いは、背景色が交互に設定することです。


[![DataList コントロールでは、製品が表示されています。](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**図 1**: DataList コントロールでは、製品が表示されている ([フルサイズの画像を表示する をクリックします](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))。


このチュートリアルでは、s のすべての製品 20.00 ドル未満の価格には、両方の名前になり、単位価格黄色で強調表示するように、DataList を書式設定を使用できます。

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>手順 2: ItemDataBound イベント ハンドラー内のデータの値をプログラムで判断します。

20.00 ドルが下の価格の製品だけがある、適用されるカスタム書式設定、ために、各製品の価格を決定できる必要があります予定です。 DataList にデータをバインドするときに、DataList そのデータ ソース内のレコードの列挙し、レコードごとに、作成、`DataListItem`インスタンスをバインドするデータ ソースのレコード、`DataListItem`します。 現在、特定のレコードの後にデータがバインドされて`DataListItem`オブジェクト、DataList の`ItemDataBound`イベントが発生します。 現在のデータ値を検査するには、このイベントのイベント ハンドラーを作成します`DataListItem`と、それらの値に基づいて、書式設定の変更のために必要なことです。

作成、 `ItemDataBound` DataList のイベントと、次のコードを追加。


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

概念と DataList s の背後にあるセマンティクスを while`ItemDataBound`イベント ハンドラーは、GridView s で使用されるものと同じ`RowDataBound`内のイベント ハンドラー、*カスタム書式設定時にデータ*チュートリアルでは、構文は異なります微妙に。 ときに、`ItemDataBound`イベントの起動、`DataListItem`だけを使用して対応するイベント ハンドラーに渡されるデータにバインドされた`e.Item`(の代わりに`e.Row`GridView s と同様に、`RowDataBound`イベント ハンドラー)。 DataList s`ItemDataBound`を発生させるイベント ハンドラー*各*行のヘッダー行、フッター行、および行の区切り記号を含む、DataList に追加します。 ただし、製品の情報は、データ行にはバインドのみです。 そのためを使用する場合、`ItemDataBound`最初ことを確認する必要があります、データを検査するイベントは、DataList にバインドするデータ項目で作業しています。 これは、チェックを実行できます、 `DataListItem` s [ `ItemType`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx)のいずれかである[次の 8 つの値](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

両方`Item`と`AlternatingItem``DataListItem`の構成 DataList のデータ項目。 操作すると仮定しています、`Item`または`AlternatingItem`、実際にアクセスして`ProductsRow`インスタンスに現在バインドされていた`DataListItem`。 `DataListItem` S [ `DataItem`プロパティ](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx)への参照が含まれています、`DataRowView`オブジェクト、`Row`プロパティは、実際への参照を提供`ProductsRow`オブジェクト。

次に、確認、`ProductsRow`インスタンス`UnitPrice`プロパティ。 Products テーブル s 以降`UnitPrice`フィールドでは、`NULL`へのアクセスを試みる前に、値、`UnitPrice`プロパティまずを確認するかどうかは、`NULL`値を使用して、`IsUnitPriceNull()`メソッド。 場合、`UnitPrice`値でない`NULL`、続いてチェックかどうかを 20.00 ドル未満の秒。 20.00 ドルで実際の場合、カスタムの書式を適用する必要があります。

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>手順 3: 製品の名前と価格を強調表示

製品の価格が 20.00 ドル未満であるがわかれば、残っているはその名前と価格を強調表示します。 これを実現する必要がありますまずプログラムで参照のラベル コントロール、`ItemTemplate`価格と製品の名前を表示します。 次に、黄色の背景を表示するようにする必要があります。 この書式設定情報は、ラベルを直接変更して適用できる`BackColor`プロパティ (`LabelID.BackColor = Color.Yellow`)。 理想的には、カスケード スタイル シートで、すべてのディスプレイに関連する問題を表す必要があります。 実際で定義されている目的の書式を提供するスタイル シートが既にある`Styles.css`  -  `AffordablePriceEmphasis`、これが作成されで説明した、*カスタム書式設定時にデータ*チュートリアル。

書式設定を適用する 2 つのラベルの Web コントロール設定`CssClass`プロパティ`AffordablePriceEmphasis`次のコードに示すように。


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

`ItemDataBound`イベント ハンドラーが完了すると、再アクセス、`Formatting.aspx`ブラウザーでページ。 図 2 に示すように、20.00 ドルで、価格がこれらの製品は、名前と強調表示されている価格があります。


[![これらの製品よりも小さい 20.00 ドルが強調表示されます。](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**図 2**: これらの製品よりも小さい 20.00 ドルが強調表示されます ([フルサイズの画像を表示する をクリックします](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))。


> [!NOTE]
> DataList は、HTML としてレンダリングされるので`<table>`その`DataListItem`インスタンス全体の項目に特定のスタイルを適用する設定可能なスタイルに関連するプロパティがあります。 強調表示したい場合など、*全体*の価格が 20.00 ドル未満の場合は黄項目、でしたに置き換えられましたラベルを参照するコードを設定し、`CssClass`次のコード行を持つプロパティ: `e.Item.CssClass = "AffordablePriceEmphasis"`(図 3 を参照してください。)


`RepeaterItem`ただし、don t で、Repeater コントロールを構成するは、このようなスタイルのレベル プロパティを提供します。 図 2 で行ったのと同じよう、Repeater のテンプレート内での Web コントロールにスタイル プロパティのアプリケーションがそのため、必要に、Repeater のカスタム書式設定を適用します。


[![製品 20.00 ドルの製品項目全体が強調表示されます。](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**図 3**: 20.00 ドルで製品の製品項目を全体が強調表示されます ([フルサイズの画像を表示する をクリックします](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))。


## <a name="using-formatting-functions-from-within-the-template"></a>テンプレート内で書式設定関数を使用

*GridView コントロールで TemplateFields を使用して*GridView s の行にバインドされているチュートリアル GridView TemplateField 内で書式設定関数を使用して、データに基づくカスタム書式設定を適用する方法を説明しました。 書式設定関数は、テンプレートから呼び出すことができますし、その場所に出力する HTML を返すメソッドです。 書式設定関数の ASP.NET ページの分離コード クラスに存在できるまたは内のクラス ファイルに集中できます、`App_Code`フォルダーまたは別のクラス ライブラリ プロジェクト。 ASP.NET ページの分離コード クラスから書式設定関数の移動は、複数の ASP.NET ページで、またはその他の ASP.NET web アプリケーションでは、同じ書式設定関数を使用する予定の場合に最適です。

書式設定関数を示すためには、let s がある場合は、s の製品名の横にある [DISCONTINUED] のテキストを含める製品情報、s が廃止されました。 また、価格の強調表示されている黄色場合のある let s、20.00 ドル未満の秒 (で行ったよう、`ItemDataBound`イベント ハンドラーの例) 場合は、価格は、20.00 ドルが、s が高いことができますに実際の料金が表示されないまたは見積もり価格の代わりに、テキストをしてください。 図 4 は、商品の書式指定規則が適用されたこれらの一覧のスクリーン ショットを示します。


[![高価な製品は、価格はテキストを呼び出して見積もり価格にしてください、置き換えられます。](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**図 4**: 高価な製品には、価格は、テキストを呼び出して見積もり価格にしてくださいに置き換えられます ([フルサイズの画像を表示する をクリックします](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))。


## <a name="step-1-create-the-formatting-functions"></a>手順 1: 作成、書式設定関数

この例では 2 つの書式設定関数、別および必要な場合は、[DISCONTINUED] のテキストと共に、製品名を表示する必要がある場合は、いずれかを強調表示されている価格を表示する、s よりも小さい、20.00 ドルまたはテキスト、それ以外の場合価格の見積もりを呼び出してください。 ASP.NET ページの分離コード クラスでこれらの関数を作成し、名前を付けます s をできるように`DisplayProductNameAndDiscontinuedStatus`と`DisplayPrice`します。 両方のメソッドを文字列として表示するために HTML を返す必要があるし、マークする必要があります`Protected`(または`Public`) ために、ASP.NET ページの宣言構文の部分から呼び出すことができます。 これら 2 つのメソッドのコードに従います。


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

なお、`DisplayProductNameAndDiscontinuedStatus`メソッドの値を受け取ります、`productName`と`discontinued`一方にデータが、スカラー値としてフィールド、`DisplayPrice`メソッドは、`ProductsRow`インスタンス (ではなく`unitPrice`スカラー値)。 どちらの方法は機能します。ただし、データベースを含むことのできるスカラー値の書式設定関数が機能している`NULL`値 (など`UnitPrice`どちら;`ProductName`も`Discontinued`許可`NULL`値)、特別な注意が必要で、これらの処理スカラー入力します。

具体的には、入力パラメーターが型でなければなりません`Object`受信した値がある可能性がありますので、`DBNull`必要なデータ型ではなくインスタンス。 さらに、受信した値がデータベースかどうかを確認するチェックを行う必要があります`NULL`値。 つまり、必要な場合、 `DisplayPrice` d スカラー値として、価格をそのまま使用するメソッドは、次のコードを使用する必要があります。


[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

なお、`unitPrice`型の入力パラメーターが`Object`場合を確認するために、条件付きステートメントが変更されたと`unitPrice`は`DBNull`かどうか。 さらに、以降、`unitPrice`入力パラメーターとして渡される、`Object`が 10 進値にキャストする必要があります。

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>手順 2: は、DataList の ItemTemplate から書式設定関数を呼び出す

ASP.NET ページ分離コード クラスに追加の書式設定関数で残っているはこれらの関数から DataList s を書式指定を呼び出す`ItemTemplate`します。 テンプレートから書式設定関数を呼び出すには、データ バインド構文内の関数呼び出しを配置します。


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

DataList s で`ItemTemplate`、 `ProductNameLabel` Label Web コントロールが現在表示されて、s の製品名を割り当てることによってその`Text`プロパティ、結果の`<%# Eval("ProductName") %>`します。 必要な場合の名前とテキスト [DISCONTINUED] を表示させる代わりに ように宣言の構文を更新、`Text`プロパティ値の`DisplayProductNameAndDiscontinuedStatus`メソッド。 使用して、提供が中止された値と製品の名前で渡す必要がありますこれを行うときに、`Eval("columnName")`構文。 `Eval` 型の値を返します`Object`が、`DisplayProductNameAndDiscontinuedStatus`メソッド型の入力パラメーターが必要ですが`String`と`Boolean`。 したがって、によって返される値をキャストする必要があります、`Eval`次のように想定される入力パラメーターの型、メソッド。


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

価格を表示するには、単に設定できます、`UnitPriceLabel`ラベル s`Text`プロパティによって返される値を`DisplayPrice`メソッド、製品の名前を表示するためでしたし、テキストを [廃止] と同様です。 ただしではなく、`UnitPrice`スカラーの入力パラメーターとして代わりに渡す全体で`ProductsRow`インスタンス。


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

配置で書式設定関数を呼び出し、ブラウザーで、進行状況を表示するのには少しをを。 [DISCONTINUED] のテキストを含む提供が中止された製品と図 5 のようになります、画面として見積もり価格の呼び出しのテキストと、コストを超える 20.00 ドルの価格を持つこれらの製品が置き換えします。


[![高価な製品は、価格はテキストを呼び出して見積もり価格にしてください、置き換えられます。](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**図 5**: 高価な製品には、価格は、テキストを呼び出して見積もり価格にしてくださいに置き換えられます ([フルサイズの画像を表示する をクリックします](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))。


## <a name="summary"></a>まとめ

データに基づいて DataList または Repeater コントロールの内容を書式設定できます 2 つの手法を使用します。 最初の手法は、イベント ハンドラーを作成する、`ItemDataBound`を新しいデータ ソース内の各レコードがバインドされているように発生するイベント、`DataListItem`または`RepeaterItem`します。 `ItemDataBound`イベント ハンドラー、現在の項目のデータを検証およびテンプレートのか、内容に適用できる書式設定し、`DataListItem`全体、項目自体への秒。

または、カスタム書式設定は、関数の書式設定を実現できます。 書式設定の関数は、DataList から呼び出すことができるメソッドまたは Repeater のテンプレートをその場所に出力する HTML を返します。 多くの場合、書式設定関数によって返される HTML は、現在の項目にバインドされている値によって決定されます。 これらの値は、スカラー値または項目にバインドされているオブジェクト全体を渡すことによって、書式設定の関数に渡すことができます (など、`ProductsRow`インスタンス)。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Yaakov Ellis、Randy Schmidt、Liz Shulok でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [次へ](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
