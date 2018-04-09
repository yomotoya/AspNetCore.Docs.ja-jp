---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
title: 検証コントロールを追加、編集し、インターフェイス (c#) を挿入する |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、詳細を提供するには、EditItemTemplate と Web コントロールのデータの後に検証コントロールを追加することがいかに簡単思いますしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2086cb1a-ab78-49ae-9c0b-03891c69776a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs
msc.type: authoredcontent
ms.openlocfilehash: 3df80984d15e13efddc52497d600d396d775d365
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-c"></a>検証コントロールを追加、編集し、インターフェイス (c#) を挿入します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_CS.exe)または[PDF のダウンロード](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/datatutorial19cs1.pdf)

> このチュートリアルでがどのように簡単により安全な方法でユーザー インターフェイスを提供するには、EditItemTemplate と Web コントロールのデータの後に検証コントロールを追加することが表示されます。


## <a name="introduction"></a>はじめに

例では、GridView および DetailsView コントロールおについて説明してきました、過去、3 つのチュートリアルがすべてされてとから成る BoundFields CheckBoxFields (フィールドの種類、GridView または DetailsView をデータ ソースにバインドするときに、Visual Studio によって自動的に追加コントロールのスマート タグから)。 GridView または DetailsView で行を編集するには、読み取り専用ではない BoundFields それらは、テキスト ボックスに変換されます、エンドユーザーが、既存のデータを変更できます。 同様に、ときに挿入する新しいレコードを DetailsView コントロール、その BoundFields を`InsertVisible`プロパティに設定されている`true`(既定)、ユーザーが新しいレコードのフィールドの値を指定できる空のテキスト ボックスとしてレンダリングされます。 同様に、標準、読み取り専用のインターフェイスの無効化されている CheckBoxFields は、編集およびインターフェイスの挿入で有効になっているチェック ボックスをオンに変換されます。

既定値を編集し、BoundField と CheckBoxField のインターフェイスを挿入することができます、中に、インターフェイスには、あらゆる種類の検証が不足しています。 かどうか、ユーザーを犯しデータ エントリの省略することなど、`ProductName`フィールドまたはに対して無効な値を入力する`UnitsInStock`(-50) など、例外は内から発生する、アプリケーションのアーキテクチャの深さを取得します。 中で示したように、この例外を適切に処理することができます、[前のチュートリアル](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)、理想的には、編集、またはユーザー インターフェイスを挿入することが含まれます検証コントロール ユーザーがこのような無効なデータを入力することを防ぐためにには、最初に配置します。

カスタマイズされた編集または挿入するインターフェイスを提供するのには、TemplateField と BoundField や CheckBoxField を置き換える必要があります。 個のあるディスカッション トピック TemplateFields で、 [GridView コントロールを使用して TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)と[DetailsView コントロールを使用して TemplateFields](../custom-formatting/using-templatefields-in-the-detailsview-control-cs.md)チュートリアルについては、複数含めることができますテンプレートの定義は、別の行の状態用のインターフェイスを区切ります。 TemplateField の`ItemTemplate`はために使用される読み取り専用のフィールドや、DetailsView または GridView コントロール内の行を表示する際に対し、`EditItemTemplate`と`InsertItemTemplate`編集し、それぞれのモードを挿入するのに使用するインターフェイスを指定します。

このチュートリアルでは会いしましょうに TemplateField の検証コントロールを追加することがいかに簡単`EditItemTemplate`と`InsertItemTemplate`より安全な方法でユーザー インターフェイスを提供します。 具体的には、このチュートリアルでは、例で作成した、 [、イベントに関連付けられている挿入、更新、および削除を確認する](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)適切な検証をインクルードするインターフェイスのチュートリアルと編集と挿入を強化します。

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-csmd"></a>手順 1: から例をレプリケートする[挿入、更新、および削除に関連付けられているイベントを確認します。](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)

[、イベントに関連付けられている挿入、更新、および削除を確認する](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)名前と、編集可能な GridView では、製品の価格を表示するページを作成したチュートリアルです。 さらに、ページには、DetailsView が含まれますが`DefaultMode`プロパティに設定された`Insert`、それによって常に挿入モードで表示します。 この DetailsView からユーザーでした新製品の価格と名前を入力、挿入 をクリックし、システムに加えることが (図 1 を参照してください)。


[![前の例では新製品の追加および編集する既存のユーザーに](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image1.png)

**図 1**:、前の例により、ユーザーに新しい製品の追加と編集の既存の ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image3.png))


