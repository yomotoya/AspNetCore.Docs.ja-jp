---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: "カスタム データベース駆動型サイト マップ プロバイダー (c#) のビルド |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET 2.0 の既定のサイト マップ プロバイダーは、静的な XML ファイルからのデータを取得します。 XML ベースのプロバイダーが適切に多くの小規模および中規模 siz 中."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: f09d8d24cea43e80e66b0d8ce50911fcb978c85e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>カスタム データベース駆動型サイト マップ プロバイダー (c#) のビルド
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip)または[PDF のダウンロード](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> ASP.NET 2.0 の既定のサイト マップ プロバイダーは、静的な XML ファイルからのデータを取得します。 XML ベースのプロバイダーは、多くの小規模および中規模の Web サイトに適していますが、大規模な Web アプリケーションより動的なサイト マップが必要です。 このチュートリアルでは、ビジネス ロジック層からそのデータを取得するカスタムのサイト マップ プロバイダーをビルドでをさらに、データベースからデータを取得します。


## <a name="introduction"></a>はじめに

ASP.NET 2.0 のサイト マップ機能などの XML ファイルに、永続的ないくつか中で web アプリケーションのサイト マップを定義するページの開発者を有効にします。 サイト マップ データをプログラムでを介してアクセスできますを定義した後、 [ `SiteMap`クラス](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx)で、 [ `System.Web`名前空間](https://msdn.microsoft.com/en-us/library/system.web.aspx)など、さまざまなナビゲーション Web コントロール、または、サイト マップ パス、メニューのおよびツリー ビュー コントロール。 マップのサイト システムを使用して、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)別のサイト マップのシリアル化の実装を作成し、web アプリケーションに接続されているようにします。 ASP.NET 2.0 に付属する既定のサイト マップ プロバイダーには、XML ファイル内のサイト マップ構造が永続化します。 戻り、[マスター ページとサイトのナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)という名前のファイルを作成したチュートリアル`Web.sitemap`をこの構造体に含まれ、新しいチュートリアル セクションで、XML を更新されています。

既定の XML ベースのサイト マップ プロバイダーは、サイト マップの構造は、これらのチュートリアルのなど、比較的静的な場合はうまく機能します。 多くのシナリオでただしより動的なサイト マップが必要です。 各分類と製品を web サイトの構造内のセクションとして表示される、図 1 に示すサイト マップを検討してください。 このサイト マップを使用したルート ノードに対応する web ページへのアクセス可能性がありますすべてを一覧表示、カテゴリの一方、そのカテゴリの製品のリストが特定のカテゴリの web ページにアクセスし、特定の製品の web ページを表示するはその製品 s の詳細を表示します。


[![カテゴリおよび製品の構成、サイト マップ s 構造体](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**図 1**:、カテゴリおよび製品の構成はサイト マップの構造 ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


この分類および製品に基づく構造体にハードコーディングでしたが、`Web.sitemap`ファイル、ファイルごとにカテゴリを更新する必要がありますまたは製品が追加、削除、または名前を変更します。 そのため、サイト マップの保守は大幅に簡略化、データベースからまたは、理想的には、アプリケーションのアーキテクチャのビジネス ロジック層から、その構造が取得された場合。 こうすれば、製品およびカテゴリが追加されるため、名前変更、または削除、これらの変更を反映するように、サイト マップが自動的に更新します。

ASP.NET 2.0 のサイト マップのシリアル化がプロバイダー モデルの上に組み込まれているため、データベースやアーキテクチャなど、代替データ ストアからデータを取得する独自のカスタム サイト マップ プロバイダーを作成できます。 このチュートリアルでは、BLL からそのデータを取得する、カスタム プロバイダーを作成おします。 開始 s を let!

> [!NOTE]
> このチュートリアルで作成したカスタムのサイト マップ プロバイダーは、アプリケーションのアーキテクチャとデータ モデルに密接に結び付いてます。 Jeff Prosise s [SQL Server のサイト マップを格納する](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)と[SQL サイト マップ プロバイダーしている待機](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)アーティクルに SQL Server でのサイト マップ データを格納する汎用的なアプローチを確認します。


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>手順 1: カスタムのサイト マップ プロバイダーの Web ページを作成します。

開始前にカスタムのサイト マップ プロバイダーを作成する、s が最初に追加しなければならない、このチュートリアルでは ASP.NET ページを使用できます。 という名前の新しいフォルダーを追加することによって開始`SiteMapProvider`です。 次に、各ページに関連付けるように、そのフォルダーに次の ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

追加も、`CustomProviders`には、サブフォルダー、`App_Code`フォルダーです。


![サイト マップ プロバイダーに関連するチュートリアルについては、ASP.NET ページを追加します。](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**図 2**: 追加のサイトの ASP.NET ページ マップ プロバイダーに関連するチュートリアル


このセクションの 1 つだけのチュートリアルなので、必要ありません`Default.aspx`s セクションのチュートリアルの一覧を表示します。 代わりに、 `Default.aspx` GridView コントロールのカテゴリが表示されます。 これは、手順 2. で取り組むします。

次に、更新`Web.sitemap`への参照を含める、`Default.aspx`ページ。 具体的には、キャッシュ後に、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、唯一のサイト マップ プロバイダー チュートリアルでは、項目が含まれています。


![サイト マップではサイト マップ プロバイダー チュートリアルでは、エントリが含まれるようになりました](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**図 3**: サイト マップではサイト マップ プロバイダー チュートリアルでは、エントリが含まれるようになりました


このチュートリアルの主な作業では、カスタムのサイト マップ プロバイダーを作成して、そのプロバイダーを使用する web アプリケーションの構成を示すためです。 具体的には、図 1 に示されている各分類および製品のノードとルート ノードを含むサイト マップを表すプロバイダーおビルドします。 一般に、サイト マップ内の各ノードは、URL を指定することがあります。 マイクロソフトのサイト マップのルート ノードの URL になります`~/SiteMapProvider/Default.aspx`データベース内のカテゴリをすべて一覧します。 サイト マップ内の各カテゴリ ノードを指す URL になります`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`、指定した製品をすべて一覧する*categoryID*です。 最後に、各製品サイト マップ ノードが指す`~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`、特定の製品の詳細が表示されるされます。

開始するを作成する必要があります、 `Default.aspx`、 `ProductsByCategory.aspx`、および`ProductDetails.aspx`ページ。 これらのページについては、それぞれ 2、3、および第 4 のステップで完了します。 サイト マップ プロバイダーには、このチュートリアルのため、および過去のチュートリアルを説明を作成するため、このような複数ページのマスター/詳細が報告から手順 2 から 4 までに急いではおします。 複数のページにまたがるマスター/詳細レポートの作成の状態更新機能を必要がある場合に戻って、[マスター/詳細フィルター間で 2 つのページ](../masterdetail/master-detail-filtering-across-two-pages-cs.md)チュートリアルです。

## <a name="step-2-displaying-a-list-of-categories"></a>手順 2: カテゴリの一覧を表示します。

開く、 `Default.aspx`  ページで、`SiteMapProvider`フォルダーと、デザイナーの設定には、ツールボックスからドラッグ、GridView、`ID`に`Categories`です。 GridView s のスマート タグからにバインドして、という名前の新しい ObjectDataSource`CategoriesDataSource`しを使用してそのデータを取得するため、構成、`CategoriesBLL`クラスの`GetCategories`メソッドです。 この GridView だけカテゴリを表示、データ変更機能を提供しませんので、UPDATE、INSERT でドロップダウン リストを設定し、(なし) にタブを削除します。


[![返すメソッドを使用してカテゴリ ObjectDataSource を構成します。](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**図 4**: 構成カテゴリを返すを使用してに ObjectDataSource、`GetCategories`メソッド ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![UPDATE、INSERT のドロップダウン リストを設定し、(なし) タブを削除します。](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**図 5**: (なし) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


Visual Studio がの BoundField を追加、データ ソースの構成ウィザードを完了すると、 `CategoryID`、 `CategoryName`、 `Description`、 `NumberOfProducts`、および`BrochurePath`です。 GridView の編集のみが含まれるように、`CategoryName`と`Description`BoundFields し、更新、 `CategoryName` BoundField の`HeaderText`プロパティ カテゴリをします。

次に、内を追加して、そのように配置が s、左端のフィールドです。 設定、`DataNavigateUrlFields`プロパティを`CategoryID`と`DataNavigateUrlFormatString`プロパティを`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`です。 設定、`Text`製品を表示するプロパティです。


![カテゴリ GridView に内を追加します。](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**図 6**: を内の追加、 `Categories` GridView


後、ObjectDataSource を作成して、GridView のフィールドのカスタマイズ、宣言型マークアップを 2 つのコントロールは、次のようになります。


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

図 7 は`Default.aspx`ブラウザーで表示したときにします。 カテゴリの製品の表示をクリックするとリンクをクリックすると、 `ProductsByCategory.aspx?CategoryID=categoryID`、ここでは手順 3. で構築します。


[![各カテゴリは、ビューの製品リンクを含むに沿って表示されています。](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**図 7**: の製品のリンクにビューに表示されているに沿っては、各カテゴリ ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>手順 3:、選択したカテゴリの製品が一覧表示します。

開く、`ProductsByCategory.aspx`ページし、その名前を付け、GridView を追加`ProductsByCategory`です。 スマート タグからという名前の新しい ObjectDataSource を GridView にバインド`ProductsByCategoryDataSource`です。 構成を使用する ObjectDataSource、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドおよびセット ドロップダウン リストが UPDATE、INSERT、および DELETE の各タブで (なし) に一覧します。


[![ProductsBLL クラスの GetProductsByCategoryID(categoryID) メソッドを使用します。](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**図 8**: を使用して、`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


データ ソース構成ウィザードの最後の手順のパラメーターのソースの入力を求める*categoryID*です。 この情報はクエリ文字列フィールドを介して渡されるため`CategoryID`ドロップダウン リストから QueryString を選択して、図 9 に示すように、QueryStringField ボックスで、categoryid を入力します。 ウィザードを完了するには、[完了] をクリックします。


[![CategoryID パラメーターの使用 [categoryid] Querystring フィールド](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**図 9**: を使用して、`CategoryID`のクエリ文字列フィールド、 *categoryID*パラメーター ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


ウィザードを完了するは、Visual Studio は製品のデータ フィールドの GridView にも、対応する BoundFields と、CheckBoxField に追加されます。 削除が、 `ProductName`、 `UnitPrice`、および`SupplierName`BoundFields です。 これら 3 つ BoundFields をカスタマイズ`HeaderText`読み取れません製品、価格、および供給業者、それぞれのプロパティです。 形式、`UnitPrice`通貨として BoundField です。

次に、内に追加し、左端の位置に移動します。 設定の`Text`詳細を表示、プロパティ、`DataNavigateUrlFields`プロパティを`ProductID`、およびその`DataNavigateUrlFormatString`プロパティを`~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`です。


![ProductDetails.aspx を指すビューの詳細内を追加します。](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**図 10**: ビューの詳細を指す内の追加`ProductDetails.aspx`


これらのカスタマイズを行った後、GridView と ObjectDataSource s 宣言型マークアップが、次のようになります。


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

表示に戻る`Default.aspx`飲み物のリンクをブラウザーや製品の表示をクリックします。 これは、操作により`ProductsByCategory.aspx?CategoryID=1`飲み物のカテゴリに属している Northwind データベースの名前、価格、および製品の仕入先を表示する (図 11 を参照してください)。 ユーザー ページに戻り、カテゴリの一覧へのリンクを含めるには、このページをさらに強化するためにもかまいません (`Default.aspx`) と DetailsView またはフォーム ビューのコントロールで、選択したカテゴリの名前と説明を表示します。


[![飲み物の名前、価格、および仕入先が表示されます。](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**図 11**:、飲み物の名前、価格、および仕入先が表示されます ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>手順 4: 製品の詳細を表示します。

最終ページで、 `ProductDetails.aspx`、選択した製品の詳細を表示します。 開いている`ProductDetails.aspx`DetailsView をツールボックスからデザイナーにドラッグします。 DetailsView s を設定`ID`プロパティを`ProductInfo`をクリアし、`Height`と`Width`プロパティの値。 スマート タグから DetailsView をという名前の新しい ObjectDataSource にバインド`ProductDataSource`、ObjectDataSource からそのデータをプルする構成、`ProductsBLL`クラスの`GetProductByProductID(productID)`メソッドです。 同様に手順 2. および 3. で作成された以前の web ページ、UPDATE、INSERT でのドロップダウン リストの設定を (なし) タブを削除します。


[![ObjectDataSource GetProductByProductID(productID) メソッドを使用して構成します。](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**図 12**: 構成を使用する ObjectDataSource、`GetProductByProductID(productID)`メソッド ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


データ ソース構成ウィザードの最後の手順は、元のメッセージが表示されます、 *productID*パラメーター。 このデータは、クエリ文字列フィールドを介して渡されたため`ProductID`、クエリ文字列と ProductID する QueryStringField テキスト ボックスに、ドロップダウン リストを設定します。 最後に、ウィザードを完了するには、[完了] をクリックします。


[![ProductID ProductID Querystring フィールドからその値を取得するパラメーターを構成します。](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**図 13**: 構成、 *productID*からその値を取得するパラメーター、 `ProductID` Querystring フィールド ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


データ ソース構成ウィザードを完了すると、Visual Studio が対応する BoundFields および、CheckBoxField を製品のデータ フィールドの DetailsView に作成されます。 削除、 `ProductID`、 `SupplierID`、および`CategoryID`BoundFields し、必要に応じて表示とその他のフィールドを構成します。 美しく配置構成のいくつか後、は、my DetailsView および ObjectDataSource s 宣言型マークアップは、次のように検索。


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

このページをテストするを返す`Default.aspx`飲み物のカテゴリの製品の表示をクリックします。 飲み物の製品の一覧については、チャイの詳細を表示するリンクをクリックします。 これは、操作により`ProductDetails.aspx?ProductID=1`チャイ s (図 14 を参照してください) の詳細が示されます。


[![チャイ s 供給業者、カテゴリ、価格、およびその他の情報が表示されます。](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**図 14**: チャイ s 供給業者、カテゴリ、価格、およびその他の情報が表示されます ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>手順 5: サイト マップ プロバイダーの内部動作を理解します。

サイト マップが web サーバーのメモリ内のコレクションとして表される`SiteMapNode`階層を形成するインスタンス。 正確に 1 つのルートである必要があります、ただ 1 つの親ノードが必要ですがルート以外のすべてのノードおよびすべてのノードが子の任意の数を含めることがあります。 各`SiteMapNode`オブジェクトは、web サイトの構造内のセクションを表します。 これらのセクションでは、対応する web ページをよくがあります。 その結果、 [ `SiteMapNode`クラス](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx)などのプロパティを持つ`Title`、 `Url`、および`Description`、セクションの情報を提供する、`SiteMapNode`を表します。 `Key`それぞれを一意に識別するプロパティ`SiteMapNode`、階層、だけでなくこの階層を確立するために使用されるプロパティ`ChildNodes`、 `ParentNode`、 `NextSibling`、`PreviousSibling`などのようにします。

図 15 は、図 1 であっても、実装の詳細より詳細にスケッチを持つ、全般的なサイト マップ構造を示します。


[![各 SiteMapNode はプロパティと同様にタイトル、Url、キー、およびなど](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**図 15**: 各`SiteMapNode`プロパティなどが`Title`、 `Url`、`Key`され ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


サイト マップが経由でアクセスできる、 [ `SiteMap`クラス](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx)で、 [ `System.Web`名前空間](https://msdn.microsoft.com/en-us/library/system.web.aspx)です。 このクラス s`RootNode`プロパティは、サイト マップのルートを返します`SiteMapNode`インスタンス。`CurrentNode`を返します、`SiteMapNode`が`Url`現在の要求されたページの URL と一致するプロパティです。 このクラスは、ASP.NET 2.0 のナビゲーション Web コントロールによって内部的に使用します。

ときに、`SiteMap`クラス s のプロパティにアクセスは、メモリにいくつかの永続的なメディアからのサイト マップ構造がシリアル化する必要があります。 ただし、サイト マップのシリアル化ロジックはハードにコード化された、`SiteMap`クラスです。 代わりに、実行時に、`SiteMap`クラスは、サイトのマップを決定*プロバイダー*にシリアル化に使用します。 既定では、 [ `XmlSiteMapProvider`クラス](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx)使用が適切にフォーマットされた XML ファイルからサイト マップ s 構造体を読み取ります。 ただし、作業のほとんどのビットを使用して、独自のカスタムのサイト マップ プロバイダーを作成できます。

すべてのサイト マップ プロバイダーはから派生する必要があります、 [ `SiteMapProvider`クラス](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.aspx)重要なメソッドを含む、およびサイトに必要なプロパティは、プロバイダーをマップが、実装の詳細の多くを省略します。 2 番目のクラスを[ `StaticSiteMapProvider` ](https://msdn.microsoft.com/en-us/library/system.web.staticsitemapprovider.aspx)、拡張、`SiteMapProvider`クラスし、必要な機能のより堅牢に実装が含まれています。 内部的には、`StaticSiteMapProvider`格納、`SiteMapNode`のマップに、サイトのインスタンス、`Hashtable`などのメソッドを提供し、 `AddNode(child, parent)`、`RemoveNode(siteMapNode),`と`Clear()`追加および削除する`SiteMapNode`内部 s`Hashtable`です。 `XmlSiteMapProvider` は、`StaticSiteMapProvider` から派生しています。

拡張するカスタムのサイト マップ プロバイダーを作成するときに`StaticSiteMapProvider`、2 つの抽象メソッドをオーバーライドする必要があります: [ `BuildSiteMap` ](https://msdn.microsoft.com/en-us/library/system.web.staticsitemapprovider.buildsitemap.aspx)と[ `GetRootNodeCore`](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.getrootnodecore.aspx)です。 `BuildSiteMap`、名前からわかるように、は永続的なストレージからのサイト マップ構造体の読み込みと、メモリ内で構築を行います。 `GetRootNodeCore`サイト マップのルート ノードを返します。

前に、web アプリケーションは、アプリケーションの構成に登録する必要がありますが、サイト マップ プロバイダーを使用できます。 既定では、`XmlSiteMapProvider`クラスが名前で登録されている`AspNetXmlSiteMapProvider`です。 追加のサイト マップ プロバイダーを登録するには、次に示すマークアップを追加`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

*名前*値が中にプロバイダーを人間が判読できる名前を割り当てます*型*サイト マップ プロバイダーの完全修飾型名を指定します。 具体的な値を見ていきましょう、*名前*と*型*手順 7. で値された後、カスタムのサイト マップ プロバイダーを作成しました。

詳細については、サイトの プロバイダー クラスのマップには、初めてからアクセスがインスタンス化される、`SiteMap`クラスとは、web アプリケーションの有効期間中にメモリに残ります。 プロバイダーのメソッドである必要が呼び出された場合、複数の同時実行の web サイトの訪問者からのサイト マップ プロバイダーのインスタンスを 1 つだけなので、*スレッド セーフである*です。

パフォーマンスとスケーラビリティ上の理由から、メモリ内のサイト キャッシュすることが重要は構造体をマップし、これはキャッシュたびに再作成するのではなく、構造体を返す、`BuildSiteMap`メソッドが呼び出されます。 `BuildSiteMap`可能性があります複数回呼び出すページと、サイト マップ構造の深さで使用中のナビゲーション コントロールによって、ユーザーごとのページ要求ごと。 いかなる場合においても場合は、サイト マップ構造にキャッシュしないお`BuildSiteMap`られようとしてが呼び出されるたび、再 (あるため、クエリをデータベースに) アーキテクチャから製品と分類の情報を取得します。 キャッシュの前のチュートリアルで説明したよう、キャッシュされたデータが古くなることができます。 この対処するため、時間、または SQL キャッシュ依存関係に基づく切れのいずれかを使用できます。

> [!NOTE]
> サイト マップ プロバイダーが必要に応じてオーバーライドして、 [ `Initialize`メソッド](https://msdn.microsoft.com/en-us/library/system.web.sitemapprovider.initialize.aspx)です。 `Initialize`サイト マップ プロバイダーが最初にインスタンス化され、プロバイダーに割り当てられているカスタム属性に渡されるときに呼び出される`Web.config`で、`<add>`ような要素:`<add name="name" type="type" customAttribute="value" />`です。 プロバイダーのコードを変更することがなくさまざまなサイト マップ プロバイダーに関連する設定を指定するページの開発者を許可する場合に便利です。 たとえば、分類と製品データを可能性があります d おアーキテクチャではなく、データベースから直接読み取るおされた場合を許可するを通じてデータベース接続文字列を指定するページの開発者`Web.config`ハード コーディングされたを使用するのではなくプロバイダーのコード内の値。 手順 6 でビルド、カスタムのサイト マップ プロバイダーがこれをオーバーライドしません`Initialize`メソッドです。 使用する例については、`Initialize`メソッドを参照してください[Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [SQL Server のサイト マップを格納する](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)資料です。


## <a name="step-6-creating-the-custom-site-map-provider"></a>手順 6: カスタムのサイト マップ プロバイダーを作成します。

カテゴリおよび Northwind データベース内の製品からのサイト マップを構築するカスタムのサイト マップ プロバイダーを作成する必要がありますを拡張するクラスを作成する`StaticSiteMapProvider`です。 手順 1. で求められるを追加する、`CustomProviders`内のフォルダー、`App_Code`フォルダー - このという名前のフォルダーに新しいクラスを追加`NorthwindSiteMapProvider`です。 `NorthwindSiteMapProvider` クラスに次のコードを追加します。


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

このクラス s の探索で始まる s をできるように`BuildSiteMap`で始まるメソッド、 [ `lock`ステートメント](https://msdn.microsoft.com/en-us/library/c5kehkcz.aspx)です。 `lock`ステートメントのみが 1 つのスレッドを入力する時にこれにより、コードへのアクセスのシリアル化され、2 つの同時実行スレッドが互い s 生まれたでステップ実行するを妨げています。

クラス レベル`SiteMapNode`変数`root`サイト マップ構造のキャッシュに使用します。 サイト マップが構築されている場合、最初に、または基になるデータが変更された後に初めて`root`なります`null`サイト マップ構造が作成されるとします。 サイト マップのルート ノードに割り当てられた`root`構築時にプロセスを次にこのメソッドが呼び出された`root`されません`null`です。 その結果、限り`root`は`null`サイト マップ構造が再作成することがなく、呼び出し元に返されます。

ルートが場合`null`製品とカテゴリ情報からサイト マップ構造体を作成します。 作成することで、サイト マップが構築された、`SiteMapNode`インスタンスとしを呼び出すことで、階層を形成する、`StaticSiteMapProvider`クラスの`AddNode`メソッドです。 `AddNode`格納する、さまざまな内部のブックキーピングを実行`SiteMapNode`のインスタンスにある、`Hashtable`です。 まず、階層の構築を開始する前に、は、呼び出すことによって、`Clear`メソッドの内部からの要素を消去`Hashtable`です。 次に、`ProductsBLL`クラス s`GetProducts`メソッドおよび結果`ProductsDataTable`はローカル変数に格納します。

ルート ノードを作成してに割り当てることによって、サイト マップ s を構築を開始`root`です。 オーバー ロード、 [ `SiteMapNode` s のコンス トラクター](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.sitemapnode.aspx)ここで、ここで使用される`BuildSiteMap`は、次の情報に渡されます。

- サイト マップ プロバイダーへの参照を (`this`)。
- `SiteMapNode` S`Key`です。 これは、必須の値をそれぞれに一意にする必要があります`SiteMapNode`です。
- `SiteMapNode` S`Url`です。 `Url`オプションですが、指定した場合、それぞれが、 `SiteMapNode` s`Url`値は一意である必要があります。
- `SiteMapNode` S`Title`に必要になります。

`AddNode(root)`メソッドの呼び出しを追加、 `SiteMapNode` `root`ルートとしてサイト マップにします。 次に、各`ProductRow`で、`ProductsDataTable`列挙されます。 場合は既に存在する`SiteMapNode`現在の製品のカテゴリの参照されています。 それ以外の場合、新しい`SiteMapNode`カテゴリが作成され、追加の子として、`SiteMapNode``root`を通じて、`AddNode(categoryNode, root)`メソッドの呼び出しです。 適切なカテゴリの後に`SiteMapNode`ノードが見つからないか、作成された、`SiteMapNode`が、現在の製品作成され、カテゴリの子として追加`SiteMapNode`を介して`AddNode(productNode, categoryNode)`です。 注意してください、カテゴリ`SiteMapNode`s`Url`プロパティの値が`~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`製品を while `SiteMapNode` s`Url`プロパティが割り当てられている`~/SiteMapNode/ProductDetails.aspx?ProductID=productID`です。

> [!NOTE]
> データベースが存在している製品`NULL`値をその`CategoryID`、カテゴリの下でグループ化`SiteMapNode`が`Title`プロパティが None およびに設定`Url`プロパティが空の文字列に設定します。 設定することにしました`Url`から空の文字列を`ProductBLL`クラス s`GetProductsByCategory(categoryID)`メソッドを返すには、これらの製品と機能を現在がない、 `NULL` `CategoryID`値。 また、ナビゲーション コントロールの表示方法を示すたい、`SiteMapNode`の値が不足しているその`Url`プロパティです。 このチュートリアルを拡張することをお勧めできるように、None `SiteMapNode` s`Url`プロパティが指す`ProductsByCategory.aspx`の製品をまだのみが表示されます`NULL``CategoryID`値。


サイト マップを構築した後で、SQL キャッシュ依存関係を使用して、データ キャッシュに任意のオブジェクトが追加、`Categories`と`Products`テーブルを介して、`AggregateCacheDependency`オブジェクト。 前のチュートリアルでの SQL キャッシュ依存関係の使用を検討お*を使用して SQL キャッシュ依存関係*です。 カスタム サイト マップ プロバイダーは、ただし、データ キャッシュ s のオーバー ロードを使用`Insert`メソッド私たちを探索するには、まだしました。 このオーバー ロードは、その最後の入力パラメーターとして、オブジェクトがキャッシュから削除されたときに呼び出されるデリゲートを受け取ります。 具体的には、新規の渡します[`CacheItemRemovedCallback`委任](https://msdn.microsoft.com/en-us/library/system.web.caching.cacheitemremovedcallback.aspx)を指す、`OnSiteMapChanged`メソッド定義の下、`NorthwindSiteMapProvider`クラスです。

> [!NOTE]
> クラス レベルの変数を介してサイト マップのメモリ内表現がキャッシュされる`root`です。 Web アプリケーションでそのインスタンスはすべてのスレッド間で共有されているためと、カスタムのサイト マップ プロバイダー クラスのインスタンスを 1 つだけなので、このクラスの変数は、キャッシュとして機能します。 `BuildSiteMap`メソッドは、データベースでデータを基になるときに通知を受信するための手段としてのみ、データ キャッシュを使用するも、`Categories`または`Products`テーブルを変更します。 データ キャッシュに格納値が現在の日付と時刻だけであることを注意してください。 実際のサイト マップ データが*いない*をデータ キャッシュに格納します。


`BuildSiteMap`メソッドは、サイト マップのルート ノードを返すことによって完了するとします。

残りのメソッドは、非常に簡単です。 `GetRootNodeCore`ルート ノードを返すことを担当します。 `BuildSiteMap` 、ルートを返します`GetRootNodeCore`を単純に返します`BuildSiteMap`s が値を返します。 `OnSiteMapChanged`メソッドのセット`root`に`null`キャッシュ項目が削除されたとき。 ルートのセットに戻すと`null`、次回`BuildSiteMap`が呼び出されると、サイト マップ構造が再構築されます。 最後に、`CachedDate`プロパティは、このような値が存在する場合に、データ キャッシュに格納されている日付と時刻の値を返します。 このプロパティは、サイト マップ データが最後にキャッシュされてを確認 ページの開発者によって使用できます。

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>手順 7: 登録します`NorthwindSiteMapProvider`

使用するように web アプリケーションの順序で、`NorthwindSiteMapProvider`手順 6 で作成したサイト マップ プロバイダーに登録する必要があります、`<siteMap>`のセクション`Web.config`です。 具体的には、内で次のマークアップを追加、`<system.web>`内の要素`Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

このマークアップは次の 2 つ: ことを示して最初に、組み込み`AspNetXmlSiteMapProvider`既定のサイト マップ プロバイダーは、次に、Northwind のわかりやすい名前を持つ手順 6 で作成したカスタムのサイト マップ プロバイダーを登録します。

> [!NOTE]
> アプリケーション %s にあるサイト マップ プロバイダー`App_Code`フォルダーの値、`type`属性は、単純なクラス名。 また、カスタムのサイト マップ プロバイダーでしたで作成されている、別のクラス ライブラリ プロジェクトで、コンパイルされたアセンブリに web アプリケーションの配置`/Bin`ディレクトリ。 その場合は、`type`属性値になります*Namespace*.*ClassName*、 *AssemblyName*です。


更新した後に`Web.config`、すぐをブラウザーで、チュートリアルから任意のページを表示します。 左側のナビゲーション インターフェイスであっても、セクションでは、およびチュートリアルがで定義されていることに注意してください`Web.sitemap`です。 これは、ので`AspNetXmlSiteMapProvider`既定のプロバイダーとして。 使用するナビゲーション ユーザー インターフェイス要素を作成するために、 `NorthwindSiteMapProvider`Northwind のサイト マップ プロバイダーを使用することを明示的に指定する必要があります。 手順 8 で実行する方法を会いしましょう。

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>手順 8: カスタムのサイト マップ プロバイダーを使用してサイト マップ情報を表示します。

サイトとのカスタム マップ プロバイダーが作成されで登録`Web.config`、おナビゲーション コントロールを追加する準備ができたら、 `Default.aspx`、 `ProductsByCategory.aspx`、および`ProductDetails.aspx`内のページ、`SiteMapProvider`フォルダーです。 開いて開始、`Default.aspx`ページし、ドラッグ、`SiteMapPath`ツールボックスからデザイナーにします。 サイト マップ パス コントロールは、ツールボックスの [ナビゲーション] セクションにあります。


[![Default.aspx に、サイト マップ パスを追加します。](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**図 16**: 追加するサイト マップ パス`Default.aspx`([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


サイト マップ パス コントロールは、サイト マップ内で現在のページ s の位置を示す、階層リンクを表示します。 マスター ページの上部に、サイト マップ パスを追加おに戻り、*マスター ページとサイト ナビゲーション*チュートリアルです。

ブラウザーからこのページを表示するのにはしばらくを実行します。 図 16 で追加サイト マップ パス プロバイダーを使用して、デフォルト サイト マップ、そのデータを抽出`Web.sitemap`です。 したがって、ある階層リンクを示しますホーム&gt;右上隅にある階層リンクと同じように、サイト マップをカスタマイズします。


[![ある階層リンクが既定のサイト マップ プロバイダーを使用します。](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**図 17**: 階層リンク プロバイダーを使用して、既定のサイト マップ ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


サイト マップ パス図 16 の追加手順 6 で作成したカスタムのサイト マップ プロバイダーを使用するのには、次のように設定します。 その[`SiteMapProvider`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx)、northwind データベースを名前おに割り当てられている、`NorthwindSiteMapProvider`で`Web.config`です。 残念ながら、デザイナーが既定のサイト マップ プロバイダーを使用し続けますが、場合、このプロパティの変更を行った後、ブラウザーからページを参照してください階層リンクは、カスタムのサイト マップ プロバイダーを今すぐ使用も表示します。


[![ある階層リンクはカスタムのサイト マップ プロバイダー NorthwindSiteMapProvider を使用するようになりました](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**図 18**: 階層リンクは、カスタムのサイト マップ プロバイダーを使用するようになりました`NorthwindSiteMapProvider`([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


サイト マップ パス コントロールでより機能的ユーザー インターフェイスを表示、`ProductsByCategory.aspx`と`ProductDetails.aspx`ページ。 これらのページは、設定に、サイト マップ パスを追加、`SiteMapProvider`を Northwind に両方のプロパティです。 `Default.aspx`飲み物の製品の表示のリンクとチャイの詳細を表示するリンクし をクリックします。 現在のサイト マップ セクション (チャイ) とその先祖が階層リンクに含まれています図 19 に示す: Beverages とすべてのカテゴリ。


[![ある階層リンクはカスタムのサイト マップ プロバイダー NorthwindSiteMapProvider を使用するようになりました](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**図 19**: 階層リンクは、カスタムのサイト マップ プロバイダーを使用するようになりました`NorthwindSiteMapProvider`([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


メニューと TreeView コントロールなどサイト マップ パスだけでなく、他のナビゲーションのユーザー インターフェイス要素を使用できます。 `Default.aspx`、 `ProductsByCategory.aspx`、および`ProductDetails.aspx`このチュートリアルでは、たとえば、ダウンロードのページにはすべてが含まれてメニュー コントロールが (図 20 を参照してください)。 参照してください[ASP.NET 2.0 の検査 s サイト ナビゲーション機能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)と[サイト ナビゲーション コントロールを使用した](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx)のセクションで、 [ASP.NET 2.0 のクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/)をより詳しく用、ナビゲーション コントロールとサイト システムで ASP.NET 2.0 をマップします。


[![メニュー コントロールには、各分類と製品の一覧表示します。](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**図 20**:、メニュー コントロールを一覧表示各分類と製品の ([フルサイズのイメージを表示するをクリックして](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


ようにすると、このチュートリアルで前に、サイト マップ構造体をプログラムでを介してアクセスできます、`SiteMap`クラスです。 次のコードは、ルートを返します`SiteMapNode`既定のプロバイダーの。


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

以降、`AspNetXmlSiteMapProvider`は、既定のプロバイダーのアプリケーションでは、上記のコードで定義されているルート ノードを返します`Web.sitemap`です。 既定以外のサイト マップ プロバイダーを参照するには、使用、`SiteMap`クラス s [ `Providers`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.sitemap.providers.aspx)次のようにします。


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

ここで*名前*カスタム サイト マップ プロバイダー (Northwind、web アプリケーション) の名前を指定します。

サイト マップ プロバイダーに固有のメンバーにアクセスする`SiteMap.Providers["name"]`プロバイダーのインスタンスを取得し、適切な型にキャストします。 例については、表示する、 `NorthwindSiteMapProvider` s `CachedDate` ASP.NET ページ内のプロパティは、次のコードを使用します。


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> SQL キャッシュ依存関係の機能をテストすることを確認します。 アクセスした後、 `Default.aspx`、 `ProductsByCategory.aspx`、および`ProductDetails.aspx`ページが編集、挿入、および削除」セクションのチュートリアルは、のいずれかに移動し、カテゴリまたは製品の名前を編集します。 内のページのいずれかに戻ります、`SiteMapProvider`フォルダーです。 ポーリング機構の基になるデータベースに変更を確認するための十分な時間が経過した場合、サイト マップは、新しい製品またはカテゴリの名前を表示を更新する必要があります。


## <a name="summary"></a>概要

ASP.NET 2.0 のサイト マップ機能が含まれています、`SiteMap`クラス、組み込みのナビゲーション Web の数を制御、およびサイト マップ情報が必要ですが、既定サイト マップ プロバイダーが XML ファイルに保存します。 データベースからなど、他のソースからサイト マップ情報を使用するために、アプリケーションのアーキテクチャまたはカスタムのサイトを作成する必要があります、リモート Web サービス プロバイダーをマップします。 直接的または間接的に派生するクラスを作成する必要がありますから、`SiteMapProvider`クラスです。

このチュートリアルでは、サイト マップをアプリケーションのアーキテクチャの製品と分類の情報に基づくカスタム サイト マップ プロバイダーを作成する方法を説明しました。 拡張、プロバイダー、`StaticSiteMapProvider`クラスと作成に含まれる、`BuildSiteMap`データを取得するメソッドがサイト マップの階層を構築し、クラス レベルの変数に結果の構造をキャッシュします。 コールバック関数で SQL キャッシュ依存関係を使用して、キャッシュが無効になるに構成するときに、基になる`Categories`または`Products`データが変更されました。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [SQL Server でのサイト マップを格納する](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/)と[SQL サイト マップ プロバイダーしている待機](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [ASP.NET 2.0 を見てのプロバイダー モデルします。](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [プロバイダー ツールキット](https://msdn.microsoft.com/en-us/asp.net/aa336558.aspx)
- [ASP.NET 2.0 を調べて s サイト ナビゲーション機能](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Dave ガードナー、Zack Jones、Teresa マーフィー、および「社長補佐 Leigh でした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[次へ](building-a-custom-database-driven-site-map-provider-vb.md)
