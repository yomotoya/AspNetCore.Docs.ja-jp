---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: (C#) のオプション ボタンの GridView 列を追加する |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルは、ラジオ ボタンの列の 1 つの行を選択した場合のより直観的な方法をユーザーに提供する GridView コントロールを追加する方法を検索しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: 32bfe8463d80dfcff925f3bd6f31de67b071fc0b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>(C#) のオプション ボタンの GridView 列を追加します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe)または[PDF のダウンロード](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> このチュートリアルは、GridView の 1 つの行を選択した場合のより直観的な方法をユーザーに提供する GridView コントロールにラジオ ボタンの列を追加する方法を検索します。


## <a name="introduction"></a>はじめに

GridView コントロールには、多くの組み込み機能が用意されています。 テキスト、イメージ、ハイパーリンク、およびボタンを表示するためのさまざまなフィールドの数が含まれています。 カスタマイズのためのテンプレートがサポートしています。 マウスの数回のクリックで、s、ボタンを使用して各行を選択できる GridView を作成したり、編集または削除する機能を有効にすることができます。 提供されている機能の多く、にもかかわらず多くの場合がありますを追加する必要がありますがサポートされていない機能に追加する場合。 このチュートリアルを次の 2 つの追加機能を組み込む GridView の機能を強化する方法について確認します。

このチュートリアルと次のセクションは、列の選択プロセスの向上にフォーカスします。 調べると、[マスター/詳細 DetailView で選択可能なマスター GridView の使用について詳しく説明](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)を選択 ボタンを含む GridView に、CommandField を追加できます。 クリックするとポストバックに陥りますと GridView の`SelectedIndex`プロパティがある選択ボタンがクリックされた行のインデックスを更新します。 [マスター/詳細 DetailView で選択可能なマスター GridView の使用について詳しく説明](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)チュートリアルでは、この機能を使用して、選択された GridView 行の詳細を表示する方法を説明しました。

多くの状況で動作しますが、選択ボタン、いないも他のユーザーが作業可能性があります。 ボタンを使用してではなく他の 2 つのユーザー インターフェイス要素でよく使われる選択: オプション ボタンおよびチェック ボックスをオンします。 GridView お補強できます。 できるように、[選択] ボタンでは、代わりに、各行には、オプション ボタンまたはチェック ボックスが含まれています。 ここでユーザーのみが選択できる GridView レコードの 1 つのシナリオでラジオ ボタン可能性がありますよりも優先される、選択ボタン。 ユーザーが選択できる可能性がある状況では、web ベースの電子メール アプリケーションで、ユーザーを選択するチェック ボックスを削除する複数のメッセージなど、複数のレコードは、[選択] ボタンまたはオプション ボタンから利用可能な機能を提供していますユーザー インターフェイス。

このチュートリアルは、GridView にラジオ ボタンの列を追加する方法を検索します。 作業を進めるチュートリアルでは、チェック ボックスの使用について説明します。

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>手順 1: 拡張 GridView Web ページを作成します。

ラジオ ボタンの列を含めるように GridView の拡張を開始する前に、は、まず、このチュートリアルを次の 2 つの必要がありますを web サイト プロジェクトに、ASP.NET ページを作成するのにはしばらく s を使用できます。 という名前の新しいフォルダーを追加することによって開始`EnhancedGridView`です。 次に、各ページに関連付けるように、そのフォルダーに次の ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![SqlDataSource に関連するチュートリアルについては、ASP.NET ページを追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**図 1**: SqlDataSource に関連するチュートリアルについては、ASP.NET ページを追加します。


などの他のフォルダーで`Default.aspx`で、`EnhancedGridView`フォルダーは、チュートリアルのセクションに一覧表示します。 注意してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


最後に、これら 4 つのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、使用方法の後に、次のマークアップを追加、SqlDataSource コントロール`<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、編集、挿入、およびチュートリアルを削除する項目が含まれています。


![サイト マップでは GridView のチュートリアルを強化するためのエントリが含まれるようになりました](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**図 3**: サイト マップでは GridView のチュートリアルを強化するためのエントリが含まれるようになりました


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>手順 2: GridView に仕入先を表示します。

このチュートリアルは、s、ラジオ ボタンを提供する GridView の各行と、顧客が米国からサプライヤーを一覧表示する GridView のビルドを使用できます。 ラジオ ボタンを使用して仕入先を選択すると、ユーザーがボタンをクリックして、供給業者製品を表示できます。 このタスクを簡単に思えるかもしれませんもできるように、特にわかりにくい微妙な色の数。 これらの微妙なを詳しくお前にまず仕入先を一覧表示する GridView s を使用できます。

開いて開始、 `RadioButtonField.aspx`  ページで、 `EnhancedGridView` GridView をツールボックスからデザイナーにドラッグしてフォルダーです。 集合 GridView s`ID`に`Suppliers`し、そのスマート タグから新しいデータ ソースを作成する選択します。 名前付き、ObjectDataSource を具体的には、作成`SuppliersDataSource`からデータを抽出する、`SuppliersBLL`オブジェクト。


[![SuppliersDataSource をという名前の新しい ObjectDataSource を作成します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**図 4**: 名前付き新しい ObjectDataSource 作成`SuppliersDataSource`([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![構成、ObjectDataSource SuppliersBLL クラスを使用するには](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**図 5**: 構成を使用する ObjectDataSource、`SuppliersBLL`クラス ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


のみ、米国に住む業者を一覧表示する、選択、`GetSuppliersByCountry(country)`メソッドを選択 タブで、ドロップダウン リストからです。


[![構成、ObjectDataSource SuppliersBLL クラスを使用するには](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**図 6**: 構成を使用する ObjectDataSource、`SuppliersBLL`クラス ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


更新プログラム タブから、選択オプション (なし) と、次へ をクリックします。


[![構成、ObjectDataSource SuppliersBLL クラスを使用するには](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**図 7**: 構成を使用する ObjectDataSource、`SuppliersBLL`クラス ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


以降、`GetSuppliersByCountry(country)`メソッドは、パラメーターを受け取り、データ ソースの構成ウィザードの指示に従って us そのパラメーターのソース。 指定するには、ハード コーディングされた値 (アメリカ合衆国、この例では)、ソースのドロップダウン リストが None に設定し、テキスト ボックスで、既定値を入力パラメーターのままにします。 ウィザードを完了するには、[完了] をクリックします。


[![国のパラメーターの既定値として USA を使用します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**図 8**: の既定値として使用する USA、`country`パラメーター ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


ウィザードを完了すると、各仕入先データ フィールドの BoundField が GridView に含まれます。 削除以外のすべての`CompanyName`、`City`と`Country`BoundFields、名前を変更し、 `CompanyName` BoundFields`HeaderText`業者にプロパティです。 その後、GridView と ObjectDataSource 宣言の構文を次のようになります。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

このチュートリアルでは、秒を表示できるようにする、仕入先を選択した製品仕入先の一覧と同じページで、または別のページを使用できます。 これに合わせて、ページに 2 つのボタンの Web コントロールを追加します。 I ve セット、`ID`これら 2 つのボタンの s`ListProducts`と`SendToProducts`、アイデア時に`ListProducts`がクリックされたポストバックが発生し、選択した供給業者の製品が、同じページに表示されます`SendToProducts`クリックすると、ユーザーは、製品を一覧表示する別のページに示されているされます。

図 9 を示しています、 `Suppliers` GridView と 2 つのボタンの Web ブラウザーで表示したときに制御します。


[![米国から業者がある、名前、市区町村、および国の情報を一覧表示](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**図 9**: USA がある、名前、市区町村、および国の情報が表示されてからサプライヤーもの ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>手順 3: オプション ボタンの列の追加

この時点で、 `Suppliers` GridView は、米国に住む会社名、市区町村、および各仕入先の国または地域を表示する次の 3 つの BoundFields します。 これがまだ不足しているラジオ ボタンの列ただしです。 残念ながら、GridView されないには、組み込み RadioButtonField、それ以外の場合おでしただけグリッドに追加して実行が含まれます。 TemplateField を追加し、構成代わりに、その`ItemTemplate`ラジオ ボタン GridView の行ごとに結果として得られる、ラジオ ボタンの表示にします。

最初に、おように見えますが、RadioButton Web コントロールを追加することで、目的のユーザー インターフェイスを実装することができます、 `ItemTemplate` TemplateField のです。 GridView の行ごとに 1 つのオプション ボタンを追加これが実際に、ラジオ ボタンはグループ化することはできませんし、したがって相互に排他的ではありません。 つまり、エンドユーザーは GridView から同時に複数のオプション ボタンを選択することです。

Let s がので、このアプローチを実装する場合でも、RadioButton Web コントロールの TemplateField を使用しても、必要な機能は提供しません、結果として得られるラジオ ボタンがグループ化されていない理由について確認する価値はあるでしょう。 まず、左端のフィールドを行う、仕入先 GridView に TemplateField を追加します。 次に、GridView s のスマート タグからテンプレートの編集リンクをクリックし、RadioButton Web コントロールを TemplateField s をツールボックスからドラッグ`ItemTemplate`(図 10 参照)。 集合 RadioButton s`ID`プロパティを`RowSelector`と`GroupName`プロパティを`SuppliersGroup`です。


[![ItemTemplate に RadioButton Web コントロールを追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**図 10**: する RadioButton Web コントロールを追加、 `ItemTemplate` ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


デザイナーでこれらの追加を行った後、GridView のマークアップを次のようになります。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

RadioButton s [ `GroupName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx)は一連のラジオ ボタンをグループ化するために使用されます。 すべての RadioButton コントロールを同じ`GroupName`値では、グループ化と見なされます、一度に 1 つだけのラジオ ボタン グループから選択できます。 `GroupName`プロパティが表示されるオプション ボタン秒の値を指定`name`属性。 ブラウザーがラジオ ボタンを調べて`name`無線を決定する属性のグループ化 ボタンをクリックします。

追加された RadioButton Web コントロールで、`ItemTemplate`ブラウザーからこのページを参照してくださいし、グリッドの行のラジオ ボタンをクリックします。 図 11 としてすべての行を選択できるようにする、事前の通知方法のオプション ボタン グループ分けされていないを示しています。


[![GridView のオプション ボタンはグループ化されていません。](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**図 11**: GridView のオプション ボタンはグループ化されていない ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


ラジオ ボタンがグループ化されていないためにです、レンダリングされた`name`属性が異なる場合、同じでも`GroupName`プロパティの設定。 これらの相違点を表示するには、ブラウザーから/ソースの表示を実行し、ラジオ ボタンのマークアップを確認します。


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

両方の通知、`name`と`id`属性プロパティ ウィンドウで指定された正確な値ではないその他の数が付加される`ID`値。 その他`ID`、レンダリングの先頭に追加した値`id`と`name`属性は、`ID`無線の親コントロールのボタン、 `GridViewRow` s `ID` s, GridView の`ID`では、コンテンツ コントロール s `ID`、および Web フォームの`ID`です。 これら`ID`s は追加されるため、それぞれレンダリングされます GridView で Web コントロールに一意な`id`と`name`値。

各レンダリング管理の要件が異なる`name`と`id`これは、どのブラウザーは、クライアント側とそれを識別する方法、web サーバーにどのようなアクション上の各コントロールを一意に識別またはポストバック時に変更が発生したためです。 たとえば、RadioButton s チェック状態が変更されたときに、いくつかのサーバー側コードを実行したいとします。 このためには、RadioButton s を設定して`AutoPostBack`プロパティを`true`のイベント ハンドラーを作成して、`CheckChanged`イベント。 ただし場合、レンダリングされた`name`と`id`値はすべてのオプション ボタンの同じでしたのポストバックをどのような固有の仕様を確認できませんでしたラジオ ボタンがクリックしてされました。

これの短い形式は、RadioButton Web コントロールを使用して GridView にラジオ ボタンの列を作成することはできないことです。 代わりではなく古風な手法を使用して GridView 行ごとに適切なマークアップが挿入されることを確認してください。 おあります。

> [!NOTE]
> RadioButton Web コントロールは、オプション ボタンをテンプレートに追加すると、HTML コントロールを一意に含まれますと同じように`name`属性をグループ化を解除するグリッドでラジオ ボタンを作成します。 HTML コントロールに慣れていない場合は、自由にこのノートでは無視して HTML コントロールはほとんど使用されるため、ASP.NET 2.0 では特にください。 詳細に知りたい場合を参照してください。 しかし、 [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s ブログ エントリ[Web コントロールや HTML コントロール](http://www.odetocode.com/Articles/348.aspx)です。


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>リテラル コントロールを使用してラジオ ボタンのマークアップを挿入します

すべての GridView でラジオ ボタン グループ正しく、するために必要がありますにラジオ ボタンのマークアップを手動で挿入、`ItemTemplate`です。 各オプション ボタンでは、同じ必要がある`name`属性しますが、一意である必要があります`id`属性の場合にクライアント側スクリプトを使用して、オプション ボタンにアクセスするようにします)。 ユーザーがラジオ ボタンを選択すると、投稿、ページのバックアップ、ブラウザーが返信選択したオプション ボタン秒の値`value`属性。 したがって、各オプション ボタンを一意な必要`value`属性。 最後に、ポストバックでいただくために追加することを確認、`checked`属性が選択されている、それ以外の場合、ユーザーが選択および投稿バックアップ後に 1 つのオプション ボタンをオプション ボタンに戻りますは既定の状態 (すべて選択されていない)。

テンプレートに低レベルのマークアップを挿入するために実行できる 2 つの方法はあります。 さまざまなマークアップと分離コード クラスで定義されているメソッドを書式設定への呼び出しを行うには 1 つです。 この手法はで最初に説明した、 [GridView コントロールを使用して TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)チュートリアルです。 ここで、ようになります。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

ここでは、`GetUniqueRadioButton`と`GetRadioButtonValue`返される、適切な分離コード クラスで定義されているメソッドになります`id`と`value`属性の各オプション ボタンの値。 このアプローチで割り当てることは、`id`と`value`、その属性が遅れを指定する必要がある場合、 `checked` databinding 構文はデータがまず GridView にバインドされている場合にのみ実行されるため、属性値。 したがって、GridView のビュー ステートを有効になっている場合は、書式指定メソッドだけ起動時にページが最初に読み込まれた (または、データ ソースに GridView が明示的に再バインドすると)、および関数を設定するため、`checked`で t が勝利した属性は呼び出せませんポストします。 これではなく微妙な問題と少し任せこのため、この記事の範囲外です。 ただし、いただくために、上記の方法を使用してする行き詰まったをポイントする作業を通じてすればしないでください。 このような won 演習 t を取得すると、作業中のバージョンに近い、中には、GridView と、データ バインドのライフ サイクルの理解を促進するのに役立ちます。

追加する他のアプローチの挿入のカスタム テンプレートとこのチュートリアルで使用するアプローチでの低レベルのマークアップが、 [Literal コントロール](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx)テンプレートにします。 その後、GridView s `RowCreated`または`RowDataBound`、イベント ハンドラー Literal コントロールをプログラムでアクセスできると、その`Text`プロパティを出力するマークアップに設定します。

TemplateField s から RadioButton を削除することによって開始`ItemTemplate`、Literal コントロールに置き換えます。 S リテラル コントロール設定`ID`に`RadioButtonMarkup`です。


[![リテラル コントロール ItemTemplate を追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**図 12**: するリテラル コントロールを追加、 `ItemTemplate` ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


GridView s のイベント ハンドラーを次に、作成`RowCreated`イベント。 `RowCreated`イベントが追加されたすべての行に対してかどうか、データはされている再バインド GridView に 1 回だけを起動します。 つまり、ポストバックでも、データがビュー状態から再読み込みされるときに、`RowCreated`イベントが発生し、これは、代わりに使用している理由`RowDataBound`(を発生させるだけデータが明示的にデータ バインド時、Web コントロール)。

このイベント ハンドラーでのみが必要場合の続行 re データ行を処理することです。 プログラムで参照するデータ行ごとに、 `RadioButtonMarkup` Literal コントロールとその`Text`プロパティを出力するマークアップをします。 マークアップを生成されますが、ラジオを作成、次のコードに示すボタンが`name`属性に設定されている`SuppliersGroup`が`id`属性に設定されている`RowSelectorX`ここで、 *X* GridView の行のインデックスは、および`value`属性が GridView の行のインデックスに設定します。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

GridView の行を選択し、ポストバックが発生したとご感想、`SupplierID`仕入先を選択したのです。 したがって、考えることが各オプション ボタンの値を実際にする必要があります、`SupplierID`ではなく GridView の行のインデックス)。 これが特定の状況で動作、中にはなります、セキュリティ上のリスクを無条件に同意して処理する、`SupplierID`です。 たとえば、GridView には、米国に住む業者のみが一覧表示します。 ただし場合、`SupplierID`有害ユーザーから、操作を停止するには、どのような s、オプション ボタンから直接渡される、`SupplierID`ポストバックで送り返す値しますか? として行のインデックスを使用して、`value`とし、取得、`SupplierID`からのポストバック時に、`DataKeys`コレクション、ユーザーのみを使用しているのいずれかの確認できる、 `SupplierID` GridView 行の 1 つに関連付けられている値。

このイベント ハンドラーのコードを追加した後、時間がかかるをブラウザーでページをテストします。 最初に、その 1 つだけのラジオに注意してください、グリッドでボタンを一度に選択できます。 ただし、ラジオ ボタンを選択し、一方のボタンをクリックして、ポストバックが発生して、すべてのオプション ボタンが初期状態に戻す、ポストバックで選択したオプション ボタンが不要になったオンになっている)。 この問題を解決する必要がありますを補強、`RowCreated`ポストバックから送信された選択したラジオ ボタンのインデックスを検査して追加ようにイベント ハンドラー、`checked="checked"`行インデックスの一致結果のマークアップを生成するための属性です。

ポストバックが発生すると、ブラウザーを返信、`name`と`value`選択したオプション ボタンのです。 プログラムで値を使用して`Request.Form["name"]`です。 [ `Request.Form`プロパティ](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx)提供、 [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx)されたフォーム変数を表すです。 フォーム変数では、名前および web ページのフォーム フィールドの値はされ、ポストバックに陥りますされるたびに、web ブラウザーによって再び送信されます。 レンダリングされた`name`GridView のラジオ ボタンの属性が`SuppliersGroup`web ページがポストバック ブラウザーが送信されるときに、 `SuppliersGroup=valueOfSelectedRadioButton` (他のフォーム フィールド) と共に web サーバーにします。 この情報にアクセスすることができますし、`Request.Form`プロパティを使用して:`Request.Form["SuppliersGroup"]`です。

以降お必要がありますを決定する、選択したオプション ボタン インデックスが作成されますだけではなく、`RowCreated`ですが、イベント ハンドラー、 `Click` Button Web コントロールのイベント ハンドラー、let s を追加、`SuppliersSelectedIndex`プロパティを返す分離コードクラスを`-1`ラジオ ボタンが選択されていない場合、オプション ボタンのいずれかが選択されている場合は、選択されたインデックス。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

このプロパティを追加するで追加するわかって、`checked="checked"`内のマークアップ、`RowCreated`イベント ハンドラーときに`SuppliersSelectedIndex`と等しい`e.Row.RowIndex`です。 このロジックを含めるイベント ハンドラーを更新します。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

この変更により、選択したオプション ボタンのままポストバック後に選択します。 ページが最初にアクセスすると、最初の GridView 行のラジオ ボタンが選択されました (ではなく現在これは既定で選択したオプション ボタンを適用しない場合は、ことができるように、動作を変更おでしたラジオ ボタンが選択されているを指定することができました動作)。 既定で選択されている最初のラジオ ボタンを表示するには、単に変更する、`if (SuppliersSelectedIndex == e.Row.RowIndex)`には、次のステートメント:`if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`です。

この時点でグループ化されたラジオ ボタンの列では、1 つが GridView の行を選択し、ポストバック間で記憶される GridView に追加されました。 次のステップでは、選択した業者によって提供される製品を表示します。 手順 4. で会いしましょうユーザーを他のページにリダイレクトする方法、選択したに沿って送信`SupplierID`です。 手順 5 では同じページ上の GridView で選択したサプライヤーの製品を表示する方法が表示されます。

> [!NOTE]
> TemplateField (この時間のかかる手順 3 のフォーカス設定) を使用して作成することも、カスタム`DataControlField`適切なユーザー インターフェイスと機能を表示するクラス。 [ `DataControlField`クラス](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx)BoundField、CheckBoxField、TemplateField、およびその他の組み込みの GridView と DetailsView のフィールドが派生元となる基本クラスです。 カスタム作成する`DataControlField`クラスがありことを意味ラジオ ボタンの列だけ宣言の構文を使用して追加でしたも実行するように他の web ページと非常に簡単に他の web アプリケーションの機能をレプリケートします。


ASP.NET のコントロールをコンパイルする、カスタムを作成するしたら、ただし、わかりますこうする全世界のかなりし、伴って微妙なと慎重に処理する必要がありますのエッジ ケースのホスト。 そのため、おはせず、カスタムのラジオ ボタンの列を実装する`DataControlField`ここではクラス、および TemplateField オプションのオプションを使用します。 おそらく作成、使用、およびカスタムの展開を調査する必要があります`DataControlField`将来チュートリアル内のクラスです。

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>手順 4: 別のページで、選択したサプライヤー製品を表示します。

ユーザーが GridView の行を選択した後仕入先を選択した製品を表示する必要があります。 状況によってを同じページで行うにはお勧め他のユーザーに、別のページにこれらの製品を表示することもできます。 最初に、別のページで、製品を表示する方法を調べて s を使用できます。手順 5 に紹介する GridView の追加`RadioButtonField.aspx`選択した供給業者の商品を表示します。

現在はページ上の 2 つのボタン Web コントロール`ListProducts`と`SendToProducts`です。 ときに、`SendToProducts`ボタンがクリックされると、ユーザーに送信する`~/Filtering/ProductsForSupplierDetails.aspx`です。 このページで作成されて、[マスター/詳細フィルター間で 2 つのページ](../masterdetail/master-detail-filtering-across-two-pages-cs.md)チュートリアルと業者の製品が表示されますが`SupplierID`という名前のクエリ文字列フィールドを通じて渡される`SupplierID`です。

この機能を提供する、イベント ハンドラーを作成、`SendToProducts`ボタンの`Click`イベント。 手順 3 では追加、`SuppliersSelectedIndex`プロパティ、インデックスを返す行のあるラジオ ボタンが選択されています。 対応する`SupplierID`GridView s から取得できる`DataKeys`にコレクションと、ユーザーを送信することができますし、`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`を使用して`Response.Redirect("url")`です。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

このコードは、ラジオ ボタンのいずれかが GridView から選択されている限りすばらしくは動作します。 GridView には、選択すると、任意のオプション ボタンはありません最初に、であり、ユーザーがクリックした場合、`SendToProducts`ボタン、`SuppliersSelectedIndex`なります`-1`、以降にスローされる例外が発生する`-1`が、のインデックスの範囲外`DataKeys`。コレクション。 問題ではありません、ただし、更新を行う場合、`RowCreated`最初に選択した GridView に最初のラジオ ボタンがあるため、手順 3. で説明したように、イベント ハンドラー。

対応する、`SuppliersSelectedIndex`値`-1`GridView 上のページにラベル Web コントロールを追加します。 設定その`ID`プロパティを`ChooseSupplierMsg`、その`CssClass`プロパティを`Warning`、その`EnableViewState`と`Visible`プロパティを`false`、およびその`Text`くださいにプロパティ グリッドから仕入先を選択します。 CSS クラス`Warning`赤、斜体、太字、大きなフォントでテキストを表示しで定義された`Styles.css`です。 設定して、`EnableViewState`と`Visible`プロパティ`false`、以外は、where をポストバックだけのラベルはレンダリングされませんコントロール s`Visible`プロパティ プログラムで`true`です。


[![GridView 上のラベル Web コントロールを追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**図 13**: ラベル Web コントロールの上、GridView の追加 ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


次に、拡張したり、`Click`を表示するイベント ハンドラー、`ChooseSupplierMsg`場合にラベル付け`SuppliersSelectedIndex`未満の場合は 0 は、ユーザーをリダイレクトして`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`それ以外の場合。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

クリックするとブラウザー ページにアクセスして、 `SendToProducts` GridView から仕入先を選択する前にボタンをクリックします。 図 14 では、これを表示、`ChooseSupplierMsg`ラベル。 次に、仕入先を選択し、をクリックして、`SendToProducts`ボタンをクリックします。 選択したサプライヤーから供給される製品を一覧するページにこの whisk はします。 図 15 を示しています、`ProductsForSupplierDetails.aspx`ビッグフット醸造酒および仕入先を選択したときのページです。


[![いいえ供給業者が選択されている場合、ChooseSupplierMsg ラベルが表示されます。](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**図 14**:`ChooseSupplierMsg`いいえ供給業者が選択されている場合、ラベルが表示されます ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![ProductsForSupplierDetails.aspx で、仕入先の選択した製品が表示されます。](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**図 15**: に、選択した業者製品が表示される`ProductsForSupplierDetails.aspx`([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>手順 5: 同じページに、選択したサプライヤー製品を表示します。

手順 4 では、製品を選択した仕入先を表示する別の web ページにユーザーに送信する方法を説明しました。 代わりに、同じページに、選択したサプライヤー製品を表示できます。 これを示すためには、追加する別の GridView`RadioButtonField.aspx`仕入先を選択した s 製品を表示します。

のみを業者は選択した状態を表示する製品のこの GridView、下にあるパネル Web コントロールを追加、 `Suppliers` GridView を設定、`ID`に`ProductsBySupplierPanel`とその`Visible`プロパティを`false`です。 パネル内で選択されている仕入先の製品のテキストを追加後にという名前の GridView`ProductsBySupplier`です。 GridView s のスマート タグからという名前の新しい ObjectDataSource にバインドする選択`ProductsBySupplierDataSource`です。


[![新しい ObjectDataSource ProductsBySupplier GridView にバインドします。](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**図 16**: バインド、`ProductsBySupplier`新しい ObjectDataSource を GridView ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


次に、構成を使用する ObjectDataSource、`ProductsBLL`クラスです。 のみを選択した業者によって提供されるこれらの製品を取得する、ので、ObjectDataSource を呼び出す必要があることを指定、`GetProductsBySupplierID(supplierID)`そのデータを取得します。 (なし) を更新、挿入でのドロップダウン リストから選択し、タブを削除します。


[![ObjectDataSource GetProductsBySupplierID(supplierID) メソッドを使用して構成します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**図 17**: 構成を使用する ObjectDataSource、`GetProductsBySupplierID(supplierID)`メソッド ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![UPDATE、INSERT の (なし) ドロップダウン リストを設定し、タブを削除します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**図 18**: UPDATE、INSERT、および削除のタブで、ドロップダウン リストを (なし) を設定 ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


選択を構成すると、更新を挿入してタブを削除する、[次へ] をクリックします。 以降、`GetProductsBySupplierID(supplierID)`メソッドに入力パラメーターが必要ですが、データ ソースの作成ウィザードの指示に従って、パラメーターの値のソースを指定します。

ある、いくつかのオプションは、ここで、パラメーター値のソースを指定します。 既定のパラメーター オブジェクトを使用し、プログラムによっての値を割り当てる可能性があります、`SuppliersSelectedIndex`プロパティ パラメーター s を`DefaultValue`ObjectDataSource s プロパティ`Selecting`イベント ハンドラー。 戻って、 [ObjectDataSource のパラメーターの値をプログラムによって設定](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)ObjectDataSource のパラメーターに値を割り当てるプログラムでの更新のためのチュートリアルです。

代わりに、いただければ、ControlParameter を使用して参照してください、、 `Suppliers` GridView s [ `SelectedValue`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx)(図 19 を参照してください)。 GridView s`SelectedValue`プロパティから返される、`DataKey`に対応する値、 [ `SelectedIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx)です。 このオプションを使用するためには、必要があります GridView s をプログラムで設定する`SelectedIndex`プロパティを選択した行の場合、`ListProducts`ボタンをクリックします。 設定して、追加のメリットとして、`SelectedIndex`で選択したレコードを実行する、`SelectedRowStyle`で定義されている、`DataWebControls`テーマ (黄色の背景色)。


[![使用して、ControlParameter を指定 GridView の SelectedValue パラメーター ソース](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**図 19**: を使用して、ControlParameter 指定 GridView の SelectedValue パラメーター ソース ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


ウィザードを完了すると、Visual Studio は製品のデータ フィールドのフィールドを自動的に追加されます。 削除以外のすべての`ProductName`、 `CategoryName`、および`UnitPrice`BoundFields、し、変更、`HeaderText`製品カテゴリ、および価格プロパティです。 構成、 `UnitPrice` BoundField その値が通貨として書式設定されるようにします。 これらの変更を加えたら、パネル、ObjectDataSource、GridView、s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

この演習を完了する必要がありますを GridView s を設定する`SelectedIndex`プロパティを`SelectedSuppliersIndex`と`ProductsBySupplierPanel`パネル s`Visible`プロパティを`true`ときに、`ListProducts`ボタンをクリックします。 これを実現する、イベント ハンドラーを作成、`ListProducts`ボタン Web コントロールの`Click`イベントし、次のコードを追加します。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

GridView からサプライヤーが選択されていない場合、`ChooseSupplierMsg`ラベルが表示されます、`ProductsBySupplierPanel`パネルを非表示にします。 それ以外の場合、サプライヤーが選択されている場合、`ProductsBySupplierPanel`が表示されますと GridView の`SelectedIndex`プロパティを更新します。

図 20 ビッグフット醸造酒および仕入先が選択されているし、[ページ] ボタンを表示する製品がクリックしてされた後、結果を示しています。


[![同じページに一覧表示されますビッグフット醸造酒およびによって提供される製品](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**図 20**: 同じページに一覧表示されますビッグフット醸造酒およびによって提供される製品を ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>まとめ

説明したように、[マスター/詳細 DetailView で選択可能なマスター GridView の使用について詳しく説明](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)チュートリアルでは、レコードは、CommandField を使用した GridView から選択することができますが`ShowSelectButton`プロパティに設定されている`true`です。 CommandField、そのボタンが正規のプッシュ ボタン、リンク、またはイメージとして表示されます。 行の代替選択ユーザー インターフェイスは、オプション ボタンまたは GridView 各行のチェック ボックスを提供します。 このチュートリアルでは、ラジオ ボタンの列を追加する方法を確認します。

残念ながら、想像の 1 つは、単純または単純なとしてラジオ ボタンが t の列を追加します。 ボタンのクリックで追加できる組み込みの RadioButtonField はなく、TemplateField 内にある RadioButton Web コントロールを使用して、独自の問題のセットが導入されています。 最後に、このようなインターフェイスを提供することがあるか、カスタムを作成する`DataControlField`クラスまたは中に TemplateField に適切な HTML を挿入するための手段、`RowCreated`イベント。

ラジオ ボタンの列を追加する方法について説明しましたが、チェック ボックスの列を追加することに注目みましょう。 チェック ボックスの列には、ユーザーは 1 つまたは複数の GridView 行を選択し、選択された行 (web ベースの電子メール クライアントからの電子メールの設定を選択し、選択したすべての電子メールを削除する選択) などのすべてのいくつかの操作を実行しています。 次のチュートリアルは、このような列を追加する方法が表示されます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客は、David Suru をでした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](adding-a-gridview-column-of-checkboxes-cs.md)
