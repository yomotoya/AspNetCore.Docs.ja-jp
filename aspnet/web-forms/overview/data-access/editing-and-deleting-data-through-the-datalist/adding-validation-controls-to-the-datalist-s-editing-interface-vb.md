---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: DataList に検証コントロールを追加の編集インターフェイス (VB) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、DataList の後に編集 int です。 ユーザー間違えにくいを提供するために検証コントロールを追加するがいかに簡単か見て.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 6f7df844462b016ed74430db782005931562fda5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833573"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>検証コントロールを追加するには、DataList の編集インターフェイス (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe)または[PDF のダウンロード](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> このチュートリアルではより安全な方法での編集ユーザー インターフェイスを提供するために、DataList の後に検証コントロールを追加するがいかに簡単か見ていきます。


## <a name="introduction"></a>はじめに

チュートリアルのこれまで編集 DataList、不足している製品名や、例外が負の価格の結果などの無効なユーザー入力も事前にユーザー入力の検証の場所を編集インターフェイス データリストが含まれませんが。 [前のチュートリアル](handling-bll-and-dal-level-exceptions-vb.md)例外処理 DataList s にコードを追加する方法について確認しました`UpdateCommand`をキャッチして適切に発生した例外に関する情報を表示するには、イベント ハンドラー。 理想的には、ただし、編集インターフェイスが含まれますをユーザーが最初にこのような無効なデータを入力するを防ぐために検証コントロール。

このチュートリアルでは見て DataList s に検証コントロールを追加するがいかに簡単か`EditItemTemplate`間違えにくい編集ユーザー インターフェイスを提供するためにします。 具体的には、このチュートリアルでは、前のチュートリアルで作成した例は、し、適切な検証をインクルードする編集インターフェイスを拡張します。

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>手順 1: レプリケーションの例では、[BLL レベルと DAL レベルの例外の処理](handling-bll-and-dal-level-exceptions-vb.md)

[処理 BLL - と DAL レベルの例外](handling-bll-and-dal-level-exceptions-vb.md)チュートリアルの名前と 2 列の編集可能な DataList では、製品の価格を一覧表示するページを作成しました。 このチュートリアルの目標は、検証コントロールを追加するには、DataList s 編集インターフェイスの拡張です。 具体的には、この検証ロジックを行います。

- 製品の名前を指定する必要があります。
- 価格に入力された値が有効な通貨の形式であることを確認します。
- 価格は、負の値以降、0 以上を入力した値を必ず`UnitPrice`値は無効です

最初の例をレプリケートする必要がありますに検証を含める前の例の拡張について考えることができます、前に、`ErrorHandling.aspx`ページで、`EditDeleteDataList`フォルダーのこのチュートリアルでは、ページに`UIValidation.aspx`します。 両方の経由でコピーする必要があります。 これを実現するために、 `ErrorHandling.aspx` s 宣言型マークアップとそのソース コードをページ。 まず、次の手順を実行することによって宣言型マークアップにコピーします。

1. 開く、 `ErrorHandling.aspx` Visual Studio でのページ
2. ページの宣言型マークアップ (ページの下部にある [ソース] ボタンをクリックします) に移動します。
3. 内のテキストをコピー、`<asp:Content>`と`</asp:Content>`図 1 としてタグ (行 3 ~ 32)。


[![内のテキストのコピー、 &lt;Asp:content&gt;コントロール](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**図 2**: 内のテキストのコピー、`<asp:Content>`コントロール ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))。


1. 開く、`UIValidation.aspx`ページ
2. ページの宣言型マークアップに移動します。
3. 内のテキストを貼り付け、`<asp:Content>`コントロール。

ソース コードをコピーするには、開く、`ErrorHandling.aspx.vb`ページし、同様のテキストをコピー*内*、`EditDeleteDataList_ErrorHandling`クラス。 3 つのイベント ハンドラーをコピー (`Products_EditCommand`、 `Products_CancelCommand`、および`Products_UpdateCommand`) と共に、`DisplayExceptionDetails`メソッドだ**いない**クラス宣言をコピーまたは`using`ステートメント。 コピーしたテキストを貼り付ける*内*、`EditDeleteDataList_UIValidation`クラス`UIValidation.aspx.vb`します。

