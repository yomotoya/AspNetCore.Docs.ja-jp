---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: カスタム データベース駆動型サイト マップ プロバイダー (VB) の構築 |Microsoft Docs
author: rick-anderson
description: ASP.NET 2.0 の既定のサイト マップ プロバイダーでは、静的な XML ファイルからのデータを取得します。 XML ベースのプロバイダーは多くの小規模および中規模 siz に適した.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e041a5a9163c7f9fe55c6aa06f35301cbdb48a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393971"
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>カスタム データベース駆動型サイト マップ プロバイダー (VB) のビルド
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip)または[PDF のダウンロード](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> ASP.NET 2.0 の既定のサイト マップ プロバイダーでは、静的な XML ファイルからのデータを取得します。 XML ベースのプロバイダーは、多くの中小規模の Web サイトに適している、大規模な Web アプリケーションをより動的サイト マップが必要です。 このチュートリアルでは、ビジネス ロジック層からのデータを取得するカスタム サイト マップ プロバイダーを構築でをさらに、データベースからデータを取得します。


## <a name="introduction"></a>はじめに

ASP.NET 2.0 のサイト マップ機能により、XML ファイルなど、いくつかの永続的な中で web アプリケーションのサイト マップを定義するページの開発者。 サイト マップ データを使用したプログラムでアクセスできますを定義した後、 [ `SiteMap`クラス](https://msdn.microsoft.com/library/system.web.sitemap.aspx)で、 [ `System.Web`名前空間](https://msdn.microsoft.com/library/system.web.aspx)など、さまざまなナビゲーション Web コントロール、または、SiteMapPath、メニューのおよび TreeView コントロール。 サイト マップのシステムを使用して、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)別のサイト マップのシリアル化の実装を作成して、web アプリケーションに接続できるようにします。 ASP.NET 2.0 に付属する既定のサイト マップ プロバイダーでは、XML ファイル内のサイト マップ構造を保持します。 戻り、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-vb.md)という名前のファイルを作成したチュートリアル`Web.sitemap`をこの構造体に含まれているし、新しい各チュートリアル セクションでその XML を更新されました。

既定の XML ベースのサイト マップ プロバイダーは、これらのチュートリアルについては、サイト マップ構造がなどの比較的静的な場合にも動作します。 多くのシナリオより動的なサイト マップが必要です。 各カテゴリと製品を web サイトの構造内のセクションとして表示される、図 1 に示すサイト マップを検討してください。 このサイト マップを使用したルート ノードに対応する web ページにアクセス可能性がありますすべてを一覧表示、カテゴリの特定のカテゴリの web ページにアクセスするとそのカテゴリの製品のリストし、特定の製品の web ページの表示はその製品 s の詳細を表示します。


[![カテゴリおよび製品の構成サイト マップの構造](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**図 1**:、カテゴリおよび製品の構成サイト マップの構造 ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))。


このカテゴリおよび製品に基づく構造体にハード コードされた可能性がありますが、`Web.sitemap`ファイル、ファイルは、カテゴリのたびに更新する必要がありますまたは製品が追加、削除、または名前を変更します。 その結果、サイト マップの保守が大幅に簡略化、データベースから、または、理想的には、アプリケーションのアーキテクチャのビジネス ロジック層から、その構造が取得された場合。 これにより、製品およびカテゴリが追加されるため、名前の変更、または削除された、サイト マップが自動的にこれらの変更を反映するように更新します。

ASP.NET 2.0 のサイト マップのシリアル化はプロバイダー モデルの上に構築される、ので、データベースやアーキテクチャなど、代替データ ストアからデータを取得する独自のカスタム サイト マップ プロバイダーを作成できます。 このチュートリアルでは、BLL からそのデータを取得するカスタム プロバイダーを作成します。 Let s を始めましょう。

> [!NOTE]
> このチュートリアルで作成したカスタム サイト マップ プロバイダーは、アプリケーションのアーキテクチャとデータ モデルに密結合します。 Jeff Prosise s [SQL Server のサイト マップを格納する](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)と[SQL サイト マップ プロバイダーを ve 待望の](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)記事は、SQL Server にサイト マップ データを格納する汎用化されたアプローチを確認します。


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>手順 1: カスタム サイト マップ プロバイダーの Web ページを作成します。

カスタム サイト マップ プロバイダーの作成を始める前に、まずこのチュートリアルの必要があります、ASP.NET ページを追加する秒を使用できます。 という名前の新しいフォルダーを追加することで開始`SiteMapProvider`します。 次に、次の ASP.NET ページを使用する各ページに関連付けることを確認、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

追加も、`CustomProviders`には、サブフォルダー、`App_Code`フォルダー。


![サイト マップ プロバイダーに関連するチュートリアルについては、ASP.NET ページを追加します。](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**図 2**: 追加のサイトの ASP.NET ページ マップ プロバイダーに関連するチュートリアル


必要ありません、このセクションのチュートリアルを 1 つだけであるため`Default.aspx`をセクションのチュートリアルを一覧表示します。 代わりに、 `Default.aspx` GridView コントロールで、カテゴリが表示されます。 これは、手順 2. で取り組むします。

次に、更新`Web.sitemap`への参照を含める、`Default.aspx`ページ。 具体的には、次のマークアップを追加後、キャッシュ`<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューで、唯一のサイト マップ プロバイダー チュートリアルでは、項目できるようになりました。


![サイト マップ サイト マップ プロバイダー チュートリアルでは、エントリになりました](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**図 3**: サイト マップ サイト マップ プロバイダー チュートリアルでは、エントリになりました


このチュートリアルの重点では、カスタム サイト マップ プロバイダーを作成して、そのプロバイダーを使用する web アプリケーションの構成を示しています。 具体的には、図 1 に示すように、カテゴリと製品ごとのノードとルート ノードを含むサイト マップを返すプロバイダーを作成します。 一般に、サイト マップ内の各ノードは、URL を指定することがあります。 ルート ノードの URL がある、サイト マップの`~/SiteMapProvider/Default.aspx`データベース内のカテゴリをすべて一覧します。 サイト マップ内の各カテゴリ ノードを指す URL になります`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`、指定した製品をすべて一覧する*categoryID*します。 最後に、各製品のサイト マップ ノードがポイントして`~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`、特定の製品の詳細が表示されます。

開始するを作成する必要があります、 `Default.aspx`、 `ProductsByCategory.aspx`、および`ProductDetails.aspx`ページ。 これらのページは、それぞれ 2、3、および 4 の手順で完了します。 過去チュートリアル説明を作成するためと、サイト マップ プロバイダーが、このチュートリアルのためこのような複数ページのマスター/詳細を報告は急げばから手順 2 4 からです。 複数のページのマスター/詳細レポートを作成する場合は、参照戻り、[マスター/詳細フィルター間で 2 つのページ](../masterdetail/master-detail-filtering-across-two-pages-vb.md)チュートリアル。

## <a name="step-2-displaying-a-list-of-categories"></a>手順 2: カテゴリの一覧を表示します。

開く、`Default.aspx`ページで、`SiteMapProvider`フォルダーと、デザイナーの設定には、ツールボックスからドラッグ GridView をその`ID`に`Categories`します。 GridView のスマート タグからという名前の新しい ObjectDataSource にバインド`CategoriesDataSource`を使用してそのデータを取得するように構成し、`CategoriesBLL`クラスの`GetCategories`メソッド。 この GridView はだけカテゴリが表示されますおよび、データ変更の機能を提供しません、ためは、UPDATE、INSERT でドロップダウン リストを設定し、(None) にタブを削除します。


[![ObjectDataSource を返すメソッドを使用してカテゴリを構成します。](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**図 4**: 返すカテゴリを使用する ObjectDataSource を構成、`GetCategories`メソッド ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))。


[![UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**図 5**: (None) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))。


データ ソース構成ウィザードを完了すると、Visual Studio が、BoundField 用に追加`CategoryID`、 `CategoryName`、 `Description`、 `NumberOfProducts`、および`BrochurePath`します。 のみが含まれるように、GridView を編集、`CategoryName`と`Description`BoundFields し、更新、 `CategoryName` BoundField の`HeaderText`プロパティをカテゴリ。

次に、内を追加し、そのために配置する s 一番左のフィールド。 設定、`DataNavigateUrlFields`プロパティを`CategoryID`と`DataNavigateUrlFormatString`プロパティを`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`します。 設定、`Text`製品を表示するプロパティ。


![カテゴリの GridView 内に追加します。](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**図 6**: を内の追加、 `Categories` GridView


ObjectDataSource を作成して、GridView のフィールドをカスタマイズすると、2 つのコントロールの宣言型マークアップを次のようになります。


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

図 7 は`Default.aspx`とき、ブラウザーで表示します。 カテゴリの製品の表示をクリックするとリンクするのに`ProductsByCategory.aspx?CategoryID=categoryID`、手順 3. で作成しました。


[![各カテゴリは、ビューの製品リンクを含むに沿って表示](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**図 7**: ビューの製品リンクを含むに沿って表示されている各カテゴリは、([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))。


## <a name="step-3-listing-the-selected-category-s-products"></a>手順 3: 選択したカテゴリ、製品を一覧表示します。

開く、`ProductsByCategory.aspx`ページし、その名前を付け、GridView を追加`ProductsByCategory`します。 という名前の新しい ObjectDataSource を GridView にバインド、スマート タグから`ProductsByCategoryDataSource`します。 構成を使用する ObjectDataSource、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドと、ドロップダウン リストを UPDATE、INSERT、および DELETE タブ (None) に一覧表示します。


[![ProductsBLL クラスの GetProductsByCategoryID(categoryID) メソッドを使用します。](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**図 8**: 使用して、`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))。


データ ソース構成ウィザードの最後の手順のパラメーターのソースの入力を求める*categoryID*します。 この情報はクエリ文字列フィールドを介して渡されるため`CategoryID`、ドロップダウン リストからクエリ文字列を選択し、図 9 に示すようには、QueryStringField ボックスには、CategoryID を入力します。 ウィザードを完了するには、[完了] をクリックします。


[![CategoryID パラメーターの使用、CategoryID Querystring フィールド](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**図 9**: 使用して、`CategoryID`のクエリ文字列フィールド、 *categoryID*パラメーター ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))。


ウィザードの完了後は、Visual Studio は製品データ フィールドで、GridView にも、対応する BoundFields と、CheckBoxField に追加されます。 削除以外のすべて、 `ProductName`、 `UnitPrice`、および`SupplierName`BoundFields します。 これら 3 つ BoundFields カスタマイズ`HeaderText`製品、価格、および、サプライヤーをそれぞれ読み取るプロパティ。 形式、 `UnitPrice` BoundField を通貨として。

次に、内を追加し、左端の位置に移動します。 設定の`Text`詳細を表示、プロパティ、`DataNavigateUrlFields`プロパティを`ProductID`とその`DataNavigateUrlFormatString`プロパティを`~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`します。


![ProductDetails.aspx を指すビューの詳細内を追加します。](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**図 10**: ビューの詳細を指す内の追加 `ProductDetails.aspx`


これらのカスタマイズを行った後 GridView コントロールと ObjectDataSource s の宣言型マークアップが、次のようになります。


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

表示に戻る`Default.aspx`飲み物のリンクをブラウザーや製品の表示をクリックします。 これは、移動`ProductsByCategory.aspx?CategoryID=1`飲み物のカテゴリに属している Northwind データベースの名前、価格、および製品の仕入先を表示する (図 11 を参照してください)。 自由にユーザーをカテゴリの一覧ページに戻るリンクを含めるには、このページをさらに強化 (`Default.aspx`) および DetailsView または FormView コントロールを選択したカテゴリの名前と説明が表示されます。


[![飲み物の名前、価格、および仕入先が表示されます。](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**図 11**:、飲み物の名前、価格、および仕入先が表示されます ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))。


## <a name="step-4-showing-a-product-s-details"></a>手順 4: 製品の詳細情報を表示します。

最後のページ`ProductDetails.aspx`、選択した製品の詳細が表示されます。 開いている`ProductDetails.aspx`DetailsView をツールボックスからデザイナーにドラッグします。 DetailsView s を設定`ID`プロパティを`ProductInfo`クリアとその`Height`と`Width`プロパティの値。 スマート タグ、DetailsView をという名前の新しい ObjectDataSource にバインド`ProductDataSource`、構成からそのデータをプルする ObjectDataSource、`ProductsBLL`クラスの`GetProductByProductID(productID)`メソッド。 2. および手順 3. で作成された前の web ページと同様で、UPDATE、INSERT、ドロップダウン リストを設定し、(なし) タブを削除します。


[![ObjectDataSource GetProductByProductID(productID) メソッドを使用して構成します。](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**図 12**: 構成に使用する ObjectDataSource、`GetProductByProductID(productID)`メソッド ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))。


データ ソース構成ウィザードの最後の手順は、元のメッセージが表示されます、 *productID*パラメーター。 このデータのクエリ文字列フィールドを取得するため`ProductID`、クエリ文字列と ProductID する QueryStringField テキスト ボックスに、ドロップダウン リストを設定します。 最後に、ウィザードを完了するには、[完了] をクリックします。


[![ProductID ProductID のクエリ文字列フィールドからその値を取得するパラメーターを構成します。](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**図 13**: 構成、 *productID*からその値を取得するパラメーター、 `ProductID` Querystring フィールド ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))。


データ ソースの構成ウィザードの完了後は、Visual Studio は製品データ フィールドで、DetailsView でも、対応する BoundFields と、CheckBoxField に作成されます。 削除、 `ProductID`、 `SupplierID`、および`CategoryID`BoundFields し、必要に応じてその他のフィールドを構成します。 少数の見た目の構成の後に、DetailsView、ObjectDataSource s 宣言型マークアップは、次のような検索。


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

このページをテストするに戻る`Default.aspx`飲み物のカテゴリの製品の表示をクリックします。 飲み物の製品の一覧については、Chai 紅茶の詳細を表示するリンクをクリックします。 これは、移動`ProductDetails.aspx?ProductID=1`、Chai 紅茶 s (図 14 を参照してください) の詳細を表示します。


[![Chai 紅茶 s 仕入先、カテゴリ、価格、およびその他の情報が表示されます。](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**図 14**: Chai 紅茶 s 仕入先、カテゴリ、価格、およびその他の情報が表示されます ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))。


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>手順 5: サイト マップ プロバイダーの内部動作を理解します。

サイト マップは、web サーバーのメモリ内のコレクションとして表される`SiteMapNode`階層を形成するインスタンス。 正確に 1 つのルートである必要があります、ルート以外のすべてのノードは 1 つの親ノードが必要し、すべてのノードが子の任意の数を必要があります。 各`SiteMapNode`オブジェクトは、web サイトの構造体のセクションを表します。 これらのセクションでは、対応する web ページをよくがあります。 その結果、 [ `SiteMapNode`クラス](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)などのプロパティを持つ`Title`、 `Url`、および`Description`、セクションの情報を提供する、`SiteMapNode`を表します。 `Key`プロパティそれぞれを一意に識別する`SiteMapNode`階層と、この階層を確立するために使用されるプロパティで`ChildNodes`、 `ParentNode`、 `NextSibling`、`PreviousSibling`となります。

図 15 より詳細に側実装の詳細が、図 1 からの全般的なサイト マップ構造を示します。


[![各 SiteMapNode がプロパティのようにタイトル、Url、キー、およびなど](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**図 15**: 各`SiteMapNode`プロパティなどが`Title`、 `Url`、`Key`など ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))。


サイト マップが使用してアクセスできますが、 [ `SiteMap`クラス](https://msdn.microsoft.com/library/system.web.sitemap.aspx)で、 [ `System.Web`名前空間](https://msdn.microsoft.com/library/system.web.aspx)します。 このクラスは s`RootNode`プロパティは、サイト マップのルートを返します`SiteMapNode`インスタンス。`CurrentNode`を返します、`SiteMapNode`が`Url`現在要求されているページの URL と一致するプロパティ。 このクラスは、ASP.NET 2.0 のナビゲーション Web コントロールによって内部的に使用されます。

ときに、`SiteMap`クラスのプロパティにアクセス、メモリにいくつかの永続的なメディアからサイト マップ構造がシリアル化する必要があります。 ただし、サイト マップのシリアル化のロジックは難しくありませんにコード化された、`SiteMap`クラス。 代わりに、実行時に、`SiteMap`クラスは、どのサイト マップを決定します。*プロバイダー*シリアル化に使用します。 既定で、 [ `XmlSiteMapProvider`クラス](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)を使用するサイト マップの構造を適切にフォーマットされた XML ファイルから読み取ります。 ただし、若干の開発では、独自のカスタム サイト マップ プロバイダーを作成することができます。

すべてのサイト マップ プロバイダーを派生する必要があります、 [ `SiteMapProvider`クラス](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx)重要なメソッドを含む、およびサイトに必要なプロパティは、プロバイダーをマップが、実装の詳細の多くを省略します。 2 番目のクラスを[ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx)、拡張、`SiteMapProvider`クラスし、必要な機能のより堅牢に実装が含まれています。 内部的には、`StaticSiteMapProvider`格納、`SiteMapNode`のマップに、サイトのインスタンスを`Hashtable`などのメソッドを提供し、 `AddNode(child, parent)`、`RemoveNode(siteMapNode),`と`Clear()`を追加および削除`SiteMapNode`内部に`Hashtable`します。 `XmlSiteMapProvider` は、`StaticSiteMapProvider` から派生しています。

拡張するカスタム サイト マップ プロバイダーを作成するときに`StaticSiteMapProvider`、2 つの抽象メソッドをオーバーライドする必要があります: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx)と[ `GetRootNodeCore`](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx)します。 `BuildSiteMap`、その名前が示すように、は永続的なストレージからのサイト マップ構造体の読み込みと、メモリ内の構築を担当します。 `GetRootNodeCore` サイト マップのルート ノードを返します。

Web の前に、アプリケーションは、アプリケーションの構成で登録されている必要がありますが、サイト マップ プロバイダーを使用できます。 既定で、`XmlSiteMapProvider`クラスが名前で登録されている`AspNetXmlSiteMapProvider`します。 追加のサイト マップ プロバイダーを登録するには、次のマークアップを追加`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

*名前*値が中にプロバイダーに人間が判読できる名前を割り当てます*型*サイト マップ プロバイダーの完全修飾型名を指定します。 具体的な値について説明します、*名前*と*型*手順 7 では、値された後、カスタム サイト マップ プロバイダーを作成しました。

サイト マップ プロバイダーのクラスからアクセスされて初めてがインスタンス化、`SiteMap`クラスとは、web アプリケーションの有効期間にわたってメモリに残ります。 プロバイダーのメソッドである必要が、複数の同時実行の web サイトの訪問者から呼び出すことができるサイト マップ プロバイダーのインスタンスを 1 つだけなので、*スレッド セーフな*します。

パフォーマンスとスケーラビリティの理由からメモリ内のサイトをキャッシュすることが重要ですが構造をマップし、これはキャッシュたびに再作成するのではなく、構造体を返す、`BuildSiteMap`メソッドが呼び出されます。 `BuildSiteMap` 呼び出すことは複数回使用ページおよびサイト マップ構造の深さのナビゲーション コントロールによって、ユーザーごとのページ要求ごと。 いかなる場合において、私たちのサイト マップ構造をキャッシュしない場合`BuildSiteMap`が呼び出されるたびに私たちがする必要があります (これはデータベースにクエリの結果) アーキテクチャから製品と分類の情報を再取得します。 キャッシュの前のチュートリアルで説明したように、キャッシュされたデータが古くなることができます。 この時間、または SQL キャッシュ依存関係に基づく切れを使えばことができます。

> [!NOTE]
> サイト マップ プロバイダーが必要に応じてオーバーライドして、 [ `Initialize`メソッド](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx)します。 `Initialize` サイト マップ プロバイダーが最初にインスタンス化されでプロバイダーに割り当てられているカスタム属性に渡されるときに呼び出される`Web.config`で、`<add>`のような要素:`<add name="name" type="type" customAttribute="value" />`します。 プロバイダーのコードを変更することがなくさまざまなサイト マップ プロバイダーに関連する設定を指定するページの開発者を許可する場合に便利です。 たとえば、ページの開発者からデータベース接続文字列を指定できるようにする場合は、カテゴリおよび製品のデータを可能性があります d、アーキテクチャではなく、データベースから直接読み取ることがする`Web.config`ハード コーディングされたを使用してではなくプロバイダーのコード内の値。 手順 6 でビルドしますカスタム サイト マップ プロバイダーがこれをオーバーライドしません`Initialize`メソッド。 使用する例については、`Initialize`メソッドを参照してください[Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [SQL Server のサイト マップを格納する](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)記事。


## <a name="step-6-creating-the-custom-site-map-provider"></a>手順 6: カスタム サイト マップ プロバイダーを作成します。

カテゴリと製品の Northwind データベースからサイト マップを構築するカスタム サイト マップ プロバイダーを作成する必要がありますを拡張するクラスを作成する`StaticSiteMapProvider`します。 手順 1. で質問をしたを追加する、`CustomProviders`フォルダーで、`App_Code`フォルダー - という名前のこのフォルダーに新しいクラスを追加`NorthwindSiteMapProvider`します。 `NorthwindSiteMapProvider` クラスに次のコードを追加します。


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

S がこのクラス s の調査を開始できるように`BuildSiteMap`メソッドで始まる、 [ `lock`ステートメント](https://msdn.microsoft.com/library/c5kehkcz.aspx)します。 `lock`ステートメントがそのコードへのアクセスをシリアル化して、2 つの同時実行スレッドが互い s 生まれたでステップ実行するを防ぐため、入力するには、一度に 1 つのスレッドがのみが許可されます。

クラス レベル`SiteMapNode`変数`root`サイト マップ構造のキャッシュに使用します。 最初に、または基になるデータが変更された後に初めてのサイト マップが作成されるときに`root`なります`Nothing`られサイト マップ構造が作成されます。 サイト マップのルート ノードが割り当てられている`root`構築中にプロセスようにその次回は、このメソッドが呼び出される、`root`されません`Nothing`します。 その結果、限り`root`ない`Nothing`サイト マップ構造が再作成しなくても、呼び出し元に返されます。

ルートがある場合`Nothing`、商品カテゴリ、および情報からサイト マップ構造が作成されます。 サイト マップが作成して構築された、`SiteMapNode`インスタンスとの呼び出しを通じて、階層を形成し、`StaticSiteMapProvider`クラスの`AddNode`メソッド。 `AddNode` さまざまなを格納する、内部ブックキーピングを実行します`SiteMapNode`インスタンス、`Hashtable`します。 まず、階層の構築を始める前に、呼び出すことによって、`Clear`メソッド内部から要素を消去する`Hashtable`します。 次に、`ProductsBLL`クラス s`GetProducts`メソッドおよび結果`ProductsDataTable`ローカル変数に格納されます。

ルート ノードを作成してに割り当てることでは、サイト マップ s を構築、まず`root`します。 オーバー ロード、 [ `SiteMapNode`コンス トラクター](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx)ここで、このために使用`BuildSiteMap`次の情報が渡されます。

- サイト マップ プロバイダーへの参照 (`Me`)。
- `SiteMapNode` S`Key`します。 これは、必須の値は、それぞれに一意である必要があります`SiteMapNode`します。
- `SiteMapNode` S`Url`します。 `Url` 省略可能で指定されている場合、それぞれが`SiteMapNode`s`Url`値は一意である必要があります。
- `SiteMapNode` S `Title`、これが必要です。

`AddNode(root)`メソッドの呼び出しを追加、 `SiteMapNode` `root`ルートとしてサイト マップにします。 次に、各`ProductRow`で、`ProductsDataTable`列挙されます。 場合が既に存在する、`SiteMapNode`現在の製品のカテゴリが参照されます。 それ以外の場合、新しい`SiteMapNode`カテゴリが作成され、追加の子として、`SiteMapNode``root`を通じて、`AddNode(categoryNode, root)`メソッドの呼び出し。 適切なカテゴリの後に`SiteMapNode`ノードが検出されたか、作成、`SiteMapNode`が、現在の製品の作成され、カテゴリの子として追加`SiteMapNode`を介して`AddNode(productNode, categoryNode)`します。 なお、カテゴリ`SiteMapNode`s`Url`プロパティの値が`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`製品の中に`SiteMapNode`s`Url`プロパティが割り当てられている`~/SiteMapNode/ProductDetails.aspx?ProductID=productID`します。

> [!NOTE]
> データベースがあるこれらの製品`NULL`の値、`CategoryID`カテゴリでグループ化された`SiteMapNode`が`Title`None を持つプロパティが`Url`プロパティが空の文字列に設定します。 設定することにしました`Url`から空の文字列に、`ProductBLL`クラス s`GetProductsByCategory(categoryID)`メソッドを返すには、これらの製品と機能を現在がない、 `NULL` `CategoryID`値。 また、ナビゲーション コントロールのレンダリング方法を示すたい、`SiteMapNode`の値が不足しているその`Url`プロパティ。 このチュートリアルを拡張することをお勧めように"なし" `SiteMapNode` s`Url`プロパティが指す`ProductsByCategory.aspx`の製品をまだのみが表示されます`NULL``CategoryID`値。


サイト マップを構築するには後で SQL キャッシュ依存関係を使用してデータのキャッシュに任意のオブジェクトが追加、`Categories`と`Products`テーブルを介して、`AggregateCacheDependency`オブジェクト。 SQL キャッシュ依存関係を使用して、前のチュートリアルで詳しく見てきました*を使用して SQL キャッシュ依存関係*します。 カスタム サイト マップ プロバイダー、ただし、データ キャッシュ s のオーバー ロードを使用する`Insert`メソッド私たちを探索するには、まだ ve します。 このオーバー ロードは、その最後の入力パラメーターとしてオブジェクトがキャッシュから削除されたときに呼び出されるデリゲートを受け取ります。 具体的には、新しい渡します[`CacheItemRemovedCallback`委任](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx)を指す、`OnSiteMapChanged`メソッド定義の下、`NorthwindSiteMapProvider`クラス。

> [!NOTE]
> サイト マップのメモリ内表現がクラス レベル変数を通じてキャッシュ`root`します。 そのインスタンスは、web アプリケーションですべてのスレッド間で共有されるため、カスタム サイト マップ プロバイダー クラスのインスタンスを 1 つだけなので、このクラスの変数は、キャッシュとして機能します。 `BuildSiteMap`メソッドは、データベースでデータを基になるときに通知を受信するための手段としてのみ、データ キャッシュを使用するも、`Categories`または`Products`テーブルを変更します。 データ キャッシュに格納する値が現在の日付と時刻だけのことに注意してください。 実際のサイト マップ データが*いない*データ キャッシュに格納します。


`BuildSiteMap`メソッドは、サイト マップのルート ノードを返すことによって完了します。

残りのメソッドは、非常に簡単です。 `GetRootNodeCore` ルート ノードを返すことを担当します。 `BuildSiteMap` 、ルートを返します`GetRootNodeCore`を単純に返します`BuildSiteMap`戻り値の値。 `OnSiteMapChanged`メソッド セット`root`に`Nothing`キャッシュ項目が削除されます。 ルートのセットに戻すと`Nothing`、次回`BuildSiteMap`が呼び出されると、サイト マップ構造が再構築されます。 最後に、`CachedDate`プロパティは、このような値が存在する場合に、データ キャッシュに格納されている日付と時刻の値を返します。 このプロパティは、サイト マップ データが最後にキャッシュされているかを判断するページの開発者が使用できます。

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>手順 7: 登録します`NorthwindSiteMapProvider`

Web アプリケーションを使用するために、`NorthwindSiteMapProvider`手順 6 で作成したサイト マップ プロバイダーに登録する必要があります、`<siteMap>`の`Web.config`します。 具体的には、内の次のマークアップを追加、`<system.web>`要素`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

このマークアップは次の 2 つ: ことを示して最初に、組み込み`AspNetXmlSiteMapProvider`既定のサイト マップ プロバイダーは、次に、Northwind のわかりやすい名前を手順 6 で作成したカスタム サイト マップ プロバイダーを登録します。

> [!NOTE]
> サイト マップ プロバイダーが、アプリケーション内にある`App_Code`の値は、フォルダー、`type`属性はクラス名だけです。 または、カスタム サイト マップ プロバイダーでしたで作成された別のクラス ライブラリ プロジェクト、web アプリケーション内に配置するコンパイル済みアセンブリで`/Bin`ディレクトリ。 その場合は、`type`属性の値になります*Namespace*.*ClassName*、 *AssemblyName*します。


更新した後`Web.config`、時間、ブラウザーで、チュートリアルから任意のページを表示するのにはかかりません。 定義されているチュートリアルを左側のナビゲーション インターフェイスのセクションを示したまま`Web.sitemap`します。 これは、ので`AspNetXmlSiteMapProvider`として既定のプロバイダー。 使用するナビゲーション ユーザー インターフェイス要素を作成するには、 `NorthwindSiteMapProvider`Northwind のサイト マップ プロバイダーを使用することを明示的に指定する必要があります。 手順 8. でこれを実現する方法を見ていきます。

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>手順 8: カスタム サイト マップ プロバイダーを使用して、サイト マップ情報を表示します。

カスタム サイト マップ プロバイダーが作成されに登録されている`Web.config`、ナビゲーション コントロールを追加する準備ができたら、 `Default.aspx`、 `ProductsByCategory.aspx`、および`ProductDetails.aspx`ページで、`SiteMapProvider`フォルダー。 開いて開始、`Default.aspx`ページし、ドラッグ、`SiteMapPath`ツールボックスからデザイナーにします。 SiteMapPath コントロールはツールボックスの [ナビゲーション] セクションにあります。


[![Default.aspx に、SiteMapPath を追加します。](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**図 16**: 追加する、SiteMapPath `Default.aspx` ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))。


SiteMapPath コントロールでは、サイト マップ内の現在のページの場所を示す、階層リンクが表示されます。 マスター ページの上部に、SiteMapPath を追加しましたに戻り、*マスター ページとサイト ナビゲーション*チュートリアル。

ブラウザーからこのページを表示する時間がかかります。 図 16 で追加された SiteMapPath がそのデータを抽出、既定のサイト マップ プロバイダーを使用して`Web.sitemap`します。 そのため、階層リンクを示しますホーム&gt;右上隅にある階層リンクと同じように、サイト マップをカスタマイズします。


[![階層リンクは、既定のサイト マップ プロバイダーを使用します。](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**図 17**: 階層リンクは、既定のサイト マップ プロバイダーを使用して ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))。


手順 6 で作成したカスタム サイト マップ プロバイダーを使用して、SiteMapPath 図 16 に追加するのには、次のように設定します。 その[`SiteMapProvider`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx)を Northwind に名前に割り当てた、`NorthwindSiteMapProvider`で`Web.config`します。 残念ながら、デザイナーは、引き続き既定のサイト マップ プロバイダーを使用するが、このプロパティの変更を行った後、ブラウザーでページにアクセスする場合、階層リンクは、カスタム サイト マップ プロバイダーを今すぐ使用が表示されます。


[![階層リンクは、カスタム サイト マップ プロバイダー NorthwindSiteMapProvider を使うようになりました](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**図 18**: 階層リンクは、カスタム サイト マップ プロバイダーを使用`NorthwindSiteMapProvider`([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))。


機能的なユーザー インターフェイスを表示する SiteMapPath コントロール、`ProductsByCategory.aspx`と`ProductDetails.aspx`ページ。 これらのページは、設定、SiteMapPath を追加、`SiteMapProvider`を Northwind に両方のプロパティ。 `Default.aspx`クリック飲み物、製品の表示のリンクをし Chai 紅茶の詳細を表示するリンクをクリックします。 図 19 に示すよう、現在のサイト マップ セクション (Chai 紅茶) とその先祖が階層リンクにはで含まれています: Beverages とすべてのカテゴリ。


[![階層リンクは、カスタム サイト マップ プロバイダー NorthwindSiteMapProvider を使うようになりました](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**図 19**: 階層リンクは、カスタム サイト マップ プロバイダーを使用`NorthwindSiteMapProvider`([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))。


メニューを TreeView コントロールなど、SiteMapPath だけでなく、他のナビゲーション ユーザー インターフェイス要素を使用できます。 `Default.aspx`、 `ProductsByCategory.aspx`、および`ProductDetails.aspx`このチュートリアル用のダウンロード ページすべてメニューにコントロールがあります (図 20 を参照してください)。 参照してください[ASP.NET 2.0 の検査 s サイト ナビゲーション機能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)と[サイト ナビゲーション コントロールを使用した](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx)のセクション、 [ASP.NET 2.0 クイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/)詳細については、ナビゲーション コントロールとサイトは、ASP.NET 2.0 のシステムをマップします。


[![メニュー コントロールでは、各カテゴリと製品の一覧表示します。](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**図 20**:、メニュー コントロールの一覧各カテゴリと製品の ([フルサイズの画像を表示する をクリックします](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))。


このチュートリアルの前半で説明したように、サイト マップ構造にアクセスできますでプログラムによって、`SiteMap`クラス。 次のコードは、ルートを返します`SiteMapNode`の既定のプロバイダー。


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

以降、`AspNetXmlSiteMapProvider`は既定のプロバイダーのアプリケーションでは、上記のコードで定義されているルート ノードを返します`Web.sitemap`します。 既定以外のサイト マップ プロバイダーを参照するには、使用、`SiteMap`クラス s [ `Providers`プロパティ](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx)ようになります。


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

場所*名前*(Northwind の web アプリケーションに) カスタム サイト マップ プロバイダーの名前を指定します。

サイト マップ プロバイダーに固有のメンバーにアクセスするには、使用`SiteMap.Providers["name"]`をプロバイダーのインスタンスを取得し、適切な型にキャストします。 たとえば、表示するため、 `NorthwindSiteMapProvider` s `CachedDate` ASP.NET ページのプロパティは、次のコードを使用します。


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> SQL キャッシュ依存関係の機能をテストすることを確認します。 アクセスした後、 `Default.aspx`、 `ProductsByCategory.aspx`、および`ProductDetails.aspx`ページが編集、挿入、および削除のセクションのチュートリアルのいずれかに移動し、カテゴリ、製品の名前を編集します。 内のページのいずれかに戻るし、`SiteMapProvider`フォルダー。 ポーリング メカニズムを基になるデータベースの変更を確認するための十分な時間が経過した場合、新しい製品またはカテゴリの名前を表示するサイト マップが更新する必要があります。


## <a name="summary"></a>まとめ

ASP.NET 2.0 のサイト マップ機能が含まれています、`SiteMap`クラスのさまざまな組み込みのナビゲーション Web コントロールし、サイト マップ情報が必要とする既定のサイト マップ プロバイダーが XML ファイルに保存します。 データベースからなど、他のソースからサイト マップ情報を使用するには、アプリケーション アーキテクチャ、またはカスタムのサイトを作成する必要があります。 リモートの Web サービスは、プロバイダーをマップします。 直接的または間接的に派生するクラスを作成する必要がありますから、`SiteMapProvider`クラス。

このチュートリアルでは、サイト マップをアプリケーション アーキテクチャの製品と分類の情報に基づくカスタム サイト マップ プロバイダーを作成する方法を説明しました。 このプロバイダーの拡張、`StaticSiteMapProvider`クラスと作成に含まれる、`BuildSiteMap`データを取得するメソッドは、サイト マップの階層を構築し、クラスレベルの変数に結果の構造のキャッシュします。 コールバック関数を使用した、キャッシュを無効にするための SQL キャッシュ依存関係を使いました構造体ときに、基になる`Categories`または`Products`データが変更されました。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [SQL Server でのサイト マップを格納する](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)と[SQL サイト マップ プロバイダーを ve を待機しています。](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [プロバイダー モデルを ASP.NET 2.0 を参照してください。](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [プロバイダー ツールキット](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [ASP.NET 2.0 を調べて s サイト ナビゲーションの機能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Dave Gardner、Zack Jones、Teresa Murphy、および「社長補佐 Leigh でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](building-a-custom-database-driven-site-map-provider-cs.md)
