---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
title: 追加して、(c#) GridView にボタンに応答する |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、テンプレートと GridView、DetailsView コントロールのフィールドの両方にカスタム ボタンを追加する方法を紹介します。 具体的には、ビルド番号があるとしています.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 128fdb5f-4c5e-42b5-b485-f3aee90a8e38
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a081b1633e7762560aea68500f5bd614e4fb5a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833343"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-c"></a>追加して、(c#) GridView にボタンに応答するには
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_CS.exe)または[PDF のダウンロード](adding-and-responding-to-buttons-to-a-gridview-cs/_static/datatutorial28cs1.pdf)

> このチュートリアルでは、テンプレートと GridView、DetailsView コントロールのフィールドの両方にカスタム ボタンを追加する方法を紹介します。 具体的には、仕入先のページをユーザーに許可するフォーム ビューのあるインターフェイスをビルドします。


## <a name="introduction"></a>はじめに

多くのレポート シナリオでは、レポート データを読み取り専用のアクセスを伴う、中にレポートに表示されるデータに基づいてアクションを実行する機能を含めることも珍しくはありません。 レポートに表示される各レコードで、ボタン、LinkButton、ImageButton Web コントロールを追加する通常この関連するをクリックすると、ポストバックが発生して、いくつかのサーバー側コードを呼び出します。 レコード単位に基づいてデータ編集と削除は、最も一般的な例です。 実際には、以降で説明したように、[概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)チュートリアルでは、編集および削除は GridView、DetailsView、FormView コントロールがなく、このような機能をサポートできる一般的なので、1 行のコードを記述するため必要があります。

さらに編集およびボタン、GridView、DetailsView、およびフォーム ビューを削除するコントロールも、ボタン、Linkbutton、または ImageButtons をクリックすると、いくつかのカスタム サーバー側ロジックを実行します。 このチュートリアルでは、テンプレートと GridView、DetailsView コントロールのフィールドの両方にカスタム ボタンを追加する方法を紹介します。 具体的には、仕入先のページをユーザーに許可するフォーム ビューのあるインターフェイスをビルドします。 、特定のサプライヤー FormView に Button Web コントロールをクリックすると、はマーク設定すべてが関連付けられている製品の提供が中止されたと仕入先に関する情報が表示されます。 さらに、GridView が価格を増やすと、クリックすると、発生または軽減するため、製品の割引価格ボタンを含む行ごとに、選択したサプライヤーによって提供されるこれらの製品を一覧表示`UnitPrice`を 10% が (図 1 参照)。


[![FormView や GridView の両方がカスタム アクションを実行するボタンを含む](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image1.png)

**図 1**: 両方、FormView と GridView を含むボタンをカスタムの操作の実行 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image3.png))。


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>手順 1: ボタン チュートリアル Web ページの追加

カスタム ボタンを追加する方法を説明する前にまずみましょう。 このチュートリアルの必要がありますが、web サイト プロジェクトで ASP.NET ページを作成します。 という名前の新しいフォルダーを追加することで開始`CustomButtons`します。 次に、次の 2 つの ASP.NET ページを使用する各ページに関連付けるように、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `CustomButtons.aspx`


![カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image4.png)

**図 2**: カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加


などの他のフォルダーで`Default.aspx`で、`CustomButtons`フォルダーは、チュートリアルのセクションで一覧表示します。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`ページの デザイン ビューの ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image5.png)

**図 3**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image7.png))。


最後に、ページに追加するエントリとして、`Web.sitemap`ファイル。 具体的には、ページングと並べ替えの後に、次のマークアップを追加`<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample1.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューには、編集、挿入、および削除のチュートリアルの項目が含まれています。


![サイト マップが含まれています、エントリにはカスタム ボタンのチュートリアル](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image8.png)

**図 4**: サイト マップが含まれています、エントリにはカスタム ボタンのチュートリアル


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>手順 2: 仕入先の一覧を表示するフォーム ビューの追加

