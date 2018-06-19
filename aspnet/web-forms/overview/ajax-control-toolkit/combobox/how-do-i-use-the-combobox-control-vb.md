---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: ComboBox コントロールの使用方法 (VB) |Microsoft ドキュメント
author: microsoft
description: コンボ ボックスは、ユーザーが選択できるオプションの一覧で、テキスト ボックスの柔軟性を結合する ASP.NET AJAX コントロールです。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e42844e326cb190502a51c5a85195b4752d7e827
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875410"
---
<a name="how-do-i-use-the-combobox-control-vb"></a>ComboBox コントロールの使用方法 (VB)
====================
によって[Microsoft](https://github.com/microsoft)

> コンボ ボックスは、ユーザーが選択できるオプションの一覧で、テキスト ボックスの柔軟性を結合する ASP.NET AJAX コントロールです。


このチュートリアルの目的では、AJAX コントロール Toolkit コンボ ボックス コントロールをについて説明します。 コンボ ボックスは、標準の ASP.NET DropDownList コントロールとテキスト ボックス コントロールの間で組み合わせと同様に動作します。 既存のアイテムの一覧から選択するか、新しい項目を入力します。

コンボ ボックスにオートコンプリート コントロール エクステンダーに似ていますが、さまざまなシナリオで、コントロールを使用します。 オートコンプリート extender は、一致するエントリを取得する web サービスを照会します。 これに対し、コンボ ボックス コントロールは、一連の項目で初期化されます。 オートコンプリートの extender を使用して意味コンボ ボックス コントロールを使用しているときに大きなデータ (数百万の自動車) セットを使用しているときに意味データの小さなセットを使用する場合 (数十個の車の部分)。

## <a name="selecting-from-a-static-list-of-items"></a>項目の静的な一覧から選択します。

S コンボ ボックス コントロールを使用する単純なサンプルを使用できます。 ドロップダウン リストで項目の静的な一覧を表示することを想像してください。 ただし、する一覧が不完全で発生する可能性を開いたままにしておきます。 ユーザーが一覧にカスタム値を入力できるようにするには。

お ll 新しい ASP.NET Web フォーム ページを作成し、ページのコンボ ボックス コントロールを使用します。 プロジェクトに新しい ASP.NET ページを追加し、[デザイン] ビューに切り替えます。

ページのコンボ ボックス コントロールを使用する場合は、ページに ScriptManager コントロールを追加する必要があります。 [AJAX Extensions] タブの下から ScriptManager コントロールをデザイナー画面にドラッグします。 ページの上部にある ScriptManager コントロールを追加する必要があります。開くサーバー側の下にすぐに追加できます&lt;フォーム&gt;タグ。

次に、コンボ ボックス コントロールをページにドラッグします。 コンボ ボックス コントロールは、他の AJAX コントロール Toolkit コントロールおよびコントロール エクステンダー (図 1 を参照してください) を使用して、ツールボックスで参照できます。


[![名刺を作成するための簡単なフォーム](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**図 01**: コンボ ボックス コントロールをツールボックスから選択する ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image2.png))


私たちの選択肢の静的な一覧を表示するコンボ ボックス コントロールを使用します。 ユーザーは、次の 3 つの選択肢の一覧から、食品の spiciness の特定のレベルを選択できます: 中性、Medium、およびホット (図 2 を参照してください)。


[![項目の静的な一覧から選択します。](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**図 02**: アイテムの静的な一覧から選択する ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image4.png))


2 つの方法がコンボ ボックス コントロールをこれらのオプションを追加することができます。 まず、デザイン ビューでコントロールにマウス ポインターを置いたときにオプションの編集タスク オプションを選択し、項目エディターを開きます (図 3 を参照してください)。


[![コンボ ボックス項目の編集](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**図 03**: コンボ ボックスの編集項目 ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image6.png))


2 番目のオプションは、開始タグと終了の間の項目のリストを追加する&lt;asp: ComboBox&gt;ソース ビュー内のタグ。 1 のリスト内のページには、項目の一覧を含む更新されたコンボ ボックスが含まれています。

**1 - Static.aspx を一覧表示します。**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

1 の一覧で、ページを開くときに、コンボ ボックスから既存のオプションのいずれかを選択できます。 つまり、コンボ ボックスは、DropDownList コントロールと同じように動作します。

ただし、既存のリストに含まれていない新しい選択肢 (たとえば、スーパー スパイシー) を入力するオプションもあります。 そのため、コンボ ボックスは、テキスト ボックス コントロールと同様にでも動作します。

既存を選択するかどうかに関係なく項目またはカスタム項目とき入力フォームを送信すると、選択したラベル コントロールに表示されます。 BtnSubmit、フォームを送信するとき\_クリック ハンドラーを実行し、ラベルを更新します (図 4 を参照してください)。


[![選択した項目を表示します。](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**図 04**: 選択した項目を表示する ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image8.png))


コンボ ボックスには、フォームが送信されると、選択した項目を取得するための DropDownList コントロールと同じプロパティがサポートされています。

- SelectedItem.Text - には、選択した項目のテキスト プロパティの値が表示されます。
- SelectedItem.Value - は、選択したアイテムの Value プロパティの値を表示またはコンボ ボックスに入力したテキストを表示します。
- SelectedValue - SelectedItem.Value としてこのプロパティを使用する既定の (初期) 選択した項目を指定する点を除いて同じです。

入力した場合、コンボ ボックスの ユーザー設定を選択し、カスタムの選択肢が SelectedItem.Text と SelectedItem.Value の両方のプロパティに割り当てられます。

## <a name="selecting-the-list-of-items-from-the-database"></a>データベースからアイテムの一覧を選択します。

コンボ ボックスを表示する項目の一覧は、データベースから取得できます。 たとえば、SqlDataSource コントロール、ObjectDataSource コントロール、LinqDataSource または、EntityDataSource ComboBox をバインドできます。

コンボ ボックスで、ムービーの一覧を表示することを想像してください。 映画のデータベース テーブルからムービーの一覧を取得するには。 この場合は、以下の手順に従ってください。

1. Movies.aspx をという名前のページを作成します。
2. ページには、ツールボックスで、[AJAX Extensions] タブの下には、ScriptManager をドラッグして、ページに ScriptManager コントロールを追加します。
3. ページに、ページに、コンボ ボックスをドラッグして、コンボ ボックス コントロールを追加します。
4. デザイン ビューで、マウス ポインターを移動、コンボ ボックス コントロールを選択、**データ ソースの選択**オプションのタスク (図 5 を参照してください)。 データ ソース構成ウィザードが起動します。
5. **データ ソースを選択**手順、select、&lt;新しいデータ ソース&gt;オプション。
6. **データ ソースの種類を選択して**ステップでデータベースを選択します。
7. **データ接続の選択**ステップで、データベース (たとえば、MoviesDB.mdf) を選択します。
8. **アプリケーション構成ファイルへの接続文字列を保存**ステップで、接続文字列を保存するオプションを選択します。
9. **、Select ステートメントを構成する**手順、映画データベース テーブルを選択、およびすべての列を選択します。
10. **テスト クエリ**ステップ、[完了] ボタンをクリックします。
11. 戻り、**データ ソースの選択**手順で、タイトルを表示するフィールドと、Id 列のデータのフィールド (図を参照してください) を選択します。
12. ウィザードを閉じて、[OK] をクリックします。


[![データ ソースの選択](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**図 05**: データ ソースの選択 ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image10.png))


[![データのテキストと値のフィールドの選択](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**図 06**: データのテキストと値のフィールドの選択 ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image12.png))


上記の手順を完了すると、コンボ ボックス、映画データベース テーブルから、ムービーを表す SqlDataSource コントロールにバインドされます。 ページのソースは、(I は、書式設定、もう少しにクリーンアップ) を一覧表示する 2 のようになります。

**2 - Movies.aspx を一覧表示します。**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

コンボ ボックス コントロールが SqlDataSource コントロールを指す DataSourceID プロパティを持つことに注意してください。 ブラウザーでページを開くと、データベースからムービーの一覧が表示されます (図 7 を参照してください)。 いずれか、選択リストからムービーを作成することができますか、コンボ ボックスに、ムービーを入力し、新しいムービーを入力します。


[![ムービーの一覧を表示します。](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**図 07**: ムービーの一覧を表示する ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>DropDownStyle の設定

コンボ ボックス DropDownStyle プロパティを使用して、コンボ ボックスの動作を変更することができます。 このプロパティではある使用可能な値。

- ドロップダウン リストに (既定値)、コンボ ボックスを表示して、矢印をクリックすると、ドロップダウン リスト ボックスの一覧は、カスタム値を入力することができます。
- 単純な - ComboBox がドロップダウン リストを自動的に表示し、カスタム値を入力することができます。
- DropDownList のコンボ ボックスは、DropDownList コントロールと同じように動作します。

項目の一覧が表示されるときに、さまざまなドロップダウン リストの間で単純なのです。 単純な場合は、コンボ ボックスにフォーカスを移動するとすぐに一覧が表示されます。 ドロップダウン リストの場合は、項目の一覧を表示する矢印をクリックする必要があります。

DropDownList 値では、コンボ ボックス コントロールを標準の DropDownList コントロールと同じように動作が発生します。 ただし、これにはここで重要な違いがあります。 古いバージョンの Internet Explorer の DropDownList コントロールを表示する無限の z インデックスを持つため、その前に記述される任意のコントロールの前に、コントロールが表示されます。 コンボ ボックスに、HTML 表示ため&lt;div&gt;タグ、HTML ではなく&lt;選択&gt;タグ、コンボ ボックス正しく尊重 z オーダーです。

## <a name="setting-the-autocompletemode"></a>設定、AutoCompleteMode

ComboBox AutoCompleteMode プロパティを使用して、テキストのコンボ ボックスに入力すると動作を指定します。 このプロパティは、次の値を指定します。

- None - (既定値)、コンボ ボックスでは、オート コンプリートの動作が提供されません。
- 提案の一覧を表示するコンボ ボックスとリストに一致する項目を強調表示 (図 8 を参照してください)。
- 追加 - コンボ ボックスに一覧表示されませんし、リストから入力した (図 9 を参照してください) に一致する項目を追加します。
- SuggestAppend - コンボ ボックスは、一覧が表示されます両方をリストから入力した (図 10 参照) に一致する項目を追加します。


[![コンボ ボックスは、修正案](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**図 08**: コンボ ボックスは、修正案 ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image16.png))


[![コンボ ボックスは、一致するテキストを追加します。](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**図 09**: ComboBox が一致するテキストを追加 ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image18.png))


[![ComboBox を提案し、追加](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**図 10**: コンボ ボックスを提案し、追加 ([フルサイズのイメージを表示するをクリックして](how-do-i-use-the-combobox-control-vb/_static/image20.png))


## <a name="summary"></a>まとめ

このチュートリアルでは、コンボ ボックス コントロールを使用して項目の固定セットを表示する方法について学習しました。 ComboBox コントロールの両方を項目の設定を静的とデータベース テーブルにバインドされました。 最後に、その DropDownStyle、AutoCompleteMode プロパティを設定して、コンボ ボックスの動作を変更する方法を学習します。

> [!div class="step-by-step"]
> [前へ](how-do-i-use-the-combobox-control-cs.md)
