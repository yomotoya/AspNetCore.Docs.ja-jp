---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: バッチ更新 (VB) を実行する |Microsoft Docs
author: rick-anderson
description: 完全に編集を作成する方法について説明します DataList ですべての項目が編集モードとで、[すべて更新] ボタンをクリックして保存できる値を持つ、.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ab034880c8140e6156721956059b7cdd3f077b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838933"
---
<a name="performing-batch-updates-vb"></a>バッチ更新 (VB) を実行します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe)または[PDF のダウンロード](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> 完全に編集を作成する方法について説明します DataList ですべての項目が編集モードと ページの すべて更新 ボタンをクリックして値を保存できます。


## <a name="introduction"></a>はじめに

[前のチュートリアル](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)アイテム レベル DataList を作成する方法について確認しました。 編集可能な標準の GridView、DataList 内の各項目に含まれるように編集ボタンで、クリックされると、項目の編集可能になります。 この項目のレベルも随時更新のみがデータの編集の動作、中に特定のユース ケース シナリオには多くのレコードを編集するユーザーが必要です。 ユーザーが数十個のレコードを編集する必要があるし、編集 をクリックして、自分の変更を行い、1 つの更新 をクリックしてしなければ、クリックしての量は、生産性を妨げられることができます。 DataList で完全に編集を提供することですより適切なオプションでは、このような状況では、1 つの where*すべて*その項目の編集モードでページ上のすべての更新ボタンをクリックして値を編集できますが (図 1 参照)。


[![完全に編集可能な DataList 内の各項目を変更できます。](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**図 1**: 完全に編集可能な DataList で各項目を変更できます ([フルサイズの画像を表示する をクリックします](performing-batch-updates-vb/_static/image3.png))。


このチュートリアルでは完全に編集可能な DataList を使用して仕入先のアドレス情報を更新するユーザーを有効にする方法を説明します。

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>手順 1: DataList s itemtemplate の編集可能なユーザー インターフェイスを作成します。

前のチュートリアルで、標準的なアイテム レベルの編集可能な DataList を作成すること使用されている 2 つのテンプレート。

- `ItemTemplate` 読み取り専用のユーザー インターフェイス (各製品の名前と価格を表示するためのラベルの Web コントロール) が含まれています。
- `EditItemTemplate` 編集モードのユーザー インターフェイス (2 つの TextBox Web コントロール) が含まれています。

DataList s`EditItemIndex`プロパティではどのような`DataListItem`(ある場合) を使用して、表示、`EditItemTemplate`します。 具体的には、`DataListItem`が`ItemIndex`値に一致する DataList s`EditItemIndex`を使用してプロパティを表示、`EditItemTemplate`します。 このモデルでは、完全に編集可能な DataList を作成するときに、時間が間隔が 1 つだけの項目を編集できる場合に機能します。

DataList で完全に編集可能にする*すべて*の`DataListItem`編集可能なインターフェイスを使用して表示します。 これを実現する最も簡単な方法で編集可能なインターフェイスの定義には、`ItemTemplate`します。 仕入先のアドレス情報を変更するため、編集可能なインターフェイスには、アドレス、市区町村、および国の値のテキストとし、テキスト ボックスと業者の名前が含まれています。

開いて開始、`BatchUpdate.aspx`ページで、DataList コントロールを追加し、設定、`ID`プロパティを`Suppliers`します。 という名前の新しい ObjectDataSource コントロールを追加することを選択、DataList s のスマート タグから`SuppliersDataSource`します。


[![SuppliersDataSource という名前の新しい ObjectDataSource を作成します。](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**図 2**: 名前付き新しい ObjectDataSource 作成`SuppliersDataSource`([フルサイズの画像を表示する をクリックします](performing-batch-updates-vb/_static/image6.png))。


構成を使用してデータを取得する ObjectDataSource、`SuppliersBLL`クラスの`GetSuppliers()`メソッド (図 3 を参照してください)。 前のチュートリアルではなく、ObjectDataSource を仕入先情報の更新と同様、ビジネス ロジック層を直接操作を行います。 します。 そのため、更新プログラム] タブで [(なし) ドロップダウン リストを設定 (図 4 参照)。


[![GetSuppliers() メソッドを使用して仕入先情報を取得します。](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**図 3**: 取得仕入先情報を使用して、`GetSuppliers()`メソッド ([フルサイズの画像を表示する をクリックします](performing-batch-updates-vb/_static/image9.png))。


[![更新プログラム タブで、ドロップダウン リストを (なし) を設定します。](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**図 4**: (None) にドロップダウン リストを更新 タブで設定 ([フルサイズの画像を表示する をクリックします](performing-batch-updates-vb/_static/image12.png))。


ウィザードを完了すると、Visual Studio を自動的に生成 DataList の`ItemTemplate`Label Web コントロール内のデータ ソースによって返される各データ フィールドの表示にします。 代わりに、編集用のインターフェイスを提供するように、このテンプレートを変更する必要があります。 `ItemTemplate` DataList s のスマート タグからのテンプレートの編集オプションを使用して、デザイナー、または宣言型構文を通じて直接カスタマイズできます。

テキスト、として、サプライヤーの名前を表示しますが、仕入先の住所や市区町村、国の値のテキスト ボックスが含まれています編集インターフェイスを作成する時間がかかります。 これらの変更を加えたら、ページの宣言構文は次のようなになります。


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> このチュートリアルとに、このチュートリアルでは、DataList にも、そのビュー ステートを有効になっている必要があります。


`ItemTemplate`は 2 つの新しい CSS クラスを使用して`SupplierPropertyLabel`と`SupplierPropertyValue`に追加されましたが、`Styles.css`クラスし、同じスタイル設定を使用するように構成、`ProductPropertyLabel`と`ProductPropertyValue`CSS クラス。


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

これらの変更を行った後は、ブラウザーからこのページを参照してください。 図 5 に示すよう各 DataList 項目はテキストとして仕入先の名前を表示し、アドレス、市区町村、国を表示するテキスト ボックスを使用します。


[![各仕入先、DataList では編集可能なです。](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**図 5**: DataList で各仕入先が編集可能な ([フルサイズの画像を表示する をクリックします](performing-batch-updates-vb/_static/image15.png))。


## <a name="step-2-adding-an-update-all-button"></a>手順 2: すべてのボタンの更新プログラムの追加

図 5 には、各業者は、そのアドレス、市区町村、およびテキスト ボックスに表示される国のフィールドには、現在はありません更新ボタンをクリックします。 項目ごとの Update ボタンではなくで完全に編集可能なデータリストが通常すべて更新ボタンを 1 つページで、クリックすると、更新*すべて*DataList 内のレコード。 このチュートリアルでは、s (ただし、いずれかのボタンをクリックすると、同じ効果がありますが)、-、ページの上部にあるいずれかと、下部にあるいずれかの 2 つのすべて更新ボタンを追加できます。

DataList とセットの上ボタン Web コントロールを追加することにより、その`ID`プロパティを`UpdateAll1`します。 次に、設定、DataList、下にある 2 つ目のボタンの Web コントロールを追加、`ID`に`UpdateAll2`します。 設定、`Text`すべて更新する 2 つのボタンのプロパティ。 最後に、2 つのボタン イベント ハンドラーを作成`Click`イベント。 3 番目のメソッドにそのロジックをリファクタリングする let s の各イベント ハンドラーで更新ロジックを複製するのではなく`UpdateAllSupplierAddresses`、イベント ハンドラーをこの 3 つ目のメソッドを呼び出すだけです。


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

図 6 は、更新プログラムのすべてのボタンを追加した後に、ページを示します。


[![2 つの更新プログラムすべてのボタンがページに追加されました](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**図 6**: 2 つの更新プログラムすべてのボタンがページに追加されました ([フルサイズの画像を表示する をクリックします](performing-batch-updates-vb/_static/image18.png))。


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>手順 3: すべての仕入先のアドレス情報を更新しています

すべての編集インターフェイスを表示する DataList s のアイテムと、すべての更新ボタンを追加するとすべてそのままは、バッチ更新を実行するコードを記述は。 具体的には、DataList の項目と呼び出しをループする必要があります、`SuppliersBLL`クラスの`UpdateSupplierAddress`それぞれのメソッド。

コレクション`DataListItem`DataList s を介してアクセスできるよう、DataList、その構成をインスタンス化[`Items`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx)します。 参照を`DataListItem`、対応するを取得できます`SupplierID`から、`DataKeys`コレクションとプログラムでテキスト ボックスに Web コントロールの内の参照、`ItemTemplate`次のコードに示すように。


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

いずれかの更新プログラムのすべてのボタンをクリックすると、`UpdateAllSupplierAddresses`メソッドは、それぞれを反復処理`DataListItem`で、 `Suppliers` DataList、および呼び出し、`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッドは、対応する値を渡します。 アドレス、市区町村、国のパスの入力以外の値は、の値`Nothing`に`UpdateSupplierAddress`(空白文字列の場合) ではなく、データベースになる`NULL`の基になるレコードのフィールド。

> [!NOTE]
> 拡張機能としてバッチ更新が実行された後に、いくつかの確認メッセージを提供するページに状態ラベル Web コントロールを追加します。


## <a name="updating-only-those-addresses-that-have-been-modified"></a>変更されたアドレスのみを更新しています

このチュートリアルの呼び出しに使用されるバッチ更新アルゴリズム、`UpdateSupplierAddress`メソッド*すべて*自分の住所情報が変更されているかどうかに関係なく、DataList で業者にします。 このような blind 更新は t は、通常、パフォーマンスの問題、発生する可能性余分なレコード re 監査するが、データベース テーブルに変更された場合。 たとえば、すべてを記録するトリガーを使用する`UPDATE`s、`Suppliers`監査テーブル、ユーザーは、ユーザーがいずれかを行われるかどうかに関係なく、システムで、各仕入先の新しい監査レコードを作成するがすべて更新 ボタンをクリックするたびに変更します。

ADO.NET DataTable、DataAdapter クラスは、のみ、変更、削除、および新しいレコードがデータベース通信では、結果をバッチ更新をサポートするために設計されています。 データ テーブルの各行が、 [ `RowState`プロパティ](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)行がから、変更、削除、DataTable に追加されたまたはままかどうかを示します。 DataTable の内容が最初に表示されたら、すべての行が変更されていないマークされます。 任意の行の列の値を変更すると、変更済みとして、行がマークされます。

`SuppliersBLL`クラスに 1 つの仕入先のレコードの最初の読み込み、指定した仕入先のアドレス情報を更新しました、`SuppliersDataTable`し、設定、 `Address`、 `City`、および`Country`次のコードを使用して列の値。


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

このコードは、単純では、渡されたアドレス、市区町村、および国の値に割り当てます、`SuppliersRow`で、`SuppliersDataTable`値が変更されたかどうかに関係なく。 これらの変更により、 `SuppliersRow` s`RowState`プロパティを変更済みとしてマークされます。 ときにデータ アクセス層 s`Update`メソッドが呼び出されると、認識する、`SupplierRow`変更されており、したがって送信、`UPDATE`データベースにコマンド。

しかし、これらとは異なる場合のみ、渡されたアドレス、市区町村、および国の値を割り当てるには、このメソッドにコードを追加しました、 `SuppliersRow` s の既存の値。 アドレス、市区町村、国が同じである既存のデータとしてのケースでは変更されない、 `SupplierRow` s`RowState`としてマークされている左を unchanged に指定します。 最終的な結果がいるときに DAL s`Update`メソッドが呼び出されると、データベースの呼び出しは行われませんので、`SuppliersRow`が変更されていません。

この変更を適用するには、渡されたアドレス、市区町村、国の値を次のコードを無条件に割り当てることがステートメントに置き換えます。


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

この追加コード、DAL s`Update`メソッド送信、`UPDATE`ステートメントはそのアドレスに関連する値が変更されたレコードだけをデータベースにします。

または、でしたの渡されたアドレス フィールドと、データベースのデータとの違いがあるかどうかを追跡し、なしを使用する必要がある場合は、DAL の呼び出しをバイパス単に`Update`メソッド。 このアプローチは DB のダイレクト メソッドは t が渡されるため、メソッドを直接、DB を使用する場合にも、`SuppliersRow`インスタンスを`RowState`データベース呼び出しが実際に必要かどうかを判断するチェックすることができます。

> [!NOTE]
> 毎回、`UpdateSupplierAddress`メソッドが呼び出される、更新されたレコードに関する情報を取得、データベースへの呼び出しが確立します。 次に、データに変更がある場合は、テーブルの行を更新するデータベースへの別の呼び出しが行われます。 作成して最適化されますが、このワークフロー、`UpdateSupplierAddress`を受け取るメソッド オーバー ロードを`EmployeesDataTable`を持つインスタンス*すべて*からの変更の`BatchUpdate.aspx`ページ。 データベースに 1 回の呼び出しは、すべてのレコードの取得を行う可能性がありますし、`Suppliers`テーブル。 2 つの結果セットを列挙し、でき、変更が発生したレコードのみを更新できませんできます。


## <a name="summary"></a>まとめ

このチュートリアルでは、完全に編集可能な DataList でを複数の仕入先のアドレス情報をすばやく変更するユーザーを作成する方法を説明しました。 S DataList の編集インターフェイス仕入先の住所や市区町村、国の値のテキスト ボックスに Web コントロールを定義することで開始した`ItemTemplate`します。 次に、上と下、DataList の更新プログラムのすべてのボタンを追加します。 ユーザーが自分の変更を行って更新プログラムのすべてのボタンのいずれかをクリックした後、 `DataListItem` s が列挙されると呼び出しを`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッドが呼び出さ。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Zack Jones、Ken Pespisa でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [次へ](handling-bll-and-dal-level-exceptions-vb.md)
