---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: 挿入、更新、およびデータ (c#) の削除の概要 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルではおとが表示され、ObjectDataSource の Insert()、Update() にマップする方法を構成する方法だけでなく、BLL のメソッドに Delete() メソッドがクラス.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: dbd111f79eda6006cb9aed59d8fd0b0342415833
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891231"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>挿入、更新、およびデータ (c#) の削除の概要
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe)または[PDF のダウンロード](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> このチュートリアルでは、ObjectDataSource の Insert()、Update() にマップする方法を見てみましょうおよびデータ変更機能を提供する GridView、DetailsView、フォーム ビューの各コントロールを構成する方法だけでなく、BLL のメソッドに Delete() メソッドがクラスです。


## <a name="introduction"></a>はじめに

過去のいくつかのチュートリアルでは、GridView、DetailsView、フォーム ビューのコントロールを使用して、ASP.NET ページでデータを表示する方法について説明しました。 これらのコントロールは、それらに指定されたデータで単に動作します。 一般的には、これらのコントロール、ObjectDataSource などのデータ ソース コントロールを使用してデータにアクセスします。 ASP.NET ページと基になるデータの間のプロキシとしての ObjectDataSource の機能説明しました。 その ObjectDataSource の GridView は、データを表示する必要があると、呼び出さ`Select()`メソッドで、さらにメソッドを呼び出しから、ビジネス ロジック層 (BLL)、適切なデータ アクセス レイヤーの (DAL) のメソッドを呼び出すが、TableAdapter に送信する、`SELECT` Northwind データベースへのクエリ。

DAL で Tableadapter を作成した際に注意してくださいを[、最初のチュートリアル](../introduction/creating-a-data-access-layer-cs.md)、Visual Studio が自動的に追加メソッドを挿入、更新、およびデータベース テーブルを基になるからデータを削除します。 さらに、[ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-cs.md)と呼ばれる BLL 内のメソッドにこれらのデータ変更 DAL メソッドように設計します。

加え、 `Select()` 、ObjectDataSource も`Insert()`、 `Update()`、および`Delete()`メソッドです。 同様に、`Select()`メソッド、3 つの方法は、基になるオブジェクトのメソッドにマップすることができます。 挿入、更新、またはデータを削除するように構成、GridView、DetailsView、フォーム ビュー コントロールは基になるデータを変更するユーザー インターフェイスを提供します。 このユーザー インターフェイスを呼び出す、 `Insert()`、 `Update()`、および`Delete()`基になるオブジェクトを呼び出し、ObjectDataSource のメソッドに関連付けられているメソッド (図 1 を参照してください)。


[![ObjectDataSource の Insert()、Update()、および Delete() メソッド、BLL にプロキシとして機能します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**図 1**:、ObjectDataSource `Insert()`、 `Update()`、および`Delete()`メソッドが、BLL にプロキシとして機能 ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


このチュートリアルでは、ObjectDataSource をマップする方法を会いしましょう`Insert()`、 `Update()`、および`Delete()`BLL とデータの変更を提供する GridView、DetailsView、およびフォーム コントロールを構成する方法でクラスのメソッドをメソッド機能します。

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>手順 1: Insert、Update、および Delete のチュートリアルの Web ページの作成

挿入、更新、およびデータを削除する方法の探索を開始して、前にまずみましょう。 このチュートリアルを次のいくつかのものが必要な web サイト プロジェクトに、ASP.NET ページを作成します。 という名前の新しいフォルダーを追加することによって開始`EditInsertDelete`です。 次に、各ページに関連付けるように、そのフォルダーに次の ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![データの変更に関連するチュートリアルについては、ASP.NET ページを追加します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**図 2**: データの変更に関連するチュートリアルについては、ASP.NET ページを追加します。


などの他のフォルダーで`Default.aspx`で、`EditInsertDelete`フォルダーは、チュートリアルのセクションに一覧表示します。 注意してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`ページの [デザイン] ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**図 3**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


最後に、エントリとして、ページを追加、`Web.sitemap`ファイル。 具体的には、カスタマイズされた書式設定後に、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、編集、挿入、およびチュートリアルを削除する項目が含まれています。


![サイト マップでは、編集、挿入、および削除のチュートリアルのエントリが含まれるようになりました](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**図 4**: サイト マップでは、編集、挿入、および削除のチュートリアルのエントリが含まれるようになりました


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>手順 2: 追加して、ObjectDataSource コントロールを構成します。

以降、GridView、DetailsView、およびそのデータ変更機能やレイアウトが異なる各 FormView、それぞれを個別に調べてみましょう。 独自の ObjectDataSource を使用して各コントロールがあるのではなくただし、作成してみましょうだけ 3 つのコントロールの例はすべてを共有できる単一 ObjectDataSource。

開く、 `Basics.aspx`  ページで、ObjectDataSource をツールボックスからデザイナーにドラッグして、スマート タグからのデータ ソースの構成リンクをクリックします。 以降、`ProductsBLL`編集、挿入、および削除する方法、構成をこのクラスを使用する ObjectDataSource を提供する唯一の BLL クラスです。


[![構成、ObjectDataSource ProductsBLL クラスを使用するには](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**図 5**: 構成を使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


次の画面では、どのような手段を指定できます、`ProductsBLL`クラスは、ObjectDataSource にマップされます`Select()`、 `Insert()`、 `Update()`、および`Delete()`を適切なタブを選択し、ドロップダウン リストからメソッドを選択します。 図 6 はなじみのここまでで、マップ、ObjectDataSource`Select()`メソッドを`ProductsBLL`クラスの`GetProducts()`メソッドです。 `Insert()`、 `Update()`、および`Delete()`メソッドは、上部にある一覧から適切なタブを選択して構成できます。


[![ObjectDataSource を返すすべての製品](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**図 6**: ObjectDataSource を返すすべての製品のある ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


図 7、8、9 ObjectDataSource の更新、挿入、および削除を表示するタブ。 これらのタブを構成できるように、 `Insert()`、 `Update()`、および`Delete()`メソッドを呼び出し、`ProductsBLL`クラスの`UpdateProduct`、 `AddProduct`、および`DeleteProduct`メソッド、それぞれします。


[![ObjectDataSource の Update() メソッドに、ProductBLL UpdateProduct クラスをマップします。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**図 7**: マップ ObjectDataSource の`Update()`メソッドを`ProductBLL`クラスの`UpdateProduct`メソッド ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![ObjectDataSource の Insert() メソッドに、ProductBLL AddProduct クラスをマップします。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**図 8**: マップ ObjectDataSource の`Insert()`メソッドを`ProductBLL`クラスの追加`Product`メソッド ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![ObjectDataSource の Delete() メソッドに、ProductBLL DeleteProduct クラスをマップします。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**図 9**: マップ ObjectDataSource の`Delete()`メソッドを`ProductBLL`クラスの`DeleteProduct`メソッド ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


お気付き UPDATE、INSERT、および DELETE の各タブで、ドロップダウン リストが選択されているこれらのメソッドに既に存在します。 これは、使用についてのご協力に感謝、`DataObjectMethodAttribute`のメソッドを装飾する`ProducstBLL`です。 たとえば、DeleteProduct メソッドは、次のシグネチャを持ちます。


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

`DataObjectMethodAttribute`属性を選択すると、挿入、更新、または削除、および既定値であるかどうかは、かどうか、各メソッドの目的を示します。 これらの属性を省略すると、BLL クラスを作成するときを必要がありますを手動で更新プログラムからメソッドを選択挿入、およびタブを削除します。

適切なことを確認した後`ProductsBLL`メソッドが ObjectDataSource にマップされます`Insert()`、 `Update()`、および`Delete()`メソッド、ウィザードを完了するには、[完了] をクリックします。

## <a name="examining-the-objectdatasources-markup"></a>ObjectDataSource のマークアップを確認します。

構成した後、ウィザードを通じて ObjectDataSource に生成された宣言型マークアップをソース ビューに移動します。 `<asp:ObjectDataSource>`タグは、基になるオブジェクトと呼び出すメソッドを指定します。 さらは`DeleteParameters`、 `UpdateParameters`、および`InsertParameters`の入力パラメーターにマップされる、`ProductsBLL`クラスの`AddProduct`、 `UpdateProduct`、および`DeleteProduct`メソッド。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource の一覧と同じように、その関連付けられているメソッドの入力パラメーターの各パラメーターが含まれる`SelectParameter`s は現在の入力パラメーターを受け取る select メソッドを呼び出す、ObjectDataSource が構成されている場合 (など`GetProductsByCategoryID(categoryID)`). おについては後ほど、これらの値として`DeleteParameters`、 `UpdateParameters`、および`InsertParameters`GridView、DetailsView、およびフォーム ビューで、ObjectDataSource を呼び出す前に自動的に設定されます`Insert()`、 `Update()`、または`Delete()`メソッド。 これらの値も設定できます必要に応じて、プログラムで今後のチュートリアルで説明するようです。

ObjectDataSource するように構成ウィザードを使用する 1 つの副作用は、Visual Studio が設定される、 [OldValuesParameterFormatString プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx)に`original_{0}`です。 このプロパティの値は編集対象のデータの元の値を含めるに使用され、2 つのシナリオで役に立ちます。

- レコードを編集するには、ユーザーは、主キーの値を変更します。 場合、 この場合、新しい主キーの値と元のプライマリ キーの値の両方必要がありますを指定する元のプライマリ キーの値を持つレコードが見つかりませんしてそれに従って更新その値を持つできるようにします。
- ときに、オプティミスティック同時実行制御を使用します。 オプティミスティック同時実行制御ことを確認する 2 つの手法の同時ユーザーは、いずれかに別の変更を上書きしないし、今後のチュートリアルのトピックは、です。

`OldValuesParameterFormatString`プロパティは、基になるオブジェクトの更新プログラムと元の値の削除メソッドで、入力パラメーターの名前を示します。 オプティミスティック同時実行制御をナビゲートするときは、このプロパティは、さらに詳しくは、その目的について説明します。 移行ここで、ただし、当社 BLL のメソッドでは、元の値が予期しないと、したがってはこのプロパティを削除する必要があるためです。 終了、`OldValuesParameterFormatString`プロパティが既定値以外に設定 (`{0}`) Web コントロールのデータが、ObjectDataSource を起動しようとしたときにエラーが発生`Update()`または`Delete()`メソッド、ObjectDataSource のため両方に渡して、`UpdateParameters`または`DeleteParameters`と、元の値パラメーターを指定します。

これがいない場合それほど明確この時点で、ご安心ください今後のチュートリアルでこのプロパティは、そのユーティリティについて見ていきましょう。 ここでは、だけを必ず宣言構文から完全にこのプロパティの宣言を削除するか、既定値 ({0}) に値を設定します。

> [!NOTE]
> だけオフにした場合、`OldValuesParameterFormatString`プロパティ [デザイン] ビューで [プロパティ] ウィンドウからプロパティ値は、宣言の構文では引き続き存在しますが、空の文字列に設定します。 これには、残念ながら、引き続き、上記で説明した同じ問題が発生されます。 そのため、削除するか完全宣言構文から、または、[プロパティ] ウィンドウからプロパティ値、既定値と`{0}`です。


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>手順 3: データ Web コントロールを追加して、データ変更用に構成します。

ObjectDataSource をページに追加し、構成後は、データの表示し、変更を加えて、エンドユーザーの手段を提供の両方のページに Web コントロールのデータを追加する準備ができました。 紹介 GridView、DetailsView、およびフォーム ビューとは別に、これらの Web コントロールのデータは、データ変更機能と構成で異なる。

後ほどお見せこの記事の残りの部分で非常に基本的な編集、挿入、および GridView DetailsView、によるサポートの削除を追加すると FormView 制御は本当に簡単に、いくつかのチェック ボックスをチェックします。 多くの微妙なとよりも複雑だけにポイントしをクリックしてこのような機能を提供することを構成する実際のエッジ ケースがあります。 このチュートリアルでは、ただし、単純なデータ変更機能を証明するについてのみ説明します。 今後のチュートリアルでは、必ず実際の設定で発生する問題を確認します。

## <a name="deleting-data-from-the-gridview"></a>GridView からデータを削除します。

GridView をツールボックスからデザイナーにドラッグして開始します。 GridView のスマート タグのドロップ ダウン リストから選択することによって GridView に、ObjectDataSource を次に、バインドします。 この時点で、GridView の宣言型マークアップになります。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

スマート タグから ObjectDataSource、GridView にバインドすると、2 つの利点があります。

- BoundFields と CheckBoxFields は、ObjectDataSource によって返されるフィールドの各自動的に作成されます。 さらに、BoundField および CheckBoxField のプロパティは、基になるフィールドのメタデータに基づいて設定されます。 たとえば、 `ProductID`、 `CategoryName`、および`SupplierName`フィールドは読み取り専用とでマークされて、`ProductsDataTable`のためべきでは更新可能な編集時にします。 この、これらの BoundFields をそれに合わせて[読み取り専用プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx)に設定されている`true`です。
- [DataKeyNames プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx)は基になるオブジェクトの主キー フィールドに割り当てられます。 これは、プロセスを使用して GridView データの削除、編集、またはこのプロパティは、フィールド (または一連のフィールド) を示します。 つまり一意各レコードを識別します。 詳細については、`DataKeyNames`プロパティに戻って、[マスター/詳細 DetailView で選択可能なマスター GridView の使用について詳しく説明](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)チュートリアルです。

GridView は、[プロパティ] ウィンドウまたは宣言の構文を ObjectDataSource にバインドすることができます、そう必要がありますに適切な BoundField を手動で追加して`DataKeyNames`マークアップ。

GridView コントロールは、行レベルの編集および削除するための組み込みサポートを提供します。 削除をサポートする GridView を構成するには、削除ボタンの列が追加されます。 エンドユーザーは、特定の行の削除 ボタンをクリックするとポストバックに陥ります GridView は、次の手順を実行します。

1. ObjectDataSource の`DeleteParameters`値が割り当てられています。
2. ObjectDataSource の`Delete()`メソッドが呼び出され、指定されたレコードを削除します。
3. GridView の再バインド数自体、ObjectDataSource を呼び出すことによってその`Select()`メソッド

割り当てられた値、`DeleteParameters`の値が格納されて、`DataKeyNames`の削除 ボタンがクリックしてされた行のフィールドです。 したがってことが重要を GridView の`DataKeyNames`プロパティが正しく設定されます。 存在しない場合、`DeleteParameters`が割り当てられる、 `null` 、さらに、いずれかでは発生しませんが、手順 1. の値は、手順 2. でレコードを削除します。

> [!NOTE]
> `DataKeys`つまり GridView のコントロールの状態でコレクションが格納された、 `DataKeys` GridView のビュー ステートが無効になっている場合でも、ポストバック間で値が記憶されます。 ただし、編集または削除する (既定の動作) をサポートする Gridview のビュー ステートを有効のまま、非常に重要なは。 GridView s を設定した場合`EnableViewState`プロパティを`false`、編集、および動作を削除すると、1 人のユーザーを問題なく動作が、同時実行ユーザーがデータを削除する場合は、これらの同時実行ユーザーが誤ってが発生する可能性が存在するがあります削除または編集を記録するようにでした t 予定です。 私のブログ記事を参照してください[警告: 同時実行の問題と ASP.NET 2.0 Gridview/DetailsView/FormViews その編集をサポートするか削除すると、をビュー ステートが無効になっている](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx)、詳細についてはします。


この同じ警告は、DetailsViews FormViews にも適用されます。

に GridView を削除する機能を追加するには、のスマート タグに移動し、削除を有効にする チェック ボックスを確認します。


![有効にする チェック ボックスを削除の確認します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**図 10**: 有効にする チェック ボックスを削除の確認


スマート タグからの削除を有効にするチェック ボックスをオン、CommandField を GridView に追加します。 次のタスクの 1 つ以上を実行するためのボタンを備えた GridView 内の列をレンダリングする、CommandField: レコードを選択して、レコードの編集、レコードを削除します。 内のレコードを選択する操作において CommandField を以前に説明しました、[マスター/詳細 DetailView で選択可能なマスター GridView の使用について詳しく説明](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)チュートリアルです。

桁数を格納する、CommandField`ShowXButton`プロパティを示す、CommandField でどのような一連のボタンが表示されます。 削除を有効にするチェック ボックスをオン、CommandField が`ShowDeleteButton`プロパティは`true`が GridView の Columns コレクションに追加されました。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

この時点で、信じられないおは完了して GridView に削除のサポートを追加します。 図 11 では、ブラウザーの削除 ボタンの列からこのページへのアクセスが存在する場合です。


[![CommandField 削除 ボタンの列を追加します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**図 11**: CommandField ボタンを追加、列の削除 ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


構築してきたこのチュートリアルをゼロから自分でをクリックすると、このページをテストするときに場合、[削除] ボタンには、例外が発生します。 あとは、これらの例外が発生した理由、およびそれらを修正する方法について説明します。

> [!NOTE]
> このチュートリアルに付属するダウンロードして使用している場合は、これらの問題が既にされて反映されます。 ただし、ことをお勧めすると、発生する可能性のある問題と適切な回避策を識別できるように、以下に示す詳細情報を読み取る。


場合、そのメッセージがに似ていますが、例外が発生する製品を削除すると、"*ObjectDataSource 'ObjectDataSource1' 非ジェネリック メソッドのパラメーターを持つ ' DeleteProduct' が見つかりませんでした: productID、オリジナル\_ProductID*、"を削除する忘れた可能性があります、 `OldValuesParameterFormatString` ObjectDataSource からプロパティです。 `OldValuesParameterFormatString` 、ObjectDataSource 両方で渡すしようとしました。 プロパティが指定された`productID`と`original_ProductID`入力パラメーター、`DeleteProduct`メソッドです。 `DeleteProduct`、ただし、のみ 1 つ入力パラメーターを受け取り、そのため、例外。 削除、`OldValuesParameterFormatString`プロパティ (または設定する`{0}`) ObjectDataSource いない元の入力パラメーターに渡してしようとするように指示します。


[![OldValuesParameterFormatString プロパティがクリアされたことを確認してください。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**図 12**: いることを確認、`OldValuesParameterFormatString`プロパティがされた消去アウト ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


削除した場合でも、`OldValuesParameterFormatString`プロパティ、まだは例外が発生した製品を削除するメッセージを使用しようとしています:"*参照制約と競合して DELETE ステートメント ' FK\_順序\_の詳細\_の製品の*"。Northwind データベースには間の外部キー制約が含まれています、`Order Details`と`Products`テーブルでの 1 つまたは複数のレコードがある場合、製品をシステムから削除できないことを意味、`Order Details`テーブル。 Northwind データベース内のすべての製品内には、少なくとも 1 つのレコードがあるので`Order Details`、ため、最初に、製品の関連する注文詳細レコードを削除するまで、すべての製品を削除できません。


[![外部キー制約には、製品の削除が禁止されています](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**図 13**: 外部キー制約には、製品の削除が禁止されています ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


このチュートリアルでみましょうだけを削除、すべてのレコードから、`Order Details`テーブル。 実際のアプリケーションでは、いずれかに必要があります。

- 別の画面が注文明細情報を管理するには
- 補強、`DeleteProduct`メソッドを指定した製品の注文の詳細を削除するためのロジックを含める
- TableAdapter を指定した製品の注文の詳細の削除を含めるために使用する SQL クエリを変更します。

説明のみを削除、すべてのレコードから、`Order Details`外部キー制約を回避するためのテーブルです。 Visual Studio でサーバー エクスプ ローラーを右クリックし、`NORTHWND.MDF`ノード、新しいクエリを選択します。 次に、クエリ ウィンドウでは、次の SQL ステートメントを実行します。 `DELETE FROM [Order Details]`


[![Order Details テーブルからすべてのレコードを削除します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**図 14**: からのすべてのレコードを削除、`Order Details`テーブル ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


消去した後、`Order Details`テーブルの削除 ボタンをクリックするとエラーが発生せず、製品が削除されます。 GridView のことを確認する場合は [削除] ボタンをクリックすると、製品が削除されることはできない、確認`DataKeyNames`プロパティが主キー フィールドに設定 (`ProductID`)。

> [!NOTE]
> [削除] ボタンをクリックするとポストバックに陥りますされ、レコードが削除されます。 これはする誤って正しくない行の削除 ボタンをクリックする簡単であるため危険です。 今後のチュートリアルでは、レコードを削除するときに、クライアント側の確認を追加する方法が表示されます。


## <a name="editing-data-with-the-gridview"></a>GridView にデータの編集

削除、と共に GridView コントロールは組み込み行レベルの編集機能も提供します。 編集をサポートする GridView を構成するには、編集ボタンの列が追加されます。 編集可能になる行の行の編集 ボタンの原因をクリックすると、エンドユーザーの視点からは、既存の値を格納していると、更新プログラムの編集 ボタンと キャンセル ボタンを置換テキスト ボックスに、セルにすることです。 必要な変更を行った後は変更をコミットする更新ボタンまたは [キャンセル] ボタンをクリックして破棄するエンド ユーザーことができますをクリックします。 Update または [キャンセル] をクリックした後をいずれの場合、GridView はその編集済み状態に戻ります。

ページ開発者は、当社の観点から、エンドユーザーには、特定の行の編集 ボタンがクリックすると、ポストバックに陥ります、GridView は、次の手順を実行します。

1. GridView の`EditItemIndex`プロパティがある編集ボタンがクリックされた行のインデックスに割り当てられています。
2. GridView の再バインド数自体、ObjectDataSource を呼び出すことによってその`Select()`メソッド
3. 一致する行のインデックス、 `EditItemIndex` 「編集モード」で表示されます このモードでは、[編集] ボタンが置き換えの更新および [キャンセル] ボタンと BoundFields が`ReadOnly`プロパティが False (既定値) の TextBox Web コントロールとしてレンダリングされます`Text`プロパティが、データ フィールドの値に割り当てられます。

この時点で、マークアップがエンドユーザーに行のデータを変更する、ブラウザーに返されます。 ユーザーには、[更新] ボタンがクリックすると、ポストバックが発生し、GridView は、次の手順を実行します。

1. ObjectDataSource の`UpdateParameters`値には、エンドユーザーが GridView の編集用のインターフェイスに入力した値が割り当てられます
2. ObjectDataSource の`Update()`メソッドが呼び出され、指定されたレコードの更新
3. GridView の再バインド数自体、ObjectDataSource を呼び出すことによってその`Select()`メソッド

割り当てられている主キーの値、`UpdateParameters`手順 1. で指定された値から取得、`DataKeyNames`プロパティを編集した行の TextBox Web コントロールのテキストを取得する非プライマリ キー値がします。 削除することが重要とされる GridView の`DataKeyNames`プロパティが正しく設定されます。 存在しない場合、`UpdateParameters`主キーの値が割り当てられる、 `null` 、さらに、いずれかでは発生しませんが、手順 1. の値には、手順 2. でレコードが更新されました。

GridView のスマート タグの編集を有効にするチェック ボックスをチェックするだけで、編集機能をアクティブにできます。


![有効にする チェック ボックスを編集の確認します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**図 15**: 有効にする チェック ボックスを編集の確認


編集を有効にする チェック ボックスは、(必要な場合)、CommandField に追加されますをチェックし、セット、`ShowEditButton`プロパティを`true`です。 前述のように、CommandField の個数`ShowXButton`プロパティを示す、CommandField でどのような一連のボタンが表示されます。 追加の編集を有効にするチェック ボックスをオン、`ShowEditButton`既存 CommandField プロパティ。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

すべて基本的な編集のサポートを追加するのには存在します。 Figure16 に示す編集インターフェイスはなく粗雑各 BoundField を`ReadOnly`プロパティに設定されている`false`(既定) は、テキスト ボックスとしてレンダリングされます。 ようなフィールドが含まれます`CategoryID`と`SupplierID`、他のテーブルにキーがあります。


[![クリックすると Chai s の編集 ボタンが編集モードで、行を表示します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**図 16**: クリックすると Chai s [編集] ボタンが編集モードで、行を表示 ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


外部キーの値を直接編集するユーザーを確認するだけでなく編集インターフェイスのインターフェイスは、次の方法で不足しています。

- ユーザーが入力した場合、`CategoryID`または`SupplierID`、データベースに存在しない、`UPDATE`が発生する例外を発生させる、外部キー制約に違反します。
- 編集のインターフェイスには、いずれかの検証が含まれていません。 必須の値を指定しない場合 (など`ProductName`)、または入力文字列 (「が多すぎる!」を入力するなどの数値が必要な場所 `UnitPrice`  ボックスに)、例外がスローされます。 今後のチュートリアルでは、編集のユーザー インターフェイスに検証コントロールを追加する方法を確認します。
- 現在、*すべて*GridView で読み取り専用ではない製品フィールドを含める必要があります。 GridView からフィールドを削除する場合、たとえば`UnitPrice`GridView のデータ更新が設定されていない場合、 `UnitPrice` `UpdateParameters`値で、変更、データベース レコードの`UnitPrice`を`NULL`値。 同様に場合の必須フィールドなど`ProductName`が削除された GridView から更新プログラムは同じで失敗"*列 'ProductName' が null を許容しない*"例外が上記です。
- 必要なことはたくさんのまま編集のインターフェイスの書式設定します。 `UnitPrice`が 4 つの 10 進数のポイントを表示します。 理想的には、`CategoryID`と`SupplierID`値には、システム内のカテゴリと仕入先を一覧表示 DropDownLists にが含まれます。

これらをここがの有効期限がないすべての欠点は、今後のチュートリアルでアドレス指定することです。

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>挿入する、編集、および DetailsView でデータを削除します。

以前のチュートリアルでは、コントロールを一度に 1 つのレコードを表示でき、GridView のように編集および現在表示されているレコードの削除を DetailsView 説明しました。 DetailsView と ASP.NET 側からワークフローのアイテムの削除を編集して両方、エンドユーザーのエクスペリエンスは GridView のものと同じです。 DetailsView と異なる GridView は、挿入の組み込みサポートも提供します。

GridView のデータ変更機能を示すためを起動する DetailsView を追加することによって、`Basics.aspx`既存 GridView 上のページし、DetailsView のスマート タグから既存の ObjectDataSource にバインドします。 次に、DetailsView をクリアします`Height`と`Width`プロパティ、およびスマート タグからのページングを有効にするオプションをチェックします。 編集を可能に挿入、および削除のサポート、単に編集を有効にする、挿入を有効にするには、および削除を有効にするチェック ボックスをオン スマート タグにします。


![サポートの編集、挿入、および削除を DetailsView を構成します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**図 17**: サポートの編集、挿入、および削除を DetailsView を構成します。


として、GridView、編集、追加で挿入、またはサポートを削除、CommandField を DetailsView、として追加宣言の構文を示します。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

なお DetailsView、CommandField の既定で列のコレクションの末尾に表示されます。 DetailsView のフィールドは行としてレンダリングされます、Insert の行として表示されます、CommandField のため、編集、および、DetailsView の下部にあるボタンを削除します。


[![サポートの編集、挿入、および削除を DetailsView を構成します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**図 18**: 編集をサポート、挿入、および削除を DetailsView を構成する ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


GridView と同様のイベントの同じシーケンスを開始する [削除] ボタンをクリックすると: ポストバック; a設定、ObjectDataSource の DetailsView 続けて`DeleteParameters`に基づいて、 `DataKeyNames` ; の値し、その ObjectDataSource の呼び出しで完了`Delete()`メソッドで、実際には、データベースから製品を削除します。 DetailsView で編集でも、GridView のものと同じ方法で動作します。

挿入すると、エンドユーザーが表示されます、New とボタンをクリックすると、「insert モード」で DetailsView のレンダリング 「挿入モード」では、新しいボタンが置き換え挿入し、[キャンセル] ボタンとそれらの BoundFields のみが`InsertVisible`プロパティに設定されている`true`(既定) が表示されます。 など、自動インクリメント フィールドとして識別されるデータ フィールド`ProductID`がその[InsertVisible プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx)'éý' `false` DetailsView をスマート タグからのデータ ソースにバインドするときにします。

Visual Studio の設定、スマート タグから DetailsView にデータ ソースをバインドするときに、`InsertVisible`プロパティを`false`自動インクリメント フィールドに対してのみです。 読み取り専用のフィールドと同様に`CategoryName`と`SupplierName`、しない限り、「insert モード」のユーザー インターフェイスに表示されます、`InsertVisible`プロパティ明示的に`false`です。 これら 2 つのフィールドを設定するには、しばらく時間かかる`InsertVisible`プロパティを`false`、スマート タグのリンク DetailsView の宣言構文またはフィールドを編集します。 図 19 の設定を表示する、`InsertVisible`プロパティを`false`でフィールドの編集 をクリックしてリンクします。


[![Northwind traders 社に Acme Tea が用意されています](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**図 19**: Northwind Traders ここでは、Acme 紅茶 ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


設定した後、`InsertVisible`プロパティ、ビュー、`Basics.aspx`ブラウザーでページおよび新しいボタンをクリックします。 新しい飲み物を追加するときに、図 20 が DetailsView を示しています、製品ラインに、Acme Tea です。


[![Northwind traders 社に Acme Tea が用意されています](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**図 20**: Northwind Traders ここでは、Acme 紅茶 ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


ポストバックに陥ります Acme 紅茶の詳細を入力すると、[挿入] ボタンをクリックするとに、新しいレコードを追加、`Products`データベース テーブルです。 この DetailsView には、データベース テーブルに存在する順序では、製品が一覧表示、ためお必要がありますページの最後の製品、新しい製品を確認するためにします。


[![Acme 紅茶の詳細](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**図 21**: Acme 紅茶の詳細 ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> DetailsView の[CurrentMode プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx)表示されているインターフェイスを示し、値は次のいずれかになります: `Edit`、 `Insert`、または`ReadOnly`です。 [DefaultMode プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx)を示し、DetailsView が、編集の後に戻るか、または挿入モードが完了したは永続的に編集または挿入モードを DetailsView を表示するために便利です。


ポイント アンド クリックを挿入して、編集、DetailsView の機能は、GridView と同じ制限に悩まさ: 既存のユーザーが入力する必要があります`CategoryID`と`SupplierID`テキスト ボックスを使用して値以外のインターフェイスに検証ロジック以外のすべて許可されていない製品フィールド`NULL`値または、既定値がありません。 挿入のインターフェイスを示すで、データベース レベルで指定された値を含める必要があります。

方法について調べますの拡張、および強化 GridView の編集用のインターフェイスで将来 DetailsView コントロールの編集と同様のインターフェイスを挿入するのにはアーティクルを適用できます。

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>FormView を使用して、柔軟なデータ変更のユーザー インターフェイスについて

FormView データの削除、挿入、編集、およびの組み込みサポートを提供するにはフィールドではなくテンプレートを使用しているため、BoundFields またはデータを提供する、GridView と DetailsView コントロールで使用される CommandField を追加する場所変更のインターフェイスです。 代わりに、ユーザーを収集するための Web コントロール新しい項目の追加時の入力または編集、削除、挿入、更新、およびキャンセル ボタン、新規と既存の 1 つの編集は、このインターフェイスは、適切なテンプレートを手動で追加する必要があります。 幸いにも、Visual Studio が自動的に作成、必要なインターフェイスのスマート タグで、ドロップダウン リストをデータ ソースに、フォーム ビューをバインドするときにします。

これらの手法を示すためを起動する FormView を追加することによって、`Basics.aspx`ページし、フォーム ビューのスマート タグから既に作成 ObjectDataSource にバインドします。 これが生成されます、 `EditItemTemplate`、 `InsertItemTemplate`、および`ItemTemplate`新規のユーザーの入力とボタンの Web コントロールを収集するための TextBox Web コントロールをフォーム ビュー、編集、削除、挿入、更新、およびキャンセル ボタン。 さらに、FormView の`DataKeyNames`プロパティが主キー フィールドに設定 (`ProductID`)、ObjectDataSource によって返されるオブジェクトの。 最後に、フォーム ビューのスマート タグの表示でページングを有効にするオプションを確認します。

FormView の宣言型マークアップを次に示します`ItemTemplate`FormView、ObjectDataSource をバインドした後です。 既定では、各非ブール値 product フィールドがバインドされている、`Text`ブール値の各フィールドの中にラベル Web コントロールのプロパティ (`Discontinued`) にバインドされて、`Checked`無効になっているチェック ボックスをオン Web コントロールのプロパティです。 命令型は、新規、編集、および削除のボタン クリックされたときに特定のフォーム ビューの動作をトリガーするためを`CommandName`値に設定されます`New`、 `Edit`、および`Delete`、それぞれします。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

図 22 示します FormView の`ItemTemplate`ブラウザーで表示したときにします。 下部にある、新規、編集、および削除のボタンを持つ各 product フィールドが表示されます。


[![既定の FormView ItemTemplate と共に新しい各 Product フィールドを一覧表示、編集、およびボタンを削除](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**図 22**:「既定 FormView`ItemTemplate`一覧各製品フィールドに沿った新規編集、および削除ボタンを使用 ([フルサイズのイメージを表示するには、をクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


などの GridView と削除 ボタンまたは、ボタン、LinkButton、または ImageButton をクリックして、DetailsView で持つ`CommandName`プロパティがポストバックを Delete 原因に設定されて、ObjectDataSource の設定`DeleteParameters`FormView のに基づいて`DataKeyNames`値に設定して、ObjectDataSource を呼び出す`Delete()`メソッドです。

ポストバックに陥りますにデータが再バインドの編集 ボタンがクリックされたときに、 `EditItemTemplate`、これは、編集用のインターフェイスをレンダリングします。 このインターフェイスには、更新、およびキャンセル ボタンと共にデータを編集するための Web コントロールが含まれています。 既定値`EditItemTemplate`によって生成された Visual Studio には、自動インクリメント フィールドのラベルが含まれています (`ProductID`)、各非ブール値フィールドをテキスト ボックスおよびブール値の各フィールドのチェック ボックスをオンします。 この動作は、GridView および DetailsView コントロールに自動生成された BoundFields によく似ています。

> [!NOTE]
> FormView の自動生成を小さな問題の 1 つ、`EditItemTemplate`レンダリング ボックスに Web にそれらのフィールドは読み取り専用などをコントロールには、`CategoryName`と`SupplierName`です。 この問題に対応する方法をお見せ直後。


テキスト ボックス コントロールで、`EditItemTemplate`がその`Text`プロパティの対応するフィールドを使用してデータの値にバインド*双方向データ バインド*です。 示される双方向データ バインド、`<%# Bind("dataField") %>`テンプレートにデータをバインドするとき、およびレコードの編集を挿入したり、ObjectDataSource のパラメーターを設定するときに両方のデータをバインドを実行します。 つまり、ユーザーをクリックしたときから、[編集]、 `ItemTemplate`、`Bind()`メソッドは、指定したデータ フィールドの値を返します。 ユーザーは、変更を行い、更新プログラムをクリックして後の値がポスト バックされた対応するを使用して指定データ フィールドに`Bind()`ObjectDataSource に適用される`UpdateParameters`です。 一方向のデータ バインドがで示される代わりに、 `<%# Eval("dataField") %>`、のみをテンプレートにデータをバインドするときに、データ フィールドの値を取得およびは*いない*ポストバック時にデータ ソースのパラメーターにユーザーが入力した値を返します。

次の宣言型マークアップ示します FormView の`EditItemTemplate`します。 なお、`Bind()`メソッドは、データ バインドの構文で使用され、更新とキャンセル ボタンの Web コントロールがある、`CommandName`を適宜設定のプロパティです。

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

当社`EditItemTemplate`、このポイントし、それを使用しようとしている場合にスローされる例外が発生します。 問題は、`CategoryName`と`SupplierName`TextBox Web コントロールのようにフィールドが表示される、`EditItemTemplate`です。 か、これらのテキスト ボックスをラベルに変更するか、それらを完全に削除する必要があります。 単に削除から完全に、`EditItemTemplate`です。

図 23 Chai の編集 ボタンがクリックしてされた後に、ブラウザーで、フォーム ビューを示しています。 なお、`SupplierName`と`CategoryName`で表示されるフィールド、`ItemTemplate`だけから削除したとは、存在しなく、`EditItemTemplate`です。 [更新] ボタンがクリックされたときに、フォーム ビュー、GridView と DetailsView コントロールとして同じ一連の手順を進めです。


[![既定で、EditItemTemplate が各編集可能な製品フィールドをテキスト ボックスまたはチェック ボックスを表示します。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**図 23**: 既定では、`EditItemTemplate`示します各編集可能な製品フィールドとしてテキスト ボックスやチェック ボックスをオン ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


[挿入] ボタンが FormView のクリックされたときに`ItemTemplate`ポストバックに陥ります。 ただし、データがバインドされていない、フォーム ビューに新しいレコードが追加されているためです。 `InsertItemTemplate`インターフェイスに挿入し、[キャンセル] ボタンと、新しいレコードを追加するための Web コントロールが含まれています。 既定値`InsertItemTemplate`によって生成された Visual Studio には、各非ブール値フィールドをテキスト ボックスと、自動生成されるように各ブール値フィールドのチェック ボックスが含まれています。`EditItemTemplate`のインターフェイスです。 TextBox コントロールがその`Text`プロパティが双方向データ バインドを使用して、対応するデータ フィールドの値にバインドします。

次の宣言型マークアップ示します FormView の`InsertItemTemplate`します。 注意してください、`Bind()`メソッドは、データ バインドの構文で使用し、Insert およびキャンセル ボタンの Web コントロールがその`CommandName`プロパティを適宜設定します。


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

FormView の自動生成で注意が必要です、`InsertItemTemplate`です。 でもそれらのフィールドは読み取り専用などを TextBox Web コントロールを作成する具体的には、`CategoryName`と`SupplierName`です。 使用するような`EditItemTemplate`からこれらのテキスト ボックスを削除する必要があります、`InsertItemTemplate`です。

図 24 は、新しい製品、Acme コーヒーを追加するときに、ブラウザーで FormView を示します。 なお、`SupplierName`と`CategoryName`で表示されるフィールド、`ItemTemplate`だけ削除したとは、存在しなくです。 [挿入] ボタンがクリックされた DetailsView コントロールとして同じ一連の手順を通じて FormView 収入ときに、新しいレコードの追加、`Products`テーブル。 図 25 では、挿入された後に、FormView で Acme コーヒー製品の詳細を示します。


[![FormView の挿入のインターフェイスを決定した後](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**図 24**: `InsertItemTemplate` FormView の挿入インターフェイスが決まります ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![FormView で新しい製品、Acme コーヒーの詳細が表示されます。](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**図 25**: FormView で新しい製品、Acme コーヒーの詳細が表示されます ([フルサイズのイメージを表示するをクリックして](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


読み取り専用分割、編集、およびインターフェイスを次の 3 つの個別のテンプレートに挿入する FormView、DetailsView と GridView よりも細かくこれらのインターフェイスが細かく制御できます。

> [!NOTE]
> DetailsView、FormView のと同様に`CurrentMode`プロパティが表示されているインターフェイスを示すと、その`DefaultMode`プロパティは、編集の後にモードをフォーム ビューを返しますを示しますまたは挿入が完了しました。


## <a name="summary"></a>まとめ

このチュートリアルでは、挿入、編集、および GridView、DetailsView、フォーム ビューを使用してデータの削除の基本を検査します。 これらのコントロールの 3 つすべては、Web コントロールのデータと、ObjectDataSource 感謝の ASP.NET ページで、1 つの行のコードを記述することがなく使用できる組み込みのデータ変更機能のいくつかのレベルを指定します。 ただし、単純なものがポイントして、かなり壊れ手法レンダリングと単純なデータ変更のユーザー インターフェイス をクリックします。 検証を提供するには、プログラムで値を挿入、例外を適切に処理、ユーザー インターフェイスのカスタマイズおよび、しなければならない一連の次のいくつかのチュートリアルで説明する手法に依存します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [次へ](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
