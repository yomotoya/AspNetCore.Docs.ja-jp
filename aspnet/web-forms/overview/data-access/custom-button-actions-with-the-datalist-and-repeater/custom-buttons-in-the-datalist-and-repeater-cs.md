---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: "DataList でリピータ (c#) カスタム ボタン |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、リピータを使用して、システムでは、その associ を表示するためのボタンを提供する各カテゴリにカテゴリを一覧表示するインターフェイスを構築しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a072ae18bbb19d086eb825c6e72b68d40b2e429
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>DataList でリピータ (c#) カスタム ボタン
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe)または[PDF のダウンロード](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> このチュートリアルではおリピータを使用して、システムでは、BulletedList コントロールを使用して関連付けられた製品を表示するためのボタンを提供する各カテゴリにカテゴリを一覧表示するインターフェイスを構築します。


## <a name="introduction"></a>はじめに

過去 17 個の DataList とリピータ チュートリアル全体では読み取り専用の例と編集および削除の例を作成しました。 編集と削除、DataList 内で機能させるために、DataList s にボタンを追加お`ItemTemplate`、ポストバックの原因となったクリックして、[s] ボタンに対応する DataList イベントを発生させた`CommandName`プロパティです。 たとえば、ボタンを追加する、`ItemTemplate`で、`CommandName`編集のプロパティの値により DataList s`EditCommand`ポストバックを発生させると 1 つ、`CommandName`削除が発生し、`DeleteCommand`です。

さらに編集ボタンを削除してに、DataList およびリピータ コントロール含めることができますもボタン、ある、または ImageButtons をクリックすると、なんらかのカスタム サーバー側ロジックを実行します。 このチュートリアルではリピータを使用して、システムで、カテゴリを一覧表示するインターフェイスを構築します。 リピータ カテゴリごとに関連付けられている製品 BulletedList コントロールを使用して、カテゴリを表示するためのボタンが含まれます (図 1 を参照してください)。


[![表示製品リンク表示では、カテゴリの製品箇条書きリストをクリックして](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**図 1**: 箇条書きで s 製品カテゴリを製品のリンクを表示する表示をクリックすると ([フルサイズのイメージを表示するをクリックして](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>手順 1: カスタム ボタンのチュートリアル Web ページの追加

カスタム ボタンを追加する方法を見ると、前に、まず、このチュートリアルに必要とする、web サイト プロジェクトで ASP.NET ページを作成するのにはしばらく s を使用できます。 という名前の新しいフォルダーを追加することによって開始`CustomButtonsDataListRepeater`です。 次に、各ページに関連付けるように、そのフォルダーに次の 2 つの ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `CustomButtons.aspx`


![カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加します。](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**図 2**: カスタム ボタンに関連するチュートリアルについては、ASP.NET ページを追加します。


などの他のフォルダーで`Default.aspx`で、`CustomButtonsDataListRepeater`フォルダーは、チュートリアルのセクションに一覧表示します。 注意してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 このユーザー コントロールを追加して`Default.aspx`をページのデザイン ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**図 3**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


最後に、エントリとして、ページを追加、`Web.sitemap`ファイル。 具体的には、DataList リピータとページングと並べ替えの後に、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、編集、挿入、およびチュートリアルを削除する項目が含まれています。


![サイト マップではカスタム ボタンのチュートリアル」のエントリが含まれるようになりました](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**図 4**: サイト マップではカスタム ボタンのチュートリアル」のエントリが含まれるようになりました


## <a name="step-2-adding-the-list-of-categories"></a>手順 2: カテゴリの一覧を追加します。

このチュートリアルの必要がありますを表示する製品 LinkButton と共にすべてのカテゴリを一覧表示する Repeater を作成するをクリックすると、箇条書きリストに関連付けられているカテゴリの製品が表示されます。 システム内のカテゴリを一覧表示する単純な Repeater をまず作成 s を使用できます。 開いて開始、 `CustomButtons.aspx`  ページで、`CustomButtonsDataListRepeater`フォルダーです。 ツールボックスからデザイナーとセットにリピータをドラッグしてその`ID`プロパティを`Categories`です。 次に、リピータ s のスマート タグから新しいデータ ソース コントロールを作成します。 具体的には、という名前の新しい ObjectDataSource コントロールを作成`CategoriesDataSource`からそのデータを選択する、`CategoriesBLL`クラスの`GetCategories()`メソッドです。


[![ObjectDataSource CategoriesBLL クラスの GetCategories() メソッドを使用して構成します。](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**図 5**: 構成を使用する ObjectDataSource、`CategoriesBLL`クラス s`GetCategories()`メソッド ([フルサイズのイメージを表示するをクリックして](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


Visual Studio が、既定値を作成、DataList コントロールとは異なり`ItemTemplate`データ ソースに基づき、s リピータ テンプレート必要があります手動で定義します。 さらに、リピータのテンプレートを作成して、宣言的に編集する必要があります (つまり、ある s テンプレートの編集なしオプション リピータ s のスマート タグで)。

左下隅の [ソース] タブをクリックし、追加、`ItemTemplate`項目 %s の名前を表示する、`<h3>`要素とその説明の段落にタグを付ける; が含まれて、`SeparatorTemplate`水平線を表示する (`<hr />`) 間カテゴリです。 追加の LinkButton こともその`Text`プロパティを表示する製品に設定します。 次の手順を完了すると、ページ s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

図 6 は、ブラウザーで表示したときに、ページを示します。 各カテゴリの名前と説明が表示されます。 製品の表示 ボタンをクリックすると、ポストバックがアクションをまだ実行しません。


[![各カテゴリ名と説明が表示されます、製品を表示する LinkButton と共に](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**図 6**: 各カテゴリの名前と説明が表示されます、製品を表示する LinkButton と共に ([フルサイズのイメージを表示するをクリックして](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>実行するサーバー側ロジックときに、表示製品 LinkButton がクリックされた手順 3。

ポストバックが発生するボタン、linkbutton コントロールまたは DataList またはリピータ テンプレート内の ImageButton がクリックされたときと DataList またはリピータの`ItemCommand`イベントが発生します。 加え、`ItemCommand`イベント、DataList コントロールも発生する可能性が、別の特定のイベント場合ボタン s`CommandName`プロパティが、予約済み (削除、編集、キャンセル、Update、または選択)、文字列のいずれかに設定が、`ItemCommand`イベントが*常に*発生します。

DataList またはリピータ内で、ボタンをクリックすることがよくを渡す必要が (であることを複数の両方の編集など、コントロール内のボタンと [削除] ボタンの場合) クリックしてされたボタンおよび追加の情報をおそらくと共に (次のようにプライマリ キーの値のボタンがクリックされたアイテム)。 ボタン、LinkButton、および ImageButton に渡される値を持つ 2 つのプロパティを提供、`ItemCommand`イベントのハンドラー。

- `CommandName`通常、テンプレート内の各ボタンの識別に使用する文字列
- `CommandArgument`主キーの値など、いくつかのデータ フィールドの値を保持するために一般的に使用

この例では、設定、LinkButton s `CommandName` ShowProducts と現在のレコードの主キー値をバインドするプロパティ`CategoryID`を`CommandArgument`databinding 構文を使用してプロパティ`CategoryArgument='<%# Eval("CategoryID") %>'`です。 これら 2 つのプロパティを指定してから LinkButton s の宣言構文は次のようになります。


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

ときに、ボタンがクリックされた、ポストバックが発生し、DataList またはリピータの`ItemCommand`イベントが発生します。 イベント ハンドラーは、[s] ボタンに渡されます`CommandName`と`CommandArgument`値。

リピータ s のイベント ハンドラーを作成`ItemCommand`、イベント ハンドラーに渡されたイベントと 2 番目のパラメーター (名前付き`e`)。 この 2 番目のパラメーターの型は[ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx)し、次の 4 つのプロパティします。

- `CommandArgument`クリックされたボタン秒の値`CommandArgument`プロパティ
- `CommandName`ボタンの秒の値`CommandName`プロパティ
- `CommandSource`クリックされたボタン コントロールへの参照
- `Item`参照、 [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx)がクリックされたボタンが含まれている; リピータにバインドされている各レコードとして示されています、`RepeaterItem`

選択したカテゴリ s 以降`CategoryID`経由で渡される、`CommandArgument`プロパティで選択したカテゴリに関連付けられている製品のセットを取得できます、`ItemCommand`イベント ハンドラー。 BulletedList コントロールにこれらの製品をバインドすることができますし、 `ItemTemplate` (私たちを追加するには、まだしました)。 参照を設定した後、残っている、BulletedList を追加するにはすべて、`ItemCommand`イベント ハンドラー、し、手順 4. で取り上げる、選択したカテゴリの製品のセットをバインドします。

> [!NOTE]
> DataList s`ItemCommand`イベント ハンドラーは型のオブジェクトに渡されます[ `DataListCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx)と同じ 4 つのプロパティを提供する、`RepeaterCommandEventArgs`クラスです。


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>手順 4: 箇条書きリストで選択したカテゴリの製品を表示します。

リピータ s 内で選択したカテゴリの製品を表示できる`ItemTemplate`コントロールの任意の数を使用します。 リピータ、DataList、DropDownList、GridView、およびなど入れ子になった別追加でした。 箇条書きとして製品を表示するので、ただし、BulletedList コントロールを使用します。 返す、 `CustomButtons.aspx` s 宣言型マークアップのページで、BulletedList コントロールを追加して、`ItemTemplate`表示製品 LinkButton 後です。 集合 BulletedLists s`ID`に`ProductsInCategory`です。 BulletedList を介して指定されたデータ フィールドの値を表示する、`DataTextField`プロパティです。 このコントロールは製品の情報にバインドされている、設定であるため、`DataTextField`プロパティを`ProductName`です。


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

`ItemCommand` 、イベント ハンドラーを使用してこのコントロールを参照`e.Item.FindControl("ProductsInCategory")`し、選択したカテゴリに関連付けられている製品のセットにバインドします。


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

のどのアクションを実行する前に、 `ItemCommand` 、イベント ハンドラーが最初に値を確認して、入力方向が賢明です`CommandName`です。 以降、`ItemCommand`イベント ハンドラーは、ときに発生*任意*ボタンがクリックされたテンプレートの使用に複数のボタンがある場合、`CommandName`を実行するアクションを識別する値。 チェック、`CommandName`ここでは、議論を 1 つのボタンのみがあるため、フォームにことをお勧めします。 次に、`CategoryID`から取得したカテゴリの`CommandArgument`プロパティです。 BulletedList コントロール、テンプレートでは参照されているしの結果にバインドされている、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。

など、DataList 内のボタンを使用する前のチュートリアルで[編集の概要と、DataList のデータを削除する](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)を使用して特定のアイテムの主キーの値と判断しました、`DataKeys`コレクション。 このアプローチでは、DataList にも、中にリピータが、`DataKeys`プロパティです。 代わりに、s をクリックしてなどの主キーの値を提供するため、別の方法を使用する必要があります`CommandArgument`プロパティまたはテンプレート内で非表示のラベルの Web コントロールに主キーの値を割り当てる場合や、に戻り、値を読み取る`ItemCommand`イベント ハンドラーを使用して`e.Item.FindControl("LabelID")`です。

完了した後、 `ItemCommand` 、イベント ハンドラー、ブラウザーのこのページをテストします。 図 7 に示す、製品を表示するをクリックするとリンク ポストバックを発生させるし、BulletedList で選択したカテゴリの製品が表示されます。 さらに、注意してくださいことは、この製品の情報が残っている場合でも、他のカテゴリを表示する製品のリンクをクリックします。

> [!NOTE]
> BulletedList コントロール s を単に設定する場合、このレポートの動作を変更する、一度に 1 つだけのカテゴリの製品が表示されているように、`EnableViewState`プロパティを`False`です。


[![選択したカテゴリの製品を表示する、BulletedList を使用します。](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**図 7**: A BulletedList を使用して、選択したカテゴリの製品を表示 ([フルサイズのイメージを表示するをクリックして](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>まとめ

DataList とリピータ コントロールは、任意の数をテンプレート内のボタン、ある、または ImageButtons を含めることができます。 クリックすると、このようなボタンがポストバックを発生させるし、生成、`ItemCommand`イベント。 クリックしてされたボタンにカスタム サーバー側のアクションを関連付けるのイベント ハンドラーを作成、`ItemCommand`イベント。 このイベント ハンドラーで受信をまずチェック`CommandName`クリックしてされたボタンを決定する値。 S をクリックして追加情報を指定することができます必要に応じて`CommandArgument`プロパティです。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Dennis Patterson しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[次へ](custom-buttons-in-the-datalist-and-repeater-vb.md)
