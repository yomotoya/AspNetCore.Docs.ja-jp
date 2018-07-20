---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: (C#) GridView コントロールで TemplateFields を使用する |Microsoft Docs
author: rick-anderson
description: 柔軟性を提供するには、GridView は、テンプレートを使用して描画すると、TemplateField を提供します。 テンプレートは、Web コントロールの静的 HTML の組み合わせを含めることができますとしています.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: b01d2018d4164f8db7c86226f7f1d5999743e6c2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826703"
---
<a name="using-templatefields-in-the-gridview-control-c"></a>(C#) GridView コントロールで TemplateFields を使用します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe)または[PDF のダウンロード](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> 柔軟性を提供するには、GridView は、テンプレートを使用して描画すると、TemplateField を提供します。 テンプレートは、組み合わせを含めることができます、静的な html Web コントロール、およびデータ バインド構文。 このチュートリアルでは、TemplateField を使用して、多くの GridView コントロールを使用してカスタマイズを実現する方法を説明します。


## <a name="introduction"></a>はじめに

GridView は、一連からには、どのようなプロパティを示すフィールドで構成されます、`DataSource`データの表示方法と表示される出力に含まれます。 最も単純なフィールドの型は、BoundField では、データ値をテキストとして表示します。 その他のフィールドの種類は、代替の HTML 要素を使用してデータを表示します。 チェックされた状態は、指定したデータ フィールドの値によって異なります。 チェック ボックスとして表示されます」など、CheckBoxFieldImageField がイメージ ソースが指定したデータ フィールドに基づいてイメージをレンダリングします。 ハイパーリンクやボタンの状態は、基になるデータ フィールドの値によって異なります。 は、内と ButtonField フィールドの種類を使用して表示することができます。

データの代替ビュー CheckBoxField、ImageField、内、および ButtonField フィールドの種類を許可する、まだが書式設定に関するかなり限られています。 一方、ImageField は 1 つのイメージのみを表示、CheckBoxField のみ単一のチェック ボックスを表示できます。 場合、特定のフィールドは、チェック ボックスをオンするには、いくつかのテキストを表示する必要があります。*と*イメージ、別のデータ フィールドの値に基づいてすべてでしょうか。 または、チェック ボックス、イメージ、ハイパーリンク、またはボタン以外の Web コントロールを使用してデータを表示したい場合ですか。 さらに、BoundField は、表示が、1 つのデータ フィールドを制限します。 1 つの GridView 列に 2 つ以上のデータ フィールドの値を表示する場合はどうでしょうか。

このレベルの柔軟性を対応するために、GridView は描画を使用すると、TemplateField を提供する*テンプレート*します。 テンプレートは、組み合わせを含めることができます、静的な html Web コントロール、およびデータ バインド構文。 さらに、TemplateField には、さまざまなさまざまな状況の表示をカスタマイズするために使用できるテンプレートがあります。 たとえば、`ItemTemplate`行ごとのセルを表示するために、既定で使用されますが、`EditItemTemplate`データを編集するときに、インターフェイスをカスタマイズするテンプレートを使用できます。

このチュートリアルでは、TemplateField を使用して、多くの GridView コントロールを使用してカスタマイズを実現する方法を説明します。 [前のチュートリアル](custom-formatting-based-upon-data-cs.md)を使用して基になるデータに基づく書式をカスタマイズする方法を説明しました、`DataBound`と`RowDataBound`イベント ハンドラー。 基になるデータに基づく書式をカスタマイズする別の方法は呼び出すことによって書式設定メソッドをテンプレート内で。 このチュートリアルではこの手法に注目します。

このチュートリアルでは従業員の一覧の外観をカスタマイズ TemplateFields を使用します。 すべての従業員、一覧に表示されますが、従業員が表示されます具体的には、1 つの列で、予定表コントロールと日数したされて使用することで、会社であることを示す [状態] 列で、雇用された日付の最初と最後の名前。


[![次の 3 つ TemplateFields を使用して、表示をカスタマイズするには](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**図 1**: 次の 3 つ TemplateFields を使用して、表示をカスタマイズする ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image3.png))。


## <a name="step-1-binding-the-data-to-the-gridview"></a>手順 1: データを GridView にバインドします。

レポートの外観をカスタマイズ TemplateFields を使用する必要があるシナリオでのですが、まず BoundFields だけを含む GridView コントロールを作成して開始する最も簡単なし、新しい TemplateFields を追加するかを既存の BoundFields を変換するにはTemplateFields に応じて。 そのため、デザイナーを使用してページに GridView を追加し、従業員の一覧を返す、ObjectDataSource にバインドしましょうこのチュートリアルを開始します。 Employee フィールドの各 BoundFields で、次の手順は、GridView を作成します。

開く、`GridViewTemplateField.aspx`ページし、GridView をツールボックスからデザイナーにドラッグします。 GridView のスマート タグからを起動する新しい ObjectDataSource コントロールを追加することも、`EmployeesBLL`クラスの`GetEmployees()`メソッド。


[![GetEmployees() メソッドを呼び出す新しい ObjectDataSource コントロールを追加します。](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**図 2**: 新しい ObjectDataSource コントロールを追加するには、その呼び出します、`GetEmployees()`メソッド ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image6.png))。


この方法で、GridView にバインドが自動的に追加、BoundField、従業員のプロパティの各: `EmployeeID`、 `LastName`、 `FirstName`、 `Title`、 `HireDate`、 `ReportsTo`、および`Country`します。 このレポートのみましょうことはありませんを表示する、 `EmployeeID`、 `ReportsTo`、または`Country`プロパティ。 これらの BoundFields を削除するには、ことができます。

- GridView のスマート タグからの列の編集リンク フィールド ダイアログ ボックスをクリックを使用して、このダイアログ ボックスを表示します。 次に、選択の左下から BoundFields を一覧表示し、赤い X 印をクリックします。 は、BoundField を削除するボタンします。
- GridView の宣言型構文をソース ビューから手動で編集、削除、 `<asp:BoundField>` BoundField を削除する要素。

削除した後、 `EmployeeID`、 `ReportsTo`、および`Country`BoundFields、GridView のマークアップのようになります。


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

ブラウザーで、進行状況を表示する時間がかかります。 各従業員の 4 つの列のレコードを含むテーブルが表示この時点で: 従業員の姓、名、ユーザー名前のいずれか、もう 1 つのタイトルを雇用された日付の 1 つ。


[![各従業員の LastName、FirstName、タイトル、および HireDate フィールドが表示されます。](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**図 3**: `LastName`、 `FirstName`、 `Title`、および`HireDate`各従業員のフィールドが表示されます ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image9.png))。


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>手順 2: 1 つの列に姓と名を表示します。

現時点では、各従業員の姓と別の列に姓と名が表示されます。 代わりに 1 つの列に結合する便利な場合があります。 これを実現するには、TemplateField を使用する必要があります。 できますか、新しい TemplateField を追加、必要なマークアップと、データ バインディング構文を追加および削除し、`FirstName`と`LastName`BoundFields、またはここに変換できる、`FirstName`を TemplateField、BoundField に含める TemplateField の編集`LastName`値、および削除し、 `LastName` BoundField します。

両方のアプローチは、同じ結果を net ここで個人 TemplateFields 可能な場合に BoundFields を変換する変換が自動的に追加されているため、`ItemTemplate`と`EditItemTemplate`Web コントロールと外観を模倣するためにデータ バインド構文BoundField の機能を選択します。 利点は、以下の作業を行う、TemplateField に変換プロセスが実行される作業の一部の必要があります。

既存の BoundField を TemplateField に変換するには、フィールドのダイアログ ボックスの GridView のスマート タグからの列の編集リンクをクリックします。 左下隅で、一覧から変換し、右下隅の"Convert このフィールドを TemplateField"リンクをクリックして BoundField を選択します。


[![BoundField フィールド ダイアログ ボックスから TemplateField に変換します。](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**図 4**: フィールド ダイアログ ボックスから、TemplateField に BoundField を変換 ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image12.png))。


さあ、変換、 `FirstName` BoundField TemplateField にします。 この変更後は、デザイナーでの立場に立っての違いはありません。 これは、BoundField のルック アンド フィールを維持する TemplateField を作成、BoundField を TemplateField に変換されるためです。 この変換プロセスには、デザイナーでビジュアルの違いをこの時点でされていませんに関係なく BoundField の宣言型構文は置き換えられて`<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />`- TemplateField の次の構文で。


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

TemplateField から成る 2 つのテンプレート、ご覧のとおり、`ItemTemplate`ラベルを持つが`Text`の値に設定されて、`FirstName`データ フィールド、および`EditItemTemplate`TextBox で制御が`Text`プロパティも設定されて`FirstName`データ フィールド。 データ バインディング構文 -`<%# Bind("fieldName") %>`のことを示しますデータ フィールド*`fieldName`* Web コントロールの指定したプロパティにバインドされます。

追加する、`LastName`データ フィールドの値で別のラベルの Web コントロールを追加する必要があります。 この TemplateField を、`ItemTemplate`バインドとその`Text`プロパティを`LastName`します。 これは、手動でまたはデザイナーのいずれかに実現できます。 手動では、単に適切な宣言型構文を追加、 `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

デザイナーでそれを追加するには、GridView のスマート タグからのテンプレートの編集リンクをクリックします。 GridView のテンプレートの編集インターフェイスが表示されます。 このインターフェイスのスマート タグは、GridView でテンプレートの一覧を示します。 のみがあるため 1 つ TemplateField この時点で、ドロップダウン リストに記載のみのテンプレートは、これらのテンプレートの`FirstName`と共に TemplateField、`EmptyDataTemplate`と`PagerTemplate`します。 `EmptyDataTemplate`テンプレートは、指定されている場合、; GridView にバインドされたデータで結果がない場合は、GridView の出力を表示するために使用が、 `PagerTemplate`、指定した場合にページングをサポートする GridView ページング インターフェイスを表示するために使用します。


[![GridView のテンプレートは、デザイナーで編集できます。](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**図 5**: GridView のテンプレートが編集を通じて、デザイナー ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image15.png))。


表示することも、`LastName`で、 `FirstName` TemplateField ラベル コントロールをドラッグして、ツールボックスから、 `FirstName` TemplateField の`ItemTemplate`GridView でのテンプレート編集インターフェイス。


[![FirstName TemplateField の ItemTemplate にラベル Web コントロールを追加します。](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**図 6**: ラベル Web コントロールを追加、 `FirstName` TemplateField の ItemTemplate ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image18.png))。


TemplateField に追加されたラベル Web コントロールが、この時点でその`Text`プロパティが"Label"に設定します。 このプロパティの値にバインドされるように変更する必要があります、`LastName`データ フィールドの代わりにします。 ラベル コントロールのスマート タグをクリックしてを実行し、DataBindings の編集オプションを選択します。


[![ラベルのスマート タグから DataBindings の編集 オプションを選択します。](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**図 7**: ラベルのスマート タグから DataBindings の編集 オプションを選択 ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image21.png))。


これは、[DataBindings] ダイアログ ボックスが表示されます。 ここからは、左側の一覧からのデータ バインドに参加し、右側のドロップダウン リストからデータをバインドするフィールドを選択し、プロパティを選択できます。 選択、`Text`左側からプロパティと`LastName`右側のフィールドし、[ok] をクリックします。


[![[氏名] データ フィールドに Text プロパティをバインドします。](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**図 8**: バインド、`Text`プロパティを`LastName`データ フィールド ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image24.png))。


> [!NOTE]
> DataBindings ダイアログ ボックスでは、双方向データ バインドを実行するかどうかを指定できます。 オフの場合、指定しない場合、データ バインド構文`<%# Eval("LastName")%>`の代わりに使用される`<%# Bind("LastName")%>`します。 どちらの方法は、このチュートリアルでは問題ありません。 双方向データ バインドは、挿入や、データの編集時に重要になります。 データを単に表示するには、ただし、どちらの方法は動作も同じようにします。 今後のチュートリアルでは、双方向のデータ バインドの詳細について説明します。


ブラウザーからこのページを表示する時間がかかります。 ご覧のように、GridView にはも 4 つの列が含まれますただし、`FirstName`に列が表示されます*両方*、`FirstName`と`LastName`データ フィールドの値。


[![1 つの列、FirstName と LastName の値が表示されます。](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**図 9**: 両方、`FirstName`と`LastName`値の 1 つの列に表示されます ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image27.png))。


この最初の手順を完了するには、削除、 `LastName` BoundField の名前を変更し、 `FirstName` TemplateField の`HeaderText`プロパティ"Name"をします。 これらの変更後に、次のよう GridView の宣言型マークアップになります。


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![各従業員の最初と最後の名前は、1 つの列に表示されます。](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**図 10**: 1 つの列に各従業員の最初と最後の名前が表示されます ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image30.png))。


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>手順 3: を表示するカレンダー コントロールを使用して、`HiredDate`フィールド

データ フィールドの値を表示する GridView でテキストとしては、BoundField を使用して同じくらい簡単です。 特定のシナリオでは、ただし、データは最適な表されますテキストだけではなく、特定の Web コントロールを使用します。 データの表示には、このようなカスタマイズ TemplateFields でことができます。 などではなくテキストとして、従業員の雇用された日付を表示、表示しても、カレンダー (を使用して[予定表コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx))、雇用された日付を強調表示されます。

これを行うには、変換することで開始、 `HiredDate` BoundField TemplateField にします。 単に、GridView のスマート タグに移動し、フィールド ダイアログ ボックスを立ち上げて、列の編集リンクをクリックします。 選択、 `HiredDate` BoundField し「変換このフィールドを TemplateField」。


[![HiredDate BoundField を TemplateField に変換します。](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**図 11**: 変換、`HiredDate`を TemplateField に BoundField ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image33.png))。


手順 2. で説明したように、BoundField を含む TemplateField に置き換えられます、`ItemTemplate`と`EditItemTemplate`ラベルとテキスト ボックスにある`Text`プロパティにバインドされます、 `HiredDate` データバインディング構文を使用して値`<%# Bind("HiredDate")%>`.

予定表コントロールでテキストを置換するには、ラベルを削除して、予定表コントロールの追加、テンプレートを編集します。 デザイナーでは、GridView のスマート タグからテンプレートの編集を選択し、選択、 `HireDate` TemplateField の`ItemTemplate`ドロップダウン リストから。 次に、ラベル コントロールを削除し、カレンダー コントロールをツールボックスから、テンプレートの編集インターフェイスにドラッグします。


[![予定表コントロールを追加、HireDate TemplateField の ItemTemplate](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**図 12**: するカレンダー コントロールを追加、 `HireDate` TemplateField の`ItemTemplate`([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image36.png))。


GridView の各行がで予定表コントロールを含むこの時点でその`HiredDate`TemplateField します。 ただし、従業員の実際の`HiredDate`値が設定されていない任意の場所の既定値は、現在の月と日付を表示するには、各カレンダー コントロールの原因と、予定表コントロールでします。 これを解決する必要がありますに割り当てる各従業員の`HiredDate`を予定表コントロールの[SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx)と[VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx)プロパティ。

予定表コントロールのスマート タグから DataBindings の編集を選択します。 次に、どちらもバインド`SelectedDate`と`VisibleDate`プロパティを`HiredDate`データ フィールド。


[![SelectedDate と VisibleDate プロパティを HiredDate データ フィールドにバインドします。](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**図 13**: バインド、`SelectedDate`と`VisibleDate`プロパティを`HiredDate`データ フィールド ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image39.png))。


> [!NOTE]
> 選択した日付の予定表コントロールの必要があります必ずしも表示されません。 など、カレンダーが 8 月 1 日あります<sup>st</sup>、1999 として選択した日付が現在の月と年に表示されます。 選択した日付と日付の表示が、予定表コントロールの指定された`SelectedDate`と`VisibleDate`プロパティ。 従業員の両方の選択になるため`HiredDate`にこれらのプロパティの両方にバインドする必要がありますには、かどうかを確認して、`HireDate`データ フィールド。


ブラウザーでページを表示するときに、予定表は今すぐ従業員の採用日の月を表示し、特定の日付を選択します。


[![従業員の HiredDate が予定表コントロールで表示されます。](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**図 14**:、従業員の`HiredDate`カレンダー コントロールに表示されます ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image42.png))。


> [!NOTE]
> これまでに見たすべての例とは異なりこのチュートリアルで行った*いない*設定`EnableViewState`プロパティを`false`この GridView にします。 この決定の理由は、予定表コントロールの日付をクリックすると、ポストバックでカレンダーの選択した日付をクリックした日付に設定です。 GridView のビュー ステートが無効になっている場合、ポストバックごとに GridView のデータがバインド カレンダーの選択の日付を設定すると、その基になるデータ ソースに*戻る*に従業員の`HireDate`、上書きします。ユーザーによって選択された日付。


このチュートリアルではこれは議論については、ユーザーは、従業員を更新することはないため`HireDate`します。 その日付が選択できるようにするために、Calendar コントロールを構成する最適なもあります。 関係なく、このチュートリアルはいくつかの状況では、ビュー ステートを特定の機能を提供するために有効する必要があることを表示します。

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>手順 4: 会社の作業して、従業員の日数を表示

これまでに TemplateFields の 2 つのアプリケーションがきました。

- 1 つの列に 2 つ以上のデータ フィールドの値を結合し、
- テキストではなく、Web コントロールを使用してデータ フィールドの値を表現します。

TemplateFields の 3 つ目の使用は、GridView のに関するメタデータを表示するために基になるデータ。 だけでなく、従業員の雇用日を表示するには、たとえば、する可能性がもしたジョブの数の合計日数を表示する列があります。

まだ TemplateFields の別の用途は、基になるデータが格納された形式でデータベースよりも web ページのレポートに異なる方法で表示する必要がある場合のシナリオで発生します。 想像してみてください、`Employees`表が、`Gender`文字が格納されているフィールド`M`または`F`を従業員の性別を示すためにします。 Web ページで、この情報を表示するときに"Male"または"Female"のいずれかとして、"M"または"F"だけではなく、性別を表示したい場合があります。

これら両方のシナリオを作成して処理できる、*書式指定メソッド*ASP.NET ページの分離コード クラスで (またはとして実装される、別のクラス ライブラリで、`static`メソッド)、テンプレートから呼び出されます。 このような書式指定メソッドは、先ほど説明したのと同じデータ バインディング構文を使用して、テンプレートから呼び出されます。 書式指定メソッドは、パラメーターの任意の数を取得できますが、文字列を返す必要があります。 この返される文字列は、テンプレートに挿入される HTML です。

この概念を示すためには、従業員がしたジョブの合計日数を一覧表示する列を表示するには、このチュートリアルを強化しましょう。 この書式指定メソッドになります、`Northwind.EmployeesRow`オブジェクトし、文字列として、従業員が採用されているされた日数を返します。 このメソッドは、ASP.NET ページの分離コード クラスに追加できますが、*する必要があります*としてマークする`protected`または`public`テンプレートからアクセスできるようにするためにします。


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

`HiredDate`フィールドに格納できる`NULL`、値がないことを確認する必要があります最初の値をデータベース`NULL`計算に進む前にします。 場合、`HiredDate`値は`NULL`でない場合単に文字列"Unknown"; を返すこと`NULL`、現在の時刻の差を計算し、`HiredDate`値し、の日数を返します。

このメソッドを使用するには、データ バインディング構文を使用して、gridview TemplateField から呼び出す必要があります。 GridView のスマート タグで列の編集リンクをクリックし、新しい TemplateField を追加する新しい TemplateField を GridView に追加することで開始します。


[![新しい TemplateField を GridView に追加します。](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**図 15**: 新しい TemplateField を GridView に追加 ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image45.png))。


この新しい TemplateField 設定`HeaderText`「のジョブに日」にプロパティとその`ItemStyle`の`HorizontalAlign`プロパティを`Center`します。 呼び出す、 `DisplayDaysOnJob` 、テンプレートからメソッドを追加、`ItemTemplate`し、次のデータ バインディング構文を使用します。


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` 返します、`DataRowView`に対応するオブジェクト、`DataSource`にバインドされているレコード、`GridViewRow`します。 その`Row`プロパティは、厳密に型指定を返します`Northwind.EmployeesRow`に渡される、`DisplayDaysOnJob`メソッド。 このデータ バインド構文で直接表示されることができます、`ItemTemplate`に示すように次の宣言構文) を割り当てることができますか、 `Text` Label Web コントロールのプロパティ。

> [!NOTE]
> 渡すことではなく、別の方法として、`EmployeesRow`インスタンス、私たちでした渡すだけで、`HireDate`値を使用して`<%# DisplayDaysOnJob(Eval("HireDate")) %>`。 ただし、`Eval`メソッドを返します。、`object`で変更する必要があるため、当社`DisplayDaysOnJob`型の入力パラメーターを受け入れるようにメソッド シグネチャ`object`を代わりにします。 盲目的にキャストできません、`Eval("HireDate")`への呼び出しを`DateTime`ため、`HireDate`内の列、`Employees`テーブルに含めることができます`NULL`値。 受け入れる必要がありますが、そのため、`object`の入力パラメーターとして、`DisplayDaysOnJob`メソッドは、データベースをれているかを確認`NULL`値 (を使用して実現する`Convert.IsDBNull(objectToCheck)`)、しそれに従って進んでください。


全体を渡すため、これらの細部については、選択した`EmployeesRow`インスタンス。 使用するための複数の調整例が表示されます次のチュートリアルでは、`Eval("columnName")`書式指定メソッドに入力パラメーターを渡すための構文。

TemplateField に追加した後、次が、GridView の宣言の構文を表示し、`DisplayDaysOnJob`から呼び出されるメソッド、 `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

図 16 は、ブラウザーで表示したときに、完了したチュートリアルを示します。


[![従業員が、そのジョブのした日の数が表示されます。](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**図 16**:、日数、従業員が入っていたジョブが表示されます ([フルサイズの画像を表示する をクリックします](using-templatefields-in-the-gridview-control-cs/_static/image48.png))。


## <a name="summary"></a>まとめ

他のフィールドのコントロールで使用できるよりもデータを表示するために柔軟性の高い、GridView コントロールで TemplateField になります。 TemplateFields は状況に適しています。

- 複数のデータ フィールドを 1 つの GridView 列に表示される必要があります。
- データがプレーン テキストではなく、Web コントロールを使用して最適な表現します。
- 出力は、メタデータを表示するなど、データを再フォーマットでは、基になるデータによって異なります。

だけでなく、データの表示をカスタマイズ TemplateFields がおわかり後でチュートリアルを編集し、データの挿入のために使用するユーザー インターフェイスをカスタマイズするためも使用されます。

次の 2 つのチュートリアルでは、テンプレートを参照してください、DetailsView で TemplateFields を使用する以降の調査を続けます。 次に、代わりにフィールド テンプレートを使用して、柔軟なレイアウトとデータの構造でフォーム ビューを有効にします。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Dan Jagers でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](custom-formatting-based-upon-data-cs.md)
> [次へ](using-templatefields-in-the-detailsview-control-cs.md)
