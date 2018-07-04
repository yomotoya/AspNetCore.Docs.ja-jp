---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: (C#) のオプション ボタンの GridView 列を追加する |Microsoft Docs
author: rick-anderson
description: このチュートリアルは、ユーザーの 1 つの行を選択した場合の直観的に提供する GridView コントロールをラジオ ボタンの列を追加する方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e1691b3c0c5fb576f25b84e8f4d7125a8d0c698
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366917"
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>(C#) のオプション ボタンの GridView 列を追加します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe)または[PDF のダウンロード](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> このチュートリアルでは、GridView の 1 つの行を選択した場合のより直感的な方法をユーザーに提供する GridView コントロールをラジオ ボタンの列を追加する方法で検索します。


## <a name="introduction"></a>はじめに

GridView コントロールには、多くの組み込み機能が用意されています。 これには、さまざまなテキスト、イメージ、ハイパーリンク、およびボタンを表示するためのさまざまなフィールドが含まれます。 カスタマイズのためのテンプレートがサポートしています。 マウスの数回のクリックで、各行をボタンのクリックで選択できる GridView を作成または編集または削除の機能を有効にすることができます。 提供される機能の多くに関係なく多くの場合、場合がありますを追加する必要がありますがサポートされていない機能に追加します。 このチュートリアルで次の 2 つ追加機能を組み込むに GridView の機能を拡張する方法を考察します。

このチュートリアルと次のセクションは、列の選択プロセスの向上にフォーカスします。 調べると、[マスター/詳細の選択可能なマスター GridView を使用すると詳細 DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)、Select ボタンが含まれる GridView に、[commandfield] を追加できます。 クリックすると、ポストバックに陥りますと GridView の`SelectedIndex`プロパティを持つ Select ボタンがクリックされた行のインデックスが更新されます。 [マスター/詳細の選択可能なマスター GridView を使用すると詳細 DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)チュートリアルでは、選択した GridView 行の詳細を表示するこの機能を使用する方法を説明しました。

多くの状況で動作しますが、[選択] ボタン、いないも他の操作可能性があります。 ボタンを使用してではなく他の 2 つのユーザー インターフェイス要素でよく使われる選択: オプション ボタンおよびチェック ボックスをオンします。 ラジオ ボタンまたはチェック ボックスを [選択] ボタンでは、代わりに各行に含まれるようは GridView の強化点ことができます。 ユーザー選択ができますのみ GridView レコードの 1 つのシナリオでラジオ ボタンもあります優先ボタンを選択します。 ユーザーが選択できる可能性がある状況では、web ベースの電子メール アプリケーションでは、チェック ボックスを削除する複数のメッセージを選択するユーザーが先など、複数のレコードから、[選択] ボタンまたはラジオ ボタンが利用できない機能を提供していますユーザー インターフェイス。

このチュートリアルでは、GridView にラジオ ボタンの列を追加する方法を検索します。 先に進むのチュートリアルでは、チェック ボックスを使用してについて説明します。

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>手順 1: 拡張 GridView Web ページを作成します。

ラジオ ボタンの列を含めるように GridView を拡張する入る前に、このチュートリアルと次の 2 つの必要がありますが、web サイト プロジェクトで ASP.NET ページを作成する最初に少し s を使用できます。 という名前の新しいフォルダーを追加することで開始`EnhancedGridView`します。 次に、次の ASP.NET ページを使用する各ページに関連付けることを確認、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![SqlDataSource に関連するチュートリアルについては、ASP.NET ページに追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**図 1**: SqlDataSource に関連するチュートリアルについては、ASP.NET ページに追加します。


などの他のフォルダーで`Default.aspx`で、`EnhancedGridView`フォルダーは、チュートリアルのセクションで一覧表示します。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))。


最後に、これら 4 つのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、次のマークアップを追加、使用後、SqlDataSource コントロール`<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューには、編集、挿入、および削除のチュートリアルの項目が含まれています。


![サイト マップ、GridView のチュートリアルを強化するためのエントリになりました](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**図 3**: サイト マップ、GridView のチュートリアルを強化するためのエントリになりました


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>手順 2: GridView で仕入先を表示します。

このチュートリアルでは、s を GridView の各行ラジオ ボタンを提供すると、米国からサプライヤーを一覧表示する GridView を構築することができます。 ラジオ ボタンを使用して仕入先を選択すると、ユーザーがボタンをクリックして仕入先、製品を表示できます。 このタスクを簡単に聞こえるかもしれませんは、さまざまなことを特に厄介な細部があります。 これらの細部にこれについては、前にまず仕入先の一覧を表示する GridView s を使用できます。

開いて開始、`RadioButtonField.aspx`ページで、 `EnhancedGridView` GridView をツールボックスからデザイナーにドラッグしてフォルダー。 GridView s 設定`ID`に`Suppliers`と、新しいデータ ソースを作成することも、スマート タグから。 具体的には、作成するという、ObjectDataSource`SuppliersDataSource`からデータを抽出する、`SuppliersBLL`オブジェクト。


[![SuppliersDataSource という名前の新しい ObjectDataSource を作成します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**図 4**: 名前付き新しい ObjectDataSource 作成`SuppliersDataSource`([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))。


[![SuppliersBLL クラスを使用する ObjectDataSource を構成します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**図 5**: 構成に使用する ObjectDataSource、`SuppliersBLL`クラス ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))。


米国内で業者を一覧表示するのみ、選択、`GetSuppliersByCountry(country)`選択 タブで、ドロップダウン リストからメソッド。


[![SuppliersBLL クラスを使用する ObjectDataSource を構成します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**図 6**: 構成に使用する ObjectDataSource、`SuppliersBLL`クラス ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))。


更新プログラム] タブで、[オプションし、[次へ] (なし)。


[![SuppliersBLL クラスを使用する ObjectDataSource を構成します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**図 7**: 構成に使用する ObjectDataSource、`SuppliersBLL`クラス ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))。


以降、`GetSuppliersByCountry(country)`メソッドが受け取るパラメーターは、データ ソースの構成ウィザードでは、私たちを入力パラメーターのソース。 指定するには、(USA, この例では)、ハード コーディングされた値には、ソースのドロップダウン リストを None に設定し、テキスト ボックスで、既定値を入力パラメーターがままにします。 ウィザードを完了するには、[完了] をクリックします。


[![既定値として (米国) を使用して、お住まいの国のパラメーター](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**図 8**: の既定値として使用 (米国)、`country`パラメーター ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))。


ウィザードを完了すると、各仕入先のデータ フィールドの BoundField が GridView に含まれます。 削除以外のすべての`CompanyName`、 `City`、および`Country`BoundFields、名前を変更し、 `CompanyName` BoundFields`HeaderText`業者にプロパティ。 その後、GridView コントロールと ObjectDataSource 宣言の構文は次のようになります。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

このチュートリアルでは、s の仕入先の一覧と同じページで、または別のページで選択したサプライヤーを表示する製品を許可することができます。 これに合わせて、2 つのボタンの Web コントロールをページに追加します。 Ve セット、`ID`これら 2 つのボタンの`ListProducts`と`SendToProducts`、アイデアに`ListProducts`がクリックされたポストバックが発生し、選択した供給業者の製品が、同じページに表示されます`SendToProducts`がクリックされました。ユーザーは、製品を一覧表示する別のページに示されているされます。

図 9 は、 `Suppliers` GridView コントロールとボタンの 2 つの Web ブラウザーで表示した場合を制御します。


[![米国から業者がある、名前、City、および国の情報を一覧表示](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**図 9**: その名前のある (米国)、City、および国の情報が表示されてからサプライヤーもの ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))。


## <a name="step-3-adding-a-column-of-radio-buttons"></a>手順 3: ラジオ ボタンの列を追加します。

この時点で、 `Suppliers` GridView が 3 つの BoundFields、米国の会社名、市区町村、および各仕入先の国を表示します。 ラジオのボタンの列は、ただし、まだありませんが。 残念ながら、GridView されないには、組み込み RadioButtonField、それ以外の場合、グリッドを追加すれば済みますし実行が含まれます。 TemplateField を追加し、構成代わりに、その`ItemTemplate`GridView 各行ラジオ ボタンで、ラジオ ボタンの表示にします。

最初に、可能性がありますと仮定する RadioButton Web コントロールを追加することで、目的のユーザー インターフェイスを実装することができます、 `ItemTemplate` TemplateField の。 GridView の行ごとに 1 つのオプション ボタンを追加これが実際には、ラジオ ボタン グループ化することはできませんし、したがって相互に排他的ではありません。 これは、エンドユーザーは GridView から同時に複数のオプション ボタンを選択することがあります。

Let s がので、このアプローチを実装する場合でも、RadioButton Web コントロールの TemplateField を使用する必要がある機能は提供しません、結果として得られるラジオ ボタンのグループ分けされていない理由を検討する価値はあるでしょう。 まず、左端のフィールドになります、サプライヤーの GridView を TemplateField を追加します。 次に、GridView s のスマート タグからテンプレートの編集リンクをクリックし、TemplateField s に RadioButton Web コントロールをツールボックスからドラッグ`ItemTemplate`(図 10 参照)。 RadioButton s 設定`ID`プロパティを`RowSelector`と`GroupName`プロパティを`SuppliersGroup`します。


[![ItemTemplate に RadioButton Web コントロールを追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**図 10**: する RadioButton Web コントロールを追加、 `ItemTemplate` ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))。


デザイナーを通じてこれらの追加を行った後、GridView のマークアップを次のようになります。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

RadioButton s [ `GroupName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx)は一連のラジオ ボタンをグループ化に使われます。 すべての RadioButton コントロールを同じ`GroupName`値では、グループ化と見なされます、一度に 1 つだけのラジオ ボタン グループから選択できます。 `GroupName`プロパティが表示されるオプション ボタン s の値を指定`name`属性。 ブラウザーがラジオ ボタンを調べて`name`無線を決定する属性のボタンのグループ化します。

