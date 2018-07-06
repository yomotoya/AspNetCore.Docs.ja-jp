---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: 検証コントロールを追加、編集および挿入インターフェイス (VB) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、詳細を提供するには、後とデータ Web コントロールの後に検証コントロールを追加するがいかに簡単か見て.
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: a9527cad45e506268a9d5f19a445cae939345540
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839977"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>検証コントロールを追加、編集および挿入インターフェイス (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe)または[PDF のダウンロード](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> このチュートリアルではより安全な方法でユーザー インターフェイスを提供するには、後とデータ Web コントロールの後に検証コントロールを追加するがいかに簡単か見ていきます。


## <a name="introduction"></a>はじめに

例で GridView および DetailsView コントロールしました、過去、3 つのチュートリアルがすべてされてから成る BoundFields CheckBoxFields (フィールドの種類、GridView や DetailsView をデータ ソースにバインドするときに、Visual Studio によって自動的に追加コントロールのスマート タグ経由)。 GridView、DetailsView 内の行を編集するには、読み取り専用ではないこれら BoundFields は、テキスト ボックスに変換されます、エンドユーザーが、既存のデータを変更できます。 同様に、ときに挿入する新しいレコードに DetailsView コントロール、その BoundFields を`InsertVisible`プロパティに設定されて`True`(既定)、ユーザーが新しいレコードのフィールドの値を指定できる空のテキスト ボックスとしてレンダリングされます。 同様に、CheckBoxFields で、標準的な読み取り専用のインターフェイスは無効になっては、有効になっているチェック ボックスを編集および挿入インターフェイスに変換されます。

編集および挿入 BoundField と CheckBoxField インターフェイスの既定値は便利ですが、インターフェイスには、あらゆる種類の検証が不足しています。 省略することなどのデータ エントリ ミスを行うかどうか、`ProductName`フィールドまたはに対して無効な値を入力する`UnitsInStock`(-50) など、例外が発生しますからのアプリケーション アーキテクチャ内で。 中で説明するよう、この例外を適切に処理することができます、[前のチュートリアル](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)、編集、またはユーザー インターフェイスの挿入をユーザーがこのような無効なデータを入力するを防ぐために検証コントロールを含める理想的には、最初に配置します。

カスタマイズされた編集または挿入のインターフェイスを提供するためには、BoundField または CheckBoxField TemplateField に置き換える必要があります。 TemplateFields、された説明のトピックで、 [GridView コントロールで TemplateFields を使用して](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)と[DetailsView コントロールで TemplateFields を使用して](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md)のチュートリアルでは、複数ので構成できますテンプレートを定義するインターフェイスを異なる行の状態を分離します。 TemplateField の`ItemTemplate`を使用するか GridView、DetailsView コントロールで行または読み取り専用フィールドを表示するときに対し、`EditItemTemplate`と`InsertItemTemplate`編集モードをそれぞれ挿入に使用するインターフェイスを示します。