始めましょうこのチュートリアルを業者を一覧表示するフォーム ビューを追加することで。 概要で既に説明した、この FormView は GridView では業者によって提供される製品を示す、仕入先を使用してページにユーザーを許可します。 さらに、この FormView がボタンを含めてをクリックするはすべての業者の製品としてマーク廃止されました。 自分たちに関係します、FormView にカスタム ボタンを追加して、前に最初に作成しましょう、FormView 仕入先の情報を表示するようです。

開いて開始、`CustomButtons.aspx`ページで、`CustomButtons`フォルダー。 FormView をページに、デザイナーとセットには、ツールボックスからドラッグして追加の`ID`プロパティを`Suppliers`します。 という名前の新しい ObjectDataSource を作成することを選択、FormView のスマート タグから`SuppliersDataSource`します。


[![SuppliersDataSource という名前の新しい ObjectDataSource を作成します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image9.png)

**図 5**: 名前付き新しい ObjectDataSource 作成`SuppliersDataSource`([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image11.png))。


クエリを実行するように、この新しい ObjectDataSource を構成、`SuppliersBLL`クラスの`GetSuppliers()`メソッド (図 6 参照)。 以降、このフォーム ビューでは、供給業者は、選択、更新プログラム タブで、ドロップダウン リストからオプション (なし) を更新するためのインターフェイスは提供されません。


[![SuppliersBLL クラス GetSuppliers() メソッドを使用するデータ ソースの構成します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image12.png)

**図 6**: を使用するデータ ソースの構成、`SuppliersBLL`クラスの`GetSuppliers()`メソッド ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image14.png))。


ObjectDataSource を構成した後、Visual Studio が生成されます、 `InsertItemTemplate`、 `EditItemTemplate`、および`ItemTemplate`FormView の。 削除、`InsertItemTemplate`と`EditItemTemplate`および変更、`ItemTemplate`だけ仕入先の会社名、電話番号が表示されるようにします。 最後に、そのスマート タグからのページングを有効にするチェック ボックスをオン、FormView のページング サポートを有効にする (または、設定してその`AllowPaging`プロパティを`True`)。 これらの変更後、ページの宣言型マークアップを次のようになります。

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample2.aspx)]

図 7 では、ブラウザーで表示する際、CustomButtons.aspx ページを示します。


[![FormView、CompanyName と、現在選択されている業者からの電話のフィールドを一覧表示します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image15.png)

**図 7**: 一覧表示、FormView、`CompanyName`と`Phone`、現在選択されている業者からのフィールド ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-suppliers-products"></a>手順 3: 選択した業者の製品を一覧表示する GridView の追加

FormView のテンプレートをすべての製品の中止 ボタンを追加すると、前に、選択した業者によって提供される製品を一覧表示するフォーム ビューの下に、GridView を追加してみましょう最初。 ページに GridView を追加するこれを実現するに次のように設定します。 その`ID`プロパティを`SuppliersProducts`、という名前の新しい ObjectDataSource を追加および`SuppliersProductsDataSource`します。


[![SuppliersProductsDataSource という名前の新しい ObjectDataSource を作成します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image18.png)

**図 8**: 名前付き新しい ObjectDataSource 作成`SuppliersProductsDataSource`([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image20.png))。


この ObjectDataSource ProductsBLL クラスの使用を構成する`GetProductsBySupplierID(supplierID)`メソッド (図 9 参照)。 この GridView が調整される製品の価格の許可が、編集または GridView から機能を削除する、組み込みが使用ことはありません。 そのため、ObjectDataSource の UPDATE、INSERT、およびタブを削除する (なし) をドロップダウン リストを設定できます。


[![ProductsBLL クラス GetProductsBySupplierID(supplierID) メソッドを使用するデータ ソースの構成します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image21.png)

**図 9**: を使用するデータ ソースの構成、`ProductsBLL`クラスの`GetProductsBySupplierID(supplierID)`メソッド ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image23.png))。


以降、`GetProductsBySupplierID(supplierID)`メソッドは入力パラメーターを受け取ります、ObjectDataSource ウィザードでこのパラメーターの値のソースが米国。 渡す、 `SupplierID` 、FormView からの値は、パラメーターのソースのドロップダウン リストをコントロールと ControlID のドロップダウン リストに設定`Suppliers`(手順 2. で作成された、フォーム ビューの ID)。


[![示すこと、supplierID パラメーターから取得するようにサプライヤー FormView コントロール](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image24.png)

**図 10**: いることを示す、 *`supplierID`* からパラメーターを取得するように、 `Suppliers` FormView コントロール ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image26.png))。


