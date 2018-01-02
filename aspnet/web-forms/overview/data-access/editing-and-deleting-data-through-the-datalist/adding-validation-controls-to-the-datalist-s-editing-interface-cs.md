---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: "DataList への検証コントロールの追加の編集インターフェイス (c#) |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、DataList の後に編集 int です。 ユーザーより安全な方法で提供するために検証コントロールを追加することがいかに簡単思います."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 06f3e59d0e6fd59a83934084422816360e915bd7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>DataList の編集インターフェイス (c#) に検証コントロールを追加します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe)または[PDF のダウンロード](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> このチュートリアルより安全な方法で編集するユーザー インターフェイスを提供するために、DataList の後に検証コントロールを追加することがいかに簡単おが表示されます。


## <a name="introduction"></a>はじめに

チュートリアルのこれまで編集 DataList、不足している製品名など、例外が負の値の価格の結果、無効なユーザー入力も事前にユーザー入力の検証の場所をこの後、Datalist インターフェイスの編集が含まれませんが。 [前のチュートリアル](handling-bll-and-dal-level-exceptions-cs.md)例外処理コード DataList s を追加する方法について説明しました`UpdateCommand`をキャッチして適切に発生した例外に関する情報を表示するためにイベント ハンドラー。 理想的には、ただし、編集のインターフェイスは追加の検証コントロールをユーザーがこのような無効なデータのため、最初に入力するを防ぐためにします。

このチュートリアルでは会いしましょう DataList s に検証コントロールを追加することがいかに簡単`EditItemTemplate`より安全な方法で編集するユーザー インターフェイスを提供するためにします。 具体的には、このチュートリアルでは、前のチュートリアルで作成された例を受け取るし、適切な検証を追加、編集のインターフェイスを拡張します。

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>手順 1: から例をレプリケートする[BLL および DAL レベルの例外処理](handling-bll-and-dal-level-exceptions-cs.md)

[処理 BLL および DAL レベルの例外](handling-bll-and-dal-level-exceptions-cs.md)名前と 2 つの列に、編集できる DataList では、製品の価格を表示するページを作成したチュートリアルです。 このチュートリアルの目標は、検証コントロールを追加するには、DataList s 編集インターフェイスを強化することです。 具体的には、この検証ロジックを行います。

- 製品の名前を指定することが必要
- 価格の値が有効な通貨の形式であることを確認します。
- 価格がより大きいか等しいには、0、負の値から入力された値を確認してください`UnitPrice`値は無効です

最初の例をレプリケートする必要があります拡張検証をインクルードする前の例で見ることができます、前に、 `ErrorHandling.aspx`  ページで、`EditDeleteDataList`このチュートリアルでは、ページをフォルダー`UIValidation.aspx`です。 両方の経由でコピーする必要があります。 これを実現するために、 `ErrorHandling.aspx` s 宣言型マークアップとそのソース コード ページです。 まず、次の手順を実行することによって宣言型マークアップをコピーします。

1. 開く、 `ErrorHandling.aspx` Visual Studio でのページ
2. ページの宣言型マークアップ (ページの下部にある [ソース] ボタンをクリック) を参照してください。
3. 内のテキストをコピー、`<asp:Content>`と`</asp:Content>`図 1 としてタグ (行 3 ~ 32)。


[![内のテキストのコピー、 &lt;Asp:content&gt;コントロール](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**図 1**: 内のテキストのコピー、`<asp:Content>`コントロール ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. 開く、`UIValidation.aspx`ページ
2. S 宣言型マークアップをページに移動します。
3. 内のテキストを貼り付けます、`<asp:Content>`コントロール。

ソース コードをコピーするには、開く、`ErrorHandling.aspx.vb`ページし、同様のテキストをコピー*内*、`EditDeleteDataList_ErrorHandling`クラスです。 3 つのイベント ハンドラーをコピー (`Products_EditCommand`、 `Products_CancelCommand`、および`Products_UpdateCommand`) と共に、`DisplayExceptionDetails`メソッドが、**いない**クラス宣言をコピーまたは`using`ステートメントです。 コピーしたテキストを貼り付ける*内*、`EditDeleteDataList_UIValidation`クラス`UIValidation.aspx.vb`です。

コンテンツおよびからコード経由で移動した後`ErrorHandling.aspx`に`UIValidation.aspx`ブラウザーでページをテストします。 同じ出力を表示し、これら 2 つのページ (図 2 を参照してください) のそれぞれで同じ機能を操作する必要があります。


[![UIValidation.aspx ページ模倣 ErrorHandling.aspx の機能](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**図 2**:`UIValidation.aspx`ページと同じ動作に`ErrorHandling.aspx`([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>手順 2: DataList の後に検証コントロールを追加します。

データ エントリ フォームを構築するときに、重要なは、すべての必須フィールドに入力し、指定されたすべての入力が有効で適切にフォーマットされた値です。 ユーザーの入力が有効であることを確認するには、ASP.NET には、1 つの入力 Web コントロールの値を検証するよう設計されている 5 つの組み込みの検証コントロールが用意されています。

- [RequiredFieldValidator](https://msdn.microsoft.com/en-us/library/5hbw267h(VS.80).aspx)値が指定されていることを確認
- [CompareValidator](https://msdn.microsoft.com/en-us/library/db330ayw(VS.80).aspx)値を別の Web コントロールの値または定数の値の検証や、秒の値の形式が指定されたデータ型に対して有効であることを確認
- [RangeValidator](https://msdn.microsoft.com/en-us/library/f70d09xt.aspx)値が値の範囲内であることを確認
- [RegularExpressionValidator](https://msdn.microsoft.com/en-US/library/eahwtc9e.aspx)に対して値を検証、[正規表現](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/en-us/library/9eee01cx(VS.80).aspx) 、ユーザー定義のカスタム メソッドに対して値を検証

これら 5 つの制御の詳細を参照してください戻る、[検証コントロールを編集および挿入するインターフェイスを追加する](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)チュートリアルまたはチェック アウト、[検証コントロール セクション](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx)の[ASP.NET のクイック スタート チュートリアル](https://quickstarts.asp.net)です。

このチュートリアルでは、製品名の値が指定されていることを確認する RequiredFieldValidator と価格を入力が 0 以上の値を持つし、有効な通貨形式で表示されることを確認 CompareValidator を使用する必要になります。

> [!NOTE]
> ASP.NET を while 1.x が同じこれら 5 つの検証コントロール、ASP.NET 2.0 には多数の機能強化が追加、主要な 2 つのクライアント側スクリプトの中だけでなく Internet Explorer ブラウザーおよびサポートにページ上のパーティション検証コントロールする機能検証のグループです。 詳細については、検証コントロール 2.0 の新機能を参照してください[ASP.NET 2.0 の検証コントロールを分解](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)です。


S DataList s に必要な検証コントロールを追加することによって開始できるように`EditItemTemplate`です。 この作業を実行して、DataList s のスマート タグからテンプレートの編集リンクをクリックして、デザイナー、または宣言の構文を使用します。 S プロセスの手順に従って、[デザイン] ビューからテンプレートの編集オプションを使用することができます。 DataList s を編集する選択した後に`EditItemTemplate`後の配置テンプレートの編集インターフェイスにツールボックスからドラッグして、RequiredFieldValidator を追加、 `ProductName`  テキスト ボックス。


[![ProductName TextBox 後に、EditItemTemplate、RequiredFieldValidator を追加します。](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**図 3**: に RequiredFieldValidator を追加、 `EditItemTemplate After` 、 `ProductName` TextBox ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


すべての検証コントロールは、1 つの ASP.NET Web コントロールの入力を検証することによって動作します。 そのため、追加したばかり RequiredFieldValidator がに対して検証する必要があることを示すために必要があります、 `ProductName`  テキスト ボックスですこれは、s の検証コントロールを設定で[`ControlToValidate`プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx)を、`ID`の。適切な Web コントロール (`ProductName`、このインスタンスで)。 次に、設定、 [ `ErrorMessage`プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx)には、製品の名前を指定する必要があります、 [ `Text`プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx)に\*です。 `Text`プロパティの値、指定した場合は、検証が失敗した場合、検証コントロールによって表示されるテキストです。 `ErrorMessage`場合 ValidationSummary コントロールによって必要となるプロパティの値が使用される、`Text`プロパティの値を省略すると、`ErrorMessage`プロパティ値が無効な入力の検証コントロールによって表示されます。

RequiredFieldValidator のこれら 3 つのプロパティを設定した後、画面は図 4 のようになります。


[![RequiredFieldValidator の ControlToValidate、エラー メッセージ、およびテキストのプロパティを設定します。](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**図 4**: 集合 RequiredFieldValidator s `ControlToValidate`、 `ErrorMessage`、および`Text`プロパティ ([フルサイズのイメージを表示する をクリックします](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))。


追加された RequiredFieldValidator で、`EditItemTemplate`すべての製品の価格 ボックスに必要な検証を追加するは残りますが、します。 以降、`UnitPrice`は省略可能、RequiredFieldValidator に追加する必要があるレコードを編集するとき。 ただし、ことを確認する CompareValidator を追加する必要がありますには、 `UnitPrice`、指定した場合、通貨の形式が正しく、0 以上。

CompareValidator を追加、`EditItemTemplate`設定とその`ControlToValidate`プロパティを`UnitPrice`、その`ErrorMessage`価格にプロパティが 0 以上にする必要がありますおよび通貨記号を含めることはできませんし、その`Text`プロパティ\*. 示すために、`UnitPrice`値が 0 以上でなければ、CompareValidator s を設定する必要があります[`Operator`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx)に`GreaterThanEqual`、その[`ValueToCompare`プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx)を 0 にし、その[`Type`プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx)に`Currency`です。

DataList s、これらの 2 つの検証コントロールを追加した後`EditItemTemplate`s 宣言の構文は、次のようになります。


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

これらの変更を行った後は、ブラウザーでページを開きます。 名前を省略するか、製品の編集時に、無効な価格の値を入力しようとすると、テキスト ボックスの横にアスタリスクが表示されます。 図 5 に示す $19.95 などの通貨記号を含む価格の値が無効と見なされます。 CompareValidator s `Currency` `Type`では、桁区切り記号 (コンマ、ピリオド、カルチャの設定によってなど) と先頭プラスまたはマイナス記号が、*いない*通貨記号を許可します。 この動作は、編集用のインターフェイスが現在表示ユーザーを perplex 可能性があります、`UnitPrice`通貨形式を使用します。


[![無効な入力でテキスト ボックスの横にアスタリスクが表示されます。](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**図 5**: An アスタリスクが表示されますへの無効な入力でテキスト ボックス ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


として検証機能の中では、ユーザーが許されないレコードの編集時に、通貨記号を手動で削除します。 さらが有効な場合は、編集の入力はどちらも、更新プログラムも [キャンセル] ボタンのクリックされると、ポストバックを呼び出すときにやり取りできます。 理想的には、[キャンセル] ボタンはユーザーの入力の有効性に関係なく、編集済みの状態を DataList を返します。 また、DataList s の製品情報を更新する前に、ページ データが有効なことを確認する必要があります`UpdateCommand`としてブラウザーの JavaScript サポートがないか、保持して、ユーザーがクライアント側のロジックをバイパスできる検証コントロールのイベント ハンドラーサポートを無効になっています。

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>EditItemTemplate の UnitPrice ボックスから通貨記号を削除します。

CompareValidator s を使用するときに`Currency``Type`、検証対象の入力には、通貨記号を含めることはできません。 このようなシンボルの存在は、入力に無効なをマークする CompareValidator をによりします。 ただし、編集のインターフェイスは、通貨記号を現在を含めて、 `UnitPrice`  ボックスに、つまり、ユーザーが、変更を保存する前に通貨記号を明示的に削除する必要があります。 これを解決するには、3 つのオプションがあります。

1. 構成、`EditItemTemplate`できるように、 `UnitPrice`  ボックスに値が通貨としてフォーマットされていません。
2. CompareValidator を削除して、正しく書式設定された通貨値をチェックする RegularExpressionValidator に置き換えることによって、通貨記号を入力できます。 ここで難しい通貨値を検証する正規表現は、CompareValidator として単純ではありません、カルチャ設定を組み込むたい場合に、コードの記述を必要とするとします。
3. 検証コントロールを完全に削除し、GridView s でカスタム サーバー側の検証ロジックに依存`RowUpdating`イベント ハンドラー。

このチュートリアルではオプション 1 と s を使用できます。 現在、 `UnitPrice` 、テキスト ボックス内のデータ バインド式により通貨値として書式設定、 `EditItemTemplate`:`<%# Eval("UnitPrice", "{0:c}") %>`です。 変更、`Eval`ステートメント`Eval("UnitPrice", "{0:n2}")`、有効桁数の数値 2 桁の数字で、結果の書式を設定します。 これは、宣言の構文を直接または行えますから databindings の編集 リンクをクリックして、 `UnitPrice` DataList s で TextBox`EditItemTemplate`です。

この変更により、編集インターフェイスで書式設定された価格は桁区切り記号としてコンマ、小数点区切り文字としてピリオドが含まれていますが、通貨記号をオフのままにします。

> [!NOTE]
> 場合、編集可能なインターフェイスから、通貨の書式を削除するときにした方が便利な通貨記号をテキスト ボックスの外側のテキストとして配置します。 これは、通貨記号を提供する必要がないことをユーザーにヒントとして機能します。


## <a name="fixing-the-cancel-button"></a>[キャンセル] ボタンを修正します。

既定では、Web の検証コントロールは、クライアント側で検証を実行する JavaScript を生成します。 ボタン、LinkButton、または ImageButton がクリックされたときに、ポストバックが発生する前にクライアント側で、ページ上の検証コントロールがチェックされます。 無効なデータがある場合、ポストバックが取り消されました。 特定のボタンは、データの妥当性できない可能性があります材料です。このような場合は、データが無効なため取り消されましたポストバックが迷惑です。

[キャンセル] ボタンは、このような例を示します。 たとえば、ことユーザー、s の製品名を省略することなどの無効なデータが入力し彼女が t 保存したい製品すべてを決定し、[キャンセル] ボタンをヒットします。 現時点では、キャンセル ボタンは、製品名が不足しているがポストバックを防ぐためであることを報告する ページで、検証コントロールをトリガーします。 ユーザーにテキストを入力するには、 `ProductName`  ボックスに、編集のプロセスをキャンセルするだけです。

さいわい、ボタン、LinkButton、および ImageButton が、 [`CausesValidation`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.causesvalidation.aspx)ことを示す をクリックするかどうか、ボタンは、検証ロジックを開始する必要があります (既定値は`True`)。 設定は [キャンセル] ボタン s`CausesValidation`プロパティを`False`です。

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>有効な UpdateCommand イベント ハンドラーでは、入力のことを確認

検証コントロールによって生成されますクライアント側スクリプトにより無効な入力を入力した場合の検証コントロールをキャンセル ボタン、LinkButton、によって開始される任意のポストバック コントロールのまたは ImageButton が`CausesValidation`プロパティは、 `True` (。既定値) です。 ただし、時代遅れブラウザーまたはの JavaScript のサポートが無効にされているユーザーを訪問している場合、クライアント側検証チェックは実行されません。

ASP.NET の検証コントロールのすべてのポストバック時にすぐに検証ロジックを繰り返されを使用して、ページ入力の全体的な有効性を報告、 [ `Page.IsValid`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.page.isvalid.aspx)です。 ただし、ページ フローまたはされていない中断されたいずれかの値に基づく方法で停止`Page.IsValid`です。 開発者は、ことを確認する責任は、`Page.IsValid`プロパティの値を持つ`True`入力データを有効に与えられているコードに進む前にします。

ユーザーに JavaScript が無効になっている場合は、このページにアクセス、製品を編集、すぎますの価格の値を入力負荷の高い、更新ボタンをクリックして、クライアント側の検証がバイパスされ、ポストバックにつながります。 ポストバックで ASP.NET ページ`UpdateCommand`イベント ハンドラーを実行し、すぎるを解析しようとするときに、例外が発生するコストになる、`Decimal`です。 例外処理があるため、このような例外が適切に処理するおできない無効なデータのみを続行して、最初の場所から遅れて、`UpdateCommand`イベント ハンドラー場合`Page.IsValid`の値を持つ`True`します。

先頭に次のコードを追加、 `UpdateCommand` 、イベント ハンドラーの直前に、`Try`ブロック。


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

この追加すると、製品は、送信されたデータが有効な場合にのみ更新を試みます。 T が勝利したほとんどのユーザーが無効なデータの検証コントロールのクライアント側スクリプトのためのポストバックできるが、ブラウザーのない t サポート JavaScript または JavaScript のサポートのあるユーザーをクライアント側のチェックを回避できます無効にして、無効なデータを送信します。

> [!NOTE]
> 絶好のリーダーを思い出してを GridView にデータを更新するときになかった t が明示的にチェックする必要があります、`Page.IsValid`ページの分離コード クラスのプロパティ。 これは、GridView を参照しているため、`Page.IsValid`プロパティの値を返す場合にのみ、更新プログラムを続行のみと us`True`です。


## <a name="step-3-summarizing-data-entry-problems"></a>手順 3: 概要データ エントリの問題を作成します。

ASP.NET には、5 つの検証コントロールのほか、 [ValidationSummary コントロール](https://msdn.microsoft.com/en-US/library/f9h59855(VS.80).aspx)が表示される、`ErrorMessage`の無効なデータが検出された検証コントロール。 この概要データは、モーダル、クライアント側のメッセージ ボックス データベースを介して、web ページ上のテキストとして表示できます。 S を任意の検証に関する問題の概要を作成するクライアント側のメッセージ ボックスを含めるには、このチュートリアルを向上させることができます。

これを実現するには、ツールボックスからデザイナーに ValidationSummary コントロールをドラッグします。 ValidationSummary コントロールが t の場所本当に問題からのみ、メッセージ ボックス データベースとしての概要を表示するように構成することです。 コントロールを追加すると、次のように設定します。 その[`ShowSummary`プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx)に`False`とその[`ShowMessageBox`プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx)に`True`です。 この追加のクライアント側のメッセージ ボックスに、この検証エラーをまとめたもの (図 6 を参照してください)。


[![クライアント側のメッセージ ボックスの妥当性確認エラーをまとめたもの](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**図 6**: クライアント側のメッセージ ボックスの 検証エラーをまとめたもの ([フルサイズのイメージを表示するをクリックして](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>概要

このチュートリアルでは、事前に 更新のワークフローでの使用を試行する前に、ユーザー入力が有効であることを確認する検証コントロールを使用して、例外の可能性を減少する方法を説明しました。 ASP.NET は、特定の Web を検査するよう設計されている 5 つの検証 Web コントロールが s 入力を制御し、入力の有効性に報告を提供します。 このチュートリアルで使用してこれら 5 つのコントロールの 2 つ、RequiredFieldValidator と、CompareValidator に製品の名前が指定されていることと、価格より大きいかゼロに等しい値を持つ通貨書式であることを確認してください。

上にドラッグなどの単純な編集インターフェイス DataList s を検証コントロールを追加するは、`EditItemTemplate`からツールボックスといくつかのプロパティを設定します。 既定では、検証コントロールに自動的にスクリプトを出力クライアント側の検証。累積的な結果を格納する、ポストバック時にもサーバー側の検証を提供する、`Page.IsValid`プロパティです。 ボタン、LinkButton、または ImageButton がクリックされたときに、クライアント側の検証をバイパスする設定ボタン s`CausesValidation`プロパティを`False`です。 また、ポストバック時に送信されたデータのいずれかのタスクを実行する前にいることを確認、`Page.IsValid`プロパティから返される`True`です。

すべてのチュートリアルを編集 DataList お解析してきた ve お持ちの非常に単純な編集インターフェイス、s の製品名のテキスト ボックスと別の価格です。 ただし、編集のインターフェイスは、DropDownLists、カレンダー、ボタン、チェック ボックスなどの別の Web コントロールの組み合わせを含めることができます。 [次へ]、チュートリアルでは、さまざまな Web コントロールを使用するインターフェイスの構築に紹介します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Dennis Patterson、Ken Pespisa Liz Shulok がされていました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](handling-bll-and-dal-level-exceptions-cs.md)
[次へ](customizing-the-datalist-s-editing-interface-cs.md)
