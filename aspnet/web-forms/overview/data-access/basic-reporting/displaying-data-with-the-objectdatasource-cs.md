---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: ObjectDataSource (c#) でデータの表示 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルは、havi なし前のチュートリアルで作成された BLL から取得されたデータをバインドすることができます、このコントロールを使用して、ObjectDataSource コントロールに検索しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 6fbad218f3e02bde13b8788bf6f1eefac0c4990c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="displaying-data-with-the-objectdatasource-c"></a>ObjectDataSource (c#) でデータの表示
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe)または[PDF のダウンロード](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> このチュートリアルを行のコードを記述しなくても、前のチュートリアルで作成された BLL から取得されたデータをバインドすることができます、このコントロールを使用して、ObjectDataSource コントロールを見ます。


## <a name="introduction"></a>はじめに

マイクロソフト アプリケーション アーキテクチャと web サイト ページ、レイアウトと完了のさまざまな一般的なデータおよびレポート関連のタスクを実行する方法の探索を開始する準備ができました。 前のチュートリアルで、プログラムによってデータ、ASP.NET ページ内の Web コントロールに、DAL BLL からデータをバインドする方法を説明しました。 この構文を割り当てるデータ Web コントロールの`DataSource`プロパティを表示し、呼び出すことで、コントロールのデータを`DataBind()`メソッド 1.x の ASP.NET アプリケーションで使用するパターン、2.0、アプリケーションでは使用を続行できます。 ただし、ASP.NET 2.0 の新しいデータ ソース コントロールは、宣言型データを操作する方法を提供します。 作成された、BLL から取得されたデータをバインドするこれらのコントロールを使用して、[前のチュートリアル](../introduction/creating-a-business-logic-layer-cs.md)コードの行を記述する必要はありません。

5 つの組み込みのデータ ソース コントロールに付属して ASP.NET 2.0 [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx)、 [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx)、 [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx)、 [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx)、および[SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx)独自に作成することができますが[カスタム データ ソース コントロール](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp)、必要に応じて。 チュートリアル アプリケーションのアーキテクチャを開発してきたために使用する、ObjectDataSource BLL クラスに対してです。


![ASP.NET 2.0 には、5 つの組み込みのデータ ソース コントロールが含まれています。](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**図 1**: ASP.NET 2.0 には、5 つの組み込みのデータ ソース コントロールが含まれています。


ObjectDataSource は、その他のオブジェクトを操作するためのプロキシとして機能します。 構成する、ObjectDataSource お指定このオブジェクトとそのメソッドが ObjectDataSource にどのようにマップするかを基になる`Select`、 `Insert`、 `Update`、および`Delete`メソッドです。 この基になるオブジェクトが指定されているし、そのメソッドが ObjectDataSource にマップされている、Web コントロールのデータに、ObjectDataSource し、バインドことができます。 ASP.NET は、Web、やなどのコントロール、GridView、DetailsView、RadioButtonList、DropDownList、他の多くのデータに付属します。 ページのライフ サイクル中に、Web コントロールのデータがこれを実現するか、ObjectDataSource を呼び出すことによってにバインドされているデータにアクセスする必要があります`Select`メソッド; Web コントロールのデータを挿入、更新、サポートしているか削除すると、呼び出しする変更内容が、ObjectDataSource の`Insert`、 `Update`、または`Delete`メソッドです。 これらの呼び出しは、次の図に示すように、適切な基になるオブジェクトのメソッドに ObjectDataSource によってルーティングされます。


[![ObjectDataSource をプロキシとして機能します。](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**図 2**: プロキシとして、ObjectDataSource 機能 ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image4.png))


挿入するためのメソッドを呼び出すには、ObjectDataSource を使用できますが、更新、または、データを削除するだけに注目しましょう; のデータを返す今後のチュートリアルは、ObjectDataSource とデータのデータを変更する Web コントロールを使用して調査します。

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>手順 1: 追加して、ObjectDataSource コントロールの構成

開いて開始、 `SimpleDisplay.aspx`  ページで、`BasicReporting`フォルダーで、デザイン ビューに切り替えるし、ページのデザイン画面には、ツールボックスから、ObjectDataSource コントロールをドラッグします。 すべてのマークアップが生成されないために、デザイン サーフェイス上の灰色のボックスとして、ObjectDataSource が表示されます。指定したオブジェクトからメソッドを呼び出すことによってデータだけにアクセスします。 データの GridView、DetailsView、FormView などの Web コントロールによって、ObjectDataSource によって返されるデータを表示できます。

> [!NOTE]
> またはは、最初のページに Web コントロールのデータを追加し、次に、そのスマート タグから次のように選択します。、&lt;新しいデータ ソース&gt;ドロップダウン リストからオプション。


ObjectDataSource の基になるオブジェクトとそのオブジェクトのメソッドが ObjectDataSource にどのようにマップするかを指定するには、ObjectDataSource のスマート タグからのデータ ソースの構成のリンクをクリックします。


[![クリックして、スマート タグからのデータ ソースのリンクを構成します。](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**図 3**: スマート タグから構成データ ソース リンクをクリックして ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image7.png))


データ ソース構成ウィザードが表示されます。 最初に、ObjectDataSource を使用するオブジェクトを指定する必要があります。 この画面にドロップ ダウン リストで修飾されたオブジェクトのみを一覧表示「データ コンポーネントのみを表示する」のチェック ボックスをオンにした場合、`DataObject`属性。 現在、リストには、型指定されたデータセットと前のチュートリアルで作成した BLL クラスで Tableadapter が含まれます。 追加するを忘れた場合、`DataObject`属性、ビジネス ロジック層クラスには表示されませんにこの一覧にします。 その場合は、(型指定された DataSet、Datatable、Datarow、およびなどの他のクラス) および BLL クラスを含む必要があるすべてのオブジェクトを表示する「データ コンポーネントのみを表示する」のチェック ボックスをオフにします。

この最初の画面からを選択して、`ProductsBLL`ドロップダウン リストからクラスし、[次へ] をクリックします。


[![ObjectDataSource コントロールを使用するオブジェクトを指定します。](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**図 4**: ObjectDataSource コントロールを使用するオブジェクトを指定 ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image10.png))


ウィザードの次の画面では、どのようなメソッドを呼び出す必要がある、ObjectDataSource を選択するように求められます。 ドロップダウン リストには、データを返す、前の画面から選択したオブジェクトでこれらのメソッドが一覧表示します。 次に示す`GetProductByProductID`、 `GetProducts`、 `GetProductsByCategoryID`、および`GetProductsBySupplierID`です。 選択、`GetProducts`メソッドをクリックしてドロップダウン リストから [完了] (追加した場合、`DataObjectMethodAttribute`を`ProductBLL`の前のチュートリアルでは、このオプションで示すようにメソッドが既定で選択されます)。


[![タブからデータを返すための方法を選択します。](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**図 5**: 返すデータの選択 タブからメソッドを選択 ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>ObjectDataSource を手動で構成します。

ObjectDataSource のデータ ソースの構成ウィザードを使用してオブジェクトを指定しにどのようなオブジェクトのメソッドが呼び出されるを関連付ける簡単な方法が用意されています。 [プロパティ] ウィンドウを使用するか、宣言型マークアップ内で直接、そのプロパティを介して ObjectDataSource を構成することができます、ただし、します。 設定するだけ、`TypeName`プロパティを使用するには、基になるオブジェクトの型と`SelectMethod`データを取得するときに呼び出されるメソッドにします。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

でも場合は、データ ソースの構成ウィザードの必要がある生じる場合、ObjectDataSource を手動で構成するように、ウィザードでは、開発者が作成したクラスのみ一覧表示である可能性があります。 など、.NET Framework のクラスに、ObjectDataSource をバインドする場合、[メンバーシップ クラス](https://msdn.microsoft.com/library/system.web.security.membership.aspx)、ユーザー アカウント情報にアクセスまたは[ディレクトリ クラス](https://msdn.microsoft.com/library/system.io.directory.aspx)ファイル システム情報を使用するにはObjectDataSource のプロパティを手動で設定する必要があります。

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>手順 2: データ Web コントロールを追加して、ObjectDataSource へのバインド

Web コントロールをデータに、ObjectDataSource によって返されるデータを表示するページを追加する準備ができました、ObjectDataSource がページに追加し、構成された、`Select`メソッドです。 Web コントロールのデータは、ObjectDataSource; にバインドすることができます。ObjectDataSource のデータを表示する GridView、DetailsView、および FormView を見てみましょう。

## <a name="binding-a-gridview-to-the-objectdatasource"></a>ObjectDataSource、GridView にバインド

GridView コントロールを追加する場合は、ツールボックスから`SimpleDisplay.aspx`のデザイン画面。 GridView のスマート タグの表示からには、手順 1. で追加 ObjectDataSource コントロールを選択します。 これは自動的に作成、BoundField ObjectDataSource からのデータによって返される各プロパティの GridView で`Select`メソッド (つまり、製品データ テーブルで定義されたプロパティ)。


[![GridView がページに追加されて、ObjectDataSource にバインドされていると](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**図 6**: A GridView に追加されました ページと、ObjectDataSource をバインド ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image16.png))


できますし、カスタマイズ、再配置、または削除する GridView の BoundFields スマート タグから列の編集オプションをクリックします。


[![列の編集 ダイアログ ボックスで、GridView の BoundFields を管理します。](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**図 7**: 管理 GridView の BoundFields を通じて、編集の列 ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image19.png))


GridView の BoundFields、削除の変更をとって、 `ProductID`、 `SupplierID`、 `CategoryID`、 `QuantityPerUnit`、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`BoundFields です。 単純に左下にある一覧から、BoundField を選択し、削除 ボタン (赤色の X) それらを削除する をクリックします。 BoundFields を次に、再配置できるように、`CategoryName`と`SupplierName`BoundFields の前に、 `UnitPrice` BoundField をこれら BoundFields を選択し、上矢印をクリックします。 設定、`HeaderText`プロパティに残り BoundFields の`Products`、 `Category`、 `Supplier`、および`Price`、それぞれします。 次が、 `Price` BoundField、通貨形式、BoundField を設定して`HtmlEncode`プロパティを False に、その`DataFormatString`プロパティを`{0:c}`です。 最後に、水平方向に整列、`Price`右側に、`Discontinued`経由でセンター内のチェック ボックス、 `ItemStyle` / `HorizontalAlign`プロパティです。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![GridView の BoundFields がカスタマイズされています。](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**図 8**: GridView の BoundFields はカスタマイズされている ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>一貫した外観のテーマの使用

これらのチュートリアル限りコントロール レベルのスタイル設定を削除する、カスケード スタイル シートを代わりに使用可能な限り、外部ファイルで定義されています。 `Styles.css`ファイルが含まれます`DataWebControlStyle`、 `HeaderStyle`、 `RowStyle`、および`AlternatingRowStyle`Web これらのチュートリアルで使用されるコントロールを CSS クラスのデータの外観を決定するために使用する必要があります。 GridView のためには、設定して`CssClass`プロパティを`DataWebControlStyle`、およびその`HeaderStyle`、 `RowStyle`、および`AlternatingRowStyle`プロパティの`CssClass`プロパティに応じて。

これらを設定お場合`CssClass`プロパティ、すべてのデータのこれらのプロパティ値を明示的に設定する必要があります、Web コントロールを Web コントロールのチュートリアルに追加します。 管理しやすいアプローチは、GridView、DetailsView の既定の CSS 関連のプロパティを定義して、FormView でテーマの使用を制御します。 テーマは、コントロール レベルのプロパティの設定、画像、および一般的なルック アンド フィールを適用するサイト全体でのページに適用できる CSS クラスのコレクションです。

このテーマは含まれず、イメージや CSS ファイル (スタイル シートのままに`Styles.css`として-web アプリケーションのルート フォルダーで定義されている場合は、)、2 つのスキンなどが含まれますが、します。 スキンは、Web コントロールの既定のプロパティを定義するファイルです。 具体的には、必要がある、GridView と DetailsView コントロールのスキン ファイル既定値を示す`CssClass`-関連するプロパティです。

という名前のプロジェクトに新しいスキン ファイルを追加することによって開始`GridView.skin`ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。


[![GridView.skin をという名前のスキン ファイルを追加します。](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**図 9**: スキン ファイルの名前を追加`GridView.skin`([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image25.png))


スキン ファイル内に配置されていますが、テーマに配置することが必要、`App_Themes`フォルダーです。 このようなフォルダーがあるまだありません、ため初期レプリケーションが完了するまでお待ちくださいレプリケーションに時間がかかっている場合は、初期レプリケーションを実行できるだけのネットワーク帯域幅が利用できるかどうかを確認してください、最初のスキンを追加するときにのいずれかを作成する Visual Studio が提供されます。 作成するには、[はい] をクリックして、`App_Theme`フォルダーし、新しい配置`GridView.skin`ファイルがあります。


[![App_Theme フォルダーを作成して Visual Studio を使用できます。](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**図 10**: Visual Studio で、作成、`App_Theme`フォルダー ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image28.png))


これで新しいテーマが作成されます、`App_Themes`スキン ファイルに GridView をという名前のフォルダー`GridView.skin`です。


![GridView テーマに追加された App_Theme フォルダー](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**図 11**: GridView テーマに追加されて、`App_Theme`フォルダー


DataWebControls を GridView テーマの名前を変更 (GridView フォルダーを右クリックし、`App_Theme`フォルダー名前の変更を選択) します。 次のマークアップを次に、入力、`GridView.skin`ファイル。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

既定のプロパティを定義、 `CssClass`-DataWebControls テーマを使用するすべてのページで、gridview に関連するプロパティです。 DetailsView で間もなく使用する Web コントロールのデータの別のスキンを追加してみましょう。 新しいスキンをという名前の DataWebControls テーマに追加`DetailsView.skin`し、次のマークアップを追加します。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

テーマを定義すると、最後の手順は、ASP.NET ページにテーマを適用します。 ページ単位ごとに、または web サイトのすべてのページ、テーマを適用できます。 このテーマを使用して、web サイトのすべてのページみましょう。 これを実現するには、次のマークアップを追加`Web.config`の`<system.web>`セクション。


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

すべてであることには! `styleSheetTheme`設定は、テーマで指定したプロパティが必要があることを示します*いない*コントロール レベルで指定されたプロパティをオーバーライドします。 テーマの設定がコントロールの設定をより優先する必要がありますを指定するには、使用、`theme`の代わりに属性`styleSheetTheme`; 残念ながら、を介して指定されたテーマの設定、`theme`属性は、Visual Studio のデザイン ビューでは表示されません。 参照してください[ASP.NET のテーマとスキン概要](https://msdn.microsoft.com/library/ykzx33wh.aspx)と[サーバー側のスタイルを使用してテーマ](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)テーマとスキン; について、次を参照してください[How To: ASP.NET のテーマを適用](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx)の詳細について。テーマを使用するページを構成します。


[![GridView は、製品の名前、カテゴリ、供給業者、価格、および提供が中止された情報が表示されます。](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**図 12**: GridView は、製品の名前、カテゴリ、供給業者、価格、および提供が中止された情報が表示されます ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>DetailsView で、一度に 1 つのレコードを表示します。

GridView は、バインドされているデータ ソース コントロールによって返されるレコードごとに 1 つの行を表示します。 ただし、一度に唯一のレコードまたは 1 つのレコードを表示したい場合がある場合があります。 [DetailsView コントロール](https://msdn.microsoft.com/library/s3w1w7t4.aspx)HTML としてレンダリングして、この機能を提供`<table>`2 つの列と列や、コントロールにバインドされたプロパティごとに 1 行を使用します。 DetailsView は、単一レコードに 90 度回転 GridView と考えることができます。

DetailsView コントロールを追加することによって開始*上*で GridView`SimpleDisplay.aspx`です。 次に、GridView として同じ ObjectDataSource コントロールにバインドします。 ObjectDataSource のによって返されるオブジェクト内の各プロパティの DetailsView に追加されます BoundField GridView のと同じように`Select`メソッドです。 唯一の違いは、DetailsView の BoundFields レイアウトされること水平方向にはなく、垂直方向にします。


[![DetailsView をページに追加して、ObjectDataSource へのバインド](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**図 13**: DetailsView をページに追加して、ObjectDataSource へのバインド ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image35.png))


GridView のように、ObjectDataSource によって返されるデータのより細かくカスタマイズされた表示を提供する DetailsView の BoundFields を調整できます。 図 14 は、その BoundFields 後 DetailsView と`CssClass`の外観を GridView の例のようになるプロパティが構成されています。


[![DetailsView は、1 つのレコードを示しています。](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**図 14**: DetailsView が 1 つのレコードを示しています ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image38.png))


DetailsView がそのデータ ソースによって返される最初のレコードをしか表示されないことに注意してください。 DetailsView のページングを有効におをステップ実行、すべてのレコードを 1 つずつ、ユーザーを許可します。 これを行うには、Visual Studio に戻るし、DetailsView のスマート タグの表示でページングを有効にするチェック ボックスをオンします。


[![DetailsView コントロールでページングを有効にします。](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**図 15**: DetailsView コントロールでページングを有効にする ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image41.png))


[![DetailsView ではページングを有効にすると、任意の製品を表示します。](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**図 16**: ページング有効 DetailsView では、製品のいずれかを表示するユーザー ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image44.png))


ページング将来のチュートリアルについて説明します。

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>一度に 1 つのレコードを表示するためより柔軟にレイアウト

DetailsView では、ObjectDataSource から返された各レコードをどのように表示にかなり rigid です。 データのより柔軟なビューを必要があります。 たとえば、製品の名前、カテゴリ、供給業者、価格、および各提供が中止された情報が表示される個別の行に、代わりすることも、製品名を表示してでの価格、`<h4>`見出しで表示される情報は、カテゴリおよび仕入先フォント サイズの価格と名前の下。 およびお可能性があります、値の横にあるプロパティ名 (製品、カテゴリ、およびなど) を表示する気にしません。

[FormView コントロール](https://msdn.microsoft.com/library/fyf1dk77.aspx)このレベルのカスタマイズを提供します。 FormView フィールド (のように、GridView と DetailsView) を使用するのではなくを静的な HTML の Web コントロールの混在を使用できるテンプレートを使用して、 [databinding 構文](http://www.15seconds.com/issue/040630.htm)です。 リピータ コントロール ASP.NET から慣れているかどうかは 1.x を考えることができます、FormView リピータの 1 つのレコードを表示します。

追加するコントロールをフォーム ビュー、`SimpleDisplay.aspx`ページのデザイン画面。 最初に FormView 表示灰色のブロックと制御するために、少なくともの必要があるかを知らせる`ItemTemplate`です。


[![FormView する必要がありますが、ItemTemplate を含める](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**図 17**: FormView を含める必要がありますが、 `ItemTemplate` ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image47.png))


FormView をバインドするには、既定値を作成フォーム ビューのスマート タグからのデータ ソース コントロールに直接`ItemTemplate`自動的に (と共に、`EditItemTemplate`と`InsertItemTemplate`場合は、ObjectDatatSource コントロールの`InsertMethod`と`UpdateMethod`プロパティが設定されます)。 ただし、この例でみましょう FormView にデータをバインドし、指定の`ItemTemplate`手動でします。 FormView に設定して開始`DataSourceID`プロパティを`ID`ObjectDataSource コントロールの`ObjectDataSource1`します。 次に、作成、`ItemTemplate`製品の名前と価格を表示するよう、`<h4>`要素およびフォント サイズの下にはカテゴリや出荷業者の名前。


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![最初の製品 (Chai) がカスタム形式で表示されます。](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**図 18**: カスタム形式で、最初の製品 (Chai) が表示されます ([フルサイズのイメージを表示するをクリックして](displaying-data-with-the-objectdatasource-cs/_static/image50.png))


`<%# Eval(propertyName) %>` Databinding 構文です。 `Eval`メソッド FormView コントロールにバインドされている現在のオブジェクトの指定したプロパティの値を返します。 Alex Homer の記事をご覧[簡体字と ASP.NET 2.0 でのデータ バインド構文の拡張](http://www.15seconds.com/issue/040630.htm)のデータ バインドの詳細については、その中身について学習します。

FormView は DetailsView と同様に、最初に、ObjectDataSource から返されるレコードのみ表示されます。 一度に 1 つの製品間ステップへの訪問者を許可する FormView でページングを有効にすることができます。

## <a name="summary"></a>まとめ

アクセスして、ビジネス ロジック層からデータを表示する、行の ObjectDataSource コントロールの ASP.NET 2.0 のコードを記述することがなく実現できます。 ObjectDataSource はクラスの指定したメソッドを呼び出すし、結果を返します。 これらの結果は、データ、ObjectDataSource にバインドされている Web コントロールを表示できます。 このチュートリアルでは、ObjectDataSource への GridView、DetailsView、フォーム ビューの各コントロールのバインドについて説明しました。

これまでのみ見たパラメーターのないメソッドを呼び出すため、ObjectDataSource を使用する方法が、どうしたらよいものと想定するメソッドを呼び出すための入力など、パラメーター、`ProductBLL`クラスの`GetProductsByCategoryID(categoryID)`しますか? 1 つまたは複数のパラメーターを必要とするメソッドを呼び出すためには、これらのパラメーターの値を指定する ObjectDataSource を構成お必要があります。 これを実現する方法をお見せ、[次のチュートリアル](declarative-parameters-cs.md)です。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [独自のデータ ソース コントロールを作成します。](https://msdn.microsoft.com/library/ms364049.aspx)
- [ASP.NET 2.0 の GridView の例](https://msdn.microsoft.com/library/aa479339.aspx)
- [簡素化および拡張構文を ASP.NET 2.0 でバインド データ](http://www.15seconds.com/issue/040630.htm)
- [ASP.NET 2.0 のテーマ](http://www.odetocode.com/Articles/423.aspx)
- [テーマを使用してサーバー側のスタイル](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [方法: プログラムによって ASP.NET のテーマを適用](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Hilton Giesenow しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](declarative-parameters-cs.md)
