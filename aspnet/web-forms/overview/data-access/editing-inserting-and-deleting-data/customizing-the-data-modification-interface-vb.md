---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: データ変更インターフェイス (VB) のカスタマイズ |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは紹介では、標準のテキスト ボックスに置き換えることで、編集可能な GridView のインターフェイスをカスタマイズする方法と、alternati でチェック ボックスを制御しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7d334a86fcf2fbd1069628527c6e89f3ab655dd5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888631"
---
<a name="customizing-the-data-modification-interface-vb"></a>データ変更インターフェイス (VB) のカスタマイズ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe)または[PDF のダウンロード](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> このチュートリアルでは紹介では、標準のテキスト ボックスに置き換えることで、編集可能な GridView のインターフェイスをカスタマイズする方法と代替の入力 Web コントロールのチェック ボックスを制御します。


## <a name="introduction"></a>はじめに

BoundFields と CheckBoxFields GridView と DetailsView コントロールで使用されるデータを読み取り専用で、編集、および挿入可能なインターフェイスを表示するには、その機能のための変更のプロセスを簡略化します。 これらのインターフェイスは、追加の宣言型マークアップやコードを追加することがなくても表示できます。 ただし、BoundField および CheckBoxField のインターフェイスには、多くの場合、実際のシナリオで必要なカスタマイズが不足しています。 GridView または DetailsView では、編集可能または挿入可能なインターフェイスをカスタマイズするためには、代わりに、TemplateField を使用する必要があります。

[前のチュートリアル](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)Web の検証コントロールを追加することで、データ変更のインターフェイスをカスタマイズする方法を説明しました。 このチュートリアルでは、BoundField と CheckBoxField の標準のテキスト ボックスを置き換える実際のデータ コレクションの Web コントロールと代替の入力 Web コントロールのチェック ボックス コントロールをカスタマイズする方法について見ていきます。 具体的には、製品の名前、カテゴリ、供給業者、および提供が中止された状態を更新するをできる編集可能な GridView をビルドします。 特定の行を編集するには、カテゴリおよび仕入先のフィールドは DropDownLists、としてレンダリングはからを選択するサプライヤーと利用可能なカテゴリのセットを格納します。 さらに、CheckBoxField の既定 チェック ボックスは 2 つのオプションを提供する RadioButtonList コントロールで置き換えること:"Active"、「中止」です。


[![GridView の編集インターフェイスには、DropDownLists およびオプション ボタンが含まれます。](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**図 1**: GridView のインターフェイスが含まれています DropDownLists を編集し、オプション ボタン ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>手順 1: 作成、適切な`UpdateProduct`オーバー ロード

このチュートリアルでは、製品の名前、カテゴリ、供給業者、編集を許可し、提供が中止された状態を編集可能な GridView を構築します。 そのため、必要な`UpdateProduct`を 5 つの入力パラメーターこれら 4 つの製品の値を受け入れるオーバー ロードと`ProductID`です。 ように、以前のオーバー ロードがこのは、1 つで

1. 指定されたデータベースから製品情報を取得`ProductID`、
2. 更新プログラム、 `ProductName`、 `CategoryID`、 `SupplierID`、および`Discontinued`フィールド、および
3. TableAdapter のを通じて、DAL に更新要求を送信`Update()`メソッドです。

簡略化のため、この特定のオーバー ロードが省略されていた廃止としてマークされている製品にその業者によって提供される唯一の製品がされていないことを確認するビジネス ルール チェックします。 だと自由に追加する場合、または、理想的には、別個のメソッドにロジックをリファクターします。

次のコードは、新しい`UpdateProduct`でオーバー ロード、`ProductsBLL`クラス。


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>手順 2: 作成、編集可能な GridView

`UpdateProduct`オーバー ロードが追加されると、編集可能な GridView を作成する準備ができました。 開いている、 `CustomizedUI.aspx`  ページで、`EditInsertDelete`フォルダー GridView コントロールをデザイナーに追加します。 次に、GridView のスマート タグから新しい ObjectDataSource を作成します。 構成を使用して製品情報を取得する ObjectDataSource、`ProductBLL`クラスの`GetProducts()`メソッドを使用して製品データを更新して、`UpdateProduct`作成したばかりのオーバー ロードします。 挿入および削除のタブからは、ドロップダウン リストから () を選択します。


[![先ほど作成した UpdateProduct オーバー ロードを使用する ObjectDataSource を構成します。](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**図 2**: 構成を使用する ObjectDataSource、`UpdateProduct`先ほど作成した、オーバー ロード ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image6.png))


これまで見てきたデータ変更のチュートリアル全体で、Visual Studio によって作成された ObjectDataSource の宣言の構文を割り当てます、`OldValuesParameterFormatString`プロパティを`original_{0}`です。 これには、もちろんでは使用できません、ビジネス ロジック層、メソッドは、元の必要がないため`ProductID`で渡される値。 したがって、前のチュートリアルで実行したを実行する宣言構文からこのプロパティの割り当てを削除するか、代わりに、このプロパティの値を設定にはしばらく`{0}`です。

この変更後に、ObjectDataSource の宣言型マークアップは、次のようになります。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

注意してください、`OldValuesParameterFormatString`プロパティが削除されましたがあることと、`Parameter`で、`UpdateParameters`で想定されている入力パラメーターの各コレクション、`UpdateProduct`オーバー ロードします。

GridView が表示されている、ObjectDataSource は製品の値のサブセットのみを更新するように構成、*すべて*製品フィールドです。 GridView を編集するようにします。

- のみが含まれます、 `ProductName`、 `SupplierName`、 `CategoryName` BoundFields と`Discontinued`CheckBoxField
- `CategoryName`と`SupplierName`(左) にする前に表示されるフィールドを`Discontinued`CheckBoxField
- `CategoryName`と`SupplierName`BoundFields'`HeaderText`プロパティがそれぞれ「カテゴリ」および「業者」に設定
- 編集のサポートが有効になっている (GridView のスマート タグの編集を有効にするチェック ボックスを確認してください)

これらの変更後に、デザイナーようになります図 3 は、次に示す GridView の宣言の構文を使用します。


[![GridView から不要なフィールドを削除します。](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**図 3**: GridView から不要なフィールドを削除 ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

この時点で、GridView の読み取り専用の動作が完了しました。 データを表示すると各製品が製品の名前、カテゴリ、供給業者を示す GridView の行として表示される提供が中止された状態。


[![GridView の読み取り専用のインターフェイスは、完了です。](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**図 4**: GridView の読み取り専用のインターフェイスは、完了 ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image12.png))


> [!NOTE]
> 説明したよう[、概要の挿入、更新、および削除するデータのチュートリアル](an-overview-of-inserting-updating-and-deleting-data-cs.md)、s ビューステートする GridView に (既定の動作) が有効になっていることが大切です。 GridView s を設定した場合`EnableViewState`プロパティを`false`、同時実行ユーザーが誤って削除または編集を記録する危険を実行します。 参照してください[警告: 同時実行の問題と ASP.NET 2.0 Gridview/DetailsView/FormViews その編集をサポートするか削除すると、をビュー ステートが無効になっている](http://scottonwriting.net/sowblog/posts/10054.aspx)詳細についてはします。


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>手順 3: DropDownList を使用して仕入先のインターフェイスを編集し、カテゴリに

注意してください、`ProductsRow`オブジェクトを含む`CategoryID`、 `CategoryName`、 `SupplierID`、および`SupplierName`で実際の外部キーの ID 値を提供するプロパティ、`Products`データベース テーブルと、対応する`Name`値が、`Categories`と`Suppliers`テーブル。 `ProductRow`の`CategoryID`と`SupplierID`できます両方する読み取りおよび書き込み時に、`CategoryName`と`SupplierName`プロパティが読み取り専用にマークされます。

読み取り専用状態のため、`CategoryName`と`SupplierName`プロパティ、対応する BoundFields お持ちの`ReadOnly`プロパティに設定`True`、これらの値が行を編集する際に変更されていることを妨げています。 設定することがときに、`ReadOnly`プロパティを`False`、レンダリング、`CategoryName`と`SupplierName`BoundFields 編集中にテキスト ボックスに、このようなアプローチは、例外発生、ユーザーがあるため、製品を更新しようとしたときなし`UpdateProduct`で受け取るオーバー ロード`CategoryName`と`SupplierName`入力します。 実際には、2 つの理由でこのようなオーバー ロードを作成したくありません。

- `Products`テーブルがない`SupplierName`または`CategoryName`フィールドが、`SupplierID`と`CategoryID`です。 したがって、これら特定 ID の値をそのルックアップ テーブルの値ではなくに渡される、メソッドが必要です。
- 利用可能なカテゴリと仕入先と、正しいスペルを知っているユーザーが必要な業者またはカテゴリの名前を入力する必要はありませんはより小さい理想的です。

供給業者とカテゴリ フィールドには、カテゴリと読み取り専用モードのときに供給業者の名前を表示する必要があります (ここでは) とし、編集されているときに、該当するオプションのドロップダウン リスト。 ドロップダウン リストを使用して、エンドユーザーは、どのようなカテゴリと仕入先がの中から選択できるすばやく確認でき、詳細作業が簡単に、選択します。

この動作を提供する必要がありますに変換する、`SupplierName`と`CategoryName`TemplateFields に BoundFields が`ItemTemplate`が出力、`SupplierName`と`CategoryName`値と持つ`EditItemTemplate`リストに DropDownList コントロールを使用して、利用可能なカテゴリと仕入先。

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>追加する、`Categories`と`Suppliers`DropDownLists

変換することで開始、`SupplierName`と`CategoryName`によって TemplateFields に BoundFields: GridView のスマート タグからの列の編集リンクをクリックすると; 左下にある一覧から、BoundField を選択しをクリックすると、"には、このフィールドを変換、TemplateField"にリンクします。 変換プロセスは両方と TemplateField を作成、`ItemTemplate`と`EditItemTemplate`宣言の構文を以下に示すように、します。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

BoundField が読み取り専用としてマークされているため両方、`ItemTemplate`と`EditItemTemplate`Label Web が含まれているコントロール`Text`プロパティが、該当するデータ フィールドにバインドされている (`CategoryName`、上記の構文で)。 変更する必要があります、 `EditItemTemplate`DropDownList のコントロールにラベル Web コントロールを置換します。

前のチュートリアルで見てきたよう、デザイナーを使用または宣言構文から直接、テンプレートを編集できます。 デザイナーを使用には、それを編集するには、GridView のスマート タグからのテンプレートの編集リンクをクリックし、カテゴリ フィールドの作業を選択する`EditItemTemplate`です。 ラベルの Web コントロールを削除し、次のドロップダウン リストの ID プロパティを設定、DropDownList コントロールで置き換えます`Categories`です。


[![ テキスト ボックスでを削除し、EditItemTemplate の DropDownList の追加](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**図 5**: テキスト ボックスでを削除する DropDownList を追加して、 `EditItemTemplate` ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image15.png))


次に、利用可能なカテゴリでの DropDownList を設定する必要があります。 DropDownList のスマート タグからデータ ソースの選択 リンクをクリックし、という名前の新しい ObjectDataSource を作成することを選択`CategoriesDataSource`です。


[![CategoriesDataSource をという名前の新しい ObjectDataSource コントロールを作成します。](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**図 6**: 新しい ObjectDataSource コントロールの名前付き作成`CategoriesDataSource`([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image18.png))


すべてのカテゴリを返すこの ObjectDataSource にバインド、`CategoriesBLL`クラスの`GetCategories()`メソッドです。


[![CategoriesBLL の GetCategories() メソッドに、ObjectDataSource をバインドします。](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**図 7**: バインドに ObjectDataSource、`CategoriesBLL`の`GetCategories()`メソッド ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image21.png))


最後に、次のドロップダウン リストの設定を構成するよう、`CategoryName`各 DropDownList にフィールドを表示`ListItem`で、`CategoryID`フィールドの値として使用します。


[![区分名 フィールドが表示されますが、CategoryID が値として使用あり](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**図 8**: が、`CategoryName`フィールドが表示されると、`CategoryID`値として使用 ([フルサイズのイメージを表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image24.png))。


これらの宣言型マークアップを変更した後、`EditItemTemplate`で、 `CategoryName` TemplateField には DropDownList と、ObjectDataSource の両方が含まれます。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> DropDownList、`EditItemTemplate`そのビュー ステートを有効になっている必要があります。 すぐにデータ バインド構文に追加 DropDownList の宣言の構文となどのデータ バインド コマンド`Eval()`と`Bind()`ビュー状態が有効になっているコントロールにのみ表示できます。


これらの手順を繰り返してという名前の DropDownList の追加`Suppliers`を`SupplierName`TemplateField の`EditItemTemplate`します。 DropDownList を追加することが必要、`EditItemTemplate`別 ObjectDataSource を作成するとします。 `Suppliers` DropDownList の ObjectDataSource、ただしを構成する呼び出し、`SuppliersBLL`クラスの`GetSuppliers()`メソッドです。 さらに、構成、 `Suppliers` DropDownList を表示する、`CompanyName`フィールドに使用して、`SupplierID`フィールドの値としてその`ListItem`s。

2 つに、DropDownLists を追加した後`EditItemTemplate`s、ブラウザーでページを読み込むし、Chef Anton の Cajun Seasoning 商品の編集 をクリックします。 図 9 に示す、製品のカテゴリおよび仕入先の列は、ドロップダウン リストから選択するには、仕入先、利用可能なカテゴリを含むとしてレンダリングされます。 しかし、なお、*最初*両方のドロップダウン リスト内の項目は、供給業者、としては Chef Anton Cajun Seasoning 新規 Orleans Cajun によって提供される Condiment で、既定 (飲み物のカテゴリの) および特別な液体で選択されますコーヒーです。


[![既定では、ドロップダウン リストの最初の項目が選択されています。](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**図 9**: ドロップダウン リストの最初の項目が既定で選択されている ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image27.png))


さらに、更新プログラムをクリックすると、わかる製品の`CategoryID`と`SupplierID`値に設定されます`NULL`です。 動作が原因と望ましくないこれらの両方で DropDownLists、 `EditItemTemplate` s が、基になる製品データから、データ フィールドにバインドされていません。

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>バインドに DropDownLists、`CategoryID`と`SupplierID`データ フィールド

適切な値と BLL に送信されるこれらの値を持つ、編集した製品のカテゴリと仕入先があるためにドロップダウン リストを設定`UpdateProduct`DropDownLists' をバインドしなければならない更新プログラムをクリックすると、メソッド`SelectedValue`プロパティを`CategoryID`と`SupplierID`双方向データ バインドを使用してデータ フィールドです。 これを実現し、 `Categories` DropDownList を追加できます`SelectedValue='<%# Bind("CategoryID") %>'`宣言の構文に直接できます。

または、デザイナーでテンプレートを編集する DropDownList のスマート タグから databindings の編集 リンクをクリックして DropDownList のデータ バインディングを設定できます。 次に、ことを示すため、`SelectedValue`プロパティにバインドする必要があります、`CategoryID`双方向データ バインドを使用してフィールド (図 10 参照)。 いずれか、宣言的またはデザイナーをバインドするプロセスを繰り返して、`SupplierID`データ フィールドを`Suppliers`DropDownList です。


[![双方向データ バインドを使用して、DropDownList の SelectedValue プロパティに、区分をバインドします。](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**図 10**: バインド、 `CategoryID` DropDownList のする`SelectedValue`プロパティを使用して双方向データ バインド ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image30.png))


バインディングに適用された後、 `SelectedValue` DropDownLists を 2 つのプロパティを編集した製品のカテゴリおよび仕入先の列は既定で、現在の製品の値にします。 更新プログラムをクリックすると、`CategoryID`と`SupplierID`にドロップダウン リストを選択した項目の値が渡される、`UpdateProduct`メソッドです。 図 11 は、データ バインド ステートメントが追加された後に、チュートリアルを示していますChef Anton Cajun Seasoning のドロップダウン リストを選択した項目は正しく Condiment および新規の Orleans Cajun コーヒー方法に注意してください。


[![編集の製品の現在のカテゴリと仕入先の値が既定で選択されています。](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**図 11**:「編集製品現在のカテゴリ、および業者値は、既定で選択されます ([フルサイズのイメージを表示するには、をクリックして](customizing-the-data-modification-interface-vb/_static/image33.png))


## <a name="handlingnullvalues"></a>処理`NULL`値

`CategoryID`と`SupplierID`内の列、 `Products` table には、 `NULL`、まだに DropDownLists、`EditItemTemplate`秒を表すリスト項目を含めません、`NULL`値。 これは、2 つの影響があります。

- ユーザーは、のインターフェイスを使用して以外から製品のカテゴリまたは仕入先を変更することはできません`NULL`値を`NULL`いずれか
- 製品の場合、 `NULL` `CategoryID`または`SupplierID`、[編集] ボタンをクリックすると、例外が発生します。 これは、ため、`NULL`によって返される値`CategoryID`(または`SupplierID`) で、`Bind()`ステートメントが次のドロップダウン リスト内の値にマップされていない (DropDownList は例外をスローときにその`SelectedValue`の値に設定されます*いない*そのリスト項目のコレクションで)。

サポートするために`NULL``CategoryID`と`SupplierID`値、もう 1 つ追加する必要があります`ListItem`各 DropDownList を表すために、`NULL`値。 [マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)チュートリアルでは、追加する方法を説明しました`ListItem`DropDownList の設定が関係をデータ バインディングの DropDownList に`AppendDataBoundItems`プロパティを`True`と手動で追加、`ListItem`です。 チュートリアルではその前、ただし、追加しました、`ListItem`で、`Value`の`-1`します。 ASP.NET では、データ バインドのロジックは、ただしは、空の文字列に自動的に変換されます、`NULL`値と逆にします。 そのため、このチュートリアルのたい、`ListItem`の`Value`空の文字列にします。

両方 DropDownLists' を設定して開始`AppendDataBoundItems`プロパティを`True`です。 次に、追加、 `NULL` `ListItem` 、次を追加することによって`<asp:ListItem>`各 DropDownList 要素などの宣言型マークアップが見えるようにします。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

選択して"(None)"を使用するテキスト値としてこの`ListItem`、希望の場合も、空の文字列であることを変更することができます。

> [!NOTE]
> 説明したとおり、*マスター/詳細のフィルター処理での DropDownList*チュートリアルでは、 `ListItem` s は DropDownList をクリックすると、デザイナーでの DropDownList に追加できる`Items`プロパティ ウィンドウでプロパティ (表示されます、`ListItem`コレクション エディター)。 ただし、追加するようにして、 `NULL` `ListItem`このチュートリアルでは宣言の構文を使用します。 使用する場合、`ListItem`コレクション エディターで生成された宣言型の構文が省略されます、`Value`のような宣言型マークアップを作成する、空の文字列が割り当てられている場合に、設定完全:`<asp:ListItem>(None)</asp:ListItem>`です。 これは、影響を与えません見えるかもしれませんが、値がありませんを使用する DropDownList、`Text`代わりに、プロパティの値。 つまり、その場合この`NULL``ListItem`は選択すると、"(None)"値が試行されますに割り当てられる、 `CategoryID`、例外が発生します。 明示的に設定して`Value=""`、`NULL`に割り当てられる値`CategoryID`ときに、 `NULL` `ListItem`が選択されています。


サプライヤーの DropDownList の次の手順を繰り返します。

この追加`ListItem`、編集のインターフェイスを割り当てるようにできます`NULL`製品の値`CategoryID`と`SupplierID`フィールド、図 12 に示すようにします。


[![製品のカテゴリまたは業者の NULL 値を割り当てる (なし)](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**図 12**: に割り当てる (なし)、`NULL`製品のカテゴリまたは仕入先の値 ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>手順 4: 提供が中止された状態のオプション ボタンを使用します。

現在、製品の`Discontinued`CheckBoxField では、読み取り専用の行のチェック ボックスが無効になっていると編集されている行の有効になっているチェック ボックスを表示を使用して、データ フィールドが表されます。 このユーザー インターフェイスでは適切な多くの場合、TemplateField を使用して必要な場合はカスタマイズできます。 このチュートリアルでは、変更してみましょう CheckBoxField を元のユーザーが指定できる製品の 2 つのオプション"Active"、「中止」RadioButtonList コントロールを使用する TemplateField`Discontinued`値。

変換することで開始、`Discontinued`を TemplateField を作成し、TemplateField に CheckBoxField、`ItemTemplate`と`EditItemTemplate`です。 両方のテンプレート付きのチェック ボックスは、その`Checked`プロパティにバインドされる、`Discontinued`データ フィールドに記述されていること、2 つの唯一の違い、`ItemTemplate`のチェック ボックスの`Enabled`プロパティに設定されている`False`です。

両方のチェック ボックスを置き換える、`ItemTemplate`と`EditItemTemplate`RadioButtonList コントロールでは、設定が両方 RadioButtonLists'`ID`プロパティ`DiscontinuedChoice`です。 次を示す、RadioButtonLists 必要がありますが含まれる 2 つのオプション ボタン、1 つのラベルの付いた「アクティブ」、"False"、「中止」というラベルの付いた 1 つの値"True"の値を使用します。 入力するか、これを実現する、`<asp:ListItem>`内の要素を使用して、宣言構文から直接、`ListItem`デザイナーからコレクション エディターです。 図 13 を示しています、`ListItem`コレクション エディター後、2 つのラジオ ボタンのオプションが指定されています。


[![RadioButtonList にアクティブで廃止されたオプションを追加します。](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**図 13**: 追加のアクティブなと RadioButtonList 廃止されたオプション ([フルサイズのイメージを表示するをクリックして](customizing-the-data-modification-interface-vb/_static/image39.png))


RadioButtonList 以降、`ItemTemplate`編集可能にすることはできません設定、`Enabled`プロパティを`False`したまま、`Enabled`プロパティを`True`(既定) で RadioButtonList の`EditItemTemplate`です。 これは、読み取り専用でない編集行にあるオプション ボタンが作成されますが、RadioButton 値を編集した行を変更するユーザーを許可します。

RadioButtonList コントロールを割り当てる必要があります`SelectedValue`プロパティ該当するラジオ ボタンが選択されるように基づく、製品の`Discontinued`データ フィールドです。 このチュートリアルで前に検査 DropDownLists とこのデータ バインド構文か、または追加できます宣言型マークアップに直接 RadioButtonLists のスマート タグで databindings の編集 リンクを使用します。

2 つの RadioButtonLists を追加して、それらを構成したら、 `Discontinued` TemplateField の宣言型マークアップのようになります。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

これらの変更、 `Discontinued` (図 14 を参照してください) ラジオ ボタンの組み合わせの一覧を表示チェック ボックスの一覧から列を変換されています。 該当するラジオ ボタンが選択されている製品を編集するには、および他のオプション ボタンを選択し、更新プログラムをクリックして、製品の提供が中止された状態を更新できます。


[![廃止された対応するチェック ボックスをラジオ ボタンの組み合わせに置き換えられています](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**図 14**:「提供が中止されたチェック ボックス オンに置換されたラジオ ボタンの組み合わせ ([フルサイズ イメージを表示するに、をクリックして](customizing-the-data-modification-interface-vb/_static/image42.png))


> [!NOTE]
> 以降、`Discontinued`内の列、`Products`データベースことはできません`NULL`値、お必要はありませんキャプチャについて心配する`NULL`インターフェイスの情報です。 場合、ただし、`Discontinued`列を含めたり`NULL`値は 3 つ目を追加するラジオ ボタンを使用して、リストにその`Value`、空の文字列に設定 (`Value=""`) カテゴリと業者 DropDownLists と同様、します。


## <a name="summary"></a>まとめ

BoundField と CheckBoxField 読み取り専用で、編集、および挿入のインターフェイスを自動的にレンダリングして、ときに、カスタマイズ機能が不足しています。 多くの場合、ただし、必要があります、編集またはインターフェイスの挿入をカスタマイズする (で示したように前のチュートリアル) の検証コントロールを追加するか (このチュートリアルで示した) ように、データ コレクションのユーザー インターフェイスをカスタマイズしています。 TemplateField を持つインターフェイスをカスタマイズすることができますに要約、次の手順。

1. TemplateField を追加するか、既存の BoundField または CheckBoxField を TemplateField に変換
2. 必要に応じて、インターフェイスを拡張します。
3. 適切なデータ フィールドを双方向データ バインドを使用して、新しく追加された Web コントロールにバインドします。

組み込みの ASP.NET Web コントロールを使用するだけでなく、カスタムのコンパイル済みのサーバー コントロールおよびユーザー コントロールを TemplateField のテンプレートをカスタマイズすることもできます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [次へ](implementing-optimistic-concurrency-vb.md)
