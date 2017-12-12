---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: "(VB) のチェック ボックスの GridView 列を追加する |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、チェック ボックスの列を G. の複数の行を選択した場合の直感的な方法をユーザーに提供する GridView コントロールに追加する方法、."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 326201f9fe9ba5f482308dc8bfd7d2decb9fbd8f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>(VB) のチェック ボックスの GridView 列を追加します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe)または[PDF のダウンロード](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> このチュートリアルを GridView の複数の行を選択した場合の直感的な方法をユーザーに提供する GridView コントロールのチェック ボックスの列を追加する方法を見ます。


## <a name="introduction"></a>はじめに

前のチュートリアルは、特定のレコードを選択するために GridView にラジオ ボタンの列を追加する方法を調査します。 ラジオ ボタンの列は、ユーザーは、グリッドから最大で 1 つの項目を選択するのには限定ときに適切なユーザー インターフェイスです。 ときに、ただし、することも、グリッドからのアイテムの任意の数を取得するユーザーを許可します。 Web ベースの電子メール クライアント、たとえば、通常表示チェック ボックスの列を含むメッセージの一覧です。 ユーザーは、メッセージの任意の数を選択し、電子メールを別のフォルダーに移動または削除を行うなど、何らかのアクションを実行できます。

このチュートリアルではチェック ボックスの列を追加する方法とポストバックにチェックインされたどのようなチェック ボックスを確認する方法が表示されます。 具体的には、web ベースの電子メール クライアントのユーザー インターフェイスを正確に模倣する例をビルドします。 この例では、製品を一覧表示するページの GridView が含まれます、`Products`各チェック ボックスを持つデータベース テーブルの行 (図 1 を参照してください)。 選択した製品の削除ボタンをクリックすると、選択したこれらの製品が削除されます。


[![各製品の行には、チェック ボックスが含まれています。](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**図 1**: 各製品の行には、チェック ボックスが含まれています ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>手順 1: 製品情報を一覧表示するページ GridView の追加

チェック ボックスの列を追加する方法を気にし、前にページングをサポートする GridView では、製品の一覧に最初注目を使用できます。 開いて開始、 `CheckBoxField.aspx`  ページで、`EnhancedGridView`フォルダーと、デザイナーの設定には、ツールボックスからドラッグ、GridView、`ID`に`Products`です。 次に、という名前の新しい ObjectDataSource、GridView にバインドする選択`ProductsDataSource`です。 構成を使用する ObjectDataSource、`ProductsBLL`クラス、呼び出し、`GetProducts()`データを返すメソッド。 この GridView は読み取り専用になる、ため、更新、挿入でドロップダウン リストを設定し、(なし) にタブを削除します。


[![ProductsDataSource をという名前の新しい ObjectDataSource を作成します。](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**図 2**: 名前付き新しい ObjectDataSource 作成`ProductsDataSource`([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![構成、ObjectDataSource GetProducts() メソッドを使用してデータを取得するには](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**図 3**: 構成を使用してデータを取得する ObjectDataSource、`GetProducts()`メソッド ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![UPDATE、INSERT のドロップダウン リストを設定し、(なし) タブを削除します。](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**図 4**: (なし) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


データ ソース構成ウィザードを完了すると、Visual Studio は BoundColumns および製品に関連するデータ フィールドの CheckBoxColumn 自動的に作成されます。 前のチュートリアルで行ったように、削除以外のすべての`ProductName`、 `CategoryName`、および`UnitPrice`BoundFields、し、変更、`HeaderText`製品カテゴリ、および価格プロパティです。 構成、 `UnitPrice` BoundField その値が通貨として書式設定されるようにします。 また、スマート タグからページングを有効にする チェック ボックスをチェックしてページングをサポートする GridView を構成します。

追加のユーザー インターフェイスを選択した製品を削除することも s を使用できます。 設定、GridView の下にあるボタン Web コントロールを追加、`ID`に`DeleteSelectedProducts`とその`Text`プロパティを選択した製品を削除します。 データベースから製品を実際に削除するではなくこの例でおだけを表示、削除された製品を示すメッセージがします。 これに合わせて、ボタンの下にラベル Web コントロールを追加します。 ID を設定`DeleteResults`をクリア、その`Text`プロパティ、およびセットその`Visible`と`EnableViewState`プロパティ`False`です。

これらの変更を加えたら、GridView、ObjectDataSource、ボタン、およびラベルは宣言型マークアップを次のように行ってください。


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

ブラウザーでページを表示する (図 5 を参照してください)。 この時点で名前、カテゴリ、および最初の 10 個の製品の価格を参照する必要があります。


[![名前、カテゴリ、および最初の 10 個の製品の価格の一覧が表示されます。](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**図 5**: Name、カテゴリ、および最初の 10 個の製品の価格の一覧が表示されます ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>手順 2: チェック ボックスの列を追加します。

ASP.NET 2.0 には、CheckBoxField が含まれているために使用 GridView にチェック ボックスの列を追加することが考えることができます。 残念ながら、れていない場合は、CheckBoxField がブール型のデータ フィールドを使用するために設計されました。 つまり、CheckBoxField を使用するために基になるデータ フィールドが表示されたチェック ボックスがオンになっているかどうかを決定する値を持つを検討する必要があります指定します。 CheckBoxField のチェック ボックスをオンになっていない列を含めるだけを使用できません。

代わりに、TemplateField を追加し、するチェック ボックスをオン Web コントロールを追加する必要があります、`ItemTemplate`です。 TemplateField を追加してください、 `Products` GridView され、最初の (左端まで) フィールドになります。 GridView s のスマート タグからのテンプレートの編集リンクをクリックし、ツールボックスから、チェック ボックスをオン Web コントロールをドラッグ、`ItemTemplate`です。 このチェック ボックスをオン s 設定`ID`プロパティを`ProductSelector`です。


[![TemplateField の ItemTemplate を ProductSelector をという名前のチェック ボックスをオン Web コントロールを追加します。](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**図 6**: という名前 チェック ボックス Web コントロールを追加`ProductSelector`TemplateField s `ItemTemplate` ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


各行には TemplateField CheckBox Web コントロールに追加された、チェック ボックスが含まれています。 図 7 は、TemplateField とチェック ボックスを追加した後、ブラウザーで表示したときに、このページを示します。


[![各製品の行には、チェック ボックスが含まれています](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**図 7**: 各製品の行には、チェック ボックスが含まれています ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>手順 3: はポストバックでチェックされたどのようなチェック ボックスを決定します。

この時点で、列のチェック ボックスがどのようなチェック ボックスがポストバック時にチェックを決定する方法がありませんがあります。 ただし、選択した製品の削除 ボタンがクリックされたときに、これらの製品を削除するのにはどのようなチェック ボックスをオンにチェックインされたおく必要があります。

GridView s [ `Rows`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows.aspx)GridView 内のデータ行へのアクセスを提供します。 これらの行を反復処理できる CheckBox コントロールをプログラムでアクセスし、しを参照してください、`Checked`チェック ボックスが選択されているかどうかを決定するプロパティです。

イベント ハンドラーを作成、`DeleteSelectedProducts`ボタン Web コントロールの`Click`イベントし、次のコードを追加します。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

`Rows`プロパティのコレクションを返します`GridViewRow`GridView のデータ行をインスタンス化します。 `For Each`ループをここでは、このコレクションを列挙します。 各`GridViewRow`オブジェクト、行のチェック ボックスを使用してプログラムでアクセス`row.FindControl("controlID")`です。 チェック ボックスをオンにした場合、秒の行の対応する`ProductID`から値を取得、`DataKeys`コレクション。 この演習では単に情報メッセージを表示で、`DeleteResults`ラベル付け、実用的なアプリケーションで d は代わりにも作成への呼び出し、`ProductsBLL`クラスの`DeleteProduct(productID)`メソッドです。

このイベント ハンドラーの追加により、今すぐ選択した製品の削除 ボタンをクリックすると表示、`ProductID`選択した製品の秒。


[![選択されている製品の削除ボタンがクリックされたときに、選択した製品 Productid が一覧表示されます。](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**図 8**: ときに、削除選択されている製品ボタンがクリックされた選択した製品`ProductID`s が一覧表示されます ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>手順 4: 追加のすべてをチェックおよびすべてのボタンをオフに

場合は、ユーザーが現在のページ上のすべての製品を削除しようとすると、10 個のチェック ボックスの各チェックする必要があります。 すべてを確認して追加することでこのプロセスを高速化を支援ボタンをクリックすると、グリッド内のすべてのチェック ボックスの選択します。 オフにしてすべてのボタンが均等に役に立ちます。

GridView 上に配置すること、ページには、2 つのボタンの Web コントロールを追加します。 最初の 1 つの s を設定`ID`に`CheckAll`とその`Text`すべてをチェックするプロパティ以外のセット、2 つ目の 1 つの s`ID`に`UncheckAll`とその`Text`プロパティをすべてオフにします。


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

という名前の分離コード クラスにメソッドを次に、作成`ToggleCheckState(checkState)`、呼び出されると、列挙、 `Products` GridView s`Rows`コレクション設定の各チェック ボックスをオン s および`Checked`プロパティを渡されたの値にで*checkState*パラメーター。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

次に、作成`Click`のイベント ハンドラー、`CheckAll`と`UncheckAll`ボタン。 `CheckAll` S イベント ハンドラー、単に呼び出し`ToggleCheckState(True)` `UncheckAll`、呼び出す`ToggleCheckState(False)`です。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

このコードは、すべてのボタンをクリックするとポストバックを発生させるし、すべて GridView で対応するチェック ボックスのチェックします。 同様に、すべてをオフにしてをクリックすると、すべてのチェック ボックスが選択解除します。 図 9 は、すべてのボタンがチェックされた後、画面を示しています。


[![[すべて] ボタン、チェックをクリックすると、すべてのチェック ボックスを選択します。](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**図 9**: チェックすべてボタンを選択すべてチェック ボックスをクリックすると ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> ときに、チェック ボックスをオンまたはオフにすると、対応するチェック ボックスのすべての方法の 1 つの列を表示するには、ヘッダー行のチェック ボックスです。 さらに、すべて現在チェック/オフにしてすべての実装には、ポストバックが必要です。 対応するチェック ボックス使用できる checked または unchecked、ただし、完全 ~ クライアント側-スクリプト、良くユーザー エクスペリエンスを提供します。 探索と共にクライアント側の技法の使用についての詳細ですべてをチェックし、すべてオフのヘッダー行のチェック ボックスを使用するのにはチェック アウト[チェックすべてのチェック ボックスで、クライアント側スクリプトを使用して GridView と、すべてチェック ボックスの確認](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx)です。


## <a name="summary"></a>概要

行の任意の数を続行する前に GridView から選択できるようにする必要がある場合、1 つのオプションは、チェック ボックスの列を追加します。 このチュートリアルで説明したとおり、GridView のチェック ボックスの列を含むだけで、CheckBox Web コントロールで TemplateField を追加します。 (前のチュートリアルで行ったようには、マークアップ、テンプレートに直接挿入する) と Web コントロールを使用して、ASP.NET は、何のチェック ボックス、ポストバック間ではチェックされませんでしたに自動的に保存します。 対応するチェック ボックスのコードで確認するのに指定されたチェック ボックスを確認するかどうかチェック状態を変更するプログラムからもアクセスできます。

このチュートリアルと最後の 1 つは、GridView に、行セレクターの列を追加することで検索します。 [次へ]、チュートリアルでは、作業のビット、お機能を追加できます挿入 GridView にについて確認します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

>[!div class="step-by-step"]
[前へ](adding-a-gridview-column-of-radio-buttons-vb.md)
[次へ](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