ObjectDataSource ウィザードの完了後は、GridView は各製品のデータ フィールドのも、BoundField または CheckBoxField に含まれます。 表示してみましょうスリムするこれだけで`ProductName`と`UnitPrice`と共に BoundFields、 `Discontinued` CheckBoxField; さらに、書式設定してみましょう、 `UnitPrice` BoundField そのテキストが通貨として書式設定されるようします。 GridView と`SuppliersProductsDataSource`ObjectDataSource の宣言型マークアップよう、次のマークアップになります。

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample3.aspx)]

この時点でこのチュートリアルでは、上部にある FormView から仕入先を選択し、下部にある GridView を通じてその業者によって提供される製品を表示するユーザーを許可する、マスター/詳細レポートが表示されます。 図 11 は、FormView から東京 Traders 仕入先を選択するときに、このページのスクリーン ショットを示します。


[![製品、仕入先の選択は GridView に表示されます。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image27.png)

**図 11**: GridView に、選択されている業者の製品が表示されます ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image29.png))。


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>手順 4: サプライヤーのすべての製品を中止するには、DAL と BLL メソッドを作成します。

FormView にボタンを追加するためをクリックすると、すべて中断の業者の製品では、まず必要があります、DAL とこのアクションを実行する BLL の両方にメソッドを追加します。 具体的には、このメソッドの名前は`DiscontinueAllProductsForSupplier(supplierID)`します。 FormView のボタンがクリックされたときにがメソッドを呼び出すこの、ビジネス ロジック層で渡すことで選択されている業者の`SupplierID`; BLL をは、発行の対応するデータ アクセス層メソッドを呼び出し、`UPDATE`ステートメントデータベースには、指定された業者の製品が中断されます。

前のチュートリアルで行ったように使用しますボトムアップ方式では、以降では、DAL メソッド、then、BLL メソッドを作成し、最後に、ASP.NET ページで、機能を実装します。 開く、`Northwind.xsd`で型指定されたデータセット、`App_Code/DAL`フォルダーに新しいメソッドを追加、 `ProductsTableAdapter` (を右クリックし、`ProductsTableAdapter`とクエリの追加 を選択)。 これは、私たちの新しいメソッドを追加するプロセスについて説明すると、TableAdapter クエリ構成ウィザードが表示されます。 DAL メソッドが、アドホック SQL ステートメントを使用することを示す開始します。


[![アドホック SQL ステートメントを使用して、DAL メソッドの作成します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image30.png)

**図 12**: アドホック SQL ステートメントを使用して、DAL メソッドを作成 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image32.png))。


次に、私たちを作成するクエリの種類か求められます。 以降、`DiscontinueAllProductsForSupplier(supplierID)`メソッドは更新する必要があります、`Products`設定、データベース テーブル、 `Discontinued`  フィールドを指定したによって提供されるすべての製品の 1  *`supplierID`* データを更新するクエリを作成する必要があります。


[![更新クエリの種類を選択します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image33.png)

**図 13**: 更新クエリの種類を選択 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image35.png))。


次のウィザード画面は、TableAdapter の既存の`UPDATE`ステートメントでは、各で定義されているフィールドを更新する、 `Products` DataTable です。 このクエリのテキストを次のステートメントに置き換えます。

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample4.sql)]

このクエリを入力し、[次へ] をクリックすると、最後ウィザード画面では、新しいメソッドの名前を使用`DiscontinueAllProductsForSupplier`します。 [完了] ボタンをクリックしてウィザードを完了します。 データセット デザイナーに戻るで新しいメソッドを参照する必要があります、`ProductsTableAdapter`という`DiscontinueAllProductsForSupplier(@SupplierID)`します。


