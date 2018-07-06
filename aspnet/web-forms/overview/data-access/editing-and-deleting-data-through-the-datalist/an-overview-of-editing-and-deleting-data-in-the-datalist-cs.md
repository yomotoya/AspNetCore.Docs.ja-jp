---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: (C#)、DataList でデータ編集と削除の概要 |Microsoft Docs
author: rick-anderson
description: DataList は、組み込みの編集と削除機能がない、中にこのチュートリアルでは方法について説明します、DataList の編集および削除の o をサポートするを作成する.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: a9be3e332ec19f78c4dcc2e78d3dd4609c27fddf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810486"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>(C#)、DataList でデータ編集と削除の概要
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe)または[PDF のダウンロード](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> DataList は、組み込みの編集と削除機能がない、中にこのチュートリアルでは方法について説明します編集およびその基になるデータの削除をサポートする DataList を作成します。


## <a name="introduction"></a>はじめに

[挿入の概要、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)チュートリアルの挿入、更新、およびアプリケーション アーキテクチャ、ObjectDataSource、および GridView、DetailsView と FormView を使用してデータを削除する方法を説明しましたコントロール。 ObjectDataSource とこれらの 3 つのデータ Web コントロールでは、単純なデータ変更インターフェイスを実装する、スナップイン、関係だけで、スマート タグからチェック ボックスをオンに時間を刻みします。 コードを記述する必要はありません。

残念ながら、DataList では、組み込みの編集および削除の GridView コントロールに固有の機能が不足しています。 不足しているこの機能は、DataList は、宣言型のデータ ソース コントロールとコードのないデータの変更ページは使用できなかったときに、ASP.NET の以前のバージョンからの relic をファクトの一部が原因です。 DataList ASP.NET 2.0 では提供されませんすぐ同じデータ変更機能として、GridView、中にこのような機能を含める ASP.NET 1.x の手法を使用できます。 このアプローチのコードが必要ですが、このチュートリアルでは表示されるよう、DataList いくつかのイベントおよびプロパティでこのプロセスを支援する場所です。

このチュートリアルでは、編集、およびその基になるデータの削除をサポートする DataList を作成する方法がわかります。 今後のチュートリアルより高度な編集しシナリオを削除する、入力フィールドの検証を含む、データ アクセスまたはビジネス ロジック層、およびなどから発生した例外を適切に処理を説明します。

> [!NOTE]
> など、DataList、Repeater コントロールには、帯の挿入、更新、または削除する機能が不足しています。 このような機能を追加することができます、DataList には、プロパティおよび Repeater に見つかりませんでしたを簡単にこのような機能を追加するイベントが含まれます。 そのため、このチュートリアルと将来的に編集および削除が確認されるは、DataList に厳密に集中します。


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>手順 1: 作成、編集と削除は Web ページのチュートリアル

更新および DataList からデータを削除する方法の調査を始める前に、このチュートリアルの次のいくつかの必要がありますが、web サイト プロジェクトで ASP.NET ページを作成する最初に少し s を使用できます。 という名前の新しいフォルダーを追加することで開始`EditDeleteDataList`します。 次に、次の ASP.NET ページを使用する各ページに関連付けることを確認、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![チュートリアルについては、ASP.NET ページに追加します。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**図 1**: チュートリアルについては、ASP.NET ページに追加します。


などの他のフォルダーで`Default.aspx`で、`EditDeleteDataList`フォルダーは、そのセクションでは、チュートリアルを一覧表示されます。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))。


最後に、ページに追加するエントリとして、`Web.sitemap`ファイル。 具体的には、DataList と Repeater によるマスター/詳細レポートの後に、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューで、DataList の編集および削除のチュートリアルの項目できるようになりました。