このチュートリアルの目標は、検証コントロールを提供するには、DetailsView と GridView を強化することです。 具体的には、この検証ロジックを行います。

- 挿入または商品を編集するときに、名前を指定することが必要
- レコードを挿入するときに、価格が提供されることが必要お、価格が要求されますが、gridview のプログラム ロジックを使用して、レコードを編集するには、`RowUpdating`イベント ハンドラーがまだ存在して、以前のチュートリアルから
- 価格の値が有効な通貨の形式であることを確認します。

最初の例をレプリケートする必要があります拡張検証をインクルードする前の例で見ることができます、前に、`DataModificationEvents.aspx`このチュートリアルでは、ページにページ`UIValidation.aspx`です。 両方の経由でコピーする必要があります。 これを実現する、`DataModificationEvents.aspx`ページの宣言型マークアップとそのソース コードです。 まず、次の手順を実行することによって宣言型マークアップをコピーします。

1. 開く、 `DataModificationEvents.aspx` Visual Studio でのページ
2. ページの宣言型マークアップ (ページの下部にある [ソース] ボタンをクリック) を参照してください。
3. 内のテキストをコピー、`<asp:Content>`と`</asp:Content>`図 2 タグ (行 3 44 ~)。


[![内のテキストのコピー、 &lt;Asp:content&gt;コントロール](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image4.png)

**図 2**: 内のテキストのコピー、`<asp:Content>`コントロール ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image6.png))


1. 開く、`UIValidation.aspx`ページ
2. ページの宣言型マークアップに移動します。
3. 内のテキストを貼り付けます、`<asp:Content>`コントロール。

ソース コードをコピーするには、開く、`DataModificationEvents.aspx.cs`ページし、同様のテキストをコピー*内*、`EditInsertDelete_DataModificationEvents`クラスです。 3 つのイベント ハンドラーをコピー (`Page_Load`、 `GridView1_RowUpdating`、および`ObjectDataSource1_Inserting`)、操作を行いますが、**いない**クラス宣言をコピーまたは`using`ステートメントです。 コピーしたテキストを貼り付ける*内*、`EditInsertDelete_UIValidation`クラス`UIValidation.aspx.cs`です。

コンテンツおよびからコード経由で移動した後`DataModificationEvents.aspx`に`UIValidation.aspx`ブラウザーで進行状況をテストします。 同じ出力し、これら 2 つのページのそれぞれに同じ機能が表示されます (のスクリーン ショットを図 1 に戻って`DataModificationEvents.aspx`の動作)。

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>手順 2: TemplateFields、BoundFields に変換します。

編集および挿入のインターフェイスに検証コントロールを追加するには、DetailsView および GridView コントロールで使用される BoundFields を TemplateFields に変換する必要があります。 これを実現するには、リンクをクリックして、列の編集とフィールドの編集 スマート タグの GridView と DetailsView のそれぞれ。 BoundFields のそれぞれを選択し、「このフィールドを TemplateField に変換」リンクをクリックします。


[![TemplateFields DetailsView のおよび GridView の BoundFields の各変換します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image7.png)

**図 3**: 各 BoundFields に TemplateFields DetailsView のおよび GridView の変換 ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image9.png))


BoundField をフィールド ダイアログ ボックスを通じて TemplateField に変換するを備えた、読み取り専用で、編集、および挿入すると同じインターフェイス自体 BoundField TemplateField が生成されます。 次のマークアップの宣言の構文を示しています、 `ProductName` DetailsView TemplateField に変換された後でフィールド。

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample1.aspx)]

