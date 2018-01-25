---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
title: "宣言型のパラメーター (c#) |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、ハード コーディングされた値に設定パラメーターを使用して、DetailsView コントロールに表示するデータを選択する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 603c9bd3-b895-4ec6-853b-0c81ff36d580
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
msc.type: authoredcontent
ms.openlocfilehash: 4bbe32ae65f71196fc1f939671b9d1a24bee8c34
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="declarative-parameters-c"></a>宣言型のパラメーター (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_5_CS.exe)または[PDF のダウンロード](declarative-parameters-cs/_static/datatutorial05cs1.pdf)

> このチュートリアルでは、ハード コーディングされた値に設定パラメーターを使用して、DetailsView コントロールに表示するデータを選択する方法について説明します。


## <a name="introduction"></a>はじめに

[前回のチュートリアル](displaying-data-with-the-objectdatasource-cs.md)、GridView、DetailsView、フォーム ビュー コントロールを呼び出した ObjectDataSource コントロールにバインドされているとデータの表示について説明しました、`GetProducts()`メソッドから、`ProductsBLL`クラスです。 `GetProducts()`メソッドは、設定、すべてのレコードから、Northwind データベースの厳密に型指定された DataTable を返します`Products`テーブル。 `ProductsBLL`クラスには、その他の商品の単なるサブセットを返すメソッドが含まれています。 `GetProductByProductID(productID)`、 `GetProductsByCategoryID(categoryID)`、および`GetProductsBySupplierID(supplierID)`です。 これら 3 つの方法では、返された製品情報をフィルター処理する方法を示す、入力パラメーターを想定します。

ためにこれらのパラメーターの値がどこから得られる指定する必要がありますが、入力パラメーターは、期待するメソッドを呼び出す、ObjectDataSource を使用できます。 パラメーターの値がハードコーディングすることができますか、さまざまななど、動的なソースから取得できます。 クエリ文字列値、セッション変数、またはその他のページ上の Web コントロールのプロパティの値。

このチュートリアルでは、ハード コーディングされた値に設定パラメーターを使用する方法を示すでスタートしましょう。 具体的には、説明、つまり Chef Anton Gumbo ミックス、ある特定の製品に関する情報を表示するページへの DetailsView の追加、`ProductID`は 5 です。 次に、Web コントロールに基づくパラメーターの値を設定する方法を会いしましょう。 具体的には、ユーザーがその後、その国にある仕入先の一覧を表示するのにボタンをクリックしたことができます、国に入力できるテキスト ボックスを使用します。

## <a name="using-a-hard-coded-parameter-value"></a>パラメーターのハードコード値を使用します。

最初の例では、DetailsView コントロールを追加することによって、起動、 `DeclarativeParams.aspx`  ページで、`BasicReporting`フォルダーです。 DetailsView のスマート タグから選択&lt;新しいデータ ソース&gt;ドロップダウン リストで一覧表示し、ObjectDataSource を追加します。


[![ページに、ObjectDataSource を追加します。](declarative-parameters-cs/_static/image2.png)](declarative-parameters-cs/_static/image1.png)

**図 1**: ObjectDataSource をページに追加 ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image3.png))


ObjectDataSource コントロールのデータ ソースの選択ウィザードが自動的に開始されます。 選択、`ProductsBLL`ウィザードの最初の画面からクラスです。


[![ProductsBLL クラスを選択します。](declarative-parameters-cs/_static/image5.png)](declarative-parameters-cs/_static/image4.png)

**図 2**: 選択、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image6.png))


使用する特定の製品に関する情報を表示するので、`GetProductByProductID(productID)`メソッドです。


[![GetProductByProductID(productID) 方法を選択します。](declarative-parameters-cs/_static/image8.png)](declarative-parameters-cs/_static/image7.png)

**図 3**: 選択、`GetProductByProductID(productID)`メソッド ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image9.png))


選択したメソッドには、パラメーターが含まれているため場所お尋ね、パラメーターに使用する値を定義すると、ウィザードの 1 つ以上の画面があります。 左側の一覧には、選択したメソッドのパラメーターのすべてが表示されます。 `GetProductByProductID(productID)`が 1 つだけある`productID`です。 右側の選択したパラメーターの値を指定できます。 パラメーターのソースのドロップダウン リストでは、パラメーター値のさまざまな可能なソースを列挙します。 5 用のハード コーディングされた値を指定するので、`productID`パラメーター、None とパラメーターのソースのままにし、DefaultValue ボックスに「5」と入力します。


[![Hard-Coded パラメーター値の 5 を使用するパラメーター productID の](declarative-parameters-cs/_static/image11.png)](declarative-parameters-cs/_static/image10.png)