追加された RadioButton Web コントロールで、`ItemTemplate`ブラウザーからこのページを参照してください。 し、グリッドの行のラジオ ボタンをクリックします。 通知がないラジオ ボタンをグループ化方法、図 11 としてすべての行を選択することを示しています。


[![GridView s のラジオ ボタンがグループ化されていません。](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**図 11**: s ラジオ ボタンの GridView がグループ化されていない ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))。


ラジオ ボタンのグループ分けされていない理由は、レンダリングされた`name`属性が異なる場合であっても、同じ`GroupName`プロパティの設定。 これらの違いを表示するには、ブラウザーから/ソースの表示を実行し、ラジオ ボタンのマークアップを確認します。


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

両方の通知、`name`と`id`属性は、[プロパティ] ウィンドウで指定された正確な値ではありませんが、その他の数が付いています`ID`値。 追加`ID`、レンダリングの前に追加された値`id`と`name`属性は、`ID`ラジオのボタン コントロールの親、 `GridViewRow` s `ID` s、GridView の`ID`、コンテンツ コントロール s `ID`、および Web フォームの`ID`します。 これら`ID`s は追加できるように各レンダリング Web コントロール、GridView では、一意`id`と`name`値。

各レンダリング制御のニーズが異なる`name`と`id`これは、どのブラウザー、クライアント側とそれを識別する方法、web サーバーにどのようなアクションでは、各コントロールを一意に識別するまたは変更がポストバック時に発生したためです。 たとえば、RadioButton s チェックの状態が変更されたときに、いくつかのサーバー側コードを実行したいとします。 RadioButton s を設定して、これを実現しましたでした`AutoPostBack`プロパティを`true`のイベント ハンドラーを作成して、`CheckChanged`イベント。 ただし場合、レンダリングされた`name`と`id`値のすべてのラジオ ボタンは同じでポストバックを具体的にどのを特定できませんでしたオプション ボタンがクリックしてされました。