この TemplateField に自動的に作成される 3 つのテンプレートがあることに注意してください`ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`です。 `ItemTemplate` 1 つのデータ フィールドの値が表示されます (`ProductName`) ラベル Web コントロールを使用するときに、`EditItemTemplate`と`InsertItemTemplate`テキスト ボックスのデータ フィールドに関連付ける ボックスに Web コントロールにデータ フィールドの値を提示`Text`双方向データ バインドを使用するプロパティです。 削除することはおのみを使用している DetailsView でこのページを挿入するため、`ItemTemplate`と`EditItemTemplate`2 つの TemplateFields からそのまま害はありませんが。

GridView は、組み込み DetailsView の機能を挿入する、GridView への変換をサポートしていないので`ProductName`のみ結果のフィールドを TemplateField、`ItemTemplate`と`EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample2.aspx)]

"Convert このフィールドを TemplateField"をクリックすると、Visual Studio が持つテンプレートが変換された BoundField のユーザー インターフェイスを模倣 TemplateField を作成します。 これは、ブラウザーからこのページにアクセスして確認できます。 TemplateFields の動作と外観が同じである、エクスペリエンスに BoundFields が代わりに使用する場合があります。

> [!NOTE]
> 自由に、必要に応じて、テンプレートの編集のインターフェイスをカスタマイズできます。 たとえば、可能性がありますが必要なテキスト ボックス、 `UnitPrice` TemplateFields よりも小さいテキスト ボックスとしてレンダリングされます、 `ProductName`  テキスト ボックス。 これを実現するには、テキスト ボックスを設定することができます`Columns`プロパティを適切な値を使用して絶対幅を提供したり、`Width`プロパティ。 次のチュートリアルは、代替データ エントリ Web コントロールをテキスト ボックスを置き換えることで、編集用のインターフェイスを完全にカスタマイズする方法が表示されます。


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>手順 3: GridView への検証コントロールの追加`EditItemTemplate`s

データ エントリ フォームを構築するときに、重要なは、すべての必須フィールドに入力し、提供されているすべての入力が有効で適切にフォーマットされた値です。 ユーザーの入力が有効であることを確認するには、ASP.NET には、1 つの入力コントロールの値を検証するために使用するよう設計されている 5 つの組み込みの検証コントロールが用意されています。

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx)値が指定されていることを確認
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx)検証値を別の Web コントロールの値または定数値、または値の形式が指定されたデータ型に対して有効であることを確認
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx)値が値の範囲内であることを確認
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx)に対して値を検証、[正規表現](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) 、ユーザー定義のカスタム メソッドに対して値を検証

これら 5 つのコントロールの詳細については、チェック アウト、[検証コントロール セクション](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx)の[ASP.NET クイック スタート チュートリアル](https://asp.net/QuickStart/aspnet/)です。

このチュートリアルの必要があります DetailsView と GridView の両方で RequiredFieldValidator を使用して`ProductName`TemplateFields と DetailsView ので RequiredFieldValidator `UnitPrice` TemplateField です。 さらに、両方のコントロールを CompareValidator を追加する必要があります`UnitPrice`TemplateFields に確実に価格を入力とを持つ 0 以上の値が有効な通貨形式で表示されます。

> [!NOTE]
> ASP.NET を while 1.x が同じこれら 5 つの検証コントロール、ASP.NET 2.0 には多数の機能強化が追加、主要な 2 つのクライアント側スクリプトの中のサポートの Internet Explorer 以外のブラウザーとに、ページ上のパーティション検証コントロールする機能検証のグループです。 詳細については、検証コントロール 2.0 の新機能を参照してください[ASP.NET 2.0 の検証コントロールを分解](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)です。


必要な検証コントロールを追加して始めましょう、 `EditItemTemplate` GridView の TemplateFields で s。 これを実現するには、テンプレート編集インターフェイスを表示する GridView のスマート タグからのテンプレートの編集リンクをクリックします。 ここでは、ドロップダウン リストから編集するテンプレートを選択できます。 編集のインターフェイスを拡張するので、検証コントロールを追加する必要があります、`ProductName`と`UnitPrice`の`EditItemTemplate`s。


[![ProductName および UnitPrice の EditItemTemplates を拡張する必要があります。](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image10.png)

**図 4**: 拡張する必要があります、`ProductName`と`UnitPrice`の`EditItemTemplate`s ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image12.png))


`ProductName` `EditItemTemplate`、テキスト ボックスの後に配置する、テンプレートの編集インターフェイスにツールボックスからドラッグして、RequiredFieldValidator を追加します。


[![ProductName EditItemTemplate、RequiredFieldValidator に追加します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image13.png)

**図 5**: に RequiredFieldValidator を追加、 `ProductName` `EditItemTemplate` ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image15.png))


すべての検証コントロールは、1 つの ASP.NET Web コントロールの入力を検証することによって動作します。 そのため、追加したばかり RequiredFieldValidator がでテキスト ボックスに対して検証する必要があることを示すために必要があります、 `EditItemTemplate`; これを実現するには、検証コントロールの[ControlToValidate プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)に、`ID`の適切な Web コントロールです。 テキスト ボックスには現在はではなくあまり特徴のない`ID`の`TextBox1`が、適切なものに変更してみましょう。 テンプレートのテキスト ボックスをクリックし、次に、[プロパティ] ウィンドウから次のように変更します。、`ID`から`TextBox1`に`EditProductName`です。


[![テキスト ボックスの ID を EditProductName に変更します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image16.png)

**図 6**: 変更するテキスト ボックスの`ID`に`EditProductName`([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image18.png))


次に、設定、RequiredFieldValidator`ControlToValidate`プロパティを`EditProductName`です。 最後に、設定、 [ErrorMessage プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)「製品の名前を指定する必要があります」に、 [Text プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)に"\*"です。 `Text`プロパティの値、指定した場合は、検証が失敗した場合、検証コントロールによって表示されるテキストです。 `ErrorMessage`場合 ValidationSummary コントロールによって必要となるプロパティの値が使用される、`Text`プロパティの値を省略すると、`ErrorMessage`プロパティの値が無効な入力の検証コントロールによって表示されるテキストでもします。

RequiredFieldValidator のこれら 3 つのプロパティを設定した後、画面は、図 7 のようになります。


[![RequiredFieldValidator の ControlToValidate、エラー メッセージ、およびテキストのプロパティを設定します。](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image19.png)

**図 7**: 設定 RequiredFieldValidator の`ControlToValidate`、 `ErrorMessage`、および`Text`プロパティ ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image21.png))


追加された RequiredFieldValidator で、 `ProductName` `EditItemTemplate`、すべてのままにするために必要な検証を追加するが、 `UnitPrice` `EditItemTemplate`です。 ここで決定するには、このページのため、`UnitPrice`が省略可能な場合に、レコードを編集、する必要はありません、RequiredFieldValidator を追加します。 ただし、ことを確認する CompareValidator を追加する必要がありますには、 `UnitPrice`、指定した場合、通貨の形式が正しく、0 以上。

CompareValidator を追加する前に、 `UnitPrice` `EditItemTemplate`、最初から TextBox Web コントロールの ID を変更してみましょう`TextBox2`に`EditUnitPrice`です。 この変更を行った後、CompareValidator を追加設定、`ControlToValidate`プロパティを`EditUnitPrice`、その`ErrorMessage`「価格より大きいまたは 0 に等しい必要があります、通貨記号を含めることはできません」プロパティとその`Text`プロパティ"\*".

示すために、`UnitPrice`値が 0 以上でなければ、CompareValidator を設定する必要があります[演算子プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)に`GreaterThanEqual`、その[ValueToCompare プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)「0」、およびその[Type プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)に`Currency`です。 宣言型の構文を示します、 `UnitPrice` TemplateField の`EditItemTemplate`これらの変更が加えられた後。

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample3.aspx)]

これらの変更を行った後は、ブラウザーでページを開きます。 名前を省略するか、製品の編集時に、無効な価格の値を入力しようとすると、テキスト ボックスの横にアスタリスクが表示されます。 図 8 に示す $19.95 などの通貨記号を含む価格の値が無効と見なされます。 CompareValidator の`Currency``Type`では、桁区切り記号 (コンマ、ピリオド、カルチャの設定によってなど) と先頭プラスまたはマイナス記号が、*いない*通貨記号を許可します。 この動作は、編集用のインターフェイスが現在表示ユーザーを perplex 可能性があります、`UnitPrice`通貨形式を使用します。

> [!NOTE]
> 注意してください、*イベントに挿入、更新、および削除すると、関連付けられている*BoundField の設定のチュートリアル`DataFormatString`プロパティを`{0:c}`通貨として書式設定するためにします。 さらに、設定、`ApplyFormatInEditMode`プロパティを true が GridView の原因の書式を設定するインターフェイスが編集、`UnitPrice`通貨として。 Visual Studio がこれらの設定を記載し、テキスト ボックスの書式設定、BoundField を TemplateField に変換するときに`Text`プロパティをデータ バインド構文を使用して通貨として`<%# Bind("UnitPrice", "{0:c}") %>`です。


