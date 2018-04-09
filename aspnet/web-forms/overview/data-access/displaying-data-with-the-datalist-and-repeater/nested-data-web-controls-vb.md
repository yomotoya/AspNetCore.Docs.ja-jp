---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: 入れ子になったデータ Web コントロール (VB) |Microsoft ドキュメント
author: rick-anderson
description: について学びますこのチュートリアルでは、リピータを使用する方法は、別リピータ内に入れ子にします。 内部リピータ両方 d を設定する方法を例しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: d8bb5eae2003273fa8d8a06cc4adaa959378f1e2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="nested-data-web-controls-vb"></a>入れ子になったデータ Web コントロール (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe)または[PDF のダウンロード](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> について学びますこのチュートリアルでは、リピータを使用する方法は、別リピータ内に入れ子にします。 例は宣言またはプログラムによって内部 Repeater を設定する方法を示します。


## <a name="introduction"></a>はじめに

静的な HTML およびデータ バインドの構文は、に加えてテンプレートは Web コントロールおよびユーザー コントロールも含めることができます。 これらの Web コントロールがそのプロパティを持つことができます、宣言型のデータ バインド構文を使用して割り当てられているか、適切なサーバー側のイベント ハンドラーでプログラムからアクセスできます。

テンプレート内のコントロールを埋め込むことで、外観、およびユーザー エクスペリエンスをカスタマイズして改善しました。、 たとえば、 [GridView コントロールを使用して TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)チュートリアルでは、従業員の入社日が s を表示する TemplateField; で、予定表コントロールを追加することによって GridView の表示をカスタマイズする方法を説明しました、[追加編集や挿入のインターフェイスに検証コントロール](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)と[データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)チュートリアルについては、見た編集をカスタマイズする方法と挿入のインターフェイスの検証を追加することでコントロール、テキスト ボックス、DropDownLists、およびその他の Web コントロールです。

テンプレートは、その他のデータの Web コントロールを含めることもできます。 つまりとを含む別 DataList (またはリピータまたは GridView または DetailsView など)、テンプレート内で DataList ことができます。 このようなインターフェイスを使用してチャレンジの Web コントロールの内部データに適切なデータのバインドはします。 使用可能なプログラム的なものに ObjectDataSource を使用して宣言型のオプションの範囲は、いくつかの方法です。

について学びますこのチュートリアルでは、リピータを使用する方法は、別リピータ内に入れ子にします。 外部リピータ、カテゴリの名前と説明を表示する、データベース内の各カテゴリの項目が含まれます。 各カテゴリ項目 s 内部リピータには、そのカテゴリに属する各製品の情報が表示されます (図 1 を参照してください) 箇条書きリストにします。 例は、宣言またはプログラムによって内部 Repeater を設定する方法を示しています。


[![その製品と共に、各カテゴリの一覧が表示されます。](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**図 1**: と共にその製品の各カテゴリの一覧が表示されます ([フルサイズのイメージを表示するをクリックして](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>手順 1: 項目のリストを作成します。

使用するページを構築すると、Web コントロールのデータが入れ子になっているときに I デザインすると便利、作成、および心配も内部に入れ子になった制御最初に、最も外側にあるデータ Web コントロールをテストします。 そのため、名前と各カテゴリの説明を一覧表示するページにリピータを追加する必要な手順をウォークすることによって開始 s を使用できます。

開いて開始、 `NestedControls.aspx`  ページで、`DataListRepeaterBasics`フォルダー リピータ コントロール、ページの設定を追加し、その`ID`プロパティを`CategoryList`です。 リピータ s のスマート タグからという名前の新しい ObjectDataSource を作成する選択`CategoriesDataSource`です。


[![名前を新しい ObjectDataSource CategoriesDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**図 2**: 名前を新しい ObjectDataSource `CategoriesDataSource` ([フルサイズのイメージを表示するをクリックして](nested-data-web-controls-vb/_static/image6.png))


そのデータを取得できるように、ObjectDataSource を構成、`CategoriesBLL`クラスの`GetCategories`メソッドです。


[![構成、ObjectDataSource CategoriesBLL クラスのメソッドを使用するには](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**図 3**: 構成を使用する ObjectDataSource、`CategoriesBLL`クラス s`GetCategories`メソッド ([フルサイズのイメージを表示するをクリックして](nested-data-web-controls-vb/_static/image9.png))


リピータのテンプレートを指定するには、コンテンツいただくために、ソース ビューに移動し、宣言の構文を手動で入力します。 追加、`ItemTemplate`項目 %s の名前を表示する、`<h4>`要素と段落要素のカテゴリの説明 (`<p>`)。 Let s が、水平方向の規則と各カテゴリを分離するさらに、(`<hr>`)。 これらの変更を行った後、ページはリピータとは、次のような ObjectDataSource 宣言の構文を含める必要があります。


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

図 4 は、ブラウザーで表示したときに、進行状況を示します。


[![各カテゴリ名と説明が表示されて、区切り線で区切られました。](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**図 4**: 各カテゴリの名前と説明が表示されて、区切り線で区切られた ([フルサイズのイメージを表示するをクリックして](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>手順 2: 入れ子になった製品 Repeater を追加します。

完全な一覧表示するカテゴリには、次のタスクがする中継ぎ局を追加する、 `CategoryList` s`ItemTemplate`適切なカテゴリに属する製品についての情報を表示します。 さまざまなデータの 2 つを見ていきましょう間もなく、この内部リピータの取得できる方法があります。 ここでは、let s だけの製品を作成リピータ内で、`CategoryList`リピータの`ItemTemplate`です。 具体的には、製品それぞれ箇条書きリストでは、各製品が製品の名前と価格を含むリスト項目リピータ表示 s を使用できます。

内部のリピータ s 宣言構文およびにテンプレートを手動で入力する必要があります。 この Repeater を作成する、 `CategoryList` s`ItemTemplate`です。 内で次のマークアップを追加、`CategoryList`リピータの`ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>手順 3: ProductsByCategoryList Repeater をカテゴリ別の製品をバインドします。

この時点で、ブラウザーでページにアクセスした場合、画面になります図 4 のものと同じためおリピータにすべてのデータをバインドするには、まだしました。 適切な製品レコードを取得したりリピータ、他のものより効率的なにバインドできますをいくつかの方法があります。 主要な課題をここでの適切な製品の指定したカテゴリの戻る取得です。

内部 Repeater コントロールにバインドするデータかからアクセスできます宣言的に、ObjectDataSource、`CategoryList`リピータの`ItemTemplate`、またはプログラムでは、ASP.NET ページの分離コード ページです。 同様に、このデータにバインドできる内部リピータか、宣言の内部リピータ s を通じて`DataSourceID`プロパティまたはデータ バインドを宣言の構文またはプログラムによって内部リピータ内を参照することによって、`CategoryList`リピータ s`ItemDataBound`プログラムによって設定、イベント ハンドラーの`DataSource`プロパティ、および呼び出し元の`DataBind()`メソッドです。 これらの方法は、それぞれ s を使用できます。

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>ObjectDataSource コントロールを使用して宣言によってデータにアクセスして、`ItemDataBound`イベント ハンドラー

以降このチュートリアル シリーズの次の例は、ObjectDataSource のオプションを使用するデータにアクセスするための最も自然な選択全体を通じて広く ObjectDataSource も使用しました。 `ProductsBLL`クラスには、`GetProductsByCategoryID(categoryID)`を指定したに属している製品に関する情報を返すメソッド *`categoryID`*です。 ObjectDataSource を追加できるため、`CategoryList`リピータの`ItemTemplate`し、このクラスのメソッドからそのデータにアクセスするように構成します。

残念ながら、リピータされないのためこの ObjectDataSource コントロールの宣言の構文を手動で追加する必要はデザイン ビューで編集するには、そのテンプレートを許可します。 次の構文に示す、`CategoryList`リピータ s`ItemTemplate`この新しい ObjectDataSource を追加した後 (`ProductsByCategoryDataSource`)。


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

ObjectDataSource アプローチを使用するときに設定する必要があります、`ProductsByCategoryList`リピータ s`DataSourceID`プロパティを`ID`、ObjectDataSource の (`ProductsByCategoryDataSource`)。 またに注意してください、ObjectDataSource のある、`<asp:Parameter>`を指定する要素、 *`categoryID`*に渡される値、`GetProductsByCategoryID(categoryID)`メソッドです。 しかし、この値を指定する方法をおでしょうか。 理想的には、d にのみ設定できる、`DefaultValue`のプロパティ、`<asp:Parameter>`要素のデータ バインドの構文を使用して次のように。


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

残念ながら、データ バインド構文は、のみを持つコントロールでは有効で、`DataBinding`イベント。 `Parameter`クラスのようなイベントがないと、そのため、上記の構文は無効であり、ランタイム エラーが発生します。

この値を設定する必要がありますのイベント ハンドラーを作成する、`CategoryList`リピータの`ItemDataBound`イベント。 注意してください、`ItemDataBound`イベントがリピータにバインドされている項目ごとに 1 回発生します。 そのため、外部リピータに対してこのイベントが発生するたびに割り当てることができます、現在`CategoryID`値を`ProductsByCategoryDataSource`ObjectDataSource の`CategoryID`パラメーター。

イベント ハンドラーを作成、`CategoryList`リピータの`ItemDataBound`次のコードでイベント。


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

このイベント ハンドラーは、ヘッダー、フッター、または区切り記号の項目ではなく、データを処理する re お項目されるようにすることで開始します。 次に、参照、実際`CategoriesRow`だけが、現在にバインドされているインスタンス`RepeaterItem`です。 最後は、参照で ObjectDataSource、`ItemTemplate`を割り当てると、`CategoryID`パラメーター値を`CategoryID`、現在の`RepeaterItem`します。

このイベント ハンドラーを持つ、`ProductsByCategoryList`各リピータ`RepeaterItem`でこれらの製品にバインドされて、`RepeaterItem`のカテゴリ。 図 5 は、結果の出力のスクリーン ショットを示します。


[![外部リピータがそれぞれのカテゴリを一覧表示します。内部の 1 つは、そのカテゴリの製品を一覧表示します。](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**図 5**: 外部 Repeater を一覧表示各カテゴリ; 内部の 1 つリストそのカテゴリの製品 ([フルサイズのイメージを表示するをクリックして](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>プログラムで、製品カテゴリのデータにアクセス

現在のカテゴリの製品を取得する、ObjectDataSource を使用する代わりに、ASP.NET ページの分離コード クラスにメソッドを作成おでした (または、`App_Code`フォルダーまたは別のクラス ライブラリ プロジェクト) の適切なセットを返す製品に渡されるとき、`CategoryID`です。 たとえば、ことが発生しました。 このようなメソッドで、ASP.NET ページの分離コード クラスとがという名前の`GetProductsInCategory(categoryID)`します。 配置でこのメソッドを使用して、次の宣言の構文を使用して内部リピータに現在のカテゴリの製品バインドでした。


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

リピータ s`DataSource`プロパティは、そのデータの取得元を示すためにデータ バインド構文を使用して、`GetProductsInCategory(categoryID)`メソッドです。 `Eval("CategoryID")`型の値を返します`Object`、オブジェクトにキャストして、`Integer`に渡す前に、`GetProductsInCategory(categoryID)`メソッドです。 注意してください、`CategoryID`ここで、データ バインドを介して構文がアクセスされる、`CategoryID`で、*外部*リピータ (`CategoryList`)、1 つのレコードにバインドされている s、`Categories`テーブル。 そのため、それがわかった`CategoryID`データベースは使用できません`NULL`お無条件にキャストできる値、`Eval`場合をチェックせずメソッドを処理する re お、`DBNull`です。

この方法で作成する必要があります、`GetProductsInCategory(categoryID)`メソッドの指定された、指定された製品の適切なセットを取得することと *`categoryID`*です。 単に返すことによってこの作業を行うことができます、`ProductsDataTable`によって返される、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。 %S を作成できるように、`GetProductsInCategory(categoryID)`の分離コード クラスのメソッド、`NestedControls.aspx`ページ。 次のコードを使用するための操作を行います。


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

このメソッドのインスタンスを作成、`ProductsBLL`メソッドの結果を返すと、`GetProductsByCategoryID(categoryID)`メソッドです。 メソッドをマークする必要がありますに注意してください`Public`または`Protected`以外の場合は、メソッドがマークされている`Private`、ASP.NET ページの宣言型マークアップからアクセスできることはできません。

この新しい手法を使用してこれらの変更を加えたら、すぐをブラウザーでページを表示します。 ObjectDataSource を使用する場合に、出力が、出力と同じにする必要がありますと`ItemDataBound`イベント ハンドラーのアプローチ (戻るは図 5 のスクリーン ショットを参照してください)。

> [!NOTE]
> 作成するには、ユーザーに代わって業務に思えるかもしれませんが、 `GetProductsInCategory(categoryID)` ASP.NET ページの分離コード クラスのメソッドです。 結局のところ、このメソッドは、のインスタンスだけを作成、`ProductsBLL`クラスし、結果が返されますその`GetProductsByCategoryID(categoryID)`メソッドです。 なぜメソッドを呼び出すだけこの内部のリピータ内のデータ バインド構文から直接のような:`DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`しますか? この構文は、現在の実装では動作しませんが、`ProductsBLL`クラス (ので、`GetProductsByCategoryID(categoryID)`メソッドがインスタンス メソッド)、変更する可能性があります`ProductsBLL`に静的なを含める`GetProductsByCategoryID(categoryID)`メソッド クラスの静的なを含めることも`Instance()`の新しいインスタンスを返すメソッドを`ProductsBLL`クラスです。


必要性がなくなりますが、このような変更、 `GetProductsInCategory(categoryID)` ASP.NET ページの分離コード クラスのメソッド、分離コード クラスのメソッド得間もなくおわかりのように、取得したデータの操作をより柔軟です。

## <a name="retrieving-all-of-the-product-information-at-once"></a>すべての製品情報を一度に取得します。

2 つの以前の手法お調べる ve では、現在のカテゴリの製品をつかんでを呼び出すことによって、`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッド (最初の方法としてくれて、ObjectDataSource を通じて、2 番目から、`GetProductsInCategory(categoryID)`メソッドで、分離コード クラスの場合)。 このメソッドが呼び出されるたびに、データ アクセス層にビジネス ロジック層呼び出しから行を返す SQL ステートメントを使用して、データベース クエリ、`Products`テーブルです`CategoryID`フィールドが指定された入力パラメーターと一致します。

指定された*N*システムのカテゴリは、このアプローチのネット*N* + 1 の呼び出しをデータベースの 1 つのデータベースのクエリすべてのカテゴリを取得し、 *N*製品を取得する呼び出し各カテゴリに固有です。 ただし、カテゴリとすべての製品を取得する他のすべてを取得する 2 つのデータベース呼び出し 1 回の呼び出しに必要なすべてのデータを取得おことができます。 すべての製品したらをフィルター処理するこれらの製品のため現在に一致する製品だけを`CategoryID`s そのカテゴリにバインドされた内部リピータします。

この機能を提供するだけいただくためにわずかな変更を加えて、 `GetProductsInCategory(categoryID)` ASP.NET ページの分離コード クラスのメソッドです。 無条件の結果を返すのではなく、`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッド、おできます代わりに最初にアクセス*すべて*の製品の (t いない場合されて既にアクセス) のフィルターされたビューだけを返すと、製品に基づいて渡された`CategoryID`です。


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

ページ レベルの変数の追加に注意してください`allProducts`です。 これに関するすべての製品情報を保持され、最初の時間が設定されます、`GetProductsInCategory(categoryID)`メソッドが呼び出されます。 確認した後、`allProducts`メソッドは、ある行だけになるように DataTable の結果をフィルター処理、オブジェクトが作成され、設定`CategoryID`、指定された一致`CategoryID`アクセスします。 このアプローチから、データベースがアクセスした回数が減少*N*は + 1 を 2 つまでです。

この機能強化では、ページのマークアップを表示するすべての変更を提供しません。 また他の方法よりも少ない戻るレコードも表示できません。 単に、データベースへの呼び出しの数を減らします。

> [!NOTE]
> いるデータベースへのアクセスの数を減らす速くてパフォーマンスが向上直感的に理由 1 つがあります。 ただし、大文字と小文字をできないこの可能性があります。 製品の数が大きい場合が`CategoryID`は`NULL`、例では、への呼び出し、`GetProducts`メソッドが表示されることはありません製品の数を返します。 すべての製品を返すことができますがさらに、無駄なである場合がある場合、ページングを実装している場合のカテゴリのサブセットのみを表示します。


常に、ときに渡される 2 つの方法のパフォーマンスの分析、のみとれるメジャーのアプリケーションの一般的なシナリオに対応したコントロールのテストの実行を開始します。

## <a name="summary"></a>まとめ

このチュートリアルで方法を説明しましたを 1 つのデータ、別の Web コントロールを入れ子にする具体的には、外部リピータ箇条書きリスト内の各カテゴリの製品を一覧表示する内部のリピータと各カテゴリの項目を表示する方法を確認します。 アクセスと、内部のデータの Web コントロールに正しいデータをバインドには、入れ子になったユーザー インターフェイスの構築で主要な課題があります。 さまざまな手法が使用できますが、このチュートリアルで確認おうち 2 つがあります。 最初の調査方法は、Web コントロールの外部のデータで、ObjectDataSource を使用`ItemTemplate`を使用して内部データ Web コントロールにバインドされていたをその`DataSourceID`プロパティです。 2 番目の手法は、ASP.NET ページの分離コード クラスのメソッドを使用してデータにアクセスします。 このメソッドは、内部データ Web コントロール s にし、バインドできる`DataSource`databinding 構文を使用してプロパティです。

調べるこのチュートリアルでは、入れ子になったユーザー インターフェイスに使用されますリピータ内に入れ子にリピータが、これらの手法は、その他の Web コントロールのデータを拡張できます。 GridView または DataList 内での GridView 内 Repeater を入れ子にできにすることができます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Zack Jones および Liz Shulok がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