このチュートリアルでは見てに TemplateField の検証コントロールを追加するがいかに簡単か`EditItemTemplate`と`InsertItemTemplate`より安全な方法でユーザー インターフェイスを提供します。 具体的には、このチュートリアルでは、例で作成した、 [、イベントに関連付けられている挿入、更新、および削除の確認](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)適切な検証をインクルードするインターフェイスのチュートリアルと編集および挿入を強化します。

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>手順 1: レプリケーションの例では、[挿入、更新、および削除に関連付けられているイベントを調べる](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

[、イベントに関連付けられている挿入、更新、および削除の確認](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)チュートリアルの名前と、編集可能な GridView では、製品の価格を一覧表示するページを作成しました。 さらに、ページには、DetailsView が含まれますが`DefaultMode`プロパティに設定されました`Insert`、それによって、常に、挿入モードでレンダリングします。 この DetailsView からのユーザーでした新製品の価格と名前を入力、挿入 をクリックしておよびシステムに加えることが (図 1 参照)。


[![前の例では、新しい製品を追加および編集する既存のユーザーには、します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**図 1**:、前の例により、ユーザーに新しい製品の追加と編集の既存の ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))。


このチュートリアルの目標は、検証コントロールを提供するには、DetailsView と GridView を拡張します。 具体的には、この検証ロジックを行います。

- 挿入または製品を編集するときに名前を提供することが必要です。
- レコードを挿入するときに、価格が提供することが必要です。価格、必要がありますもいますが、gridview のプログラム ロジックを使用にレコードを編集するには、`RowUpdating`以前のチュートリアルからに既に存在するイベント ハンドラー
- 価格に入力された値が有効な通貨の形式であることを確認します。

最初の例をレプリケートする必要がありますに検証を含める前の例の拡張について考えることができます、前に、`DataModificationEvents.aspx`このチュートリアルでは、ページにページ`UIValidation.aspx`します。 両方の経由でコピーする必要があります。 これを実現する、`DataModificationEvents.aspx`ページの宣言型マークアップとそのソース コード。 まず、次の手順を実行することによって宣言型マークアップにコピーします。

1. 開く、 `DataModificationEvents.aspx` Visual Studio でのページ
2. ページの宣言型マークアップ (ページの下部にある [ソース] ボタンをクリックします) に移動します。
3. 内のテキストをコピー、`<asp:Content>`と`</asp:Content>`図 2 タグ (行 3 ~ 44)。


[![内のテキストのコピー、 &lt;Asp:content&gt;コントロール](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**図 2**: 内のテキストのコピー、`<asp:Content>`コントロール ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))。


1. 開く、`UIValidation.aspx`ページ
2. ページの宣言型マークアップに移動します。
3. 内のテキストを貼り付け、`<asp:Content>`コントロール。

ソース コードをコピーするには、開く、`DataModificationEvents.aspx.vb`ページし、同様のテキストをコピー*内*、`EditInsertDelete_DataModificationEvents`クラス。 3 つのイベント ハンドラーをコピー (`Page_Load`、 `GridView1_RowUpdating`、および`ObjectDataSource1_Inserting`)、操作を行いますが、**いない**クラス宣言をコピーします。 コピーしたテキストを貼り付ける*内*、`EditInsertDelete_UIValidation`クラス`UIValidation.aspx.vb`します。

コンテンツとコードからの上に移動した後は`DataModificationEvents.aspx`に`UIValidation.aspx`ブラウザーで、進行状況をテストする少し。 出力は同じで、これら 2 つのページのそれぞれで同じ機能が発生することがわかります (のスクリーン ショットを図 1 に戻って`DataModificationEvents.aspx`の動作)。

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>手順 2: TemplateFields、BoundFields に変換します。

検証コントロールを編集および挿入インターフェイスに追加するには、し GridView、DetailsView コントロールで使用される BoundFields を TemplateFields に変換する必要があります。 これを実現するにはの スマート タグの GridView および DetailsView の列の編集とフィールドの編集リンクをそれぞれクリックします。 BoundFields のそれぞれを選択し、「このフィールドを TemplateField に変換」リンクをクリックします。


[![TemplateFields の GridView、DetailsView の BoundFields の各変換します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**図 3**: 各 BoundFields に TemplateFields の GridView、DetailsView の変換 ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))。


TemplateField フィールド ダイアログ ボックスで、BoundField に変換するには、BoundField 自体として同じ読み取り専用、編集、および挿入インターフェイスを示す TemplateField が生成されます。 次のマークアップの宣言構文を示しています、`ProductName`フィールドを TemplateField に変換された後、DetailsView で。

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

注この TemplateField が自動的に作成された 3 つのテンプレートを持っている`ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`します。 `ItemTemplate` 1 つのデータ フィールドの値が表示されます (`ProductName`) ラベル Web コントロールを使用するときに、`EditItemTemplate`と`InsertItemTemplate`、テキスト ボックスのデータ フィールドに関連付けるテキスト ボックスに Web コントロールでデータ フィールドの値を提示`Text`プロパティが双方向データ バインドを使用します。 削除することはのみを使用している、DetailsView でこのページを挿入するため、`ItemTemplate`と`EditItemTemplate`2 つの TemplateFields からそのままでも問題はありませんが。

GridView、DetailsView の機能を挿入する、変換、GridView の組み込みをサポートしていないので`ProductName`だけ結果でフィールドを TemplateField、`ItemTemplate`と`EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

"Convert このフィールドを TemplateField"をクリックすると、Visual Studio が持つテンプレートが変換された BoundField のユーザー インターフェイスを模倣 TemplateField を作成します。 これを確認するには、ブラウザーからこのページを参照してください。 TemplateFields の動作と外観が同じである、エクスペリエンスを BoundFields が代わりに使用する場合があります。

> [!NOTE]
> 自由に、必要に応じて、テンプレートの編集インターフェイスをカスタマイズできます。 など、テキスト ボックスにすることがあります、`UnitPrice`よりも小さいボックスとしてレンダリング TemplateFields、`ProductName`テキスト ボックス。 これを実現するには、テキスト ボックスの設定できます`Columns`プロパティに適切な値を使用して絶対の幅を提供または、`Width`プロパティ。 次のチュートリアルでは、テキスト ボックスに Web コントロールの代替データ エントリを置き換えることにより、編集用のインターフェイスを完全にカスタマイズする方法がわかります。


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>手順 3: を GridView の検証コントロールを追加する`EditItemTemplate`s

データ入力フォームを構築するときに、必須の各フィールドに入力し、指定されたすべての入力は有効で正しい形式の値であることは重要です。 ユーザーの入力が有効であることを確認するには、ASP.NET では、1 つの入力コントロールの値の検証に使用するように設計された 5 つの組み込みの検証コントロールが用意されています。

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx)により値が指定されているようになります
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx)値を別の Web コントロールの値または定数の値を検証しますまたは、値の形式が指定されたデータ型に対して有効であることを確認。
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx)値が値の範囲内であることを確認
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx)に対して値を検証、[正規表現](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx)に対して、ユーザー定義のカスタム メソッドの値を検証します。

これら 5 つのコントロールの詳細については、チェック アウト、[検証コントロール セクション](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx)の[ASP.NET クイック スタート チュートリアル](https://asp.net/QuickStart/aspnet/)します。

このチュートリアルでの GridView と DetailsView の RequiredFieldValidator を使用する必要があります`ProductName`TemplateFields と DetailsView ので、RequiredFieldValidator `UnitPrice` TemplateField します。 さらに、両方のコントロールに、CompareValidator を追加する必要があります`UnitPrice`TemplateFields 確実に価格を入力が 0 以上の値を持つしが有効な通貨の形式で表示されます。

> [!NOTE]
> While ASP.NET 1.x がこれらの同じ 5 つの検証コントロール、ASP.NET 2.0 にはさまざまな機能強化が追加、メインの 2 つのクライアント側スクリプトの中、サポート、Internet Explorer 以外のブラウザーにページ上のパーティションの検証コントロールの機能検証グループ。 2.0 の検証コントロールの新機能に関する詳細についてを参照してください[詳細に分析する ASP.NET 2.0 の検証コントロール](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)します。


必要な検証コントロールを追加してみましょう、 `EditItemTemplate` GridView の TemplateFields で s。 これを実現するには、テンプレートの編集インターフェイスを表示する GridView のスマート タグからのテンプレートの編集リンクをクリックします。 ここでは、ドロップダウン リストから編集するには、どのテンプレートを選択できます。 編集インターフェイスを拡張するので、検証コントロールを追加する必要があります、`ProductName`と`UnitPrice`の`EditItemTemplate`秒。


[![ProductName と UnitPrice の EditItemTemplates を拡張する必要があります。](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**図 4**: 拡張する必要があります、`ProductName`と`UnitPrice`の`EditItemTemplate`s ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))。


`ProductName` `EditItemTemplate`、テキスト ボックスの後に配置する、テンプレートの編集インターフェイスに、ツールボックスからドラッグして、RequiredFieldValidator を追加します。


[![ProductName 後に、RequiredFieldValidator を追加します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**図 5**: の RequiredFieldValidator を追加、 `ProductName` `EditItemTemplate` ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))。


すべての検証コントロールは、1 つの ASP.NET Web コントロールの入力を検証することで動作します。 そのため、RequiredFieldValidator を追加しましたが、テキスト ボックスに対して検証する必要があることを示す必要があります、 `EditItemTemplate`; この検証コントロールの設定によって実現されます[ControlToValidate プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)に、`ID` Web コントロールを適切なのです。 テキスト ボックスは現在ではなくあまりが`ID`の`TextBox1`が、適切なものに変更してみましょう。 テンプレートのテキスト ボックスをクリックし、次に、[プロパティ] ウィンドウから次のように変更します。、`ID`から`TextBox1`に`EditProductName`します。


[![テキスト ボックスの ID を EditProductName に変更します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**図 6**: 変更、テキスト ボックスの`ID`に`EditProductName`([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))。


RequiredFieldValidator を次に、設定`ControlToValidate`プロパティを`EditProductName`します。 最後に、設定、 [ErrorMessage プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)「製品の名前を提供する必要があります」と[Text プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)に"\*"。 `Text`プロパティの値を指定されている場合は、検証が失敗した場合、検証コントロールによって表示されるテキスト。 `ErrorMessage`場合を必要なプロパティ値が ValidationSummary コントロールによって使用される、`Text`プロパティの値を省略すると、`ErrorMessage`プロパティの値が無効な入力の検証コントロールによって表示されるテキストでも。

これら 3 つ、RequiredFieldValidator のプロパティを設定した後、画面を図 7 のようなはずです。


[![RequiredFieldValidator の ControlToValidate、エラー メッセージ、およびテキストのプロパティを設定します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**図 7**: 設定 RequiredFieldValidator の`ControlToValidate`、 `ErrorMessage`、および`Text`プロパティ ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))。


RequiredFieldValidator を追加すると、 `ProductName` `EditItemTemplate`残っては必要な検証を追加することはすべて、 `UnitPrice` `EditItemTemplate`。 しました、このページのため、`UnitPrice`レコードを編集する省略可能な場合に、追加、RequiredFieldValidator 必要はありません。 ただし、いることを確認する CompareValidator を追加する必要があります、 `UnitPrice`、指定した場合は、0 以上でありが正しく、通貨として書式設定します。

CompareValidator を追加する前に、 `UnitPrice` `EditItemTemplate`、最初からテキスト ボックスに Web コントロールの ID を変更してみましょう`TextBox2`に`EditUnitPrice`します。 この変更を行った後、CompareValidator の追加設定その`ControlToValidate`プロパティを`EditUnitPrice`その`ErrorMessage`「価格は 0 以上にする必要がありますし、通貨記号を含めることはできません、」プロパティとその`Text`プロパティを"\*".

いることを示す、`UnitPrice`値が 0 以上、CompareValidator を設定する必要があります[演算子プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)に`GreaterThanEqual`その[ValueToCompare プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)「0」をその[プロパティを入力して](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)に`Currency`します。 次の宣言型構文に示す、 `UnitPrice` TemplateField の`EditItemTemplate`後、これらの変更が加えられました。

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

これらの変更を行った後は、ブラウザーでページを開きます。 名前を省略するか、製品を編集するときに、無効な価格の値を入力しようとすると、テキスト ボックスの横にある場合は、アスタリスクが表示されます。 図 8 に示すよう 19.95 ドルなどの通貨記号を含む価格の値は無効と見なされます。 CompareValidator の`Currency` `Type` (コンマ、ピリオド、カルチャ設定によってなど) の桁区切り記号と先頭プラスまたはマイナス記号、できますが、*いない*通貨記号を許可します。 この動作は、編集インターフェイスをレンダリング現在ユーザーに perplex 可能性があります、`UnitPrice`通貨形式を使用します。

> [!NOTE]
> することを思い出してください、*イベントは、挿入、更新、および削除に関連付けられている*BoundField の設定のチュートリアル`DataFormatString`プロパティを`{0:c}`を通貨として書式設定するためにします。 さらに、設定、`ApplyFormatInEditMode`プロパティを true に、書式設定するインターフェイスの編集の原因で、GridView、`UnitPrice`通貨として。 Visual Studio がこれらの設定の説明し、テキスト ボックスの書式設定、BoundField を TemplateField に変換するときに`Text`プロパティをデータ バインディング構文を使用して通貨として`<%# Bind("UnitPrice", "{0:c}") %>`します。


[![無効な入力をテキスト ボックスの横にアスタリスクが表示されます。](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**図 8**: An アスタリスクが表示されますへの無効な入力をテキスト ボックス ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))。


として検証機能の中では、ユーザーが許容されないレコードを編集するときに、通貨記号を手動で削除する必要があります。 この問題を解決するのには、3 つのオプションがあります。

1. 構成、`EditItemTemplate`ように、`UnitPrice`値が通貨として書式設定されません。
2. CompareValidator の削除と、正しく書式設定された通貨値を正しくチェック RegularExpressionValidator に置き換えることによって、通貨記号を入力するユーザーを許可します。 ここでの問題は、通貨値を検証する正規表現が美しいことし、カルチャ設定を反映する場合は、コードの記述になります。
3. 検証コントロールを完全に削除し、gridview のサーバー側の検証ロジックに依存`RowUpdating`イベント ハンドラー。

この演習ではオプション 1 を使用します。 現在、`UnitPrice`は、テキスト ボックス内のデータ バインディング式のための通貨として書式設定、 `EditItemTemplate`:`<%# Bind("UnitPrice", "{0:c}") %>`します。 バインド ステートメントを変更する`Bind("UnitPrice", "{0:n2}")`、有効桁数 2 桁の番号として、結果の書式を設定します。 これ行う宣言型構文またはから DataBindings の編集リンクをクリックして直接、 `EditUnitPrice`  テキスト ボックスに、 `UnitPrice` TemplateField の`EditItemTemplate`(図 9 および 10 を参照してください)。


[![テキスト ボックスの DataBindings の編集リンクをクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**図 9**: テキスト ボックスの [DataBindings の編集リンクをクリックします ([フルサイズの画像を表示する] をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))。


[![バインド ステートメントで、書式指定子を指定します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**図 10**: 指定の書式指定子、`Bind`ステートメント ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))。


この変更によりは、編集インターフェイスで書式設定された料金は、グループ区切り記号としてコンマと、小数点区切り文字としてピリオドが含まれていますが、通貨記号をオフのままです。

> [!NOTE]
> `UnitPrice` `EditItemTemplate` RequiredFieldValidator、ポストバックを発生したりして、更新ロジックを開始する許可が含まれていません。 ただし、`RowUpdating`からコピーされたイベント ハンドラー、 *、イベントに関連付けられている挿入、更新、および削除の確認*チュートリアルにあるプログラムによるチェックが含まれています、`UnitPrice`提供されます。 このロジックを削除するには、そのまま使用でお気軽に、または、RequiredFieldValidator への追加、 `UnitPrice` `EditItemTemplate`します。


## <a name="step-4-summarizing-data-entry-problems"></a>手順 4: 集約データ エントリの問題

ASP.NET には、5 つの検証コントロールのほか、 [ValidationSummary コントロール](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)が表示される、`ErrorMessage`の無効なデータが検出された検証コントロール。 この概要データは、モーダル、クライアント側のメッセージ ボックスから web ページ上のテキストとして表示できます。 クライアント側のメッセージ ボックス検証問題の集計を含めるには、このチュートリアルを強化しましょう。

これを実現するには、ツールボックスからデザイナーに ValidationSummary コントロールをドラッグします。 検証コントロールの位置は関係ありません本当にのみ、メッセージ ボックスとしての概要を表示するように構成していきますので。 コントロールを追加すると、次のように設定します。 その[ShowSummary プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)に`False`とその[ShowMessageBox プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)に`True`します。 これにより、検証エラーは、クライアント側のメッセージ ボックスにまとめたものです。


[![検証エラーがクライアント側のメッセージ ボックスにまとめます](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**図 11**: クライアント側のメッセージ ボックスで、検証エラーをまとめます ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))。


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>手順 5: DetailsView への検証コントロールの追加`InsertItemTemplate`

このチュートリアルでは、DetailsView の挿入インターフェイスに検証コントロールを追加します。 DetailsView のテンプレートに検証コントロールを追加するプロセスは、手順 3 で説明されているのと同じです。そのため、この手順では、タスクによって breeze します。 GridView のと同様`EditItemTemplate`s、ことをお勧めする名前を変更する、 `ID` 、あまりからテキスト ボックスの s`TextBox1`と`TextBox2`に`InsertProductName`と`InsertUnitPrice`します。

RequiredFieldValidator を追加、 `ProductName` `InsertItemTemplate`します。 設定、`ControlToValidate`を`ID`テンプレートでは、テキスト ボックスの`Text`プロパティを"\*"とその`ErrorMessage`プロパティを「製品の名前を指定する必要があります」。

`UnitPrice`の RequiredFieldValidator を追加する新しいレコードを追加するときに、このページに必要な、 `UnitPrice` `InsertItemTemplate`設定、その`ControlToValidate`、`Text`と`ErrorMessage`プロパティ適切にします。 CompareValidator を最後に、追加、 `UnitPrice` `InsertItemTemplate`同様に、構成、 `ControlToValidate`、 `Text`、 `ErrorMessage`、 `Type`、 `Operator`、および`ValueToCompare`を使用したときと同じようにプロパティ`UnitPrice`の gridview の CompareValidator`EditItemTemplate`します。

これらの検証コントロールを追加すると、新しい成果物の名前が指定されていない場合、またはその価格が負の数値である場合は、システムに追加または不正にフォーマットすることはできません。


[![DetailsView の挿入インターフェイスに検証ロジックが追加されました](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**図 12**: DetailsView の挿入インターフェイスに検証ロジックが追加されました ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))。


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>手順 6: 検証グループに、検証コントロールをパーティション分割

ページは、2 つの異なる論理的に検証コントロールのセットで構成されます。 インターフェイスの編集、GridView に対応すると、DetailsView に対応するインターフェイスを挿入するのです。 ポストバックが発生した場合、既定*すべて*ページの検証コントロールがチェックされます。 ただし、レコードを編集するときに、検証する検証コントロールを DetailsView の挿入インターフェイスのほしくありません。 図 13 ユーザーが、製品を完全に有効な値を編集するときに、現在のジレンマを示していますを挿入のインターフェイスで、名前と価格値は空であるために、クリックして更新プログラムが、検証エラーを発生します。


[![挿入インターフェイスの検証コントロールを起動すると、製品を更新](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**図 13**: を挿入するインターフェイスの検証コントロールを起動すると、製品を更新 ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))。


ASP.NET 2.0 の検証コントロールは、検証を使用してグループに分割することができます、`ValidationGroup`プロパティ。 グループ内の検証コントロールのセットを関連付けるを設定するだけの`ValidationGroup`プロパティを同じ値にします。 このチュートリアルでは、設定、`ValidationGroup`を GridView の TemplateFields の検証コントロールのプロパティ`EditValidationControls`と`ValidationGroup`に DetailsView の TemplateFields のプロパティ`InsertValidationControls`します。 これらの変更は、宣言型マークアップで直接実行できますか、デザイナーの使用時のプロパティ ウィンドウからインターフェイスのテンプレートを編集します。

だけでなく、検証コントロール、ボタン、および ASP.NET 2.0 のボタンに関連するコントロールを含めることも、`ValidationGroup`プロパティ。 検証グループの検証コントロールの有効性チェックは同じボタンでポストバックが発生した場合にのみ`ValidationGroup`プロパティの設定。 DetailsView の [挿入] ボタンをトリガーするためになど、 `InsertValidationControls` CommandField を設定する必要があります。 検証グループ`ValidationGroup`プロパティを`InsertValidationControls`(図 14 を参照してください)。 GridView をさらに、設定 [commandfield] の`ValidationGroup`プロパティを`EditValidationControls`します。


[![セット、DetailsView の InsertValidationControls CommandField の ValidationGroup プロパティ](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**図 14**: DetailsView の設定 [commandfield] の`ValidationGroup`プロパティを`InsertValidationControls`([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))。


これらの変更後の GridView、DetailsView TemplateFields とコマンドに次のようななる必要があります。

DetailsView の TemplateFields および CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

GridView の CommandField と TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

この時点で、編集に固有の検証コントロール起動 GridView の Update ボタンがクリックされたときのみと insert 固有の検証コントロールの火災 DetailsView の挿入 ボタンをクリックすると、図 13 で強調表示されている問題を解決する場合にのみです。 ただし、この変更により、ValidationSummary コントロールは表示されなくなります無効なデータを入力するときにします。 ValidationSummary コントロールも含まれています、`ValidationGroup`プロパティと、そのグループの検証でそれらの検証コントロールの概要情報は表示のみ。 そのため、このページのいずれかで 2 つの検証コントロールに必要があります、`InsertValidationControls`検証グループの 1 つと`EditValidationControls`します。

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

これにより、チュートリアルが完了しました。

## <a name="summary"></a>まとめ

BoundFields には、両方の挿入と編集インターフェイスを提供できます、インターフェイス、カスタマイズできません。 一般的には、編集および挿入インターフェイスに必要な入力の有効な形式で入力をことを確認する検証コントロールを追加します。 これを実現する TemplateFields に、BoundFields を変換して、検証コントロールを適切なテンプレートに追加する必要があります。 このチュートリアルでは拡張の例では、 *、イベントに関連付けられている挿入、更新、および削除の確認*インターフェイスと GridView の挿入のチュートリアルでは、両方、DetailsView に検証コントロールを追加インターフェイスを編集します。 さらに、ValidationSummary コントロールを使用する検証の概要情報を表示する方法と、ページの検証コントロールを個別の検証グループに分割する方法を説明しました。

このチュートリアルで説明したように TemplateFields を含める検証コントロールを編集および挿入インターフェイスを使用できます。 TemplateFields 拡張することも、追加の入力 Web コントロールを含める方が適切な Web コントロールによって置き換えられるテキスト ボックスを有効にします。 次のチュートリアルでは、外部キーを編集するときに最適なデータ バインド DropDownList コントロールと TextBox コントロールを置換する方法を見ていきます (など`CategoryID`または`SupplierID`で、`Products`テーブル)。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Liz Shulok と Zack Jones でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [次へ](customizing-the-data-modification-interface-vb.md)
