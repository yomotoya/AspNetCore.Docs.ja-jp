---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: 追加して、GridView (VB) をボタンに応答して |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、テンプレートと GridView または DetailsView コントロールのフィールドの両方にカスタム ボタンを追加する方法を紹介します。 具体的には、bui をします.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 58b570c897810eeaa182a201616a182c02e9d92c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878101"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>GridView (VB) をボタンに応答して追加します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe)または[PDF のダウンロード](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> このチュートリアルでは、テンプレートと GridView または DetailsView コントロールのフィールドの両方にカスタム ボタンを追加する方法を紹介します。 具体的には、仕入先のページをユーザーができるフォーム ビューのあるインターフェイスを構築します。


## <a name="introduction"></a>はじめに

レポートの多くのシナリオには、レポート データを読み取り専用のアクセスが含まれます。 中に、s ことは、操作を実行する機能を含めるレポートが表示されるデータに基づきます。 ボタン、LinkButton、ImageButton Web コントロールを追加する、レポートに表示される各レコードに関係しているこの通常、クリックすると、ポストバックを発生させるし、いくつかのサーバー側コードを呼び出します。 編集し、レコードごとにデータを削除すると、最も一般的な例です。 実際には、以降で説明したように、[の概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルでは、編集および削除は、GridView、DetailsView、およびフォーム ビュー コントロールがなくこのような機能をサポートできることもよく、1 行のコードを記述するため必要があります。

さらに編集およびボタン、GridView、DetailsView、およびフォーム ビューを削除するコントロールも、ボタン、ある、または ImageButtons をクリックすると、なんらかのカスタム サーバー側ロジックを実行します。 このチュートリアルでは、テンプレートと GridView または DetailsView コントロールのフィールドの両方にカスタム ボタンを追加する方法を紹介します。 具体的には、仕入先のページをユーザーができるフォーム ビューのあるインターフェイスを構築します。 特定の業者の FormView にボタン Web コントロールをクリックするはマーク設定すべて関連付けられている製品の提供が中止されたと共に仕入先に関する情報が表示されます。 GridView に増やす価格と割引価格ボタンをクリックすると、発生させるか製品 s を減らすを含む行ごとに、選択した業者によって提供されるこれらの製品が一覧表示さらに、`UnitPrice`を 10% (図 1 を参照してください)。


[![フォーム ビューと GridView の両方がカスタム アクションを実行するボタンを含む](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**図 1**: 両方 FormView および GridView を含むボタンをカスタムの操作の実行 ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>手順 1: ボタンのチュートリアル Web ページの追加

カスタム ボタンを追加する方法を見ると、前に、まず、このチュートリアルに必要とする、web サイト プロジェクトで ASP.NET ページを作成するのにはしばらく s を使用できます。 という名前の新しいフォルダーを追加することによって開始`CustomButtons`です。 次に、各ページに関連付けるように、そのフォルダーに次の 2 つの ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `CustomButtons.aspx`


![カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**図 2**: カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加します。


などの他のフォルダーで`Default.aspx`で、`CustomButtons`フォルダーは、チュートリアルのセクションに一覧表示します。 注意してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**図 3**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


最後に、エントリとして、ページを追加、`Web.sitemap`ファイル。 具体的には、ページングと並べ替えの後に、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、編集、挿入、およびチュートリアルを削除する項目が含まれています。


![サイト マップではカスタム ボタンのチュートリアル」のエントリが含まれるようになりました](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**図 4**: サイト マップではカスタム ボタンのチュートリアル」のエントリが含まれるようになりました


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>手順 2: 仕入先の一覧を表示するフォーム ビューの追加

S 仕入先の一覧を表示するフォーム ビューを追加することでこのチュートリアルを始めることができます。 導入で既に説明した、この FormView は GridView で業者によって提供される製品を示す、仕入先からのページにユーザーを許可します。 さらに、このフォーム ビューは、ボタンをクリックするはすべてのサプライヤーの製品としてマーク提供が中止されました。 自分に関係お FormView へのカスタム ボタンの追加と、前にまずを作成、FormView 仕入先の情報を表示するよう s を使用できます。

開いて開始、 `CustomButtons.aspx`  ページで、`CustomButtons`フォルダーです。 デザイナーとセットには、ツールボックスからドラッグして、FormView をページに追加の`ID`プロパティを`Suppliers`です。 という名前の新しい ObjectDataSource を作成することを選択 FormView s のスマート タグから`SuppliersDataSource`です。


[![SuppliersDataSource をという名前の新しい ObjectDataSource を作成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**図 5**: 名前付き新しい ObjectDataSource 作成`SuppliersDataSource`([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


クエリを実行するように、この新しい ObjectDataSource を構成、`SuppliersBLL`クラスの`GetSuppliers()`メソッド (図 6 を参照してください)。 以降、このフォーム ビューでは、仕入先については、選択更新 タブで、ドロップダウン リストからオプション (なし) を更新するためのインターフェイスは提供されません。


[![SuppliersBLL クラスの GetSuppliers() メソッドを使用するデータ ソースを構成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**図 6**: を使用するデータ ソースを構成、`SuppliersBLL`クラス s`GetSuppliers()`メソッド ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


Visual Studio は生成、ObjectDataSource を構成した後、 `InsertItemTemplate`、 `EditItemTemplate`、および`ItemTemplate`FormView の。 削除、`InsertItemTemplate`と`EditItemTemplate`および変更、`ItemTemplate`だけ、供給業者の会社名と電話番号を表示するようです。 スマート タグからのページングを有効にするチェック ボックスをオンして、FormView のページング サポートを有効に最後に、(かを設定してその`AllowPaging`プロパティを`True`)。 これらの変更後にページ s の宣言型マークアップを次のようになります。


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

図 7 は、ブラウザーで表示したときに、CustomButtons.aspx ページを示します。


[![FormView は、CompanyName と現在選択されている、業者からの電話のフィールドを一覧表示します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**図 7**: フォーム ビューを一覧表示、`CompanyName`と`Phone`、現在選択されている業者からのフィールド ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>手順 3: 選択したサプライヤーの製品を一覧表示する GridView の追加

FormView のテンプレートにすべての製品の中止 ボタンを追加できるようにする前に s は最初、選択した業者によって提供される製品を一覧表示するフォーム ビューの下にある GridView を追加します。 これには、GridView ページを追加するには設定、`ID`プロパティを`SuppliersProducts`、という名前の新しい ObjectDataSource を追加および`SuppliersProductsDataSource`です。


[![SuppliersProductsDataSource をという名前の新しい ObjectDataSource を作成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**図 8**: 名前付き新しい ObjectDataSource 作成`SuppliersProductsDataSource`([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


構成 ProductsBLL クラス s を使用するには、この ObjectDataSource`GetProductsBySupplierID(supplierID)`メソッド (図 9 を参照してください)。 何もこの GridView は、調整する製品の価格の許可は、編集または GridView から機能を削除する、組み込みを使用しません。 そのため、設定できます (なし) にドロップ ダウン リスト ObjectDataSource s の UPDATE、INSERT、および DELETE のタブ。


[![ProductsBLL クラスの GetProductsBySupplierID(supplierID) メソッドを使用するデータ ソースを構成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**図 9**: を使用するデータ ソースを構成、`ProductsBLL`クラス s`GetProductsBySupplierID(supplierID)`メソッド ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


以降、`GetProductsBySupplierID(supplierID)`メソッドは、入力パラメーターを受け取り、ObjectDataSource ウィザードの指示に従って us このパラメーター値のソース。 渡す、 `SupplierID` FormView からの値、コントロールと ControlID のドロップダウン リストにパラメーターのソースのドロップダウン リストを設定`Suppliers`(FormView の ID は、手順 2. で作成) します。


[![SupplierID パラメーターから取得するように Suppliers FormView コントロールを示します](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**図 10**: ことを示すため、 *`supplierID`* からパラメーターを取得する、 `Suppliers` FormView コントロール ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


ObjectDataSource ウィザードの完了後は、GridView はの各製品のデータ フィールドのも、BoundField または CheckBoxField に含まれます。 Let s 増減これを表示するだけ、`ProductName`と`UnitPrice`と共に BoundFields、 `Discontinued` CheckBoxField; s 形式をさらに、使用できます、 `UnitPrice` BoundField そのテキストが通貨として書式設定されるようです。 GridView と`SuppliersProductsDataSource`ObjectDataSource s の宣言型マークアップは、次のマークアップのようになります。


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

この時点で、チュートリアルでは、上部にある FormView から仕入先を選択して、下部にある GridView を通じてその業者によって提供される製品を表示するユーザーを許可する、マスター/詳細レポートが表示されます。 図 11 は、FormView から東京 Traders 仕入先を選択するときに、このページのスクリーン ショットを示します。


[![GridView に、仕入先の選択した製品が表示されます。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**図 11**: GridView で、選択した業者製品が表示されます ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>手順 4: 作成、業者のすべての製品を中止するには、DAL と BLL メソッド

FormView にボタンを追加するをクリックすると、すべてを停止供給業者の製品の最初いただくために DAL とこのアクションを実行する BLL の両方にメソッドを追加します。 具体的には、このメソッドの名前は`DiscontinueAllProductsForSupplier(supplierID)`します。 FormView のボタンがクリックされたときにおがメソッドを呼び出すこのビジネス ロジック層に渡す仕入先の選択した s `SupplierID`; BLL をを発行するの対応するデータ アクセス層メソッドを呼び出し、`UPDATE`ステートメント指定された業者の製品を中断するデータベース。

ように実行して、前のチュートリアルを使用しますボトムアップ方式では、DAL メソッド、then BLL メソッドを作成し、最後に、機能を実装する ASP.NET ページで始まります。 開く、`Northwind.xsd`に型指定されたデータセット、`App_Code/DAL`フォルダー新しいメソッドを追加して、 `ProductsTableAdapter` (を右クリックし、`ProductsTableAdapter`クエリの追加を選択) します。 これにより、新しいメソッドを追加する手順を追って us TableAdapter クエリの構成ウィザードが表示されます。 DAL メソッドが、アドホック SQL ステートメントを使用することを示す開始します。


[![アドホック SQL ステートメントを使用して、DAL メソッドを作成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**図 12**: アドホック SQL ステートメントを使用して、DAL メソッドを作成 ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


次に、ウィザードの指示に従ってに関してどのようなクエリの種類を作成することです。 以降、`DiscontinueAllProductsForSupplier(supplierID)`メソッドは更新する必要があります、`Products`設定、データベース テーブル、 `Discontinued`  フィールドを指定したによって提供されるすべての製品の 1  *`supplierID`* データを更新するクエリを作成する必要があります。


[![更新クエリ タイプを選択します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**図 13**: 更新クエリの種類を選択 ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


次のウィザード画面は、既存の TableAdapter s`UPDATE`ステートメントでは、それぞれで定義されたフィールドを更新する、 `Products` DataTable です。 このクエリのテキストを次のステートメントに置き換えます。


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

このクエリを入力し、新しいメソッドの名前を要求する最後のウィザードの画面、[次へ] をクリックした後を使用して`DiscontinueAllProductsForSupplier`です。 [完了] ボタンをクリックして、ウィザードを完了します。 データセット デザイナーに戻ったときに表示されるはずで新しいメソッド、`ProductsTableAdapter`という`DiscontinueAllProductsForSupplier(@SupplierID)`です。


[![名前を新しい DAL メソッド DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**図 14**: 新しい DAL メソッド名前`DiscontinueAllProductsForSupplier`([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


`DiscontinueAllProductsForSupplier(supplierID)`を作成する、データ アクセス レイヤーで作成したメソッドを次のタスクは、`DiscontinueAllProductsForSupplier(supplierID)`ビジネス ロジック層内のメソッドです。 これを実現する、開く、`ProductsBLL`クラス ファイルと、次を追加します。


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

下にこのメソッドを呼び出すだけ、`DiscontinueAllProductsForSupplier(supplierID)`メソッドに渡す提供されている、DAL *`supplierID`* パラメーターの値。 サプライヤーに特定の状況で中止された製品のみを許可する任意のビジネス ルールがある場合、それらのルールに実装して、ここでは、BLL です。

> [!NOTE]
> 異なり、`UpdateProduct`でオーバー ロード、`ProductsBLL`クラス、`DiscontinueAllProductsForSupplier(supplierID)`メソッド シグネチャを含まない、`DataObjectMethodAttribute`属性 (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`)。 そのため、 `DiscontinueAllProductsForSupplier(supplierID)` ObjectDataSource s データ ソースの構成ウィザードの s - ドロップダウン リストから、[更新] タブでのメソッドです。I ve おを呼び出すためにのこの属性を省略すると、 `DiscontinueAllProductsForSupplier(supplierID)` ASP.NET ページ内のイベント ハンドラーから直接メソッドです。


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>手順 5: 追加、FormView にすべての製品ボタンを中止します。

`DiscontinueAllProductsForSupplier(supplierID)` BLL および DAL でメソッドが完了して、最後の手順を選択した供給業者が Button Web コントロールをフォーム ビュー s に追加するのには、すべての製品を中止する機能を追加する`ItemTemplate`です。 Let s ボタン テキストをすべての製品の中止 s 仕入先電話番号の下にこのようなボタンを追加して、`ID`のプロパティの値`DiscontinueAllProductsForSupplier`です。 FormView s のスマート タグのテンプレートの編集リンクをクリックして、デザイナーでは、このボタン Web コントロールを追加できます (図 15 を参照)、または宣言の構文を直接使用します。


[![追加、FormView の ItemTemplate にすべての製品ボタン Web コントロールを中止します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**図 15**: を中止するすべての製品ボタン コントロールを追加 Web FormView s `ItemTemplate` ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


ページ ポストバックに陥りますユーザーにアクセスして、FormView のボタンがクリックされたときに[`ItemCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx)発生します。 カスタム コードを実行するには、このボタンをクリックしてに応答するには、ことができます、このイベントのイベント ハンドラーを作成します。 点に注意して、ただし、`ItemCommand`イベントが発生するたびに*任意*FormView でボタン、LinkButton、ImageButton Web コントロールをクリックします。 つまり、ユーザーが 1 つのページ、フォーム ビュー内の別に移動すると、`ItemCommand`イベントの起動以外の同じユーザーが、編集、新規をクリックするか、挿入、更新、または削除をサポートするフォーム ビューを削除します。

以降、`ItemCommand`イベント ハンドラーお必要があるすべての製品の中止 ボタンがクリックしてされたかどうかを決定する方法、またはその他のいくつかのボタンが、どのようなボタンがクリックされたに関係なく発生します。 これを行うには、ボタン Web コントロール s を設定できます`CommandName`プロパティを識別するいくつかの値にします。 ときに、ボタンがクリックされた、この`CommandName`に値が渡された、`ItemCommand`すべての製品の中止 ボタンがクリックされたボタンをしたかどうかを判断することを有効にすると、イベント ハンドラー。 すべての製品ボタンの中止 s を設定`CommandName`DiscontinueProducts するプロパティです。

最後に、ユーザーが、選択した仕入先の商品を中止するが本当にことを確認するクライアント側の確認 ダイアログ ボックスを使用して $s を使用できます。 示したように、[削除時にクライアント側の確認を追加する](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md)チュートリアルでは、これは JavaScript のビットを使用して実行できます。 具体的には、ボタンの Web コントロールの OnClientClick プロパティに設定します。 `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

これらの変更を加えたら、FormView s の宣言構文は次のようになります。


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

FormView s のイベント ハンドラーを次に、作成`ItemCommand`イベント。 このイベント ハンドラーでは、最初にすべての製品の中止 ボタンがクリックしてされたかどうかを確認する必要があります。 インスタンスを作成するため場合、`ProductsBLL`クラスし、呼び出し、`DiscontinueAllProductsForSupplier(supplierID)`に渡して、メソッド、`SupplierID`選択 FormView の。


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

なお、 `SupplierID` FormView で現在選択されている仕入先のことができますを使用してアクセス FormView s [ `SelectedValue`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx)です。 `SelectedValue`プロパティは、最初のデータ、フォーム ビューに表示されているレコードの値のキーを返します。 FormView s [ `DataKeyNames`プロパティ](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx)に自動的に設定されたフィールドのデータを元には、キーの値からプルされたデータを示す`SupplierID`を FormView 後に、ObjectDataSource をバインドするときに、Visual Studio で手順 2。

`ItemCommand`作成されると、イベント ハンドラーをとって、ページをテストします。 Cooperativa de Quesos を参照 ' Las Cabras' supplier (、私にとって FormView で業者に 5 番目の s)。 この供給業者は、2 つの製品、Queso Cabrales と Queso Manchego La Pastora、どちらも*いない*廃止されました。

Cooperativa de Quesos ' Las Cabras' が外に出てもビジネスとそのため、製品がある中止を想像してください。 クリックして、すべての製品ボタンを中止します。 クライアント側の確認ダイアログが表示されます (図 16 を参照してください)。


[![Cooperativa de Quesos Las Cabras 装置 2 つのアクティブな製品](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**図 16**: Cooperativa de Quesos Las Cabras 装置 2 つのアクティブな製品 ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


クライアント側の確認 ダイアログ ボックスで ok をクリックすると、フォームの送信は続行、ポストバックをもたらす FormView の`ItemCommand`イベントが発生します。 作成したイベント ハンドラーは実行、呼び出し、`DiscontinueAllProductsForSupplier(supplierID)`メソッドおよび Queso Cabrales と Queso Manchego La Pastora の両方の製品を中止します。

GridView のビュー ステートを無効にした場合、GridView はポストバックのたびに基になるデータ ストアに再バインドされているし、そのためするこれら 2 つの製品は、廃止されました (図 17 を参照してください) を反映するためにすぐに更新されます。 ただし、GridView での表示状態に無効にしたはない場合は、この変更を行った後 GridView にデータを手動で再バインドする必要があります。 これを実現する GridView s への呼び出しを行うだけで`DataBind()`メソッドの呼び出し後にすぐに、`DiscontinueAllProductsForSupplier(supplierID)`メソッドです。


[![すべての製品の中止 ボタンをクリックすると、業者 s は、製品が適宜更新](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**図 17**: すべての製品の中止 ボタンをクリックすると、業者 s は、製品が適宜更新 ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>手順 6: 製品の価格を調整するためのビジネス ロジック層で UpdateProduct オーバー ロードを作成します。

同様を中止するすべての製品ボタンで、フォーム ビューの拡大と縮小 GridView で製品の価格のボタンを追加するために必要があります最初に、適切なデータ アクセス層とビジネス ロジック層のメソッドを追加します。 DAL で 1 つの製品の行を更新するメソッドが既にある、ためこのような機能の新しいオーバー ロードを作成することで提供できます、 `UpdateProduct` BLL 内のメソッドです。

以前は、`UpdateProduct`スカラーの入力値として製品フィールドのいくつかの組み合わせで取得したし、指定された製品のフィールドだけを更新し、オーバー ロードします。 このオーバー ロードのためには、この標準は若干異なります、製品 s に、代わりに渡すお行います`ProductID`、割合を調整する、 `UnitPrice` (新しいに渡すこととは対照的調整`UnitPrice`自体)。 このアプローチには、ASP.NET ページの分離コード クラスで記述する必要のあるコードが簡略化、t が気に現在の製品 s を判断するにする必要があるないため`UnitPrice`です。

`UpdateProduct`オーバー ロードのため、このチュートリアルを次に示します。


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

このオーバー ロードは、DAL s を介して、指定された製品に関する情報を取得`GetProductByProductID(productID)`メソッドです。 これは、後かを確認するかどうか製品 s`UnitPrice`データベースが割り当てられている`NULL`値。 価格はままになっている場合は、変更されていません。 ただし、あるない場合は、`NULL` `UnitPrice`値、メソッドの更新プログラムの製品 s`UnitPrice`指定 % (`unitPriceAdjustmentPercent`)。

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>手順 7: GridView の増加と縮小ボタンの追加

GridView (および DetailsView) は両方の構成されて、フィールドのコレクション。 BoundFields、CheckBoxFields、および TemplateFields、に加えては、ASP.NET には、その名前からわかるように、ボタン、LinkButton、ImageButton に行ごとの列として表示すると、ButtonField が含まれています。 クリックすると、フォーム ビューのような*任意*内、GridView ページング ボタン、編集または削除のボタン、並べ替えボタンは、ボタンは、ポストバックが発生させ、GridView s [ `RowCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)です。

ButtonField が、`CommandName`そのボタンのそれぞれに、指定された値を割り当てるプロパティ`CommandName`プロパティです。 FormView、使用するような`CommandName`によって値が使用される、`RowCommand`イベント ハンドラー ボタンがクリックしてされました。

Let s 価格 +10 のボタン テキストのいずれかの GridView に 2 つの新しい ButtonFields を追加する % および価格-10 テキストと、その他の % です。 これら ButtonFields を追加するには、[GridView s のスマート タグからの列の編集リンクをクリックして左上にある一覧から ButtonField フィールドの種類を選択して追加] をクリックします。


![GridView に 2 つの ButtonFields を追加します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**図 18**: GridView に 2 つの ButtonFields を追加


最初の 2 つの GridView フィールドとして表示されるようにするには、2 つの ButtonFields を移動します。 次に、設定、`Text`プロパティ + 10% の価格を設定するこれら 2 つの ButtonFields および価格-10% と`CommandName`IncreasePrice および DecreasePrice、プロパティをそれぞれします。 既定では、ButtonField は、あるとしてその列のボタンをレンダリングします。 これは、ただし、ButtonField s [ `ButtonType`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)です。 これら 2 つの ButtonFields 正規プッシュ ボタンとしてレンダリングされますがある s を使用できます。このため、設定、`ButtonType`プロパティを`Button`です。 図 19 フィールドに表示 ダイアログ ボックス後、これらの変更が加えられました。GridView s の宣言型マークアップは、次のことです。


![ButtonFields テキスト、CommandName、および ButtonType プロパティを構成します。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**図 19**: 構成、ButtonFields `Text`、 `CommandName`、および`ButtonType`プロパティ


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

これら ButtonFields を作成すると、使用は、最後の手順を GridView s のイベント ハンドラーを作成し`RowCommand`イベント。 発生した場合、このイベント ハンドラー、価格 +10% または価格-10% ボタンがクリックすると、確認する必要があります、`ProductID`持つボタンがクリックしてされたを呼び出す行の`ProductsBLL`クラスの`UpdateProduct`適切なに渡して、メソッド`UnitPrice`と共に割合の調整、`ProductID`です。 次のコードでは、これらのタスクを実行します。

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

判断するため、`ProductID`行の価格が +10% または価格-10% ボタンがクリックされると、GridView s を参照する必要があります`DataKeys`コレクション。 このコレクションで指定されたフィールドの値を保持する、 `DataKeyNames` GridView の行ごとのプロパティです。 GridView s 以降`DataKeyNames`プロパティは、Visual Studio によって、ObjectDataSource を GridView にバインドする場合で、ProductID に設定された`DataKeys(rowIndex).Value`提供、 `ProductID` 、指定された*rowIndex*です。

ButtonField が自動的に渡されます、 *rowIndex*経由のボタンがクリックされた行の`e.CommandArgument`パラメーター。 したがってを決定する、`ProductID`行の価格が +10% または価格-10% ボタンがクリックされたを使用します:`Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`です。

すべての製品の中止 ボタンで GridView s のビューステートを無効にした場合 GridView ポストバックのたびに基になるデータ ストアに再バインドされているし、そのため、クリックしてから発生する料金変更を反映するためにすぐに更新されます。ボタンのいずれか。 ただし、GridView での表示状態に無効にしたはない場合は、この変更を行った後 GridView にデータを手動で再バインドする必要があります。 これを実現する GridView s への呼び出しを行うだけで`DataBind()`メソッドの呼び出し後にすぐに、`UpdateProduct`メソッドです。

図 20 は、おばあちゃん Kelly Homestead によって提供される製品を表示するときに、ページを示します。 図 21 は、% ボタンがクリックされたおばあちゃんの果汁 100% の分散され、価格-10% ボタンを 2 回クリック 1 回ピリピリの価格 +10 の後に結果を示します。


[![GridView には、価格 +10 が含まれています % と価格-10% ボタン。](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**図 20**: GridView 含まれるの Price + 10% と価格-10% ボタン ([フルサイズのイメージを表示する をクリックします](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))。


[![最初と 3 番目の製品の価格は、価格 +10 経由で更新されている % と価格-10% ボタン](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**図 21**:、価格の最初と 3 番目製品によって更新される価格 +10% と価格-10% ボタン ([フルサイズのイメージを表示するをクリックして](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> ボタン、ある、またはその TemplateFields に追加された ImageButtons GridView (および DetailsView) こともできます。 GridView s を発生させるように、BoundField をクリックすると、これらのボタンはポストバックが引き起こされる、`RowCommand`イベント。 ときに追加のボタンをクリックして TemplateField、ただし、ボタンの`CommandArgument`が自動的に設定されていない行のインデックスを ButtonFields を使用しているときもします。 内でクリックしてされたボタンの行インデックスを確認する必要がある場合、 `RowCommand` 、イベント ハンドラー ボタン s を手動で設定する必要があります`CommandArgument`のようなコードを使用して、TemplateField 内でその宣言の構文でのプロパティ。  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`。


## <a name="summary"></a>まとめ

GridView、DetailsView、FormView コントロールすべてには、ボタン、ある、または ImageButtons を含めることができます。 クリックすると、このようなボタンがポストバックを発生させるし、生成、 `ItemCommand` 、FormView および DetailsView コントロール内のイベントと`RowCommand`GridView でイベント。 これらの Web コントロールのデータには、削除するか、レコードの編集など、共通のコマンドに関連するアクションを処理する組み込みの機能があります。 ただし、おこともできますを使用してボタンをクリックしたときに、独自のカスタム コードの実行で応答します。

これを実現する必要がありますのイベント ハンドラーを作成する、`ItemCommand`または`RowCommand`イベント。 このイベント ハンドラーで最初に確認着信`CommandName`クリックしてされたボタンを特定し、適切なカスタム アクションを実行する値。 このチュートリアルでは、指定された業者のすべての製品を中止するまたは、10% で、特定の製品の価格を増減するボタンと ButtonFields を使用する方法を説明しました。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](adding-and-responding-to-buttons-to-a-gridview-cs.md)