**図 4**: A Hard-Coded パラメーター値の 5 を使用するため、`productID`パラメーター ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image12.png))


ObjectDataSource コントロールの宣言型マークアップを含むデータ ソース構成ウィザードを完了すると、`Parameter`内のオブジェクト、`SelectParameters`コレクションで定義されたメソッドで想定されている入力パラメーターのそれぞれについて、 `SelectMethod`プロパティ。 この例でを使用してメソッドがのみ、1 つの入力パラメーターを受け取るため`parameterID`、ここで 1 つだけのエントリが存在します。 `SelectParameters`コレクションから派生した任意のクラスを含めることができます、`Parameter`クラス内で、`System.Web.UI.WebControls`名前空間。 パラメーターのハードコード値ベースの`Parameter`クラスを使用するは、他のパラメーターのソース オプション、派生`Parameter`クラスが使用されます。 独自に作成することも[カスタム パラメーターの型](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11)、必要に応じて。

[!code-aspx[Main](declarative-parameters-cs/samples/sample1.aspx)]

> [!NOTE]
> 月の値を含める表示この時点でいる場合はしているコンピューターで、宣言型マークアップ、 `InsertMethod`、`UpdateMethod`と`DeleteMethod`プロパティだけでなく`DeleteParameters`です。 ObjectDataSource のデータ ソースの選択ウィザードからのメソッドを自動的に指定する、`ProductBLL`に使用する挿入、更新、および削除すると、明示的にオフにしたものをしない限り、上記のマークアップで含めるをできるようにします。


Web コントロールのデータが、ObjectDataSource を呼び出すこのページを訪問する際に`Select`メソッドの呼び出しは、`ProductsBLL`クラスの`GetProductByProductID(productID)`5 用のハード コーディングされた値を使用して、メソッド、`productID`入力パラメーターです。 メソッドは、厳密に型指定されたを返します`ProductDataTable`Chef Anton Gumbo ミックスに関する情報を含む 1 つの行を格納しているオブジェクト (プロダクトの product `ProductID` 5)。


[![情報は Chef Anton の Gumbo ミックスが表示されます。](declarative-parameters-cs/_static/image14.png)](declarative-parameters-cs/_static/image13.png)

**図 5**: 情報は Chef Anton の Gumbo ミックスが表示されます ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>パラメーター値を Web コントロールのプロパティの値に設定

値を設定することも、ObjectDataSource のパラメーターは、ページ上の Web コントロールの値に基づいています。 これを示すためには、すべてのユーザーによって指定された国に配置されている仕入先の一覧を表示する GridView があてみましょう。 ユーザーが国の名前を入力できるページにテキスト ボックスを追加することによってこの開始を実行します。 このテキスト ボックス コントロールの設定`ID`プロパティを`CountryName`です。 またボタン Web コントロールを追加します。


[![ID CountryName を含むページにテキスト ボックスを追加します。](declarative-parameters-cs/_static/image17.png)](declarative-parameters-cs/_static/image16.png)

**図 6**: テキスト ボックスを使用してページに追加`ID` `CountryName` ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image18.png))


次に、ページと、スマート タグから GridView を追加では、新しい ObjectDataSource の追加を選択します。 仕入先についての選択を表示するので、`SuppliersBLL`ウィザードの最初の画面からのクラスです。 2 番目の画面では、次のように選択します。、`GetSuppliersByCountry(country)`メソッドです。


[![GetSuppliersByCountry(country) 方法を選択します。](declarative-parameters-cs/_static/image20.png)](declarative-parameters-cs/_static/image19.png)

**図 7**: 選択、`GetSuppliersByCountry(country)`メソッド ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image21.png))


以降、`GetSuppliersByCountry(country)`メソッドの入力パラメーターには、ウィザードにはもう一度、パラメーターの値を選択するための最後の画面が含まれています。 この時点では、パラメーターのソースをコントロールに設定します。 これは、ページ上のコントロールの名前を持つ ControlID ドロップダウン リストが設定されます。選択、`CountryName`一覧から管理します。 ページを最初に開いたとき、 `CountryName`  テキスト ボックスは空白になります、結果が返されないため、何も表示されません。 既定でいくつかの結果を表示する場合は、DefaultValue textbox を適宜設定します。


[![CountryName コントロール値に、パラメーターの値を設定します。](declarative-parameters-cs/_static/image23.png)](declarative-parameters-cs/_static/image22.png)

**図 8**: に、パラメーターの値を設定、`CountryName`制御値 ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image24.png))


