---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: マスター/詳細の 2 つのページ (c#) 間のフィルター処理 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは 2 つのページ間でマスター/詳細レポートを分離する方法について説明します。 'Master' のページで、Repeater コントロールを使用して categ の一覧を表示する.
ms.author: aspnetcontent
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 1dc0c074ecc8ca6128376c8cdf5548028d9779e3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830734"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>マスター/詳細の 2 つのページ (c#) 間のフィルター処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe)または[PDF のダウンロード](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> このチュートリアルでは 2 つのページ間でマスター/詳細レポートを分離する方法について説明します。 「マスター」のページでは、クリックされるは、ユーザーに移動「詳細」ページ 2 列 DataList で、選択したカテゴリに属しているこれらの製品がどのように表示されているときにカテゴリの一覧を表示するために、Repeater コントロールを使用します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)Dropdownlist を使用して、「マスター」のレコードと「詳細」を表示する DataList を表示する 1 つの web ページでマスター/詳細レポートを表示する方法を説明しました マスター/詳細レポートに使用されるもう 1 つの一般的なパターンでは、1 つの web ページ上のマスター レコードと別の詳細な情報があります。 前に示した[マスター/詳細フィルター間で 2 つのページ](../masterdetail/master-detail-filtering-across-two-pages-cs.md)チュートリアルでは、仕入先のすべてのシステム内に表示する GridView を使用してこのパターンを調査しました。 この GridView には、内に渡す 2 番目のページへのリンクとしてレンダリングが含まれている、`SupplierID`クエリ文字列。 2 番目のページを選択した業者によって提供されるこれらの製品を一覧表示する GridView を使用します。

このような 2 つのページに移動してマスター/詳細レポートは、DataList と Repeater コントロールもを使用して実現できます。 唯一の違いは、DataList でも、Repeater ことは内のコントロールのサポートを提供します。 代わりに、ハイパーリンク Web コントロールまたは HTML アンカー要素を追加します (`<a>`) 内でのコントロールの`ItemTemplate`します。 ハイパーリンクの`NavigateUrl`プロパティまたはアンカーの`href`宣言的またはプログラムによるアプローチを使用して属性をカスタマイズすることができます、します。

このチュートリアルでは、Repeater コントロールを使用して 1 つのページに箇条書きリストにカテゴリを一覧表示する例について説明します。 各リスト項目には、カテゴリの名前と説明、カテゴリ名が 2 番目のページへのリンクとして表示されますが含まれます。 このリンクをクリックすると、whisk 2 番目のページにユーザー DataList が選択したカテゴリに属しているこれらの製品に表示されます。

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>手順 1: 箇条書きリストにカテゴリを表示します。

マスター/詳細レポートを作成する最初の手順では、「マスター」のレコードを表示することによって開始します。 そのため、最初のタスクは、「マスター」のページにカテゴリを表示します。 開く、`CategoryListMaster.aspx`ページで、`DataListRepeaterFiltering`フォルダーは、Repeater コントロールを追加し、新しい ObjectDataSource を追加することを選択、スマート タグから。 データにアクセスするように新しい ObjectDataSource を構成、`CategoriesBLL`クラスの`GetCategories`メソッド (図 1 参照)。


[![CategoriesBLL クラスのメソッドを使用する ObjectDataSource を構成します。](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**図 1**: 構成に使用する ObjectDataSource、`CategoriesBLL`クラスの`GetCategories`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))。


次に、箇条書きリスト内の項目として各カテゴリの名前と説明が表示されるよう、Repeater のテンプレートを定義します。 それでは、各カテゴリについて心配されていない詳細ページへのリンク。 次は、Repeater、ObjectDataSource の宣言型マークアップを示します。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

完全なこのマークアップで少し、ブラウザーで進行状況を表示します。 図 2 が示すように、Repeater は各カテゴリの名前と説明を示す箇条書きリストとしてレンダリングします。


[![各カテゴリは箇条書きリストの項目として表示されます。](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**図 2**: 各カテゴリは箇条書きリストの項目として表示されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))。


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>手順 2: 詳細ページへのリンクにカテゴリ名を有効に

特定のカテゴリの詳細」の情報を表示するユーザーを許可するには、クリックされると、2 番目のページにユーザーがかかります。 ときに、それぞれに箇条書きリスト項目のリンクを追加する (`ProductsForCategoryDetails.aspx`)。 そうすると、この 2 番目のページでは、DataList を使用して、選択したカテゴリの製品が表示されます。 リンクがクリックしてされたカテゴリを決定するために渡す必要がクリックされたカテゴリの`CategoryID`のメカニズムによって 2 ページ目にします。 ページ間でスカラーのデータを転送する最も簡単な最も簡単な方法は、クエリ文字列を通じてこのチュートリアルでを使用するオプションです。 具体的には、 `ProductsForCategoryDetails.aspx` 、選択したページに期待される*`categoryID`* という名前のクエリ文字列フィールドを介して渡される値を`CategoryID`します。 たとえば、飲み物のカテゴリの製品を表示するため、`CategoryID`は 1 で、ユーザーがアクセスしてください`ProductsForCategoryDetails.aspx?CategoryID=1`します。

ハイパーリンク Web コントロール、または HTML アンカー要素が追加する必要があります Repeater に箇条書きリストの各項目のハイパーリンクを作成する (`<a>`) に、`ItemTemplate`します。 がハイパーリンクであるシナリオが表示されたら、同じ行ごとに、どちらの方法で十分です。 たいリピータの場合、アンカー要素を使用します。 アンカー要素を使用するには、Repeater の ItemTemplate を更新します。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

なお、`CategoryID`アンカー要素の内で直接挿入することができます`href`属性ですただし、実行されるので、を区切るために、`href`以降、アポストロフィ (および注引用符) で属性の値、`Eval`メソッド。内で、`href`属性は、その文字列を区切ります (`"CategoryID"`) 引用符でします。 または、代わりに、ハイパーリンク Web コントロールを使用できます。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

注方法 URL の静的部分: `ProductsForCategoryDetails.aspx?CategoryID` -がの結果に追加されます`Eval("CategoryID")`文字列の連結を使用してデータ バインディング構文内で直接します。

ハイパーリンク コントロールを使用する利点の 1 つは、そのプログラムにアクセスできることから Repeater の`ItemDataBound`必要な場合は、イベント ハンドラー。 たとえばは、関連付けられている製品カテゴリへのリンクではなくテキストとして、カテゴリ名を表示する可能性があります。 このようなチェックにプログラムで実行、`ItemDataBound`イベント ハンドラー; なしでカテゴリを関連する製品、ハイパーリンクの`NavigateUrl`プロパティは、その特定のカテゴリ名で、空の文字列に設定する可能性がありますプレーン テキストとして (としてではなくリンク) をレンダリングします。 参照、 [DataList と Repeater のデータと書式設定](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md)を通じてプログラム ロジックに基づいて DataList と Repeater の内容を書式設定の詳細については、チュートリアル、`ItemDataBound`イベント ハンドラー。

に従っている場合は、ページのアンカー要素、またはハイパーリンク コントロールのアプローチを使用する自由します。 各カテゴリ名へのリンクとして表示するブラウザーを使用してページを表示するときに、アプローチに関係なく`ProductsForCategoryDetails.aspx`の適切なを渡して、`CategoryID`値 (図 3 を参照してください)。


[![カテゴリ名が ProductsForCategoryDetails.aspx にリンクするようになりました](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**図 3**:、カテゴリ名今すぐへのリンク`ProductsForCategoryDetails.aspx`([フルサイズの画像を表示する をクリックします](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))。


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>手順 3: 選択したカテゴリに属する製品を一覧表示します。

`CategoryListMaster.aspx` 、「詳細」ページの実装に注目する準備ができました ページで完全な`ProductsForCategoryDetails.aspx`します。 このページを開き、DataList をツールボックスからデザイナーにドラッグして設定その`ID`プロパティを`ProductsInCategory`します。 次に、DataList のスマート タグから新しい ObjectDataSource をその名前を付け、ページに追加する選択`ProductsInCategoryDataSource`します。 呼び出すように構成、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド; セット (None) に、INSERT、UPDATE、および DELETE のタブで、ドロップダウン リストを一覧表示します。


[![ObjectDataSource ProductsBLL クラスの GetProductsByCategoryID(categoryID) メソッドを使用して構成します。](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**図 4**: 構成に使用する ObjectDataSource、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))。


以降、`GetProductsByCategoryID(categoryID)`メソッドは入力パラメーターを受け取ります (*`categoryID`*)、データ ソースの選択ウィザードは私たちにパラメーターのソースを指定する機会を提供します。 パラメーターのソースを QueryStringField を使用してクエリ文字列に設定`CategoryID`します。


[![パラメーターのソースとしてクエリ文字列フィールドの CategoryID を使用します。](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**図 5**: クエリ文字列フィールドを使用して`CategoryID`パラメーターのソースとして ([フルサイズの画像を表示する をクリックします](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))。


Visual Studio が自動的に作成、データ ソースの選択ウィザードが完了したら、前のチュートリアルでご覧いただいたよう、 `ItemTemplate` DataList を各データ フィールドの名前と値の一覧を表示するのです。 このテンプレートは、製品の名前、供給業者、および料金のみを一覧表示するものに置き換えます。 DataList を設定しても、`RepeatColumns`プロパティを 2。 これらの変更後、DataList コントロールと ObjectDataSource の宣言型マークアップは次のようになります。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

開始アクションでこのページを表示する、`CategoryListMaster.aspx`ページは、次に、カテゴリの箇条書きリストにあるリンクをクリックします。 これを実行する`ProductsForCategoryDetails.aspx`に沿って渡す、`CategoryID`を通じて、クエリ文字列。 `ProductsInCategoryDataSource`で ObjectDataSource`ProductsForCategoryDetails.aspx`指定したカテゴリの製品のみを取得し、DataList では、行ごとに 2 つの製品の表示に表示します。 図 6 のスクリーン ショットに示す`ProductsForCategoryDetails.aspx`飲み物を表示するときにします。


[![飲み物を表示すると、行ごとに 2 つ](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**図 6**:、飲み物を表示すると、行ごとに 2 つ ([フルサイズの画像を表示する をクリックします](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))。


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>手順 4: ProductsForCategoryDetails.aspx カテゴリ情報を表示します。

ユーザーがのカテゴリをクリックすると`CategoryListMaster.aspx`が表示されます`ProductsForCategoryDetails.aspx`され、選択したカテゴリに属する製品を表示します。 ただし、`ProductsForCategoryDetails.aspx`に関してどのようなカテゴリを選択した視覚的な手掛かりはありません。 飲み物が誤ってクリック調味料、 をクリックするためのものをユーザーに到達すると、誤った操作を実現方法を持たない`ProductsForCategoryDetails.aspx`します。 このような問題を軽減することができます、については、選択したカテゴリを表示します: 名前と説明: の上部にある、`ProductsForCategoryDetails.aspx`ページ。

これを実現する追加 Repeater コントロールの上の FormView`ProductsForCategoryDetails.aspx`します。 次に、新しい ObjectDataSource をという名前の FormView のスマート タグからのページに追加`CategoryDataSource`を使用するように構成し、`CategoriesBLL`クラスの`GetCategoryByCategoryID(categoryID)`メソッド。


[![CategoriesBLL クラスの GetCategoryByCategoryID(categoryID) メソッドを通じてカテゴリに関する情報にアクセス](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**図 7**: を通じてカテゴリに関する情報を取得、`CategoriesBLL`クラスの`GetCategoryByCategoryID(categoryID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))。


同様、 `ProductsInCategoryDataSource` ObjectDataSource は、手順 3. で追加、`CategoryDataSource`のデータ ソースの構成ウィザードでのソースが米国、`GetCategoryByCategoryID(categoryID)`メソッドのパラメーターを入力します。 正確な設定と同じにする前に、パラメーター ソースをクエリ文字列、QueryStringField 値に設定を使用して、 `CategoryID` (戻るは図 5 を参照してください)。

ウィザードを完了すると、Visual Studio が自動的に作成されます、 `ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`FormView の。 読み取り専用のインターフェイスを提供していますので自由に削除、`EditItemTemplate`と`InsertItemTemplate`します。 また、自由にカスタマイズ FormView の`ItemTemplate`します。 余分なテンプレートを削除して、ItemTemplate をカスタマイズすると、FormView と ObjectDataSource の宣言型マークアップは次のようになります。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

図 8 は、ブラウザーからこのページを表示するときに画面を示しています。

> [!NOTE]
> カテゴリの一覧に戻り、ユーザーが取る FormView 上のハイパーリンク コントロールを追加しましただけでなく、FormView (`CategoryListMaster.aspx`)。 このリンクを別の場所に配置する、または完全に省略する自由です。


[![カテゴリ情報は、ページの上部に表示されるようになりました](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**図 8**: カテゴリ情報は、ページの上部に表示されるようになりました ([フルサイズの画像を表示する をクリックします](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))。


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>手順 5: 選択したカテゴリに属して製品が存在しない場合は、メッセージを表示します。

`CategoryListMaster.aspx`ページ、システム内のすべてのカテゴリの一覧に関係なく製品が関連付けられているかどうかが発生します。 関連付けられている製品、DataList でカテゴリをクリックすると`ProductsForCategoryDetails.aspx`はありませんが描画される、そのデータ ソースには、すべての項目はありません。 過去のチュートリアルでご覧いただいた GridView には、`EmptyDataText`プロパティのデータ ソースにレコードがない場合に表示するテキスト メッセージを指定するために使用できます。 残念ながら、DataList と Repeater のどちらもこのようなプロパティがあります。

選択したカテゴリの一致する製品はありません、追加する必要がありますのことをユーザーに通知メッセージを表示するために、ラベル コントロールをページの`Text`プロパティに一致する製品がないことを表示するメッセージが割り当てられます。 プログラムで設定する必要があります、`Visible`プロパティは、DataList で任意の項目を格納するかどうかに基づいています。

これを実現するには、DataList の下にラベルを追加することで開始します。 設定の`ID`プロパティを`NoProductsMessage`とその`Text`プロパティを「... 選択したカテゴリの製品がありません」次に、このラベルをプログラムで設定する必要があります`Visible`プロパティにすべてのデータがバインドされているかどうかに基づいて、 `ProductsInCategory` DataList します。 データが、DataList にバインドされた後は、この割り当てを行う必要があります。 GridView、DetailsView、および FormView コントロールのイベント ハンドラーを作成でした`DataBound`イベントで、データ バインドが完了した後に発生します。 ただし、DataList でも、Repeater には、`DataBound`イベントを使用できます。

このサンプルは、ラベルを割り当てることができます`Visible`プロパティ、 `Page_Load` 、DataList、ページの前に、データが割り当てられているがあるため、イベント ハンドラー`Load`イベント。 ただし、この方法もうまくいきませんの一般的なケースとして、ObjectDataSource からデータをページのライフ サイクルの後半で DataList にバインドする場合があります。 などの場合は、「マスター」のレコードを保持するために、DropDownList を使用してマスター/詳細レポートを表示するときに表示されるデータの別のコントロールの値がに基づいて、データ可能性がありますいないに再バインドまでデータ Web コントロール、`PreRender`ステージで、ページのライフ サイクルです。

割り当てるには、すべてのケースの機能を 1 つのソリューションは、`Visible`プロパティを`False`で DataList の`ItemDataBound`(または`ItemCreated`) の項目の種類をバインドするときにイベント ハンドラー`Item`または`AlternatingItem`します。 このような場合がわかっているので、少なくとも 1 つのデータは、データ ソース内の項目し、ため非表示にできます、`NoProductsMessage`ラベル。 このイベント ハンドラーだけでなく必要もありますイベント ハンドラーの DataList の`DataBinding`ラベルを初期化して、イベント`Visible`プロパティを`True`します。 以降、`DataBinding`イベントが発生する前に、`ItemDataBound`イベント、ラベルの`Visible`プロパティの初期設定`True`任意のデータ項目がある場合設定するただし、 `False`。 次のコードでは、このロジックを実装します。

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

すべてのカテゴリで、Northwind データベースは 1 つまたは複数の製品に関連付けられます。 この機能をテストするには手動でを調整して、Northwind データベースのこのチュートリアルでは、生成カテゴリに関連付けられているすべての製品を再割り当て (`CategoryID` = 7) シーフード カテゴリに (`CategoryID` = 8)。 これは、新しいクエリを選択して、次を使用して、サーバー エクスプ ローラーから実現できます`UPDATE`ステートメント。

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

データベースを適宜更新した後に戻り、`CategoryListMaster.aspx`ページし、生成のリンクをクリックします。 生成カテゴリに属するすべての製品は不要であるために、図 9 に示すように、「... 選択したカテゴリの製品がありません」メッセージが表示されます。


[![なしでは、製品の選択したカテゴリに属するがある場合、メッセージが表示されます。](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**図 9**: なしでは、製品の選択したカテゴリに属する場合、メッセージが表示されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))。


## <a name="summary"></a>まとめ

マスター/詳細レポートは、1 ページにマスター/詳細の両方のレコードを表示することができます、多くの web サイトで区切られます 2 つの web ページ。 このチュートリアルでは、「マスター」の web ページで、Repeater を使用して箇条書きリストに示されているカテゴリと「詳細」ページに示される関連付けられている製品でこのようなマスター/詳細レポートを実装する方法を説明しました。 マスター web ページ内の各リスト項目に渡されます行の詳細ページへのリンクが含まれている`CategoryID`値。

詳細ページで、指定された業者の製品を取得することによって実現されました、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド。 *`categoryID`* を使用して宣言パラメーターの値が指定されました、`CategoryID`パラメーターのソースとしてクエリ文字列値。 FormView を使用して、詳細ページにカテゴリの詳細を表示する方法と、選択したカテゴリに属する製品が存在しなかった場合は、メッセージを表示する方法についても説明しました。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Zack Jones、Liz Shulok でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [次へ](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
