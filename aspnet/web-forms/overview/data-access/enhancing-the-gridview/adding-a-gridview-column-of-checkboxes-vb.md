---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: (VB) のチェック ボックスの GridView 列を追加する |Microsoft Docs
author: rick-anderson
description: このチュートリアルを調べ、G. の複数の行を選択した場合の直感的な方法をユーザーに提供する GridView コントロールにチェック ボックスの列を追加する方法.
ms.author: aspnetcontent
ms.date: 03/06/2007
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 01515e0034d69c563cbc96dceb6ae2ee481cc1a0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819575"
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>(VB) のチェック ボックスの GridView 列を追加します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe)または[PDF のダウンロード](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> このチュートリアルは、GridView コントロールを GridView の複数の行を選択した場合の直感的な方法をユーザーに提供するチェック ボックスの列を追加する方法を検索します。


## <a name="introduction"></a>はじめに

前のチュートリアルでは、特定のレコードを選択するために GridView にラジオ ボタンの列を追加する方法について確認しました。 ラジオ ボタンの列は、グリッドから最大で 1 つの項目を選択することに、ユーザーが限られた場合、適切なユーザー インターフェイスです。 ただし、することもユーザーが任意の数の項目をグリッドから選択できるようにします。 Web ベースの電子メール クライアントでは、たとえば、通常表示チェック ボックスの列を含むメッセージの一覧します。 ユーザーは、メッセージの任意の数を選択し、電子メールを別のフォルダーに移動または削除するなど、いくつかの操作を実行します。

このチュートリアルではチェック ボックスの列を追加する方法およびどのようなチェック ボックスがポストバック時にチェックを確認する方法が表示されます。 具体的には、web ベースの電子メール クライアントのユーザー インターフェイスを正確に模倣する例をビルドします。 この例では、製品の一覧を表示するページの GridView が含まれます、`Products`各チェック ボックスにデータベース テーブルの行 (図 1 参照)。 選択した製品の削除ボタンをクリックすると、選択したこれらの製品が削除されます。


[![各製品の行には、チェック ボックスが含まれています。](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**図 1**: 製品の行ごとにチェック ボックスが含まれています ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))。


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>手順 1: 製品の情報を一覧表示するページの GridView を追加します。

チェック ボックスの列を追加する方法について気にし、前に初めてことに専念ページングをサポートする GridView では、製品の一覧を表示することができます。 開いて開始、`CheckBoxField.aspx`ページで、`EnhancedGridView`フォルダーと、デザイナーの設定には、ツールボックスからドラッグ、GridView、`ID`に`Products`します。 次に、という名前の新しい ObjectDataSource GridView にバインドする選択`ProductsDataSource`します。 構成を使用する ObjectDataSource、`ProductsBLL`クラスを呼び出し、`GetProducts()`データを返すメソッド。 この GridView は読み取り専用になる、ため、UPDATE、INSERT でドロップダウン リストを設定し、(None) にタブを削除します。


[![ProductsDataSource という名前の新しい ObjectDataSource を作成します。](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**図 2**: 名前付き新しい ObjectDataSource 作成`ProductsDataSource`([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))。


[![GetProducts() メソッドを使用してデータを取得する ObjectDataSource を構成します。](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**図 3**: 構成を使用してデータを取得する ObjectDataSource、`GetProducts()`メソッド ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))。


[![UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**図 4**: (None) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))。


データ ソース構成ウィザードを完了すると、Visual Studio は BoundColumns と製品に関連するデータ フィールドの CheckBoxColumn 自動的に作成されます。 前のチュートリアルで行ったような削除以外のすべての`ProductName`、 `CategoryName`、および`UnitPrice`BoundFields、し、変更、`HeaderText`製品カテゴリ、および価格プロパティ。 構成、 `UnitPrice` BoundField、値が通貨として書式設定されるようにします。 また、スマート タグからのページングを有効にするチェック ボックスをオンにページングをサポートするために、GridView を構成します。

秒も選択されている製品を削除するためのユーザー インターフェイスを追加できるようにします。 設定、GridView の下にあるボタンの Web コントロールを追加、`ID`に`DeleteSelectedProducts`とその`Text`プロパティを選択した製品を削除します。 データベースから製品を実際に削除するではなくこの例ではだけを表示、削除された製品を示すメッセージが。 これに合わせて、ボタンの下にラベル Web コントロールを追加します。 ID を設定`DeleteResults`チェック ボックスをオフにその`Text`プロパティ、およびセットの`Visible`と`EnableViewState`プロパティ`False`。

これらの変更を加えたら、GridView、ObjectDataSource、ボタン、およびラベルの宣言型マークアップは、次のようなが必要です。


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

ブラウザーでページを表示する少し (図 5 を参照してください)。 この時点で、名前、カテゴリ、および最初の 10 個の商品の価格があることがわかります。


[![名前、カテゴリ、および最初の 10 個の商品の価格の一覧が表示されます。](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**図 5**: 名前、カテゴリ、および最初の 10 個の商品の価格の一覧が表示されます ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))。


## <a name="step-2-adding-a-column-of-checkboxes"></a>手順 2: チェック ボックスの列を追加します。

ASP.NET 2.0 には、CheckBoxField が含まれているために、使用、GridView にチェック ボックスの列を追加することがいずれかと思うかもしれません。 残念ながらでない場合は、ように、CheckBoxField がブール型のデータ フィールドを使用するように設計します。 つまり、CheckBoxField を使用するには、表示のチェック ボックスをオンになっているかどうかを判断する値が参照される、基になるデータ フィールドを指定します必要があります。 CheckBoxField のチェック ボックスをオフの列を含めるだけに使用できません。

代わりに、TemplateField を追加し、するチェック ボックスを Web コントロールを追加する必要があります、`ItemTemplate`します。 TemplateField を追加してください、 `Products` GridView と、最初の (左端) フィールドになります。 GridView s のスマート タグからのテンプレートの編集リンクをクリックし、ツールボックスから、チェック ボックスを Web コントロールをドラッグ、`ItemTemplate`します。 このチェック ボックス s 設定`ID`プロパティを`ProductSelector`します。


[![TemplateField の ItemTemplate ProductSelector をという名前のチェック ボックスを Web コントロールを追加します。](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**図 6**: という名前チェック ボックスを Web コントロールを追加`ProductSelector`TemplateField s `ItemTemplate` ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))。


各行には TemplateField とチェック ボックスを Web コントロールを追加、チェック ボックスが含まれています。 図 7 は、TemplateField とチェック ボックスを追加した後、ブラウザーで表示したときに、このページを示します。


[![各製品の行が、チェック ボックスが含まれています](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**図 7**: 各製品の行が、チェック ボックスが含まれています ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))。


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>手順 3: は、ポストバックにチェックインされたどのようなチェック ボックスを決定します。

この時点でのチェック ボックスがポストバック時にどのようなチェック ボックスをオンにチェックインされたかを判断する方法がない列があります。 ただし、選択した製品の削除 ボタンがクリックされたときに、これらの製品を削除するにはどのようなチェック ボックスをオンにチェックインされたかを把握する必要があります。

GridView s [ `Rows`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx)GridView にデータ行へのアクセスを提供します。 これらの行を反復処理できること、チェック ボックス コントロールをプログラムでアクセスし、しを参照してください、`Checked`チェック ボックスが選択されているかどうかを決定するプロパティ。

イベント ハンドラーを作成、`DeleteSelectedProducts`ボタン Web コントロールの`Click`イベントし、次のコードを追加します。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

`Rows`プロパティのコレクションを返します`GridViewRow`GridView のデータの行をインスタンス化します。 `For Each`ループをここでは、このコレクションを列挙します。 各`GridViewRow`オブジェクト、行のチェック ボックスを使用してプログラムでアクセス`row.FindControl("controlID")`します。 チェック ボックスをオンにした場合、s 行の対応する`ProductID`から値を取得、`DataKeys`コレクション。 この演習で単に情報メッセージを表示で、`DeleteResults`する実用的なアプリケーションで d 代わりに行ったへの呼び出しが、ラベル、`ProductsBLL`クラスの`DeleteProduct(productID)`メソッド。

このイベント ハンドラーの追加により、今すぐ選択した製品の削除 ボタンをクリックすると表示されます、`ProductID`選択した製品の秒。


[![選択されている製品の削除ボタンがクリックされた場合、選択した製品 Productid が一覧表示されます。](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**図 8**: ときに、削除選択されている製品ボタンがクリックされた選択した製品`ProductID`s が一覧表示されます ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))。


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>手順 4: 追加すべてチェックし、すべてのボタンをオフにします。

場合は、ユーザーが現在のページ上のすべての製品を削除しようとすると、10 個のチェック ボックスの各チェックする必要があります。 このプロセスをすべてチェックを追加することで高速化を支援ボタンをクリックすると、グリッド内のすべてのチェック ボックスの選択。 オフにしてすべてのボタンは均等に便利になります。

GridView の上に配置すること、ページには、2 つのボタンの Web コントロールを追加します。 まず 1 つの s を設定`ID`に`CheckAll`とその`Text`すべてをチェックするプロパティです。 セットは 2 つ目の 1 つの s`ID`に`UncheckAll`とその`Text`プロパティをすべてオフにします。


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

という名前の分離コード クラスにメソッドを次に、作成`ToggleCheckState(checkState)`、呼び出されると、列挙、 `Products` GridView s`Rows`コレクション設定の各チェック ボックス s と`Checked`プロパティを渡されたの値で*checkState*パラメーター。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

次に、作成`Click`のイベント ハンドラー、`CheckAll`と`UncheckAll`ボタン。 `CheckAll` S イベント ハンドラー、単に呼び出し`ToggleCheckState(True)`、 `UncheckAll`、呼び出す`ToggleCheckState(False)`します。


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

このコードでは、すべてのボタンをクリックしてはポストバックが発生して、GridView のチェック ボックスを確認します。 同様に、すべてをオフにします をクリックしては、すべてのチェック ボックスを選択解除します。 図 9 では、すべてのボタンがチェックされた後に、画面が表示されます。


[![チェックのすべてのボタンをクリックすると、すべてのチェック ボックスを選択します。](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**図 9**: チェックすべてボタンを選択しますすべてのチェック ボックスをクリックすると ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))。


> [!NOTE]
> ときに、チェック ボックスをオンまたはオフのチェック ボックスのすべてのアプローチの 1 つの列を表示するには、ヘッダー行のチェック ボックスです。 さらに、すべて現在のチェック/ポストバックをすべてオフに実装が必要です。 チェック ボックスできます checked または unchecked、ただし、完全を使用する込むユーザー エクスペリエンスを提供するためのクライアント側スクリプト。 チェック アウトの詳細については、クライアント側の手法を使用してと共にすべてチェックをすべてオフにヘッダー行のチェック ボックスを使用して探索する[GridView を使用してクライアント側スクリプトとチェックすべてチェック ボックスのチェックすべてチェック ボックス](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx)します。


## <a name="summary"></a>まとめ

続行する前に、GridView から任意の数の行を選択できるようにする必要がある場合、チェック ボックスの列を追加することは 1 つです。 このチュートリアルで説明したように、GridView のチェック ボックスの列を含む、TemplateField と Web のチェック ボックス コントロールを追加します。 (前のチュートリアルで行ったようには、マークアップ、テンプレートに直接挿入する) と Web コントロールを使用して、チェック ボックスでされたを通知し、ポストバック間ではチェックされませんが、ASP.NET が自動的には記憶します。 特定のチェック ボックスをオンになっているかどうかを判断する、またはチェックされた状態を変更するコードのチェックもプログラムでアクセスできます。

このチュートリアルを最後の 1 つ、GridView への行セレクター列の追加について説明しました。 [次へ]、チュートリアルでは、少しの作業を追加する方法の挿入機能 GridView にについて説明します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](adding-a-gridview-column-of-radio-buttons-vb.md)
> [次へ](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
