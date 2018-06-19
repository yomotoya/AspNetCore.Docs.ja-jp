---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: マスター/詳細の 2 つのページ (c#) にわたってフィルター |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは 2 つのページ間でマスター/詳細レポートを分離する方法について説明します。 'Master' のページで、Repeater コントロールを使用して categ の一覧を表示するためにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6a3783175218438f2a9f735c3861c56e039a248e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887097"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>マスター/詳細の 2 つのページ (c#) にわたってフィルター処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe)または[PDF のダウンロード](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> このチュートリアルでは 2 つのページ間でマスター/詳細レポートを分離する方法について説明します。 「マスター」のページで、Repeater コントロールを使用するときに表示するカテゴリの一覧をクリックされるになります「詳細」のページに 2 つの列 DataList が、選択したカテゴリに属する製品を示しています。


## <a name="introduction"></a>はじめに

[前のチュートリアル](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)DropDownLists を使用して、「マスター」のレコードと「詳細」を表示する DataList を表示する単一の web ページにマスター/詳細レポートを表示する方法を説明しました マスター/詳細レポートに使用されるもう 1 つの一般的なパターンでは、1 つの web ページ上のマスター レコードと別の詳細が必要です。 前に示した[マスター/詳細フィルター間で 2 つのページ](../masterdetail/master-detail-filtering-across-two-pages-cs.md)チュートリアルでは、すべて仕入先のシステムに表示する GridView を使用するこのパターンを調べることです。 この GridView に渡す 2 番目のページへのリンクとしてレンダリングできる内が含まれている、`SupplierID`クエリ文字列にします。 2 番目のページは、選択した業者によって提供されるこれらの製品を一覧表示するのに GridView を使用します。

このような 2 ページ マスター/詳細レポートは、DataList とリピータ コントロールもを使用して実現できます。 唯一の違いは、DataList もリピータが内のコントロールのサポートを提供します。 代わりに、ハイパーリンクの Web コントロールや HTML アンカー要素を追加します (`<a>`) 内でのコントロールの`ItemTemplate`します。 ハイパーリンクの`NavigateUrl`プロパティまたはアンカーの`href`宣言的またはプログラムによるアプローチを使用して属性をカスタマイズすることができますし、します。

このチュートリアルでは、箇条書き Repeater コントロールを使用して 1 つのページ上でカテゴリを一覧表示する例を調べておみます。 各リスト項目には、カテゴリ名が 2 番目のページへのリンクとして表示されます、カテゴリの名前と説明が含まれます。 このリンクをクリックするとは whisk 2 番目のページにユーザーが選択したカテゴリに属している製品を DataList が表示されます。

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>手順 1: 箇条書きリストに、カテゴリを表示します。

マスター/詳細レポートを作成する最初の手順では、「マスター」のレコードを表示することによって開始します。 そのため、まず最初に、「マスター」のページで、カテゴリが表示されます。 開く、 `CategoryListMaster.aspx`  ページで、`DataListRepeaterFiltering`フォルダーはリピータ コントロールを追加し、新しい ObjectDataSource を追加することを選択、スマート タグからです。 データにアクセスできるように、新しい ObjectDataSource を構成、`CategoriesBLL`クラスの`GetCategories`メソッド (図 1 を参照してください)。


[![構成、ObjectDataSource CategoriesBLL クラスのメソッドを使用するには](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**図 1**: 構成を使用する ObjectDataSource、`CategoriesBLL`クラスの`GetCategories`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


次に、箇条書きリスト内のアイテムとして各カテゴリの名前と説明が表示されるようにリピータのテンプレートを定義します。 説明されていない気が各カテゴリの詳細ページへのリンク。 次は、リピータと ObjectDataSource の宣言型マークアップを示します。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

完全なマークアップがこの、ブラウザーを使って作業の進行状況を表示するのにはしばらくを受け取る。 図 2 に示すリピータは各カテゴリの名前と説明を示す箇条書きリストとして表示されます。


[![各カテゴリは箇条書きリスト項目として表示されます。](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**図 2**: 各カテゴリは箇条書きリスト項目として表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>手順 2: に、カテゴリ名の詳細 ページへのリンク

1 つのカテゴリの「詳細」の情報を表示するユーザーを許可する必要がありますクリックされると、2 ページ目にユーザーを受け取るときに、各箇条書きリスト項目のリンクを追加する (`ProductsForCategoryDetails.aspx`)。 そうすると、この 2 番目のページでは、選択したを DataList を使用してカテゴリの製品が表示されます。 リンクがクリックしてされたカテゴリを決定するために渡す必要がクリックしたカテゴリの`CategoryID`メカニズムによって 2 ページ目にします。 ページ間でスカラー データを転送する最も簡単な最も簡単な方法は、このチュートリアルで使用するオプションは、クエリ文字列を通じてです。 具体的には、`ProductsForCategoryDetails.aspx`ページは、選択を求める*`categoryID`* という名前のクエリ文字列フィールドを介して渡される値を`CategoryID`です。 例については、飲料カテゴリの製品を表示するのには、`CategoryID`は 1 で、ユーザーがアクセスしてください`ProductsForCategoryDetails.aspx?CategoryID=1`です。

ハイパーリンク Web コントロール、または HTML アンカー要素が追加する必要がありますリピータで箇条書きリストの各項目のハイパーリンクを作成する (`<a>`) に、`ItemTemplate`です。 シナリオでのハイパーリンクが表示行ごとに同じ、いずれかの方法で十分です。 リピータの (_n)、アンカー要素を使用します。 アンカー要素を使用するにリピータの ItemTemplate を更新します。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

なお、`CategoryID`アンカー要素の内に直接挿入できます`href`属性ですただし、実行されるので、を区切るために、`href`とアポストロフィ (注引用符) 以降の属性の値、`Eval`メソッド。内で、`href`属性は、その文字列を区切る (`"CategoryID"`) 引用符で囲まれます。 または、代わりに、ハイパーリンクの Web コントロールを使用できます。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

注方法、URL の静的部分 — `ProductsForCategoryDetails.aspx?CategoryID` -がの結果に追加されます`Eval("CategoryID")`文字列の連結を使用して、データ バインド構文内で直接です。

ハイパーリンク コントロールを使用する利点の 1 つをプログラムでアクセスできるリピータから`ItemDataBound`必要な場合は、イベント ハンドラー。 たとえばは、関連付けられている製品のカテゴリへのリンクではなくテキストとして、カテゴリ名を表示することができます。 このようなチェック プログラムでを実行できますが、`ItemDataBound`イベント ハンドラーですなしでカテゴリを製品、ハイパーリンクの関連付けられている`NavigateUrl`プロパティを招き、その特定のカテゴリ名で結果として空の文字列に設定する可能性があります。プレーン テキストとして (ではなくリンクとして) を表示します。 戻って、 [DataList とリピータ ベース時のデータの書式設定](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md)を通じてプログラム ロジックに基づいて、DataList とリピータの内容を書式設定の詳細についてのチュートリアル、`ItemDataBound`イベント ハンドラー。

実行する場合、自由に、ページのアンカー要素またはハイパーリンク コントロールのアプローチのいずれかを使用してください。 各カテゴリ名へのリンクとして表示するか、ブラウザーからページを表示するときに、方法に関係なく`ProductsForCategoryDetails.aspx`、適用可能なを渡して、`CategoryID`値 (図 3 を参照してください)。


[![カテゴリの名前が ProductsForCategoryDetails.aspx にリンクできるよう](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**図 3**:、カテゴリ名今すぐへのリンク`ProductsForCategoryDetails.aspx`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>手順 3: 選択したカテゴリに属している製品を一覧表示します。

`CategoryListMaster.aspx` 詳細 ページを実装することに注目する準備ができました ページで完了、`ProductsForCategoryDetails.aspx`です。 このページを開いて、DataList をツールボックスからデザイナーにドラッグ、および設定の`ID`プロパティを`ProductsInCategory`です。 次に、DataList のスマート タグからの選択 ページで、名前付けに新しい ObjectDataSource を追加`ProductsInCategoryDataSource`です。 呼び出しをするように構成、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド以外の設定を (なし)、INSERT、UPDATE、および DELETE のタブで、ドロップダウン リストを一覧表示します。


[![ObjectDataSource ProductsBLL クラスの GetProductsByCategoryID(categoryID) メソッドを使用して構成します。](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**図 4**: 構成を使用する ObjectDataSource、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


以降、`GetProductsByCategoryID(categoryID)`メソッドは、入力パラメーターを受け取ります (*`categoryID`*)、データ ソースの選択ウィザードは us にパラメーターのソースを指定する機会を提供します。 パラメーターのソース、QueryStringField を使用してクエリ文字列に設定`CategoryID`です。


[![パラメーターのソースとしての Querystring フィールド CategoryID を使用します。](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**図 5**: クエリ文字列フィールドを使用して`CategoryID`パラメーターのソースとして ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


データ ソース ウィザードの完了後に前のチュートリアルで見てきたよう、Visual Studio は自動的に作成、 `ItemTemplate` DataList を各データ フィールドの名前と値の一覧を表示するためです。 製品の名前、供給業者、および price のみを一覧表示するには、このテンプレートを置き換えます。 また、設定、DataList`RepeatColumns`プロパティを 2 です。 これらの変更後に、DataList と ObjectDataSource の宣言型マークアップを次のようになります。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

開始アクションでこのページを表示する、`CategoryListMaster.aspx`ページです。 次に、カテゴリの箇条書きリストにあるリンクをクリックします。 これにより操作により`ProductsForCategoryDetails.aspx`に沿って渡す、`CategoryID`クエリ文字列を通じてします。 `ProductsInCategoryDataSource`で ObjectDataSource`ProductsForCategoryDetails.aspx`は指定したカテゴリの製品だけを取得し、それらの行ごとに 2 つの製品を表示すると、DataList に表示します。 図 6 は、のスクリーン ショット`ProductsForCategoryDetails.aspx`飲み物を表示するときにします。


[![飲み物が表示され、行ごとに 2 つ](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**図 6**:、飲み物が表示され、行ごとに 2 つ ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>手順 4: ProductsForCategoryDetails.aspx にカテゴリ情報を表示します。

内にあるカテゴリで、ユーザーがクリックしたとき`CategoryListMaster.aspx`に取得されます。`ProductsForCategoryDetails.aspx`され、選択したカテゴリに属している製品を表示します。 ただし、`ProductsForCategoryDetails.aspx`に関してどのようなカテゴリが選択された視覚的な手掛かりはありません。 飲み物が誤ってクリックされた調味料をクリックすることを意図した、ユーザーに到達すると、誤った操作を実現する手段を持たない`ProductsForCategoryDetails.aspx`です。 このような問題を軽減するのには、選択したカテゴリに関する情報を表示できます: その名前と説明 — の上部にある、`ProductsForCategoryDetails.aspx`ページ。

これを実現する追加リピータ コントロールの上 FormView`ProductsForCategoryDetails.aspx`です。 次に、新しい ObjectDataSource をという名前のフォーム ビューのスマート タグからページを追加`CategoryDataSource`を使用するように構成し、`CategoriesBLL`クラスの`GetCategoryByCategoryID(categoryID)`メソッドです。


[![を介して、CategoriesBLL GetCategoryByCategoryID(categoryID) クラスのカテゴリに関する情報にアクセス](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**図 7**: を通じてカテゴリに関する情報を取得、`CategoriesBLL`クラスの`GetCategoryByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


同様、 `ProductsInCategoryDataSource` ObjectDataSource が手順 3. で追加、`CategoryDataSource`のデータ ソースの構成ウィザードの指示に従って us のソースで、`GetCategoryByCategoryID(categoryID)`メソッドのパラメーターを入力します。 正確な設定と同じにする前に、パラメーターのソースをクエリ文字列、QueryStringField 値に設定を使用して`CategoryID`(戻るは図 5 を参照してください)。

ウィザードを完了すると、Visual Studio が自動的に作成されます、 `ItemTemplate`、 `EditItemTemplate`、および`InsertItemTemplate`FormView 用です。 読み取り専用のインターフェイスを提供していますので自由に削除、`EditItemTemplate`と`InsertItemTemplate`です。 また、自由にカスタマイズ FormView の`ItemTemplate`します。 余分なテンプレートを削除すると、ItemTemplate のカスタマイズ、フォーム ビューと ObjectDataSource の宣言型マークアップを次のようになります。

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

図 8 は、スクリーン ショット、ブラウザーからこのページを表示している場合を示します。

> [!NOTE]
> FormView のカテゴリの一覧にユーザーを実行する上記のハイパーリンク コントロールを追加しました、FormView に加えて (`CategoryListMaster.aspx`)。 このリンクを別の場所に配置するかを完全に省略する自由です。


[![カテゴリ情報は、ページの上部に表示されるようになりました](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**図 8**: カテゴリ情報は、ページの上部に表示されるようになりました ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>手順 5: 選択したカテゴリに属している製品が存在しない場合は、メッセージを表示します。

`CategoryListMaster.aspx`ページに関係なく、システム内のすべてのカテゴリを一覧表示の製品が関連付けられているかどうかが発生します。 関連付けられている製品、DataList でカテゴリをクリックすると`ProductsForCategoryDetails.aspx`が表示されない、そのデータ ソースはすべての項目が存在しないためです。 GridView は、過去のチュートリアルで見てきたよう、`EmptyDataText`そのデータ ソースのレコードがない場合に表示するテキスト メッセージを指定するために使用できるプロパティです。 残念ながら、DataList もリピータには、このようなプロパティがあります。

選択したカテゴリに一致する製品がありませんを追加することをユーザーに通知メッセージを表示するために、ラベル コントロールをページの`Text`プロパティに一致する製品がないことを表示するメッセージが割り当てられます。 プログラムで設定する必要があります、`Visible`プロパティは、DataList が任意の項目を格納するかどうかに基づいています。

これを実現するには、DataList の下にラベルを追加することで開始します。 設定の`ID`プロパティを`NoProductsMessage`とその`Text`プロパティを「... 選択したカテゴリの製品がありません」 次に、このラベルをプログラムで設定する必要があります`Visible`プロパティにすべてのデータがバインドされているかどうかに基づいて、 `ProductsInCategory` DataList です。 DataList にデータがバインドされた後は、この割り当てを行う必要があります。 GridView、DetailsView、およびフォーム ビューのコントロールのイベント ハンドラーを作成おでした`DataBound`イベントで、データ バインドが完了した後に発生します。 ただし、DataList もリピータには、`DataBound`使用可能なイベントです。

この例のラベルの割り当てるできます`Visible`プロパティに、`Page_Load`データが、ページの前に DataList に割り当てられているされますので、イベント ハンドラー`Load`イベント。 ただし、このアプローチない一般的なケースでどおりに動作 DataList ページのライフ サイクルの後に、ObjectDataSource からデータをバインドする可能性があります。 たとえば場合は、「マスター」のレコードを保持する DropDownList を使用してマスター/詳細レポートを表示するときに表示されるデータの別のコントロールの値がに基づいて、データ可能性がありますいない再バインドされるまで、データの Web コントロールに、`PreRender`段階で、ページの有効期間。

割り当てるには、すべてのケースで動作する 1 つのソリューション、`Visible`プロパティを`False`DataList ので`ItemDataBound`(または`ItemCreated`) の項目の種類をバインドするときにイベント ハンドラー`Item`または`AlternatingItem`です。 このような場合はわかっていますがあることには、少なくとも 1 つのデータ、データ ソース内の項目し、隠すことができるため、`NoProductsMessage`ラベル。 このイベント ハンドラーに加えも必要があります、イベント ハンドラー、DataList の`DataBinding`ラベルを初期化して、イベント`Visible`プロパティを`True`です。 以降、`DataBinding`イベントが発生する前に、`ItemDataBound`イベント、ラベルの`Visible`プロパティの初期設定`True`以外の場合は、データ項目がある場合設定するただし、 `False`。 次のコードでは、このロジックを実装します。

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

すべてのカテゴリ、Northwind データベースは 1 つまたは複数の製品に関連付けられます。 この機能をテストするには手動で調整して、Northwind データベースこのチュートリアルでは、生成カテゴリに関連付けられているすべての製品を再割り当て (`CategoryID` = 7) Seafood カテゴリに (`CategoryID` = 8)。 これには、サーバー エクスプ ローラーから新しいクエリを選択して、次を使用して`UPDATE`ステートメント。

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

データベースを適宜更新した後に戻り、`CategoryListMaster.aspx`ページおよび生成のリンクをクリックします。 生成カテゴリに属しているすべての製品が不要になった、する必要がありますを参照して、「... 選択したカテゴリの製品がありません」 メッセージでは、図 9 に示すようにします。


[![選択したカテゴリに No 製品属しているものがある場合、メッセージが表示されます。](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**図 9**: 選択したカテゴリに No 製品属しているものがある場合、メッセージが表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>まとめ

マスター/詳細レポートを 1 ページにマスター/詳細の両方のレコードが表示されることができます、多くの web サイトで区切られます複数の 2 つの web ページ。 このチュートリアルでは、「マスター」の web ページで、Repeater を使用して、箇条書きリストに示されているカテゴリと [詳細] ページに示される関連付けられている製品で、このようなマスター/詳細レポートを実装する方法について説明しました。 マスターの web ページ内の各リスト項目に渡されます行の詳細ページへのリンクが含まれている`CategoryID`値。

[詳細] ページで、指定された業者のこれらの製品を取得することによって実現されました、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。 *`categoryID`* を使用して宣言パラメーターの値が指定された、`CategoryID`パラメーターのソースとしてのクエリ文字列値です。 詳細 ページを使用して、FormView でカテゴリの詳細を表示する方法と、選択したカテゴリに属する製品が存在しなかった場合にメッセージを表示する方法についても説明しました。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Zack Jones および Liz Shulok がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [次へ](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