[![名前の新しい DAL メソッド DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image36.png)

**図 14**: 新しい DAL メソッド名前`DiscontinueAllProductsForSupplier`([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image38.png))。


`DiscontinueAllProductsForSupplier(supplierID)`データ アクセス層で作成したメソッドを作成する、次のタスクでは、`DiscontinueAllProductsForSupplier(supplierID)`ビジネス ロジック層のメソッド。 これを行うには、開く、`ProductsBLL`クラス ファイルと、次を追加します。

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample5.cs)]

下にこのメソッドを呼び出すだけです、`DiscontinueAllProductsForSupplier(supplierID)`メソッドに渡して、指定された、DAL *`supplierID`* パラメーターの値。 特定の状況で中止された業者の製品のみを許可するビジネス ルールがある場合はそれらのルールが BLL にここでは、実装する必要があります。

> [!NOTE]
> 異なり、`UpdateProduct`でオーバー ロード、`ProductsBLL`クラス、`DiscontinueAllProductsForSupplier(supplierID)`メソッドのシグネチャは含まれません、`DataObjectMethodAttribute`属性 (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`)。 そのため、`DiscontinueAllProductsForSupplier(supplierID)`更新プログラム タブで ObjectDataSource のデータ ソースの構成ウィザードのドロップダウン リストからメソッド。Ve は、私たちを呼び出すために、この属性を省略すると、 `DiscontinueAllProductsForSupplier(supplierID)` ASP.NET ページで、イベント ハンドラーから直接メソッド。


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>手順 5: 追加、中止、FormView にすべての製品ボタン

`DiscontinueAllProductsForSupplier(supplierID)` FormView のボタンの Web コントロールを追加するメソッド、BLL と完全な DAL で、選択した仕入先のすべての製品を中止する機能を追加する最後の手順は`ItemTemplate`します。 このようなボタンは、以下のボタンのテキスト、中止のすべての製品と仕入先の電話番号を追加してみましょうと`ID`プロパティ値の`DiscontinueAllProductsForSupplier`します。 FormView のスマート タグでテンプレートの編集リンクをクリックして、デザイナーを使用には、このボタンの Web コントロールを追加することができます (図 15 を参照)、または宣言の構文を直接使用します。


[![追加、すべての製品ボタン Web コントロールを FormView の ItemTemplate を中止します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image39.png)

**図 15**: を中止するすべての製品ボタン Web コントロールを追加 FormView の`ItemTemplate`([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image41.png))。


ユーザーにアクセスし、ページのポストバックに陥りますして FormView のボタンがクリックされたとき[`ItemCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx)が起動します。 カスタム コードを実行するには、このボタンがクリックされるへの応答には、このイベントのイベント ハンドラーを作成できます。 理解、ただし、`ItemCommand`イベントが発生したときに*任意*ボタン、LinkButton、ImageButton Web コントロールが、フォーム ビュー内でクリックします。 つまり、フォーム ビュー内の別、ユーザーが 1 つのページから移動したときに、`ItemCommand`イベントが発生します。 [新規]、[編集] をクリックするか、挿入、更新、または削除をサポートしているフォーム ビューの削除時に同じことです。

以降、`ItemCommand`イベント ハンドラー [すべての製品の中止] ボタンがクリックしてされたかどうかを判断する方法が必要、またはその他のいくつかのボタンがあった場合、どのようなボタンがクリックされたに関係なく発生します。 これを行うには、ボタンの Web コントロールを設定できます`CommandName`プロパティをいくつかの識別値。 ボタンがクリックされたとき、この`CommandName`に値が渡される、`ItemCommand`イベント ハンドラーをすべての製品の中止 ボタンがクリックされたボタンをしたかどうかを判断できるようになりました。 設定を中止するすべての製品ボタンの`CommandName`DiscontinueProducts するプロパティ。

