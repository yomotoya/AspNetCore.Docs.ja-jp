---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: 宣言パラメーター (VB) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、DetailsView コントロールに表示するデータを選択するハード コーディングされた値に設定するパラメーターを使用する方法について説明します。
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 01330d6c743fa9534cdb5dfa42bde5dbbe954c40
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839230"
---
<a name="declarative-parameters-vb"></a>宣言パラメーター (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe)または[PDF のダウンロード](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> このチュートリアルでは、DetailsView コントロールに表示するデータを選択するハード コーディングされた値に設定するパラメーターを使用する方法について説明します。


## <a name="introduction"></a>はじめに

[の最終チュートリアル](displaying-data-with-the-objectdatasource-vb.md)データ呼び出さ ObjectDataSource コントロールにバインドされた GridView、DetailsView と FormView コントロールを表示するところ、`GetProducts()`からメソッド、`ProductsBLL`クラス。 `GetProducts()`メソッドはすべての Northwind データベースのレコードを含む厳密に型指定された DataTable を返します`Products`テーブル。 `ProductsBLL`クラスには、その他の商品のだけサブセットを返すメソッドが含まれています`GetProductByProductID(productID)`、 `GetProductsByCategoryID(categoryID)`、および`GetProductsBySupplierID(supplierID)`します。 これら 3 つのメソッドには、返された製品情報をフィルター処理する方法を示す、入力パラメーターが期待しています。

入力パラメーターは、予想されるメソッドを呼び出すそのためにはこれらのパラメーター値元の場所を指定します、ObjectDataSource を使用できます。 パラメーターの値にハードコーディングすることができますか、さまざまななどの動的ソースから取得できます。 クエリ文字列値、セッション変数、またはその他のページ上の Web コントロールのプロパティの値。

このチュートリアルではまずハード コーディングされた値に設定するパラメーターを使用する方法を説明します。 具体的には、namely Chef Anton の Gumbo するミックスが特定の製品に関する情報を表示するページに、DetailsView を追加することに注目します、`ProductID`は 5 です。 次に、Web コントロールを基にパラメーター値を設定する方法を見ていきます。 具体的には、ユーザーがその後、その国内に存在する仕入先の一覧を表示するのにボタンをクリックしたことができます、国を入力できるテキスト ボックスを使用します。

## <a name="using-a-hard-coded-parameter-value"></a>ハード コーディングされたパラメーター値を使用します。

最初の例では、DetailsView コントロールを追加することによって、起動、`DeclarativeParams.aspx`ページで、`BasicReporting`フォルダー。 DetailsView のスマート タグから次のように選択します。&lt;新しいデータ ソース&gt;ドロップダウン リストから一覧表示し、ObjectDataSource を追加します。


[![ObjectDataSource をページに追加します。](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**図 1**: ObjectDataSource をページに追加 ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image3.png))。


ObjectDataSource コントロールのデータ ソースの選択ウィザードが自動的に開始されます。 選択、`ProductsBLL`ウィザードの最初の画面からクラス。


[![特別なに感謝します。](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**図 2**: 選択、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image6.png))。


使用する特定の製品についての情報を表示するため、`GetProductByProductID(productID)`メソッド。


[![GetProductByProductID(productID) 方法を選択します。](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**図 3**: 選択、`GetProductByProductID(productID)`メソッド ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image9.png))。


選択したメソッドには、パラメーターが含まれているため、パラメーターに使用する値の定義を求める、ウィザードの詳細について 1 つの画面があります。 左側の一覧には、選択したメソッドのパラメーターのすべてが表示されます。 `GetProductByProductID(productID)` 1 つしかない`productID`します。 右側には、選択したパラメーターの値を指定できます。 パラメーターのソースのドロップダウン リストでは、パラメーター値のさまざまな原因を列挙します。 5 用のハード コーディングされた値を指定するので、`productID`パラメーター、None としてパラメーターのソースを残す場合、DefaultValue テキスト ボックスに 5 を入力します。


[![Hard-Coded パラメーター値の 5 を使用するパラメーターの productID に対して](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**図 4**: A Hard-Coded パラメーター値の 5 を使用するため、`productID`パラメーター ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image12.png))。


ObjectDataSource コントロールの宣言型マークアップを含むデータ ソースの構成ウィザードを完了したら、`Parameter`オブジェクト、`SelectParameters`で定義されたメソッドで想定されている入力パラメーターのそれぞれのコレクション、 `SelectMethod`プロパティ。 この例で使用するメソッドがだけを単一の入力パラメーターを受け取るため`parameterID`、ここで 1 つだけのエントリがあります。 `SelectParameters`コレクションから派生する任意のクラスを含めることができます、`Parameter`クラス、`System.Web.UI.WebControls`名前空間。 パラメーターのハードコード値ベースの`Parameter`クラスが使用されますが、他のパラメーターのソース オプション、派生`Parameter`クラスが使用されます。 独自に作成することも[カスタム パラメーターの型](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11)必要な場合、します。

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> 月の値を含める表示この時点でに従ってお使いコンピューターで、宣言型マークアップがある場合、 `InsertMethod`、`UpdateMethod`と`DeleteMethod`プロパティだけでなく`DeleteParameters`。 ObjectDataSource のデータ ソースの選択ウィザードからのメソッドを自動的に指定する、`ProductBLL`に使用する挿入、更新、および削除するには、明示的にオフにしたものをしない限り、上記のマークアップに含めるをできるようにします。


データ Web コントロールが ObjectDataSource を呼び出すときに、このページにアクセスして、`Select`メソッドを呼び出すが、`ProductsBLL`クラスの`GetProductByProductID(productID)`5 用のハード コーディングされた値を使用して、メソッド、`productID`入力パラメーター。 メソッドは厳密に型を返す`ProductDataTable`Chef Anton の Gumbo ミックスに関する情報を含む 1 つの行を格納しているオブジェクト (製品の`ProductID`5)。


[![情報に関する Chef Anton の Gumbo ミックスが表示されます。](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**図 5**: 情報に関する Chef Anton の Gumbo ミックスが表示されます ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image15.png))。


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Web コントロールのプロパティの値にパラメーター値の設定

ObjectDataSource のパラメーターの値を設定することも、ページ上の Web コントロールの値に基づいています。 これを示すためには、ユーザーが指定した国に所在する仕入先のすべてを一覧表示する GridView があてみましょう。 このスタートを得るためには、ユーザーが国の名前を入力できるページにテキスト ボックスを追加します。 このテキスト ボックス コントロールの設定`ID`プロパティを`CountryName`します。 また、ボタンの Web コントロールを追加します。


[![ID CountryName をページにテキスト ボックスを追加します。](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**図 6**: を含むページにテキスト ボックスを追加`ID` `CountryName` ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image18.png))。


次に、ページと、スマート タグの間に、GridView を追加では、新しい ObjectDataSource を追加する選択します。 仕入先情報の選択を表示するため、`SuppliersBLL`ウィザードの最初の画面からのクラス。 2 番目の画面から選択、`GetSuppliersByCountry(country)`メソッド。


[![GetSuppliersByCountry(country) 方法を選択します。](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**図 7**: 選択、`GetSuppliersByCountry(country)`メソッド ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image21.png))。


以降、`GetSuppliersByCountry(country)`メソッドの入力パラメーターには、ウィザードにはもう一度パラメーターの値を選択するための最後の画面が含まれています。 この時点では、パラメーター ソースをコントロールに設定します。 ページ上のコントロールの名前を持つ ControlID ドロップダウン リストが設定されます。選択、`CountryName`一覧からコントロール。 ページが初めてアクセスしたときに、`CountryName`結果が返されないと、何も表示されませんが、テキスト ボックスは空白になります。 既定でいくつかの結果を表示する場合は、DefaultValue textbox を適宜設定します。


[![CountryName コントロールの値にパラメーター値を設定します。](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**図 8**: パラメーターの値を設定、`CountryName`コントロールの値 ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image24.png))。


ObjectDataSource の宣言型マークアップは、最初の例と若干異なってを使用して、 [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx)標準ではなく`Parameter`オブジェクト。 A`ControlParameter`を指定する追加のプロパティを持つ、 `ID` Web コントロールと、パラメーターを使用するプロパティの値の (`PropertyName`)。 データ ソース構成ウィザードが決めることをテキスト ボックスのなります可能性がありますを使用するほど、`Text`パラメーター値のプロパティ。 ただし、Web コントロールから別のプロパティ値を使用する場合は、変更、`PropertyName`値ここで、またはウィザードの [のプロパティの詳細表示] リンクをクリックします。

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

最初に、ページにアクセスしたとき、`CountryName`テキスト ボックスが空です。 ObjectDataSource の`Select`、GridView の値でメソッドが呼び出さ`Nothing`に渡される、`GetSuppliersByCountry(country)`メソッド。 TableAdapter に変換します、`Nothing`をデータベースに`NULL`値 (`DBNull.Value`) が、クエリで使用される、`GetSuppliersByCountry(country)`いずれかが返されないように、メソッドが記述された値、 `NULL` 値が指定されて`@CategoryID`パラメーター。 つまり、サプライヤーは返されません。

訪問者の国、ただし、入力し、するポストバックを発生させる、ObjectDataSource のサプライヤーの表示 ボタンをクリックすると`Select`メソッドをクエリすると、TextBox コントロールの渡して`Text`値として、`country`パラメーター。


[![カナダから業者が表示されます。](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**図 9**: カナダからサプライヤーものが表示されます ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image27.png))。


## <a name="showing-all-suppliers-by-default"></a>既定ですべての仕入先の表示

はなく最初のページを表示するときに なし の仕入先よりも可能性がありますを表示する*すべて*最初は、サプライヤーに、ユーザー ボックスに、国の名前を入力して、一覧を縮小することができます。 テキスト ボックスが空で、`SuppliersBLL`クラスの`GetSuppliersByCountry(country)`メソッドが渡された`Nothing`の*`country`* 入力パラメーター。 これは、 `Nothing` DAL のに値が渡されます`GetSupplierByCountry(country)`メソッドは、データベースに変換されます`NULL`値、`@Country`次のクエリ パラメーター。

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

式`Country = NULL`常に False を返します、レコードの場合でも持つ`Country`列には、`NULL`値。 そのため、レコードは返されません。

返される*すべて*サプライヤー国のテキスト ボックスが空の場合は強化点は、`GetSuppliersByCountry(country)`メソッドを呼び出す BLL、`GetSuppliers()`その国のパラメーターがメソッド`Nothing`と DAL の呼び出しに`GetSuppliersByCountry(country)`メソッドはそれ以外の場合。 国のパラメーターが含まれる場合は、国が指定されていない場合、すべてのサプライヤーと仕入先の適切なサブセットを返すことの効果があります。

変更、`GetSuppliersByCountry(country)`メソッドで、`SuppliersBLL`次のクラス。

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

この変更により、`DeclarativeParams.aspx`初めてアクセスしたときに、仕入先のすべてのページが表示されます (またはたびに、`CountryName`テキスト ボックスが空です)。


[![All は既定で表示されるようになりました](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**図 10**: All は既定で表示されるようになりました ([フルサイズの画像を表示する をクリックします](declarative-parameters-vb/_static/image30.png))。


## <a name="summary"></a>まとめ

ObjectDataSource のパラメーターの値を指定する入力パラメーターを持つメソッドを使用するには`SelectParameters`コレクション。 さまざまな種類のパラメーターのさまざまなソースから取得するパラメーター値を許容します。 既定のパラメーター型が、ハード コーディングされた値を使用しますが、簡単に (およびコードを記述しなくても) と同様、クエリ文字列、セッション変数、cookie、およびページ上の Web コントロールからもユーザーが入力した値からパラメーターの値を取得できます。

このチュートリアルでは見たので、例では、宣言型のパラメーター値を使用する方法を説明します。 ただし、あります回パラメーターのソースで使用できない場合、現在の日付と時刻などを使用するときに、私たちのサイトは、メンバーシップ、訪問者のユーザー ID を使用していた場合。 このようなシナリオ、ObjectDataSource 前にプログラムでパラメーターの値設定し、その基になるオブジェクトのメソッドを呼び出すことができます。 これを実現する方法を見て、[次のチュートリアル](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が、Hilton Giesenow です。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](displaying-data-with-the-objectdatasource-vb.md)
> [次へ](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