[![無効な入力でテキスト ボックスの横にアスタリスクが表示されます。](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image22.png)

**図 8**: An アスタリスクが表示されますへの無効な入力でテキスト ボックス ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image24.png))


として検証機能の中では、ユーザーが許されないレコードの編集時に、通貨記号を手動で削除します。 この問題を解決するのには、3 つのオプションがあります。

1. 構成、`EditItemTemplate`できるように、`UnitPrice`値が通貨としてフォーマットされていません。
2. CompareValidator の削除と正しくを正しく書式設定された通貨値をチェックする RegularExpressionValidator に置き換えることによって、通貨記号を入力できます。 ここでの問題は、見た目のよい通貨値を検証する正規表現があると、カルチャ設定を組み込むたい場合に、コードの記述を必要とするとします。
3. 検証コントロールを完全に削除し、GridView にサーバー側の検証ロジックに依存`RowUpdating`イベント ハンドラー。

この演習ではオプション 1 を使用してみましょう。 現在、 `UnitPrice` 、テキスト ボックス内のデータ バインド式のための通貨として書式設定、 `EditItemTemplate`:`<%# Bind("UnitPrice", "{0:c}") %>`です。 バインド ステートメントを変更`Bind("UnitPrice", "{0:n2}")`、有効桁数の数値 2 桁の数字で、結果の書式を設定します。 これは、宣言の構文を直接または行えますから databindings の編集 リンクをクリックして、 `EditUnitPrice`  テキスト ボックスに、 `UnitPrice` TemplateField の`EditItemTemplate`(図 9 と 10 を参照してください)。


