---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: "ページングされたデータ (VB) のカスタムの並べ替え |Microsoft ドキュメント"
author: rick-anderson
description: "前のチュートリアルでは、web ページ上のデータを presentating ときに、カスタム ページングを実装する方法について説明しました。 このチュートリアルでは、前に、拡張する方法を参照しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f7ba21116c2f5f976ffa95955247a49dc5f81e6c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="sorting-custom-paged-data-vb"></a>ページングされたデータ (VB) のカスタムの並べ替え
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe)または[PDF のダウンロード](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> 前のチュートリアルでは、web ページ上のデータを presentating ときに、カスタム ページングを実装する方法について説明しました。 このチュートリアルでは、カスタム ページングを並べ替えをサポートするように前の例を拡張する方法を確認します。


## <a name="introduction"></a>はじめに

既定のページングと比較してカスタム ページングは桁違い、大量のデータをページングするときは、事実上のページングの実装の選択をページング カスタムことによってデータのページングのパフォーマンスを向上することができます。 カスタム ページングを実装することは、既定値のページングは、ただし、並べ替えをミックスに追加するときに特にを実装するよりも複雑です。 このチュートリアルでは並べ替えをサポートするように上記の 1 つの例を拡張*と*カスタム ページングします。

> [!NOTE]
> このチュートリアルは、先頭にはすぐ内で宣言の構文をコピーする前に上記のいずれかに基づいているため、`<asp:Content>`前のチュートリアル s web ページから要素 (`EfficientPaging.aspx`) の間で貼り付けます、 `<asp:Content>` 内の要素`SortParameter.aspx`ページ。 ステップ 1 に戻って、[検証コントロールを編集および挿入するインターフェイスを追加する](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)を別の 1 つの ASP.NET ページの機能をレプリケートする方法については詳細なチュートリアルです。


## <a name="step-1-reexamining-the-custom-paging-technique"></a>手順 1: カスタム ページング手法を再調査します。

正常に動作するカスタム ページングの開始行のインデックスと行の最大数のパラメーターを指定されたレコードの特定のサブセットを取得できる効率的にいくつかの手法を実装お必要があります。 この目的を実現するために使用できる手法のいくつかあります。 これを実現するについて説明しました前のチュートリアルを使用して Microsoft SQL Server 2005 s 新しい`ROW_NUMBER()`順位付け関数。 つまり、`ROW_NUMBER()`行番号を指定した並べ替え順序で順位付けされたが、クエリによって返される行ごとに割り当てる順位付け関数。 レコードの適切なサブセットは、番号付きの結果の特定のセクションを返すことによって取得されます。 次のクエリは、この手法を使用して番号付き 11 ~ 20 のアルファベット順で結果を順位付けとこれらの製品を返す方法を示しています、 `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

この手法は、特定の並べ替え順序を使用してページングのも (`ProductName`ここでは、アルファベット順に並べ替えられます) がクエリを別の並べ替え式によって並べ替えられた結果を表示するように変更する必要があります。 理想的には、上記のクエリ書き直すことができますにパラメーターを使用して、`OVER`句、次のようにします。


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

残念ながら、パラメーター化された`ORDER BY`句は許可されていません。 代わりに、おを受け取るストアド プロシージャを作成する必要があります、`@sortExpression`入力パラメーターは、次の回避策の 1 つを使用します。

- 使用できます。 並べ替え式の各ハード コーディングされたクエリを作成します。使用して、 `IF/ELSE` T-SQL ステートメントを実行するクエリを特定します。
- 使用して、`CASE`動的を提供するステートメント`ORDER BY`式に基づいて、 `@sortExpressio` n の入力パラメーターは、動的にクエリ結果の並べ替え」セクションに使用されているを参照してください[SQL の電源`CASE`ステートメント](http://www.4guysfromrolla.com/webtech/102704-1.shtml)詳細についてはします。
- ストアド プロシージャ内の文字列として、適切なクエリを作成し、使用して[、`sp_executesql`システム ストアド プロシージャ](https://msdn.microsoft.com/en-us/library/ms188001.aspx)動的クエリを実行します。

これらの回避策のそれぞれで、いくつかの欠点があります。 最初のオプションは、管理が容易で、その他の 2 つとして考えられる並べ替え式ごとにクエリを作成することが必要なは。 そのため、後でする場合 GridView に新しいで並べ替え可能なフィールドを追加するも必要に戻って、ストアド プロシージャを更新します。 2 番目の方法には、文字列以外のデータベース列で並べ替える場合は、パフォーマンスの問題を導入し、最初のと同様の保守容易性の問題からも低下する項目がいくつかがあります。 攻撃者が、任意の入力パラメーターの値を渡してストアド プロシージャを実行できる場合、動的 SQL を使用して、3 番目の選択肢が SQL インジェクション攻撃のリスクを紹介します。

どの方法が最適な 3 番目のオプションは、3 つの最適なと思われます。 動的 SQL の使用、柔軟性は、その他の 2 つのレベルを提供します。 さらに、SQL インジェクション攻撃は、攻撃者が任意の入力パラメーターを渡してストアド プロシージャを実行する場合にのみ利用可能です。 DAL では、パラメーター化クエリを使用するため、ADO.NET は SQL インジェクション攻撃の脆弱性は、攻撃者が、ストアド プロシージャを直接実行する場合のみが存在するので、アーキテクチャを使用してデータベースに送信されるこれらのパラメーターで保護されます。

この機能を実装するには、Northwind データベースの名前付きで、新しいストアド プロシージャを作成`GetProductsPagedAndSorted`です。 このストアド プロシージャは 3 つの入力パラメーターを受け入れる必要があります: `@sortExpression`、型の入力パラメーター `nvarchar(100`) 結果の並べ替える必要があり、直後に挿入されたを指定する、`ORDER BY`内のテキスト、 `OVER` と句です。`@startRowIndex`と`@maximumRows`から、同じ 2 つの整数入力パラメーター、`GetProductsPaged`ストアド プロシージャの前のチュートリアルで確認します。 作成、`GetProductsPagedAndSorted`ストアド プロシージャは、次のスクリプトを使用します。


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

値を確認して、ストアド プロシージャを起動、`@sortExpression`パラメーターが指定されています。 によって結果が順位付けされることが見つからない場合、`ProductID`です。 次に、動的な SQL クエリを構築します。 動的な SQL クエリをここでわずかに異なる、Products テーブルからすべての行を取得するため、以前のクエリに注意してください。 前の例については、サブクエリを使用して業者の s 名を各製品 s 関連付けられているカテゴリに取得されます。 この決定をしたに戻り、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-vb.md)チュートリアルし、代わりに使用して行った`JOIN`の TableAdapter が関連付けられている insert を自動的に作成できないために更新、および削除方法などのクエリです。 `GetProductsPagedAndSorted`ただし、ストアド プロシージャを使用する必要があります`JOIN`秒であり、結果をカテゴリまたは仕入先名順に並べ替えられます。

この動的なクエリは静的クエリ部分を連結することによって構築され、 `@sortExpression`、 `@startRowIndex`、および`@maximumRows`パラメーター。 `@startRowIndex`と`@maximumRows`整数パラメーターは、正しくを連結するために nvarchars に変換する必要があります。 使用して実行されるこの動的な SQL クエリを構築すると、`sp_executesql`です。

このストアド プロシージャに異なる値をテストする、 `@sortExpression`、 `@startRowIndex`、および`@maximumRows`パラメーター。 サーバー エクスプ ローラーからストアド プロシージャの名前を右クリックし、[実行] を選択します。 これは、先 (図 1 を参照してください) は、入力パラメーターを入力することができます、ストアド プロシージャの実行 ダイアログ ボックスが表示されます。 カテゴリ名で結果を並べ替えるには、区分を使用、`@sortExpression`業者の会社名での並べ替え、CompanyName を使用するパラメーター値。 パラメーターの値を入力すると、[ok] をクリックします。 結果は、出力ウィンドウに表示されます。 図 2 で順序付けするときに、11 ~ 20 を順位付けされる製品を返すときに結果を示しています、`UnitPrice`降順にします。


![ストアド プロシージャは次の 3 つ入力パラメーターを異なる値をお試しください。](sorting-custom-paged-data-vb/_static/image1.png)

**図 1**: ストアド プロシージャの 3 つの入力パラメーターに対して異なる値を再試行してください


[![ストアド プロシージャの結果は、出力ウィンドウに表示されます。](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**図 2**:、ストアド プロシージャの結果は、出力ウィンドウに表示されます ([フルサイズのイメージを表示するをクリックして](sorting-custom-paged-data-vb/_static/image4.png))


> [!NOTE]
> 指定した結果を順位付けと`ORDER BY`内の列、`OVER`句、SQL Server は、結果を並べ替える必要があります。 これは、結果は並べされている列にクラスター化インデックスがある場合は、クイック操作または、カバリングがあるかどうか、インデックスでもかまいません高額それ以外の場合。 十分な大きさのクエリのパフォーマンスを向上させるには、これによって、結果は並べ 列の非クラスター化インデックスを追加することを検討してください。 参照してください[SQL Server 2005 でのパフォーマンスと順位付け関数](http://www.sql-server-performance.com/ak_ranking_functions.asp)詳細についてはします。


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>手順 2: データ アクセス層とビジネス ロジック層の拡張

`GetProductsPagedAndSorted`マイクロソフト アプリケーション アーキテクチャを使用するストアド プロシージャを実行するための手段を提供する次の手順は、作成したストアド プロシージャ、します。 これは、DAL と BLL の両方に適切なメソッドを追加する必要があります。 S、DAL にメソッドを追加することによって開始できるようにします。 開く、`Northwind.xsd`型指定されたデータセットを右クリックし、 `ProductsTableAdapter`、コンテキスト メニューから、クエリの追加オプションを選択します。 -既存のストアド プロシージャを使用するこの新しい DAL 方法を構成する前のチュートリアルで行ったよう`GetProductsPagedAndSorted`、ここでします。 既存のストアド プロシージャを使用する新しい TableAdapter メソッドにすることを示すを開始します。


![既存のストアド プロシージャを使用します。](sorting-custom-paged-data-vb/_static/image5.png)

**図 3**: 既存のストアド プロシージャを使用します。


使用するストアド プロシージャを指定するには、選択、`GetProductsPagedAndSorted`次の画面で、ドロップダウン リストからプロシージャを格納します。


![GetProductsPagedAndSorted を使用してストアド プロシージャ](sorting-custom-paged-data-vb/_static/image6.png)

**図 4**: GetProductsPagedAndSorted を使用してストアド プロシージャ


このストアド プロシージャは、その結果、次の画面でわかるように表形式のデータが返されることに、レコードのセットを返します。


![ストアド プロシージャが表形式のデータを返すことを示します](sorting-custom-paged-data-vb/_static/image7.png)

**図 5**: ストアド プロシージャが表形式のデータを返すことを示します


最後に、両方の塗りつぶしを使用する DAL メソッドに DataTable を作成し、メソッドの名前を付け、DataTable パターンを返す`FillPagedAndSorted`と`GetProductsPagedAndSorted`、それぞれします。


![メソッドの名前を選択します。](sorting-custom-paged-data-vb/_static/image8.png)

**図 6**: メソッドの名前を選択


これまで ve 拡張 DAL お BLL を有効にする準備が整いました。 開く、`ProductsBLL`クラス ファイルと新しいメソッドを追加`GetProductsPagedAndSorted`です。 このメソッドは、3 つの入力パラメーターを受け入れる必要がある`sortExpression`、 `startRowIndex`、および`maximumRows`DAL s にだけ呼び出す必要があります`GetProductsPagedAndSorted`メソッド、次のようにします。


[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>手順 3: SortExpression パラメーターに渡す ObjectDataSource を構成します。

使用するメソッドを含めるには、DAL と BLL が補完、`GetProductsPagedAndSorted`で ObjectDataSource を構成するのには、残っているストアド プロシージャは、`SortParameter.aspx`に渡す新しい BLL メソッドを使用する ページ、`SortExpression`パラメーターに基づいている、ユーザーが要求して、結果を並べ替える列です。

ObjectDataSource s を変更することによって開始`SelectMethod`から`GetProductsPaged`に`GetProductsPagedAndSorted`です。 これは、データ ソースの構成ウィザードの [プロパティ] ウィンドウから、または宣言構文から直接実行できます。 次に、ObjectDataSource 秒の値を指定する必要があります[`SortParameterName`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx)です。 ObjectDataSource が GridView s に渡すしようとした場合、このプロパティを設定すると、`SortExpression`プロパティを`SelectMethod`です。 名前を持つがの値と等しい入力パラメーター、ObjectDataSource が具体的には、検索、`SortParameterName`プロパティです。 BLL s 以降`GetProductsPagedAndSorted`メソッドがという名前の並べ替え式の入力パラメーターを持つ`sortExpression`、集合 ObjectDataSource の`SortExpression`sortExpression にプロパティです。

これら 2 つの変更を加えたら、ObjectDataSource s 宣言の構文を次のようになります。


[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> 前のチュートリアルで確実にある、ObjectDataSource として*いない*その selectparameters のどのコレクションに、sortExpression、startRowIndex、または maximumRows 入力パラメーターを含めます。


GridView で並べ替えを有効にするだけで、並べ替えを有効にするチェック ボックスをオン GridView の s スマート タグには、GridView s の設定で`AllowSorting`プロパティを`true`LinkButton として表示するには、各列のヘッダー テキストを原因とします。 エンドユーザーは、あるヘッダーの 1 つでクリックするは、ポストバックが引き続き行われ、次の手順までに経過します。

1. GridView の更新プログラム、 [ `SortExpression`プロパティ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)の値に、`SortExpression`のリンクを持つヘッダーがクリックしてされたフィールドの
2. ObjectDataSource 呼び出します BLL s `GetProductsPagedAndSorted` GridView s に渡して、メソッド`SortExpression`プロパティ、メソッドの値として`sortExpression`入力パラメーター (と共に、適切な`startRowIndex`と`maximumRows`入力パラメーターの値)
3. BLL 呼び出します DAL の`GetProductsPagedAndSorted`メソッド
4. DAL の実行、`GetProductsPagedAndSorted`ストアド プロシージャを渡すことで、`@sortExpression`パラメーター (と共に、`@startRowIndex`と`@maximumRows`入力パラメーターの値)
5. ストアド プロシージャは、ObjectDataSource; を返す BLL に適切なデータのサブセットを返すこのデータが GridView にバインドされている、html 形式にレンダリングし、エンドユーザーに送信

図 7 で並べ替えられた結果の最初のページを示しています、`UnitPrice`で昇順に並べ替えます。


[![結果は、UnitPrice によって並べ替えられます。](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**図 7**: 結果は、UnitPrice によって並べ替えられます ([フルサイズのイメージを表示するをクリックして](sorting-custom-paged-data-vb/_static/image11.png))


現在の実装では、製品名、カテゴリ、単位、および単価ごとの数によって、結果を正しく並べ替えることができます、中に、ランタイム例外で、結果は業者によって名前の結果の並べ替えにしようとしています (図 8 を参照してください)。


![次のランタイム例外で業者の結果で結果を並べ替えるしようとしています。](sorting-custom-paged-data-vb/_static/image12.png)

**図 8**: 次のランタイム例外で業者の結果で結果を並べ替えるしようとしています。


この例外が発生、 `SortExpression` GridView s の`SupplierName`BoundField に設定されている`SupplierName`です。 ただし、s 供給業者の名前、`Suppliers`テーブルが実際に呼び出される`CompanyName`きましたエイリアスとしてこの列名`SupplierName`です。 ただし、`OVER`句で使用される、`ROW_NUMBER()`関数のエイリアスを使用することはできず、実際の列名を使用する必要があります。 そのため、変更、 `SupplierName` BoundField の`SortExpression`CompanyName を仕入から (図 9 を参照してください)。 図 10 では、この変更を業者によって結果を並べ替えることができます。


![CompanyName を仕入 BoundField の SortExpression に変更します。](sorting-custom-paged-data-vb/_static/image13.png)

**図 9**: CompanyName を仕入 BoundField の SortExpression に変更


[![業者によって、結果を並べ替えるようになりましたことができます。](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**図 10**:「結果これで並べ替えることができます Supplier ([フルサイズのイメージを表示するには、をクリックして](sorting-custom-paged-data-vb/_static/image16.png))


## <a name="summary"></a>概要

デザイン時に、結果が並べ替え順序を指定する、カスタム ページングの実装前のチュートリアルで調べることが必要です。 つまり、これは、カスタム ページングの実装を実装しました提供できなかったことを同時に並べ替え機能意味していました。 このチュートリアルでは、最初に含めるストアド プロシージャを拡張することによってこのような制限をたかお、`@sortExpression`入力パラメーターの結果を並べ替える可能性があります。

両方の並べ替えを提供する GridView を実装することとページングを渡す GridView s 現在 ObjectDataSource を構成することによってカスタムでしたこれを作成するにはストアド プロシージャと、DAL と BLL で新しいメソッドを作成するが、後`SortExpression`BLL プロパティ。`SelectMethod`.

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Carlos Santos しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](efficiently-paging-through-large-amounts-of-data-vb.md)
[次へ](creating-a-customized-sorting-user-interface-vb.md)
