---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: 挿入、更新、およびデータ (VB) の削除の概要 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、ObjectDataSource の Insert()、Update() にマップする方法を見ていきます、構成する方法についても、BLL のメソッドに Delete() メソッドがクラスとしています.
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 55fab6bb7a1041a14f8734a0d2ae1238b3801149
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833554"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>挿入、更新、およびデータ (VB) の削除の概要
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe)または[PDF のダウンロード](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> このチュートリアルでは、ObjectDataSource の Insert()、Update() にマップする方法を見ていき、データ変更の機能を提供する GridView、DetailsView と FormView コントロールを構成する方法についても、BLL のメソッドに Delete() メソッドのクラスします。


## <a name="introduction"></a>はじめに

過去のいくつかのチュートリアルでは、GridView、DetailsView、FormView コントロールを使用して、ASP.NET ページにデータを表示する方法について説明しました。 これらのコントロールは、単に指定されたデータで動作します。 一般的には、これらのコントロール、ObjectDataSource などのデータ ソース コントロールを使用してデータにアクセスします。 ObjectDataSource は ASP.NET ページと基になるデータの間のプロキシとして機能する方法を説明しました。 GridView では、データを表示する必要があります、その ObjectDataSource が呼び出されます`Select()`メソッドから、ビジネス ロジック層 (BLL)、適切なデータ アクセス層の (DAL) のメソッドを呼び出したメソッドが呼び出されます、TableAdapter が送信されますが、`SELECT` Northwind データベースにクエリします。

DAL で Tableadapter を作成したときにいずれ[、最初のチュートリアル](../introduction/creating-a-data-access-layer-cs.md)、Visual Studio が自動的に追加メソッドの挿入、更新、およびデータベース テーブルを基になるからデータを削除しています。 さらに、[ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-vb.md)これらのデータ変更 DAL メソッドにと呼ばれる BLL のメソッドを設計しました。

加え、`Select()`メソッド、ObjectDataSource も`Insert()`、 `Update()`、および`Delete()`メソッド。 ように、`Select()`メソッドでは、これら 3 つのメソッドは、基になるオブジェクトでメソッドにマップすることができます。 挿入、更新、またはデータを削除するように構成、GridView、DetailsView、FormView コントロールは、基になるデータを変更するためユーザー インターフェイスを提供します。 このユーザー インターフェイスを呼び出す、 `Insert()`、 `Update()`、および`Delete()`基になるオブジェクトを呼び出して、ObjectDataSource のメソッドに関連付けられているメソッド (図 1 参照)。


[![ObjectDataSource の Insert()、Update()、および Delete() メソッド、BLL にプロキシとして機能します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**図 1**:、ObjectDataSource の`Insert()`、 `Update()`、および`Delete()`メソッドは、BLL にプロキシとして機能 ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))。


このチュートリアルでは ObjectDataSource をマップする方法を見ていきます`Insert()`、 `Update()`、および`Delete()`BLL とデータの変更を提供する GridView、DetailsView と FormView コントロールを構成する方法でクラスのメソッドをメソッド機能。

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>手順 1: Insert、Update、および Delete のチュートリアルの Web ページを作成します。

挿入、更新、およびデータを削除する方法の調査を始める前にまずみましょう。 このチュートリアルの次のいくつかの必要がありますが、web サイト プロジェクトで、ASP.NET ページを作成します。 という名前の新しいフォルダーを追加することで開始`EditInsertDelete`します。 次に、次の ASP.NET ページを使用する各ページに関連付けることを確認、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![データの変更に関連するチュートリアルについては、ASP.NET ページを追加します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image4.png)

**図 2**: データの変更に関連するチュートリアルについては、ASP.NET ページを追加


などの他のフォルダーで`Default.aspx`で、`EditInsertDelete`フォルダーは、チュートリアルのセクションで一覧表示します。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`ページの デザイン ビューの ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**図 3**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))。


最後に、ページに追加するエントリとして、`Web.sitemap`ファイル。 具体的には、カスタマイズされた書式設定後、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューには、編集、挿入、および削除のチュートリアルの項目が含まれています。


![サイト マップには、編集、挿入、および削除のチュートリアルのエントリが含まれますようになりました](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**図 4**: サイト マップには、編集、挿入、および削除のチュートリアルのエントリが含まれますようになりました


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>手順 2: の追加と ObjectDataSource コントロールの構成

以降、GridView、DetailsView、およびフォーム ビューのデータ変更の機能とレイアウトにそれぞれが異なる、それぞれを個別に調べてみましょう。 独自の ObjectDataSource を使用して各コントロールがあるのではなく、作成しましょう 3 つのコントロールのすべての例を共有できる単一の ObjectDataSource。

開く、 `Basics.aspx`  ページで、ObjectDataSource をツールボックスからデザイナーにドラッグして、スマート タグからのデータ ソースの構成のリンクをクリックします。 以降、`ProductsBLL`は編集、挿入、および削除メソッドでは、このクラスを使用する ObjectDataSource を構成するだけの BLL クラスです。


[![ProductsBLL クラスを使用する ObjectDataSource を構成します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**図 5**: 構成に使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))。


次の画面のどのような方法を指定できます、`ProductsBLL`クラスは、ObjectDataSource にマップされます`Select()`、 `Insert()`、`Update()`と`Delete()`適切なタブを選択し、ドロップダウン リストからメソッドを選択します。 図 6 は、ここまでで見慣れた、ObjectDataSource のマップ`Select()`メソッドを`ProductsBLL`クラスの`GetProducts()`メソッド。 `Insert()`、 `Update()`、および`Delete()`メソッドは、上部にある一覧から適切なタブを選択して構成できます。


[![ObjectDataSource 返すすべての製品](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**図 6**: ObjectDataSource を返すすべての製品がある ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))。


図 7、8、および 9 ObjectDataSource の更新、挿入、および削除を表示するタブで行います。 これらのタブを構成できるように、 `Insert()`、`Update()`と`Delete()`メソッドを呼び出す、`ProductsBLL`クラスの`UpdateProduct`、`AddProduct`と`DeleteProduct`メソッド、それぞれします。


[![ObjectDataSource の Update() メソッドを ProductBLL クラスの UpdateProduct メソッドにマップします。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**図 7**: マップ ObjectDataSource の`Update()`メソッドを`ProductBLL`クラスの`UpdateProduct`メソッド ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))。


[![ObjectDataSource の Insert() メソッドを ProductBLL クラスの AddProduct メソッドにマップします。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**図 8**: マップ ObjectDataSource の`Insert()`メソッドを`ProductBLL`クラスの追加`Product`メソッド ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))。


[![ObjectDataSource の Delete() メソッドを ProductBLL クラスの DeleteProduct メソッドにマップします。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**図 9**: マップ ObjectDataSource の`Delete()`メソッドを`ProductBLL`クラスの`DeleteProduct`メソッド ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))。


お気付き UPDATE、INSERT、および DELETE の各タブで、ドロップダウン リストが既に選択されているこれらのメソッドを持っています。 これを使用するのに協力してくれた、`DataObjectMethodAttribute`のメソッドを装飾する`ProducstBLL`します。 たとえば、DeleteProduct メソッドは、次のシグネチャを持ちます。


[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

`DataObjectMethodAttribute`属性を選択、挿入、更新、または削除およびかどうか、既定値は、各メソッドの目的を示します。 これらの属性を省略すると、BLL クラスを作成するときに、必要があります、更新プログラムからメソッドを手動で選択を挿入するしタブを削除します。

適切なことを確認した後は`ProductsBLL`メソッドは、ObjectDataSource にマップされます`Insert()`、`Update()`と`Delete()`メソッドは、ウィザードを完了するには、[完了] をクリックします。

## <a name="examining-the-objectdatasources-markup"></a>ObjectDataSource のマークアップを調べる

ObjectDataSource ウィザードを構成した後は、生成された宣言型マークアップを確認する、ソース ビューに移動します。 `<asp:ObjectDataSource>`タグは、基になるオブジェクトとメソッドを呼び出すを指定します。 さらに、ある`DeleteParameters`、 `UpdateParameters`、および`InsertParameters`の入力パラメーターにマップされる、`ProductsBLL`クラスの`AddProduct`、 `UpdateProduct`、および`DeleteProduct`メソッド。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

ObjectDataSource の一覧と同様に、その関連付けられているメソッドの入力パラメーターの各パラメーターが含まれる`SelectParameter`s が存在する入力パラメーターが必要とする選択メソッドを呼び出して、ObjectDataSource が構成されている場合 (など`GetProductsByCategoryID(categoryID)`). ご覧のとおり、これらの値`DeleteParameters`、 `UpdateParameters`、および`InsertParameters`GridView、DetailsView、FormView して ObjectDataSource を呼び出す前に自動的に設定されます`Insert()`、 `Update()`、または`Delete()`メソッド。 これらの値も設定できます必要に応じて、プログラムによって、今後のチュートリアルで後ほど説明します。

ObjectDataSource するように構成ウィザードを使用する 1 つの副作用は、Visual Studio が設定される、 [OldValuesParameterFormatString プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx)に`original_{0}`します。 このプロパティの値は編集対象のデータの元の値を含めるために使用し、は 2 つのシナリオで役立ちます。

- レコードを編集するには、ユーザーは主キーの値を変更できます。 場合、 この場合、新しい主キーの値と元のプライマリ キー値の両方必要があります指定する元のプライマリ キー値を持つレコードが見つかりませんを適宜更新されるよう、その値を持つようにします。
- ときに、オプティミスティック同時実行制御を使用します。 オプティミスティック同時実行することを確認する 2 つの手法は、同時ユーザーは、1 つに相互の変更を上書きしないでくださいし、今後のチュートリアルのトピックでは、します。

`OldValuesParameterFormatString`プロパティは、基になるオブジェクトの更新プログラムと元の値を delete メソッドで、入力パラメーターの名前を示します。 オプティミスティック同時実行制御について説明している場合は、このプロパティは、さらに詳しくは、その目的について説明します。 起動します。 ここで、ただし、BLL のメソッドでは、元の値が予期しないであるため、このプロパティを削除重要なためです。 終了、`OldValuesParameterFormatString`プロパティの既定値以外に設定 (`{0}`) データ Web コントロールが呼び出す ObjectDataSource のしようとしたときにエラーが発生`Update()`または`Delete()`メソッド、ObjectDataSource があるため両方に渡して、`UpdateParameters`または`DeleteParameters`元の値のパラメーターと共に指定されています。

そうではそれほど明確この時点で、ご心配なく、今後のチュートリアルでこのプロパティは、そのユーティリティについて見ていきましょう。 ここでは、だけにする宣言構文から完全にこのプロパティの宣言を削除するか、既定値に値を設定する ({0})。

> [!NOTE]
> だけオフにした場合、`OldValuesParameterFormatString`プロパティ [デザイン] ビューで [プロパティ] ウィンドウからプロパティ値が宣言の構文ではそのままが空の文字列に設定します。 これには、残念ながら、引き続き、前に説明した同じ問題が発生されます。 そのため、削除するか完全宣言の構文から、または、[プロパティ] ウィンドウからプロパティ値を既定の`{0}`します。


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>手順 3: データ Web コントロールを追加して、データ変更のための構成

ObjectDataSource は、ページに追加され、構成されているとは、両方は、データを表示し、エンドユーザーを修正するための手段を提供するページにデータ Web コントロールを追加する準備ができました。 見て GridView、DetailsView、およびフォーム ビューとは別に、これらのデータ Web コントロールのデータ変更の機能と構成が異なるとします。

見て、この記事の残りの部分で非常に基本的な編集、挿入、および GridView、DetailsView、を通じてサポートを削除するを追加すると、コントロールと FormView コントロールは、実際に、いくつかのチェック ボックスをチェックだけです。 多くの細部とのみポイント アンド クリックしてより複雑なこのような機能を提供することで現実世界のエッジ ケースがあります。 このチュートリアルは、単純なデータ変更の機能を証明するのみに焦点を当てています。 今後のチュートリアルでは、必ず実際の設定で発生する問題を確認します。

## <a name="deleting-data-from-the-gridview"></a>GridView からデータを削除します。

GridView をツールボックスからデザイナーにドラッグして開始します。 次に、GridView のスマート タグでドロップダウン リストから選択して、GridView を ObjectDataSource をバインドします。 この時点で、GridView の宣言型マークアップになります。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

ObjectDataSource のスマート タグを GridView にバインドすると、2 つの利点があります。

- BoundFields と CheckBoxFields は ObjectDataSource によって返されるフィールドの各自動的に作成されます。 さらに、BoundField および CheckBoxField のプロパティは、基になるフィールドのメタデータに基づいて設定されます。 たとえば、 `ProductID`、`CategoryName`と`SupplierName`フィールドは読み取り専用とでマークされて、`ProductsDataTable`したがってすべき更新可能なを編集するときとします。 この、これらの BoundFields を対応するために[読み取り専用プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx)に設定されている`True`します。
- [DataKeyNames プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx)は基になるオブジェクトの主キー フィールドに割り当てられます。 これは、プロセスの編集または削除、データのフィールド (または一連のフィールド) に、このプロパティが示すとおり、GridView を使用している一意な各レコードを識別します。 詳細については、`DataKeyNames`プロパティに戻って、[マスター/詳細 DetailView で選択可能なマスター GridView の使用について詳しく説明](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)チュートリアル。

GridView は、[プロパティ] ウィンドウまたは宣言の構文を ObjectDataSource にバインドできます、そう必要があります手動で追加する適切な BoundField と`DataKeyNames`マークアップ。

GridView コントロールは、行レベルの編集および削除するための組み込みサポートを提供します。 構成の削除をサポートするために GridView 列の削除 ボタンを追加します。 エンドユーザーは、特定の行の削除 ボタンをクリックすると、ポストバックに陥ります、GridView は、次の手順を実行します。

1. ObjectDataSource の`DeleteParameters`値が割り当てられています。
2. ObjectDataSource の`Delete()`メソッドが呼び出される、指定されたレコードを削除します。
3. GridView を再バインド自体、ObjectDataSource を呼び出すことによってその`Select()`メソッド

割り当てられた値、`DeleteParameters`の値は、`DataKeyNames`の Delete ボタンがクリックされた行のフィールド。 したがってことが重要する GridView の`DataKeyNames`プロパティが正しく設定します。 存在しない場合、`DeleteParameters`の値が割り当てられます`Nothing`手順 1 で、これには発生しません手順 2. で削除されたレコード。

> [!NOTE]
> `DataKeys`つまり GridView のコントロールの状態でコレクションが格納された、`DataKeys`値は、GridView のビュー ステートが無効になっている場合でもポストバック間で記憶されます。 ただし、編集または削除 (既定の動作) をサポートする Gridview のビュー ステートを有効のままことが非常に重要です。 GridView 秒に設定した場合`EnableViewState`プロパティを`false`、編集と削除の動作は、1 人のユーザーを問題なく動作が、同時実行ユーザーがデータを削除する場合は、これらの同時実行ユーザーが誤ってが発生する可能性が存在するがあります削除または編集を記録するようにしなかった t 予定です。 私のブログ記事を参照してください。[警告: 同時実行の問題を ASP.NET 2.0 Gridview と DetailsView/FormViews そのサポートを編集または削除およびをビュー ステートが無効になっている](http://scottonwriting.net/sowblog/posts/10054.aspx)、詳細についてはします。


この同じ警告は、DetailsViews FormViews にも適用されます。

に GridView を削除する機能を追加するには、そのスマート タグに移動し、削除を有効にするチェック ボックスをオンします。


![有効にする チェック ボックスを削除するを確認してください。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**図 10**: 有効にする チェック ボックスを削除するを確認してください。


GridView に、[commandfield] を追加するスマート タグから削除を有効にするチェック ボックスをオンします。 [Commandfield] で、次のタスクの 1 つ以上を実行するためのボタンの GridView 列を表示します。 レコードの編集、レコードを削除するレコードを選択します。 以前の動作を内のレコードを選択すると [commandfield] を説明しました、[マスター/詳細の選択可能なマスター GridView を使用すると詳細 DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)チュートリアル。

[Commandfield] には数が含まれています`ShowXButton`プロパティを示す、[commandfield] でどのような一連のボタンが表示されます。 削除を有効にする、[commandfield] チェック ボックスをオンの`ShowDeleteButton`プロパティは`True`が GridView の列のコレクションに追加されました。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

この時点では、信じられないかもしれませんが、終わって GridView に削除のサポートを追加してください。 図 11 に示すようとブラウザーの削除 ボタンの列からこのページにアクセスが存在します。


[![[Commandfield] 列の削除 ボタンを追加します](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**図 11**: [commandfield] ボタンを追加、列の削除 ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))。


構築してきたこのチュートリアルを最初から自分でクリックすると、このページをテストするときに Delete ボタンに例外が発生します。 あとは、これらの例外が発生した理由とその解決方法について説明します。

> [!NOTE]
> このチュートリアルに付属するダウンロードを使用してに沿ってフォローしている場合は、これらの問題が既にされて反映されます。 ただし、発生する可能性のある問題と適切な回避策を特定するのには以下の詳細を読み取ることはお勧めします。


メッセージがような例外が発生した商品を削除しようとすると場合は、"*ObjectDataSource 'ObjectDataSource1' 非ジェネリック メソッドのパラメーターを持つ ' DeleteProduct' が見つかりませんでした: productID、元\_ProductID*、"を削除し忘れた可能性があります、 `OldValuesParameterFormatString` ObjectDataSource からのプロパティ。 `OldValuesParameterFormatString`プロパティが指定されて、ObjectDataSource が両方に渡そう`productID`と`original_ProductID`に対する入力パラメーターは、`DeleteProduct`メソッド。 `DeleteProduct`、ただし、のみ 1 つ入力パラメーターを受け取り、そのため、例外。 削除、`OldValuesParameterFormatString`プロパティ (またはに設定すると`{0}`)、ObjectDataSource を元の入力パラメーターを渡すよう指示します。


[![OldValuesParameterFormatString プロパティがクリアされたことを確認します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**図 12**: いることを確認、`OldValuesParameterFormatString`プロパティがされたクリア Out ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))。


削除した場合でも、`OldValuesParameterFormatString`プロパティはそれでも例外メッセージで商品を削除しようとするとき:"*DELETE ステートメントと競合して参照制約 ' FK\_順序\_の詳細\_製品の*"。Northwind データベースには間の外部キー制約が含まれています、`Order Details`と`Products`で 1 つまたは複数のレコードがある場合、製品をシステムから削除できないことを意味しているテーブル、`Order Details`テーブル。 Northwind データベースのすべての製品少なくとも 1 つのレコードを持つため`Order Details`製品、製品の関連付けられている注文の詳細レコードを削除する最初まで削除できません。


[![外部キー制約には、製品の削除が禁止されています](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**図 13**: 外部キー制約には、製品の削除が禁止されています ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))。


このチュートリアルでは、削除のすべてのレコードから、`Order Details`テーブル。 実際のアプリケーションでは、いずれかに必要があります。

- 別の画面が注文の詳細情報を管理するには
- 拡張、`DeleteProduct`メソッドを指定された製品の注文の詳細を削除するロジックを含める
- TableAdapter を指定された製品の注文の詳細の削除を含めるために使用する SQL クエリを変更します。

削除のすべてのレコードから、`Order Details`テーブルの外部キー制約を回避するためにします。 Visual Studio でサーバー エクスプ ローラーに移動しを右クリックし、`NORTHWND.MDF`ノード、新しいクエリを選択します。 次に、クエリ ウィンドウでは、次の SQL ステートメントを実行します。 `DELETE FROM [Order Details]`


[![受注明細テーブルからすべてのレコードを削除します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**図 14**: すべてのレコードを削除、`Order Details`テーブル ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))。


消去した後に、`Order Details`テーブルの削除 ボタンをクリックするとエラーが発生せず、製品が削除されます。 GridView のことを確認する場合は、[削除] ボタンをクリックすると、製品が削除されることはできませんが確認`DataKeyNames`プロパティが主キー フィールドに設定 (`ProductID`)。

> [!NOTE]
> [Delete] ボタンをクリックすると、ポストバックに陥りますされ、レコードが削除されます。 これは、誤って間違った行の削除 ボタンをクリックする簡単であるため危険であります。 今後のチュートリアルでは、レコードを削除するときに、クライアント側の確認を追加する方法を表示されます。


## <a name="editing-data-with-the-gridview"></a>GridView を使用してデータを編集

GridView コントロールには、削除、と共に組み込みの行レベルの編集機能も提供します。 構成の編集をサポートする GridView 編集ボタンの列を追加します。 行の編集 ボタンの原因に編集可能になる行をクリックすると、エンドユーザーの観点からは、既存の値を格納していると、更新プログラムの編集 ボタンと キャンセル ボタンを置換するテキスト ボックスに、セルにすること。 必要な変更を行った後、エンドユーザーは、変更をコミットする更新プログラムのボタンまたは [キャンセル] ボタンをクリックして破棄するクリックできます。 Update または [キャンセル] をクリックした後をどちらの場合、GridView は、編集済みの状態に戻ります。

ページの開発者としてマイクロソフトの展望からは、ポストバックに陥ります、エンドユーザーは、特定の行の編集 ボタンをクリックすると、し、GridView は、次の手順を実行します。

1. GridView の`EditItemIndex`プロパティがある編集ボタンがクリックされた行のインデックスに割り当てられています。
2. GridView を再バインド自体、ObjectDataSource を呼び出すことによってその`Select()`メソッド
3. 一致する行インデックス、 `EditItemIndex` 「編集モード」で表示される。 このモードで編集 ボタンは、更新のキャンセル ボタンと BoundFields に置換されますが`ReadOnly`プロパティが False (既定値) テキスト ボックスに Web コントロールとしてレンダリングされます`Text`プロパティは、データ フィールドの値に割り当てられています。

この時点で、マークアップは、エンドユーザーに行のデータを変更する、ブラウザーに返されます。 ユーザーは、[更新] ボタンをクリックすると、ポストバックが発生し、GridView は、次の手順を実行します。

1. ObjectDataSource の`UpdateParameters`値には、エンドユーザーが GridView の編集インターフェイスに入力した値が割り当てられます
2. ObjectDataSource の`Update()`メソッドが呼び出される、指定されたレコードを更新しています
3. GridView を再バインド自体、ObjectDataSource を呼び出すことによってその`Select()`メソッド

割り当てられている主キーの値、`UpdateParameters`手順 1. で指定された値から取得、`DataKeyNames`プロパティが編集された行の TextBox Web コントロールのテキストを取得する非プライマリ キー値。 削除するには、必須を GridView の`DataKeyNames`プロパティが正しく設定します。 存在しない場合、`UpdateParameters`主キーの値の値が割り当てられている`Nothing`手順 1 で、これには発生しません手順 2 で更新されたレコード。

GridView のスマート タグの編集を有効にするチェック ボックスをチェックするだけでは、編集機能をアクティブ化することができます。


![確認のチェック ボックスの編集を有効にします。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**図 15**: チェック ボックスの編集を有効にするを確認してください。


編集を有効にする チェック ボックスは、(必要な) 場合、commandfield に追加されますをチェックし、セット、`ShowEditButton`プロパティを`True`します。 [Commandfield] には数が含まれています前述のように、`ShowXButton`プロパティを示す、[commandfield] でどのような一連のボタンが表示されます。 追加の編集を有効にするチェック ボックスをオン、`ShowEditButton`プロパティを既存の [commandfield]。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

ですが基本的な編集のサポートを追加します。 編集インターフェイスはかなりおおざっぱ Figure16 に示す各 BoundField が`ReadOnly`プロパティに設定されて`False`(既定値) は、テキスト ボックスとしてレンダリングされます。 などのフィールドが含まれます`CategoryID`と`SupplierID`、他のテーブルにキーであります。


[![Chai の編集ボタンをクリックして編集モードで、行を表示します](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**図 16**: Chai の をクリックして [編集] ボタンが編集モードで、行を表示します ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))。


外部キーの値を直接編集するユーザーを確認するだけでなく、編集インターフェイスのインターフェイスは、次の方法で不足しています。

- ユーザーが入力した場合、`CategoryID`または`SupplierID`、データベースに存在しない、`UPDATE`例外が発生する外部キー制約に違反します。
- 編集インターフェイスには、いずれかの検証が含まれていません。 必要な値を指定しない場合 (など`ProductName`)、または文字列値を入力します (「あまり!」を入力するなど、数値が必要な場合 `UnitPrice` textbox)、例外がスローされます。 今後のチュートリアルでは、編集のユーザー インターフェイスに検証コントロールを追加する方法を説明します。
- 現時点では、*すべて*GridView で読み取り専用ではない製品フィールドを含める必要があります。 GridView からフィールドを削除する場合、たとえば`UnitPrice`GridView データの更新が設定されていない場合、 `UnitPrice` `UpdateParameters`値で、変更、データベース レコードの`UnitPrice`を`NULL`値。 同様に必須のフィールドの場合など`ProductName`、削除は、GridView から更新プログラムは同じで失敗"*列 'ProductName' が null を許容しない*"上記で説明した例外。
- 編集のインターフェイスの書式設定が望ましい場合に多くのままです。 `UnitPrice`付き 10 進数の 4 つの点で示されます。 理想的には、`CategoryID`と`SupplierID`値には、カテゴリと仕入先をシステムで一覧した Dropdownlist にが含まれます。

これらの現在のところですが我慢する必要がありますすべての欠点は、今後のチュートリアルで対処される予定です。

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>挿入、編集、および DetailsView を使用してデータを削除します。

前述したように、前のチュートリアルでは、DetailsView コントロールは一度に 1 つのレコードを表示でき、GridView のように編集および現在表示されているレコードの削除します。 両方のエンド ユーザー エクスペリエンスを編集してから、DetailsView と ASP.NET の側からのワークフロー項目の削除は、GridView のと同じです。 GridView、DetailsView とは異なるが、挿入の組み込みのサポートも提供します。

GridView のデータ変更機能を示すためを開始する、DetailsView を追加することで、`Basics.aspx`ページ上の既存の GridView および DetailsView のスマート タグを既存の ObjectDataSource にバインドします。 次に、DetailsView をクリアします`Height`と`Width`プロパティ、およびスマート タグからページングを有効にするオプションのチェック ボックス。 編集を有効にするには、挿入、および削除のサポート、単純にチェック ボックスを編集の有効化、挿入を有効にするには、および削除を有効にするスマート タグの。


![編集、挿入、および削除をサポートする DetailsView を構成します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**図 17**: 編集、挿入、および削除をサポートする DetailsView を構成します。


として、GridView、編集、追加で挿入、または削除のサポートとして追加します、CommandField、DetailsView を宣言型構文を次に示します。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

DetailsView、[commandfield] の既定の列コレクションの末尾に表示されるに注意してください。 DetailsView のフィールドとしてレンダリングされるので、行が編集、および DetailsView の下部にあるボタンを削除、[commandfield] は、挿入、行として表示されます。


[![編集、挿入、および削除をサポートする DetailsView を構成します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**図 18**: サポートの編集、挿入、および削除に DetailsView を構成する ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))。


GridView と同様、同じイベントのシーケンスを開始する [Delete] ボタンをクリックします。 をポストバックします。設定の ObjectDataSource の DetailsView 続けて`DeleteParameters`に基づいて、 `DataKeyNames` ; の値し、その ObjectDataSource の呼び出しで完了`Delete()`メソッドで、実際には、データベースから製品を削除します。 GridView の場合と同じ方法で動作も、DetailsView で編集します。

挿入すると、エンドユーザーが表示されます、新規のボタンをクリックすると、「挿入モード。」DetailsView を表示します。 挿入し、[キャンセル] ボタンとそれらの BoundFields のみを置き換えては「挿入モード」の新しいボタンが`InsertVisible`プロパティに設定されて`True`(既定値) が表示されます。 など、自動インクリメント フィールドとして識別されるデータ フィールド`ProductID`がその[InsertVisible プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx)に設定`False`DetailsView をスマート タグによってデータ ソースにバインドする場合。

Visual Studio の設定でスマート タグの DetailsView をデータ ソースをバインドするときに、`InsertVisible`プロパティを`False`自動インクリメント フィールドに対してのみです。 読み取り専用のフィールドのような`CategoryName`と`SupplierName`、しない限り、「insert モード」のユーザー インターフェイスに表示されます、`InsertVisible`プロパティ明示的に`False`します。 これら 2 つのフィールドを設定する少し`InsertVisible`プロパティ`False`、スマート タグのリンクを DetailsView の宣言構文またはフィールドを編集します。 図 19 の設定の表示、`InsertVisible`プロパティを`False`編集フィールドをクリックしてリンクします。


[![Northwind Traders Acme 紅茶を提供します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**図 19**: Northwind Traders ここでは、Acme 紅茶 ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))。


設定した後、`InsertVisible`プロパティ、ビュー、`Basics.aspx`ブラウザーでページし、新しいボタンをクリックします。 新しい飲み物の場合は、追加するときに、図 20 は、DetailsView を示しています、製品ラインの Acme 紅茶します。


[![Northwind Traders Acme 紅茶を提供します。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**図 20**: Northwind Traders ここでは、Acme 紅茶 ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))。


ポストバックに陥ります Acme 紅茶の詳細を入力し、[挿入] ボタンをクリックするとに新しいレコードを追加、`Products`データベース テーブル。 この DetailsView では、データベース テーブルに存在する順序で製品が一覧表示、ために必要がありますページ最後の製品、新製品を表示するには。


[![Acme 紅茶の詳細](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**図 21**: Acme 紅茶の詳細 ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))。


> [!NOTE]
> DetailsView の[CurrentMode プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx)表示されているインターフェイスを示し、値は次のいずれかを指定できます: `Edit`、 `Insert`、または`ReadOnly`します。 [DefaultMode プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx)を示し、DetailsView、編集後に戻るか、または挿入モードが完了したは、DetailsView は完全に編集または挿入モードを表示するために便利です。


ポイント アンド クリックを挿入して、編集、DetailsView の機能は、GridView と同じ制限に悩まさ: 既存のユーザーが入力する必要があります`CategoryID`と`SupplierID`値がテキスト ボックスではインターフェイスに検証ロジックはすべて製品のフィールドを許可しない`NULL`値または既定値がありません。 挿入のインターフェイスを示すでは、データベース レベルで指定された値を含める必要があります。

手法を拡張および強化、GridView の編集インターフェイス今後の記事は、DetailsView コントロールの編集および挿入インターフェイスにもに適用できるため、考察します。

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>FormView を使用してより柔軟なデータ変更のユーザー インターフェイス

FormView のデータの削除、挿入、編集、および組み込みのサポートを提供していますが、フィールドではなくテンプレートを使用しているため、BoundFields または GridView および DetailsView コントロールでデータを提供するために使用する [commandfield] を追加する場所はありません。変更のインターフェイス。 代わりに、ユーザーを収集するための Web コントロール、新しい項目を追加するときに入力または編集、削除、挿入、更新、およびキャンセル ボタン、新規と既存のリソースの編集は、このインターフェイスは、適切なテンプレートを手動で追加する必要があります。 幸いにも、Visual Studio が自動的に作成、必要なインターフェイスのスマート タグで、ドロップダウン リストをデータ ソースに、フォーム ビューをバインドする場合。

これらの手法を示すためを起動するフォーム ビューを追加することで、`Basics.aspx`ページし、FormView のスマート タグから既に作成されて ObjectDataSource にバインドします。 これが生成されます、 `EditItemTemplate`、 `InsertItemTemplate`、および`ItemTemplate`新規ユーザーの入力と Web のボタン コントロールを収集するためのテキスト ボックスに Web コントロールと FormView、編集、削除、挿入、更新、およびキャンセル ボタン。 さらに、FormView の`DataKeyNames`プロパティが主キー フィールドに設定 (`ProductID`) の ObjectDataSource によって返されるオブジェクト。 最後に、フォーム ビューのスマート タグでページングを有効にするオプションを確認します。

FormView の宣言型マークアップを次に示します`ItemTemplate`FormView、ObjectDataSource にバインドした後。 既定では各非ブール値の product フィールドにバインドされて、`Text`ブール値の各フィールドの中に、ラベルの Web コントロールのプロパティ (`Discontinued`) にバインドされて、`Checked`無効になっているチェック ボックスを Web コントロールのプロパティ。 新規、編集、および Delete ボタンがクリックされたときに特定のフォーム ビューの動作をトリガーするためは命令型を`CommandName`設定値は`New`、`Edit`と`Delete`、それぞれします。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

図 22 示します FormView の`ItemTemplate`とき、ブラウザーで表示します。 下部にある、新規、編集、および Delete ボタンを持つ製品の各フィールドが表示されます。


[![照合 FormView ItemTemplate と共に新しい各 Product フィールドを一覧表示、編集、およびボタンの削除](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**図 22**: The 照合 FormView`ItemTemplate`一覧各製品フィールドに沿った新規、編集、削除ボタンと ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))。


GridView と DetailsView を削除 ボタン、LinkButton や ImageButton をクリックすると使用するようなある`CommandName`プロパティは、削除の原因にポストバックを設定、ObjectDataSource を設定します`DeleteParameters`FormView のに基づいて`DataKeyNames`。値、および呼び出す ObjectDataSource の`Delete()`メソッド。

ポストバック後し、にデータが再バインドの編集 ボタンがクリックされたときに、 `EditItemTemplate`、これはデータの編集インターフェイスをレンダリングします。 このインターフェイスには、更新、およびキャンセル ボタンと共にデータを編集するための Web コントロールが含まれています。 既定の`EditItemTemplate`によって生成された Visual Studio には、自動インクリメント フィールドのラベルが含まれています (`ProductID`)、各非ブール値フィールドのテキスト ボックスおよびブール値の各フィールドのチェック ボックスをオンします。 この動作は、GridView および DetailsView コントロールで自動生成された BoundFields に非常に似ています。

> [!NOTE]
> 1 つのフォーム ビューの自動生成に関する問題、`EditItemTemplate`がレンダリングされるテキスト ボックスに Web コントロールは読み取り専用、ようにこれらのフィールドを`CategoryName`と`SupplierName`します。 これを考慮する方法を見てすぐします。


コントロールをテキスト ボックス、`EditItemTemplate`がその`Text`プロパティの対応するフィールドを使用してデータの値にバインドされて*双方向データ バインド*します。 表される、双方向データ バインド`<%# Bind("dataField") %>`テンプレートにデータをバインドする際、および挿入またはレコードの編集の ObjectDataSource のパラメーターを設定するときに、両方のデータをバインドを実行します。 つまり、ユーザーがから [編集] ボタンをクリックすると、 `ItemTemplate`、`Bind()`メソッドは、指定したデータ フィールドの値を返します。 ユーザーは、変更を行い、更新プログラムをクリックして後の値がポスト バックされた対応するデータ フィールドを使用して指定する`Bind()`ObjectDataSource に適用される`UpdateParameters`します。 代わりに、一方向のデータ バインドで示されています。 `<%# Eval("dataField") %>`、のみ、テンプレートにデータをバインドするときに、データ フィールドの値を取得およびは*いない*ポストバック時に、データ ソースのパラメーターにユーザーが入力した値を返します。

次の宣言型マークアップ示します FormView の`EditItemTemplate`します。 なお、`Bind()`メソッドは、データ バインドの構文で使用し、更新プログラムおよびキャンセル ボタンの Web コントロールがその`CommandName`プロパティを適宜設定します。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

この`EditItemTemplate`、これを使用する場合にスローされる例外が発生、ポイントします。 問題なは、`CategoryName`と`SupplierName`内のテキスト ボックスに Web コントロールとして、フィールドが表示される、`EditItemTemplate`します。 か、これらのテキスト ボックスをラベルに変更するか、それらを完全に削除する必要があります。 単に削除から完全に、`EditItemTemplate`します。

図 23 Chai の編集 ボタンがクリックしてされた後に、ブラウザーで、フォーム ビューを示しています。 なお、`SupplierName`と`CategoryName`で表示されるフィールド、`ItemTemplate`だけから削除しましたが存在する場合は、不要になった、`EditItemTemplate`します。 [更新] ボタンがクリックされたときに、FormView GridView および DetailsView コントロールとして同じ一連の手順を進めします。


[![既定で、後に各編集可能な製品フィールドをテキスト ボックスまたはチェック ボックスが表示されます。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**図 23**: 既定では、`EditItemTemplate`示します各編集可能な製品フィールドとしてテキスト ボックスまたはチェック ボックスをオン ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))。


[挿入] ボタンに FormView がクリックされたとき`ItemTemplate`ポストバックに陥ります。 ただし、データがバインドされていない、FormView に新しいレコードが追加されるためです。 `InsertItemTemplate`インターフェイスに挿入し、[キャンセル] ボタンと新しいレコードを追加するための Web コントロールが含まれています。 既定の`InsertItemTemplate`によって生成された Visual Studio には、各非ブール値フィールド、テキスト ボックスと自動生成されるように各ブール値フィールドのチェック ボックスが含まれています。`EditItemTemplate`のインターフェイス。 テキスト ボックス コントロールがその`Text`双方向データ バインドを使用して、対応するデータ フィールドの値にバインドされているプロパティ。

次の宣言型マークアップ示します FormView の`InsertItemTemplate`します。 なお、`Bind()`メソッドは、データ バインドの構文で使用し、Insert およびキャンセル ボタンの Web コントロールがその`CommandName`プロパティを適宜設定します。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

FormView の自動世代の注意が必要です、`InsertItemTemplate`します。 読み取り専用、ようにこれらのフィールドの場合でも、テキスト ボックスに Web コントロールを作成する具体的には、`CategoryName`と`SupplierName`します。 使用するような`EditItemTemplate`、これらのテキスト ボックスから削除する必要があります、`InsertItemTemplate`します。

図 24 は、新しい製品を Acme コーヒーを追加するときにブラウザーで、フォーム ビューを示します。 なお、`SupplierName`と`CategoryName`で表示されるフィールド、`ItemTemplate`だけ削除しましたが存在する場合は、不要になった。 [挿入] ボタンで、同じ一連の手順として、DetailsView コントロールをフォーム ビュー処理の進行状況がクリックされたときに新しいレコードを追加、`Products`テーブル。 図 25 では、挿入された後、FormView で Acme コーヒー製品の詳細が示されます。


[![FormView の挿入のインターフェイスを決定後、](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**図 24**: `InsertItemTemplate` FormView の挿入インターフェイスによって決まります ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))。


[![FormView で Acme コーヒーの新しい製品の詳細が表示されます。](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**図 25**: Acme コーヒーの新しい製品の詳細が、フォーム ビューで表示されます ([フルサイズの画像を表示する をクリックします](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))。


によって、読み取り専用の分離、編集、およびインターフェイスを次の 3 つの個別のテンプレートに挿入する、FormView が GridView と DetailsView よりも細かくのこれらのインターフェイスを制御できます。

> [!NOTE]
> などの DetailsView、FormView の`CurrentMode`プロパティが表示されているインターフェイスを示します、`DefaultMode`プロパティは、編集後、モードを FormView 返しますを示しますまたは挿入が完了します。


## <a name="summary"></a>まとめ

このチュートリアルでは、挿入、編集、および GridView、DetailsView、FormView を使用してデータの削除の基本について確認しました。 これらのコントロールの 3 つのすべてでは、いくつかデータ Web コントロールと ObjectDataSource に協力してくれたの ASP.NET ページで 1 行のコードを記述することがなく利用できる組み込みのデータ変更の機能レベルを提供します。 ただし、単純なは、ポイントして、かなり壊れ手法レンダリングと単純なデータ変更のユーザー インターフェイス をクリックします。 検証を提供するには、プログラムの値を挿入、例外を適切に処理、ユーザー インターフェイスをカスタマイズし、必要になります次のいくつかのチュートリアルで説明する手法の多くに依存します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](limiting-data-modification-functionality-based-on-the-user-cs.md)
> [次へ](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