[![テキスト ボックスの databindings の編集 リンクをクリックします。](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image25.png)

**図 9**: テキスト ボックスの databindings の編集 リンクをクリックして ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image27.png))


[![バインド ステートメントで指定し、書式指定子](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image28.png)

**図 10**: で書式指定子を指定、`Bind`ステートメント ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image30.png))


この変更により、編集インターフェイスで書式設定された価格は桁区切り記号としてコンマ、小数点区切り文字としてピリオドが含まれていますが、通貨記号をオフのままにします。

> [!NOTE]
> `UnitPrice` `EditItemTemplate` RequiredFieldValidator、議論が生じることへのポストバックのアプリケーションと更新を開始するロジックが含まれていません。 ただし、`RowUpdating`イベント ハンドラーからコピー、 *、イベントに関連付けられている挿入、更新、および削除を確認する*チュートリアルには、確実にするプログラムのチェックが含まれています、`UnitPrice`が指定されています。 このロジックを削除するのままにしてもかまいません-、またはに RequiredFieldValidator を追加、 `UnitPrice` `EditItemTemplate`です。


## <a name="step-4-summarizing-data-entry-problems"></a>手順 4: 概要データ エントリの問題を作成します。

ASP.NET には、5 つの検証コントロールのほか、 [ValidationSummary コントロール](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)が表示される、`ErrorMessage`の無効なデータが検出された検証コントロール。 この概要データは、モーダル、クライアント側のメッセージ ボックス データベースを介して、web ページ上のテキストとして表示できます。 検証問題の概要を作成するクライアント側のメッセージ ボックスを含めるには、このチュートリアルを強化してみましょう。