![サイト マップ DataList の編集および削除のチュートリアルのエントリになりました](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**図 3**: サイト マップ DataList の編集および削除のチュートリアルのエントリになりました


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>手順 2: データの更新および削除の手法を確認します。

GridView でのデータ編集と削除は、GridView と ObjectDataSource が連携しての作業に実際には、下にあるため、非常に簡単です。 説明したように、 [、イベントに関連付けられている挿入、更新、および削除の確認](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)チュートリアルでは、行の更新 ボタンがクリックされたときに、GridView を自動的に割り当てます、への双方向データバインドを使用するフィールド`UpdateParameters` 、ObjectDataSource のコレクションと ObjectDataSource s が呼び出され、`Update()`メソッド。

残念ながら、DataList では、この組み込み機能のいずれか用意されていません。 ObjectDataSource のパラメーターに、ユーザーの値が割り当てられていることを確認する必要がありますはその`Update()`メソッドが呼び出されます。 この努力ご支援、するためには、DataList は、次のプロパティとイベントを提供します。

- **[ `DataKeyField`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)** 更新や削除、DataList 内の各項目を一意に識別できる必要があります。 このプロパティは、表示されるデータの主キー フィールドを設定します。 そうは、DataList s [ `DataKeys`コレクション](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx)、指定した`DataKeyField`各 DataList 項目の値。
- **[ `EditCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)** ボタン、LinkButton、ImageButton またはときに発生が`CommandName`プロパティが編集がクリックされました。
- **[ `CancelCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)** ボタン、LinkButton、ImageButton またはときに発生が`CommandName`プロパティが [キャンセル] をクリックします。
- **[ `UpdateCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)** ボタン、LinkButton、ImageButton またはときに発生が`CommandName`プロパティが更新プログラムをクリックします。
- **[ `DeleteCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)** ボタン、LinkButton、ImageButton またはときに発生が`CommandName`プロパティに設定されて削除をクリックします。

更新および DataList からデータを削除に使用できます 4 つのアプローチはこれらのプロパティとイベントを使用して、あります。

1. **ASP.NET 1.x の手法を使用して**DataList は、ASP.NET 2.0 と ObjectDataSources の前に存在し、プログラムによる方法ですべてのデータ更新および削除することができます。 この方法は、ObjectDataSource をまったく ditches され、両方を表示するデータの取得中にビジネス ロジック層から直接、DataList にデータをバインドしますが必要ですし、更新、またはレコードを削除するときにします。
2. **ページ上の 1 つの ObjectDataSource コントロールを選択すると、更新、および削除するを使用して**DataList は、GridView s 本質的な編集と削除機能がない、中にある s t できる理由なしに追加自分たちでします。 この方法で使用します、ObjectDataSource と同じように、GridView の例でが DataList s のイベント ハンドラーを作成する必要があります`UpdateCommand`イベント、ObjectDataSource のパラメーターと呼び出しを設定します、`Update()`メソッド。
3. **ObjectDataSource コントロールを使用して、選択するが、更新および削除する直接に対して、BLL**オプション 2 を使用する場合にコードを少し記述する必要があります、`UpdateCommand`などのパラメーター値の割り当てイベント。 代わりを選択すると、ObjectDataSource を使用していますが、BLL (など、1 をオプションと共に使用) に対して直接更新および削除の呼び出しを行います。 個人的な意見で BLL と直接連動してデータの更新 ObjectDataSource s を割り当てるよりも読みやすいコードに潜在顧客`UpdateParameters`呼び出しとその`Update()`メソッド。
4. **宣言型の複数の ObjectDataSources 手段を使用して**すべて前の 3 つのアプローチのコードを必要とします。 D ではなく引き続き使用する限りはるか宣言の構文として、最後のオプションでは、ページ上の複数の ObjectDataSources を含めるは。 最初の ObjectDataSource では、BLL からデータを取得し、DataList にバインドされます。 別の ObjectDataSource を追加するが DataList s 内に直接追加更新、`EditItemTemplate`します。 サポートの削除は、まだ別の ObjectDataSource が必要で、`ItemTemplate`します。 この方法により、これらの埋め込まれた ObjectDataSource の使用`ControlParameters`宣言によって、ObjectDataSource のパラメーターをユーザー入力コントロールにバインドする (s DataList でプログラムで指定する必要はなく`UpdateCommand`イベント ハンドラー)。 この方法は、少量のコードを埋め込み、ObjectDataSource s を呼び出す必要がありますも必要があります。`Update()`または`Delete()`、他の 3 つのアプローチより小さいコマンドははるかにが必要です。 ここでの弱点は、こと複数 ObjectDataSources は乱雑にし、ページ全体の読みやすさからそれてです。

強制しかこれらの方法のいずれかを使用する場合、このパターンに対応に設計された、DataList、最大限の柔軟性を提供するためオプション 1 d は選択します。 DataList は、ASP.NET 2.0 のデータ ソース コントロールを使用する拡張された、すべての機能拡張ポイントや公式の ASP.NET 2.0 データ Web コントロール (GridView、DetailsView、およびフォーム ビュー) の機能にはありません。 オプション 2 ~ 4 が merit、なしです。

これと、将来の編集とチュートリアルを削除するが使用 ObjectDataSource データを取得するを表示し、データ (オプション 3) 更新および削除する BLL に呼び出しを転送します。

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>手順 3: DataList を追加して、その ObjectDataSource を構成します。

このチュートリアルでは、製品情報一覧が表示され、各製品の機能を提供、ユーザー名と価格を編集し、製品をすべて削除する DataList を作成します。 具体的には、ObjectDataSource を使用して表示するレコードを取得、更新を実行がおよび BLL と直接連動して削除操作がいます。 DataList の編集と削除の機能を実装する心配し、前に読み取り専用のインターフェイスで、製品を表示するページをまず取得 s を使用できます。 経過 ve は次の手順を調べる前のチュートリアルでは、次に、これを簡単にします。

開いて開始、`Basics.aspx`ページで、`EditDeleteDataList`フォルダーと、デザイン ビューでは、DataList をページに追加します。 次に、DataList s のスマート タグから新しい ObjectDataSource を作成します。 製品データを使用して、構成を使用するよう、`ProductsBLL`クラス。 取得する*すべて*、製品の選択、`GetProducts()`メソッドで、[選択] タブ。


[![ProductsBLL クラスを使用する ObjectDataSource を構成します。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**図 4**: 構成に使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))。


[![GetProducts() メソッドを使用して、製品情報を返す](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**図 5**: 使用して、製品情報を返す、`GetProducts()`メソッド ([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))。


など、GridView、DataList はいない新しいデータを挿入するために設計されていますこのため、挿入 タブで、ドロップダウン リストからオプション (なし)。(なし) の UPDATE および DELETE の各タブのため、更新および削除は、BLL を介してプログラムで実行されます。


[![ObjectDataSource の挿入でドロップダウン リストを一覧表示、更新、およびタブの削除 (None) に設定されていることを確認します。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**図 6**: ObjectDataSource の INSERT、UPDATE、および削除のタブで、ドロップダウン リストは、(なし) に設定されていることを確認します ([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))。


ObjectDataSource を構成した後、デザイナーに戻る、[完了] をクリックします。 ObjectDataSource 構成、Visual Studio を自動的に完了したときに、過去の例で確認したところを作成するよう、 `ItemTemplate` DropDownList の各データ フィールドを表示します。 これを置き換える`ItemTemplate`製品の名前と価格を表示するものにします。 また、設定、`RepeatColumns`プロパティを 2。

> [!NOTE]
> 説明したように、*概要の挿入、更新、およびデータの削除*チュートリアルでは、ObjectDataSource、アーキテクチャでは、削除する必要がありますを使用してデータを変更するときに、 `OldValuesParameterFormatString` ObjectDataSource %s からのプロパティ宣言型マークアップ (またはその既定値にリセットする`{0}`)。 このチュートリアルでただし、使用している、ObjectDataSource のみデータを取得します。 そのため、私たち ObjectDataSource s を変更する必要はありません`OldValuesParameterFormatString`プロパティの値 (ですが、これを行うには t が低下する)。


DataList の既定値に置き換えた後`ItemTemplate`をカスタマイズしたものでは、ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

ブラウザーから進行状況を表示する時間がかかります。 図 7 に示すよう、DataList では、2 つの列の各製品の製品名および単価が表示されます。


[![2 つの列 DataList で製品名と価格の表示します。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**図 7**: 2 つの列 DataList で、製品名と価格が表示されます ([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))。


> [!NOTE]
> DataList は、多数の更新および削除のプロセスに必要なプロパティと、これらの値は、ビュー ステートに格納されます。 そのため、編集またはデータの削除をサポート DataList を構築することが DataList s のビューステートが有効にすることが重要です。  
>   
>  鋭い読者なら、可能性があることが編集可能な Gridview、DetailsViews、および FormViews を作成するときに、ビュー ステートを無効にすることを思い出してください。 ASP.NET 2.0 Web コントロールを含めることができますので、これは*状態コントロール*、ビュー ステートがみなし essential のようなポストバック間で永続化状態。


無効にするだけで、GridView での状態は、単純な状態情報が省略されますが (を編集および削除するために必要な状態を含む) コントロールの状態を維持します。 ASP.NET 1.x タイム フレームで作成されている、DataList コントロールの状態は使用されませんし、したがって、ビュー ステートを有効になっている必要があります。 参照してください[コントロールの状態とします。ビュー ステート](https://msdn.microsoft.com/library/1whwt1k7.aspx)コントロールの状態とビュー ステートとは異なる方法の詳細については、目的の。

## <a name="step-4-adding-an-editing-user-interface"></a>手順 4: 編集のユーザー インターフェイスを追加します。

GridView コントロールは、フィールド (BoundFields、CheckBoxFields、TemplateFields、およびなど) のコレクションで構成されます。 これらのフィールドには、そのモードによって、出力されるマークアップを調整できます。 たとえば、読み取り専用モードで、BoundField、データ フィールドの値テキストとして表示します。編集モードでレンダリング TextBox Web いるコントロール`Text`プロパティには、データ フィールドの値が割り当てられます。

DataList では、その一方で、テンプレートを使用してそのアイテムをレンダリングします。 読み取り専用の項目を表示するを使用して、`ItemTemplate`を使用して編集モードでのアイテムが表示される一方、`EditItemTemplate`します。 この時点で、DataList はのみが、`ItemTemplate`します。 項目レベルの編集機能を追加する必要がありますをサポートするために、`EditItemTemplate`編集可能な項目に対して表示されるマークアップを格納しています。 このチュートリアルでは、s の製品名および単価を編集するためのテキスト ボックスに Web コントロール使用します。

`EditItemTemplate` (DataList s のスマート タグからのテンプレートの編集 オプションを選択) を宣言的またはデザイナーのいずれかに作成できます。 テンプレートの編集オプションを使用して、まず、スマート タグのテンプレートの編集リンクをクリックし、、`EditItemTemplate`ドロップダウン リストから項目。


[![DataList s の後で作業することを選択します。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**図 8**: s DataList で作業することを選択`EditItemTemplate`([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))。


次に、製品名を入力: と価格: ツールボックスから 2 つのテキスト ボックス コントロールをドラッグし、`EditItemTemplate`デザイナー上のインターフェイス。 テキスト ボックスの設定`ID`プロパティ`ProductName`と`UnitPrice`します。


[![製品の名前と価格のテキスト ボックスを追加します。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**図 9**: 価格、製品名のテキスト ボックスを追加 ([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))。


対応する製品のデータ フィールド値をバインドする必要があります、 `Text` 2 つのテキスト ボックスのプロパティ。 テキスト ボックスのスマート タグから DataBindings の編集リンクをクリックし、適切なデータ フィールドを関連付ける、`Text`プロパティ、図 10 に示すようにします。

> [!NOTE]
> バインドするときに、`UnitPrice`データ フィールドをテキスト ボックスの価格`Text`フィールドに、可能性があります形式にすると、通貨値 (`{0:C}`)、通常の数値 (`{0:N}`)、または書式設定されていないままにします。


![ProductName と UnitPrice データ フィールドをテキスト ボックスのテキストのプロパティにバインドします。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**図 10**: バインド、`ProductName`と`UnitPrice`データ フィールドを`Text`テキスト ボックスのプロパティ


図 10 DataBindings の編集 ダイアログ ボックスでは方法に注意してください。*いない*双方向データ バインドのチェック ボックスで、GridView、DetailsView、TemplateField または FormView のテンプレートを編集するときに存在するが含まれます。 双方向データ バインド機能に対応する ObjectDataSource 秒に自動的に割り当てられる入力 Web コントロールに入力された値が許可されている`InsertParameters`または`UpdateParameters`挿入またはデータを更新するとき。 DataList が双方向データ バインドをサポートしていませんおわかりに後で、このチュートリアルではユーザーが変更され、データを更新する準備が彼女の後に、プログラムによってこれらのテキスト ボックスにアクセスする必要があります`Text`プロパティとその値を渡す、。適切な`UpdateProduct`メソッドで、`ProductsBLL`クラス。

最後に、更新プログラムを追加する必要がありますとキャンセル ボタンに、`EditItemTemplate`します。 説明したように、[マスター/マスター レコードの箇条書きリストを使用すると詳細 DataList について詳しく説明](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)チュートリアルでは、ボタン、LinkButton や ImageButton が`CommandName`プロパティが設定されて、DataList、Repeater 内でのクリックして、Repeater、DataList または s`ItemCommand`イベントが発生します。 DataList の場合、`CommandName`プロパティは、特定の値に設定されても追加イベントを発生させる可能性があります。 特殊な`CommandName`他のユーザーの間でのプロパティの値が含まれます。

- キャンセルが発生、`CancelCommand`イベント
- 編集が発生、`EditCommand`イベント
- 更新が発生、`UpdateCommand`イベント

これらのイベントが発生したことに注意してください*に加えて*、`ItemCommand`イベント。

追加、 `EditItemTemplate` 2 つのボタンの Web コントロール、1 つ持つ`CommandName`Update と他の設定を [キャンセル] に設定されています。 これら 2 つのボタンの Web コントロールを追加した後、デザイナーに、次のようになります。


[![更新プログラムを追加し、キャンセル、後にボタン](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**図 11**: 更新プログラムの追加とキャンセル ボタン、 `EditItemTemplate` ([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))。


`EditItemTemplate`完了、DataList s 宣言型マークアップのようになります次。


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>手順 5: 編集モードに切り替わるプラミングを追加します。

この時点で、編集インターフェイスを使用して定義されているが、DataList その`EditItemTemplate`。 ただし、そこ s 現在する方法がない、ページにアクセスしたユーザーの製品の情報を編集したことを示します。 [編集] ボタンをクリックすると、レンダリング、DataList 項目の編集モードで各製品に追加する必要があります。 Edit ボタンを追加して、開始、 `ItemTemplate`、デザイナーを通じて、または宣言します。 S [編集] ボタンを設定することを`CommandName`プロパティを編集します。

この編集 ボタンを追加した後、ブラウザーでページを表示するみましょう。 これにより、各製品の一覧は、[編集] ボタンを含める必要があります。


[![更新プログラムを追加し、キャンセル、後にボタン](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**図 12**: 更新プログラムの追加とキャンセル ボタン、 `EditItemTemplate` ([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))。


ボタンをクリックすると、ポストバックを発生させるは*いない*製品のリストを編集モードにします。 製品を編集可能にするには、する必要があります。

1. DataList s 設定[`EditItemIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)のインデックスに、`DataListItem`ある編集ボタンがクリックされただけです。
2. DataList のデータを再バインドします。 DataList は再表示されたときに、`DataListItem`が`ItemIndex`DataList s に対応`EditItemIndex`はレンダリングを使用してその`EditItemTemplate`。

DataList s 以降`EditCommand`イベント編集ボタンがクリックされたときに発生、作成、`EditCommand`イベント ハンドラーを次のコード。


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

`EditCommand`イベント ハンドラーが型のオブジェクトに渡される`DataListCommandEventArgs`への参照を含む、2 番目の入力パラメーターとして、`DataListItem`ある編集ボタンがクリックされた (`e.Item`)。 イベント ハンドラーは、DataList s をまず設定`EditItemIndex`を`ItemIndex`は編集可能なの`DataListItem`DataList s を呼び出すことによって、DataList にデータを再バインドと`DataBind()`メソッド。

このイベント ハンドラーを追加した後、ブラウザーでページを再検討します。 [編集] ボタンをクリックすると、今すぐでは、製品のクリックされた編集可能な (図 13 参照)。


[![編集ボタンが編集可能な製品をクリックします。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**図 13**: 製品編集可能で、[編集] をクリックすると、([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))。


## <a name="step-6-saving-the-user-s-changes"></a>手順 6: ユーザーの変更を保存します。

編集した製品の更新プログラムまたは [キャンセル] ボタンをクリックするとは何も行いません。 この時点でDataList s のイベント ハンドラーを作成する必要があります。 この機能を追加する`UpdateCommand`と`CancelCommand`イベント。 まず、作成、`CancelCommand`イベント ハンドラーは、編集した製品の [キャンセル] ボタンをクリックし、編集済みの状態、DataList を返す際に携わることと、実行されます。

DataList のすべての読み取り専用モードでは、その項目を表示するには、する必要があります。

1. DataList s 設定[`EditItemIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)の存在しないインデックスに`DataListItem`インデックス。 `-1` 安全な選択肢のためは、`DataListItem`開始インデックス`0`します。
2. DataList のデータを再バインドします。 No 以降`DataListItem` `ItemIndex` DataList s に対応して es `EditItemIndex`、全体の DataList は読み取り専用モードで表示されます。

次のイベント ハンドラーのコードでは、次の手順を実行できます。


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

これにより、編集済みの状態に キャンセル ボタン戻ります DataList をクリックします。

最後のイベント ハンドラーが完了する必要がありますが、`UpdateCommand`イベント ハンドラー。 このイベント ハンドラーは、する必要があります。

1. 製品のユーザーが入力した名前、価格とともに s 編集済みの製品をプログラムでアクセス`ProductID`します。
2. 適切なを呼び出して、更新プロセスを開始`UpdateProduct`でオーバー ロード、`ProductsBLL`クラス。
3. DataList s 設定[`EditItemIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)の存在しないインデックスに`DataListItem`インデックス。 `-1` 安全な選択肢のためは、`DataListItem`開始インデックス`0`します。
4. DataList のデータを再バインドします。 No 以降`DataListItem` `ItemIndex` DataList s に対応して es `EditItemIndex`、全体の DataList は読み取り専用モードで表示されます。

手順 1. および 2. は、ユーザーの変更を保存するため、します。手順 3 と 4 は、変更が保存されているしで実行した手順と同じです編集済みの状態、DataList を返す、`CancelCommand`イベント ハンドラー。

更新された製品名と価格を取得する必要がありますを使用する、 `FindControl` TextBox Web コントロールの内のプログラムで参照するメソッド、`EditItemTemplate`します。 編集した製品 s を取得する必要もあります`ProductID`値。 Visual Studio に DataList s が割り当てられている場合、最初に、ObjectDataSource を DataList にバインドしました`DataKeyField`プロパティは、主キーの値をデータ ソースから (`ProductID`)。 DataList s から、この値を取得しできます`DataKeys`コレクション。 いることを確認する少し、`DataKeyField`プロパティに設定されて実際`ProductID`します。

次のコードは、4 つの手順を実装します。


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

イベント ハンドラーを編集した製品 s で読み取ることによって開始`ProductID`から、`DataKeys`コレクション。 次に、2 つのテキスト ボックス、`EditItemTemplate`参照されると、その`Text`ローカル変数に格納されているプロパティ`productNameValue`と`unitPriceValue`。 使用して、`Decimal.Parse()`メソッドから値を読み取る、`UnitPrice`いる場合、値を入力するためのテキスト ボックスに通貨記号、それが正常に変換できる、`Decimal`値。

> [!NOTE]
> 値、`ProductName`と`UnitPrice`テキスト ボックスのテキストのプロパティが指定された値がある場合に、テキスト ボックスが productNameValue と unitPriceValue 変数に割り当てられるのみです。 それ以外の場合、値の`Nothing`、変数の使用のデータベースでデータを更新の影響は`NULL`値。 コードが変換を処理するは、空のデータベースへの文字列`NULL`値で、GridView、DetailsView、FormView コントロールで編集インターフェイスの既定の動作です。


値を読み取った後、`ProductsBLL`クラス s`UpdateProduct`メソッドが呼び出されると、製品の名前を渡して、価格設定、および`ProductID`。 イベント ハンドラーのように正確な同じロジックを使用して編集済みの状態、DataList を返すことによって完了、`CancelCommand`イベント ハンドラー。

`EditCommand`、 `CancelCommand`、および`UpdateCommand`イベント ハンドラーの完了、訪問者には、製品の価格と名前を編集できます。 図 14-16 は、アクションのこの編集ワークフローを表示します。


[![ページにアクセスして、すべての製品は読み取り専用モードで](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**図 14**: すべての製品が読み取り専用モードでは、ページを最初にアクセスして、ときに ([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))。


[![製品名の価格を更新するには、[編集] ボタンをクリックします。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**図 15**: 製品名または価格を更新するには、[編集] ボタンをクリックします ([フルサイズの画像を表示する をクリックします。](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))。


[![値を変更した後、読み取り専用モードに戻るに更新 をクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**図 16**: の後に、値を変更する、読み取り専用モードに戻るの更新プログラムをクリックします ([フルサイズの画像を表示する をクリックします](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))。


## <a name="step-7-adding-delete-capabilities"></a>手順 7: 削除の機能を追加します。

DataList に削除の機能を追加するための手順では、編集機能を追加するためのものに似ています。 つまり、削除ボタンを追加する必要があります、`ItemTemplate`クリックされたとき。

1. 読み取り、対応する製品 s`ProductID`を使用して、`DataKeys`コレクション。
2. 呼び出して、削除を実行します。、`ProductsBLL`クラス s`DeleteProduct`メソッド。
3. DataList のデータを再バインドします。

S に削除 ボタンを追加することで開始できるように、`ItemTemplate`します。

クリックすると、ボタンが`CommandName`編集、更新、またはキャンセル DataList s を発生させます`ItemCommand`イベントと共に追加のイベント (編集を使用する場合など、`EditCommand`もイベントが発生します)。 同様に、ボタン、linkbutton コントロール、または、DataList で ImageButton が`CommandName`原因を削除するプロパティを設定、`DeleteCommand`イベントを発生させる (と共に`ItemCommand`)。

[編集] ボタンの横にある [削除] ボタンを追加、`ItemTemplate`設定、その`CommandName`プロパティを削除します。 このボタンを追加した後、DataList s が制御`ItemTemplate`宣言構文のようにする必要があります。


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

DataList s のイベント ハンドラーを次に、作成`DeleteCommand`イベントは、次のコードを使用します。


[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

削除 ボタンをクリックするとポストバックが発生して、DataList s が発生した`DeleteCommand`イベント。 イベント ハンドラー、クリックされた製品 s で`ProductID`から値をアクセス、`DataKeys`コレクション。 呼び出すことにより、製品を削除する次に、`ProductsBLL`クラスの`DeleteProduct`メソッド。

この製品を削除した後、DataList にデータをバインドし直すことが重要 (`DataList1.DataBind()`)、それ以外の場合、DataList は引き続き表示だけ削除された製品。

## <a name="summary"></a>まとめ

DataList は、ポイントがないし、編集および GridView によってサポートの削除 をクリックして、コードの短いビットを使用して拡張することをこれらの機能を含めます。 このチュートリアルでは、2 つの列を削除できませんでした、名前と価格を編集することが製品の一覧を作成する方法を説明しました。 適切な Web コントロールなどの問題は、編集および削除のサポートを追加、`ItemTemplate`と`EditItemTemplate`、対応するイベント ハンドラーの作成、読み取りユーザーが入力したとプライマリ キーの値、およびビジネスとやり取りロジック レイヤー。

基本的な編集と削除、DataList に機能を追加したときより高度な機能が不足しています。 などない入力フィールドの検証 - ユーザーが多すぎますの価格を入力した場合安価ですが、例外がスローされます`Decimal.Parse`すぎますを変換する際に高価な`Decimal`します。 同様に、ビジネス ロジックでデータまたはデータ アクセス層を更新中に問題がある場合、ユーザーは、標準エラー画面に表示されます。 任意並べ替えの削除 ボタンを確認するには、誤って削除した製品がすべてすぎる可能性があります。

今後のチュートリアルの編集のユーザーを向上させる方法を見ていきますが発生します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Zack Jones、Ken Pespisa、および Randy Schmidt でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](performing-batch-updates-cs.md)