最後に、こと、ユーザーが選択されている業者の製品を中止するのに本当のことを確認するのにみましょうクライアント側の確認 ダイアログ ボックスを使用します。 説明したように、[削除時にクライアント側の確認を追加する](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs.md)チュートリアルでは、これは javascript で実現できます。 具体的には、ボタンの Web コントロールの OnClientClick プロパティに設定します。 `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

これらの変更を行った後、次のようフォーム ビューの宣言型構文になります。

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample6.aspx)]

FormView のイベント ハンドラーを次に、作成`ItemCommand`イベント。 このイベント ハンドラーでは、まずすべての製品の中止 ボタンがクリックしてされたかどうかを判断する必要があります。 インスタンスを作成するため場合、`ProductsBLL`クラスを呼び出すその`DiscontinueAllProductsForSupplier(supplierID)`に渡して、メソッド、`SupplierID`選択 FormView の。

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample7.cs)]

なお、 `SupplierID` 、フォーム ビューで現在選択されている業者の使ってアクセスできる FormView の[`SelectedValue`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)します。 `SelectedValue`プロパティは、最初のデータ キー、フォーム ビューに表示されているレコードの値を返します。 FormView の[`DataKeyNames`プロパティ](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx)に自動的に設定されたフィールドの元のデータ キー値から取得したデータを示す`SupplierID`ObjectDataSource をフォーム ビューにバインドするときに、Visual Studio によって手順 2。

`ItemCommand`イベント ハンドラーを作成するには、時間がかかるページをテストします。 Cooperativa の de Quesos を参照 ' Las Cabras' 業者が (これは私にとって FormView で業者に 5 番目です)。 この業者は、2 つの製品、Queso Cabrales と Queso Manchego La Pastora、どちらも*いない*廃止されました。

Cooperativa de Quesos ' Las Cabras' が倒産しているため、製品が中止されたはことを想像してください。 をクリックして、[すべての製品] ボタンを中止します。 クライアント側の確認ダイアログが表示されます (図 16 を参照してください)。


[![Cooperativa de Quesos Las Cabras 装置 2 つのアクティブな製品](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image42.png)

**図 16**: Cooperativa de Quesos Las Cabras 装置 2 つのアクティブな製品 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image44.png))。


クライアント側の確認 ダイアログ ボックスで ok をクリックすると、フォームの送信は続行でポストバックを発生させる FormView の`ItemCommand`イベントが発生します。 作成したイベント ハンドラーは実行を呼び出し、`DiscontinueAllProductsForSupplier(supplierID)`メソッドと Queso Cabrales と Queso Manchego La Pastora の両方の製品を廃止します。

GridView のビュー ステートを無効にした場合、GridView がポストバックのたびに、基になるデータ ストアにバインドされていると、そのため、これら 2 つの製品は、廃止されました (図 17 を参照してください) を反映するようにすぐに更新されます。 ただし、GridView での表示状態を無効するが場合は、この変更を行った後、GridView にデータを手動で再バインドする必要があります。 これを実現する GridView の呼び出しを行うだけ`DataBind()`メソッドを呼び出した直後に、`DiscontinueAllProductsForSupplier(supplierID)`メソッド。


[![供給業者の製品がそれに応じて更新にはすべての製品の中止 ボタンをクリックした後](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image45.png)

**図 17**: すべての製品の中止 ボタンをクリックすると後、業者の製品には、それに応じて更新 ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image47.png))。


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-products-price"></a>手順 6: 製品の価格を調整するためのビジネス ロジック層で UpdateProduct オーバー ロードを作成します。

ように、FormView で中止のすべての製品ボタンでの拡大と縮小、GridView で製品の価格のボタンを追加するには必要があります最初に適切なデータ アクセス層とビジネス ロジック層のメソッドを追加します。 新しいオーバー ロードを作成してこのような機能を提供しましたには、DAL で 1 つの製品の行を更新するメソッドが既にある、ため、 `UpdateProduct` BLL のメソッド。

当社の過去`UpdateProduct`スカラー入力値として製品のフィールドの組み合わせで取ったし、が、指定された製品のフィールドだけを更新し、オーバー ロードします。 このオーバー ロードをこの標準とは若干異なり、製品の代わりに渡す`ProductID`と調整に使用する割合、 `UnitPrice` (新しいを渡すことではなく調整`UnitPrice`自体)。 この方法で、現在の製品を決定する頭を悩ませるがあるないために、ASP.NET ページの分離コード クラスで記述する必要のあるコードが簡略化されます`UnitPrice`します。

`UpdateProduct`オーバー ロードするため、このチュートリアルを次に示します。

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample8.cs)]

このオーバー ロードは、DAL のを通じて指定された製品に関する情報を取得`GetProductByProductID(productID)`メソッド。 これは、後で参照するためのチェックするかどうか、製品の`UnitPrice`データベースが割り当てられている`NULL`値。 価格のままの場合は、変更されていません。 ただし、ある以外の場合は、`NULL` `UnitPrice`値、メソッドの更新プログラムの製品の`UnitPrice`指定 % (`unitPriceAdjustmentPercent`)。

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>手順 7: GridView の向上と低下ボタンの追加

GridView (および DetailsView) はどちらも構成されますのフィールドのコレクション。 BoundFields、CheckBoxFields、および TemplateFields、に加えては、ASP.NET には、その名前が示すようが行ごとには、ボタン、LinkButton、ImageButton に列として表示すると、ButtonField が含まれています。 クリックすると、フォーム ビューのような*任意*、GridView ページング ボタン、編集または削除ボタン、並べ替えボタン、および、内のボタンがポストバックを発生させ、GridView の[`RowCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)します。