これを実現するには、ツールボックスからデザイナーに ValidationSummary コントロールをドラッグします。 検証コントロールの場所は関係ありません実際には、メッセージ ボックス データベースとしてのみの概要を表示するように構成することがあるため。 コントロールを追加すると、次のように設定します。 その[ShowSummary プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)に`false`とその[ShowMessageBox プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)に`true`です。 この追加すると、検証エラーは、クライアント側のメッセージ ボックスにまとめたものです。


[![クライアント側のメッセージ ボックスの妥当性確認エラーをまとめたもの](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image31.png)

**図 11**: クライアント側のメッセージ ボックスの 検証エラーをまとめたもの ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>手順 5: DetailsView への検証コントロールの追加`InsertItemTemplate`

このチュートリアルでは DetailsView の挿入のインターフェイスに検証コントロールを追加します。 DetailsView のテンプレートへの検証コントロールの追加の処理手順 3 で確認するのと同じです。そのため、この手順では、タスクによって breeze おします。 GridView ので行ったように`EditItemTemplate`s、ことをお勧めする名前を変更する、`ID`からあまり特徴のない、テキスト ボックスの s`TextBox1`と`TextBox2`に`InsertProductName`と`InsertUnitPrice`です。

追加する RequiredFieldValidator、 `ProductName` `InsertItemTemplate`です。 設定、`ControlToValidate`を`ID`テンプレートで、テキスト ボックスの`Text`プロパティを"\*"およびその`ErrorMessage`プロパティを「、製品の名前を指定する必要があります」です。

`UnitPrice`に RequiredFieldValidator 新しいレコードを追加するときに、このページに必要な追加、 `UnitPrice` `InsertItemTemplate`、設定、 `ControlToValidate`、 `Text`、および`ErrorMessage`プロパティ適切にします。 CompareValidator を最後に、追加、 `UnitPrice` `InsertItemTemplate`同様に、構成、 `ControlToValidate`、 `Text`、 `ErrorMessage`、 `Type`、 `Operator`、および`ValueToCompare`プロパティ、で行ったのと同じように`UnitPrice`の gridview の CompareValidator`EditItemTemplate`です。

これらの検証コントロールを追加すると、新製品をその名前が指定されていない場合、またはその価格が負の数値である場合、システムに追加または不正に書式設定することはできません。


[![DetailsView の挿入インターフェイスに検証ロジックが追加されました](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image34.png)

**図 12**: DetailsView の挿入インターフェイスに検証ロジックが追加されました ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>手順 6: 検証グループへの検証コントロールをパーティション分割

検証コントロールの 2 つの論理的な異なるセットのページで構成されます: GridView に対応する編集のインターフェイスとインターフェイスを挿入の DetailsView に対応するものです。 既定では、ポストバックが発生したときに*すべて*ページ上の検証コントロールがチェックされます。 ただし、レコードの編集時に DetailsView の挿入インターフェイスの検証コントロールを検証したくありません。 図 13、ユーザーが完全に有効な値を持つ製品を編集するときに、現在の課題を示しています、挿入インターフェイスの名前と価格の値が空白であるためをクリックすると更新プログラムによって、検証エラーが発生します。


[![製品の更新が原因で検証コントロールを挿入するインターフェイスの起動](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image37.png)

**図 13**: 製品の更新が原因で検証コントロールを挿入するインターフェイスの Fire ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image39.png))


ASP.NET 2.0 の検証コントロールを使用して検証グループに分割することができます、`ValidationGroup`プロパティです。 グループ内の検証コントロールのセットを関連付けるを設定するだけの`ValidationGroup`プロパティを同じ値にします。 このチュートリアルでは、次のように設定します。、`ValidationGroup`を GridView の TemplateFields の検証コントロールのプロパティ`EditValidationControls`と`ValidationGroup`を DetailsView の TemplateFields のプロパティ`InsertValidationControls`です。 これらの変更は、宣言型マークアップ内で直接実行することができますか、デザイナーを使用する場合は、[プロパティ] ウィンドウから、テンプレートのインターフェイスを編集します。

だけでなく、検証コントロール、ボタンとボタン関連のコントロールを ASP.NET 2.0 で含めることも、`ValidationGroup`プロパティです。 検証グループの検証コントロールの有効性チェックは、同じボタンによってポストバックが誘発される場合にのみ`ValidationGroup`プロパティの設定。 DetailsView の [挿入] ボタンをトリガーするためになど、`InsertValidationControls`検証グループが CommandField を設定する必要があります`ValidationGroup`プロパティを`InsertValidationControls`(図 14 を参照してください)。 さらに、設定、GridView の CommandField の`ValidationGroup`プロパティを`EditValidationControls`です。


[![DetailsView の InsertValidationControls CommandField の ValidationGroup プロパティ](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image40.png)

**図 14**: 設定 DetailsView の CommandField の`ValidationGroup`プロパティを`InsertValidationControls`([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/_static/image42.png))


これらの変更後 TemplateFields とコマンドの DetailsView および GridView のようになります次。

DetailsView の TemplateFields と CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample4.aspx)]

