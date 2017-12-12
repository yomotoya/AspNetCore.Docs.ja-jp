---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: "GridView コントロール (c#) で TemplateFields の使用 |Microsoft ドキュメント"
author: rick-anderson
description: "柔軟性を提供するには、GridView は、テンプレートを使用してレンダリングされると、TemplateField を提供します。 テンプレートは、静的な HTML、Web コントロールの組み合わせを含めることができますとしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 4202f25b241a6ca115c1ffc0a80258ee96563f72
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-templatefields-in-the-gridview-control-c"></a>GridView コントロール (c#) で TemplateFields の使用
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe)または[PDF のダウンロード](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> 柔軟性を提供するには、GridView は、テンプレートを使用してレンダリングされると、TemplateField を提供します。 テンプレートは、ミックスを含めることができます、静的な HTML の Web コントロール、およびデータ バインド構文です。 このチュートリアルでは、TemplateField を使用して、多くの GridView コントロールを使用したカスタマイズを実現する方法について確認します。


## <a name="introduction"></a>はじめに

GridView からどのようなプロパティを示すフィールドのセットで構成されます、`DataSource`データの表示方法と表示される出力に含めるは。 最も単純なフィールド型は、データ値をテキストとして表示すると、BoundField です。 その他のフィールドの種類は、代替の HTML 要素を使用してデータを表示します。 たとえば、CheckBoxField がチェックされた状態は、指定したデータ フィールドの値によって異なります。 チェック ボックスとして表示されます。ImageField がイメージ ソースを指定したデータ フィールドに基づくイメージをレンダリングします。 ハイパーリンクと状態は、基になるデータ フィールドの値によって異なります。 ボタンは、内と ButtonField フィールドの種類を使用して表示できます。

CheckBoxField、ImageField、内、および ButtonField フィールドの種類は、データの代替を表示できるようにする、まだかなり制限されることはに関して、書式設定します。 ImageField は 1 つのイメージのみを表示できますが、CheckBoxField のみ単一のチェック ボックスを表示できます。 特定のフィールドがいくつかのテキストのチェック ボックスを表示する必要がある場合*と*イメージを別のデータ フィールドの値に基づいてすべてしますか? または、チェック ボックス、イメージ、ハイパーリンク、またはボタン以外の Web コントロールを使用してデータを表示する場合ですか。 さらに、BoundField は、1 つのデータ フィールドに表示を制限します。 1 つの GridView 列に 2 つ以上のデータ フィールドの値を表示する場合はどうでしょうか。

使用してレンダリングされると、TemplateField を提供する GridView にこのレベルの柔軟性を合わせて、*テンプレート*です。 テンプレートは、ミックスを含めることができます、静的な HTML の Web コントロール、およびデータ バインド構文です。 さらに、TemplateField には、さまざまなさまざまな状況の表示をカスタマイズするために使用できるテンプレートがあります。 たとえば、`ItemTemplate`が既定で、各行のセルを表示するために使用しますが、`EditItemTemplate`インターフェイスをカスタマイズする、データの編集時にテンプレートを使用できます。

このチュートリアルでは、TemplateField を使用して、多くの GridView コントロールを使用したカスタマイズを実現する方法について確認します。 [前のチュートリアル](custom-formatting-based-upon-data-cs.md)を使用して基になるデータに基づく書式をカスタマイズする方法を説明しました、`DataBound`と`RowDataBound`イベント ハンドラー。 基になるデータに基づく書式をカスタマイズする方法は呼び出すことによって書式指定メソッドから、テンプレート内で。 この手法にもこのチュートリアルで見ていきます。

このチュートリアルでは、従業員の一覧の外観をカスタマイズするのに、TemplateFields を使用します。 具体的には、おの従業員のすべてを一覧表示されますは表示従業員の姓と名 1 つの列で、雇用された日付、予定表コントロールで、[状態] 列に何日間したを採用する企業のことを示します。


[![次の 3 つ TemplateFields を使用して、表示をカスタマイズするには](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**図 1**: 次の 3 つ TemplateFields を使用して、表示をカスタマイズする ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>手順 1: GridView にデータのバインド

レポートの外観をカスタマイズする TemplateFields を使用する必要があるシナリオでは最も簡単なをまず BoundFields だけを含む GridView コントロールを作成することで開始して、新しい TemplateFields を追加するかを既存の BoundFields を変換するには必要に応じて TemplateFields です。 そのため、このチュートリアルから始めましょう GridView をデザイナーでのページに追加して、従業員の一覧を返す ObjectDataSource にバインドすること。 Employee フィールドの各 BoundFields で、次の手順は、GridView を作成します。

開く、`GridViewTemplateField.aspx`ページおよび GridView をツールボックスからデザイナーにドラッグします。 GridView のスマート タグからを起動する新しい ObjectDataSource コントロールを追加するを選択して、`EmployeesBLL`クラスの`GetEmployees()`メソッドです。


[![GetEmployees() メソッドを呼び出す新しい ObjectDataSource コントロールの追加します。](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**図 2**: その呼び出します新しい ObjectDataSource コントロールを追加、`GetEmployees()`メソッド ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


この方法で GridView にバインドが自動的に追加 BoundField の従業員のプロパティのそれぞれ: `EmployeeID`、 `LastName`、 `FirstName`、 `Title`、 `HireDate`、 `ReportsTo`、および`Country`です。 このレポートのみましょう気にならなければを表示すると、 `EmployeeID`、 `ReportsTo`、または`Country`プロパティです。 これらの BoundFields を削除するのには、次のことができます。

- GridView のスマート タグからの列の編集リンク フィールド ダイアログ ボックスをクリックを使用して、このダイアログ ボックスを表示します。 次に、左下から BoundFields ボックスの一覧し、赤い X 印をクリックして選択 ボタンをクリックして、BoundField を削除します。
- ソース ビューから GridView の宣言の構文を手動で編集、削除、 `<asp:BoundField>` BoundField を削除するための要素。

削除した後、 `EmployeeID`、 `ReportsTo`、および`Country`BoundFields、GridView のマークアップのようになります。


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

ブラウザーで作業の進行状況を表示するのにはしばらくを実行します。 この時点で、各従業員と 4 つの列のレコードを含むテーブルを表示する必要があります: 従業員の姓、名、ユーザー名前のいずれか、そのタイトルのいずれかおよび、雇用された日付の 1 つのいずれか。


[![各従業員の姓、FirstName、タイトル、および HireDate フィールドが表示されます。](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**図 3**: `LastName`、 `FirstName`、 `Title`、および`HireDate`各従業員のフィールドが表示されます ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>手順 2: 1 つの列に姓と名を表示します。

現時点では、各従業員の姓と別の列に姓が表示されます。 代わりに 1 つの列に結合する便利な場合があります。 これを実現するには、TemplateField を使用する必要があります。 できますか、新しい TemplateField を追加、必要なマークアップおよびデータ バインドの構文を追加し、削除お、`FirstName`と`LastName`BoundFields、またはおを変換できる、`FirstName`を TemplateField、BoundField を含める TemplateField の編集`LastName`値に設定して、削除、 `LastName` BoundField です。

どちらの方法でも net 結果は同じですが、個人ここからへの変換 BoundFields TemplateFields 可能な場合は、変換が自動的に追加されているため、`ItemTemplate`と`EditItemTemplate`Web コントロールと外観を模倣するためにデータ バインド構文使用し、BoundField の機能。 利点は、以下で作業を行う、TemplateField ように、変換プロセスが行った作業のいくつかご利用の米国お必要があります。

既存の BoundField を TemplateField に変換するには、[フィールド] ダイアログ ボックスを立ち上げて、GridView のスマート タグからの列の編集リンクをクリックします。 左下隅にある一覧から変換し、右下隅で"Convert このフィールドを TemplateField"リンクをクリックして BoundField を選択します。


[![BoundField をフィールド ダイアログ ボックスから TemplateField に変換します。](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**図 4**: フィールド ダイアログ ボックスから、TemplateField に BoundField を変換する ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


さあ、変換、`FirstName`を TemplateField BoundField です。 この変更後に、デザイナーでの立場に立っての違いはありません。 これは、BoundField のルック アンド フィールを維持する TemplateField を作成、BoundField を TemplateField に変換するためです。 デザイナーでビジュアルの違いをこの時点でされていない、そこに関係なくこの変換プロセスの BoundField の宣言構文は置き換え`<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />`- 次の TemplateField 構文で。


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

TemplateField から成る 2 つのテンプレートが表示されるよう、`ItemTemplate`ラベルを持つが`Text`の値に設定されて、`FirstName`データ フィールド、および`EditItemTemplate`いるコントロール、テキスト ボックスと`Text`でもプロパティが設定`FirstName`データ フィールドです。 Databinding 構文 - `<%# Bind("fieldName") %>` -ことを示しますデータ フィールド *`fieldName`*  Web コントロールの指定したプロパティにバインドされています。

追加する、`LastName`データ フィールドの値をこの TemplateField に別のラベルの Web コントロールを追加する必要があります、`ItemTemplate`バインドとその`Text`プロパティを`LastName`です。 これには、手動でまたはデザイナーを使用します。 手作業で行うと、単純にする適切な宣言の構文を追加、 `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

デザイナーを使用には、それを追加するには、GridView のスマート タグからのテンプレートの編集リンクをクリックします。 GridView のテンプレートを編集するインターフェイスが表示されます。 このインターフェイスのスマート タグが GridView でテンプレートの一覧を示します。 のみがあるため 1 つ TemplateField この時点で、ドロップダウン リストに含まれるのみのテンプレートは、これらのテンプレートの`FirstName`と共に TemplateField、`EmptyDataTemplate`と`PagerTemplate`です。 `EmptyDataTemplate`テンプレート、指定した場合は使用; GridView にバインドされたデータの結果がない場合、GridView の出力を表示するために、 `PagerTemplate`、指定した場合は、ページングをサポートする GridView のページングのインターフェイスを表示するために使用します。


[![GridView のテンプレートは、デザイナーで編集できます。](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**図 5**: GridView のテンプレートでく編集を通じて、デザイナー ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


表示する、`LastName`で、 `FirstName` TemplateField にツールボックスからラベル コントロールをドラッグして、 `FirstName` TemplateField の`ItemTemplate`gridview のテンプレートの編集インターフェイスです。


[![FirstName TemplateField の ItemTemplate にラベル Web コントロールを追加します。](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**図 6**: するラベル Web コントロールを追加、 `FirstName` TemplateField の ItemTemplate ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


この時点で、TemplateField に追加したラベル Web コントロールがその`Text`プロパティを"Label"に設定します。 このプロパティの値にバインドされるように変更する必要があります、`LastName`データ フィールドの代わりにします。 ラベル コントロールのスマート タグをクリックしてを実行して、databindings の編集 オプションを選択します。


[![ラベルのスマート タグから DataBindings の編集 オプションを選択します。](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**図 7**: ラベルのスマート タグから DataBindings の編集 オプションを選択 ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


[DataBindings] ダイアログ ボックスが表示されます。 ここからは、左側の一覧から、データ バインドに参加し、右側のドロップダウン リストからデータをバインドするフィールドを選択するプロパティを選択できます。 選択、 `Text` 、左からプロパティと`LastName`右側からフィールドし、[ok] をクリックします。


[![テキストのプロパティは LastName データ フィールドにバインドします。](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**図 8**: バインド、`Text`プロパティを`LastName`データ フィールド ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> [DataBindings] ダイアログ ボックスでは、双方向データ バインドを実行するかどうかを指定することができます。 オフの場合、指定しない場合、データ バインド構文`<%# Eval("LastName")%>`の代わりに使用されます`<%# Bind("LastName")%>`です。 いずれかの方法は、このチュートリアルでは正常です。 双方向データ バインドは、挿入して、データの編集時に重要になります。 データを表示するだけで、ただし、どちらの方法は動作も同じようにします。 今後のチュートリアルでは、双方向のデータ バインドの詳細について説明します。


ブラウザーからこのページを表示するのにはしばらくを実行します。 GridView がまだ 4 つの列を含むことがわかりますただし、`FirstName`に列が表示されます*両方*、`FirstName`と`LastName`データ フィールドの値。


[![1 つの列に、FirstName と LastName の値を示します](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**図 9**: 両方、`FirstName`と`LastName`値は 1 つの列に表示されます ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


この最初の手順を完了するには、削除、 `LastName` BoundField の名前を変更し、 `FirstName` TemplateField の`HeaderText`"Name"プロパティです。 これらの変更後、GridView の宣言型マークアップは、次のようになります。


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![各従業員の最初と最後の名前は、1 つの列に表示されます。](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**図 10**: 各従業員の最初と最後の名前は、1 つの列に表示されます ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>手順 3: を表示するカレンダー コントロールを使用して、`HiredDate`フィールド

データ フィールドの値を表示する GridView にテキストとしては、BoundField を使用するように単純です。 特定のシナリオでは、ただし、データが最適で表現単なるテキストではなく、特定の Web コントロールを使用しています。 データの表示には、このようなカスタマイズは、TemplateFields と可能性があります。 などのではなくテキストとして従業員の雇用された日付を表示するよりおでした、予定表を表示 (を使用して[予定表コントロール](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.calendar(VS.80).aspx))、雇用された日付を強調表示されます。

これを実現する、変換することで開始、`HiredDate`を TemplateField BoundField です。 GridView のスマート タグに移動し、[フィールド] ダイアログ ボックスを立ち上げて、列の編集リンクをクリックしてだけです。 選択、 `HiredDate` BoundField とクリック「変換このフィールドを TemplateField」。


[![HiredDate BoundField を TemplateField に変換します。](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**図 11**: 変換、`HiredDate`を TemplateField に BoundField ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


手順 2 で説明したとおり、このに置き換えられます、BoundField を含む TemplateField、`ItemTemplate`と`EditItemTemplate`ラベルとテキスト ボックスに持つ`Text`プロパティにバインドされます、 `HiredDate` databinding構文を使用して値`<%# Bind("HiredDate")%>`.

テキストを予定表コントロールで置き換えるには、ラベルを削除して、予定表コントロールを追加して、テンプレートを編集します。 デザイナーで、GridView のスマート タグからテンプレートの編集を選択し、選択、 `HireDate` TemplateField の`ItemTemplate`ドロップダウン リストからです。 次に、ラベル コントロールを削除し、カレンダー コントロールをツールボックスから、テンプレートの編集インターフェイスをドラッグします。


[![カレンダー コントロールを追加、TemplateField の ItemTemplate を入社日](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**図 12**: するカレンダー コントロールを追加、 `HireDate` TemplateField の`ItemTemplate`([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


この時点で GridView の行ごとには、予定表コントロールでその`HiredDate`TemplateField です。 ただし、従業員の実際の`HiredDate`値が設定されていない任意の場所の予定表コントロールで、既定値は、現在の月と日付を表示するには、各カレンダー コントロールが原因です。 この問題を解決する必要がありますに割り当てる各従業員の`HiredDate`を予定表コントロールの[SelectedDate](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx)と[VisibleDate](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx)プロパティです。

予定表コントロールのスマート タグから databindings の編集 を選択します。 次に、両方にバインド`SelectedDate`と`VisibleDate`プロパティを`HiredDate`データ フィールドです。


[![SelectedDate と VisibleDate プロパティ HiredDate データ フィールドにバインドします。](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**図 13**: バインド、`SelectedDate`と`VisibleDate`プロパティを`HiredDate`データ フィールド ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> 予定表コントロールの日付の選択されている必要がある必ずしも表示されません。 たとえば、カレンダーあります年 8 月 1日<sup>st</sup>、選択した日付として 1999 が、現在の月と年に表示されます。 選択した日付と日付の表示が、予定表コントロールの指定された`SelectedDate`と`VisibleDate`プロパティです。 従業員の両方を選択したいので`HiredDate`に表示されているこれらのプロパティにバインドする必要があるかどうかを確認し、`HireDate`データ フィールドです。


ブラウザーでページを表示するときに、カレンダーは今すぐ従業員の採用された日付の月を表示し、特定の日付を選択します。


[![従業員の HiredDate が予定表コントロールで表示されます。](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**図 14**:、従業員の`HiredDate`カレンダー コントロールに表示されます ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> これまで見てきたきたすべての例とは対照的このチュートリアルで行った*いない*設定`EnableViewState`プロパティを`false`この GridView にします。 この決定の理由、ポストバックでクリックされた日付をカレンダーの選択した日付を設定する予定表コントロールの日付をクリックすると、ためにです。 GridView のビュー ステートが無効になっている場合、ポストバックのたびに GridView のデータが再バインド、カレンダーに選択した日付を設定すると、基になるデータ ソースの*戻る*、従業員の`HireDate`、上書きします。ユーザーが選択した日付。


このチュートリアルのこれは、議論については、ユーザーは、従業員を更新することではないため`HireDate`です。 その日が選択できるようにするために、カレンダー コントロールを構成する最適な場合がでしょう。 関係なく、このチュートリアルではいくつかの状況でビュー ステート必要がありますを有効にする特定の機能を提供するためにします。

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>手順 4: 従業員の日数を示す、会社の働きました

これまでに 2 つのアプリケーション TemplateFields のこれまで見てきた。

- 2 つ以上のデータ フィールドの値を 1 つの列に結合し、
- テキストではなく、Web コントロールを使用してデータ フィールドの値を表現します。

TemplateFields の 3 番目の使用が GridView のに関するメタデータを表示するために基になるデータ。 表示に加えて、従業員の入社日などの可能性があることも、ジョブにした数の合計日数を表示する列があります。

まだ TemplateFields の別の使用は、基になるデータが格納された形式でデータベースよりも web ページのレポートに異なる方法で表示する必要がある場合のシナリオで発生します。 想像してください、`Employees`テーブルが、`Gender`文字を格納するフィールド`M`または`F`従業員の性別を示すためにします。 Web ページでこの情報を表示するときに"Male"または"Female"のいずれかとして、"M"または"F"だけではなく、性別を表示したい場合があります。

これら両方のシナリオを作成して処理できる、*書式指定メソッド*ASP.NET ページの分離コード クラスで (またはとして実装された別のクラス ライブラリで、`static`メソッド)、テンプレートから呼び出されます。 このような書式指定メソッドが、先ほど説明のデータ バインドと同じ構文を使用してテンプレートから呼び出されます。 書式設定メソッドは、任意の数に、パラメーターのかかることができますが、文字列を返す必要があります。 この返される文字列は、テンプレートに挿入される HTML です。

この概念を示すためには、従業員したジョブの日数の合計数を一覧表示する列を表示するには、このチュートリアルを拡張してみましょう。 この書式指定メソッドになります、`Northwind.EmployeesRow`オブジェクトを文字列として、従業員が採用されているされた日数の数を返します。 このメソッドは、ASP.NET ページの分離コード クラスに追加できますが、*必要があります*としてマークする`protected`または`public`テンプレートからアクセスできるようにするためにします。


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

以降、`HiredDate`フィールドを含めることができます`NULL`、値がないことを確認する必要があります最初の値をデータベース`NULL`計算式を続行する前にします。 場合、`HiredDate`値は`NULL`、ない場合は単に文字列"Unknown"です。 を返すお`NULL`、現在の時刻の差を計算し、`HiredDate`値および日数の数を返します。

このメソッドを使用するには、データ バインドの構文を使用した GridView で TemplateField から呼び出すことが必要です。 GridView のスマート タグの列の編集リンクをクリックし、新しい TemplateField を追加することによって GridView に新しい TemplateField を追加することで開始します。


[![GridView に新しい TemplateField を追加します。](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**図 15**: GridView に新しい TemplateField を追加 ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


この新しい TemplateField 設定`HeaderText`プロパティを「日にジョブ」およびその`ItemStyle`の`HorizontalAlign`プロパティを`Center`です。 呼び出す、 `DisplayDaysOnJob` 、テンプレートからメソッドを追加、`ItemTemplate`し、次のデータ バインド構文を使用します。


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem`返します、`DataRowView`オブジェクトに対応する、`DataSource`にレコードがバインドされている、`GridViewRow`です。 その`Row`プロパティは、厳密に型指定されたを返します`Northwind.EmployeesRow`に渡される、`DisplayDaysOnJob`メソッドです。 このデータ バインド構文で直接表示されることができます、`ItemTemplate`に示すように次の宣言の構文) を割り当てることができるか、 `Text` Label Web コントロールのプロパティです。

> [!NOTE]
> 渡すことの代わりに、代わりに、`EmployeesRow`インスタンス、おでしたに渡すだけ、`HireDate`値を使用して`<%# DisplayDaysOnJob(Eval("HireDate")) %>`です。 ただし、`Eval`メソッドを返します、`object`を変更する必要がありますが、当社`DisplayDaysOnJob`型の入力パラメーターを受け入れるようにメソッド シグネチャ`object`、代わりにします。 無条件キャストおことはできません、`Eval("HireDate")`への呼び出し、`DateTime`ため、`HireDate`内の列、`Employees`テーブルを含めることができます`NULL`値。 そのまま使用する必要がありますはそのため、`object`の入力パラメーターとして、`DisplayDaysOnJob`メソッドは、データベースがあったかどうかかを確認`NULL`値 (を使用して実行する`Convert.IsDBNull(objectToCheck)`)、し、それに応じて。


これらの微妙な原因を選択した全体で渡す`EmployeesRow`インスタンス。 次のチュートリアルで使用した複数調整の例を会いしましょう、`Eval("columnName")`書式指定メソッドに入力パラメーターを渡すための構文。

次では、TemplateField が追加された後、GridView の宣言の構文を示します、`DisplayDaysOnJob`から呼び出されるメソッド、 `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

図 16 は、ブラウザーで表示したときに、完了したチュートリアルを示します。


[![従業員が、ジョブにした日数の数が表示されます。](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**図 16**:、日数、従業員が入っていたジョブが表示されます ([フルサイズのイメージを表示するをクリックして](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>概要

GridView コントロール TemplateField により、他のフィールド コントロールと使用できるよりもデータを表示するために柔軟性の高い。 TemplateFields、状況に適しています。

- 複数のデータ フィールドを 1 つの GridView 列に表示される必要があります。
- 日付がプレーン テキストではなく Web コントロールを使用して表現されますベスト
- 出力は、メタデータを表示するなど、またはデータの再フォーマットでは、基になるデータによって異なります。

データの表示をカスタマイズするだけでなくもと思います後でチュートリアルを編集し、データの挿入のために使用するユーザー インターフェイスをカスタマイズするために TemplateFields が使われます。

次の 2 つのチュートリアルでは、探索を TemplateFields を使用して、DetailsView で見てで始まるテンプレートを続行します。 次に、フィールドを使用せずにテンプレートを使用して、柔軟にレイアウトとデータの構造が、フォーム ビューを有効にします。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Dan Jagers しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](custom-formatting-based-upon-data-cs.md)
[次へ](using-templatefields-in-the-detailsview-control-cs.md)