ButtonField が、`CommandName`指定した値を割り当て、ボタンの各プロパティ`CommandName`プロパティ。 FormView を使用するような`CommandName`によって値が使用される、`RowCommand`どのボタンがクリックされたかを判断するイベント ハンドラー。

ボタン テキスト価格 +10 のいずれかと、GridView を追加して、2 つの新しい ButtonFields % および価格-10 テキストと、その他の % です。 追加するこれら ButtonFields GridView のスマート タグからの列の編集リンクをクリックして、左上の一覧から ButtonField フィールドの種類を選択を追加 ボタンをクリックします。


![GridView に 2 つの ButtonFields を追加します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image48.png)

**図 18**: GridView を 2 つの ButtonFields を追加


GridView の最初の 2 つのフィールドとして表示されるように、2 つの ButtonFields を移動します。 次に、設定、`Text`プロパティ + 10% の料金計算にこれら 2 つの ButtonFields および価格-10%、 `CommandName` IncreasePrice および DecreasePrice、プロパティをそれぞれします。 既定を ButtonField は、Linkbutton としてその列のボタンをレンダリングします。 これは、ただし、ButtonField の[`ButtonType`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)します。 これら 2 つの ButtonFields 標準プッシュ ボタンとしてレンダリングをあてしましょうそのため、設定、`ButtonType`プロパティを`Button`します。 図 19 フィールドが表示されます ダイアログ ボックス後、これらの変更が加えられました。その後は、GridView の宣言型マークアップです。


![ButtonFields テキスト、CommandName、ButtonType プロパティを構成します。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image49.png)

**図 19**: 構成、ButtonFields `Text`、 `CommandName`、および`ButtonType`プロパティ


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample9.aspx)]

これら ButtonFields を作成すると、最後の手順は、GridView のイベント ハンドラーを作成する`RowCommand`イベント。 発生した場合、このイベント ハンドラー、価格は、+10% または価格-10% ボタンがクリックすると、確認する必要があります、`ProductID`のボタンがクリックしてされたを呼び出す行の`ProductsBLL`クラスの`UpdateProduct`を渡して、適切なメソッド`UnitPrice`と共に割合の調整、`ProductID`します。 次のコードでは、これらのタスクを実行します。

[!code-csharp[Main](adding-and-responding-to-buttons-to-a-gridview-cs/samples/sample10.cs)]

