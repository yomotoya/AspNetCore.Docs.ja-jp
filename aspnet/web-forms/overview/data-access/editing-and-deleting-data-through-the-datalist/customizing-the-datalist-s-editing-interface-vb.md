---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: DataList のカスタマイズの編集インターフェイス (VB) |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、DataList DropDownLists とチェック ボックスを含む 1 つの豊富な編集インターフェイスを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fbdb4687a23604b9f505bbf782d59a8c78e5a02
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30884871"
---
<a name="customizing-the-datalists-editing-interface-vb"></a>DataList の編集インターフェイス (VB) のカスタマイズ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe)または[PDF のダウンロード](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> このチュートリアルでは、DataList DropDownLists とチェック ボックスを含む 1 つの豊富な編集インターフェイスを作成します。


## <a name="introduction"></a>はじめに

DataList の Web コントロールのマークアップと`EditItemTemplate`編集可能なインターフェイスを定義します。 内のすべての編集可能な DataList 例お ve 検証、編集可能なところ、TextBox Web コントロールのインターフェイスが作成されました。 [前のチュートリアル](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)検証コントロールを追加することで、編集時ユーザー エクスペリエンスの改善を図っています。

`EditItemTemplate`され DropDownLists、RadioButtonLists、カレンダーなど、テキスト ボックス以外の Web コントロールを含めるために拡張することができます。 テキスト ボックス、その他の Web コントロールを追加する編集のインターフェイスをカスタマイズする際と同様、次の手順を採用します。

1. Web コントロールを追加、`EditItemTemplate`です。
2. データ バインドの構文を使用して、適切なプロパティに対応するデータ フィールドの値を割り当てます。
3. `UpdateCommand` 、プログラムでアクセス、Web のイベント ハンドラーが値を制御し、それを適切な BLL メソッドに渡します。

このチュートリアルでは、DataList DropDownLists とチェック ボックスを含む 1 つの豊富な編集インターフェイスを作成します。 具体的には、製品情報を一覧表示を更新する製品の名前、供給業者、カテゴリ、および提供が中止された状態で許可される DataList を作成します (図 1 を参照してください)。


[![編集のインターフェイスには、テキスト ボックス、2 つの DropDownLists およびチェック ボックスが含まれています。](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**図 1**: 編集インターフェイスには、テキスト ボックス、2 つの DropDownLists およびチェック ボックスが含まれています ([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>手順 1: 製品情報を表示します。

DataList s の編集可能なインターフェイスを作成するには、読み取り専用のインターフェイスを作成して必要があります。 開いて開始、`CustomizedUI.aspx`ページから、`EditDeleteDataList`フォルダーと、デザイナー、DataList ページを追加、設定、`ID`プロパティを`Products`です。 DataList s のスマート タグから新しい ObjectDataSource を作成します。 名前をこの新しい ObjectDataSource`ProductsDataSource`からデータを取得するように構成し、`ProductsBLL`クラスの`GetProducts`メソッドです。 として前の編集可能な DataList チュートリアルでおを編集した製品情報の更新 s ビジネス ロジック層に直接移動するとします。 同様に、更新、挿入でドロップダウン リストを設定し、(なし) タブを削除します。


[![(なし) を更新、挿入、および DELETE のタブのドロップダウン リストを設定します。](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**図 2**: (None) に、UPDATE、INSERT、およびタブ ドロップダウン リストの削除を設定 ([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Visual Studio が、既定値を作成、ObjectDataSource を構成すると、`ItemTemplate`の各データ フィールドの名前と値を一覧表示する DataList が返されます。 変更、`ItemTemplate`テンプレート内の製品名を一覧表示できるように、`<h4>`要素およびカテゴリ名、仕入先の名前、価格、および提供が中止された状態です。 さらに、追加できるように、[編集] ボタン、`CommandName`を編集するプロパティを設定します。 宣言型マークアップ my`ItemTemplate`に従います。


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

上記のマークアップの製品情報を使用して、レイアウト、 &lt;h4&gt; s の製品名、および 4 つの列の見出し`<table>`残りのフィールドです。 `ProductPropertyLabel`と`ProductPropertyValue`で定義された CSS クラス`Styles.css`、前のチュートリアルで説明しています。 図 3 は、ブラウザーで表示したときに、進行状況を示します。


[![名前、供給業者、カテゴリ、提供が中止された状態、および各製品の価格が表示されます。](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**図 3**: Name、供給業者、カテゴリ、提供が中止された状態、および各製品の価格が表示されます ([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>手順 2: 編集インターフェイスへの Web コントロールを追加します。

最初の手順で構築するために必要な Web コントロールを追加する編集用のインターフェイスは、カスタマイズされた DataList、`EditItemTemplate`です。 具体的には、別の仕入先、および提供が中止された状態のチェック ボックスは、カテゴリの DropDownList が必要です。 製品の価格がこの例では編集可能であるためお Label Web コントロールを使用して表示を続行できます。

編集のインターフェイスをカスタマイズするには、DataList s のスマート タグからのテンプレートの編集リンクをクリックし、選択、`EditItemTemplate`ドロップダウン リストからオプション。 DropDownList の追加、`EditItemTemplate`設定とその`ID`に`Categories`です。


[![カテゴリの DropDownList を追加します。](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**図 4**: DropDownList をカテゴリの追加 ([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


次に、DropDownList s のスマート タグからデータ ソースの選択オプションを選択し、という名前の新しい ObjectDataSource を作成する`CategoriesDataSource`です。 構成を使用するには、この ObjectDataSource、`CategoriesBLL`クラスの`GetCategories()`メソッド (図 5 を参照してください)。 次に、DropDownList のデータ ソース構成ウィザード要求ごとに使用するデータ フィールドの`ListItem`s`Text`と`Value`プロパティです。 DropDownList ディスプレイ、`CategoryName`データ フィールドと使用、`CategoryID`図 6 に示すように、値として。


[![CategoriesDataSource をという名前の新しい ObjectDataSource を作成します。](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**図 5**: 名前付き新しい ObjectDataSource 作成`CategoriesDataSource`([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![DropDownList の画面を構成して、フィールドの値](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**図 6**: DropDownList s の表示と値のフィールドを構成する ([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


この一連のサプライヤーの DropDownList の作成手順を繰り返します。 設定、`ID`にこの DropDownList の`Suppliers`し名前をその ObjectDataSource`SuppliersDataSource`です。

2 つの DropDownLists を追加すると、提供が中止された状態のチェック ボックスは、製品の名前をテキスト ボックスを追加します。 設定、`ID`のチェック ボックスとテキスト ボックスに s`Discontinued`と`ProductName`、それぞれします。 ユーザーが、s の製品名の値を提供することを確認する RequiredFieldValidator を追加します。

最後に更新し、[キャンセル] ボタンを追加します。 これら 2 つのボタンが命令型をその`CommandName`プロパティを更新し、取り消すには、それぞれに設定されます。

ただしいただいて編集インターフェイス レイアウトに自由にできます。 同じ 4 つの列を使用することを選択した`<table>`として、次の宣言の構文とスクリーン ショット、読み取り専用のインターフェイスからレイアウトを示しています。


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![読み取り専用インターフェイスと同様にレイアウトを編集インターフェイスは、します。](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**図 7**:、編集のインターフェイスは、読み取り専用インターフェイスと同様に配置されるアウト ([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>手順 3: EditCommand および CancelCommand イベント ハンドラーの作成

現時点では、データ バインド構文ではありません、 `EditItemTemplate` (を除き、 `UnitPriceLabel`、これからコピーした経由で逐語的、 `ItemTemplate`)。 すぐに、データ バインドの構文を追加しますが、まずの DataList s のイベント ハンドラーを作成する`EditCommand`と`CancelCommand`イベント。 注意してくださいの責任において、`EditCommand`一方ある編集ボタンがクリックされた、DataList 項目の編集のインターフェイスを表示するためには、イベント ハンドラー、 `CancelCommand` DataList を編集済みの状態に戻すには、s ジョブです。

これらの 2 つのイベント ハンドラーを作成し、次のコードを使用してください。


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

これらの 2 つのイベント ハンドラーの場所 [編集] ボタンをクリックすると、編集用のインターフェイスが表示され、読み取り専用モードに編集された項目を返します [キャンセル] ボタンをクリックするとします。 図 8 は、Chef Anton の Gumbo ミックスの編集 ボタンがクリックしてされた後に、DataList を示します。 以降お編集のインターフェイスを任意のデータ バインド構文を追加するには、まだ ve、 `ProductName`  テキスト ボックスが空白で、`Discontinued`から、チェック ボックスをオンになっていないと、最初の項目が選択されている、`Categories`と`Suppliers`DropDownLists です。


[![編集ボタンの表示、編集用のインターフェイスをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**図 8**: 編集のインターフェイスを表示して [編集] ボタンをクリックすると ([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>手順 4: 編集インターフェイスへのデータ バインド構文の追加

現在の製品の値を表示、編集用のインターフェイスには、構文を使用するデータ バインドを適切な Web コントロールの値にデータ フィールドの値を割り当てる必要があります。 データ バインドの構文は、テンプレートの編集画面に移動して、デザイナーで適用でき、スマート タグを制御、Web から databindings の編集 リンクをクリックします。 代わりに、データ バインドの構文は、宣言型マークアップを直接追加できます。

割り当てる、`ProductName`データ フィールドの値を`ProductName`TextBox s`Text`プロパティ、`CategoryID`と`SupplierID`データ フィールドの値を`Categories`と`Suppliers`DropDownLists`SelectedValue`プロパティ、および`Discontinued`データ フィールドの値を`Discontinued`チェックの`Checked`プロパティです。 デザイナーを使用するか、宣言型のマークアップを使用して直接、これらの変更を行った後は、ブラウザーでページを再確認し、Chef Anton の Gumbo ミックスの編集 をクリックします。 図 9 に示す、データ バインド構文が追加の現在の値 ボックスに、DropDownLists、およびチェック ボックスをオンにします。


[![編集ボタンの表示、編集用のインターフェイスをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**図 9**: 編集のインターフェイスを表示して [編集] ボタンをクリックすると ([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>手順 5: UpdateCommand イベント ハンドラーでのユーザーの変更の保存

ユーザーが製品を編集し、ポストバックが発生した更新 ボタンをクリックする」および「DataList s`UpdateCommand`イベントが発生します。 イベント ハンドラー、いただくために Web コントロールから値を読み取り、`EditItemTemplate`と、データベース内の製品を更新する BLL を持つインターフェイスです。 おとして前のチュートリアルで説明しました、`ProductID`の製品の更新に使用してアクセス、`DataKeys`コレクション。 ユーザーが入力したフィールドは、プログラムでの Web コントロールを使用して参照することによってアクセスが`FindControl("controlID")`次のコードに示すようにします。


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

コードは、まずコンサルティング、`Page.IsValid`プロパティをページ上のすべての検証コントロールが有効であることを確認します。 場合`Page.IsValid`は`True`、編集した製品 s`ProductID`から値を読み取る、`DataKeys`と Web コントロールのデータ エントリのコレクション、`EditItemTemplate`はプログラムで参照されています。 次に、これらの Web コントロールからの値は、適切なに渡される変数に読み取られる`UpdateProduct`オーバー ロードします。 データを更新した後、編集済みの状態、DataList が返されます。

> [!NOTE]
> I ve 例外に追加したロジックの処理を省略すると、[処理 BLL および DAL レベルの例外](handling-bll-and-dal-level-exceptions-vb.md)コードと次の例を保持するためにチュートリアルの重点を置きます。 演習として、このチュートリアルを完了した後はこの機能を追加します。


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>手順 6: NULL CategoryID および SupplierID 値の処理

Northwind データベースで許容`NULL`の値を`Products`表 s`CategoryID`と`SupplierID`列です。 ただし、編集、インターフェイスのされない現在には適応`NULL`値。 持つ製品を編集しようとしている場合、`NULL`いずれかの値、`CategoryID`または`SupplierID`列が表示されます、`ArgumentOutOfRangeException`ようなエラー メッセージで: *'カテゴリ' が無効なため SelectedValue項目の一覧にありません。* またが s 現在 s の製品カテゴリまたは仕入先を変更する方法の値なし以外`NULL`値を`NULL`いずれか。

サポートする`NULL`カテゴリおよび業者 DropDownLists の値を追加する必要があります`ListItem`です。 I () として使用することを選択したら、`Text`値`ListItem`、d 必要な場合は (空の文字列) など他のものに変更することができますが、します。 最後に、必ず、DropDownLists`AppendDataBoundItems`に`True`ためには、カテゴリを忘れた場合に、静的に追加した DropDownList にバインドされている仕入先が上書きされます。`ListItem`です。

DataList s で DropDownLists マークアップ、これらの変更を行った後`EditItemTemplate`次のようになります。


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> 静的`ListItem`s は、デザイナーを使用したり、宣言構文から直接 DropDownList に追加することができます。 データベースを表す DropDownList 項目を追加するときに`NULL`値を追加してください、`ListItem`宣言の構文を使用します。 使用する場合、`ListItem`コレクション エディター、デザイナーで、生成された宣言型の構文が省略されます、`Value`のような宣言型マークアップを作成する、空の文字列が割り当てられている場合に、設定完全:`<asp:ListItem>(None)</asp:ListItem>`です。 この影響を与えません、見えるかもしれませんが欠落している`Value`DropDownList を使用すると、`Text`代わりに、プロパティの値。 このことを意味`NULL``ListItem`は選択すると、(なし) 値が試行されます製品データ フィールドに割り当てられるに (`CategoryID`または`SupplierID`、このチュートリアルでは)、その結果、例外、します。 明示的に設定して`Value=""`、`NULL`製品に値が割り当てられる場合は、データ フィールド、 `NULL` `ListItem`が選択されています。


ブラウザーを使って作業の進行状況を表示するのにはしばらくを実行します。 製品を編集するには、注意してください、`Categories`と`Suppliers`DropDownLists 両方がある (なし)、DropDownList の開始時のオプションです。


[![カテゴリと Suppliers DropDownLists、(なし) オプションを含める](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**図 10**:`Categories`と`Suppliers`DropDownLists が、(なし) オプションを含める ([フルサイズのイメージを表示するをクリックして](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


データベースとしてオプション (なし) を保存する`NULL`値を返す必要があります、`UpdateCommand`イベント ハンドラー。 変更、`categoryIDValue`と`supplierIDValue`変数は null 許容の整数を指定し、割り当てる値以外の値を`Nothing`場合にのみ、DropDownList の`SelectedValue`は空の文字列ではありません。


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

この変更により、値の`Nothing`に渡される、 `UpdateProduct` BLL メソッド ()、ユーザーが選択しなかった場合は、どちらかに対応するドロップダウン リストのオプション、`NULL`データベースの値。

## <a name="summary"></a>まとめ

このチュートリアルより複雑なテキスト ボックス、2 つの DropDownLists と検証コントロールと共にのチェック ボックスを次の 3 つの異なる入力 Web コントロールを含める DataList 編集インターフェイスを作成する方法を説明しました。 編集のインターフェイスを構築するときに、手順は、使用されている Web コントロールに関係なく同じ: DataList s に Web コントロールを追加することによって開始`EditItemTemplate`; に適切な Web に対応するデータ フィールドの値を割り当てるデータ バインドの構文を使用します。コントロールのプロパティです。`UpdateCommand`イベント ハンドラーでは、Web コントロールおよび BLL にその値を渡すこと、適切なプロパティをプログラムでアクセスします。

かどうか、編集のインターフェイスを作成するときに、秒のテキスト ボックスまたは別の Web コントロールのコレクションだけで構成を使用してデータベースを正しく処理することを確認して`NULL`値。 会計時`NULL`が正しくないのみを表示する既存の命令型`NULL`で編集のインターフェイスもその値と同じ値をマークするための手段を提供する`NULL`です。 この後、Datalist で DropDownLists、通常つまり静的なを追加する`ListItem`が`Value`プロパティが空の文字列に明示的に設定 (`Value=""`)、コードを追加して、`UpdateCommand`イベント ハンドラーを決定するかどうか、`NULL``ListItem`が選択されています。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Dennis Patterson、David Suru ものです。 Schmidt がされていました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
