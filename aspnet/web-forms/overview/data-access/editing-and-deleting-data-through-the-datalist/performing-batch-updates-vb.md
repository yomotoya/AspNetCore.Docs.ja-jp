---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: バッチ更新 (VB) を実行する |Microsoft ドキュメント
author: rick-anderson
description: 完全に編集を作成する方法を学習 DataList ですべての項目が編集モードと値を持つはすべて '更新' ボタンをクリックして保存できます、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d28a431c2b09de8c46079e888aa191017de4e30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "30892388"
---
<a name="performing-batch-updates-vb"></a>バッチ更新 (VB) を実行します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe)または[PDF のダウンロード](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> 完全に編集を作成する方法を学習 DataList ですべての項目が編集モードと値を持つは、ページ上の [すべて更新] ボタンをクリックして保存できます。


## <a name="introduction"></a>はじめに

[前のチュートリアル](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)アイテム レベル DataList を作成する方法について説明しました。 編集可能な標準の GridView DataList 内の各項目が含まれるように編集ボタンで、クリックされると、項目が編集可能な実行するようにします。 このアイテム レベル随時更新のみがデータに対して適切編集の動作、中に特定の使用シナリオは多くのレコードを編集するユーザーが必要です。 ユーザーが数十個のレコードを編集する必要がある、編集 をクリックし、変更を行い、1 つの更新 をクリックして強制された場合をクリックすると量には、ユーザーの生産性が妨げられる可能性がします。 このような状況より良いオプションは、DataList 編集機能を提供する 1 つの場所*すべて*その項目は編集モードと値を持つは、ページ上のすべての更新ボタンをクリックして編集できます (図 1 を参照してください)。


[![編集可能な完全 DataList 内の各項目を変更することができます。](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**図 1**: 編集可能な完全 DataList の各項目を変更することができます ([フルサイズのイメージを表示するをクリックして](performing-batch-updates-vb/_static/image3.png))


このチュートリアルでは完全に編集可能な DataList を使用して仕入先アドレス情報を更新するユーザーを有効にする方法について確認します。

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>手順 1: DataList の ItemTemplate の編集可能なユーザー インターフェイスを作成します。

チュートリアルでは、前、ここで、項目レベルの標準的な編集可能な DataList を作成することを使用した 2 つのテンプレート。

- `ItemTemplate` 読み取り専用のユーザー インターフェイス (各製品の名前と価格を表示するためのラベルの Web コントロール) が含まれています。
- `EditItemTemplate` 編集モードのユーザー インターフェイス (2 つのテキスト ボックスの Web コントロール) が含まれています。

DataList s`EditItemIndex`プロパティではどのような`DataListItem`(存在する場合) を使用して描画、`EditItemTemplate`です。 具体的には、`DataListItem`が`ItemIndex`値に一致する DataList s`EditItemIndex`を使用してプロパティを表示、`EditItemTemplate`です。 このモデルは、完全に編集可能な DataList を作成するときに、時間が間隔の割合が 1 つだけの項目を編集できる場合に機能します。

編集機能の DataList のたい*すべて*の`DataListItem`編集可能なインターフェイスを使用して表示します。 これを実現する最も簡単な方法で編集可能なインターフェイスの定義には、`ItemTemplate`です。 仕入先のアドレス情報を変更するため、編集可能なインターフェイスには、アドレス、city、および国の値のテキストとし、テキスト ボックスとして仕入先名が含まれています。

開いて開始、 `BatchUpdate.aspx`  ページで、DataList コントロールを追加して、設定、`ID`プロパティを`Suppliers`です。 という名前の新しい ObjectDataSource コントロールを追加することを選択、DataList s のスマート タグから`SuppliersDataSource`です。


[![SuppliersDataSource をという名前の新しい ObjectDataSource を作成します。](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**図 2**: 名前付き新しい ObjectDataSource 作成`SuppliersDataSource`([フルサイズのイメージを表示するをクリックして](performing-batch-updates-vb/_static/image6.png))


構成を使用してデータを取得する ObjectDataSource、`SuppliersBLL`クラスの`GetSuppliers()`メソッド (図 3 を参照してください)。 同様に、前のチュートリアルではなく、ObjectDataSource を仕入先情報を更新する、ビジネス ロジック層を直接操作を行います。 します。 そのため、ドロップダウン リストを [(なし)、更新プログラム] タブで設定 (図 4 を参照してください)。


[![GetSuppliers() メソッドを使用して仕入先の情報を取得します。](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**図 3**: 取得仕入先情報を使用して、`GetSuppliers()`メソッド ([フルサイズのイメージを表示するをクリックして](performing-batch-updates-vb/_static/image9.png))


[![更新プログラム タブで、ドロップダウン リストを (なし) を設定します。](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**図 4**: 更新] タブで、ドロップダウン リストを [(なし) を設定 ([フルサイズのイメージを表示するをクリックして](performing-batch-updates-vb/_static/image12.png))


ウィザードを完了すると、Visual Studio を自動的に生成 DataList の`ItemTemplate`Label Web コントロール内のデータ ソースによって返される各データ フィールドを表示します。 代わりに、編集用のインターフェイスを提供するように、このテンプレートを変更する必要があります。 `ItemTemplate` DataList s のスマート タグからテンプレートの編集オプションを使用してデザイナーを使用したり、宣言構文から直接にカスタマイズすることができます。

すぐに、編集するインターフェイスを作成して、テキストとして供給業者の名前が表示されますが、仕入先のアドレス、city、および国の値のテキスト ボックスが含まれています。 これらの変更を加えたら、ページ s の宣言構文は次のようになります。


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> 前のチュートリアルとに、このチュートリアルでは、DataList にも、そのビュー ステートを有効になっている必要があります。


`ItemTemplate`すれば m 2 つの新しい CSS クラスを使用して`SupplierPropertyLabel`と`SupplierPropertyValue`に追加されましたが、`Styles.css`クラスし、同じスタイル設定を使用するように構成、`ProductPropertyLabel`と`ProductPropertyValue`CSS クラス。


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

これらの変更を行った後は、ブラウザーからこのページを参照してください。 図 5 に示す、DataList の各項目はテキストとして仕入先の名前を表示し、アドレス、city、country を表示するテキスト ボックスを使用します。


[![DataList で各仕入先が編集可能です。](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**図 5**: DataList で各業者は編集可能 ([フルサイズのイメージを表示するをクリックして](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>手順 2: すべてのボタンの更新プログラムの追加

図 5 に、各仕入先には、アドレス、市区町村と都道府県 フィールドのテキスト ボックスに表示されますが、現在はありません更新ボタン使用できます。 項目ごとの更新ボタンをクリックするのではなく完全に編集可能なデータ リストというが通常すべて更新ボタンを 1 つ ページで、クリックすると、更新*すべて*DataList 内のレコードです。 このチュートリアルでは、秒 (ただし、いずれかのボタンをクリックすると同じ効果があります) のページの上部にある 1 つと下部にある 1 つの 2 つの更新すべてボタンの追加を使用できます。

DataList のセット上のボタンの Web コントロールを追加することによって開始その`ID`プロパティを`UpdateAll1`です。 次に、設定、DataList の下に 2 番目のボタンの Web コントロールを追加、`ID`に`UpdateAll2`です。 設定、`Text`すべて更新する 2 つのボタンのプロパティです。 最後に、両方のボタンのイベント ハンドラーを作成`Click`イベント。 各イベント ハンドラーで更新ロジックを複製するのではなく let s が 3 番目のメソッドには、そのロジックをリファクター `UpdateAllSupplierAddresses`、単にこの 3 つ目のメソッドを呼び出すイベント ハンドラーを持ちます。


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

図 6 は、更新プログラムのすべてのボタンを追加した後に、ページを示します。


[![2 つの更新すべてボタンがページに追加されました](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**図 6**: 2 つの更新すべてボタンがページに追加されました ([フルサイズのイメージを表示するをクリックして](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>手順 3: 更新すべての仕入先のアドレス情報

すべての編集のインターフェイスを表示する DataList の項目と、すべての更新ボタンを追加すると、すべてそのままは、バッチ更新を実行するコードを書き込んでいます。 具体的には、DataList の項目と呼び出しをループする必要があります、`SuppliersBLL`クラスの`UpdateSupplierAddress`それぞれのメソッドです。

コレクション`DataListItem`DataList DataList s 経由でアクセスできる、その構成をインスタンス化[`Items`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx)です。 参照で、 `DataListItem`、対応するをつかんでお`SupplierID`から、`DataKeys`コレクションと、プログラムで TextBox Web が内で制御の参照、`ItemTemplate`次のコードに示すように。


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

ユーザーでは、いずれかの更新プログラムのすべてのボタンがクリックすると、`UpdateAllSupplierAddresses`メソッドは、それぞれを反復処理`DataListItem`で、 `Suppliers` DataList と呼び出し、`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッドを対応する値を渡します。 アドレス、都市、または国のパスのない入力値は、の値`Nothing`に`UpdateSupplierAddress`ではなく、空の文字列)、その結果は、データベースで`NULL`の基になるレコードのフィールドです。

> [!NOTE]
> 拡張機能としてバッチ更新が実行された後に、いくつかの確認メッセージを提供するページに状態 Label Web コントロールを追加します。


## <a name="updating-only-those-addresses-that-have-been-modified"></a>変更されているアドレスのみを更新

このチュートリアルの呼び出しのために使用されるバッチ更新アルゴリズム、`UpdateSupplierAddress`メソッドを*すべて*アドレス情報が変更されたかどうかに関係なく、DataList で業者にします。 このような blind は t 通常、パフォーマンスの問題を更新中にデータベース テーブルの変更の監査する場合に余分なレコードをつながることができます。 たとえば、すべてを記録するトリガーを使用する`UPDATE`s、`Suppliers`監査テーブル、ユーザーが新しい監査レコードは、システムでは、ユーザーがいずれかを作成するかどうかに関係なく各仕入先の作成がすべて更新 ボタンをクリックするたびに変更します。

ADO.NET DataTable およびデータ アダプターのクラスは、データベースのすべての通信にのみ、変更、削除、および新しいレコードの結果をバッチ更新をサポートするために設計されています。 DataTable 内の各行が、 [ `RowState`プロパティ](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)行が変更されると、そこから削除された、DataTable に追加されましたまたはが変更されないかどうかを示すです。 DataTable の内容が最初に表示されたら、すべての行が変更されていないマークされます。 行は変更済みとしてマークの任意の行の列の値を変更します。

`SuppliersBLL`に 1 つの仕入先のレコードの最初の読み取りで指定された業者のアドレス情報を更新クラス、`SuppliersDataTable`し、設定、 `Address`、 `City`、および`Country`次のコードを使用して列の値。


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

このコードは、単純には、渡されたアドレス、市区町村、および国の値に割り当てます、`SuppliersRow`で、`SuppliersDataTable`値が変更されたかどうかに関係なく。 これらの変更により、 `SuppliersRow` s`RowState`プロパティを変更済みとしてマークされます。 ときに、データ アクセス層 s`Update`メソッドが呼び出されると、そのされることを確認、`SupplierRow`変更されており、したがって送信、`UPDATE`コマンドをデータベースにします。

しかしと異なる場合のみ、渡されたアドレス、市区町村、および国の値を割り当てるには、このメソッドにコードを追加しました、 `SuppliersRow` s 既存の値。 アドレス、市区町村と国が既存のデータと同じである場合は、変更は行われません、 `SupplierRow` s`RowState`としてマークされている変更しません。 最終的な結果がいるときに DAL s`Update`メソッドが呼び出されると、データベースの呼び出しは行われませんので、`SuppliersRow`は変更されていません。

この変更を適用するを無条件には、渡されたアドレス、city、および国の値を次のコードを代入するステートメントを置き換えます。


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

これには、コード、DAL s を追加しました。`Update`メソッド送信、`UPDATE`ステートメントがアドレスに関連する値が変更されたレコードだけをデータベースにします。

または、おでしたの渡されたアドレスのフィールドとデータベースのデータの間で違いがあるかどうかを追跡し、none を使用する必要がある場合は、DAL s への呼び出しをバイパス単に`Update`メソッドです。 このアプローチは DB 直接的な方法ではありません t が渡されるので、メソッドを指示する、DB を使用している場合にも、`SuppliersRow`インスタンスを`RowState`データベースの呼び出しが実際に必要かどうかを決定するチェックすることができます。

> [!NOTE]
> 毎回、`UpdateSupplierAddress`メソッドが呼び出されるは、データベースに、更新されたレコードに関する情報を取得する呼び出しが行われます。 次に、データに変更がある場合は、テーブルの行を更新するデータベースへの別の呼び出しが行われます。 このワークフローを作成することで最適化する可能性があります、`UpdateSupplierAddress`を受け入れるメソッドのオーバー ロード、`EmployeesDataTable`があるインスタンス*すべて*からの変更、`BatchUpdate.aspx`ページ。 次に行うことができる、すべてのレコードから取得するデータベースに 1 回の呼び出し、`Suppliers`テーブル。 2 つの結果セットを列挙し、および変更が発生したレコードのみを更新できませんできます。


## <a name="summary"></a>まとめ

このチュートリアルでは、複数の仕入先のアドレス情報をすばやく変更するユーザーを許可する、完全に編集可能な DataList を作成する方法を説明しました。 DataList s の仕入先の住所、city、および国の値 ボックスに Web コントロールの編集のインターフェイスを定義することによって開始`ItemTemplate`です。 次に、DataList の上下の更新プログラムのすべてのボタンを追加します。 ユーザーが自分の変更を行ったし、いずれかの更新プログラムのすべてのボタンをクリックした後、 `DataListItem` s が列挙を呼び出すと、`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッドが行われます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Zack Jones および Ken Pespisa がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [次へ](handling-bll-and-dal-level-exceptions-vb.md)