ObjectDataSource の宣言型マークアップと若干異なり、最初の例を使用して、 [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx)標準ではなく`Parameter`オブジェクト。 A`ControlParameter`を指定する追加のプロパティを持つ、 `ID` Web コントロールと、パラメーターで使用するプロパティ値の (`PropertyName`)。 データ ソース構成ウィザードが、テキスト ボックス、おすることが使用を決定するのに十分なスマート、`Text`パラメーター値のプロパティです。 ただし、Web コントロールから別のプロパティ値を使用する場合は、変更、`PropertyName`値ここで、またはウィザードで「詳細プロパティの表示設定」リンクをクリックします。

[!code-aspx[Main](declarative-parameters-cs/samples/sample2.aspx)]

最初に、ページにアクセスしたとき、 `CountryName`  テキスト ボックスが空です。 ObjectDataSource の`Select`GridView の値でメソッドが呼び出されますも`null`に渡される、`GetSuppliersByCountry(country)`メソッドです。 TableAdapter に変換、`null`データベースに`NULL`値 (`DBNull.Value`) で使用されるクエリでは、`GetSuppliersByCountry(country)`メソッドがいずれかが返されないように記述された値、 `NULL` の値を指定`@CategoryID`パラメーター。 つまり、サプライヤーは返されません。

訪問者の国、ただし、入力し、ポストバックを発生させる、ObjectDataSource のサプライヤーの表示ボタンをクリックした後`Select`メソッドを表示すると、テキスト ボックス コントロールの引き渡し`Text`値として、`country`パラメーター。


[![カナダから業者が表示されます。](declarative-parameters-cs/_static/image26.png)](declarative-parameters-cs/_static/image25.png)

**図 9**: カナダからサプライヤーものが表示されます ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>既定ですべてのサプライヤーの表示

はなく最初のページを表示するときに表示なしサプライヤーのより可能性がありますを表示する*すべて* ボックスで、国の名前を入力して一覧を縮小するユーザーを許可するのには最初、suppliers です。 テキスト ボックスが空で、ときに、`SuppliersBLL`クラスの`GetSuppliersByCountry(country)`メソッドが渡された、`null`値をその *`country`* 入力パラメーターです。 これは、`null`値は、に、DAL の`GetSupplierByCountry(country)`メソッド、データベースに変換されます`NULL`値を`@Country`次のクエリ パラメーター。

[!code-sql[Main](declarative-parameters-cs/samples/sample3.sql)]

式`Country = NULL`常に False を返します、レコードにもある`Country`列には、`NULL`値です。 そのため、レコードは返されません。

返される*すべて*仕入先国 テキスト ボックスが空の場合は、お補強できます、`GetSuppliersByCountry(country)`メソッドを呼び出す BLL で、`GetSuppliers()`メソッドは、その国のパラメーターと`null`と DAL の呼び出しに`GetSuppliersByCountry(country)`。メソッドはそれ以外の場合。 国のパラメーターが含まれるときに、国が指定されていない場合、すべてのサプライヤーと仕入先の適切なサブセットを返すの効果があります。

変更、`GetSuppliersByCountry(country)`メソッドで、`SuppliersBLL`には、次のクラス。

[!code-csharp[Main](declarative-parameters-cs/samples/sample4.cs)]

この変更により、`DeclarativeParams.aspx`ページが最初にアクセスされるときに、仕入先のすべてを表示 (またはたびに、 `CountryName`  テキスト ボックスが空です)。


[![All は既定で表示されるようになりました](declarative-parameters-cs/_static/image29.png)](declarative-parameters-cs/_static/image28.png)

**図 10**: All は既定で表示されるようになりました ([フルサイズのイメージを表示するをクリックして](declarative-parameters-cs/_static/image30.png))


## <a name="summary"></a>まとめ

入力パラメーターを持つメソッドを使用するために必要があります、objectdatasource のパラメーターの値を指定する`SelectParameters`コレクション。 異なる型のパラメーターの異なるソースから取得されるパラメーター値を許容します。 既定のパラメーター型が、ハード コーディングされた値を使用しますが、簡単に (およびコードの行がない) と同様、クエリ文字列、セッション変数、cookie、およびページ上のコントロールを Web からもユーザーが入力した値からパラメーターの値を取得できます。

このチュートリアルで見ておの例では、宣言型のパラメーター値を使用する方法を説明します。 ただし、場合がありますしなければならない場合は、現在の日付と時間など、使用できないパラメーター ソースを使用する必要がありますか、このサイトは、メンバーシップ、訪問者のユーザー ID を使用していた場合。 そのようなシナリオおことができます、パラメーター値を設定、ObjectDataSource 前にプログラムでその基になるオブジェクトのメソッドを呼び出します。 これを実現する方法をお見せ、[次のチュートリアル](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)です。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Hilton Giesenow しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](displaying-data-with-the-objectdatasource-cs.md)
[次へ](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
