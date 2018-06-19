---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: (C#) を削除するときに、クライアント側の確認を追加する |Microsoft ドキュメント
author: rick-anderson
description: これまでに作成しました、インターフェイスで、ユーザーは [編集] ボタンをクリックするさせようとしているときに、[削除] ボタンをクリックしてデータを誤って削除できます。 この t しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 72b15d498e45cc519a14ecfe39111b224db88c30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880272"
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>(C#) を削除するときに、クライアント側の確認を追加します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe)または[PDF のダウンロード](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> これまでに作成しました、インターフェイスで、ユーザーは [編集] ボタンをクリックするさせようとしているときに、[削除] ボタンをクリックしてデータを誤って削除できます。 このチュートリアルでは、削除 ボタンがクリックされたときに表示されるクライアント側の確認 ダイアログ ボックスを追加します。


## <a name="introduction"></a>はじめに

過去のいくつかのチュートリアルをお ve 連携を挿入する、編集、および削除機能を提供する、アプリケーションのアーキテクチャ、ObjectDataSource、および Web コントロールのデータを使用する方法について説明します。 調べる ve おインターフェイスを削除する削除で構成されてきたされているボタンで、クリックして、ポストバックを発生させるし、ObjectDataSource s を呼び出します`Delete()`メソッドです。 `Delete()`メソッド、メソッドを呼び出して、構成されている実際の発行、データ アクセス層に呼び出しを反映するビジネス ロジック層から`DELETE`ステートメントはデータベースにします。

このユーザー インターフェイスには、GridView、DetailsView、またはフォーム ビュー コントロールでレコードを削除する訪問者ができるように、中に、ユーザーが [削除] ボタンをクリックしたときに何らかの確認が不足しています。 ユーザーが誤ってクリックした場合は、[削除] ボタンをクリックして、意図したものとの編集、レコードを更新するためのものが代わりに削除されます。 このチュートリアルでは、これを防ぐために、削除 ボタンがクリックされたときに表示されるクライアント側の確認 ダイアログ ボックスを追加します。

JavaScript`confirm(string)`関数は、ok に装備されて、2 つのボタンと (図 1 を参照してください) をキャンセルするモーダル ダイアログ ボックス内のテキストとして、文字列の入力パラメーターを表示します。 `confirm(string)`関数によってどのようなボタンがクリックされたブール値を返します (`true`、ユーザーが [ok] をクリックすると、および`false`[キャンセル] をクリックした場合)。


![クライアント側のメッセージ ボックスをモーダルを表示する JavaScript confirm(string) メソッド](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**図 1**: JavaScript`confirm(string)`メソッドには、解析、クライアント側のメッセージ ボックスが表示されます


値の場合、フォームの送信中に`false`が、フォームの送信をキャンセルし、クライアント側のイベント ハンドラーから返されます。 この機能を使用する場合は、この削除ボタン s のクライアント側ができる`onclick`イベント ハンドラーがへの呼び出しの値を返す`confirm("Are you sure you want to delete this product?")`です。 ユーザーが、[キャンセル] をクリックすると`confirm(string)`false の場合、それによってをキャンセルするフォームの送信の原因が返されます。 ありませんポストバックに t が勝利した製品の削除 ボタンがクリックしてされたを削除できません。 ただし、ユーザーは確認のダイアログ ボックスで [ok] をクリックすると、ポストバックがし続けるし、製品は削除されます。 参照してください[を使用して JavaScript s`confirm()`フォームの送信を制御する方法](http://www.webreference.com/programming/javascript/confirm/)詳細については、この手法です。

必要なクライアント側スクリプトを追加することは若干異なります、CommandField を使用するときのテンプレートを使用する場合。 そのため、このチュートリアルでを見てみましょう FormView と GridView の両方の例です。

> [!NOTE]
> クライアント側の確認方法を使用して、このチュートリアルで説明したものと同様に、ユーザーが、JavaScript をサポートするブラウザーにアクセスして見なし JavaScript が有効であること。 特定のユーザーの場合は true。 これらの前提条件のいずれかの場合は、[削除] ボタンをクリックするとはすぐにポストバック (確認メッセージ ボックスを表示できません)。


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>手順 1: は、削除をサポートする、フォーム ビューの作成

フォーム ビューを追加して、開始、 `ConfirmationOnDelete.aspx`  ページで、`EditInsertDelete`フォルダーで、バックアップを使用して製品情報をプルする新しい ObjectDataSource にバインドすること、`ProductsBLL`クラスの`GetProducts()`メソッドです。 ObjectDataSource を構成できるように、`ProductsBLL`クラス s`DeleteProduct(productID)`メソッドが ObjectDataSource s にマップされて`Delete()`メソッド;、INSERT および UPDATE タブ (なし) をドロップダウン リストが設定されていることを確認します。 最後に、FormView s のスマート タグのページングを有効にするチェック ボックスを確認します。

これらの手順の後に新しい ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

オプティミスティック同時実行制御を使用していない、過去例のように、ObjectDataSource s をクリアする少し時間を取って`OldValuesParameterFormatString`プロパティです。

FormView s の削除のみをサポートする ObjectDataSource コントロールにバインドされているため`ItemTemplate`のみの削除 ボタン、新機能と更新プログラムのボタンが欠落しているを提供します。 FormView s の宣言型マークアップ、ただしに含まれる、余分な`EditItemTemplate`と`InsertItemTemplate`、これを削除することができます。 カスタマイズをとって、`ItemTemplate`されるため、製品のサブセットだけにデータ フィールドを表示します。 私の製品の名前を表示するように構成 ve、 `<h3>` (削除) と共にその業者とカテゴリ名の上の見出しです。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

これらの変更は、完全に機能の web ページにより、ユーザーは、[削除] ボタンをクリックするだけで、製品を削除する機能を一度に 1 つの製品間を切り替えることがあります。 図 2 は、ブラウザーで表示したときにこれまで、進行状況のスクリーン ショットを示します。


[![FormView には、1 つの製品に関する情報が表示されます。](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**図 2**:、FormView 表示情報について、1 つの製品 ([フルサイズのイメージを表示するをクリックして](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>手順 2: は、削除ボタン クライアント側の onclick イベントから confirm(string) 関数を呼び出す

作成 FormView、最後の手順はこのような削除ボタンを構成するときに、JavaScript、訪問者によって秒がクリックされた`confirm(string)`関数が呼び出されます。 ボタン、LinkButton、または ImageButton のクライアント側へのクライアント側スクリプトの追加`onclick`イベントの使用により実現できます、 `OnClientClick property`、ASP.NET 2.0 に新機能です。 値が必要なので、`confirm(string)`このプロパティを設定するだけ関数が返されます。 `return confirm('Are you certain that you want to delete this product?');`

この変更後に削除の LinkButton s の宣言構文ようになります。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

すべてが s です! 図 3 は、アクションでこの確認のスクリーン ショットを示します。 [削除] ボタンをクリックすると、[確認] ダイアログ ボックスが表示されます。 ユーザーは、[キャンセル] をクリックすると、ポストバックは取り消され、製品は削除されません。 ポストバックの継続のかどうか、ただし、[ok] をクリックし、ObjectDataSource の`Delete()`メソッドが呼び出され、削除されているデータベースのレコードにすると、します。

> [!NOTE]
> 渡された文字列、 `confirm(string)` JavaScript 関数は、アポストロフィ (引用符ではなく) で区切られます。 JavaScript はいずれかの文字を使用して文字列を区切ることができます。 アポストロフィここで使用できるようにに渡された文字列の区切り記号`confirm(string)`を使用する区切り記号であいまいさを持ち込んでいない、`OnClientClick`プロパティの値。


[![確認メッセージは、今すぐ表示と削除 をクリックして](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**図 3**: A の確認は今すぐ表示と削除 をクリックして ([フルサイズのイメージを表示するをクリックして](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>手順 3: CommandField の削除 ボタンの OnClientClick プロパティを構成します。

確認のダイアログ ボックスを構成するだけでそれに関連付けられたすることができますを使用する場合、ボタン、LinkButton、または ImageButton テンプレート内で直接、その`OnClientClick`、JavaScript の結果を返すプロパティ`confirm(string)`関数。 ただし、- 削除 ボタンのフィールドを追加すると、GridView、DetailsView - が CommandField は必要ありません、`OnClientClick`宣言によって設定できるプロパティです。 代わりに、適切な GridView または DetailsView s [削除] ボタンを参照する必要がありますプログラムで`DataBound`イベント ハンドラー、および設定の`OnClientClick`プロパティがあります。

> [!NOTE]
> S [削除] ボタンを設定するときに`OnClientClick`プロパティに適切な`DataBound`へのアクセスがある、イベント ハンドラー、データは、現在のレコードにバインドされました。 つまり、「は Chai 製品を削除することを確認するか」など、特定のレコードに関する詳細を含む、確認メッセージを拡張できます。 このようなカスタマイズは、データ バインドの構文を使用してテンプレートのこともできます。


実際の設定を`OnClientClick`CommandField、使用すると、s の削除ボタンのプロパティ、GridView、ページを追加します。 FormView を使用する同じ ObjectDataSource コントロールを使用するには、この GridView を構成します。 また GridView s BoundFields だけ製品の名前、カテゴリ、および業者を含めることを制限します。 最後に、GridView s のスマート タグからの削除を有効にするチェック ボックスを確認します。 GridView s に、CommandField を追加します`Columns`コレクションでその`ShowDeleteButton`プロパティに設定`true`です。

これらの変更を加えたら、GridView s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

CommandField には GridView s からプログラムでアクセスできる 1 つの削除の LinkButton インスタンスが含まれています`RowDataBound`イベント ハンドラー。 設定できる、参照とその`OnClientClick`プロパティに応じて。 イベント ハンドラーを作成、`RowDataBound`次のコードを使用して、イベント。


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

このイベント ハンドラーは、データ行 ([削除] ボタンを持つもの) と連携し、[削除] ボタンをプログラムで参照して開始します。 [全般] で、次のパターンを使用します。


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType*が CommandField - ボタン、LinkButton、または ImageButton によって使用されているボタンの種類。 既定では、CommandField があるを使用しますが、CommandField s を使用してこれをカスタマイズする`ButtonType property`です。 *CommandFieldIndex*が CommandField GridView s 内の序数インデックス`Columns`コレクション、一方、 *controlIndex*が CommandField s 内で [削除] ボタンのインデックス`Controls`コレクション。 *ControlIndex*値は、CommandField で他のボタンを基準としたボタン s の位置に依存します。 たとえば、CommandField に表示される唯一のボタンが [削除] ボタンの場合は、インデックスは 0 を使用します。 かどうか、ただしが s [削除] の前にある [編集] ボタンを使用して、インデックスの 2 です。 2 のインデックスを使用している理由は 2 つのコントロールが CommandField 削除ボタンをクリックする前にして追加されるためです。 [編集] ボタンと LiteralControl s 編集および削除のボタン間にスペースを追加するために使用します。

この特定の例では、CommandField はあると、左端のフィールドが含まれて、 *commandFieldIndex*は 0 です。 使用していないその他のボタンが CommandField、[削除] ボタンがあるため、 *controlIndex*は 0 です。

CommandField [削除] ボタンを参照した後はお次 GridView の現在の行にバインドされている製品に関する情報を取得します。 最後に、[削除] ボタン s を設定お`OnClientClick`プロパティを適切な JavaScript、s の製品名が含まれています。 JavaScript 文字列が渡されるので、`confirm(string)`関数は、アポストロフィ、s の製品名内に表示される任意のアポストロフィはエスケープする必要がありますを使用して区切られます。 S の製品名にアポストロフィをエスケープする具体的には、"`\'`"です。

これらの変更完了、GridView が表示されます (図 4 を参照してください)、カスタマイズされた確認ダイアログ ボックスで [削除] ボタンをクリックします。 として、FormView から確認メッセージ ボックスと、ユーザーは、[キャンセル] をクリックするとポストバックが取り消されると、削除の発生を回避します。

> [!NOTE]
> この方法は、プログラムにアクセスする、[削除] ボタン、DetailsView で CommandField も使用できます。 DetailsView、ただし、d を作成、イベント ハンドラーを`DataBound`DetailsView がないため、イベント、`RowDataBound`イベント。


[![GridView s の削除 ボタンをクリックすると、カスタマイズされた確認ダイアログ ボックスが表示されます。](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**図 4**: GridView s [削除] ボタンをクリックすると、カスタマイズされた確認ダイアログ ボックスが表示されます ([フルサイズのイメージを表示するをクリックして](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>TemplateFields を使用します。

CommandField の欠点の 1 つは、そのボタンをインデックスを通じてアクセスする必要があり、結果のオブジェクトを適切なボタンの種類 (ボタン、LinkButton、または ImageButton) にキャストされる必要があります。 実行時まで検出できない問題への招待の「マジック ナンバー」およびハード コーディングされた型を使用します。 たとえば、自分や他の開発者は、いくつかの時点を後で ([編集] ボタン) などの変更で CommandField に新しいボタンを追加する場合、`ButtonType`プロパティ、既存のコードはコンパイル エラー、まだがページにアクセスと、例外が発生する可能性がありますまたは、コードの記述方法およびどのような変更が加えられたによって、予期しない動作します。

別の方法が TemplateFields GridView と DetailsView のコマンドに変換します。 これで TemplateField が生成されます、 `ItemTemplate` CommandField 内の各ボタンの LinkButton (またはボタンまたは ImageButton) を持ちます。 これらのボタン`OnClientClick`、FormView に成功しましたした場合、または適切なプログラムでアクセスできるのプロパティを宣言によって、割り当てることができます`DataBound`次のパターンを使用してイベント ハンドラー。


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

ここで*controlID*ボタン秒の値は、`ID`プロパティです。 このパターンには、キャストのハードコーディング タイプも必要ですが、必要がある、インデックス作成のためのレイアウトは実行時エラーの結果として得られるせずに変更することができます。

## <a name="summary"></a>まとめ

JavaScript`confirm(string)`関数は、フォームの送信を制御するための一般的に使用される手法です。 実行すると、関数には、2 つのボタン、[ok] と [キャンセル] を含む、クライアント側のモーダル ダイアログ ボックスが表示されます。 ユーザーが [ok] をクリックすると、`confirm(string)`関数が返される`true`; [キャンセル] をクリックすると戻ります`false`です。 この機能は、送信処理中に、イベント ハンドラーを返す場合は、フォームの送信をキャンセルするブラウザー s の動作と組み合わせる`false`レコードを削除するときに、確認メッセージ ボックスを表示するために使用できます。

`confirm(string)`関数は、ボタンの Web コントロールのクライアント側を関連付けることができる`onclick`イベント ハンドラーからコントロールの`OnClientClick`プロパティです。 テンプレート - FormView のテンプレートのいずれかまたは DetailsView または GridView - TemplateField で [削除] ボタンを使用するときにこのプロパティ宣言またはプログラムによって、このチュートリアルで示したようにします。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](implementing-optimistic-concurrency-cs.md)
> [次へ](limiting-data-modification-functionality-based-on-the-user-cs.md)
