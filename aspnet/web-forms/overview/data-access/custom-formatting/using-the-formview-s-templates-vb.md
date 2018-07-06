---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: FormView のテンプレート (VB) を使用して |Microsoft Docs
author: rick-anderson
description: DetailsView とは異なり、FormView いないフィールドで構成されます。 代わりに、テンプレートを使用して、フォーム ビューがレンダリングされます。 このチュートリアルでは、F. の使用について説明します.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: bb613158343e6845e583ed823e2a2c16f958b720
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838155"
---
<a name="using-the-formviews-templates-vb"></a>FormView のテンプレート (VB) を使用します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe)または[PDF のダウンロード](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> DetailsView とは異なり、FormView いないフィールドで構成されます。 代わりに、テンプレートを使用して、フォーム ビューがレンダリングされます。 取り上げるこのチュートリアルでは、FormView コントロールを使用して、データの小さい固定表示を表示します。


## <a name="introduction"></a>はじめに

最後の 2 つのチュートリアルでは、カスタマイズ TemplateFields を使用する GridView および DetailsView コントロールの出力方法を説明しました。 TemplateFields を高度にカスタマイズするには、特定のフィールドの内容を許可するが、最終的に、GridView と DetailsView が角張ったではなく、グリッドのような外観。 多くのシナリオは、このようなグリッドのようなレイアウトに最適ですより滑らかな、小さい剛体の表示が必要な時です。 1 つのレコードを表示するときに、このような滑らかなレイアウトは、FormView コントロールを使用する可能性があります。

DetailsView とは異なり、FormView いないフィールドで構成されます。 FormView に BoundField または TemplateField を追加することはできません。 代わりに、テンプレートを使用して、フォーム ビューがレンダリングされます。 含む 1 つ TemplateField DetailsView コントロールと FormView を考えます。 FormView では、次のテンプレートがサポートされています。

- `ItemTemplate` FormView で表示される特定のレコードを表示するために使用
- `HeaderTemplate` 省略可能なヘッダー行を指定するために使用
- `FooterTemplate` 省略可能なフッター行を指定するために使用
- `EmptyDataTemplate` FormView の`DataSource`任意のレコードがない、`EmptyDataTemplate`の代わりに使用されて、`ItemTemplate`コントロールのマークアップをレンダリングするため
- `PagerTemplate` ページングが有効になっている FormViews のページング インターフェイスをカスタマイズするために使用できます。
- `EditItemTemplate` / `InsertItemTemplate` このような機能をサポートする FormViews の編集インターフェイスまたは挿入のインターフェイスをカスタマイズするために使用

取り上げるこのチュートリアルでは、FormView コントロールを使用して、製品の小さい固定表示を表示します。 名前、カテゴリ、供給業者とに、フォーム ビューのフィールドではなく`ItemTemplate`ヘッダー要素の組み合わせを使用してこれらの値が表示されます、 `<table>` (図 1 参照)。


[![FormView は、DetailsView でグリッドのようなレイアウトのブレーク アウト](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**図 1**: DetailsView で Grid-Like レイアウト表示から、FormView が中断されます ([フルサイズの画像を表示する をクリックします](using-the-formview-s-templates-vb/_static/image3.png))。


## <a name="step-1-binding-the-data-to-the-formview"></a>手順 1: データをフォーム ビューにバインドします。

開く、`FormView.aspx`ページし、FormView をツールボックスからデザイナーにドラッグします。 まず、フォーム ビューを追加するときに私たちに指示する灰色のボックスとして表示される`ItemTemplate`が必要です。


[![ItemTemplate が提供されるまで、デザイナーで、フォーム ビューをレンダリングできません。](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**図 2**: デザイナーまででレンダリング、FormView できません、`ItemTemplate`提供されます ([フルサイズの画像を表示する をクリックします](using-the-formview-s-templates-vb/_static/image6.png))。


`ItemTemplate` (宣言構文) を手動で作成することができますか、フォーム ビューをデザイナーによってデータ ソース コントロールにバインドすることによって自動作成をすることができます。 この自動作成`ItemTemplate`HTML、リストの各フィールドとラベルの名前を制御が含まれている`Text`プロパティ フィールドの値にバインドします。 このアプローチも自動で作成、`InsertItemTemplate`と`EditItemTemplate`、どちらもはの各データ ソース コントロールによって返されるデータ フィールドの入力コントロールに設定されます。

追加を呼び出す新しい ObjectDataSource コントロールをフォーム ビューのスマート タグからのテンプレートを自動作成する場合、`ProductsBLL`クラスの`GetProducts()`メソッド。 これをフォーム ビューが作成されます、 `ItemTemplate`、 `InsertItemTemplate`、および`EditItemTemplate`します。 ソース ビューから削除、`InsertItemTemplate`と`EditItemTemplate`に興味がないためには、編集、またはまだ挿入をサポートする、FormView を作成します。 次に、内のマークアップをクリアします、`ItemTemplate`から作業をクリーンな状態があるようにします。

はなく構築する場合、 `ItemTemplate` 、手動で追加し、ツールボックスからデザイナーにドラッグすることによって、ObjectDataSource を構成します。 ただし、しないと、デザイナーから、フォーム ビューのデータ ソースを設定します。 代わりに、ソース ビューに移動し、手動で設定する FormView の`DataSourceID`プロパティを`ID`ObjectDataSource の値。 次に、手動で追加、`ItemTemplate`します。

どのようなアプローチに関係なくは、するために、この時点で、フォーム ビューの宣言型マークアップを決定しました。


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

FormView のスマート タグでページングを有効にするチェック ボックスをオンする少しこれは追加、 `AllowPaging="True"` FormView の宣言型構文に属性します。 また、設定、`EnableViewState`プロパティを False にします。

## <a name="step-2-defining-theitemtemplates-markup"></a>手順 2: 定義、`ItemTemplate`のマークアップ

ページングをサポートする FormView ObjectDataSource コントロールにバインドされ、構成されていると私たちのコンテンツを指定する準備ができている、`ItemTemplate`します。 このチュートリアルに表示される製品の名前を持つみましょう、`<h3>`見出し。 次に、HTML を使用してみましょう`<table>`最初と 3 番目の列には、プロパティ名が一覧表示され、2 番目と 4 番目の値の一覧の 4 つの列テーブルに製品の残りのプロパティを表示します。

このマークアップは、デザイナーで FormView のテンプレートの編集インターフェイス経由で入力または宣言の構文を手動で入力できます。 テンプレートを使用する場合は通常方が宣言の構文を直接操作しますが、自由に最も使い慣れている手法を使用する方が手軽です。

次のマークアップの後のフォーム ビューの宣言型マークアップを示しています、`ItemTemplate`の構造が完了します。


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

注意してデータ バインディング構文 -`<%# Eval("ProductName") %>`例は、テンプレートの出力に直接挿入することができます。 つまり、その必要がありますに割り当てられませんラベル コントロールの`Text`プロパティ。 たとえば、ある、`ProductName`に表示される値、`<h3>`要素を使用して`<h3><%# Eval("ProductName") %></h3>`、製品の Chai としてレンダーする`<h3>Chai</h3>`します。

`ProductPropertyLabel`と`ProductPropertyValue`製品プロパティの名前と値のスタイルを指定するための CSS クラス、`<table>`します。 これらの CSS クラスが定義されている`Styles.css`太字と右揃えであり、右のプロパティの値に余白を追加するプロパティの名前が発生するとします。

表示するには、フォーム ビューで使用可能な CheckBoxFields がないので、`Discontinued`値チェック ボックスとしては、独自のチェック ボックス コントロールを追加する必要があります。 `Enabled`プロパティが False で、読み取り専用になりますし、チェック ボックスの`Checked`プロパティの値にバインドする、`Discontinued`データ フィールド。

`ItemTemplate`製品情報がより滑らかな方法で表示されます、完了します。 (図 4) このチュートリアルでは、FormView によって生成される出力の最後のチュートリアル (図 3) からの出力を DetailsView を比較します。


[![剛体 DetailsView 出力](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**図 3**: 剛体 DetailsView 出力 ([フルサイズの画像を表示する をクリックします](using-the-formview-s-templates-vb/_static/image9.png))。


[![滑らかな FormView 出力](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**図 4**: 流体 FormView 出力 ([フルサイズの画像を表示する をクリックします](using-the-formview-s-templates-vb/_static/image12.png))。


## <a name="summary"></a>まとめ

GridView および DetailsView コントロールでは、カスタマイズ TemplateFields を使用して、出力を持つことができます、両方が、データ形式で表示する、グリッドのような角張ったします。 1 つのレコードを表示する必要がある場合も低い剛体のレイアウトを使用して、FormView が最適な選択肢です。 FormView、DetailsView などから 1 つのレコードを表示しますその`DataSource`DetailsView とは異なりが、テンプレートだけで構成されるありフィールドをサポートしていません。

このチュートリアルで説明したように、1 つのレコードを表示するときに、FormView はより柔軟なレイアウトにできます。 将来的に同じレベルとして、FormsView 柔軟性の提供しますが、(GridView) などの複数のレコードを表示することがその DataList と Repeater コントロールについて説明しますなるチュートリアルです。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルのレビュー担当者の潜在顧客が E.R. Gilmore します。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](using-templatefields-in-the-detailsview-control-vb.md)
> [次へ](displaying-summary-information-in-the-gridview-s-footer-vb.md)
