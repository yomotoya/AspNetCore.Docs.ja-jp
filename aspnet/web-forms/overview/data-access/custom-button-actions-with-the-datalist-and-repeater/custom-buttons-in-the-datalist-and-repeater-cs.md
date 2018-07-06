---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: カスタム ボタン DataList と Repeater (c#) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、Repeater を使用して、システムでは、各カテゴリがその associ を表示するためのボタンを提供することで、カテゴリを一覧表示するインターフェイスを作成します.
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 04dc12ed20fcda0b4074add065022c42343e5ffc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822122"
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>DataList と Repeater (c#) のカスタム ボタン
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe)または[PDF のダウンロード](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> このチュートリアルでは、Repeater を使用して、システムでは、BulletedList コントロールを使用して、関連付けられている製品を表示するためのボタンを提供する各カテゴリにカテゴリを一覧表示するインターフェイスをビルドします。


## <a name="introduction"></a>はじめに

過去の 17 DataList と Repeater のチュートリアル全体で読み取り専用の例と編集および削除の例を作成しました。 編集と削除、DataList 内で機能させるために、DataList s にボタンを追加しました`ItemTemplate`をクリックすると、ポストバックの原因し、s ボタンに対応する DataList イベントを発生させた`CommandName`プロパティ。 ボタンの追加など、`ItemTemplate`で、`CommandName`プロパティの値を編集の場合、DataList s`EditCommand`ポストバックを発生させると 1 つ、`CommandName`削除が発生、 `DeleteCommand`。

さらに編集して、ボタンの削除、DataList と Repeater コントロールも、ボタン、Linkbutton、または ImageButtons をクリックすると、いくつかのカスタム サーバー側ロジックを実行します。 このチュートリアルでは、Repeater を使用して、システムで、カテゴリを一覧表示するインターフェイスをビルドします。 リピータ カテゴリごとに、BulletedList コントロールを使用して $s に関連付けられている製品カテゴリを表示するためのボタンが含まれます (図 1 参照)。


[![クリックすると表示の製品リンク表示箇条書きリストにカテゴリの製品](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**図 1**: 箇条書きリストにカテゴリの製品の製品リンクを表示する表示します をクリックして ([フルサイズの画像を表示する をクリックします](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))。


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>手順 1: カスタム ボタン チュートリアル Web ページの追加

カスタム ボタンを追加する方法を説明する前に、このチュートリアルの必要がありますが、web サイト プロジェクトで ASP.NET ページを作成する最初に少し s を使用できます。 という名前の新しいフォルダーを追加することで開始`CustomButtonsDataListRepeater`します。 次に、次の 2 つの ASP.NET ページを使用する各ページに関連付けるように、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `CustomButtons.aspx`


![カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加します。](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**図 2**: カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加


などの他のフォルダーで`Default.aspx`で、`CustomButtonsDataListRepeater`フォルダーは、チュートリアルのセクションで一覧表示します。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 追加するには、このユーザー コントロール`Default.aspx`をページのデザイン ビューに ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**図 3**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))。


最後に、ページに追加するエントリとして、`Web.sitemap`ファイル。 具体的には、DataList と Repeater によるページングと並べ替えの後に、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューには、編集、挿入、および削除のチュートリアルの項目が含まれています。


![サイト マップが含まれています、エントリにはカスタム ボタンのチュートリアル](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**図 4**: サイト マップが含まれています、エントリにはカスタム ボタンのチュートリアル


## <a name="step-2-adding-the-list-of-categories"></a>手順 2: カテゴリの一覧を追加します。

このチュートリアルを表示する製品 LinkButton と共にすべてのカテゴリを一覧表示する Repeater を作成する必要をクリックすると、箇条書きリストに関連付けられているカテゴリの製品が表示されます。 最初、システムで、カテゴリを一覧表示する単純な Repeater を作成して使用できます。 開いて開始、`CustomButtons.aspx`ページで、`CustomButtonsDataListRepeater`フォルダー。 ツールボックスからデザイナーとセットに、Repeater をドラッグしてその`ID`プロパティを`Categories`します。 次に、Repeater s のスマート タグから新しいデータ ソース コントロールを作成します。 具体的には、という名前の新しい ObjectDataSource コントロールを作成`CategoriesDataSource`からそのデータを選択を`CategoriesBLL`クラスの`GetCategories()`メソッド。


[![ObjectDataSource CategoriesBLL クラスの GetCategories() メソッドを使用して構成します。](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**図 5**: 構成に使用する ObjectDataSource、`CategoriesBLL`クラス s`GetCategories()`メソッド ([フルサイズの画像を表示する をクリックします](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))。


Visual Studio で、既定値を作成します、DataList コントロールとは異なり`ItemTemplate`データ ソースに基づき、Repeater のテンプレートする必要があります手動で定義します。 さらに、Repeater のテンプレートを作成して、宣言的に編集する必要があります (つまり、ある s テンプレートの編集なしオプション Repeater s のスマート タグで)。

左下隅の [ソース] タブをクリックし、追加、`ItemTemplate`内のカテゴリの名前を表示する、`<h3>`要素とその説明の段落にタグを含めることが、`SeparatorTemplate`水平方向の規則を表示する (`<hr />`) 各間カテゴリ。 LinkButton を追加してもその`Text`プロパティを表示する製品に設定します。 次の手順を完了すると、次のようページ s の宣言型マークアップになります。


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

図 6 は、ブラウザーで表示する際、ページを示します。 各カテゴリの名前と説明が表示されます。 製品の表示 ボタンをクリックすると、ポストバックが発生するが、任意の操作はまだ実行されません。


[![各カテゴリ名と説明が表示され、製品 linkbutton コントロールを表示します。](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**図 6**: 各カテゴリの名前と説明が表示され、製品を表示する LinkButton ([フルサイズの画像を表示する をクリックします](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))。


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>手順 3: 実行サーバー側ロジックと、表示する製品 LinkButton をクリックします。

ボタン、linkbutton コントロールまたは DataList または Repeater で、テンプレート内で ImageButton がクリックされたときにいつでもポストバックが発生し、DataList または Repeater の`ItemCommand`イベントが発生します。 加え、`ItemCommand`イベント、ユーザーことがある場合は、コントロールには別に固有のイベントは生成も DataList ボタン s `CommandName` (削除、編集をキャンセル、Update、または選択します)、予約済み文字列のいずれかに設定されていますが、`ItemCommand`イベントが*常に*発生します。

DataList または Repeater 内でボタンがクリックされたときに多くの場合渡す必要があります (であることを複数の両方、編集など、コントロール内のボタンと [削除] ボタンの場合) クリックしてされたボタンと、追加情報 (などプライマリ キーの値がボタンがクリックされた項目)。 渡される値を持つ 2 つのプロパティを提供し、ボタン、LinkButton、ImageButton、`ItemCommand`イベント ハンドラー。

- `CommandName` 通常、テンプレート内の各ボタンを識別するために使用される文字列
- `CommandArgument` 主キーの値など、いくつかのデータ フィールドの値を保持するためによく使用されます。

この例では、設定、LinkButton s`CommandName`プロパティ ShowProducts および bind、現在のレコードの主キー値を`CategoryID`を`CommandArgument`プロパティのデータ バインディング構文を使用して`CategoryArgument='<%# Eval("CategoryID") %>'`。 これら 2 つのプロパティを指定したら、次のよう LinkButton s の宣言型構文になります。


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

ボタンがクリックされたとき、ポストバックが発生し、DataList または Repeater の`ItemCommand`イベントが発生します。 イベント ハンドラーは、[s] ボタンを渡される`CommandName`と`CommandArgument`値。

Repeater s のイベント ハンドラーを作成`ItemCommand`イベント ハンドラーに渡されるイベントと 2 番目のパラメーターに注意してください (という`e`)。 この 2 つ目のパラメーターは型[ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx)あり、次の 4 つのプロパティ。

- `CommandArgument` //クリックされたボタンでは、秒の値`CommandArgument`プロパティ
- `CommandName` ボタンの秒の値`CommandName`プロパティ
- `CommandSource` クリックされたボタン コントロールへの参照
- `Item` 参照、 [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx)クリックしてされたボタンを格納している; として、Repeater にバインドされている各レコードが示されて、 `RepeaterItem`

選択したカテゴリ s 以降`CategoryID`によって渡された、`CommandArgument`プロパティで選択したカテゴリに関連付けられている製品のセットを取得しました、`ItemCommand`イベント ハンドラー。 これらの製品を BulletedList コントロールにバインドできますし、 `ItemTemplate` (私たちを追加するには、まだ ve)。 参照を設定した後、残っている、BulletedList を追加するにはすべて、`ItemCommand`イベント ハンドラー、し、手順 4. で取り上げる、選択したカテゴリの製品のセットをバインドします。

> [!NOTE]
> DataList s`ItemCommand`イベント ハンドラーの型のオブジェクトが渡される[ `DataListCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx)と同じ 4 つのプロパティを備えた、`RepeaterCommandEventArgs`クラス。


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>手順 4: 箇条書きリストで選択したカテゴリ、製品を表示します。

Repeater s 内で選択したカテゴリの製品を表示できる`ItemTemplate`多数のコントロールを使用します。 Repeater、DataList、DropDownList、GridView、およびに入れ子になった別追加でした。 箇条書きリストとして、製品を表示したいので、BulletedList コントロールを使用します。 返す、 `CustomButtons.aspx` s 宣言型マークアップをページで、BulletedList コントロールを追加、`ItemTemplate`製品の表示 LinkButton 後。 集合 BulletedLists s`ID`に`ProductsInCategory`します。 BulletedList を介して指定されたデータ フィールドの値を表示する、`DataTextField`プロパティです。 このコントロールは製品の情報にバインドされている、設定であるため、`DataTextField`プロパティを`ProductName`します。


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

`ItemCommand`イベント ハンドラーでは、このコントロールを使用して参照`e.Item.FindControl("ProductsInCategory")`選択したカテゴリに関連付けられている製品のセットにバインドします。


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

アクションを実行する前に、`ItemCommand`イベント ハンドラーに s が最初に、入力方向の値を確認することをお勧め`CommandName`。 以降、`ItemCommand`イベント ハンドラーは、ときに発生*任意*ボタンがクリックされたテンプレートの使用に複数のボタンがある場合、`CommandName`を実行するには、どのようなアクションを識別する値。 チェック、`CommandName`ここでは、議論の余地を 1 つのボタンのみがあるためが、フォームすることを推奨します。 次に、`CategoryID`から取得したカテゴリの`CommandArgument`プロパティ。 テンプレートで BulletedList コントロールが、参照されの結果にバインドされている、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド。

DataList、内のボタンを使用します。 前のチュートリアルで[編集の 概要と、DataList でデータを削除する](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)、を使用して特定の項目の主キーの値がわかりました、`DataKeys`コレクション。 このアプローチは、DataList では、中に、Repeater がありません、`DataKeys`プロパティ。 代わりに、s をクリックしてなどの主キーの値を提供するための別のアプローチを使用する必要があります`CommandArgument`プロパティまたはテンプレート内で非表示のラベルの Web コントロールに主キーの値を割り当てる場合や、に戻り、値を読み取る`ItemCommand`イベント ハンドラーを使用して`e.Item.FindControl("LabelID")`します。

完了した後、`ItemCommand`イベント ハンドラーでは、このページで、ブラウザーでテストする少し。 図 7 に示すようを表示する製品をクリックするとリンク ポストバックが発生して、BulletedList で選択したカテゴリの製品を表示します。 さらに注意してくださいことは、この製品の情報が残っている場合でも、その他のカテゴリの製品を表示するリンクをクリックします。

> [!NOTE]
> 1 つだけのカテゴリの製品が同時に表示されるように、このレポートの動作を変更する場合は、s BulletedList コントロールを設定するだけ`EnableViewState`プロパティを`False`します。


[![選択したカテゴリの製品を表示するため、BulletedList](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**図 7**: 選択したカテゴリの製品を表示するため、BulletedList ([フルサイズの画像を表示する をクリックします](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))。


## <a name="summary"></a>まとめ

DataList と Repeater コントロールでは、任意の数をテンプレート内のボタン、Linkbutton、または ImageButtons を含めることができます。 このようなボタンをクリックすると、ポストバックを発生させて、`ItemCommand`イベント。 クリックしてされたボタンには、カスタム サーバー側のアクションを関連付けるのイベント ハンドラーを作成、`ItemCommand`イベント。 このイベント ハンドラーで受信をまずチェック`CommandName`どのボタンがクリックされたかを決定する値。 S をクリックして追加情報を指定することができます必要に応じて`CommandArgument`プロパティ。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Dennis Patterson をしました。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](custom-buttons-in-the-datalist-and-repeater-vb.md)