決定するために、`ProductID`行の価格が +10% または価格-10% ボタンがクリックされると、GridView を参照する必要があります`DataKeys`コレクション。 このコレクションで指定されたフィールドの値を保持する、 `DataKeyNames` GridView 行ごとのプロパティ。 GridView の以降`DataKeyNames`プロパティは、Visual Studio で ObjectDataSource を GridView にバインドする場合で、ProductID に設定された`DataKeys(rowIndex).Value`提供、 `ProductID` 、指定された*rowIndex*します。

ButtonField が自動的に渡す、 *rowIndex*を持つボタンがクリックしてされた行の`e.CommandArgument`パラメーター。 そのため、判断する、`ProductID`行の価格が +10% または価格-10% ボタンがクリックされると、使用して:`Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`します。

すべての製品を中止 ボタンを GridView のビュー ステートを無効にした場合、GridView がポストバックのたびに、基になるデータ ストアにバインドされているとそのため、クリックしてから発生する価格変更を反映するようにすぐに更新されます。ボタンのいずれか。 ただし、GridView での表示状態を無効するが場合は、この変更を行った後、GridView にデータを手動で再バインドする必要があります。 これを実現する GridView の呼び出しを行うだけ`DataBind()`メソッドを呼び出した直後に、`UpdateProduct`メソッド。

図 20 は、おばあちゃん Kelly の Homestead によって提供される製品を表示するときに、ページを示します。 図 21 は、% ボタンがクリックされた祖母の果汁 100% に分散し、価格 -10% ボタンを 2 回クリック 1 回のピリピリ価格 +10 後に結果を示します。


[![GridView には、価格 +10 が含まれています % と価格-10% ボタン。](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image50.png)

**図 20**: GridView Includes 価格 + 10% と価格-10% ボタン ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image52.png))。


[![価格 +10 によって更新される最初と 3 番目の製品の価格と価格-10% ボタン](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image53.png)

**図 21**: 最初と 3 番目製品が更新された価格 +10 を使用して、価格 % と価格-10% ボタン ([フルサイズの画像を表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-cs/_static/image55.png))。


> [!NOTE]
> ボタン、Linkbutton、または ImageButtons、TemplateFields に追加の GridView や DetailsView) こともできます。 GridView の発生、BoundField をクリックすると、これらのボタンは、ポストバックを誘発、`RowCommand`イベント。 ときに追加のボタンをクリックして TemplateField、ただし、ボタンの`CommandArgument`が自動的に設定されていない行のインデックスを ButtonFields を使用する場合があるためです。 内でクリックしてされたボタンの行インデックスを確認する必要がある場合、`RowCommand`イベント ハンドラーでボタンを手動で設定する必要があります`CommandArgument`のようなコードを使用して、TemplateField 内でその宣言構文内のプロパティ。  
> `<asp:Button runat="server" ... CommandArgument='<%# ((GridViewRow) Container).RowIndex %>'`。


## <a name="summary"></a>まとめ

すべての GridView、DetailsView と FormView コントロールには、ボタン、Linkbutton、または ImageButtons を含めることができます。 このようなボタンをクリックすると、ポストバックを発生させて、 `ItemCommand` FormView および DetailsView コントロールでのイベントと`RowCommand`gridview イベント。 これらのデータ Web コントロールには、削除、またはレコードの編集など、一般的なコマンド関連の操作を処理する組み込みの機能があります。 ただし、私たちはも使用してボタンをクリックされたときに、独自のカスタム コードを実行することで応答します。

イベント ハンドラーを作成するためには、`ItemCommand`または`RowCommand`イベント。 このイベント ハンドラーで最初に確認受信`CommandName`値をどのボタンがクリックされたかを判断し、適切なカスタム アクションを実行します。 このチュートリアルでは、指定された業者のすべての製品を中止するまたは、10% で、特定の製品の価格を増減するボタンと ButtonFields を使用する方法を説明しました。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [次へ](adding-and-responding-to-buttons-to-a-gridview-vb.md)
