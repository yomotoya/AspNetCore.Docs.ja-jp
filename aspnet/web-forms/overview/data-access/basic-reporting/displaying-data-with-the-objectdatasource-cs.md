---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: (C#)、ObjectDataSource でデータの表示 |Microsoft Docs
author: rick-anderson
description: このチュートリアルを調べ、ObjectDataSource コントロールを havi せず、前のチュートリアルで作成した BLL から取得されたデータをバインドするこのコントロールを使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 7d4d292104703f9aebf131035920a058317011af
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395387"
---
<a name="displaying-data-with-the-objectdatasource-c"></a>(C#)、ObjectDataSource でデータの表示
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe)または[PDF のダウンロード](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> このチュートリアルは、行のコードを記述することがなく、前のチュートリアルで作成した BLL から取得されたデータをバインドするこのコントロールを使用して、ObjectDataSource コントロール。


## <a name="introduction"></a>はじめに

このアプリケーション アーキテクチャと web サイト ページ、レイアウトと完了のさまざまなデータおよびレポート関連の一般的なタスクを実行する方法の探索を開始する準備ができました。 前のチュートリアルでは、プログラムでデータ Web コントロール、ASP.NET ページで、DAL と BLL からデータをバインドする方法を説明しました。 この構文を割り当てるデータ Web コントロールの`DataSource`プロパティを表示し、コントロールを呼び出してデータを`DataBind()`メソッドが、ASP.NET 1.x アプリケーションで使用したパターンと、2.0 アプリケーションでは使用を続行できます。 ただし、ASP.NET 2.0 の新しいデータ ソース コントロールは、宣言型データを操作する方法を提供します。 作成された、BLL から取得されたデータをバインドするこれらのコントロールを使用して、[前のチュートリアル](../introduction/creating-a-business-logic-layer-cs.md)行のコードを記述することがなく!

ASP.NET 2.0 の 5 つの組み込みのデータ ソース コントロールが付属[SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx)、 [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx)、 [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx)、 [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx)、および[SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx)独自に構築することはできます[カスタム データ ソース コントロール](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp)必要な場合、します。 このチュートリアルのアプリケーション アーキテクチャを開発してきたために使用する、ObjectDataSource BLL クラスに対して。


![ASP.NET 2.0 には、5 つの組み込みのデータ ソース コントロールが含まれています。](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**図 1**: ASP.NET 2.0 には、5 つの組み込みのデータ ソース コントロールが含まれています。


ObjectDataSource は、その他のオブジェクトを操作するためのプロキシとして機能します。 ObjectDataSource を構成する指定しますこの基になるオブジェクトとそのメソッドと ObjectDataSource のマップ`Select`、 `Insert`、 `Update`、および`Delete`メソッド。 この基になるオブジェクトが指定されているし、そのメソッドは、ObjectDataSource にマップされている、ObjectDataSource をデータ Web コントロールにバインドできますがします。 ASP.NET は、Web コントロール、GridView、DetailsView、RadioButtonList、および、DropDownList を含む他のユーザーの間で多くのデータに付属します。 ページのライフ サイクル中にデータ Web コントロールがその ObjectDataSource を呼び出すことによってこれを実現するのには、バインドされているデータにアクセスする必要があります`Select`メソッド; データ Web コントロールでは、挿入、更新、サポートしているか、削除、呼び出し可能に場合、ObjectDataSource の`Insert`、 `Update`、または`Delete`メソッド。 これらの呼び出しは、次の図に示すように、適切な基になるオブジェクトのメソッドを ObjectDataSource でルーティングされます。


[![ObjectDataSource がプロキシとして機能します。](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**図 2**: プロキシとしての ObjectDataSource 機能 ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image4.png))。


ObjectDataSource を使用して、挿入するためのメソッドを呼び出すことが、更新、またはデータの削除、だけに注目しましょう。 データを返す今後のチュートリアルは、ObjectDataSource とデータを変更する Web コントロールのデータを使用して説明します。

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>手順 1: の追加と ObjectDataSource コントロールの構成

開いて開始、`SimpleDisplay.aspx`ページで、`BasicReporting`フォルダー、[デザイン] ビューに切り替えるし、ページのデザイン画面には、ツールボックスから、ObjectDataSource コントロールをドラッグします。 すべてのマークアップを生成しないために、ObjectDataSource がデザイン サーフェイス上の灰色のボックスとして表示されます。データは、指定したオブジェクトからのメソッドを呼び出すことによって単にアクセスします。 データ Web コントロール、GridView、DetailsView、FormView などによって、ObjectDataSource によって返されるデータを表示できます。

> [!NOTE]
> データ Web コントロールをページに追加するは、最初または、し、スマート タグから選択するとき、&lt;新しいデータ ソース&gt;ドロップダウン リストからオプション。


ObjectDataSource の基になるオブジェクトとそのオブジェクトのメソッドに ObjectDataSource のマップ方法を指定するには、ObjectDataSource のスマート タグからのデータ ソースの構成のリンクをクリックします。


[![をクリックして、スマート タグからのデータ ソースのリンクを構成します。](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**図 3**: スマート タグから構成データのソース リンクをクリックします ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image7.png))。


データ ソース構成ウィザードが表示されます。 最初に、ObjectDataSource が使用するには、オブジェクトを指定する必要があります。 この画面で、ドロップダウン リストで修飾されているオブジェクトのみを一覧表示「データ コンポーネントのみを表示する」のチェック ボックスをオンにした場合、`DataObject`属性。 現在、リストには、型指定されたデータセットと、前のチュートリアルで作成した BLL クラスで Tableadapter が含まれています。 追加することを忘れた場合、`DataObject`属性のビジネス ロジック層のクラスには表示されませんにこの一覧にします。 その場合は、BLL クラス (および型指定された DataSet、Datatable、Datarow でその他のクラス) を含める必要がありますが、すべてのオブジェクトを表示する「データ コンポーネントのみを表示する」のチェック ボックスをオフにします。

この最初の画面から選択、`ProductsBLL`ドロップダウン リストからクラスし、[次へ] をクリックします。


[![ObjectDataSource コントロールを使用するオブジェクトを指定します。](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**図 4**: ObjectDataSource コントロールを使用するオブジェクトを指定 ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image10.png))。


次の画面で、ウィザードでは、ObjectDataSource を呼び出す必要があるどのような方法を選択するように求められます。 ドロップダウン リストでは、前の画面から選択したオブジェクトにデータを返すメソッドが一覧表示します。 ここでわかります`GetProductByProductID`、 `GetProducts`、 `GetProductsByCategoryID`、および`GetProductsBySupplierID`します。 選択、`GetProducts`メソッド をクリックして、ドロップダウン リストから 完了 (追加した場合、`DataObjectMethodAttribute`を`ProductBLL`の前のチュートリアルでは、このオプションで示すように、メソッドは既定で選択されます)。


[![タブからデータを返す方法を選択します。](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**図 5**: データを返す処理の方法を選択 タブから選択 ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image13.png))。


## <a name="configure-the-objectdatasource-manually"></a>ObjectDataSource を手動で構成します。

ObjectDataSource のデータ ソース構成ウィザードを使用してオブジェクトを指定し、関連付けるオブジェクトのメソッドが呼び出される簡単な方法を提供します。 ただし、プロパティ ウィンドウを使用するか、宣言型マークアップで直接そのプロパティを介して ObjectDataSource を構成することができます。 設定するだけです、`TypeName`プロパティを使用するには、基になるオブジェクトの型と`SelectMethod`データを取得するときに呼び出されるメソッドにします。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

場合でも、必要がある場合、ObjectDataSource を手動で構成するように、ウィザードは、開発者が作成したクラスのみ一覧表示されます。 時間がある可能性があります、データ ソースの構成ウィザードを使用するとします。 ObjectDataSource をなど、.NET Framework のクラスにバインドする場合、[メンバーシップ クラス](https://msdn.microsoft.com/library/system.web.security.membership.aspx)、ユーザー アカウントの情報にアクセスする、または[Directory クラス](https://msdn.microsoft.com/library/system.io.directory.aspx)ファイル システム情報を使用するにはObjectDataSource のプロパティを手動で設定する必要があります。

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>手順 2: データ Web コントロールを追加して、ObjectDataSource にバインド

ObjectDataSource のによって返されるデータを表示するページにデータ Web コントロールを追加する準備ができました、ObjectDataSource がページに追加および構成されているが`Select`メソッド。 データ Web コントロールは、ObjectDataSource; にバインドできます。ObjectDataSource のデータを表示する GridView、DetailsView、および FormView で見てみましょう。

## <a name="binding-a-gridview-to-the-objectdatasource"></a>GridView を ObjectDataSource にバインドします。

GridView コントロールをツールボックスから追加`SimpleDisplay.aspx`のデザイン画面。 GridView のスマート タグからには、手順 1. で追加された ObjectDataSource コントロールを選択します。 ObjectDataSource からのデータによって返される各プロパティの GridView に、BoundField 自動的に作成されます`Select`メソッド (つまり、製品データ テーブルで定義されたプロパティ)。


[![GridView がページに追加されて、ObjectDataSource にバインド](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**図 6**: A GridView を ObjectDataSource にバインドをページに追加されたが ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image16.png))。


カスタマイズ、再配置、またはスマート タグからの列の編集オプションをクリックして、GridView の BoundFields を削除し、ことができます。


[![列の編集 ダイアログ ボックスの GridView の BoundFields を管理します。](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**図 7**: GridView の BoundFields 経由の編集の列 ダイアログ ボックスの管理 ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image19.png))。


GridView の BoundFields、削除を変更する少し、 `ProductID`、 `SupplierID`、 `CategoryID`、 `QuantityPerUnit`、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`BoundFields します。 左下にある一覧から、BoundField を選択し、削除ボタンをクリックして (赤色の X) を削除するだけです。 BoundFields を次に、再配置できるように、`CategoryName`と`SupplierName`BoundFields の前に、 `UnitPrice` BoundField をこれら BoundFields を選択し、上矢印をクリックします。 設定、`HeaderText`プロパティに残り BoundFields の`Products`、 `Category`、 `Supplier`、および`Price`、それぞれします。 次が、 `Price` BoundField を設定して、BoundField が通貨として書式設定`HtmlEncode`プロパティを False に、その`DataFormatString`プロパティを`{0:c}`します。 最後に、水平方向に整列、`Price`右側に、 `Discontinued`  チェック ボックスを使用して、センター、 `ItemStyle` / `HorizontalAlign`プロパティ。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![GridView の BoundFields がカスタマイズされています。](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**図 8**: GridView の BoundFields がカスタマイズされている ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image22.png))。


## <a name="using-themes-for-a-consistent-look"></a>一貫した外観のテーマの使用

これらのチュートリアルは、任意のコントロール レベルのスタイル設定を削除するよう努力して、可能であれば、外部ファイルで定義されているカスケード スタイル シートを代わりに使用します。 `Styles.css`ファイルが含まれます`DataWebControlStyle`、 `HeaderStyle`、 `RowStyle`、および`AlternatingRowStyle`Web これらのチュートリアルで使用されるコントロールを CSS クラスのデータの外観を決定するために使用する必要があります。 GridView のためには、設定して`CssClass`プロパティを`DataWebControlStyle`、およびその`HeaderStyle`、`RowStyle`と`AlternatingRowStyle`プロパティの`CssClass`プロパティに応じて。

これらを設定した場合`CssClass`に各データのこれらのプロパティ値を明示的に設定する必要があります Web コントロールのプロパティを Web コントロールのチュートリアルに追加します。 管理しやすいアプローチは、GridView、DetailsView の既定の CSS に関連するプロパティを定義して、FormView コントロール、テーマを使用します。 テーマは、コントロール レベルのプロパティの設定、画像、および一般的なルック アンド フィールを適用するサイト間でのページに適用できる CSS クラスのコレクションです。

このテーマは、画像や CSS ファイルに含まれません (スタイル シートのままにします`Styles.css`として-web アプリケーションのルート フォルダーで定義されている場合は、)、2 つのスキンにが含まれますが。 スキンは、Web コントロールの既定のプロパティを定義するファイルです。 具体的には、した GridView および DetailsView コントロールのスキン ファイルを既定値を示す`CssClass`-関連するプロパティ。

という名前のプロジェクトに新しいスキン ファイルを追加して開始`GridView.skin`をソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。


[![GridView.skin をという名前のスキン ファイルを追加します。](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**図 9**: スキン ファイルの名前を追加`GridView.skin`([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image25.png))。


スキン ファイルに存在する、テーマに配置することが必要、`App_Themes`フォルダー。 このようなフォルダーはまだありません、ために、Visual Studio は、最初のスキンを追加するときに、私たちの 1 つを作成する提供してください。 作成するには、[はい] をクリックして、`App_Theme`フォルダーと新しい配置`GridView.skin`ファイルがあります。


[![App_Theme フォルダーを作成する Visual Studio で自動的に](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**図 10**: Visual Studio で、作成、`App_Theme`フォルダー ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image28.png))。


これで新しいテーマが作成されます、`App_Themes`スキン ファイルを使用して、GridView をという名前のフォルダー`GridView.skin`します。


![GridView のテーマに追加された App_Theme フォルダー](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**図 11**: The GridView テーマに追加されて、`App_Theme`フォルダー


DataWebControls を GridView のテーマの名前を変更 (GridView フォルダーを右クリックし、`App_Theme`フォルダー名の変更を選択)。 次のマークアップを次に、入力、`GridView.skin`ファイル。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

これは、既定のプロパティを定義、 `CssClass`-DataWebControls テーマを使用しているページで、GridView の関連するプロパティ。 DetailsView、すぐ使用する Web コントロールのデータの別のスキンを追加してみましょう。 新しいスキンをという名前の DataWebControls テーマに追加`DetailsView.skin`し、次のマークアップを追加します。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

テーマを定義すると、最後の手順は、ASP.NET ページにテーマを適用します。 ページごとに、または web サイト内のすべてのページのテーマを適用できます。 このテーマを使用して、web サイトのすべてのページにしましょう。 これを実現するには、次のマークアップを追加`Web.config`の`<system.web>`セクション。


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

GridView を ObjectDataSource にバインドされているユーザーのブラウザーでページ上の空白の領域でどのマークアップもレンダリングしません。 `styleSheetTheme`設定では、テーマで指定したプロパティが必要があることを示します*いない*コントロール レベルで指定されたプロパティを上書きします。 テーマの設定がコントロールの設定を優先する必要がありますを指定するには、使用、`theme`属性の代わりに`styleSheetTheme`、残念ながら、テーマの設定で指定された、`theme`属性は、Visual Studio のデザイン ビューでは表示されません。 参照してください[ASP.NET のテーマおよびスキンの概要](https://msdn.microsoft.com/library/ykzx33wh.aspx)と[サーバー側のスタイルを使用してテーマ](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)テーマとスキン; の詳細については、次を参照してください[How To: ASP.NET のテーマの適用](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx)の詳細について。テーマを使用するページを構成します。


[![GridView は、製品の名前、カテゴリ、供給業者、価格、および提供が中止された情報が表示されます。](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**図 12**: 製品の名前、カテゴリ、供給業者、価格、および情報の提供が中止された GridView が表示されます ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image32.png))。


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>DetailsView で一度に 1 つのレコードを表示します。

GridView にバインドされているデータ ソース コントロールによって返されるレコードごとに 1 つの行が表示されます。 ただし、一度に唯一のレコードまたはレコード 1 つのみを表示したい場合がある場合があります。 [DetailsView コントロール](https://msdn.microsoft.com/library/s3w1w7t4.aspx)HTML としてレンダリングして、この機能を提供`<table>`2 つの列と 1 つの行の各列またはプロパティをコントロールにバインドします。 DetailsView は GridView と、単一レコードに 90 度回転として考えることができます。

DetailsView コントロールを追加して開始*上*で GridView`SimpleDisplay.aspx`します。 次に、GridView として同じ ObjectDataSource コントロールにバインドします。 ObjectDataSource のによって返されるオブジェクト内の各プロパティの DetailsView に追加されます、BoundField、GridView でよう`Select`メソッド。 唯一の違いは、垂直方向にではなく水平方向に DetailsView の BoundFields が配置されています。


[![ページに、DetailsView を追加し、ObjectDataSource にバインドします。](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**図 13**: DetailsView をページに追加し、ObjectDataSource にバインドする ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image35.png))。


GridView のような ObjectDataSource によって返されるデータのカスタマイズされた表示を提供する DetailsView の BoundFields を調整できます。 図 14 は、その BoundFields 後、DetailsView を示しています。 と`CssClass`GridView の例のような外観を作成するプロパティが構成されています。


[![DetailsView は、1 つのレコードを示しています。](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**図 14**: DetailsView は 1 つのレコードを示しています ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image38.png))。


DetailsView はそのデータ ソースによって返される最初のレコードしか表示されないことに注意してください。 ステップ実行のすべてのレコードを 1 つずつ、ユーザーを許可するには、DetailsView のページングを有効にするする必要があります。 これを行うには、Visual Studio に戻り、DetailsView のスマート タグでページングを有効にするチェック ボックスをオンします。


[![DetailsView コントロールでページングを有効にします。](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**図 15**: DetailsView コントロールでページングを有効にする ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image41.png))。


[![ページングが有効になっている、DetailsView では、製品のいずれかを表示します。](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**図 16**: ページング Enabled、DetailsView では、製品のいずれかを表示するユーザー ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image44.png))。


後でチュートリアルをページングについて説明します。

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>一度に 1 つのレコードを表示するためのより柔軟なレイアウト

DetailsView はかなり柔軟性に欠けるで ObjectDataSource から返された各レコードが表示されます。 データのより柔軟な表示をすることがあります。 たとえば、個々 の行に、製品の名前、カテゴリ、供給業者、価格、および各提供が中止された情報を表示するのではなくすることも、製品名を表示してでの価格、`<h4>`見出しで、表示される情報は、category と supplier以下のフォント サイズの価格と名前。 値の横にプロパティ名 (製品、カテゴリ、およびなど) を表示する重要ないない可能性があります。

[FormView コントロール](https://msdn.microsoft.com/library/fyf1dk77.aspx)このレベルのカスタマイズを提供します。 (GridView および DetailsView は、次の操作) などのフィールドを使用するのではなく、FormView が Web コントロール、静的な HTML の混在を使用できるテンプレートを使用し、[データ バインド構文](http://www.15seconds.com/issue/040630.htm)します。 かどうかに慣れて、Repeater コントロール ASP.NET から 1.x では、考えることができます、フォーム ビューの 1 つのレコードを表示するため、Repeater します。

FormView コントロールを追加、`SimpleDisplay.aspx`ページのデザイン画面。 最初に、フォーム ビューが表示されます灰色のブロックとして、少なくとも、コントロールを提供する必要があるかを知らせる`ItemTemplate`します。


[![FormView する必要がありますが、ItemTemplate を含める](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**図 17**:、FormView 含める必要があります、 `ItemTemplate` ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image47.png))。


FormView をバインドするには、既定値を作成、FormView のスマート タグからのデータ ソース コントロールに直接`ItemTemplate`自動的に (と共に、`EditItemTemplate`と`InsertItemTemplate`場合は、ObjectDatatSource コントロールの`InsertMethod`と`UpdateMethod`プロパティが設定されます)。 ただし、この例で、フォーム ビューにデータをバインドして指定の`ItemTemplate`手動でします。 FormView の設定で開始`DataSourceID`プロパティを`ID`、ObjectDataSource コントロールの`ObjectDataSource1`します。 次に、作成、 `ItemTemplate` 、製品の名前と価格を表示するよう、`<h4>`要素およびフォント サイズで下にはカテゴリや出荷業者の名前。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![最初の製品 (Chai) がカスタム形式で表示されます。](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**図 18**: カスタム形式で、最初の製品 (Chai) が表示されます ([フルサイズの画像を表示する をクリックします](displaying-data-with-the-objectdatasource-cs/_static/image50.png))。


`<%# Eval(propertyName) %>`データ バインディング構文を示します。 `Eval`メソッド FormView コントロールにバインドされている現在のオブジェクトの指定したプロパティの値を返します。 Alex Homer の記事[簡体字と ASP.NET 2.0 でのデータ バインディング構文の拡張](http://www.15seconds.com/issue/040630.htm)データ バインドの詳細の入出力について。

FormView には、DetailsView など、ObjectDataSource から返される最初のレコードのみ表示されます。 一度に 1 つの製品を通じてステップへの訪問者を許可するフォーム ビューでのページングを有効にすることができます。

## <a name="summary"></a>まとめ

ASP.NET 2.0 の ObjectDataSource コントロールに協力してくれたのコード行も記述せずにアクセスして、ビジネス ロジック層からのデータの表示を実現できます。 ObjectDataSource は、クラスの指定したメソッドを呼び出すし、結果を返します。 これらの結果は、データ、ObjectDataSource にバインドされている Web コントロールに表示できます。 このチュートリアルでは、ObjectDataSource を GridView、DetailsView、FormView コントロールのバインドについて説明しました。

これまでに、パラメーターのないメソッドを呼び出す、ObjectDataSource を使用する方法を見てきましたのみがなど、パラメーターの入力を受け取るメソッドを呼び出したい場合、`ProductBLL`クラスの`GetProductsByCategoryID(categoryID)`でしょうか。 1 つまたは複数のパラメーターを受け取るメソッドを呼び出すためにここには、これらのパラメーター値を指定する ObjectDataSource を構成する必要があります。 これを実現する方法を見て、[次のチュートリアル](declarative-parameters-cs.md)します。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [独自のデータ ソース コントロールを作成します。](https://msdn.microsoft.com/library/ms364049.aspx)
- [ASP.NET 2.0 の GridView の例](https://msdn.microsoft.com/library/aa479339.aspx)
- [簡素化し、データ連結 ASP.NET 2.0 の構文を拡張](http://www.15seconds.com/issue/040630.htm)
- [ASP.NET 2.0 のテーマ](http://www.odetocode.com/Articles/423.aspx)
- [テーマを使用してサーバー側のスタイル](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [方法: プログラムによって ASP.NET のテーマを適用](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が、Hilton Giesenow です。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](declarative-parameters-cs.md)
