---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: 入れ子になったデータ Web コントロール (VB) |Microsoft Docs
author: rick-anderson
description: について解説するこのチュートリアルでは、Repeater を使用する方法は、別の Repeater 内に入れ子にします。 例では、両方 d 内部 Repeater を設定する方法について説明しています.
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 45e460edb09fe9398d204e0f280dfb088a44946d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803179"
---
<a name="nested-data-web-controls-vb"></a>入れ子になったデータ Web コントロール (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe)または[PDF のダウンロード](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> について解説するこのチュートリアルでは、Repeater を使用する方法は、別の Repeater 内に入れ子にします。 例では、宣言とプログラミングは、内部の Repeater を設定する方法について説明します。


## <a name="introduction"></a>はじめに

だけでなく静的な HTML およびデータ バインドの構文では、テンプレートは Web コントロールおよびユーザー コントロールも含めることができます。 これらの Web コントロールがそのプロパティを持つことができます、宣言型のデータ バインディング構文を使用して割り当てられているか、適切なサーバー側のイベント ハンドラーでプログラムからアクセスできます。

テンプレート内のコントロールを埋め込んでには、外観とユーザー エクスペリエンスをカスタマイズおよび改良してできます。 たとえば、 [GridView コントロールで TemplateFields を使用して](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)チュートリアル, TemplateField 従業員の雇用された日付を表示するには、カレンダー コントロールを追加することによって GridView の表示をカスタマイズする方法を説明しました、[追加編集および挿入インターフェイスに検証コントロール](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)と[データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)チュートリアルでは、見た編集をカスタマイズする方法および挿入インターフェイスの検証を追加することでコントロール、テキスト ボックス、Dropdownlist、およびその他の Web コントロール。

テンプレートは、その他のデータ Web コントロールを含めることもできます。 つまり、DataList、テンプレート内で別 DataList (または Repeater または GridView や DetailsView など) を含むことができます。 このようなインターフェイスを使用してチャレンジが内部データ Web コントロールに適切なデータをバインドします。 使用可能なプログラムの ObjectDataSource を使用して宣言型のオプションの範囲はさまざまな方法をいくつか。

について解説するこのチュートリアルでは、Repeater を使用する方法は、別の Repeater 内に入れ子にします。 外部 Repeater、カテゴリの名前と説明を表示する、データベース内の各カテゴリの項目が含まれます。 各カテゴリ項目 s 内部 Repeater には、そのカテゴリに属する各製品の情報が表示されます (図 1 参照) 箇条書きリストにします。 これの例では、宣言とプログラミングは、内部の Repeater を設定する方法について説明します。


[![マイクロソフトの製品、と共に、各カテゴリの一覧が表示されます。](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**図 1**: マイクロソフトの製品、と共に、各カテゴリの一覧が表示されます ([フルサイズの画像を表示する をクリックします](nested-data-web-controls-vb/_static/image3.png))。


## <a name="step-1-creating-the-category-listing"></a>手順 1: 項目のリストを作成します。

データ Web コントロールをネストを使用するページを構築する場合は、作成、および内部の入れ子になったコントロールの概要も心配することがなく、最も外側にあるデータ Web コントロールを最初に、テストはデザインすると便利です。 そのため、名前と各カテゴリの説明を一覧表示されたページに、Repeater を追加するための手順をウォークすることによって開始 s ことができます。

開いて開始、`NestedControls.aspx`ページで、`DataListRepeaterBasics`フォルダー設定 ページに、Repeater コントロールを追加し、その`ID`プロパティを`CategoryList`します。 という名前の新しい ObjectDataSource を作成することも、Repeater のスマート タグから`CategoriesDataSource`します。


[![名前の新しい ObjectDataSource CategoriesDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**図 2**: 名前を新しい ObjectDataSource `CategoriesDataSource` ([フルサイズの画像を表示する をクリックします](nested-data-web-controls-vb/_static/image6.png))。


そのデータを取得するために、ObjectDataSource を構成、`CategoriesBLL`クラスの`GetCategories`メソッド。


[![CategoriesBLL クラスのメソッドを使用する ObjectDataSource を構成します。](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**図 3**: 構成に使用する ObjectDataSource、`CategoriesBLL`クラス s`GetCategories`メソッド ([フルサイズの画像を表示する をクリックします](nested-data-web-controls-vb/_static/image9.png))。


リピータ s のテンプレートを指定するには、コンテンツする必要があります、ソース ビューに移動し、宣言型構文を手動で入力します。 追加、`ItemTemplate`内のカテゴリの名前を表示する、`<h4>`要素と段落要素のカテゴリの説明 (`<p>`)。 さらに、let s が水平方向の規則に各カテゴリを区切ります (`<hr>`)。 これらの変更を行った後、ページは、Repeater を ObjectDataSource には、次のような宣言型構文を含める必要があります。


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

図 4 は、ブラウザーで表示したときに進行状況を示します。


[![各カテゴリ名と説明が表示されて、水平方向の規則で区切られました。](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**図 4**: 各カテゴリの名前と説明が表示されて、水平方向の規則で区切られた ([フルサイズの画像を表示する をクリックします](nested-data-web-controls-vb/_static/image12.png))。


## <a name="step-2-adding-the-nested-product-repeater"></a>手順 2: 入れ子になった製品 Repeater を追加します。

完全な一覧を表示するカテゴリで、次のタスクはする中継ぎ局を追加する、 `CategoryList` s`ItemTemplate`適切なカテゴリに属しているこれらの製品についての情報が表示されます。 これは、さまざまな方法はこの内部 Repeater、後取り上げる 2 つのデータを取得できます。 ここでは、let s だけ製品を作成、Repeater 内で、 `CategoryList` Repeater の`ItemTemplate`します。 具体的には、s 製品 Repeater の表示価格と製品の名前を含む各に箇条書きリストには、各製品一覧の項目があることができます。

内部 Repeater s 宣言の構文とにテンプレートを手動で入力する必要があります。 この Repeater を作成する、 `CategoryList` s`ItemTemplate`します。 内で次のマークアップを追加、 `CategoryList` Repeater の`ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>手順 3: 特定のカテゴリの製品を ProductsByCategoryList Repeater にバインドします。

この時点で、ブラウザーを使用してページにアクセスする場合は、画面が表示されます図 4 と同じため任意のデータを Repeater にバインドするには、まだ ve。 適切な製品レコードを取得し、それらを他よりも効率的な Repeater にバインドできることに、いくつかの方法はあります。 主な課題をここでは、指定したカテゴリの適切な製品戻る取得です。

内部 Repeater コントロールにバインドするデータか、アクセスできる宣言によってで ObjectDataSource を`CategoryList`Repeater の`ItemTemplate`、または ASP.NET ページの分離コード ページから、プログラムを使用します。 同様に、このデータにバインドできる内部 Repeater か、宣言によって、内部 Repeater s を通じて`DataSourceID`プロパティまたは宣言型データ バインディング構文またはプログラムによってで内部 Repeater を参照することによって、 `CategoryList` Repeater s`ItemDataBound`イベント ハンドラーをプログラムによって設定その`DataSource`プロパティ、および通話の`DataBind()`メソッド。 これらの各方法の詳細のことができます。

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>ObjectDataSource コントロールを宣言的データにアクセスして、`ItemDataBound`イベント ハンドラー

以降このチュートリアル シリーズでは、この例を ObjectDataSource を変えずに、データにアクセスする最も自然な選択全体で幅広く ObjectDataSource も使用しました。 `ProductsBLL`クラスには、`GetProductsByCategoryID(categoryID)`メソッドを指定した属しているこれらの製品に関する情報を返す *`categoryID`* します。 そのため、ObjectDataSource に追加できる、 `CategoryList` Repeater の`ItemTemplate`し、このクラスのメソッドからそのデータにアクセスするように構成します。

残念ながら、Repeater されないには、そのテンプレートは、この ObjectDataSource コントロールの宣言の構文を手動で追加する必要がありますので、デザイン ビューから編集することができるようにします。 次の構文に示す、 `CategoryList` Repeater s`ItemTemplate`この新しい ObjectDataSource を追加した後 (`ProductsByCategoryDataSource`)。


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

ObjectDataSource のアプローチを使用するときに設定する必要があります、 `ProductsByCategoryList` Repeater s`DataSourceID`プロパティを`ID`ObjectDataSource の (`ProductsByCategoryDataSource`)。 ObjectDataSource が通知も、`<asp:Parameter>`要素を指定する、 *`categoryID`* に渡される値、`GetProductsByCategoryID(categoryID)`メソッド。 ただし、この値を指定しますか。 理想的には、d をできるよう設定するだけで、`DefaultValue`のプロパティ、`<asp:Parameter>`要素のデータ バインドの構文を使用して次のように。


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

残念ながら、データ バインディング構文を持つコントロールに有効なのみが、`DataBinding`イベント。 `Parameter`クラスがこのようなイベントがないと、したがって上記の構文は無効であり、ランタイム エラーが発生します。

この値を設定する必要がありますのイベント ハンドラーを作成する、 `CategoryList` Repeater の`ItemDataBound`イベント。 いることを思い出してください、`ItemDataBound`イベントは、Repeater にバインドされている項目ごとに 1 回発生します。 そのため、このイベントは、外部のたびに割り当てることができます、現在`CategoryID`値を`ProductsByCategoryDataSource`ObjectDataSource の`CategoryID`パラメーター。

イベント ハンドラーを作成、 `CategoryList` Repeater の`ItemDataBound`次のコードでイベント。


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

このイベント ハンドラーは、ヘッダー、フッター、または区切り記号アイテムではなく項目 re、データを処理することができるようにして開始します。 次に、参照、実際`CategoriesRow`だけ現在のバインドされたインスタンス`RepeaterItem`します。 ObjectDataSource で最後に、参照、`ItemTemplate`を割り当てると、`CategoryID`パラメーター値を`CategoryID`、現在の`RepeaterItem`します。

このイベント ハンドラーと、`ProductsByCategoryList`各 Repeater`RepeaterItem`でこれらの製品にバインドされて、`RepeaterItem`のカテゴリ。 図 5 は、結果の出力のスクリーン ショットを示します。


[![外部 Repeater は、各カテゴリを一覧表示されます。内部の 1 つは、そのカテゴリの製品を一覧表示します。](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**図 5**: 外部 Repeater の一覧各カテゴリは内部の 1 つのリスト カテゴリの製品 ([フルサイズの画像を表示する をクリックします](nested-data-web-controls-vb/_static/image15.png))。


## <a name="accessing-the-products-by-category-data-programmatically"></a>プログラムでデータをカテゴリ別の製品へのアクセス

現在のカテゴリの製品を取得する、ObjectDataSource を使用する代わりに、ASP.NET ページの分離コード クラスでメソッドを作成でした (または、`App_Code`フォルダーまたは別のクラス ライブラリ プロジェクト) の適切なセットを返す製品に渡されるとき、`CategoryID`します。 ASP.NET ページ分離コード クラスのようなメソッドがあるというがという名前であることを考えてください`GetProductsInCategory(categoryID)`します。 この方法で現在のカテゴリの製品を次の宣言型構文を使用して内部 Repeater にバインドしますでした。


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

Repeater s`DataSource`プロパティでは、データ バインディング構文を使用して、データの取得元を示す、`GetProductsInCategory(categoryID)`メソッド。 `Eval("CategoryID")`型の値を返します`Object`、オブジェクトをキャスト、`Integer`に渡す前に、`GetProductsInCategory(categoryID)`メソッド。 なお、`CategoryID`ここで、データ バインドを介して構文がアクセスされる、`CategoryID`で、*外部*Repeater (`CategoryList`)、1 つのレコードに s がバインドされている、`Categories`テーブル。 そのため、当社は知って`CategoryID`データベースにすることはできません`NULL`盲目的にキャストできるため、値、`Eval`メソッドがあることを確認せず再処理する、 `DBNull`。

作成に必要なこの方法で、`GetProductsInCategory(categoryID)`メソッドの指定、指定された製品の適切なセットを取得することと *`categoryID`* します。 返すだけでそのため、`ProductsDataTable`によって返される、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド。 %S を作成できるように、`GetProductsInCategory(categoryID)`の分離コード クラスのメソッド、`NestedControls.aspx`ページ。 次のコードを使用して操作を行います。


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

このメソッドのインスタンスを作成、`ProductsBLL`メソッドの結果を返すと、`GetProductsByCategoryID(categoryID)`メソッド。 メソッドをマークする必要がありますに注意してください。`Public`または`Protected`メソッドがマークされている場合は`Private`、ASP.NET ページの宣言型マークアップからアクセスできることはできません。

この新しい手法を使用するこれらの変更を行った後、ブラウザーを使用してページを表示するのには少しがかかります。 ObjectDataSource を使用する場合は、出力を出力と一致させるようにと`ItemDataBound`イベント ハンドラーのアプローチ (戻るは図 5 のスクリーン ショットを参照してください)。

> [!NOTE]
> 作成するには、ユーザーに代わって業務に思えるかもしれませんが、 `GetProductsInCategory(categoryID)` ASP.NET ページの分離コード クラスのメソッド。 結局のところ、このメソッドは、のインスタンスだけを作成、`ProductsBLL`クラスし、結果が返されますその`GetProductsByCategoryID(categoryID)`メソッド。 なぜだけこのメソッドから直接呼び出す内部の Repeater でデータ バインディング構文のような:`DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`でしょうか。 この構文は、現在の実装では動作しませんが、`ProductsBLL`クラス (ため、`GetProductsByCategoryID(categoryID)`メソッドがインスタンス メソッド)、変更する可能性があります`ProductsBLL`に静的なを含める`GetProductsByCategoryID(categoryID)`メソッド クラスの静的を含めることも`Instance()`の新しいインスタンスを返すメソッドを`ProductsBLL`クラス。


必要性がなくなりますが、このような変更、 `GetProductsInCategory(categoryID)` ASP.NET ページの分離コード クラスのメソッド、分離コード クラスのメソッドによりより柔軟に間もなく表示されるように、取得したデータを使用します。

## <a name="retrieving-all-of-the-product-information-at-once"></a>すべての製品情報を一度に取得します。

2 つの手法検査 ve を呼び出すことによって現在のカテゴリの製品を取得する、`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッド (first アプローチ行った、ObjectDataSource を 2 番目から、`GetProductsInCategory(categoryID)`メソッドで、分離コード クラスの場合)。 このメソッドが呼び出されるたびに、データ アクセス層にビジネス ロジック層の呼び出しから行を返す SQL ステートメントを使用してデータベース クエリ、`Products`テーブルです`CategoryID`フィールドが指定された入力パラメーターと一致します。

指定された*N*システム カテゴリは、このアプローチのネット*N* + データベースの 1 つのデータベース クエリを 1 の呼び出しは、すべてのカテゴリを取得し、 *N*製品への呼び出し各カテゴリに特定します。 ただし、カテゴリ、およびすべての製品を取得する他のすべてを取得する 2 つのデータベース呼び出し 1 回の呼び出しで必要なすべてのデータを取得したことができます。 これらの製品のためフィルター処理のすべての製品を取得したら、現在の一致する製品だけである`CategoryID`s そのカテゴリにバインドされて内部 Repeater します。

この機能を提供することには、のみにわずかな変更を加える必要が、 `GetProductsInCategory(categoryID)` ASP.NET ページの分離コード クラスのメソッド。 無条件の結果を返すのではなく、`ProductsBLL`クラス s`GetProductsByCategoryID(categoryID)`メソッド、なるべく代わりに初めてアクセス*すべて*製品の (t いない場合されて既にアクセス) しのフィルター処理されたビューだけを返す、製品が渡されるに基づく`CategoryID`します。


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

ページ レベルの変数の追加に注意してください`allProducts`します。 これは、すべての製品に関する情報を保持し、最初の時刻が設定されて、`GetProductsInCategory(categoryID)`メソッドが呼び出されます。 いることを確認したら、`allProducts`行だけを持つように、メソッドが DataTable の結果をフィルター処理、オブジェクトを作成および設定`CategoryID`、指定された一致`CategoryID`はアクセスできます。 この方法は、データベースからへのアクセス回数の合計を軽減*N* + 1 が 2 つまでです。

この機能強化は、ページのレンダリングされたマークアップへの変更を導入していません。 また、他のアプローチよりもが少なくなるレコードが提供。 データベースへの呼び出しの数を減らすだけです。

> [!NOTE]
> 直感的理由の 1 ついるデータベースへのアクセスの数を減らす確実パフォーマンスが向上します。 ただし、大文字と小文字をできないこれがあります。 多数の製品がある場合が`CategoryID`は`NULL`、例では、その呼び出しを`GetProducts`メソッドが表示されないの製品の数を返します。 すべての製品を返すことができますがさらに、無駄である場合は再のみがある場合、ページングを実装している場合のカテゴリのサブセットを表示します。


常に、ときに渡される 2 つの手法のパフォーマンスの分析、のみ surefire メジャーでは、アプリケーションの一般的なケース シナリオに合わせた制御されたテストを実行します。

## <a name="summary"></a>まとめ

このチュートリアルでは方法を説明しました、別の Web コントロールの 1 つのデータを入れ子にする方法が箇条書きリストには、各カテゴリの製品を一覧表示する内部の Repeater を各カテゴリの項目を表示する外部の Repeater を具体的には確認します。 入れ子になったユーザー インターフェイスの構築の主な課題にアクセスして、内部データ Web コントロールに適切なデータのバインドによってもたらされます。 さまざまな手法が使用できますが、このチュートリアルで調べる 2 つがあります。 調査の最初のアプローチが外部データ Web コントロール s で ObjectDataSource を使用`ItemTemplate`を使用して内部データ Web コントロールにバインドされましたが、`DataSourceID`プロパティ。 2 番目の手法は、ASP.NET ページの分離コード クラスのメソッドを使用してデータにアクセスします。 このメソッドは、内部データ Web コントロール s にし、バインドできる`DataSource`データ バインディング構文を使用してプロパティ。

このチュートリアルで調査、入れ子になったユーザー インターフェイスを使用、Repeater 内で入れ子になった Repeater、他のデータ Web コントロールにこれらの手法を拡張できます。 Repeater 内 gridview が開きます。 または、GridView、DataList 内で入れ子にできなど。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Zack Jones、Liz Shulok でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