これの短い形式は、RadioButton Web コントロールを使用して GridView にラジオ ボタンの列を作成できないことです。 代わりではなく旧式の手法を使用して GridView 行ごとに適切なマークアップを挿入するようにする必要があります。

> [!NOTE]
> RadioButton Web コントロール、HTML コントロール、テンプレートに追加されたときにオプション ボタンが、一意では、よう`name`属性をグループに属していないグリッドのオプション ボタンを作成します。 HTML コントロールに慣れていない場合は、HTML コントロールはほとんど使用されると、ASP.NET 2.0 では特に、この注を無視する自由ください。 詳細に関心がある場合は、次を参照してください。 ただし[K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s ブログ エントリ[Web コントロールと HTML コントロール](http://www.odetocode.com/Articles/348.aspx)します。


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>リテラル コントロールを使用して、ラジオ ボタンのマークアップを挿入するには

正しく GridView 内のラジオ ボタンのすべてをグループ化、必要がありますにラジオ ボタンのマークアップを手動で挿入する、`ItemTemplate`します。 各オプション ボタンが必要な同じ`name`属性しますが、一意である必要があります`id`属性 (クライアント側スクリプト経由でのラジオ ボタンにアクセスする) 場合。 ブラウザーから戻る s 選択したラジオ ボタンの値が送信は、ユーザーがラジオ ボタンを選択すると、投稿、ページのバックアップ、`value`属性。 したがって、各オプション ボタンが一意必要`value`属性。 最後に、ポストバック必要がありますを追加することを確認する、`checked`属性を選択すると、それ以外の場合、ユーザーが選択され、バックアップの投稿を 1 つのラジオ ボタン、オプション ボタンに戻ります、既定の状態 (すべて選択されていない)。

テンプレートに低レベルのマークアップを挿入するために実行できる 2 つの方法はあります。 さまざまなマークアップおよび分離コード クラスで定義されているメソッドの書式設定への呼び出しを行う 1 つです。 この手法にで説明した、 [GridView コントロールで TemplateFields を使用して](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)チュートリアル。 ここでのように表示可能性があります。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

ここでは、`GetUniqueRadioButton`と`GetRadioButtonValue`返さ、適切な分離コード クラスで定義されているメソッドになります`id`と`value`属性の各オプション ボタンの値。 このアプローチで割り当てることは、`id`と`value`属性しますが、指定する必要がある場合は、`checked`データ バインディング構文はデータが GridView にバインドしておく場合にのみ実行されるため、属性値。 そのため、GridView のビュー ステートを有効になっている場合は、書式指定メソッドだけ起動ときに、ページが最初に読み込まれた (または、データ ソースに GridView が明示的に再バインドすると) と関数を設定するため、`checked`で t が勝利した属性は呼び出せませんポストバックします。 これはかなり分かりにくい問題と少しため、このスクリプトの選択は、この記事の範囲外です。 ただし、上記の方法を使用することお勧めしますする行き詰まったをポイントする作業を通じてはしないでください。 このような演習が勝利した t を取得すると、作業バージョンをそれ以上近づく、GridView とデータ バインドのライフ サイクルの理解を深めを促進するのに役立ちます。

追加するカスタムを挿入することを他のアプローチをテンプレートとこのチュートリアルで使用するアプローチの低レベルのマークアップは、[リテラル コントロール](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx)テンプレートにします。 その後、GridView s `RowCreated`または`RowDataBound`イベント ハンドラーでは、リテラル コントロールをプログラムでアクセスできるとその`Text`プロパティを出力する、マークアップに設定します。

TemplateField s から、オプション ボタンを削除することによって開始`ItemTemplate`、リテラル コントロールに置き換えます。 リテラル コントロール s 設定`ID`に`RadioButtonMarkup`します。


[![リテラル コントロール、ItemTemplate を追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**図 12**: するリテラル コントロールを追加、 `ItemTemplate` ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))。


次に、GridView s のイベント ハンドラーを作成`RowCreated`イベント。 `RowCreated`イベントは、すべての行が追加されるかどうか、データがされているバインド GridView に 1 回だけを起動します。 つまり、ポストバックであっても、データのビュー状態から再読み込み時、`RowCreated`イベントが発生し、これは、代わりに使用している理由`RowDataBound`(起動されるのみと、データは、明示的にバインドするデータ Web コントロール)。

このイベント ハンドラーでのみ続行する場合データ行をします。 プログラムで参照するデータ行ごとに、`RadioButtonMarkup`リテラル コントロールとその`Text`プロパティを出力するマークアップをします。 次のコードは、出力されるマークアップを作成、ラジオ ボタンを持つ`name`属性に設定されて`SuppliersGroup`が`id`属性に設定されて`RowSelectorX`ここで、 *X* GridView の行のインデックスは、その`value`属性が GridView の行のインデックスに設定します。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

GridView の行を選択し、ポストバックの発生とここでは、`SupplierID`仕入先を選択したのです。 各オプション ボタンの値が、実際はそのため、考える`SupplierID`(GridView の行のインデックス) ではありません。 これは、特定の状況で機能が、中に盲目的にそのまま使用し、処理、セキュリティ リスクことが、`SupplierID`します。 たとえば、GridView には、業者、米国内のみが一覧表示します。 ただし場合、`SupplierID`ラジオ ボタン、有害のユーザーから、操作を停止するには、どのような秒から直接渡される、`SupplierID`値がポストバックで送り返されたでしょうか。 として行のインデックスを使用して、 `value`、して、`SupplierID`からのポストバック時に、`DataKeys`コレクションできるように、ユーザーのみを使用しているいずれかの`SupplierID`GridView 行の 1 つに関連付けられた値。

このイベント ハンドラーのコードを追加した後に、ブラウザーでページをテストする少し時間がかかります。 最初に、その 1 つだけのラジオに注意してください、グリッド内のボタンを一度に選択できます。 ただし、ラジオ ボタンを選択し、ボタンのいずれかをクリックして、ポストバックが発生したし、の初期状態にすべてのラジオ ボタンを元に戻す (つまり、ポストバックで選択したオプション ボタンは選択解除されます)。 これを解決する必要がありますを増やすために、`RowCreated`ことは、ポストバックから送信された選択したラジオ ボタンのインデックスを検査し、追加するためのイベント ハンドラー、`checked="checked"`属性の行のインデックスと一致する生成されたマークアップをします。

ポストバックが発生すると、ブラウザーが返送、`name`と`value`の選択したラジオ ボタン。 値はプログラムで使用して取得`Request.Form["name"]`します。 [ `Request.Form`プロパティ](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx)提供、 [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx)フォーム変数を表します。 フォーム変数は名前と、web ページのフォーム フィールドの値と、ポストバックに陥りますたびに、web ブラウザーによって送信されます。 レンダリングされた`name`GridView のラジオ ボタンの属性が`SuppliersGroup`web ページに戻る、ブラウザーが送信されますが投稿されたときに、 `SuppliersGroup=valueOfSelectedRadioButton` web サーバー (およびその他のフォーム フィールド) にします。 この情報にアクセスすることができますし、`Request.Form`プロパティを使用して:`Request.Form["SuppliersGroup"]`します。

以降必要がありますを決定する、選択したラジオ ボタン インデックスが作成されますだけではなく、`RowCreated`ですが、イベント ハンドラー、 `Click` Button Web コントロールのイベント ハンドラー、let s の追加、`SuppliersSelectedIndex`プロパティを返す分離コードクラスを`-1`ラジオ ボタンが選択されていない場合、ラジオ ボタンのいずれかが選択されている場合は、選択されたインデックス。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

追加するには、このプロパティを追加すると、わかって、`checked="checked"`内のマークアップ、`RowCreated`イベント ハンドラーと`SuppliersSelectedIndex`equals`e.Row.RowIndex`します。 このロジックを含めるイベント ハンドラーを更新します。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

この変更は、選択したオプション ボタンは、ポストバック後に選択された状態。 ページが最初にアクセスすると、最初の GridView 行 s のラジオ ボタンが選択されました (ではなく現在これは既定で選択したラジオ ボタンがないように動作を変更ラジオ ボタンが選択されているかを指定する機能が作成できた可能性があります。動作)。 既定で選択されている最初のラジオ ボタンを表示するには、単に変更、 `if (SuppliersSelectedIndex == e.Row.RowIndex)` 、次のステートメント:`if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`します。

この時点でグループ化されたラジオ ボタンの列を選択し、ポストバック間での記憶の 1 つの GridView 行では、GridView に追加されました。 次のステップでは、選択した業者によって提供される製品を表示します。 手順 4. で方法を見ていきます別のページにユーザーをリダイレクトする、選択したに沿って送信`SupplierID`します。 手順 5 では、同じページに GridView で選択した仕入先、製品を表示する方法がわかります。

> [!NOTE]
> TemplateField (この時間のかかる手順 3 のフォーカス設定) を使用してではなくカスタムを作成できます`DataControlField`適切なユーザー インターフェイスと機能をレンダリングするクラス。 [ `DataControlField`クラス](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx)BoundField、CheckBoxField、TemplateField、およびその他の組み込みの GridView および DetailsView フィールドが派生元となる基本クラスです。 カスタムを作成`DataControlField`クラスをラジオ ボタンの列がだけ宣言の構文を使用して追加でしたし、その他の web ページに、他の web アプリケーションを大幅に簡素化機能をレプリケートすることも意味します。


ASP.NET のコントロールをコンパイルすると、これまで、カスタムで作成した場合ただしを行うのため、かなりの作業が必要ですとわかってを伴っての細部とのエッジ ケースを慎重に処理する必要があります。 そのため、私たちは断念としてカスタムのラジオ ボタンの列を実装する`DataControlField`ここではクラス、および TemplateField オプションを使用します。 作成、使用、およびカスタムの展開を調査する機会がおそらく`DataControlField`今後のチュートリアル内のクラスです。

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>手順 4: 別のページで、選択したサプライヤー製品を表示します。

ユーザーが GridView の行を選択した後、仕入先を選択した製品を表示する必要があります。 同じページをお勧めが他のユーザーに、別のページで、これらの製品を表示したい状況によっては、可能性があります。 最初に、別のページで、製品を表示する方法を調べて s を使用します。手順 5 でに GridView を追加することに注目します`RadioButtonField.aspx`仕入先の選択した製品を表示します。

現在はページ上の 2 つのボタンの Web コントロール`ListProducts`と`SendToProducts`します。 ときに、`SendToProducts`ボタンがクリックされると、ユーザーに送信する`~/Filtering/ProductsForSupplierDetails.aspx`します。 このページで作成されて、[マスター/詳細フィルター間で 2 つのページ](../masterdetail/master-detail-filtering-across-two-pages-cs.md)チュートリアルと、その業者の製品が表示されますが`SupplierID`という名前のクエリ文字列フィールドを通じて渡される`SupplierID`します。

この機能を提供するのイベント ハンドラーを作成、`SendToProducts`ボタンの`Click`イベント。 手順 3 で追加されました、`SuppliersSelectedIndex`プロパティを持つラジオ ボタンの行のインデックスを返しますが選択されています。 対応する`SupplierID`GridView s から取得できる`DataKeys`にコレクションとユーザーを送信することができますし、`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`を使用して`Response.Redirect("url")`します。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

このコードは、GridView からいずれかのラジオ ボタンが選択されている限り、すばらしくは動作します。 最初に、GridView に選択すると、ラジオ ボタンがないし、ユーザーがクリックした場合は、`SendToProducts`ボタン、`SuppliersSelectedIndex`なります`-1`、以降にスローされる例外が発生する`-1`のインデックスの範囲外です`DataKeys`コレクション。 問題ではありません、ただし、更新する場合は、`RowCreated`最初に選択した GridView に最初のラジオ ボタンがあるため、手順 3. で説明したように、イベント ハンドラー。

対応するために、`SuppliersSelectedIndex`の値`-1`、Label Web コントロール、GridView 上にあるページを追加します。 設定の`ID`プロパティを`ChooseSupplierMsg`その`CssClass`プロパティを`Warning`その`EnableViewState`と`Visible`プロパティを`false`とその`Text`くださいにプロパティ グリッドから、業者を選択します。 CSS クラス`Warning`赤、斜体、太字、大きなフォントでテキストを表示しで定義されている`Styles.css`します。 設定して、`EnableViewState`と`Visible`プロパティ`false`、以外は、where をポストバックだけのラベルは表示されませんコントロール s`Visible`プロパティ プログラムで`true`します。


[![GridView の上のラベル Web コントロールを追加します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**図 13**: ラベル Web コントロールの上、GridView の追加 ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))。


次に、拡張、`Click`を表示するイベント ハンドラー、`ChooseSupplierMsg`場合にラベル付け`SuppliersSelectedIndex`がより小さい 0、およびユーザーをリダイレクト`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`それ以外の場合。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

クリックするとブラウザー ページを参照してください、 `SendToProducts` GridView から仕入先を選択する前にボタンをクリックします。 図 14 に示す、これを表示、`ChooseSupplierMsg`ラベル。 次に、仕入先を選択し、クリックして、`SendToProducts`ボタンをクリックします。 選択したサプライヤーから供給される製品を一覧表示されたページにするを whisk これは。 図 15 を示しています、`ProductsForSupplierDetails.aspx`ビッグフット醸造酒および仕入先を選択したときのページします。


[![No 仕入先が選択されている場合、ChooseSupplierMsg ラベルが表示されます。](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**図 14**:`ChooseSupplierMsg`いいえ仕入先が選択されている場合、ラベルが表示されます ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))。


[![ProductsForSupplierDetails.aspx、仕入先の選択した製品が表示されます。](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**図 15**: に、選択した供給業者の製品が表示される`ProductsForSupplierDetails.aspx`([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))。


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>手順 5: 同じページで、選択したサプライヤー製品を表示します。

手順 4 では、製品を選択したサプライヤーを表示する別の web ページにユーザーに送信する方法を説明しました。 または、同じページで、選択したサプライヤー製品を表示できます。 これを示すためには、別の GridView に追加します`RadioButtonField.aspx`仕入先の選択した製品を表示します。

のみたいので、業者を選択したらを表示する製品のこの GridView、下のパネルの Web コントロールを追加、 `Suppliers` GridView、設定、`ID`に`ProductsBySupplierPanel`とその`Visible`プロパティを`false`します。 GridView という名前で、パネル内で選択されている仕入先の製品のテキストを追加後に`ProductsBySupplier`します。 という名前の新しい ObjectDataSource にバインドを選択する GridView s のスマート タグから`ProductsBySupplierDataSource`します。


[![新しい ObjectDataSource ProductsBySupplier GridView にバインドします。](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**図 16**: バインド、`ProductsBySupplier`新しい ObjectDataSource に GridView ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))。


使用する ObjectDataSource を次に、構成、`ProductsBLL`クラス。 のみたいので、選択した業者によって提供されるこれらの製品を取得する、ObjectDataSource を呼び出す必要があることを指定、`GetProductsBySupplierID(supplierID)`そのデータを取得します。 (なし) を UPDATE、INSERT でドロップダウン リストから選択し、タブを削除します。


[![ObjectDataSource GetProductsBySupplierID(supplierID) メソッドを使用して構成します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**図 17**: 構成に使用する ObjectDataSource、`GetProductsBySupplierID(supplierID)`メソッド ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))。


[![UPDATE、INSERT で (なし) ドロップダウン リストを設定し、タブを削除します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**図 18**: UPDATE、INSERT、および削除のタブで、ドロップダウン リストを [(なし) を設定 ([フルサイズの画像を表示する] をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))。


選択を構成した後は、更新、挿入しタブを削除する、[次へ] をクリックします。 以降、`GetProductsBySupplierID(supplierID)`メソッドに入力パラメーターが必要ですが、データ ソースの作成ウィザードでは、ソース、パラメーターの値を指定するように求められます。

ここで、パラメーターの値のソースを指定するオプションのいくつかあります。 既定のパラメーター オブジェクトを使用してプログラムでの値を割り当てることが、`SuppliersSelectedIndex`プロパティ パラメーター s を`DefaultValue`ObjectDataSource s プロパティ`Selecting`イベント ハンドラー。 参照、 [ObjectDataSource のパラメーター値をプログラムによって設定](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)ObjectDataSource のパラメーターに値を割り当てるプログラムでの更新機能のチュートリアル。

または、ControlParameter を使用して参照してください、、 `Suppliers` GridView s [ `SelectedValue`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx)(図 19 を参照してください)。 GridView s`SelectedValue`プロパティが返す、`DataKey`に対応する値、 [ `SelectedIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx)します。 このオプションを使用するためには、GridView s をプログラムで設定する`SelectedIndex`プロパティに、選択した行の場合に、`ListProducts`ボタンがクリックされました。 設定して、追加のメリットとして、 `SelectedIndex`、選択したレコードになる予定、`SelectedRowStyle`で定義されている、`DataWebControls`テーマ (黄色の背景)。


[![パラメーターのソースとして GridView の SelectedValue を指定するのに、ControlParameter を使用します。](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**図 19**: パラメーターのソースとして GridView の SelectedValue を指定する、ControlParameter を使用して ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))。


ウィザードを完了すると、Visual Studio は、製品データ フィールドのフィールドを自動的に追加するは。 削除以外のすべての`ProductName`、 `CategoryName`、および`UnitPrice`BoundFields、し、変更、`HeaderText`製品カテゴリ、および価格プロパティ。 構成、 `UnitPrice` BoundField、値が通貨として書式設定されるようにします。 これらの変更を行った後パネル、GridView、および ObjectDataSource s 宣言型マークアップは、次のようになります。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

この演習を完了する必要があります GridView s を設定する`SelectedIndex`プロパティを`SelectedSuppliersIndex`と`ProductsBySupplierPanel`パネル s`Visible`プロパティを`true`ときに、`ListProducts`ボタンをクリックします。 これを実現するには、イベント ハンドラーを作成、`ListProducts`ボタン Web コントロールの`Click`イベントし、次のコードを追加します。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

サプライヤーは、GridView から選択されていない場合、`ChooseSupplierMsg`ラベルが表示されます、`ProductsBySupplierPanel`パネルを非表示になります。 それ以外の場合、仕入先が選択されている場合、`ProductsBySupplierPanel`が表示されますおよび GridView の`SelectedIndex`プロパティが更新されます。

図 20 ビッグフット醸造酒および仕入先が選択されているし、[ページ] ボタンを表示する製品がクリックしてされた後、結果を示しています。


[![同じページには、ビッグフット醸造酒およびによって提供される製品が一覧表示されます。](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**図 20**: 同じページに一覧表示されますビッグフット醸造酒およびによって提供される製品を ([フルサイズの画像を表示する をクリックします](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))。


## <a name="summary"></a>まとめ

説明したように、[マスター/詳細 DetailView で選択可能なマスター GridView の使用について詳しく説明](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)チュートリアルでは、レコードは、[commandfield] を使用して GridView から選択できますが`ShowSelectButton`プロパティに設定されて`true`します。 [Commandfield] ボタンを標準プッシュ ボタン、リンク、またはイメージとして表示されます。 代替行選択ユーザー インターフェイスは、オプション ボタンや GridView 行ごとにチェック ボックスを提供します。 このチュートリアルでは、ラジオ ボタンの列を追加する方法について確認しました。

残念ながら、期待されるとしては、単純または単純なラジオ ボタンは t の列を追加します。 ボタンのクリックで追加できる組み込みの RadioButtonField はなく、TemplateField 内 RadioButton Web コントロールを使用して、独自の問題のセットが導入されています。 最後に、このようなインターフェイスを提供することがあるかカスタムを作成する`DataControlField`クラスまたは中に TemplateField に適切な HTML を挿入するための手段、`RowCreated`イベント。

ラジオ ボタンの列を追加する方法を説明しましたが、チェック ボックスの列を追加することに注目みましょう。 チェック ボックスの列には、ユーザーは 1 つまたは複数の GridView 行を選択し、(web ベースの電子メール クライアントでは、電子メールのセットを選択し、選択したすべての電子メールを削除する選択) など、選択した行のすべてのいくつかの操作を実行します。 次のチュートリアルでは、このような列を追加する方法がわかります。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、David Suru でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](adding-a-gridview-column-of-checkboxes-cs.md)
