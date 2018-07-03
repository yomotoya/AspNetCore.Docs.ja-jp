---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: DataList のカスタマイズの編集インターフェイス (c#) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、DataList、Dropdownlist とチェック ボックスを含む 1 つの豊富な編集インターフェイスを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f1af8086fd65f09df3bdb8b5547f7aa779c1f984
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388673"
---
<a name="customizing-the-datalists-editing-interface-c"></a>DataList の編集インターフェイス (c#) のカスタマイズ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe)または[PDF のダウンロード](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> このチュートリアルでは、DataList、Dropdownlist とチェック ボックスを含む 1 つの豊富な編集インターフェイスを作成します。


## <a name="introduction"></a>はじめに

マークアップと s DataList Web コントロール`EditItemTemplate`編集可能なインターフェイスを定義します。 DataList の編集可能な例は、のすべてで ve 検査は編集可能なところ、TextBox Web コントロールのインターフェイスが作成されました。 [前のチュートリアル](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)検証コントロールを追加することで、編集時のユーザー エクスペリエンスを改善しました。

`EditItemTemplate`し、Dropdownlist、RadioButtonLists、予定表など、テキスト ボックス以外の Web コントロールを含めるために拡張することができます。 ボックスに、その他の Web コントロールを含めるに編集インターフェイスをカスタマイズする際と同様、次の手順を使用します。

1. Web コントロールを追加、`EditItemTemplate`します。
2. データ バインディング構文を使用して、適切なプロパティに対応するデータ フィールドの値を割り当てます。
3. `UpdateCommand`イベント ハンドラーをプログラムでアクセス Web コントロールの値と、適切な BLL メソッドに渡します。

このチュートリアルでは、DataList、Dropdownlist とチェック ボックスを含む 1 つの豊富な編集インターフェイスを作成します。 具体的には、製品情報を一覧表示を更新する製品の名前、仕入先、カテゴリ、および提供が中止された状態は、DataList を作成します (図 1 参照)。


[![編集インターフェイスには、テキスト ボックス、2 つの Dropdownlist およびチェック ボックスが含まれています。](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**図 1**: 編集インターフェイスには、テキスト ボックス、2 つの Dropdownlist およびチェック ボックスが含まれています ([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))。


## <a name="step-1-displaying-product-information"></a>手順 1: 製品情報を表示します。

DataList の編集可能なインターフェイスを作成する前にまず読み取り専用のインターフェイスを構築する必要があります。 開いて開始、`CustomizedUI.aspx`ページから、`EditDeleteDataList`フォルダーと、デザイナーでは、DataList に ページで、追加の設定、`ID`プロパティを`Products`します。 DataList s のスマート タグから新しい ObjectDataSource を作成します。 名前をこの新しい ObjectDataSource`ProductsDataSource`からデータを取得するように構成し、`ProductsBLL`クラスの`GetProducts`メソッド。 としてでは、前の編集可能な DataList チュートリアル更新編集した製品情報をビジネス ロジック層に直接移動しています。 それに応じて、UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。


[![(なし) を更新、挿入、および DELETE のタブのドロップダウン リストを設定します。](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**図 2**: (None) に、UPDATE、INSERT、およびタブ ドロップダウン リストの削除を設定 ([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))。


ObjectDataSource を構成すると、Visual Studio の既定値は作成`ItemTemplate`DataList の各データ フィールドの名前と値の一覧が返されます。 変更、`ItemTemplate`テンプレート内の製品名を一覧表示できるように、`<h4>`カテゴリ名、supplier name、price、および提供が中止された状態と共に要素。 さらに、ことを確認して、編集ボタンを追加、`CommandName`プロパティを編集 を設定します。 宣言型マークアップをマイ`ItemTemplate`に従います。


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

上記のマークアップを使用して、製品情報レイアウト、 &lt;h4&gt; s の製品名、および 4 つの列の見出し`<table>`残りのフィールド。 `ProductPropertyLabel`と`ProductPropertyValue`で定義された CSS クラス`Styles.css`、前のチュートリアルで説明しています。 図 3 は、ブラウザーで表示したときに進行状況を示します。


[![名前、仕入先、カテゴリ、提供が中止された状態、および各製品の価格が表示されます。](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**図 3**: Name、仕入先、カテゴリ、提供が中止された状態、および各製品の価格が表示されます ([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))。


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>手順 2: Web コントロールを編集インターフェイスに追加します。

最初の手順でビルドに必要な Web コントロールを追加する編集インターフェイスは、カスタマイズされた DataList、`EditItemTemplate`します。 具体的には、カテゴリ、提供が中止された状態のチェック ボックスとサプライヤー用に別の DropDownList に必要です。 製品の価格は、この例では編集可能なは、Label Web コントロールを使用して表示する続行できます。

編集インターフェイスをカスタマイズするには、DataList s のスマート タグからのテンプレートの編集リンクをクリックし、選択、`EditItemTemplate`ドロップダウン リストからオプション。 DropDownList を追加、`EditItemTemplate`設定とその`ID`に`Categories`します。


[![カテゴリの DropDownList を追加します。](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**図 4**: DropDownList をカテゴリの追加 ([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))。


次に、DropDownList s のスマート タグからデータ ソースの選択オプションを選択し、という名前の新しい ObjectDataSource を作成する`CategoriesDataSource`します。 構成を使用するには、この ObjectDataSource、`CategoriesBLL`クラスの`GetCategories()`メソッド (図 5 を参照してください)。 次に、DropDownList のデータ ソース構成ウィザード要求ごとに使用するデータ フィールドの`ListItem`s`Text`と`Value`プロパティ。 DropDownList の表示、`CategoryName`データ フィールドと使用、`CategoryID`図 6 に示すように、値として。


[![CategoriesDataSource という名前の新しい ObjectDataSource を作成します。](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**図 5**: 名前付き新しい ObjectDataSource 作成`CategoriesDataSource`([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))。


[![DropDownList の画面を構成して、フィールドの値](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**図 6**: DropDownList s の表示と値のフィールドの構成 ([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))。


この一連のサプライヤーの DropDownList の作成手順を繰り返します。 設定、`ID`にこの DropDownList の`Suppliers`し名前をその ObjectDataSource`SuppliersDataSource`します。

2 つの Dropdownlist を追加した後、提供が中止された状態のチェック ボックスと、製品名のテキスト ボックスを追加します。 設定、`ID`のチェック ボックスとテキスト ボックス s`Discontinued`と`ProductName`、それぞれします。 ユーザーが、s の製品名の値を提供することを確認する RequiredFieldValidator を追加します。

最後に、更新のキャンセル ボタンを追加します。 これら 2 つのボタンは、命令型が、ことを`CommandName`プロパティを更新し、キャンセル、それぞれに設定されます。

自由に自由編集インターフェイスをレイアウトできます。 同じ 4 つの列を使用することを選択した`<table>`宣言型構文を次のスクリーン ショットと、読み取り専用のインターフェイスからレイアウトを示しています。


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]


[![読み取り専用インターフェイスと同様のレイアウトを編集インターフェイスは、します。](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**図 7**: 読み取り専用インターフェイスと同様のレイアウトを編集インターフェイスは、([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))。


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>手順 3: EditCommand および CancelCommand イベント ハンドラーの作成

現時点では、データ バインドの構文ではありません、 `EditItemTemplate` (を除き、 `UnitPriceLabel`、これからコピーされた経由でそのまま、 `ItemTemplate`)。 すぐに、データ バインディング構文を追加しますが、まずの DataList s のイベント ハンドラーを作成する`EditCommand`と`CancelCommand`イベント。 役割、`EditCommand`イベント ハンドラーは、一方の編集 ボタンがクリックされた DataList 項目の編集インターフェイスを表示するためには、`CancelCommand`のジョブは、DataList を編集済みの状態に戻すには。

これらの 2 つのイベント ハンドラーを作成し、次のコードを使用してください。


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

これらは、編集インターフェイスを表示する 2 つのイベント ハンドラーの場所でが編集ボタンをクリックし、その読み取り専用モードに編集済みの項目を返します [キャンセル] ボタンをクリックします。 図 8 は、Chef Anton の Gumbo ミックスの編集 ボタンがクリックしてされた後に、DataList を示します。 以降編集のインターフェイスを任意のデータ バインディング構文を追加するには、まだ ve、`ProductName`テキスト ボックスは空白になります、`Discontinued`から、チェック ボックスはオフ、および最初の項目が選択されている、`Categories`と`Suppliers`Dropdownlist します。


[![編集ボタンの表示、編集用のインターフェイスをクリックします。](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**図 8**: [編集] ボタンをクリックすると編集インターフェイスを表示します ([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))。


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>手順 4: データ バインディング構文を編集インターフェイスに追加します。

現在の製品の値を表示、編集インターフェイスには、データ バインディング構文を使用して Web コントロールの適切な値にデータ フィールドの値を割り当てる必要があります。 データ バインド構文は、テンプレートの編集画面に移動して、デザイナーで適用でき、スマート タグを制御する、Web から DataBindings の編集 リンクを選択します。 または、データ バインド構文は、宣言型マークアップを直接追加できます。

割り当てる、`ProductName`データ フィールドの値を`ProductName`テキスト ボックス s`Text`プロパティ、`CategoryID`と`SupplierID`データ フィールドの値を`Categories`と`Suppliers`Dropdownlist`SelectedValue`プロパティ、および`Discontinued`データ フィールドの値を`Discontinued`チェック ボックスの`Checked`プロパティ。 デザイナーを使用したり、宣言型のマークアップから直接これらの変更を行った後、ブラウザーを使用してページを再確認し、Chef Anton の Gumbo ミックスの編集 をクリックします。 図 9 に示すようデータ バインド構文がテキスト ボックスに、Dropdownlist、およびチェック ボックスをオンに現在の値を追加します。


[![編集ボタンの表示、編集用のインターフェイスをクリックします。](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**図 9**: [編集] ボタンをクリックすると編集インターフェイスを表示します ([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))。


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>手順 5: UpdateCommand イベント ハンドラーでユーザーの変更を保存します。

ユーザーが製品を編集し、ポストバックが発生した更新 ボタンをクリックし、DataList の`UpdateCommand`イベントが発生します。 イベント ハンドラー、する必要が Web コントロールの値を読み取りある、`EditItemTemplate`と、データベース内の製品を更新するは BLL とインターフェイス。 として前のチュートリアルで確認したところ、`ProductID`製品の更新の使用してアクセスできますが、`DataKeys`コレクション。 ユーザーが入力したフィールドを使用して Web コントロールをプログラムで参照することによってアクセス`FindControl("controlID")`次のコードに示すように。


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

参照して、コードが始まり、`Page.IsValid`プロパティをページ上のすべての検証コントロールが有効であることを確認します。 場合`Page.IsValid`は`True`、編集した製品 s`ProductID`から値を読み取る、`DataKeys`コレクションとでコントロールを Web データ エントリ、`EditItemTemplate`プログラムで参照されます。 次に、これらの Web コントロールから値が、適切なに渡される変数に読み取られます`UpdateProduct`オーバー ロードします。 データを更新した後、DataList には編集済みの状態が返されます。

> [!NOTE]
> 省略して、例外処理ロジックの追加、[処理 BLL - と DAL レベルの例外](handling-bll-and-dal-level-exceptions-cs.md)チュートリアル、コードと次の例を維持するために重点を置いています。 演習として、このチュートリアルを完了した後はこの機能を追加します。


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>手順 6: NULL CategoryID および SupplierID 値の処理

Northwind データベースで`NULL`の値を`Products`テーブル s`CategoryID`と`SupplierID`列。 現在、編集インターフェイスは t に合わせて、`NULL`値。 持つ製品を編集する場合、`NULL`いずれかの値、`CategoryID`または`SupplierID`列、もう少し、`ArgumentOutOfRangeException`のようなエラー メッセージで: *'Categories' が無効なため SelectedValue項目の一覧に存在します。* またが s 現在 s の製品のカテゴリまたは仕入先を変更する方法の値なし以外`NULL`値を`NULL`いずれか。

サポートするために`NULL`、category と supplier Dropdownlist の値を追加する必要があります`ListItem`します。 () として使用するように選択した、`Text`値`ListItem`が d にする場合は (空の文字列) などの別のものに変更することができます。 最後に、Dropdownlist を設定してください`AppendDataBoundItems`に`True`処理するためには、カテゴリを忘れた場合に、静的に追加した仕入先、DropDownList にバインドが上書きされます。`ListItem`します。

DataList s で Dropdownlist マークアップのこれらの変更を行った後`EditItemTemplate`次のようになります。


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> 静的`ListItem`s をデザイナー、または宣言型構文を通じて直接 DropDownList に追加できます。 データベースを表す DropDownList の項目を追加するときに`NULL`値には、追加してください、`ListItem`宣言型構文を使用します。 使用する場合、`ListItem`コレクション エディター、デザイナーで、生成された宣言型構文を省略は、`Value`のような宣言型マークアップを作成する、空の文字列が割り当てられている場合に、設定全体:`<asp:ListItem>(None)</asp:ListItem>`します。 この、無害に見えるかもしれませんが、不足している`Value`DropDownList を使用すると、`Text`代わりにプロパティの値。 このことを意味`NULL``ListItem`は選択すると、(なし) 値が、product データ フィールドに割り当てられる試行されます (`CategoryID`または`SupplierID`、このチュートリアルでは)、その結果、例外。 明示的に設定して`Value=""`、`NULL`製品に値が割り当てられる場合は、データ フィールド、 `NULL` `ListItem`が選択されています。


ブラウザーから進行状況を表示する時間がかかります。 製品を編集するときに注意してください、`Categories`と`Suppliers`Dropdownlist の両方がある (なし)、DropDownList の開始時のオプション。


[![カテゴリと仕入先の Dropdownlist を (なし) オプションを含める](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**図 10**:`Categories`と`Suppliers`Dropdownlist には、() オプションが含まれます ([フルサイズの画像を表示する をクリックします](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))。


データベースとしてオプション (なし) を保存する`NULL`値を返す必要があります、`UpdateCommand`イベント ハンドラー。 変更、`categoryIDValue`と`supplierIDValue`変数を null 許容の整数であるし、その値を割り当てる以外の`Nothing`場合にのみ、DropDownList の`SelectedValue`は空の文字列ではありません。


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

この変更では、値を`Nothing`に渡される、 `UpdateProduct` BLL メソッド ()、ユーザーが選択しなかった場合のオプションに対応するドロップダウン リストのいずれかから、`NULL`データベースの値。

## <a name="summary"></a>まとめ

このチュートリアルより複雑なテキスト ボックス、2 つの Dropdownlist、および検証コントロールと共にチェック ボックスをオンに 3 つの異なる入力 Web コントロールに含まれる DataList 編集インターフェイスを作成する方法を説明しました。 編集インターフェイスを構築するときに、手順は、使用されている Web コントロールに関係なく同じ: DataList s に Web コントロールを追加して開始`EditItemTemplate`; データ バインディング構文を使用して、適切な Web に対応するデータ フィールドの値を割り当てるコントロールのプロパティです。で、`UpdateCommand`イベント ハンドラー、Web コントロールと、BLL にその値を渡すことの適切なプロパティをプログラムでアクセスします。

かどうか、編集インターフェイスを作成するときに、秒のテキスト ボックスや別の Web コントロールのコレクションだけで構成、データベースを正しく処理するために必ず`NULL`値。 会計時`NULL`s が正しくないのみを表示する既存の命令型`NULL`編集のインターフェイスがしたもの値として値をマークするための手段が用意されて`NULL`します。 データリストで Dropdownlist を通常は静的なを追加する`ListItem`が`Value`プロパティが空の文字列に明示的に設定 (`Value=""`) にコードを追加して、`UpdateCommand`イベント ハンドラーを決定するかどうか、`NULL``ListItem`が選択されています。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Dennis Patterson、David Suru、および Randy Schmidt でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [次へ](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
