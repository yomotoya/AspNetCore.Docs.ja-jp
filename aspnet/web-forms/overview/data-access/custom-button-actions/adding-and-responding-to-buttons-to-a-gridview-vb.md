---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: 追加して、(VB) GridView にボタンに応答する |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、テンプレートと GridView、DetailsView コントロールのフィールドの両方にカスタム ボタンを追加する方法を紹介します。 具体的には、ビルド番号があるとしています.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0834d43f95bd19fffb603dcde640714bd779fd80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833548"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>追加して、(VB) GridView にボタンに応答するには
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe)または[PDF のダウンロード](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> このチュートリアルでは、テンプレートと GridView、DetailsView コントロールのフィールドの両方にカスタム ボタンを追加する方法を紹介します。 具体的には、仕入先のページをユーザーに許可するフォーム ビューのあるインターフェイスをビルドします。


## <a name="introduction"></a>はじめに

レポートの多くのシナリオは、レポート データへの読み取り専用アクセスを伴うときに、表示されるデータに基づいてレポート アクションを実行することも少なくします。 レポートに表示される各レコードで、ボタン、LinkButton、ImageButton Web コントロールを追加する通常この関連するをクリックすると、ポストバックが発生して、いくつかのサーバー側コードを呼び出します。 レコード単位に基づいてデータ編集と削除は、最も一般的な例です。 実際には、以降で説明したように、[概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルでは、編集および削除は GridView、DetailsView、FormView コントロールがなく、このような機能をサポートできる一般的なので、1 行のコードを記述するため必要があります。

さらに編集およびボタン、GridView、DetailsView、およびフォーム ビューを削除するコントロールも、ボタン、Linkbutton、または ImageButtons をクリックすると、いくつかのカスタム サーバー側ロジックを実行します。 このチュートリアルでは、テンプレートと GridView、DetailsView コントロールのフィールドの両方にカスタム ボタンを追加する方法を紹介します。 具体的には、仕入先のページをユーザーに許可するフォーム ビューのあるインターフェイスをビルドします。 、特定のサプライヤー FormView に Button Web コントロールをクリックすると、はマーク設定すべてが関連付けられている製品の提供が中止されたと仕入先に関する情報が表示されます。 さらに、GridView は増加の価格と割引価格ボタン クリックすると、発生または製品 s を軽減するを含む行ごとに、選択したサプライヤーによって提供されるこれらの製品を一覧表示されます`UnitPrice`を 10% が (図 1 参照)。


[![FormView や GridView の両方がカスタム アクションを実行するボタンを含む](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**図 1**: 両方、FormView と GridView を含むボタンをカスタムの操作の実行 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))。


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>手順 1: ボタン チュートリアル Web ページの追加

カスタム ボタンを追加する方法を説明する前に、このチュートリアルの必要がありますが、web サイト プロジェクトで ASP.NET ページを作成する最初に少し s を使用できます。 という名前の新しいフォルダーを追加することで開始`CustomButtons`します。 次に、次の 2 つの ASP.NET ページを使用する各ページに関連付けるように、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `CustomButtons.aspx`


![カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**図 2**: カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加


などの他のフォルダーで`Default.aspx`で、`CustomButtons`フォルダーは、チュートリアルのセクションで一覧表示します。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**図 3**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))。


最後に、ページに追加するエントリとして、`Web.sitemap`ファイル。 具体的には、ページングと並べ替えの後に、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューには、編集、挿入、および削除のチュートリアルの項目が含まれています。


![サイト マップが含まれています、エントリにはカスタム ボタンのチュートリアル](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**図 4**: サイト マップが含まれています、エントリにはカスタム ボタンのチュートリアル


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>手順 2: 仕入先の一覧を表示するフォーム ビューの追加

S 仕入先の一覧を表示するフォーム ビューを追加することで、このチュートリアルを開始することができます。 概要で既に説明した、この FormView は GridView では業者によって提供される製品を示す、仕入先を使用してページにユーザーを許可します。 さらに、この FormView がボタンを含めてをクリックするはすべての業者の製品としてマーク廃止されました。 自分たちに関係します、FormView にカスタム ボタンを追加して、前にまず仕入先の情報を表示するように、FormView をだけ作成 s を使用できます。

開いて開始、`CustomButtons.aspx`ページで、`CustomButtons`フォルダー。 FormView をページに、デザイナーとセットには、ツールボックスからドラッグして追加の`ID`プロパティを`Suppliers`します。 という名前の新しい ObjectDataSource を作成することを選択 FormView s のスマート タグから`SuppliersDataSource`します。


[![SuppliersDataSource という名前の新しい ObjectDataSource を作成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**図 5**: 名前付き新しい ObjectDataSource 作成`SuppliersDataSource`([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))。


クエリを実行するように、この新しい ObjectDataSource を構成、`SuppliersBLL`クラスの`GetSuppliers()`メソッド (図 6 参照)。 以降、このフォーム ビューでは、供給業者は、選択、更新プログラム タブで、ドロップダウン リストからオプション (なし) を更新するためのインターフェイスは提供されません。


[![SuppliersBLL クラス GetSuppliers() メソッドを使用するデータ ソースの構成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**図 6**: を使用するデータ ソースの構成、`SuppliersBLL`クラス s`GetSuppliers()`メソッド ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))。


ObjectDataSource を構成した後、Visual Studio が生成されます、 `InsertItemTemplate`、 `EditItemTemplate`、および`ItemTemplate`FormView の。 削除、`InsertItemTemplate`と`EditItemTemplate`および変更、`ItemTemplate`だけ、仕入先の会社名と電話番号が表示されるようにします。 最後に、そのスマート タグからのページングを有効にするチェック ボックスをオン、FormView のページング サポートを有効にする (または、設定してその`AllowPaging`プロパティを`True`)。 これらの変更後、ページの宣言型マークアップを次のようになります。


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

図 7 では、ブラウザーで表示する際、CustomButtons.aspx ページを示します。


[![FormView、CompanyName と、現在選択されている業者からの電話のフィールドを一覧表示します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**図 7**: 一覧表示、FormView、`CompanyName`と`Phone`、現在選択されている業者からのフィールド ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>手順 3: 選択した仕入先、製品の一覧を表示する GridView の追加

FormView のテンプレートをすべての製品の中止 ボタンを追加できるようにする前に s はまず、選択した業者によって提供される製品を一覧表示するフォーム ビューの下に、GridView を追加します。 ページに GridView を追加するこれを実現するに次のように設定します。 その`ID`プロパティを`SuppliersProducts`、という名前の新しい ObjectDataSource を追加および`SuppliersProductsDataSource`します。


[![SuppliersProductsDataSource という名前の新しい ObjectDataSource を作成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**図 8**: 名前付き新しい ObjectDataSource 作成`SuppliersProductsDataSource`([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))。


構成 s ProductsBLL クラスを使用するには、この ObjectDataSource`GetProductsBySupplierID(supplierID)`メソッド (図 9 参照)。 アプリケーションはこの GridView が調整される製品の価格の許可が、編集または GridView から機能を削除する組み込みを使用しません。 そのため、設定できます (なし)、ドロップ ダウン リスト ObjectDataSource s の UPDATE、INSERT、および DELETE のタブ。


[![ProductsBLL クラス GetProductsBySupplierID(supplierID) メソッドを使用するデータ ソースの構成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**図 9**: を使用するデータ ソースの構成、`ProductsBLL`クラス s`GetProductsBySupplierID(supplierID)`メソッド ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))。


以降、`GetProductsBySupplierID(supplierID)`メソッドは入力パラメーターを受け取ります、ObjectDataSource ウィザードでこのパラメーターの値のソースが米国。 渡す、 `SupplierID` 、FormView からの値は、パラメーターのソースのドロップダウン リストをコントロールと ControlID のドロップダウン リストに設定`Suppliers`(手順 2. で作成された、フォーム ビューの ID)。


[![示すこと、supplierID パラメーターから取得するようにサプライヤー FormView コントロール](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**図 10**: いることを示す、 *`supplierID`* からパラメーターを取得するように、 `Suppliers` FormView コントロール ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))。


ObjectDataSource ウィザードの完了後は、GridView は各製品の s データ フィールドのも、BoundField または CheckBoxField に含まれます。 Let s スリムこれを表示するだけで`ProductName`と`UnitPrice`と共に BoundFields、 `Discontinued` CheckBoxField; さらに、秒の形式を使用、 `UnitPrice` BoundField、テキストが通貨として書式設定されるようします。 GridView と`SuppliersProductsDataSource`ObjectDataSource s の宣言型マークアップよう、次のマークアップになります。


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

この時点でこのチュートリアルでは、上部にある FormView から仕入先を選択し、下部にある GridView を通じてその業者によって提供される製品を表示するユーザーを許可する、マスター/詳細レポートが表示されます。 図 11 は、FormView から東京 Traders 仕入先を選択するときに、このページのスクリーン ショットを示します。


[![製品、仕入先の選択は GridView に表示されます。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**図 11**: GridView に、選択されている業者 s の製品が表示されます ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))。


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>手順 4: サプライヤーのすべての製品を中止するには、DAL と BLL メソッドを作成します。

FormView にボタンを追加するためをクリックすると、すべて中断の業者の製品では、まず必要があります、DAL とこのアクションを実行する BLL の両方にメソッドを追加します。 具体的には、このメソッドの名前は`DiscontinueAllProductsForSupplier(supplierID)`します。 FormView のボタンがクリックされたときを呼び出して、ビジネス ロジック層では、このメソッド渡す仕入先の選択した s で`SupplierID`; BLL をは、発行の対応するデータ アクセス層メソッドを呼び出し、`UPDATE`ステートメント指定された業者の製品を中断するデータベース。

前のチュートリアルで行ったように使用しますボトムアップ方式では、以降では、DAL メソッド、then、BLL メソッドを作成し、最後に、ASP.NET ページで、機能を実装します。 開く、`Northwind.xsd`で型指定されたデータセット、`App_Code/DAL`フォルダーに新しいメソッドを追加、 `ProductsTableAdapter` (を右クリックし、`ProductsTableAdapter`とクエリの追加 を選択)。 これは、私たちの新しいメソッドを追加するプロセスについて説明すると、TableAdapter クエリ構成ウィザードが表示されます。 DAL メソッドが、アドホック SQL ステートメントを使用することを示す開始します。


[![アドホック SQL ステートメントを使用して、DAL メソッドの作成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**図 12**: アドホック SQL ステートメントを使用して、DAL メソッドを作成 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))。


次に、私たちを作成するクエリの種類か求められます。 以降、`DiscontinueAllProductsForSupplier(supplierID)`メソッドは更新する必要があります、`Products`設定、データベース テーブル、 `Discontinued`  フィールドを指定したによって提供されるすべての製品の 1  *`supplierID`* データを更新するクエリを作成する必要があります。


[![更新クエリの種類を選択します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**図 13**: 更新クエリの種類を選択 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))。


次のウィザード画面は、既存の TableAdapter s`UPDATE`ステートメントでは、各で定義されているフィールドを更新する、 `Products` DataTable です。 このクエリのテキストを次のステートメントに置き換えます。


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

このクエリを入力し、最後のウィザード画面が新しいメソッドの名前を求める次へ をクリックすると、使用して`DiscontinueAllProductsForSupplier`します。 [完了] ボタンをクリックしてウィザードを完了します。 データセット デザイナーに戻るで新しいメソッドを参照する必要があります、`ProductsTableAdapter`という`DiscontinueAllProductsForSupplier(@SupplierID)`します。


[![名前の新しい DAL メソッド DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**図 14**: 新しい DAL メソッド名前`DiscontinueAllProductsForSupplier`([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))。


`DiscontinueAllProductsForSupplier(supplierID)`データ アクセス層で作成したメソッドを作成する、次のタスクでは、`DiscontinueAllProductsForSupplier(supplierID)`ビジネス ロジック層のメソッド。 これを行うには、開く、`ProductsBLL`クラス ファイルと、次を追加します。


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

下にこのメソッドを呼び出すだけです、`DiscontinueAllProductsForSupplier(supplierID)`メソッドに渡して、指定された、DAL *`supplierID`* パラメーターの値。 サプライヤーに特定の状況で中止された製品のみを許可するビジネス ルールがある場合はそれらのルールが BLL にここでは、実装する必要があります。

> [!NOTE]
> 異なり、`UpdateProduct`でオーバー ロード、`ProductsBLL`クラス、`DiscontinueAllProductsForSupplier(supplierID)`メソッドのシグネチャは含まれません、`DataObjectMethodAttribute`属性 (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`)。 そのため、`DiscontinueAllProductsForSupplier(supplierID)`更新プログラム タブでは、ObjectDataSource のデータ ソースの構成ウィザードのドロップダウン一覧からのメソッド。Ve は、私たちを呼び出すために、この属性を省略すると、 `DiscontinueAllProductsForSupplier(supplierID)` ASP.NET ページで、イベント ハンドラーから直接メソッド。


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>手順 5: 追加、中止、FormView にすべての製品ボタン

`DiscontinueAllProductsForSupplier(supplierID)` BLL と DAL でメソッドが完了、仕入先を選択したが FormView s に、ボタンの Web コントロールを追加するには、すべての製品を中止する機能を追加する最後の手順`ItemTemplate`します。 Let s ボタン テキストをすべての製品の中止 s 仕入先の電話番号の下にこのようなボタンを追加して、`ID`プロパティ値の`DiscontinueAllProductsForSupplier`します。 FormView s のスマート タグのテンプレートの編集リンクをクリックしてデザイナーには、このボタンの Web コントロールを追加することができます (図 15 を参照)、または宣言の構文を直接使用します。


[![追加、すべての製品ボタン Web コントロールを FormView の ItemTemplate を中止します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**図 15**: を中止するすべての製品ボタン コントロールを追加 Web FormView s `ItemTemplate` ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))。


ユーザーにアクセスし、ページのポストバックに陥ります、FormView 秒で、ボタンがクリックされたとき[`ItemCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx)が起動します。 カスタム コードを実行するには、このボタンがクリックされるへの応答には、このイベントのイベント ハンドラーを作成できます。 理解、ただし、`ItemCommand`イベントが発生したときに*任意*ボタン、LinkButton、ImageButton Web コントロールが、フォーム ビュー内でクリックします。 つまり、フォーム ビュー内の別、ユーザーが 1 つのページから移動したときに、`ItemCommand`イベントが発生します。 [新規]、[編集] をクリックするか、挿入、更新、または削除をサポートしているフォーム ビューの削除時に同じことです。

以降、`ItemCommand`イベント ハンドラー [すべての製品の中止] ボタンがクリックしてされたかどうかを判断する方法が必要、またはその他のいくつかのボタンがあった場合、どのようなボタンがクリックされたに関係なく発生します。 これを行うには、ボタンの Web コントロール s を設定できます`CommandName`プロパティをいくつかの識別値。 ボタンがクリックされたとき、この`CommandName`に値が渡される、`ItemCommand`イベント ハンドラーをすべての製品の中止 ボタンがクリックされたボタンをしたかどうかを判断できるようになりました。 設定はすべての製品ボタンの中止 s `CommandName` DiscontinueProducts するプロパティ。

最後に、s がクライアント側の確認 ダイアログ ボックスを使用して、ユーザーが、選択した供給業者の製品を中止するが本当のことを確認することができます。 説明したように、[削除時にクライアント側の確認を追加する](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md)チュートリアルでは、これは javascript で実現できます。 具体的には、ボタンの Web コントロールの s OnClientClick プロパティに設定します。 `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

これらの変更を行った後、次のよう FormView s の宣言型構文になります。


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

FormView s のイベント ハンドラーを次に、作成`ItemCommand`イベント。 このイベント ハンドラーでは、まずすべての製品の中止 ボタンがクリックしてされたかどうかを判断する必要があります。 インスタンスを作成するため場合、`ProductsBLL`クラスを呼び出すその`DiscontinueAllProductsForSupplier(supplierID)`に渡して、メソッド、`SupplierID`選択 FormView の。


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

なお、 `SupplierID` 、フォーム ビューで現在選択されている業者の使ってアクセスできる FormView s [ `SelectedValue`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)します。 `SelectedValue`プロパティは、最初のデータ キー、フォーム ビューに表示されているレコードの値を返します。 FormView s [ `DataKeyNames`プロパティ](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx)に自動的に設定されたフィールドの元のデータ キー値から取得したデータを示す`SupplierID`FormView 背面へ移動、ObjectDataSource をバインドするときに、Visual Studio によって手順 2。

`ItemCommand`イベント ハンドラーを作成するには、時間がかかるページをテストします。 Cooperativa の de Quesos を参照 ' Las Cabras' 業者 (、私にとって FormView で業者に 5 番目の s)。 この業者は、2 つの製品、Queso Cabrales と Queso Manchego La Pastora、どちらも*いない*廃止されました。

Cooperativa de Quesos ' Las Cabras' が倒産しているため、製品が中止されたはことを想像してください。 をクリックして、[すべての製品] ボタンを中止します。 クライアント側の確認ダイアログが表示されます (図 16 を参照してください)。


[![Cooperativa de Quesos Las Cabras 装置 2 つのアクティブな製品](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**図 16**: Cooperativa de Quesos Las Cabras 装置 2 つのアクティブな製品 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))。


クライアント側の確認 ダイアログ ボックスで ok をクリックすると、フォームの送信は続行でポストバックを発生させる FormView の`ItemCommand`イベントが発生します。 作成したイベント ハンドラーは実行を呼び出し、`DiscontinueAllProductsForSupplier(supplierID)`メソッドと Queso Cabrales と Queso Manchego La Pastora の両方の製品を廃止します。

GridView のビュー ステートを無効にした場合、GridView がポストバックのたびに、基になるデータ ストアにバインドされていると、そのため、これら 2 つの製品は、廃止されました (図 17 を参照してください) を反映するようにすぐに更新されます。 ただし、GridView での表示状態を無効するが場合は、この変更を行った後、GridView にデータを手動で再バインドする必要があります。 これを実現することを GridView s への呼び出しだけ`DataBind()`メソッドを呼び出した直後に、`DiscontinueAllProductsForSupplier(supplierID)`メソッド。


[![供給業者の製品がそれに応じて更新にはすべての製品の中止 ボタンをクリックした後](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**図 17**: 供給業者の製品がそれに応じて更新にはすべての製品の中止 ボタンをクリックした後 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))。


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>手順 6: 製品の価格を調整するためのビジネス ロジック層で UpdateProduct オーバー ロードを作成します。

ように、FormView で中止のすべての製品ボタンでの拡大と縮小、GridView で製品の価格のボタンを追加するには必要があります最初に適切なデータ アクセス層とビジネス ロジック層のメソッドを追加します。 新しいオーバー ロードを作成してこのような機能を提供しましたには、DAL で 1 つの製品の行を更新するメソッドが既にある、ため、 `UpdateProduct` BLL のメソッド。

当社の過去`UpdateProduct`スカラー入力値として製品のフィールドの組み合わせで取ったし、が、指定された製品のフィールドだけを更新し、オーバー ロードします。 このオーバー ロードをこの標準とは若干異なり、製品 s で渡す代わりに`ProductID`と調整に使用する割合、 `UnitPrice` (新しいを渡すことではなく調整`UnitPrice`自体)。 このアプローチには、ASP.NET ページの分離コード クラスで記述する必要のあるコードが簡略化、t が s の現在の製品を決定するで頭を悩ませることはありませんので`UnitPrice`します。

`UpdateProduct`オーバー ロードするため、このチュートリアルを次に示します。


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

このオーバー ロードは、DAL s を指定された製品に関する情報を取得`GetProductByProductID(productID)`メソッド。 これは、後で参照するためのチェックするかどうか製品 s`UnitPrice`データベースが割り当てられている`NULL`値。 価格のままの場合は、変更されていません。 ただし、ある以外の場合は、`NULL` `UnitPrice`値、メソッドの更新プログラムの製品 s`UnitPrice`指定 % (`unitPriceAdjustmentPercent`)。

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>手順 7: GridView の向上と低下ボタンの追加

GridView (および DetailsView) はどちらも構成されますのフィールドのコレクション。 BoundFields、CheckBoxFields、および TemplateFields、に加えては、ASP.NET には、その名前が示すようが行ごとには、ボタン、LinkButton、ImageButton に列として表示すると、ButtonField が含まれています。 クリックすると、フォーム ビューのような*任意*、GridView ページング ボタン、編集または削除ボタン、並べ替えボタン、および、内のボタンがポストバックを発生させ、GridView s [ `RowCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)します。

ButtonField が、`CommandName`指定した値を割り当て、ボタンの各プロパティ`CommandName`プロパティ。 FormView を使用するような`CommandName`によって値が使用される、`RowCommand`どのボタンがクリックされたかを判断するイベント ハンドラー。

Let s 価格 +10 のボタンのテキストを 1 つ、GridView に 2 つの新しい ButtonFields を追加する % および価格-10 テキストと、その他の % です。 追加するこれら ButtonFields GridView s のスマート タグからの列の編集リンクをクリックして、左上の一覧から ButtonField フィールドの種類を選択を追加 ボタンをクリックします。


![GridView に 2 つの ButtonFields を追加します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**図 18**: GridView を 2 つの ButtonFields を追加


GridView の最初の 2 つのフィールドとして表示されるように、2 つの ButtonFields を移動します。 次に、設定、`Text`プロパティ + 10% の料金計算にこれら 2 つの ButtonFields および価格-10%、 `CommandName` IncreasePrice および DecreasePrice、プロパティをそれぞれします。 既定を ButtonField は、Linkbutton としてその列のボタンをレンダリングします。 これは、ただし、ButtonField s [ `ButtonType`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)します。 S 標準プッシュ ボタンとして表示されるこれらの 2 つ ButtonFields があることそのため、設定、`ButtonType`プロパティを`Button`します。 図 19 フィールドが表示されます ダイアログ ボックス後、これらの変更が加えられました。その後は、GridView s の宣言型マークアップです。


![ButtonFields テキスト、CommandName、ButtonType プロパティを構成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**図 19**: 構成、ButtonFields `Text`、 `CommandName`、および`ButtonType`プロパティ


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

これら ButtonFields を作成すると、最後の手順は、GridView のイベント ハンドラーを作成する`RowCommand`イベント。 発生した場合、このイベント ハンドラー、価格は、+10% または価格-10% ボタンがクリックすると、確認する必要があります、`ProductID`のボタンがクリックしてされたを呼び出す行の`ProductsBLL`クラスの`UpdateProduct`を渡して、適切なメソッド`UnitPrice`と共に割合の調整、`ProductID`します。 次のコードでは、これらのタスクを実行します。

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

決定するために、`ProductID`行の価格が +10% または価格-10% ボタンがクリックされると、GridView s を参照する必要があります`DataKeys`コレクション。 このコレクションで指定されたフィールドの値を保持する、 `DataKeyNames` GridView 行ごとのプロパティ。 GridView s 以降`DataKeyNames`プロパティは、Visual Studio で ObjectDataSource を GridView にバインドする場合で、ProductID に設定された`DataKeys(rowIndex).Value`提供、 `ProductID` 、指定された*rowIndex*します。

ButtonField が自動的に渡す、 *rowIndex*を持つボタンがクリックしてされた行の`e.CommandArgument`パラメーター。 そのため、判断する、`ProductID`行の価格が +10% または価格-10% ボタンがクリックされると、使用して:`Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`します。

すべての製品を中止 ボタンを GridView s のビューステートを無効にした場合、GridView がポストバックのたびに、基になるデータ ストアにバインドされているとそのため、クリックしてから発生する価格変更を反映するようにすぐに更新されます。ボタンのいずれか。 ただし、GridView での表示状態を無効するが場合は、この変更を行った後、GridView にデータを手動で再バインドする必要があります。 これを実現することを GridView s への呼び出しだけ`DataBind()`メソッドを呼び出した直後に、`UpdateProduct`メソッド。

図 20 は、おばあちゃん Kelly の Homestead によって提供される製品を表示するときに、ページを示します。 図 21 は、% ボタンがクリックされた祖母の果汁 100% に分散し、価格 -10% ボタンを 2 回クリック 1 回のピリピリ価格 +10 後に結果を示します。


[![GridView には、価格 +10 が含まれています % と価格-10% ボタン。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**図 20**: GridView Includes 価格 + 10% と価格-10% ボタン ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))。


[![価格 +10 によって更新される最初と 3 番目の製品の価格と価格-10% ボタン](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**図 21**: 最初と 3 番目製品が更新された価格 +10 を使用して、価格 % と価格-10% ボタン ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))。


> [!NOTE]
> ボタン、Linkbutton、または ImageButtons、TemplateFields に追加の GridView や DetailsView) こともできます。 BoundField をクリックすると、これらのボタンは、ポストバックを誘発、としては、GridView s を発生させる`RowCommand`イベント。 ときに追加のボタンをクリックして TemplateField、ただし、ボタンの`CommandArgument`が自動的に設定されていない行のインデックスを ButtonFields を使用する場合は。 内でクリックしてされたボタンの行インデックスを確認する必要がある場合、`RowCommand`イベント ハンドラーで、[s] ボタンを手動で設定する必要があります`CommandArgument`のようなコードを使用して、TemplateField 内でその宣言構文内のプロパティ。  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`。


## <a name="summary"></a>まとめ

すべての GridView、DetailsView と FormView コントロールには、ボタン、Linkbutton、または ImageButtons を含めることができます。 このようなボタンをクリックすると、ポストバックを発生させて、 `ItemCommand` FormView および DetailsView コントロールでのイベントと`RowCommand`gridview イベント。 これらのデータ Web コントロールには、削除、またはレコードの編集など、一般的なコマンド関連の操作を処理する組み込みの機能があります。 ただし、私たちはも使用してボタンをクリックされたときに、独自のカスタム コードを実行することで応答します。

イベント ハンドラーを作成するためには、`ItemCommand`または`RowCommand`イベント。 このイベント ハンドラーで最初に確認受信`CommandName`値をどのボタンがクリックされたかを判断し、適切なカスタム アクションを実行します。 このチュートリアルでは、指定された業者のすべての製品を中止するまたは、10% で、特定の製品の価格を増減するボタンと ButtonFields を使用する方法を説明しました。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](adding-and-responding-to-buttons-to-a-gridview-cs.md)