GridView の CommandField と TemplateFields

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample5.aspx)]

この時点で編集固有の検証コントロール起動 GridView の更新 ボタンがクリックされたときにのみ、insert 固有の検証コントロール火災 DetailsView の挿入 ボタンをクリックすると、図 13 で強調表示されている問題を解決する場合にのみです。 ただし、この変更により、ValidationSummary コントロールは表示されなくなります無効なデータを入力するときにします。 ValidationSummary コントロールにも含まれる、`ValidationGroup`プロパティと、そのグループの検証でそれらの検証コントロールの概要情報は表示のみです。 そのため、このページのいずれかに 2 つの検証コントロールを必要があります、`InsertValidationControls`検証グループの 1 つと`EditValidationControls`です。

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs/samples/sample6.aspx)]

この追加すると、チュートリアルが完了しました。

## <a name="summary"></a>まとめ

BoundFields には、挿入と編集インターフェイスの両方を指定できる、インターフェイスはカスタマイズできません。 一般的には、編集とユーザーが有効な形式で必須の入力を入力することを確認するインターフェイスを挿入する検証コントロールを追加します。 これを実現する TemplateFields に、BoundFields を変換して、検証コントロールを適切なテンプレートに追加する必要があります。 この例を拡張してこのチュートリアルでは、 *、イベントに関連付けられている挿入、更新、および削除を確認する*インターフェイスおよび GridView の挿入のチュートリアルでは、両方 DetailsView に検証コントロールを追加します。インターフェイスを編集します。 さらに、ValidationSummary コントロールを使用して検証の概要情報を表示する方法と、ページ上の検証コントロールを個別の検証グループにパーティション分割する方法を説明しました。

このチュートリアルで説明したとおり、TemplateFields は検証コントロールを追加する拡張するインターフェイスを編集し、挿入を許可します。 TemplateFields 拡張することも入力 Web コントロールを追加する適切な Web コントロールによって置き換えられるテキスト ボックスを有効にします。 次のチュートリアルでは外部キーを編集するときに最適なデータ バインディングの DropDownList コントロールでテキスト ボックス コントロールを置き換える方法が表示されます (など`CategoryID`または`SupplierID`で、`Products`テーブル)。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Liz Shulok および Zack Jones がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
> [次へ](customizing-the-data-modification-interface-cs.md)
