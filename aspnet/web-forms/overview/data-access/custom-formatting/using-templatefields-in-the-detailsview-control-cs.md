---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: DetailsView コントロール (c#) で TemplateFields の使用 |Microsoft ドキュメント
author: rick-anderson
description: GridView で利用できる同じ TemplateFields 機能も DetailsView コントロールで使用できます。 このチュートリアルでは 1 つの製品を表示しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f1d2e8312451c0bd1b3aba448963317f5fe06029
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879109"
---
<a name="using-templatefields-in-the-detailsview-control-c"></a>DetailsView コントロール (c#) で TemplateFields の使用
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe)または[PDF のダウンロード](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> GridView で利用できる同じ TemplateFields 機能も DetailsView コントロールで使用できます。 このチュートリアルでは 1 つの製品を含む TemplateFields DetailsView を使用して、一度に表示します。


## <a name="introduction"></a>はじめに

TemplateField、BoundField、CheckBoxField、内、およびその他のデータ フィールドのコントロールよりも高いレベルの表示データの柔軟性を提供します。 [前のチュートリアル](using-templatefields-in-the-gridview-control-cs.md)を GridView で、TemplateField の使用について説明しました。

- 1 つの列に複数のデータ フィールドの値を表示します。 具体的には、どちらも、`FirstName`と`LastName`フィールドが 1 つの GridView 列に統合されました。
- データ フィールドの値を表現するのにには、別の Web コントロールを使用します。 表示する方法を説明しました、`HiredDate`カレンダー コントロールを使用する値します。
- 基になるデータに基づき、状態情報を表示します。 中に、`Employees`テーブルに従業員がしたジョブの日数を表す列が含まれていない場合、GridView の例では TemplateField と書式設定メソッドの使用と前のチュートリアルでこのような情報を表示することができました。

GridView で利用できる同じ TemplateFields 機能も DetailsView コントロールで使用できます。 このチュートリアルでは 1 つの製品を含む 2 つの TemplateFields DetailsView を使用して、一度に表示します。 最初の TemplateField が結合されて、 `UnitPrice`、 `UnitsInStock`、および`UnitsOnOrder`DetailsView は 1 行にデータ フィールドです。 2 番目の TemplateField の値が表示されます、`Discontinued`フィールドでは、"YES"を表示する場合に書式指定メソッドを使用して、`Discontinued`は`true`、"NO"を返します。


[![2 つの TemplateFields を使用して、表示をカスタマイズするには](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**図 1**: 2 つの TemplateFields を使用して、表示をカスタマイズする ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))


開始しましょう!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>手順 1: DetailsView にデータのバインド

TemplateFields BoundFields だけを含む DetailsView コントロールを作成することで開始し、追加新しい TemplateFields またはとして TemplateFields 既存 BoundFields に変換する最も簡単なは多くの場合、使用するときに、前のチュートリアルで説明したように必要. そのため、DetailsView をデザイナーでのページに追加して、製品のリストを返す ObjectDataSource にバインドすることによって、このチュートリアルを開始します。 次の手順は、各の製品の非ブール値フィールド、および 1 つのブール値フィールド (生産中止) の CheckBoxField、DetailsView を BoundFields で作成されます。

開く、`DetailsViewTemplateField.aspx`ページし、DetailsView をツールボックスからデザイナーにドラッグします。 DetailsView のスマート タグからを起動する新しい ObjectDataSource コントロールを追加するを選択して、`ProductsBLL`クラスの`GetProducts()`メソッドです。


[![GetProducts() メソッドを呼び出す新しい ObjectDataSource コントロールの追加します。](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**図 2**: その呼び出します新しい ObjectDataSource コントロールを追加、`GetProducts()`メソッド ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))


このレポートの削除、 `ProductID`、 `SupplierID`、 `CategoryID`、および`ReorderLevel`BoundFields です。 BoundFields の次に、順序を変更できるように、`CategoryName`と`SupplierName`BoundFields 直後、 `ProductName` BoundField です。 自由に調整する、`HeaderText`プロパティとも BoundFields の書式設定のプロパティに合わせてを参照してください。 同様に、GridView とこれらの BoundField レベルの編集実行できます (DetailsView のスマート タグの表示でフィールドの編集リンクをクリックしてアクセス可能) フィールド ダイアログ ボックス、または宣言の構文を使用。 DetailsView の最後に、クリア`Height`と`Width`DetailsView を許可するためにプロパティの値がコントロールに表示されるデータに基づいた展開をスマート タグのページングを有効にするチェック ボックスをオンします。

これらの変更を加えたら、DetailsView コントロールの宣言型マークアップを次のようになります。


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

すぐをブラウザーでページを表示します。 この時点で、製品の名前、カテゴリ、供給業者、価格、在庫数、順序、上のユニットおよび提供が中止された状態を示す行で表示されている 1 つの製品 (Chai) が表示されます。


[![BoundFields の系列を使用して、製品の詳細が表示されます。](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**図 3**: BoundFields の系列を使用して、製品の詳細が表示されます ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>手順 2: は、1 つの行に価格、在庫、および Order の単位を組み合わせること。

DetailsView が 1 行のデータ、 `UnitPrice`、 `UnitsInStock`、および`UnitsOnOrder`フィールドです。 結合できますこれらのデータ フィールドが単一行に TemplateField に新しい TemplateField を追加することで、または既存のいずれかに変換して`UnitPrice`、 `UnitsInStock`、および`UnitsOnOrder`を TemplateField BoundFields です。 個人 (_n) 既存 BoundFields を変換する、中には、新しい TemplateField を追加することで練習しましょう。

DetailsView のスマート タグのフィールド ダイアログ ボックスを呼び出すことでフィールドの編集リンクをクリックして開始します。 次に、新しい TemplateField を追加し、設定、`HeaderText`プロパティを「価格と在庫」および移動新しい TemplateField 上ことが配置されているため、 `UnitPrice` BoundField です。


[![DetailsView コントロールに新しい TemplateField を追加します。](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**図 4**: DetailsView コントロールに新しい TemplateField を追加 ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))


この新しい TemplateField で現在表示されている値が含まれているので、 `UnitPrice`、 `UnitsInStock`、および`UnitsOnOrder`BoundFields、それらを削除します。

定義するのには、この手順の最後のタスク、`ItemTemplate`価格とできるインベントリ TemplateField マークアップが DetailsView のテンプレートの編集コントロールの宣言の構文をデザイナーで、または手動でのインターフェイスを使用するかを実行します。 同様に、GridView DetailsView のテンプレートを編集するインターフェイスは、スマート タグのテンプレートの編集リンクをクリックしてアクセスできます。 ここでは、ドロップダウン リストから編集し、ツールボックスからすべての Web コントロールを追加するテンプレートを選択できます。

このチュートリアルでは、価格と在庫 TemplateField にラベル コントロールを追加することによって、起動`ItemTemplate`です。 次に、ラベルの Web コントロールのスマート タグから databindings の編集 リンクをクリックし、バインド、`Text`プロパティを`UnitPrice`フィールドです。


[![ラベルのテキストのプロパティは UnitPrice データ フィールドにバインドします。](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**図 5**: ラベルのバインド`Text`プロパティを`UnitPrice`データ フィールド ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>価格を通貨として書式設定

この追加ラベル Web コントロール価格と在庫 TemplateField ようになりましたが表示されます、選択した製品の価格の情報だけです。 図 6 は、ブラウザーで表示したときにこれまで、進行状況のスクリーン ショットを示します。


[![価格と在庫 TemplateField 価格を示しています。](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**図 6**:、価格と在庫 TemplateField 価格を示しています ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))


製品の価格が通貨として書式設定いないことに注意してください。 BoundField、書式設定は設定して、`HtmlEncode`プロパティを`false`と`DataFormatString`プロパティを`{0:formatSpecifier}`です。 TemplateField、ただし、任意の書式設定命令指定してくださいまたは (ASP.NET ページの分離コード クラスなど)、アプリケーションのコード内のどこかに定義されている書式指定メソッドを使用して、データ バインドの構文でします。

構文については、データ バインド Label Web コントロールで使用される書式設定を指定するには、ラベルのスマート タグから databindings の編集 リンクをクリックして DataBindings ダイアログ ボックスに戻ります。 形式のドロップダウン リストで直接書式設定命令を入力したり、定義済みの書式指定文字列のいずれかを選択できます。 BoundField の使用するような`DataFormatString`プロパティを使用して指定は、書式設定`{0:formatSpecifier}`です。

`UnitPrice`した適切なドロップダウン リストの値を選択するか、入力することで指定された通貨の書式設定を使用してフィールド`{0:C}`を手動でします。


[![価格を通貨として書式設定します。](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**図 7**: 価格を通貨として書式設定 ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))


2 番目のパラメーターとして書式設定の仕様が示されている宣言、`Bind`または`Eval`メソッドです。 宣言型マークアップに次のデータ バインド式でデザイナーの結果を行った設定:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>残りのデータ フィールドを TemplateField に追加します。

この時点で表示および書式設定した、`UnitPrice`データは、価格と在庫 TemplateField にフィールドしますが、表示する必要があります、`UnitsInStock`と`UnitsOnOrder`フィールドです。 みましょう価格以下とかっこ内の行にこれらを表示します。 デザイナーでテンプレートの編集インターフェイスから、テンプレート内でカーソルを配置および表示するテキストを入力するだけでこのようなマークアップを追加できます。 また、このマークアップは、宣言の構文で直接入力できます。

Static のマークアップ、ラベルの Web コントロール、およびデータ バインド構文を追加して、価格と在庫 TemplateField 価格とインベントリ情報を表示します。 ように次のようにします。

*UnitPrice*  
(**在庫/注文:** *UnitsInStock* / *UnitsOnOrder*)

このタスクの実行後に DetailsView の宣言型マークアップを次のようになります。


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

これらの変更と統合すれば、価格とインベントリ情報が単一の DetailsView 行にします。


[![価格とインベントリ情報は、1 つの行に表示されます。](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**図 8**:、価格とインベントリ情報は、1 つの行に表示されます ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>手順 3: 廃止されたフィールドの情報をカスタマイズします。

`Products`テーブルの`Discontinued`列は、製品が終了しているかどうかを示すビット値。 ブール値フィールドなどのデータ ソース コントロールに DetailsView (または GridView) をバインドするときに`Discontinued`、CheckBoxFields として実装されがなどの非ブール値フィールドは`ProductID`、`ProductName`など、として実装されますBoundFields です。 CheckBoxField で場合は True と unchecked がそれ以外の場合は、データ フィールドの値がチェックを無効になっているチェック ボックスとして表示されます。

CheckBoxField を表示するのではなく可能性があります代わりに表示するテキストを示す製品が提供が中止されたかどうか。 これを実現するには、DetailsView から CheckBoxField を削除してから、BoundField を追加おでしたが`DataField`プロパティに設定された`Discontinued`です。 これを行うにはしばらくを実行します。 この変更後に DetailsView にはテキスト"True"の提供が中止された製品"False"アクティブな製品用です。


[![文字列は True と false の場合、提供が中止された状態の表示に使用されます。](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**図 9**: 文字列 True と False が使用される提供が中止された状態を表示する ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))


を"YES"と"NO"が、代わりに使用するために、文字列"True"または"False"を選びましたしなかった想像してください。 このようなカスタマイズは TemplateField と書式設定メソッドを使用して実行できます。 書式指定メソッドでは、任意の数に、入力パラメーターのかかることができますが、テンプレートに挿入する (文字列) として HTML を返す必要があります。

書式設定メソッドを追加する、`DetailsViewTemplateField.aspx`という名前のページの分離コード クラス`DisplayDiscontinuedAsYESorNO`を入力パラメーターとしてブール値を受け取り、文字列を返します。 前のチュートリアルでは、このメソッドで説明したよう*必要があります*としてマークする`protected`または`public`テンプレートからアクセスできるようにするためにします。


[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

このメソッドは、入力パラメーターをチェック (`discontinued`) である場合は"YES"を返しますと`true`、それ以外の場合"NO"です。

> [!NOTE]
> 書式指定メソッドに含めることができるデータ フィールドに渡していた以前のチュートリアル リコール検査で`NULL`s ためかどうか確認するために必要と従業員の`HiredDate`プロパティの値が、データベースを持って`NULL`値の前にアクセス、`EmployeesRow`の`HiredDate`プロパティです。 以降、このようなチェックはここで必要ありません、`Discontinued`列にデータベースを使用することはありません`NULL`割り当てられた値。 さらに、このため、メソッドは、ブール値を受け入れることのではなく、パラメーターの入力を受け入れることができます、`ProductsRow`インスタンスまたは型のパラメーター`object`です。


この書式指定メソッドで完了は TemplateField から呼び出す`ItemTemplate`です。 削除するか、TemplateField を作成する、 `Discontinued` BoundField 新しい TemplateField を追加するか、変換、`Discontinued`を TemplateField BoundField です。 次に、宣言型マークアップ ビューから編集、TemplateField を呼び出す ItemTemplate だけが含まれているように、`DisplayDiscontinuedAsYESorNO`メソッド、現在の値を渡して`ProductRow`インスタンスの`Discontinued`プロパティです。 これを使用してアクセスできる、`Eval`メソッドです。 具体的には、TemplateField のマークアップはようになります。


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

これにより、 `DisplayDiscontinuedAsYESorNO` 、DetailsView を表示するときに呼び出されるメソッドに渡して、`ProductRow`インスタンスの`Discontinued`値。 以降、`Eval`メソッド型の値を返します`object`、ですが、`DisplayDiscontinuedAsYESorNO`メソッド型の入力パラメーターが必要ですが`bool`、キャストお、`Eval`メソッドに値を返す`bool`です。 `DisplayDiscontinuedAsYESorNO`し、メソッドは"YES"または"NO"、値に応じてを受信します。 戻り値は、この DetailsView に表示されている行 (図 10 参照)。


[![はいまたは NO の値は、生産中止の行に示すようになりました](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**図 10**: YES または NO の値は、提供が中止された行に表示されます ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))


## <a name="summary"></a>まとめ

DetailsView コントロールで TemplateField により、データの表示は、他のフィールド コントロールで使用できますが状況に適しているよりも柔軟性の高い場所。

- 複数のデータ フィールドを 1 つの GridView 列に表示される必要があります。
- 日付がプレーン テキストではなく Web コントロールを使用して表現されますベスト
- 出力は、メタデータを表示するなど、またはデータの再フォーマットでは、基になるデータによって異なります。

TemplateFields より柔軟に DetailsView の基になるデータのレンダリングできるようにする、DetailsView 出力もが少し角張った各フィールドが HTML 内の行として表示されると`<table>`です。

FormView コントロールには、表示される出力の構成で柔軟性の高いが用意されています。 FormView にフィールドが、一連のテンプレートではなくだけが含まれていない (`ItemTemplate`、 `EditItemTemplate`、`HeaderTemplate`など)。 FormView を使用して、次のチュートリアルでは、レンダリングされたレイアウトのさらに多くの制御を実現する方法を会いしましょう。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Dan Jagers しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](using-templatefields-in-the-gridview-control-cs.md)
> [次へ](using-the-formview-s-templates-cs.md)
