---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: (VB) DetailsView コントロールで TemplateFields を使用する |Microsoft Docs
author: rick-anderson
description: GridView で利用できる同じ TemplateFields 機能も、DetailsView コントロールで使用できます。 このチュートリアルでは 1 つの製品が表示されたら送りしています.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a994df097148428779c9e219ed08247d47ea1a5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821736"
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>(VB) DetailsView コントロールで TemplateFields を使用します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe)または[PDF のダウンロード](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> GridView で利用できる同じ TemplateFields 機能も、DetailsView コントロールで使用できます。 このチュートリアルで TemplateFields を含む DetailsView を使用して一度に 1 つの製品を表示しますします。


## <a name="introduction"></a>はじめに

TemplateField、BoundField、CheckBoxField、内、およびその他のデータ フィールドのコントロールよりも高いレベルのレンダリング データの柔軟性を提供します。 [前のチュートリアル](using-templatefields-in-the-gridview-control-vb.md)TemplateField に GridView を使用しました。

- 1 つの列では、複数のデータ フィールドの値を表示します。 具体的には、どちらも、`FirstName`と`LastName`フィールドは、GridView 列を 1 つに統合されました。
- データ フィールドの値を表現するのにには、別の Web コントロールを使用します。 表示する方法を説明しました、`HiredDate`カレンダー コントロールを使用する値します。
- 基になるデータに基づき、状態情報を表示します。 中に、`Employees`テーブルには、従業員が、そのジョブのした日数が返す列が含まれていない場合、TemplateField と書式設定メソッドを使用して前のチュートリアルでは GridView の例ではこのような情報を表示することができました。

GridView で利用できる同じ TemplateFields 機能も、DetailsView コントロールで使用できます。 このチュートリアルでは 1 つの製品を含む 2 つの TemplateFields DetailsView を使用して一度に表示します。 最初の TemplateField を組み合わせて、 `UnitPrice`、`UnitsInStock`と`UnitsOnOrder`DetailsView は 1 行にデータ フィールド。 2 番目の TemplateField の値が表示されます、`Discontinued`フィールドが、書式設定メソッドを使用して、"YES"を表示する場合は`Discontinued`は`True`、"NO"を返します。


[![2 つの TemplateFields を使用して、表示をカスタマイズするには](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**図 1**: 2 つの TemplateFields を使用して、表示をカスタマイズする ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))。


それでは、始めましょう!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>手順 1: データを DetailsView にバインドします。

BoundFields だけを格納している DetailsView コントロールを作成して開始し、および新しい TemplateFields を追加またはとして TemplateFields 既存 BoundFields に変換する最も簡単なことがよくあります TemplateFields を使用する場合に、前のチュートリアルで説明したように必要な. そのため、このチュートリアルを開始、デザイナーを使用してページを DetailsView を追加し、製品のリストを返す、ObjectDataSource にバインドします。 各製品の非ブール値フィールドと 1 つのブール値フィールド (Discontinued) 用の CheckBoxField BoundFields で、次の手順は、DetailsView を作成します。

開く、`DetailsViewTemplateField.aspx`ページし、DetailsView をツールボックスからデザイナーにドラッグします。 呼び出す新しい ObjectDataSource コントロールを追加することも、DetailsView のスマート タグから、`ProductsBLL`クラスの`GetProducts()`メソッド。


[![GetProducts() メソッドを呼び出す新しい ObjectDataSource コントロールを追加します。](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**図 2**: 新しい ObjectDataSource コントロールを追加するには、その呼び出します、`GetProducts()`メソッド ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))。


このレポートの削除、 `ProductID`、 `SupplierID`、 `CategoryID`、および`ReorderLevel`BoundFields します。 BoundFields の次に、順序を変更できるように、`CategoryName`と`SupplierName`直後後 BoundFields に表示されます、 `ProductName` BoundField します。 自由に調整、`HeaderText`プロパティとするとして BoundFields の書式設定プロパティに合わせて参照してください。 など、GridView で、[フィールド] ダイアログ ボックス (DetailsView のスマート タグのフィールドの編集リンクをクリックしてアクセスできます)、または宣言の構文を通じて、これら BoundField レベルの編集を実行できます。 最後に、DetailsView のクリア`Height`と`Width`DetailsView を許可するにはプロパティの値が表示されるデータに基づいた展開への制御し、スマート タグのページングを有効にするチェック ボックスをオンします。

これらの変更を行った後、DetailsView コントロールの宣言型マークアップを次のようになります。


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

ブラウザーでページを表示する時間がかかります。 この時点で製品の名前、カテゴリ、供給業者、価格、在庫数、ユニットの順序、およびその提供が中止された状態を示す行で表示されている 1 つの製品 (Chai) を表示する必要があります。


[![製品の詳細については、一連の BoundFields を使用して表示されます。](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**図 3**: BoundFields のシリーズを使用して、製品の詳細が表示されます ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))。


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>手順 2: は、価格、在庫、および注文のユニットを 1 つの行に組み合わせること

DetailsView は 1 行のデータ、 `UnitPrice`、 `UnitsInStock`、および`UnitsOnOrder`フィールド。 結合できますこれらのデータ フィールドを 1 つの行を TemplateField に新しい TemplateField を追加するか、既存のいずれかを変換することで`UnitPrice`、 `UnitsInStock`、および`UnitsOnOrder`BoundFields TemplateField にします。 既存 BoundFields の変換は個人的には必要に応じて、中には、新しい TemplateField を追加することで練習しましょう。

フィールドのダイアログ ボックスを表示する DetailsView のスマート タグのフィールドの編集リンクをクリックして開始します。 次に、新しい TemplateField を追加し、設定、`HeaderText`プロパティを「価格およびインベントリ」に移動その it は、上に配置されますので、新しい TemplateField、 `UnitPrice` BoundField します。


[![DetailsView コントロールに新しい TemplateField を追加します。](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**図 4**: DetailsView コントロールに新しい TemplateField を追加 ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))。


この新しい TemplateField で現在表示されている値が含まれているので、 `UnitPrice`、 `UnitsInStock`、および`UnitsOnOrder`BoundFields、今回は削除します。

この手順の最後のタスクが定義には、`ItemTemplate`価格およびできるインベントリ TemplateField マークアップがコントロールの宣言構文からインターフェイス デザイナー、または手動での編集、DetailsView のテンプレートを使用するかを実行します。 同様に、GridView、DetailsView のテンプレートの編集インターフェイスをスマート タグのテンプレートの編集リンクをクリックしてアクセスできます。 ここからは、ドロップダウン リストから編集し、ツールボックスのすべての Web コントロールを追加するテンプレートを選択できます。

このチュートリアルでは、価格と在庫 TemplateField のラベル コントロールを追加することで開始`ItemTemplate`します。 次に、ラベルの Web コントロールのスマート タグから DataBindings の編集リンクをクリックし、バインド、`Text`プロパティを`UnitPrice`フィールド。


[![UnitPrice データ フィールドにラベルのテキストのプロパティをバインドします。](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**図 5**: ラベルのバインド`Text`プロパティを`UnitPrice`データ フィールド ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))。


## <a name="formatting-the-price-as-a-currency"></a>価格の通貨として書式設定

これにより、Label Web コントロールの価格と在庫 TemplateField 表示されます、選択した製品の料金だけです。 図 6 は、ブラウザーで表示したときにこれまで、進行状況のスクリーン ショットを示します。


[![価格と在庫 TemplateField には、価格が表示されます。](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**図 6**:、価格と在庫 TemplateField には、価格が表示されます ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))。


製品の価格が、通貨として書式設定しないことに注意してください。 BoundField、書式設定は設定して、`HtmlEncode`プロパティを`False`と`DataFormatString`プロパティを`{0:formatSpecifier}`します。 TemplateField、ただし、書式設定の手順については、する必要がありますまたはで指定データ バインディング構文 (ASP.NET ページの分離コード クラスなど)、アプリケーションのコード内のどこかに定義されている書式指定メソッドを使用しています。

Label Web コントロールで使用されるデータ バインディング構文の書式設定を指定するには、ラベルのスマート タグから DataBindings の編集リンクをクリックして、DataBindings ダイアログ ボックスに戻ります。 形式のドロップダウン リストで直接書式設定命令を入力したり、定義済み書式指定文字列のいずれかを選択できます。 BoundField のような`DataFormatString`プロパティを使用して指定の書式設定が`{0:formatSpecifier}`します。

`UnitPrice` 、適切なドロップダウン リストの値を選択するかを入力して指定された通貨の書式設定を使用してフィールド`{0:C}`を手動でします。


[![価格を通貨として書式設定します。](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**図 7**: 価格の通貨として書式設定 ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))。


2 番目のパラメーターとして書式設定の仕様が示されます、宣言によって、`Bind`または`Eval`メソッド。 宣言型マークアップでは、次のデータ バインド式でデザイナーの結果から作成したばかりの設定:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>残りのデータ フィールドを TemplateField に追加します。

この時点で表示し、書式設定しました、`UnitPrice`データは、価格と在庫 TemplateField、フィールドしますが、表示する必要があります、`UnitsInStock`と`UnitsOnOrder`フィールド。 以下の価格と、かっこ内の行にこれらのファイルを表示してみましょう。 デザイナーのテンプレート編集インターフェイスから、テンプレート内でカーソルを配置および表示するテキストを入力するだけでこのようなマークアップを追加できます。 また、このマークアップは、宣言構文内で直接入力できます。

価格と在庫 TemplateField、価格と在庫情報が表示されるように static のマークアップ、ラベルの Web コントロール、およびデータ バインディング構文を追加するようになります。

*UnitPrice*  
(**在庫/注文:** *UnitsInStock* / *UnitsOnOrder*)

このタスクを実行した後、DetailsView の宣言型マークアップは、次のような検索する必要があります。


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

これらの変更を単一 DetailsView 行に、価格とインベントリ情報を統合しました。


[![価格とインベントリ情報は、1 つの行に表示されます。](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**図 8**: 1 つの行に、価格とインベントリ情報が表示されます ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))。


## <a name="step-3-customizing-the-discontinued-field-information"></a>手順 3: 提供が中止されたフィールドの情報をカスタマイズします。

`Products`テーブルの`Discontinued`列は、製品は廃止されているかどうかを示すビット値。 ブール値のフィールドなどのデータ ソース コントロールにバインドは、DetailsView (または GridView) を使用すると、`Discontinued`などの非ブール値のフィールドは CheckBoxFields として実装されます`ProductID`、`ProductName`でとして実装されますBoundFields します。 データ フィールドの値は True と unchecked をそれ以外の場合がある場合にチェックされているチェック ボックスがオフ、CheckBoxField を表示します。

CheckBoxField を表示するのではなく、代わりに表示するテキストを示す、製品が提供が中止されたかどうかたい場合があります。 これを実現するには、DetailsView から、CheckBoxField を削除して追加、BoundField でしたが`DataField`プロパティに設定されました`Discontinued`します。 これを行う時間がかかります。 この変更後、DetailsView テキストが表示されます、"True"生産中止の製品と"False"の製品がアクティブなままにします。


[![文字列は True と false の場合、提供が中止された状態を表示する使用は](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**図 9**: 文字列 True と False 使用中止状態を表示する ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))。


、"YES"と"NO"が、代わりに使用するために、文字列"True"または"False"することを想像してください。 このようなカスタマイズを TemplateField と書式設定メソッドを使用して実行できます。 書式指定メソッドは、入力パラメーターの任意の数を取得できますが、テンプレートに挿入する (文字列) として、HTML を返す必要があります。

書式設定メソッドを追加、`DetailsViewTemplateField.aspx`という名前のページの分離コード クラス`DisplayDiscontinuedAsYESorNO`を受け入れる、`Northwind.ProductsRow`オブジェクトを入力パラメーターとして文字列を返します。 前のチュートリアルでは、このメソッドで説明したように*する必要があります*としてマークする`Protected`または`Public`テンプレートからアクセスできるようにするためにします。


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

このメソッドは、入力パラメーターをチェックします (`discontinued`) である場合は、"YES"を返します`True`、"NO"を返します。

> [!NOTE]
> 書式設定メソッドが含まれるデータ フィールドを渡していた以前のチュートリアル再呼び出しに検査で`NULL`s 場合を確認するために必要、従業員の`HiredDate`プロパティの値は、データベースを持っていた`NULL`変更前に、の値アクセス、`EmployeesRow`の`HiredDate`プロパティ。 以降、このようなチェックはここで必要ありません、`Discontinued`列にデータベースを使用することはありません`NULL`割り当てられた値。 さらに、このため、メソッドがブール値を受け入れることのではなく、パラメーターの入力を受け入れる、`ProductsRow`インスタンスまたは型のパラメーター`Object`します。


完全な書式指定メソッドがこのに残っているの TemplateField から呼び出すこと`ItemTemplate`します。 削除するか、TemplateField を作成する、 `Discontinued` BoundField 新しい TemplateField を追加すると、変換、 `Discontinued` BoundField TemplateField にします。 次に、宣言型マークアップ ビューで、編集、TemplateField だけを呼び出す ItemTemplate が含まれているように、`DisplayDiscontinuedAsYESorNO`メソッド、現在の値を渡して`ProductRow`インスタンスの`Discontinued`プロパティ。 これを使用してアクセスできる、`Eval`メソッド。 具体的には、マークアップを TemplateField のようになります。


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

これにより、 `DisplayDiscontinuedAsYESorNO` 、DetailsView を表示するときに呼び出されるメソッドに渡して、`ProductRow`インスタンスの`Discontinued`値。 以降、`Eval`メソッド型の値を返します`Object`が、`DisplayDiscontinuedAsYESorNO`メソッド型の入力パラメーターが必要ですが`Boolean`、キャスト、`Eval`メソッドに値を返す`Boolean`します。 `DisplayDiscontinuedAsYESorNO`メソッドは"YES"を返します、または"NO"の値に応じて受信します。 返される値は、この DetailsView に表示されている行 (図 10 参照)。


[![はいまたは NO の値は、生産中止の行に示すようになりました](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**図 10**: YES または NO の値は、生産中止の行に示すようになりました ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))。


## <a name="summary"></a>まとめ

DetailsView コントロールで TemplateField で高度な柔軟性は状況に最適ですが他のフィールドのコントロールで使用可能なよりもデータを表示するのには、場所。

- 複数のデータ フィールドを 1 つの GridView 列に表示される必要があります。
- データがプレーン テキストではなく、Web コントロールを使用して最適な表現します。
- 出力は、メタデータを表示するなど、データを再フォーマットでは、基になるデータによって異なります。

いるに対し TemplateFields DetailsView の基になるデータのレンダリングで柔軟性の高い、DetailsView 出力も感じて少し角張った HTML 内の行として各フィールドが表示された`<table>`します。

FormView コントロールに表示される出力の構成で柔軟性の高いが用意されています。 フィールドが、一連のテンプレートだけで済みますが、フォーム ビューに含まれていない (`ItemTemplate`、 `EditItemTemplate`、`HeaderTemplate`など)。 次のチュートリアルに表示されるレイアウトのよりきめ細かい制御を実現するために、FormView を使用する方法を見ていきます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Dan Jagers でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](using-templatefields-in-the-gridview-control-vb.md)
> [次へ](using-the-formview-s-templates-vb.md)