コンテンツとコードからの上に移動した後は`ErrorHandling.aspx`に`UIValidation.aspx`ブラウザーでページをテストする少し。 同じ出力を参照してください。 これら 2 つのページ (図 2 参照) のそれぞれで同じ機能を体験してください。


[![UIValidation.aspx ページは ErrorHandling.aspx の機能を模倣しています](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**図 2**:`UIValidation.aspx`ページ内の機能を模倣する`ErrorHandling.aspx`([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))。


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>手順 2: DataList s の後に、検証コントロールを追加します。

データ入力フォームを構築するときに、必須の各フィールドに入力し、指定されたすべての入力は有効で正しい形式の値であることは重要です。 ユーザーの入力値が有効であることを確認するには、ASP.NET では、1 つの入力 Web コントロールの値を検証するように設計された 5 つの組み込みの検証コントロールが用意されています。

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx)により値が指定されているようになります
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx)値を別の Web コントロールの値または定数の値を検証しますまたは、秒の値の形式が指定されたデータ型に対して有効であることを確認。
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx)値が値の範囲内であることを確認
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx)に対して値を検証、[正規表現](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx)に対して、ユーザー定義のカスタム メソッドの値を検証します。

これら 5 つのコントロールの詳細については参照、[編集および挿入インターフェイスに検証コントロールを追加](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)チュートリアルまたはチェック アウト、[検証コントロール セクション](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx)の[ASP.NET クイック スタート チュートリアル](https://quickstarts.asp.net)します。

このチュートリアルを RequiredFieldValidator、製品名の値が指定されていることを確認して、CompareValidator を十分に価格を入力が 0 以上の値を持つし、有効な通貨の形式で表示を使用する必要があります。

> [!NOTE]
> While ASP.NET 1.x がこれらの同じ 5 つの検証コントロール、ASP.NET 2.0 にはさまざまな機能強化が追加、ブラウザー Internet Explorer だけでなく、パーティションの検証コントロールをページにする機能がサポートされるクライアント側スクリプト 2 メイン検証グループ。 2.0 の検証コントロールの新機能に関する詳細についてを参照してください[詳細に分析する ASP.NET 2.0 の検証コントロール](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)します。


S DataList s に、必要な検証コントロールを追加することで開始できるように`EditItemTemplate`します。 DataList s のスマート タグからのテンプレートの編集リンクをクリックして、デザイナー、または宣言型構文を通じて、このタスクを実行できます。 デザイン ビューからのテンプレートの編集オプションを使用して、プロセスを s ステップを使用できます。 DataList の編集を選択した後`EditItemTemplate`、テンプレートの編集インターフェイスに、ツールボックスからドラッグして、RequiredFieldValidator を追加後に配置、`ProductName`テキスト ボックス。


[![ProductName テキスト ボックスの後に、後に、RequiredFieldValidator を追加します。](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**図 3**: の RequiredFieldValidator を追加、 `EditItemTemplate After` 、 `ProductName` TextBox ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))。


すべての検証コントロールは、1 つの ASP.NET Web コントロールの入力を検証することで動作します。 そのため、追加した RequiredFieldValidator が検証を示す必要があります、`ProductName`テキスト ボックスですこれは、検証コントロールの設定で[`ControlToValidate`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)を、`ID`の。適切な Web コントロール (`ProductName`、このインスタンスで)。 次に、設定、 [ `ErrorMessage`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)するには、製品の名前を指定する必要があります、 [ `Text`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)に\*します。 `Text`プロパティの値を指定されている場合は、検証が失敗した場合、検証コントロールによって表示されるテキスト。 `ErrorMessage`場合を必要なプロパティ値が ValidationSummary コントロールによって使用される、`Text`プロパティの値を省略すると、`ErrorMessage`プロパティの値が無効な入力の検証コントロールによって表示されます。

これら 3 つ、RequiredFieldValidator のプロパティを設定した後、画面を図 4 のようなはずです。


[![RequiredFieldValidator の ControlToValidate、エラー メッセージ、およびテキストのプロパティを設定します。](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**図 4**: RequiredFieldValidator s 設定`ControlToValidate`、 `ErrorMessage`、および`Text`プロパティ ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))。


RequiredFieldValidator を追加すると、`EditItemTemplate`すべて残ってはボックス製品の価格のために必要な検証を追加することです。 以降、`UnitPrice`は省略可能なレコードを編集するには、こと、RequiredFieldValidator に追加する必要はありません。 ただし、いることを確認する CompareValidator を追加する必要があります、 `UnitPrice`、指定した場合は、0 以上でありが正しく、通貨として書式設定します。

CompareValidator の追加、`EditItemTemplate`設定とその`ControlToValidate`プロパティを`UnitPrice`その`ErrorMessage`価格にプロパティが 0 以上にする必要があり、通貨記号を含めることはできませんし、その`Text`プロパティ\*. いることを示す、`UnitPrice`値が 0 以上、CompareValidator s を設定する必要があります[`Operator`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)に`GreaterThanEqual`その[`ValueToCompare`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)を 0 にし、その[`Type`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)に`Currency`します。

DataList s、これらの 2 つの検証コントロールを追加した後`EditItemTemplate`s 宣言の構文に、次のようになります。


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

これらの変更を行った後は、ブラウザーでページを開きます。 名前を省略するか、製品を編集するときに、無効な価格の値を入力しようとすると、テキスト ボックスの横にある場合は、アスタリスクが表示されます。 図 5 に示すよう 19.95 ドルなどの通貨記号を含む価格の値は無効と見なされます。 CompareValidator s `Currency` `Type` (コンマ、ピリオド、カルチャ設定によってなど) の桁区切り記号と先頭プラスまたはマイナス記号、できますが、*いない*通貨記号を許可します。 この動作は、編集インターフェイスをレンダリング現在ユーザーに perplex 可能性があります、`UnitPrice`通貨形式を使用します。


[![無効な入力をテキスト ボックスの横にアスタリスクが表示されます。](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**図 5**: An アスタリスクが表示されますへの無効な入力をテキスト ボックス ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))。


として検証機能の中では、ユーザーが許容されないレコードを編集するときに、通貨記号を手動で削除する必要があります。 さらに、存在が有効な場合、編集の入力のどちらも、更新プログラムもキャンセル ボタンのクリックされると、ポストバックを呼び出すときにインターフェイスです。 理想的には、[キャンセル] ボタンは、ユーザーの入力の妥当性に関係なく、編集済みの状態を DataList を返します。 また、DataList s で製品情報を更新する前に、ページ データが有効であることを確認する必要があります`UpdateCommand`としてユーザーのブラウザーは JavaScript サポートがないかがクライアント側ロジックをバイパスすることができます、検証コントロールのイベント ハンドラーサポートを無効になっています。

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>後の s UnitPrice TextBox から通貨記号を削除します。

CompareValidator s を使用する場合`Currency``Type`、検証対象の入力は、通貨記号を含めることはできません。 このようなシンボルの存在ととして無効な入力をマークする CompareValidator です。 通貨記号が現在の編集インターフェイスに含まれています、ただし、 `UnitPrice`  ボックスに、つまり、ユーザーがその変更を保存する前に通貨記号を明示的に削除する必要があります。 これを解決するには、3 つのオプションがあります。

1. 構成、`EditItemTemplate`ように、`UnitPrice`テキスト ボックスの値が通貨として書式設定されません。
2. CompareValidator の削除と適切に書式設定された通貨値をチェックする RegularExpressionValidator に置き換えることによって、通貨記号を入力するユーザーを許可します。 ここでチャレンジでは、通貨値を検証する正規表現は、CompareValidator として簡単ではなく、およびカルチャ設定を反映する場合は、コードの記述を必要とは。
3. 検証コントロールを完全に削除し、GridView s でカスタム サーバー側の検証ロジックに依存`RowUpdating`イベント ハンドラー。

このチュートリアルではオプション 1 を使用してのことができます。 現在、`UnitPrice`は、テキスト ボックス内のデータ バインディング式のための通貨値として書式設定、 `EditItemTemplate`:`<%# Eval("UnitPrice", "{0:c}") %>`します。 変更、`Eval`ステートメント`Eval("UnitPrice", "{0:n2}")`、有効桁数 2 桁の番号として、結果の書式を設定します。 これ行う宣言型構文またはから DataBindings の編集リンクをクリックして直接、 `UnitPrice` DataList のボックス`EditItemTemplate`します。

この変更によりは、編集インターフェイスで書式設定された料金は、グループ区切り記号としてコンマと、小数点区切り文字としてピリオドが含まれていますが、通貨記号をオフのままです。

> [!NOTE]
> 編集可能なインターフェイスから通貨書式を削除するときに便利です、テキスト ボックスの外側のテキストとして通貨記号を配置します。 これは、通貨記号を提供する必要のないユーザーにヒントとして機能します。


## <a name="fixing-the-cancel-button"></a>[キャンセル] ボタンを修正

既定では、Web の検証コントロールは、クライアント側で検証を実行する JavaScript を生成します。 ボタン、LinkButton や ImageButton がクリックされたとき、ポストバックが発生する前にクライアント側でページの検証コントロールがチェックされます。 無効なデータがある場合、ポストバックが取り消されました。 特定のボタンをデータの妥当性できない可能性があります素材です。このような場合は、無効なデータのため取り消されましたポストバックを発生は厄介です。

[キャンセル] ボタンは、このような例を示します。 ユーザーなど、s の製品名を省略すると、無効なデータを入力した製品をすべて保存する彼女はたくを決定および [キャンセル] ボタンをヒットを想像してください。 現時点では、キャンセル ボタンは、製品名が不足しているが、ポストバックを防ぐためであることを報告する ページで、検証コントロールをトリガーします。 ユーザーがテキストを入力する必要があります、`ProductName`テキスト ボックスに編集のプロセスをキャンセルするだけです。

さいわい、ボタン、LinkButton、および ImageButton が、 [ `CausesValidation`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx)ことを示すをクリックするかどうか、ボタンは、検証ロジックを開始する必要があります (既定値は`True`)。 設定は [キャンセル] ボタンの s`CausesValidation`プロパティを`False`します。

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>UpdateCommand イベント ハンドラーで有効では、入力のことを確認します。

検証コントロールによって出力されるクライアント側スクリプトにより、ユーザーが入力すると無効な入力検証コントロールのキャンセル ボタン、linkbutton コントロールで開始されたすべてのポストバック ImageButton コントロールまたは持つ`CausesValidation`プロパティは、 `True` (、既定値)。 ただし、ユーザーは旧式のブラウザーまたは JavaScript のサポートが無効になっている 1 つにアクセスして、クライアント側検証チェックは実行されません。

ASP.NET 検証コントロールのすべてのポストバック時にすぐに、検証ロジックを繰り返すし、を使用して、ページ入力の全体的な有効性を報告、 [ `Page.IsValid`プロパティ](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx)します。 ただし、ページ フローが中断されるかいずれかの値に基づく方法で停止`Page.IsValid`します。 開発者として、いることを確認する必要がありますが、`Page.IsValid`プロパティの値を持つ`True`入力データの有効なが想定するコードに進む前にします。

ページにアクセスし、製品を編集、すぎますの価格の値を入力をユーザーに JavaScript を無効になっている場合は、高価で、[更新] ボタンをクリックしては、クライアント側の検証をバイパスし、ポストバックにつながります。 、ASP.NET ページのポストバックの`UpdateCommand`イベント ハンドラーを実行し、すぎるの解析中に例外が発生する負荷の高い、 `Decimal`。 例外処理がある、このような例外は正常に処理されますが、私たちを進めるだけで、最初にを通じて遅れてから無効なデータを妨げる可能性があるため、`UpdateCommand`イベント ハンドラー場合`Page.IsValid`の値を持つ`True`します。

先頭に次のコードを追加、`UpdateCommand`イベント ハンドラーの直前に、`Try`ブロック。


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

これにより、送信されたデータが有効な場合にのみ更新する製品を試みます。 T が勝利したほとんどのユーザーが、検証コントロールのクライアント側スクリプトにより、無効なデータをポストバックできるが、ブラウザーの JavaScript サポートのない、または JavaScript のサポートのユーザーのクライアント側のチェックをバイパスし無効なデータを送信を無効にできます。

> [!NOTE]
> 鋭い読者なら、前述のとおりです、GridView でデータを更新するときに私たちは一致しませんでした t が明示的に確認する必要があります、`Page.IsValid`ページ分離コード クラスのプロパティ。 これは、GridView を参照しているため、`Page.IsValid`プロパティの値を返す場合にのみ更新プログラムを続行のみと`True`します。


## <a name="step-3-summarizing-data-entry-problems"></a>手順 3: データ エントリの問題の要約

ASP.NET には、5 つの検証コントロールのほか、 [ValidationSummary コントロール](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx)が表示される、`ErrorMessage`の無効なデータが検出された検証コントロール。 この概要データは、モーダル、クライアント側のメッセージ ボックスから web ページ上のテキストとして表示できます。 S をクライアント側のメッセージ ボックス検証問題の集計を含めるには、このチュートリアルを拡張することができます。

これを実現するには、ツールボックスからデザイナーに ValidationSummary コントロールをドラッグします。 ValidationSummary コントロールは t の場所は問題で、以降にのみ、メッセージ ボックスとしての概要を表示するように構成します。 コントロールを追加すると、次のように設定します。 その[`ShowSummary`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)に`False`とその[`ShowMessageBox`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)に`True`します。 これにより、検証エラーがクライアント側のメッセージ ボックス データベースで集計されます (図 6 参照)。


[![検証エラーがクライアント側のメッセージ ボックスにまとめます](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**図 6**: クライアント側のメッセージ ボックスで、検証エラーをまとめます ([フルサイズの画像を表示する をクリックします](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))。


## <a name="summary"></a>まとめ

このチュートリアルでは、事前に、ユーザー入力が更新のワークフローで使用する前に有効であることを確認する検証コントロールを使用して、例外の可能性を低減する方法を説明しました。 ASP.NET では、特定の Web を検査するように設計された 5 つの検証 Web コントロールが入力を制御し、入力の有効性を報告を提供します。 このチュートリアルで使用してこれら 5 つのコントロールの 2 つ、RequiredFieldValidator およびに CompareValidator s の製品名を指定して、価格がより大きいまたは 0 以下の値を持つ通貨書式を持っていることを確認しました。

上にドラッグするだけではインターフェイスの編集、DataList に検証コントロールを追加、`EditItemTemplate`からツールボックスといくつかのプロパティを設定します。 既定では、検証コントロールに自動的に生成クライアント側の検証スクリプトサーバー側の検証で累積的な結果を格納する、ポストバックの提供、`Page.IsValid`プロパティ。 ボタン、LinkButton や ImageButton がクリックされたときに、クライアント側の検証をバイパスする設定ボタン s`CausesValidation`プロパティを`False`します。 また、ポストバック時に送信されたデータのすべてのタスクを実行する前にすることを確認、`Page.IsValid`プロパティが返す`True`します。

DataList の編集のチュートリアルのすべて ve これまでに調べる必要がありました編集インターフェイスを非常に単純なテキスト ボックス、s の製品名と価格の。 ただし、編集のインターフェイスは、Dropdownlist、カレンダー、ボタン、チェック ボックスなどの別の Web コントロールの組み合わせを含めることができます。 次のチュートリアルでは、さまざまな Web コントロールを使用するインターフェイスの構築に紹介します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Dennis Patterson、Ken Pespisa、Liz Shulok でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](handling-bll-and-dal-level-exceptions-vb.md)
> [次へ](customizing-the-datalist-s-editing-interface-vb.md)
