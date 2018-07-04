---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: データ変更インターフェイス (VB) のカスタマイズ |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、標準のテキスト ボックスを置き換えることで、編集可能な GridView のインターフェイスをカスタマイズする方法に注目します チェック ボックス コントロール alternati.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 5eea1f226cbcfa07c2fcca3d68fb5ee1cb88ab61
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370378"
---
<a name="customizing-the-data-modification-interface-vb"></a>データ変更インターフェイス (VB) のカスタマイズ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe)または[PDF のダウンロード](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> このチュートリアルでは、標準のテキスト ボックスを置き換えることで、編集可能な GridView のインターフェイスをカスタマイズする方法に注目し、代替の入力 Web コントロールを含む チェック ボックスを制御します。


## <a name="introduction"></a>はじめに

BoundFields と CheckBoxFields GridView および DetailsView コントロールで使用される読み取り専用、編集、および挿入可能なインターフェイスをレンダリングするには、その機能のためのデータの変更のプロセスを簡略化します。 追加の宣言型マークアップまたはコードを追加する必要はありませんは、これらのインターフェイスを表示できます。 ただし、BoundField および CheckBoxField のインターフェイスには、多くの場合、実際のシナリオで必要なカスタマイズ機能が不足しています。 GridView、DetailsView で編集可能または挿入可能なインターフェイスをカスタマイズするためには、代わりに、TemplateField を使用する必要があります。

[前のチュートリアル](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)検証 Web コントロールを追加して、データ変更インターフェイスをカスタマイズする方法を説明しました。 このチュートリアルでは、BoundField と CheckBoxField の標準のテキスト ボックスに置き換える実際のデータ コレクションの Web コントロールと代替の入力 Web コントロールを含む チェック ボックス コントロールをカスタマイズする方法について説明します。 具体的には、製品の名前、カテゴリ、供給業者、および提供が中止された状態を更新できる編集可能な GridView をビルドします。 特定の行を編集するには、category と supplier フィールドは、Dropdownlist としてレンダリングされますから選択するサプライヤーと利用可能なカテゴリのセットを含みます。 さらに、に代わって CheckBoxField の既定のチェック ボックスで、RadioButtonList コントロールを 2 つのオプションを提供します。"アクティブ"と"廃止"。


[![GridView の編集インターフェイスには、Dropdownlist、オプション ボタンが含まれています。](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**図 1**: GridView のインターフェイスが含まれています Dropdownlist を編集し、オプション ボタン ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image3.png))。


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>手順 1: 作成、適切な`UpdateProduct`オーバー ロード

このチュートリアルでは、製品の名前、カテゴリ、供給業者、編集を許可し、ステータスの提供が中止された編集可能な GridView を作成します。 そのため、必要があります、`UpdateProduct`を 5 つの入力パラメーターこれら 4 つの製品の値を受け入れるオーバー ロードだけでなく、`ProductID`します。 など、以前オーバー ロードをこの 1 つには。

1. 指定したデータベースから製品情報を取得`ProductID`、
2. 更新プログラム、 `ProductName`、 `CategoryID`、 `SupplierID`、および`Discontinued`フィールド、および
3. TableAdapter のを通じて、DAL に更新要求を送信`Update()`メソッド。

簡潔にするため、この特定のオーバー ロードした省略していますビジネス ルール チェック確実に提供が中止されたとマークされる製品がその供給業者によって提供される唯一の製品はありません。 自由に場合で追加または、理想的には、ロジックを別のメソッドにリファクタリングします。

次のコードは、新しい`UpdateProduct`でオーバー ロード、`ProductsBLL`クラス。


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>手順 2: 作成、編集可能な GridView

`UpdateProduct`オーバー ロードを追加、編集可能な GridView を作成する準備ができました。 開く、`CustomizedUI.aspx`ページで、`EditInsertDelete`フォルダーと GridView コントロールをデザイナーに追加します。 次に、GridView のスマート タグから新しい ObjectDataSource を作成します。 構成を使用して製品情報を取得する ObjectDataSource、`ProductBLL`クラスの`GetProducts()`メソッドを使用して製品データを更新して、`UpdateProduct`オーバー ロードを作成したばかりです。 INSERT および DELETE のタブからは、ドロップダウン リストから () を選択します。


[![先ほど作成した UpdateProduct オーバー ロードを使用する ObjectDataSource を構成します。](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**図 2**: 構成に使用する ObjectDataSource、`UpdateProduct`先ほど作成したオーバー ロード ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image6.png))。


データ変更のチュートリアル全体で見たように Visual Studio によって作成された ObjectDataSource の宣言構文を割り当てます、`OldValuesParameterFormatString`プロパティを`original_{0}`します。 これは、もちろん、機能しません、ビジネス ロジック層で、メソッドは、元の予定がないため`ProductID`で渡される値。 このため、前のチュートリアルで行ったように、宣言型構文からこのプロパティの割り当てを削除するか、代わりに、このプロパティの値を設定には少しを要する`{0}`します。

この変更後、次のよう ObjectDataSource の宣言型マークアップになります。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

なお、`OldValuesParameterFormatString`プロパティが削除されているし、は、`Parameter`で、`UpdateParameters`で想定されている入力パラメーターのそれぞれのコレクション、`UpdateProduct`オーバー ロードします。

製品の値のサブセットのみを更新するには、ObjectDataSource が構成、GridView が現在表示*すべて*製品フィールド。 GridView を編集する少しように。

- のみを含み、 `ProductName`、 `SupplierName`、 `CategoryName` BoundFields と`Discontinued`CheckBoxField
- `CategoryName`と`SupplierName`前に、(左) に表示するフィールド、 `Discontinued` CheckBoxField
- `CategoryName`と`SupplierName`BoundFields'`HeaderText`プロパティがそれぞれ"Category"と「業者」に設定
- 編集のサポートが有効になっている (GridView のスマート タグの編集を有効にするチェック ボックスをチェック)

これらの変更後、デザイナーはようになります図 3 は、次に示す GridView の宣言型構文を使用します。


[![GridView から不要なフィールドを削除します。](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**図 3**: GridView から不要なフィールドを削除 ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image9.png))。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

この時点で、GridView の読み取り専用の動作が完了しました。 データを表示するには、各製品が製品の名前、カテゴリ、供給業者、表示、GridView の行として表示され、ステータスの提供が中止されました。


[![GridView の読み取り専用のインターフェイスは、完了です。](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**図 4**: GridView の読み取り専用のインターフェイスは、完了 ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image12.png))。


> [!NOTE]
> 説明したよう[、更新、および削除するデータ挿入の 概要のチュートリアル](an-overview-of-inserting-updating-and-deleting-data-cs.md)s のビュー ステートが GridView に (既定の動作) が有効になっていることがきわめて重要です。 GridView 秒に設定した場合`EnableViewState`プロパティを`false`、同時実行ユーザーが誤って削除または編集を記録するというリスクを実行します。 参照してください[警告: 同時実行の問題を ASP.NET 2.0 Gridview と DetailsView/FormViews そのサポートを編集または削除およびをビュー ステートが無効になっている](http://scottonwriting.net/sowblog/posts/10054.aspx)詳細についてはします。


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>手順 3: Category と Supplier インターフェイスの編集の DropDownList を使用します。

いることを思い出してください、`ProductsRow`オブジェクトが含まれます`CategoryID`、 `CategoryName`、 `SupplierID`、および`SupplierName`の実際の外部キーの ID 値を提供するには、プロパティ、`Products`データベース テーブルと、対応する`Name`値、`Categories`と`Suppliers`テーブル。 `ProductRow`の`CategoryID`と`SupplierID`両方からの読み取りおよび書き込みできる時に、`CategoryName`と`SupplierName`プロパティが読み取り専用にマークされます。

読み取り専用状態のため、`CategoryName`と`SupplierName`プロパティ、対応する BoundFields があった、`ReadOnly`プロパティに設定`True`、これらの値が行を編集する際に変更されることを防止します。 設定することが中に、`ReadOnly`プロパティを`False`、レンダリング、`CategoryName`と`SupplierName`BoundFields 編集中にテキスト ボックスに、このアプローチは例外が発生、ユーザーがあるため、製品を更新しようとするとありません`UpdateProduct`で取るオーバー ロード`CategoryName`と`SupplierName`入力します。 実際には、2 つの理由でこのようなオーバー ロードを作成するはありません。

- `Products`テーブルがない`SupplierName`または`CategoryName`フィールドが`SupplierID`と`CategoryID`します。 そのため、これらの ID 値のルックアップ テーブルの値ではなくに渡される方法では、します。
- サプライヤーまたはカテゴリの名前を入力するユーザーを要求するはユーザーが利用可能なカテゴリと仕入先と、正しいスペルを知っている必要がある未満に最適です。

サプライヤーとカテゴリ フィールドには、カテゴリと読み取り専用モードで仕入先の名前を表示する必要があります (ここでは) と、編集中の場合、該当するオプションのドロップダウン リスト。 ドロップダウン リストを使用して、エンドユーザーは、どのようなカテゴリと仕入先は、間で選択可能なすばやく確認できより簡単にできるを選択します。

この動作を提供するには、変換する必要、`SupplierName`と`CategoryName`TemplateFields に BoundFields が`ItemTemplate`出力、`SupplierName`と`CategoryName`値と持つ`EditItemTemplate`リストに DropDownList コントロールを使用して、利用可能なカテゴリと仕入先。

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>追加、`Categories`と`Suppliers`Dropdownlist

変換することで開始、`SupplierName`と`CategoryName`で TemplateFields を BoundFields: GridView のスマート タグからの列の編集リンクをクリックし; 左下に一覧から、BoundField を選択しをクリックして、"には、このフィールドを変換、TemplateField"をリンクします。 変換プロセスは両方と TemplateField を作成、`ItemTemplate`と`EditItemTemplate`宣言の構文を次のように、します。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

BoundField が読み取り専用としてマークされているため両方、`ItemTemplate`と`EditItemTemplate`ラベル Web が含まれているコントロール`Text`プロパティが適用されるデータ フィールドにバインドされます (`CategoryName`、上記の構文で)。 変更する必要があります、 `EditItemTemplate`、DropDownList コントロールと Label Web コントロールを置換します。

前のチュートリアルできたよう、デザイナーを使用または宣言型構文から直接テンプレートを編集できます。 デザイナーで編集、GridView のスマート タグからのテンプレートの編集リンクをクリックしを選択して、カテゴリ フィールドの操作`EditItemTemplate`します。 ラベルの Web コントロールを削除し、DropDownList の ID プロパティを設定、DropDownList コントロールに置き換えます`Categories`します。


[![ テキスト ボックスを削除し、後に、DropDownList を追加](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**図 5**: テキスト ボックスを削除し、DropDownList を追加、 `EditItemTemplate` ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image15.png))。


次に、利用可能なカテゴリで DropDownList を設定する必要があります。 DropDownList のスマート タグから データ ソースのリンクをクリックし、という名前の新しい ObjectDataSource を作成することを選択`CategoriesDataSource`します。


[![CategoriesDataSource という名前の新しい ObjectDataSource コントロールを作成します。](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**図 6**: 新しい ObjectDataSource コントロールの名前付き作成`CategoriesDataSource`([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image18.png))。


この ObjectDataSource を返すすべてのカテゴリには、バインドして、`CategoriesBLL`クラスの`GetCategories()`メソッド。


[![ObjectDataSource を CategoriesBLL の GetCategories() メソッドにバインドします。](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**図 7**: バインドを ObjectDataSource、`CategoriesBLL`の`GetCategories()`メソッド ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image21.png))。


最後に、DropDownList の設定を構成するよう、`CategoryName`フィールドは、各 DropDownList に表示される`ListItem`で、`CategoryID`フィールドの値として使用します。


[![区分名 フィールドが表示されますが、CategoryID を値として使用あり](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**図 8**: が、`CategoryName`フィールドが表示されると、`CategoryID`値として使用 ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image24.png))。


これらの宣言型マークアップを変更した後、`EditItemTemplate`で、 `CategoryName` TemplateField には DropDownList と ObjectDataSource の両方が含まれます。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> DropDownList、`EditItemTemplate`そのビュー ステートを有効になっている必要があります。 DropDownList の宣言型構文をデータ バインディング構文に追加できるだけとなどのデータ バインド コマンド`Eval()`と`Bind()`はビュー ステートが有効になっているコントロールにのみ表示されます。


という名前の DropDownList を追加するには、この手順を繰り返します`Suppliers`を`SupplierName`TemplateField の`EditItemTemplate`します。 DropDownList を追加するこのプロセスでは、`EditItemTemplate`と別の ObjectDataSource を作成します。 `Suppliers` DropDownList の ObjectDataSource、ただし、行う必要がありますを呼び出す、`SuppliersBLL`クラスの`GetSuppliers()`メソッド。 さらに、構成、 `Suppliers` DropDownList を表示する、`CompanyName`フィールドし、を使用して、`SupplierID`フィールドの値としてその`ListItem`秒。

2 つに、Dropdownlist を追加した後`EditItemTemplate`s、ブラウザーでページを読み込むし、Chef Anton の Cajun Seasoning 製品の編集 ボタンをクリックします。 図 9 に示す、製品のカテゴリと仕入先の列は、利用可能なカテゴリと仕入先から選択を含むドロップダウン リストとしてレンダリングされます。 ただし、注意、*最初*両方のドロップダウン リスト内の項目は、供給業者、として場合でも、ニューオーリンズ Cajun によって提供される Condiment は、Chef Anton の Cajun Seasoning で、既定 (飲み物のカテゴリ) と風変わり液体で選択されますDelights します。


[![既定では、ドロップダウン リストの最初の項目が選択されています。](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**図 9**: 既定では、ドロップダウン リストで、最初の項目が選択されている ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image27.png))。


さらに、更新プログラムをクリックすると、見つかります製品の`CategoryID`と`SupplierID`値に設定されます`NULL`します。 動作が原因となったため、望ましくないこれらの両方で Dropdownlist、`EditItemTemplate`製品の基になるデータから、データのフィールドにバインドされていないことを s。

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Dropdownlist をバインド、`CategoryID`と`SupplierID`データ フィールド

適切な値と BLL に送信されるこれらの値を持つ、編集した製品のカテゴリと仕入先を確保するためにドロップダウン リストを設定`UpdateProduct`更新 をクリックすると、メソッド、Dropdownlist をバインドする必要があります`SelectedValue`プロパティを`CategoryID`と`SupplierID`双方向データ バインドを使用してデータ フィールド。 これを実現し、 `Categories` DropDownList を追加できます`SelectedValue='<%# Bind("CategoryID") %>'`宣言の構文に直接します。

または、デザイナーを使用してテンプレートを編集して DropDownList のスマート タグから DataBindings の編集リンクをクリックすると、DropDownList の databindings を設定できます。 次に、いることを示す、`SelectedValue`プロパティにバインドする必要があります、`CategoryID`双方向データ バインドを使用してフィールド (図 10 参照)。 バインドを宣言的またはデザイナーのプロセスを繰り返し、`SupplierID`のデータ フィールドを`Suppliers`DropDownList します。


[![CategoryID を双方向データ バインドを使用して、次のドロップダウン リストの SelectedValue プロパティにバインドします。](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**図 10**: バインド、 `CategoryID` DropDownList のする`SelectedValue`プロパティを使用して双方向データ バインド ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image30.png))。


バインドが適用されたら、 `SelectedValue` 2 つの Dropdownlist のプロパティを編集した製品のカテゴリと仕入先の列は既定で、現在の製品の値。 更新プログラムをクリックすると、`CategoryID`と`SupplierID`のドロップダウン リストを選択した項目の値に渡される、`UpdateProduct`メソッド。 データ バインド ステートメントを追加した後、図 11 は、チュートリアルを示しています。Chef Anton の Cajun Seasoning のドロップダウン リストを選択した項目が正しく Condiment とニューオーリンズ Cajun Delights 方法に注意してください。


[![編集の製品の現在のカテゴリと仕入先の値が既定で選択されています。](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**図 11**: The 編集製品の現在のカテゴリと仕入先の値が既定で選択されている ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image33.png))。


## <a name="handlingnullvalues"></a>処理`NULL`値

`CategoryID`と`SupplierID`内の列、`Products`テーブルできます`NULL`、まだで Dropdownlist、`EditItemTemplate`秒を表すリスト項目を含めないでください、`NULL`値。 これは、2 つの影響があります。

- ユーザーは、このインターフェイスを使用して以外から、製品のカテゴリまたは仕入先を変更することはできません`NULL`値を`NULL`いずれか
- 製品の場合、 `NULL` `CategoryID`または`SupplierID`、[編集] ボタンをクリックすると、例外が発生します。 ため、これは、`NULL`によって返される値`CategoryID`(または`SupplierID`) で、`Bind()`ステートメントは、DropDownList の値にマップされません (DropDownList 例外をスローするときにその`SelectedValue`値に設定されて*いない*リスト項目のコレクション)。

サポートするために`NULL``CategoryID`と`SupplierID`値、もう 1 つ追加する必要があります`ListItem`各 DropDownList を表すために、`NULL`値。 [マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)チュートリアル, に、追加する方法を説明しました`ListItem`DropDownList の設定が関係の databound DropDownList では、する`AppendDataBoundItems`プロパティを`True`と手動で追加する、追加`ListItem`します。 その前のチュートリアルでは、追加しました、`ListItem`で、`Value`の`-1`します。 ASP.NET では、データ バインドのロジックは、ただしは、空の文字列に自動的に変換されます、`NULL`値と、逆にします。 そのため、このチュートリアルではたい、`ListItem`の`Value`空の文字列にします。

開始するには、両方の Dropdownlist の`AppendDataBoundItems`プロパティを`True`します。 次に、追加、 `NULL` `ListItem` 、次を追加することで`<asp:ListItem>`など宣言型マークアップが表示されるように各 DropDownList 要素。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

選択して"(None)"を使用するテキスト値としてこの`ListItem`、希望される場合も空の文字列であることに変更することができます。

> [!NOTE]
> 説明したように、*マスター/詳細のフィルター処理で、DropDownList*チュートリアルでは、 `ListItem` s は、DropDownList をクリックして、デザイナーでの DropDownList に追加できる`Items`プロパティ ウィンドウでプロパティ (表示されます、`ListItem`コレクション エディター)。 ただし、追加することを確認する、 `NULL` `ListItem`このチュートリアルでは、宣言型構文を使用します。 使用する場合、`ListItem`コレクション エディターでは、生成された宣言型構文を省略は、`Value`のような宣言型マークアップを作成する、空の文字列が割り当てられている場合に、設定全体:`<asp:ListItem>(None)</asp:ListItem>`します。 これは、無害に見えるかもしれませんが、欠損値が、使用する DropDownList、`Text`代わりにプロパティ値。 このことを意味`NULL``ListItem`は選択すると、"(None)"値が試行されますに割り当てられる、 `CategoryID`、例外が発生します。 明示的に設定して`Value=""`、`NULL`に値が割り当てられる`CategoryID`ときに、 `NULL` `ListItem`が選択されています。


サプライヤーの DropDownList の次の手順を繰り返します。

この追加`ListItem`、編集インターフェイスに割り当てることができますようになりました`NULL`値を製品の`CategoryID`と`SupplierID`フィールド、図 12 に示すようにします。


[![製品のカテゴリまたは仕入先の NULL 値を割り当てる (なし)](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**図 12**: に割り当てる [(なし)、`NULL`製品のカテゴリまたは仕入先の値 ([フルサイズの画像を表示する] をクリックします](customizing-the-data-modification-interface-vb/_static/image36.png))。


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>手順 4: 提供が中止された状態のオプション ボタンを使用します。

現在、製品の`Discontinued`CheckBoxField では、読み取り専用の行のチェック ボックスが無効になっていると編集されている行を有効になっているチェック ボックスをオンの表示を使用してデータ フィールドが表されます。 このユーザー インターフェイスは多くの場合に適していますが、TemplateField を使用して必要な場合はカスタマイズできます。 このチュートリアルでは、変更、CheckBoxField を TemplateField RadioButtonList コントロールを「アクティブ」と「廃止」の 2 つのオプションを使用しているユーザーが製品を指定できます`Discontinued`値。

変換することで開始、`Discontinued`を TemplateField を作成し、TemplateField に CheckBoxField、`ItemTemplate`と`EditItemTemplate`します。 両方のテンプレートのチェック ボックスは、その`Checked`プロパティにバインドされて、`Discontinued`データ フィールドを 2 つの間の唯一の違い、`ItemTemplate`のチェック ボックスの`Enabled`プロパティに設定されて`False`します。

両方のチェック ボックスを置き換える、`ItemTemplate`と`EditItemTemplate`RadioButtonList コントロールでは、設定の両方 RadioButtonLists の`ID`プロパティを`DiscontinuedChoice`します。 次を示す、RadioButtonLists する必要がありますが含まれる 2 つのラジオ ボタン、1 つのラベルの付いた「アクティブ」値"False"に「廃止」というラベルの付いた 1 つの値は"True"です。 入力するか、これを実現する、`<asp:ListItem>`内の要素の宣言構文または使用から直接、`ListItem`デザイナーからコレクション エディター。 図 13 では、`ListItem`コレクション エディターの後、2 つのラジオ ボタンのオプションが指定されています。


[![RadioButtonList にアクティブで廃止されたオプションを追加します。](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**図 13**: 追加のアクティブなとオプションの提供が中止された、RadioButtonList ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image39.png))。


以降で RadioButtonList、`ItemTemplate`編集可能にすることはできません設定その`Enabled`プロパティを`False`したまま、`Enabled`プロパティを`True`(既定値) で RadioButtonList の`EditItemTemplate`します。 これにより、読み取り専用で非編集行でラジオ ボタンが作成されますが、編集された行のオプション ボタンの値を変更する許可。

RadioButtonList コントロールを割り当てる必要があります`SelectedValue`プロパティ、該当するラジオ ボタンが選択されるように基づく、製品の`Discontinued`データ フィールド。 このチュートリアルで前に検査 Dropdownlist とこのデータ バインド構文か、または追加できます宣言型マークアップに直接 RadioButtonLists のスマート タグで DataBindings の編集リンクを使用します。

2 つの RadioButtonLists を追加して、それらを構成したら、 `Discontinued` TemplateField の宣言型マークアップのようになります。


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

これらの変更により、 `Discontinued` (図 14 を参照してください) ラジオ ボタンのペアのリストにチェック ボックスの一覧から列を変換されました。 該当するラジオ ボタンが選択されている製品を編集するときと、他のオプション ボタンを選択し、[更新] をクリックして、製品の提供が中止された状態を更新できます。


[![提供が中止されたチェック ボックスがラジオ ボタンの組み合わせに置き換えられています。](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**図 14**:、廃止されたチェック ボックスをオンに置き換えられているラジオ ボタンのペア ([フルサイズの画像を表示する をクリックします](customizing-the-data-modification-interface-vb/_static/image42.png))。


> [!NOTE]
> 以降、`Discontinued`内の列、`Products`データベースことはできません`NULL`値必要はありませんキャプチャについて心配する`NULL`インターフェイスの情報。 場合、ただし、`Discontinued`列を含めたり`NULL`値は 3 つ目を追加するラジオ ボタンを使用してリストにその`Value`空の文字列に設定 (`Value=""`) category と supplier Dropdownlist のと同様、します。


## <a name="summary"></a>まとめ

BoundField と CheckBoxField 読み取り専用、編集、および挿入インターフェイスを自動的にレンダリングして、中に、カスタマイズ機能が不足しています。 多くの場合、ただし、必要があります、編集、またはインターフェイスの挿入をカスタマイズする (前のチュートリアルで説明した) のように、検証コントロールを追加するか (このチュートリアルで説明した) のようにデータ コレクションのユーザー インターフェイスをカスタマイズしています。 TemplateField にインターフェイスをカスタマイズする自問することで、次の手順。

1. TemplateField を追加または既存の BoundField または CheckBoxField を TemplateField に変換
2. 必要に応じて、インターフェイスを強化します。
3. 適切なデータ フィールドを双方向データ バインドを使用して、新しく追加された Web コントロールにバインドします。

組み込みの ASP.NET Web コントロールを使用するだけでなく、コンパイル済みのカスタム サーバー コントロールおよびユーザー コントロールを TemplateField のテンプレートをカスタマイズすることもできます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [次へ](implementing-optimistic-concurrency-vb.md)
