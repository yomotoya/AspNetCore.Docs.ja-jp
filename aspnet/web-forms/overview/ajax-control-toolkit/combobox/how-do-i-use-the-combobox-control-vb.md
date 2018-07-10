---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: コンボ ボックス コントロールの使用方法 (VB) |Microsoft Docs
author: microsoft
description: コンボ ボックスは、ユーザーが選択できるオプションの一覧と、テキスト ボックスの柔軟性を組み合わせた ASP.NET AJAX コントロールです。
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 745871b7e8cca14b3458d9004d581dfacf55ea24
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837725"
---
<a name="how-do-i-use-the-combobox-control-vb"></a>コンボ ボックス コントロールの使用方法 (VB)
====================
によって[Microsoft](https://github.com/microsoft)

> コンボ ボックスは、ユーザーが選択できるオプションの一覧と、テキスト ボックスの柔軟性を組み合わせた ASP.NET AJAX コントロールです。


このチュートリアルの目的では、AJAX Control Toolkit のコンボ ボックス コントロールについて説明します。 コンボ ボックスは、標準の ASP.NET DropDownList コントロールと TextBox コントロールの組み合わせのように動作します。 既存項目の一覧から選択するか、新しい項目を入力できます。

コンボ ボックスは、オートコンプリート コントロール エクステンダーに似ていますが、さまざまなシナリオで、コントロールを使用します。 AutoComplete エクステンダーでは、一致するエントリを取得する web サービスを照会します。 これに対し、コンボ ボックス コントロールは、一連の項目で初期化されます。 AutoComplete エクステンダーの使用を使用してコンボ ボックス コントロールを使用しているときに、多数のデータ (数百万の車の部分) を使用している場合意味データの小さなセットを使用する場合 (多数の車の部分)。

## <a name="selecting-from-a-static-list-of-items"></a>項目の静的リストから選択します。

S コンボ ボックス コントロールを使用して単純なサンプルを始めることができます。 ドロップダウン リストの項目の静的な一覧を表示することを想像してください。 ただし、リストが不完全で発生する可能性を開いたままにします。 一覧にカスタム値を入力するユーザーを許可するには。

Ll が新しい ASP.NET Web フォーム ページを作成し、ページで、コンボ ボックス コントロールを使用します。 新しい ASP.NET ページをプロジェクトに追加し、デザイン ビューに切り替えます。

ページで、コンボ ボックス コントロールを使用する場合は、ページに ScriptManager コントロールを追加する必要があります。 AJAX Extensions タブの下から、ScriptManager コントロールをデザイナー画面にドラッグします。 ページの上部にある、ScriptManager コントロールを追加する必要があります。開始サーバー側の下にすぐに追加できます&lt;フォーム&gt;タグ。

次に、コンボ ボックス コントロールをページにドラッグします。 その他の AJAX Control Toolkit のコントロールとコントロールのエクステンダー (図 1 を参照してください) で、ツールボックスにコンボ ボックス コントロールが表示されます。


[![ビジネスのカードを作成するための簡単なフォーム](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**図 01**: コンボ ボックス コントロールをツールボックスから選択 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image2.png))。


私たちの選択肢の静的な一覧を表示するコンボ ボックス コントロールを使用します。 ユーザーは、3 つの選択肢の一覧から、食品の spiciness の特定のレベルを選択できます: 軽度、メディア、およびホット (図 2 参照)。


[![項目の静的リストから選択します。](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**図 02**: 項目の静的な一覧から選択 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image4.png))。


コンボ ボックス コントロールにこれらの選択肢を追加するには、2 つの方法はあります。 まず、デザイン ビューでコントロールの上にマウス ポインターを置くオプションの編集タスク オプションを選択し、項目エディターを開きます (図 3 を参照してください)。


[![コンボ ボックス項目の編集](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**図 03**: 編集コンボ ボックス アイテム ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image6.png))。


2 番目のオプションは、開始タグと終了の間にある項目の一覧を追加することです。 &lt;asp: コンボ ボックス&gt;ソース ビュー内のタグ。 リスト 1 で、ページには、項目の一覧を含む更新された ComboBox が含まれています。

**1 - Static.aspx を一覧表示します。**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

リスト 1 で、ページを開くと、コンボ ボックスから既存のオプションのいずれかを選択できます。 つまり、コンボ ボックスは、DropDownList コントロールと同じように動作します。

ただし、既存のリストに含まれていない新しい選択肢 (たとえば、スーパー スパイシー) を入力するオプションもあります。 そのため、コンボ ボックスは、TextBox コントロールのようにも機能します。

既存を選択するかどうかに関係なく項目またはするカスタム項目をとき、入力フォームを送信すると、選択したラベル コントロールに表示されます。 BtnSubmit、フォームを送信するとき\_ハンドラーを実行し、ラベルを更新 をクリックします (図 4 参照)。


[![選択した項目を表示します。](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**図 04**: 選択した項目を表示する ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image8.png))。


コンボ ボックスには、フォームが送信された後に、選択した項目を取得するための DropDownList コントロールと同じプロパティがサポートされています。

- SelectedItem.Text - には、選択した項目の Text プロパティの値が表示されます。
- SelectedItem.Value - は、選択した項目の Value プロパティの値を表示またはコンボ ボックスに入力されたテキストが表示されます。
- SelectedValue - このプロパティを使用する既定の (初期) 選択した項目を指定する点を除いて SelectedItem.Value と同じです。

入力した場合、カスタム選択し、コンボ ボックスにカスタムの選択肢は SelectedItem.Text と SelectedItem.Value の両方のプロパティに割り当てられます。

## <a name="selecting-the-list-of-items-from-the-database"></a>データベースから項目の一覧を選択します。

コンボ ボックスを表示する項目の一覧は、データベースから取得できます。 たとえば、SqlDataSource コントロール、ObjectDataSource コントロールの LinqDataSource または、EntityDataSource コンボ ボックスをバインドできます。

コンボ ボックスで、ムービーの一覧を表示することを想像してください。 映画データベース テーブルからムービーの一覧を取得するには。 この場合は、以下の手順に従ってください。

1. Movies.aspx という名前のページを作成します。
2. ページには、ツールボックスで、[AJAX Extensions] タブの下から、scriptmanager コントロールをドラッグして、ページに ScriptManager コントロールを追加します。
3. ページに、ページに、コンボ ボックスをドラッグして、コンボ ボックス コントロールを追加します。
4. デザイン ビューでは、コンボ ボックス コントロールの上にマウスを移動し、選択、**データ ソースの選択**タスク オプション (図 5 を参照してください)。 データ ソース構成ウィザードを起動します。
5. **データ ソースの選択**手順で、&lt;新しいデータ ソース&gt;オプション。
6. **データ ソースの種類を選択**ステップで、データベースを選択します。
7. **データ接続の選択**ステップで、データベース (たとえば、MoviesDB.mdf) を選択します。
8. **アプリケーション構成ファイルへの接続文字列を保存**ステップで、接続文字列を保存するオプションを選択します。
9. **の Select ステートメントを構成する**ステップ、映画データベース テーブルを選択し、すべての列を選択します。
10. **テスト クエリ**ステップで、[完了] ボタンをクリックします。
11. 戻り、**データ ソースの選択**手順で、表示するフィールドのタイトルの列とデータの Id 列のフィールド (図を参照してください) を選択します。
12. ウィザードを閉じる [ok] ボタンをクリックします。


[![データ ソースの選択](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**図 05**: データ ソースの選択 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image10.png))。


[![データのテキストと値のフィールドの選択](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**図 06**: データのテキストと値のフィールドの選択 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image12.png))。


上記の手順を完了すると、コンボ ボックスは、映画データベース テーブルから、ムービーを表す、SqlDataSource コントロールにバインドされます。 ページのソースは、リスト 2 が (私はもう少し書式をクリーンアップ) のようになります。

**2 - Movies.aspx を一覧表示します。**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

コンボ ボックス コントロールに SqlDataSource コントロールを指す DataSourceID プロパティがあることに注意してください。 ブラウザーでページを開くと、データベースからムービーの一覧が表示されます (図 7 を参照してください)。 いずれかを選択、一覧からビデオを実行できますか、コンボ ボックスに、ムービーを入力して、新しいムービーを入力します。


[![ムービーの一覧を表示します。](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**図 07**: ムービーの一覧を表示する ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image14.png))。


## <a name="setting-the-dropdownstyle"></a>設定、DropDownStyle

コンボ ボックス DropDownStyle プロパティを使用して、コンボ ボックスの動作を変更することができます。 このプロパティではある使用可能な値。

- ドロップダウン - (既定値)、コンボ ボックスを表示して、矢印をクリックすると、ドロップダウン ボックスの一覧は、カスタム値を入力することができます。
- シンプル - コンボ ボックスが自動的にドロップダウン リストを表示し、カスタム値を入力することができます。
- DropDownList のコンボ ボックスを DropDownList コントロールと同様に動作します。

項目の一覧が表示される場合は、さまざまなドロップダウン間、および単純なのです。 単純な場合は、コンボ ボックスにフォーカスを移動するとすぐに一覧が表示されます。 ドロップダウン リストの場合は、項目の一覧を表示する矢印をクリックする必要があります。

DropDownList の値により、標準の DropDownList コントロールと同様に動作するコンボ ボックス コントロール。 ただし、ここで重要な違いがあります。 以前のバージョンの Internet Explorer は、その前に配置される、コントロールの前に、コントロールに表示されます、無限の z インデックスを持つ DropDownList コントロールを表示します。 コンボ ボックス、HTML をレンダリングするため、 &lt;div&gt;タグ、HTML ではなく&lt;選択&gt;タグ、コンボ ボックス正しく尊重 z オーダーします。

## <a name="setting-the-autocompletemode"></a>設定、AutoCompleteMode

コンボ ボックスの AutoCompleteMode プロパティを使用して、テキスト、コンボ ボックスに入力すると動作を指定します。 このプロパティは、次の値を受け取ります。

- None - (既定値)、コンボ ボックスは、オート コンプリートの動作を提供していません。
- 提案のコンボ ボックスに一覧が表示され、リストに一致する項目が強調表示されます (図 8 参照)。
- 追加し、コンボ ボックスで、一覧が表示されない - をリストから入力した内容が (図 9 参照) に一致する項目を追加します。
- パネルのコンボ ボックス両方は、一覧が表示されをリストから入力した内容が (図 10 参照) に一致する項目を追加します。


[![コンボ ボックスは、修正案](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**図 08**: コンボ ボックスは、提案 ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image16.png))。


[![コンボ ボックスは、一致するテキストを追加します。](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**図 09**: コンボ ボックスは、一致するテキストを追加します ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image18.png))。


[![コンボ ボックスの内容が提示され、追加します](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**図 10**: コンボ ボックスが提示され、追加します ([フルサイズの画像を表示する をクリックします](how-do-i-use-the-combobox-control-vb/_static/image20.png))。


## <a name="summary"></a>まとめ

このチュートリアルでは、コンボ ボックス コントロールを使用して、項目の固定セットを表示する方法について説明しました。 バインドされた項目の設定を静的なデータベース テーブルに両方のコンボ ボックス コントロール。 最後に、その DropDownStyle と AutoCompleteMode プロパティを設定して、コンボ ボックスの動作を変更する方法を学習しました。

> [!div class="step-by-step"]
> [前へ](how-do-i-use-the-combobox-control-cs.md)
