---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: 編集および削除 DataList (VB) 内のデータの概要 |Microsoft ドキュメント
author: rick-anderson
description: DataList では、組み込みの編集と削除機能がない、ときにこのチュートリアルでは会いしましょうを編集および削除の o をサポートする DataList を作成する方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 6956777e91184a92e189db7aa716a4bd7dbbfccd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892778"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>編集および削除 DataList (VB) 内のデータの概要
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe)または[PDF のダウンロード](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> DataList では、組み込みの編集と削除機能がない、ときにこのチュートリアルでは会いしましょうを編集およびその基になるデータの削除をサポートする DataList を作成する方法です。


## <a name="introduction"></a>はじめに

[、概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)チュートリアルでは、挿入、更新、およびアプリケーションのアーキテクチャ、ObjectDataSource、GridView、DetailsView、およびフォーム ビューを使用してデータを削除する方法について説明しました制御します。 ObjectDataSource とこれら 3 つのデータの Web コントロールでは、単純なデータ変更のインターフェイスを実装する、スナップインが関与してスマート タグのチェック ボックスをオンに時間刻みだけを実行します。 コードを記述する必要はありません。

残念ながら、DataList では、編集、および GridView コントロールに固有の機能を削除すると、組み込みが不足しています。 不足しているこの機能は、一部には、DataList は、宣言型のデータ ソース コントロールとコードを必要としないデータ変更のページは使用できなかったときに、ASP.NET の以前のバージョンから relic をファクト、due です。 DataList ASP.NET 2.0 では提供しませんすぐ同じデータ変更機能として GridView、中に、ASP.NET 1.x 手法を使用して、このような機能が含まれておできます。 このアプローチには、コードのビットが必要ですが、このチュートリアルで後ほどお見せ、DataList れているようにいくつかのイベントおよびプロパティでこのプロセスを支援する場所。

このチュートリアルでは編集、およびその基になるデータの削除をサポートする DataList を作成する方法が表示されます。 今後のチュートリアルより高度な編集しシナリオを削除すると、入力フィールドの検証を含む、データ アクセスまたはビジネス ロジック層、およびなどから発生した例外を適切に処理を調べます。

> [!NOTE]
> DataList と同様にリピータ コントロールには、外の挿入、更新、または削除するボックスの機能が不足しています。 このような機能を追加することができます、DataList には、プロパティおよびリピータに見つかりませんを簡単にこのような機能を追加するイベントが含まれます。 そのため、このチュートリアルを編集および削除を見て今後もにフォーカスが置か厳密に DataList です。


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>手順 1: 編集と削除のチュートリアルの Web ページの作成

更新して、DataList からデータを削除する方法の探索を開始して、前に、まず、このチュートリアルを次のいくつかのものが必要な web サイト プロジェクトに、ASP.NET ページを作成するのにはしばらく s を使用できます。 という名前の新しいフォルダーを追加することによって開始`EditDeleteDataList`です。 次に、各ページに関連付けるように、そのフォルダーに次の ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![チュートリアルについては、ASP.NET ページを追加します。](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**図 1**: チュートリアルについては、ASP.NET ページを追加します。


などの他のフォルダーで`Default.aspx`で、`EditDeleteDataList`フォルダーは、そのセクションのチュートリアルを一覧表示されます。 注意してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


最後に、エントリとして、ページを追加、`Web.sitemap`ファイル。 具体的には、DataList リピータとマスター/詳細レポートの後に、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、DataList 編集および削除のチュートリアルの項目が含まれています。


![サイト マップでは DataList 編集および削除のチュートリアルのエントリが含まれるようになりました](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**図 3**: サイト マップでは DataList 編集および削除のチュートリアルのエントリが含まれるようになりました


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>手順 2: データを更新し、削除の方法を確認します。

編集および GridView にデータの削除はとても簡単、背後の ObjectDataSource、GridView 連携動作です。 説明したように、 [、イベントに関連付けられている挿入、更新、および削除を確認する](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)チュートリアルでは、行の更新 ボタンがクリックされたときに、GridView を自動的に割り当てます、に双方向データバインドを使用するフィールド`UpdateParameters` 、ObjectDataSource を集めた ObjectDataSource s を呼び出して`Update()`メソッドです。

残念ながら、DataList では、この組み込みの機能のいずれかの用意されていません。 ObjectDataSource のパラメーターにユーザー秒の値が割り当てられていることを確認する責任はその`Update()`メソッドが呼び出されます。 私たちを役立てるため、この作業は、DataList は、次のプロパティとイベントを提供します。

- **[ `DataKeyField`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)** 更新または削除すると、DataList 内の各項目を一意に識別できる必要があります。 このプロパティは、表示されるデータの主キー フィールドに設定します。 DataList s を表示するように[`DataKeys`コレクション](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx)、指定した`DataKeyField`DataList の各項目の値。
- **[ `EditCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)** ボタン、LinkButton、または ImageButton ときに発生が`CommandName`プロパティに設定されている編集をクリックします。
- **[ `CancelCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)** ボタン、LinkButton、または ImageButton ときに発生が`CommandName`プロパティに設定されている [キャンセル] をクリックします。
- **[ `UpdateCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)** ボタン、LinkButton、または ImageButton ときに発生が`CommandName`プロパティに設定されている更新プログラムをクリックします。
- **[ `DeleteCommand`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)** ボタン、LinkButton、または ImageButton ときに発生が`CommandName`プロパティに設定されている Delete をクリックします。

これらのプロパティとイベントを使用して、4 つの方法お使用して更新し、DataList からデータを削除できます。

1. **ASP.NET 1.x 手法を使用して**DataList ASP.NET 2.0 および ObjectDataSources する前に存在し、プログラムによる方法ですべてのデータ更新および削除することができます。 この方法は、ObjectDataSource をまったく ditches、両方表示するデータの取得中にビジネス ロジック層から直接、DataList にデータのバインドが必要とされ、更新、またはレコードを削除するとします。
2. **ページ上の単一 ObjectDataSource コントロールを使用して、選択すると、更新、および削除の**DataList は GridView s 本質的な編集と削除機能がない、ときに s t ことが理由なしで追加自分でします。 この方法でお GridView の例と同じように、ObjectDataSource を使用してが DataList s のイベント ハンドラーを作成する必要があります`UpdateCommand`イベント、ObjectDataSource のパラメーターと呼び出しをセットしてその`Update()`メソッドです。
3. **ObjectDataSource コントロールを使用して、選択するが、更新および削除すると直接に対して、BLL**オプション 2 を使用する場合のコードのビットを書き込む必要があります、`UpdateCommand`などのパラメーター値の割り当てイベント。 代わりに、ObjectDataSource を使用して、選択するためのオプションを使用しましたが、BLL (と同じように 1 をオプションと共に使用) に対して直接更新と削除の呼び出しを行います。 筆者の意見で BLL と直接やり取りによりデータの更新、ObjectDataSource s を割り当てるよりも読みやすいコードに潜在顧客`UpdateParameters`を呼び出すと、`Update()`メソッドです。
4. **複数 ObjectDataSources を通じて宣言的な手段を使用して**すべて前の 3 つの方法は、少量のコードを必要とします。 使用する場合 d ではなく保持可能なかなりの宣言の構文として、最後のオプション ページでの複数の ObjectDataSources を含めるを開始します。 最初の ObjectDataSource は BLL からデータを取得し、DataList にバインドします。 別の ObjectDataSource の追加しますが、DataList s 内に直接追加の更新、`EditItemTemplate`です。 サポートの削除は、まだ別 ObjectDataSource 本来必要で、`ItemTemplate`です。 この方法でこれらの埋め込まれた ObjectDataSource の使用`ControlParameters`宣言によって ObjectDataSource のパラメーターをユーザーの入力コントロールにバインドする (DataList s でプログラムで指定する必要はなく`UpdateCommand`イベント ハンドラー)。 このアプローチには少量のコードを埋め込み、ObjectDataSource s を呼び出す必要がありますが必要である`Update()`または`Delete()`3 つのアプローチを他のより小さい値までのコマンドが必要です。 欠点は、ここでは、こと複数 ObjectDataSources は塞ぎ、ページ全体の読みやすさが低下します。

強制された場合だけこれらのアプローチのいずれかを使用する、d を選択オプション 1 に、このパターンに合わせて設計された、DataList、最大限の柔軟性を提供するためです。 DataList は、ASP.NET 2.0 のデータ ソース コントロールを使用する拡張された、すべての機能拡張ポイントまたは正式な ASP.NET 2.0 のデータ (GridView、DetailsView、およびフォーム ビュー) の Web コントロールの機能にはありません。 オプション 2. ~ 4. いない価値、せずただしされます。

これと、今後編集および削除のチュートリアルは使用、ObjectDataSource データを取得する表示し、データ (オプション 3) を更新および削除 BLL への呼び出しを指示します。

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>手順 3: DataList を追加して、ObjectDataSource を構成します。

このチュートリアルでは、DataList 製品情報を一覧表示し、各製品の機能を提供、ユーザーの名前と価格を編集し、製品を完全に削除することを作成します。 具体的には、おは取得、ObjectDataSource を使用して表示するレコードが更新を実行し、BLL と直接やり取りしてアクションを削除します。 DataList を編集および削除する機能を実装する心配し、前に読み取り専用のインターフェイスに製品を表示するページをまず取得 s を使用できます。 以降お ve が次の手順を調べる前のチュートリアルではあります続行それらを簡単にします。

開いて開始、 `Basics.aspx`  ページで、`EditDeleteDataList`フォルダーと、デザイン ビューから、DataList をページに追加します。 次に、DataList s のスマート タグから新しい ObjectDataSource を作成します。 製品データを使用している、のでするように構成を使用して、`ProductsBLL`クラスです。 取得する*すべて*、製品を選択、`GetProducts()`メソッドを選択 タブにします。


[![構成、ObjectDataSource ProductsBLL クラスを使用するには](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**図 4**: 構成を使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![GetProducts() メソッドを使用して製品情報を返す](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**図 5**: 製品を使用して情報を返す、`GetProducts()`メソッド ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


GridView のように、DataList ものではありません。 新しいデータを挿入します。このため、挿入 タブで、ドロップダウン リストからオプション (なし)。(なし)、UPDATE および DELETE のタブのため、更新および削除は、BLL 経由でプログラムによって実行されます。


[![(なし) に ObjectDataSource の挿入のドロップダウンを一覧表示、更新、および削除のタブを設定することを確認します。](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**図 6**: ObjectDataSource の INSERT、UPDATE、および削除のタブで、ドロップダウン リストは、(なし) に設定されていることを確認 ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


ObjectDataSource を構成した後、デザイナーに戻る、[完了] をクリックします。 おとして自動的に Visual Studio、ObjectDataSource 構成を完了するときに、過去の例で見てきましたが作成、`ItemTemplate`の DropDownList の各データ フィールドを表示します。 これを置き換える`ItemTemplate`製品の名前と価格を表示するものとします。 また、設定、`RepeatColumns`プロパティを 2 です。

> [!NOTE]
> 説明したように、*の概要の挿入、更新、およびデータの削除*チュートリアルでは、当社アーキテクチャでは、削除する必要があります ObjectDataSource を使用してデータを変更するときに、 `OldValuesParameterFormatString` ObjectDataSource s からプロパティ宣言型マークアップ (またはその既定値にリセット`{0}`)。 このチュートリアルでただし、使用している、ObjectDataSource のみデータを取得します。 そのため、お ObjectDataSource s を変更する必要はありません`OldValuesParameterFormatString`プロパティの値 (ですが、これを行うには t を低下させる可能性)。


既定値 DataList に置き換えた後に`ItemTemplate`カスタマイズされたもので、ページの宣言型マークアップは、次のようななります。


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

ブラウザーを使って作業の進行状況を表示するのにはしばらくを実行します。 図 7 に示す、DataList は 2 つの列に各製品の製品名および単価を表示します。


[![製品の名前と価格は、2 つの列 DataList に表示されます。](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**図 7**: 2 つの列 DataList に、製品名および価格が表示されます ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> DataList はさまざまな更新と削除プロセスでは、必要なプロパティを持ち、ビュー ステートにこれらの値が格納されています。 したがって、DataList の構築をサポートしている編集またはデータを削除するときに、DataList のビュー ステートを有効にする必要です。  
>   
>  絶好のリーダーはでした。 編集可能な Gridview、DetailsViews、および FormViews を作成するときに、ビュー ステートを無効にすることを注意してください。 これは、ASP.NET 2.0 Web コントロールを含めることができますので*状態の制御*、どのビュー ステートがみなし essential のようなポストバック間で状態が保持します。


無効にする GridView で状態だけで、単純な状態情報を省略が、コントロールの状態 (を編集および削除するために必要な状態を含む) を保持します。 ASP.NET 1.x 時期に作成されている、DataList コントロールの状態を使用しないと、したがって、ビュー ステートを有効になっている必要があります。 参照してください[コントロールの状態とします。ビュー ステート](https://msdn.microsoft.com/library/1whwt1k7.aspx)について、目的のコントロールの状態と表示状態とは異なる方法です。

## <a name="step-4-adding-an-editing-user-interface"></a>手順 4: 編集のユーザー インターフェイスの追加

GridView コントロールは、フィールド (BoundFields CheckBoxFields、TemplateFields、やなど) のコレクションで構成されます。 これらのフィールドには、そのモードによって、レンダリングされるマークアップを調整できます。 たとえば、読み取り専用モードのときに、BoundField、データ フィールドの値テキストとして表示;編集モードで表示 ボックスに Web いるコントロール`Text`プロパティには、データ フィールドの値が割り当てられます。

DataList は、その一方で、テンプレートを使用してその項目をレンダリングします。 読み取り専用の項目を表示するを使用して、`ItemTemplate`経由で編集モードでのアイテムがレンダリングされる一方、`EditItemTemplate`です。 この時点で、DataList はのみが、`ItemTemplate`です。 項目レベルの編集機能を追加する必要がありますをサポートするために、`EditItemTemplate`編集可能な項目を表示するマークアップを格納しています。 このチュートリアルを秒の製品名および単価を編集するため、TextBox Web コントロールを使用します。

`EditItemTemplate` (オプションを選択して、テンプレートの編集、DataList s のスマート タグから) 宣言的またはデザイナーのいずれかに作成できます。 テンプレートの編集オプションを使用する 最初にスマート タグのテンプレートの編集リンクをクリックし、選択、`EditItemTemplate`ドロップダウン リストから項目。


[![DataList の EditItemTemplate で作業することを選択します。](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**図 8**: DataList s で作業することを選択`EditItemTemplate`([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


次に、製品名を入力: と価格: ツールボックスから 2 つのテキスト ボックス コントロールをドラッグし、`EditItemTemplate`デザイナー上のインターフェイスです。 テキスト ボックスの設定`ID`プロパティ`ProductName`と`UnitPrice`です。


[![製品の名前と価格のテキスト ボックスを追加します。](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**図 9**: 製品名をテキスト ボックスと価格の追加 ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


対応する製品データ フィールドの値をバインドする必要があります、`Text`の 2 つのテキスト ボックスのプロパティです。 テキスト ボックスのスマート タグから databindings の編集 リンクをクリックし、適切なデータ フィールドを関連付ける、`Text`プロパティ、図 10 に示すようにします。

> [!NOTE]
> バインドするときに、`UnitPrice`データ フィールド ボックスに秒の価格を`Text`フィールド、可能性があります形式にすると、通貨の値 (`{0:C}`) は通常の数値 (`{0:N}`)、書式設定なしのままにします。


![ProductName と UnitPrice データ フィールドをテキスト ボックスのテキストのプロパティにバインドします。](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**図 10**: バインド、`ProductName`と`UnitPrice`データ フィールドを`Text`のテキスト ボックスのプロパティ


図 10 で databindings の編集 ダイアログ ボックスでは方法に注意してください*いない*GridView または DetailsView、TemplateField または FormView でテンプレートを編集するときに表示される双方向データ バインドのチェック ボックスが含まれます。 双方向データ バインド機能に対応する ObjectDataSource 秒に自動的に割り当てられる入力 Web コントロールに入力した値が許可されている`InsertParameters`または`UpdateParameters`挿入またはデータを更新する場合。 DataList が双方向データ バインドをサポートするいないと思います後でこのチュートリアルで変更し、データを更新する準備が自分のユーザーが後に、プログラムによってこれらのテキスト ボックスにアクセスする必要があります`Text`プロパティとそれらの値にパスします。適切な`UpdateProduct`メソッドで、`ProductsBLL`クラスです。

最後に、更新プログラムを追加する必要がありますとキャンセルするボタン、`EditItemTemplate`です。 示したように、[マスター/詳細レコードを使用して、箇条書きリストのマスター詳細 DataList で](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)チュートリアルでは、ボタン、LinkButton、または ImageButton が`CommandName`プロパティが設定されてリピータまたは DataList、内からのクリックしてが、リピータまたは DataList の`ItemCommand`イベントが発生します。 DataList の場合、`CommandName`プロパティが特定の値に設定、追加のイベントはも発生する可能性があります。 特別な`CommandName`の他のプロパティの値が含まれます。

- キャンセルが発生し、`CancelCommand`イベント
- 編集が発生し、`EditCommand`イベント
- 更新が発生し、`UpdateCommand`イベント

これらのイベントが発生したことに注意してください*に加えて*、`ItemCommand`イベント。

追加、 `EditItemTemplate` 2 つのボタンの Web コントロール、1 つ持つ`CommandName`更新と [キャンセル] を設定、他の秒に設定されています。 これら 2 つのボタンの Web コントロールを追加した後、デザイナーは、次のようになります。


[![更新プログラムを追加し、キャンセル、EditItemTemplate するボタン](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**図 11**: 更新プログラムの追加とキャンセル ボタン、 `EditItemTemplate` ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


`EditItemTemplate`完了、DataList s 宣言型マークアップのようになります次。


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>手順 5: 編集モードに組み込みの追加

この時点で編集インターフェイスによって定義されているは、DataList その`EditItemTemplate`。 ただし、ある s 現在方法がありませんのユーザーが、ページを訪問した製品の情報を編集することを示すためにします。 をクリックすると、レンダリングその DataList 項目される編集モードで各製品に [編集] ボタンを追加する必要があります。 [編集] ボタンを追加して、開始、 `ItemTemplate`、デザイナーを通じて、または宣言します。 S [編集] ボタンを設定する`CommandName`プロパティを編集します。

この編集 ボタンを追加した後は、すぐブラウザーでページを表示します。 このさらに、各製品の一覧項目は [編集] ボタンを含める必要があります。


[![更新プログラムを追加し、キャンセル、EditItemTemplate するボタン](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**図 12**: 更新プログラムの追加とキャンセル ボタン、 `EditItemTemplate` ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


ボタンをクリックすると、ポストバックを発生させるが、*いない*リストを編集モードに製品を表示します。 製品を編集可能にする必要があります。

1. DataList s 設定[`EditItemIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)のインデックスに、`DataListItem`の編集 ボタンがクリックされただけです。
2. DataList にデータをバインドします。 DataList は再レンダリングされたときに、`DataListItem`が`ItemIndex`DataList s に対応する`EditItemIndex`を使用して表示されます、`EditItemTemplate`です。

DataList s 以降`EditCommand`[編集] ボタンがクリックされたときにイベントが発生した、作成、`EditCommand`イベント ハンドラーを次のコードで。


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

`EditCommand`型のオブジェクトに渡されるイベント ハンドラー`DataListCommandEventArgs`への参照が含まれますが、2 番目の入力パラメーターとして、`DataListItem`の編集 ボタンがクリックしてされました (`e.Item`)。 最初に、イベント ハンドラーは、DataList s を設定`EditItemIndex`を`ItemIndex`の編集可能な`DataListItem`DataList s を呼び出すことによって、DataList にデータを再バインドおよび`DataBind()`メソッドです。

このイベント ハンドラーを追加すると、ブラウザーでページを再表示します。 クリックした製品編集可能な (参照図 13) は、今すぐ編集ボタンをクリックします。


[![編集ボタンは編集可能な製品をクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**図 13**: 製品編集可能な [編集] ボタンをクリックすると、([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>手順 6: ユーザーの変更を保存します。

編集した製品の更新プログラムまたは [キャンセル] ボタンをクリックするとは何もこの時点でです。DataList s のイベント ハンドラーを作成する必要があります。 この機能を追加する`UpdateCommand`と`CancelCommand`イベント。 まずを作成して、`CancelCommand`イベント ハンドラーは、編集した製品 s [キャンセル] ボタンをクリックして、編集済みの状態、DataList を返す際に、よう課されたときに実行されます。

すべての読み取り専用モードで項目をレンダリング DataList は、する必要があります。

1. DataList s 設定[`EditItemIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)の存在しないインデックスを`DataListItem`インデックス。 `-1` 安全な選択肢のためは、`DataListItem`インデックスから始まります`0`です。
2. DataList にデータをバインドします。 No 以降`DataListItem` `ItemIndex` DataList s に対応している es `EditItemIndex`、DataList 全体が読み取り専用モードでレンダリングされます。

次のイベント ハンドラーのコードでは、次の手順を実行できます。


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

この追加、編集済みの状態は [キャンセル] ボタンを返します、DataList をクリックします。

最後のイベント ハンドラーが完了する必要がありますが、`UpdateCommand`イベント ハンドラー。 このイベント ハンドラーは、する必要があります。

1. 製品のユーザーが入力した名前と価格だけでなく、編集した製品 s をプログラムでアクセス`ProductID`です。
2. 呼び出して、適切な更新プロセスを開始`UpdateProduct`でオーバー ロード、`ProductsBLL`クラスです。
3. DataList s 設定[`EditItemIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)の存在しないインデックスを`DataListItem`インデックス。 `-1` 安全な選択肢のためは、`DataListItem`インデックスから始まります`0`です。
4. DataList にデータをバインドします。 No 以降`DataListItem` `ItemIndex` DataList s に対応している es `EditItemIndex`、DataList 全体が読み取り専用モードでレンダリングされます。

手順 1. と 2. はユーザーの変更を保存するため、します。手順 3. および 4. 状態に戻す DataList 事前編集、変更が保存されてされ後で実行した手順と同じ、`CancelCommand`イベント ハンドラー。

更新された製品の名前と価格を取得する必要がありますを使用する、`FindControl`メソッド内でテキスト ボックス、Web の制御参照プログラムを`EditItemTemplate`です。 また、編集した製品 s を取得する必要があります`ProductID`値。 Visual Studio に DataList s が割り当てられている最初にバインドされている場合、ObjectDataSource DataList を`DataKeyField`プロパティは、主キーの値をデータ ソースから (`ProductID`)。 DataList 秒から、この値を取得し、できる`DataKeys`コレクション。 確実に少し時間を取って、`DataKeyField`プロパティが実際に設定されている`ProductID`です。

次のコードでは、4 つの手順を実装します。


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

編集した製品 s で読み取ることによって、イベント ハンドラーが起動`ProductID`から、`DataKeys`コレクション。 次に、2 つのテキスト ボックス、`EditItemTemplate`が参照され、その`Text`ローカル変数に格納されているプロパティ`productNameValue`と`unitPriceValue`です。 使用して、`Decimal.Parse()`から値を読み取るためのメソッド、`UnitPrice`ようにする場合、値を入力 ボックスに、通貨記号には、まだ正常に変換できる、`Decimal`値。

> [!NOTE]
> 値を`ProductName`と`UnitPrice`テキスト ボックスのみ割り当てられている productNameValue と unitPriceValue 変数にテキスト ボックスのテキストのプロパティが指定された値を持つ場合です。 それ以外の場合、値は`Nothing`、変数の使用によるデータベース、データの更新の影響は`NULL`値。 コードが変換を処理するは、データベースへの文字列を空にする`NULL`GridView、DetailsView、フォーム ビュー コントロール内にある編集インターフェイスの既定の動作は、の値。


値が読み取られた後、`ProductsBLL`クラス s`UpdateProduct`メソッドが呼び出されると、価格、製品の名前で渡すことと`ProductID`です。 編集済みの状態で正確な同じロジックを使用して、DataList を返すことによって、イベント ハンドラーが完了する、`CancelCommand`イベント ハンドラー。

`EditCommand`、 `CancelCommand`、および`UpdateCommand`イベント ハンドラーの完了、訪問者が製品の価格と名前を編集できます。 図 14-16 では、アクションの編集このワークフローを表示します。


[![最初のページへのアクセス、すべての製品は読み取り専用モードで](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**図 14**: すべての製品が読み取り専用モードでは最初のページへのアクセス、ときに ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![製品名の価格を更新するには、編集ボタンをクリックします。](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**図 15**: 更新するには、製品名または価格編集 をクリックします ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![値を変更した後、読み取り専用モードに戻るに更新 をクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**図 16**: 値を変更した後、更新、読み取り専用モードに戻る をクリックして ([フルサイズのイメージを表示するをクリックして](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>手順 7: 削除機能を追加します。

DataList に削除の機能を追加するための手順は、編集機能を追加するためのものに似ています。 つまりを削除 ボタンを追加する必要があります、`ItemTemplate`クリックされたときに。

1. 対応する製品の s で読み取り`ProductID`を介して、`DataKeys`コレクション。
2. 呼び出して、削除を実行、`ProductsBLL`クラスの`DeleteProduct`メソッドです。
3. DataList にデータをバインドします。

%S を削除 ボタンを追加することによって開始できるように、`ItemTemplate`です。

ボタンをクリックしたときに`CommandName`編集、更新、または [キャンセル] を DataList s を発生させます`ItemCommand`と共に追加のイベントのイベント (Edit を使用する場合など、`EditCommand`もイベントは)。 同様に、すべてのボタン、LinkButton、または DataList の ImageButton が`CommandName`原因を削除するプロパティが設定されて、`DeleteCommand`イベントを発生させる (と共に`ItemCommand`)。

[編集] ボタンの横にある [削除] ボタンを追加、 `ItemTemplate`、設定、`CommandName`プロパティを削除します。 追加した後、このボタンを制御、DataList の`ItemTemplate`宣言構文のようになります。


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

DataList s のイベント ハンドラーを次に、作成`DeleteCommand`イベントは、次のコードを使用します。


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

[削除] ボタンをクリックするとポストバックを発生させるし、DataList s を発生させる`DeleteCommand`イベント。 イベント ハンドラー、クリックした製品 s で`ProductID`値がからアクセスされる、`DataKeys`コレクション。 呼び出すことによって、製品を削除する次に、`ProductsBLL`クラスの`DeleteProduct`メソッドです。

製品を削除した後、DataList にデータを再バインドおことが重要 (`DataList1.DataBind()`)、それ以外の場合、DataList は引き続きが削除されるだけで、製品を表示します。

## <a name="summary"></a>まとめ

DataList では、ポイントがないし、編集および GridView で楽しんでサポートの削除 をクリックして、中にコードの短いビットを使用して、拡張にこれらの機能を含めます。 このチュートリアルでは、2 つの列を削除できませんでした、名前と価格を編集することが製品の一覧を作成する方法を説明しました。 適切な Web コントロールを含むは、編集および削除のサポートを追加する、`ItemTemplate`と`EditItemTemplate`対応するイベント ハンドラーの作成、読み取り、ユーザーが入力したとプライマリ キーの値、およびビジネスとのやり取り層のロジックです。

基本的な編集および削除 DataList に機能が追加されましたが、中には、高度な機能が不足しています。 たとえばはありません入力フィールドの検証 - ユーザーが多すぎますの価格を入力した場合高価な例外がスローされますが`Decimal.Parse`すぎますを変換しようとするときに高価な`Decimal`です。 同様に、ビジネス ロジックでのデータまたはデータ アクセス層を更新中に問題がある場合、ユーザーは、標準的なエラー画面で表示されます。 何らかの削除 ボタンで確認メッセージなしは、誤って削除したり、製品すべてすぎる可能性があります。

後で編集のユーザーを向上させるために手順を説明するチュートリアルが発生します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Zack Jones、Ken Pespisa ものです。 Schmidt がされていました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](customizing-the-datalist-s-editing-interface-cs.md)
> [次へ](performing-batch-updates-vb.md)
