---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: ページングされたデータ (c#) をカスタムの並べ替え |Microsoft Docs
author: rick-anderson
description: 前のチュートリアルでは、web ページ上のデータを presentating 場合に、カスタム ページングを実装する方法について説明しました。 このチュートリアルでは、前に、拡張する方法を見る.
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 8057b11f9d2d812b9b2bf8d9e016ed17cff4e2f2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816747"
---
<a name="sorting-custom-paged-data-c"></a>ページングされたデータ (c#) をカスタムの並べ替え
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe)または[PDF のダウンロード](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> 前のチュートリアルでは、web ページ上のデータを presentating 場合に、カスタム ページングを実装する方法について説明しました。 このチュートリアルではカスタム ページングを並べ替えをサポートするように前の例を拡張する方法を参照してください。


## <a name="introduction"></a>はじめに

既定のページングと比較してカスタム ページングは、カスタムの大量のデータをページングするときに、業界標準のページングの実装の選択をページングを行ういくつかの注文の大きさによってデータのページングのパフォーマンスを向上させることができます。 カスタム ページングを実装すると、既定のページング、ただし、並べ替えをミックスに追加するときに特にを実装するよりも複雑です。 並べ替えをサポートするように前の例では、このチュートリアルでは拡張*と*カスタム ページングします。

> [!NOTE]
> このチュートリアルは、先頭は、内で宣言型構文をコピーする少し前に、上記の 1 つに基づいているため、`<asp:Content>`前のチュートリアルの web ページから要素 (`EfficientPaging.aspx`) の間で貼り付けます、 `<asp:Content>` 内の要素`SortParameter.aspx`ページ。 手順 1. に戻って、[編集および挿入インターフェイスに検証コントロールを追加](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)別に 1 つの ASP.NET ページの機能をレプリケートする方法については詳細なチュートリアルです。


## <a name="step-1-reexamining-the-custom-paging-technique"></a>手順 1: カスタム ページング手法を再調査します。

適切に機能するカスタム ページング、開始行インデックスと行の最大数のパラメーターを指定されたレコードの特定のサブセットを取得できる効率的にいくつかの手法を実装します必要があります。 この目標を達成するために使用できる手法のいくつかあります。 これを実現しました前のチュートリアルで使用して Microsoft SQL Server 2005 の新しい`ROW_NUMBER()`順位付け関数。 つまり、`ROW_NUMBER()`によって指定された並べ替え順序が付けられます。 順位クエリによって返される行ごとに行番号を割り当てます順位付け関数。 レコードの適切なサブセットは、番号付きの結果の特定のセクションを返すことによって取得されます。 次のクエリは、これらの製品でアルファベット順で結果を順位付けと 11 ~ 20 の番号を返すこの手法を使用する方法を示しています、 `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

この手法は、特定の並べ替え順序を使用してページングのも (`ProductName`ここでは、アルファベット順に並べ替えられます)、クエリを別の並べ替え式で並べ替えられた結果を表示するように変更する必要があります。 理想的でのパラメーターを使用して、上記のクエリを書き換えることができます、`OVER`句では、次のようにします。


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

残念ながら、パラメーター化された`ORDER BY`句は許可されていません。 代わりを受け取るストアド プロシージャを作成する必要があります、`@sortExpression`の入力パラメーターですが次の回避策の 1 つを使用します。

- 各使用可能性があります。 並べ替え式をクエリのハード コーディングされた記述します。使用して、 `IF/ELSE` T-SQL ステートメントを実行するクエリを確認します。
- 使用して、`CASE`動的を提供するステートメント`ORDER BY`式に基づいて、 `@sortExpressio` n の入力パラメーターは、動的にクエリ結果の並べ替え セクションに使用されているを参照してください[SQL の電源`CASE`ステートメント](http://www.4guysfromrolla.com/webtech/102704-1.shtml)詳細についてはします。
- ストアド プロシージャ内の文字列として、適切なクエリを作成し、使用して[、`sp_executesql`システム ストアド プロシージャ](https://msdn.microsoft.com/library/ms188001.aspx)動的クエリを実行します。

これらの回避策のそれぞれは、いくつかの欠点があります。 最初のオプションは、管理が容易で他の 2 つとして考えられる並べ替え式ごとにクエリを作成する必要があるではありません。 そのため、後で、GridView に、並べ替え可能な新しいフィールドを追加する場合はも必要に戻るし、ストアド プロシージャを更新します。 2 番目の方法では、文字列以外のデータベース列で並べ替える場合は、パフォーマンスの問題を紹介し、1 番目と同じの保守に関する問題に苦しんでも項目がいくつかがあります。 攻撃者が、任意の入力パラメーターの値を渡してストアド プロシージャを実行できる場合、動的 SQL を使用して、3 番目の選択肢が、SQL インジェクション攻撃のリスクを紹介します。

どの方法が最適な 3 番目のオプションは、3 つの最適なと思います。 動的 SQL を使用するとその他の 2 つがしない柔軟性のレベルを提供します。 さらに、SQL インジェクション攻撃は、攻撃者は、任意の入力パラメーターを渡してストアド プロシージャを実行することが場合にのみ利用可能です。 DAL では、パラメーター化クエリを使用するため、ADO.NET は SQL インジェクション攻撃の脆弱性は、攻撃者が、ストアド プロシージャを直接実行する場合のみが存在するので、アーキテクチャを使用して、データベースに送信されるこれらのパラメーターで保護されます。

この機能を実装するには、Northwind データベースの名前付きで新しいストアド プロシージャを作成`GetProductsPagedAndSorted`です。 このストアド プロシージャは 3 つの入力パラメーターを受け入れる必要があります: `@sortExpression`、型の入力パラメーター `nvarchar(100`)、結果の並べ替えするかし、直後に挿入されますを指定する、`ORDER BY`内のテキスト、`OVER`句; と`@startRowIndex`と`@maximumRows`から、同じ 2 つの整数入力パラメーター、`GetProductsPaged`ストアド プロシージャの前のチュートリアルで調査します。 作成、`GetProductsPagedAndSorted`ストアド プロシージャの次のスクリプトを使用します。


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

値のことを確認することによって、ストアド プロシージャを開始、`@sortExpression`パラメーターが指定されています。 不足している場合は、結果が順`ProductID`します。 次に、動的な SQL クエリが構築されます。 動的な SQL クエリをここでわずかに異なる Products テーブルからすべての行を取得するため、前のクエリに注意してください。 前の例については、サブクエリを使用して業者の s 名を各製品の関連付けられているカテゴリに取得します。 この決定が行われた、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルが代わりに使用して行われたと`JOIN`s、TableAdapter は、関連付けられている insert、自動的に作成できませんので、更新、および削除などのメソッドクエリ。 `GetProductsPagedAndSorted`ただし、ストアド プロシージャを使用する必要があります`JOIN`の結果をカテゴリまたは仕入先名順に並べ替えられます。

静的クエリ部分を連結することによってこの動的なクエリが作成され、 `@sortExpression`、 `@startRowIndex`、および`@maximumRows`パラメーター。 `@startRowIndex`と`@maximumRows`整数パラメーターが正しく連結するには nvarchars に変換する必要があります。 この動的な SQL クエリが構築されると、使用して実行されるが`sp_executesql`します。

このストアド プロシージャに異なる値をテストする少し、 `@sortExpression`、 `@startRowIndex`、および`@maximumRows`パラメーター。 サーバー エクスプ ローラーで、ストアド プロシージャ名を右クリックし、実行を選択します。 (図 1 参照)、入力パラメーターを入力する先のストアド プロシージャの実行 ダイアログ ボックスが表示されます。 カテゴリ名で結果を並べ替えるの区分を使用して、 `@sortExpression` s 仕入先の会社名での並べ替え、CompanyName を使用するパラメーター値。 パラメーターの値を指定したら、[ok] をクリックします。 結果は、出力ウィンドウに表示されます。 図 2 で順序付けするときに、11 ~ 20 をランク付けされた製品を返すときに結果を示しています、`UnitPrice`降順にします。


![ストアド プロシージャの 3 つ入力パラメーターを異なる値をお試しください。](sorting-custom-paged-data-cs/_static/image1.png)

**図 1**: ストアド プロシージャの 3 つの入力パラメーターに異なる値をお試しください


[![ストアド プロシージャの結果は、出力ウィンドウに表示されます。](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**図 2**:、ストアド プロシージャの結果は、出力ウィンドウに表示されます ([フルサイズの画像を表示する をクリックします](sorting-custom-paged-data-cs/_static/image4.png))。


> [!NOTE]
> 指定した結果を順位付けと`ORDER BY`内の列、`OVER`句では、SQL Server は、結果を並べ替える必要があります。 これは、結果は並べ替え対象の列にクラスター化インデックスがある場合は、クイック操作またはカバリングがあるかどうか、インデックスしますが、それ以外の場合コストが高くすることができます。 十分な大きさのクエリのパフォーマンスを向上させるのには、によって使用される結果が順序付けられた列の非クラスター化インデックスを追加を検討してください。 参照してください[順位付け関数と SQL Server 2005 のパフォーマンス](http://www.sql-server-performance.com/ak_ranking_functions.asp)の詳細。


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>手順 2: データ アクセスとビジネス ロジック層の拡張

`GetProductsPagedAndSorted`ストアド プロシージャを作成、次の手順は、そのストアド プロシージャは、アプリケーションのアーキテクチャを実行するための手段を提供します。 これは、DAL と BLL の両方に、適切なメソッドを追加する必要があります。 S DAL に対するメソッドを追加することで開始できるようにします。 開く、`Northwind.xsd`型指定されたデータセットを右クリックし、 `ProductsTableAdapter`、コンテキスト メニューから、クエリの追加オプションを選択します。 -既存のストアド プロシージャを使用するこの新しい DAL 方法を構成する前のチュートリアルで行ったよう`GetProductsPagedAndSorted`、ここでします。 既存のストアド プロシージャを使用する新しい TableAdapter メソッドにすることを指定して起動します。


![既存のストアド プロシージャを使用します。](sorting-custom-paged-data-cs/_static/image5.png)

**図 3**: 既存のストアド プロシージャを使用します。


使用するストアド プロシージャを指定するには、選択、`GetProductsPagedAndSorted`ドロップダウン リストから手順を次の画面に格納されています。


![GetProductsPagedAndSorted を使用してストアド プロシージャ](sorting-custom-paged-data-cs/_static/image6.png)

**図 4**: GetProductsPagedAndSorted を使用してストアド プロシージャ


このストアド プロシージャは、その結果、次の画面で示す表形式のデータが返されるレコードのセットを返します。


![ストアド プロシージャが表形式のデータを返すことを示します](sorting-custom-paged-data-cs/_static/image7.png)

**図 5**: ストアド プロシージャが表形式のデータを返すことを示します


最後に、DataTable の塗りつぶしの両方を使用する DAL メソッドを作成し、メソッドの名前を付け、DataTable のパターンを返す`FillPagedAndSorted`と`GetProductsPagedAndSorted`、それぞれします。


![メソッド名を選択します。](sorting-custom-paged-data-cs/_static/image8.png)

**図 6**: メソッドの名前を選択


これまで ve 拡張、DAL BLL に有効にする準備が整いました。 開く、`ProductsBLL`クラス ファイルと、新しいメソッドを追加`GetProductsPagedAndSorted`します。 このメソッドは、3 つの入力パラメーターを受け入れる必要がある`sortExpression`、 `startRowIndex`、および`maximumRows`DAL s にだけ呼び出す必要がありますと`GetProductsPagedAndSorted`メソッドでは、次のように。


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>手順 3: SortExpression パラメーターに渡す ObjectDataSource を構成します。

使用するメソッドを含めるには、DAL と BLL を拡張すること、 `GetProductsPagedAndSorted` 、残っているストアド プロシージャがで ObjectDataSource を構成するのには、`SortParameter.aspx`ページに渡すと新しい BLL メソッドを使用して、`SortExpression`パラメーターがに基づいて、ユーザーが要求して、結果を並べ替える列。

ObjectDataSource s を変更することで開始`SelectMethod`から`GetProductsPaged`に`GetProductsPagedAndSorted`します。 これは、データ ソースの構成ウィザードの [プロパティ] ウィンドウから、または宣言の構文を直接実行できます。 次に、ObjectDataSource 秒の値を指定する必要があります[`SortParameterName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx)します。 ObjectDataSource が GridView s に渡すしようとした場合、このプロパティを設定すると、`SortExpression`プロパティを`SelectMethod`します。 具体的には、ObjectDataSource は、入力パラメーターの名前を持つがの値と等しい、`SortParameterName`プロパティ。 BLL s 以降`GetProductsPagedAndSorted`メソッドという並べ替え式の入力パラメーターには`sortExpression`、ObjectDataSource s 設定`SortExpression`プロパティ sortExpression をします。

これら 2 つの変更を行った後 ObjectDataSource s の宣言型構文に、次のようになります。


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> 前のチュートリアルでは、使用すると、確実、ObjectDataSource では、*いない*SelectParameters コレクション sortExpression、既定値、または maximumRows の入力パラメーターが含まれます。


GridView の並べ替え機能を有効にするチェック ボックスを並べ替えを有効にする GridView の s スマート タグには、GridView s の設定で`AllowSorting`プロパティを`true`linkbutton コントロールとして表示するには、各列のヘッダー テキストの原因とします。 エンドユーザーは、Linkbutton ヘッダーのいずれかをクリックすると、ポストバック後し、次の手順が発生します。

1. GridView の更新プログラムの[`SortExpression`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)の値に、`SortExpression`のリンクを持つヘッダーがクリックしてされたフィールド
2. ObjectDataSource 呼び出す BLL s `GetProductsPagedAndSorted` GridView s に渡して、メソッド`SortExpression`s メソッドの値としてプロパティ`sortExpression`入力パラメーター (と共に、適切な`startRowIndex`と`maximumRows`入力パラメーターの値)
3. BLL は DAL s を呼び出す`GetProductsPagedAndSorted`メソッド
4. DAL の実行、`GetProductsPagedAndSorted`ストアド プロシージャを渡すことで、`@sortExpression`パラメーター (と共に、`@startRowIndex`と`@maximumRows`入力パラメーターの値)
5. ストアド プロシージャが、BLL は、ObjectDataSource; に返しますに適切なデータのサブセットを返しますこのデータが GridView にバインドされている、HTML にレンダリングし、エンドユーザーに送信

図 7 で並べ替えたときの結果の最初のページを示しています、`UnitPrice`で昇順に並べ替えます。


[![結果は、UnitPrice 順に並べ替えられます。](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**図 7**: 結果は、UnitPrice で並べ替えられます ([フルサイズの画像を表示する をクリックします](sorting-custom-paged-data-cs/_static/image11.png))。


現在の実装では、製品名、カテゴリ名、単位、および単価ごとの数の結果を並べ替えることができます正しく、中に、ランタイム例外は業者によって結果名結果の順序をしようとしています (図 8 参照)。


![次のランタイム例外で供給業者の結果で結果を並べ替えるしようとしています。](sorting-custom-paged-data-cs/_static/image12.png)

**図 8**: 次のランタイム例外で供給業者の結果で結果を並べ替えるしようとしています。


この例外が発生、 `SortExpression` GridView s の`SupplierName`に設定されている BoundField`SupplierName`します。 ただし、s 供給業者の名前、`Suppliers`テーブルが実際に呼び出される`CompanyName`としては、この列名を別名にされている`SupplierName`。 ただし、`OVER`句で使用される、`ROW_NUMBER()`関数、エイリアスを使用することはできませんし、実際の列名を使用する必要があります。 そのため、変更、 `SupplierName` BoundField の`SortExpression`CompanyName を仕入から (図 9 参照)。 業者によって結果を並べ替えることができます、この変更後の図 10 に示す。


![変更 CompanyName を仕入 BoundField の SortExpression](sorting-custom-paged-data-cs/_static/image13.png)

**図 9**: 仕入 BoundField の SortExpression を CompanyName に変更します。


[![業者によって、結果を並べ替えるようになりました](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**図 10**:、結果は今すぐで並べ替えられます Supplier ([フルサイズの画像を表示する をクリックします](sorting-custom-paged-data-cs/_static/image16.png))。


## <a name="summary"></a>まとめ

前のチュートリアルで調べるカスタム ページングの実装によって、デザイン時に使用されるが、結果を並べ替える順序を指定することが必要です。 つまり、カスタム ページングの実装を実装しました提供できなかったこと、同時に、並べ替え機能これを意味します。 このチュートリアルでに含める最初からストアド プロシージャを拡張することによってこの制限を克服しました、`@sortExpression`入力パラメーターの結果を並べ替える可能性があります。

両方の並べ替えを提供する GridView を実装することとページングを渡す GridView s 現在 ObjectDataSource を構成することによってカスタムでしたこれを作成するにはストアド プロシージャと、DAL と BLL で新しいメソッドを作成するが、後`SortExpression`BLL プロパティ`SelectMethod`。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Carlos Santos でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](efficiently-paging-through-large-amounts-of-data-cs.md)
> [次へ](creating-a-customized-sorting-user-interface-cs.md)
