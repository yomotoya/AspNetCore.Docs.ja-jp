---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: マスター ページ (c#) からコンテンツ ページと対話する |Microsoft Docs
author: rick-anderson
description: メソッドを呼び出す、マスター ページのコードから、[コンテンツ] ページのプロパティなどを設定する方法について説明します。
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 815752ee70eb761d7f9da24c9eada9d4c0c833a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828076"
---
<a name="interacting-with-the-content-page-from-the-master-page-c"></a>マスター ページ (c#) からコンテンツ ページと対話します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> メソッドを呼び出す、マスター ページのコードから、[コンテンツ] ページのプロパティなどを設定する方法について説明します。


## <a name="introduction"></a>はじめに

上記のチュートリアルでは、コンテンツ ページがそのマスター ページとプログラムでやり取りする方法を調査します。 再現率が最も最近、5 つを表示する GridView コントロールに含めるマスター ページを更新しましたでは、製品を追加します。 [コンテンツ] ページのユーザーが新しい製品を追加でしたを作成しました。 新しい製品を追加すると、マスター ページだけで追加された製品にが含ま ために、GridView を更新するように指示するコンテンツ ページが必要です。 この機能は、更新されたデータにバインドされている、GridView では、マスター ページをパブリック メソッドを追加し、[コンテンツ] ページからそのメソッドを呼び出すいました。

コンテンツとマスター ページの相互作用の最も一般的な形式は、[コンテンツ] ページから発生します。 ただしの操作に現在のコンテンツ ページを rouse にマスター ページの可能性があり、マスター ページには、ユーザーが [コンテンツ] ページにも表示されているデータを変更できるユーザー インターフェイス要素が含まれている場合、このような機能が必要な場合があります。 コンテンツ ページがクリックされたときに、ボタンを含むマスター ページ コントロールを GridView の製品情報は、制御が表示されます、すべての製品の価格を倍にします。 前のチュートリアルで例と同様、GridView が二重の価格、新しい価格を表示するようにボタンがクリックされた後に更新する必要がありますが、このシナリオでは、[コンテンツ] ページを rouse がアクションにする必要があるマスター ページ。

このチュートリアルでは、マスター ページ、[コンテンツ] ページで定義されている機能を起動する方法について説明します。

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>イベントとイベント ハンドラーを使用してプログラムの対話を仕掛ける

マスター ページからコンテンツ ページの機能を呼び出すことは、別の方法よりもより困難になります。 コンテンツ ページからプログラムの対話を仕掛けるときに、コンテンツ ページが単一のマスター ページは、どのようなパブリック メソッドとプロパティは、自由に、わかっています。 マスター ページ、ただし、多くの異なるコンテンツ ページ、それぞれに独自のプロパティとメソッドのセットもかまいません。 方法については、次に、記述すればコード実行時までにどのようなコンテンツ ページが呼び出されますがわからないときに、そのコンテンツのページで何らかのアクションを実行するマスター ページのでしょうか。

ボタン コントロールなどの ASP.NET Web コントロールを検討してください。 ボタン コントロールは、任意の数の ASP.NET ページに表示できるし、メカニズムを知らせることができます、ページがクリックしてされたことが必要です。 これは*イベント*します。 具体的には、ボタン コントロールを発生させますその`Click`イベント ボタンを含む ASP.NET ページが経由でその通知に応答できるオプションをクリックすると、*イベント ハンドラー*します。

これと同じパターンは、そのコンテンツのページで、マスター ページのトリガー機能させるために使用できます。

1. イベントは、マスター ページを追加します。
2. マスター ページは、そのコンテンツ ページと通信する必要があるときにイベントが発生します。 たとえば、マスター ページは、ユーザーには、価格が倍増したの [コンテンツ] ページにアラートを生成する必要があります、そのイベントは発生します価格が 2 倍にした直後に。
3. いくつかの操作を行う必要のあるコンテンツ ページで、イベント ハンドラーを作成します。

このチュートリアルの残りの部分実装; 概要で説明した例具体的には、データベース内の製品を一覧表示されたコンテンツ ページとボタンを含むマスター ページは、価格の 2 倍に制御します。

## <a name="step-1-displaying-products-in-a-content-page"></a>手順 1: コンテンツのページで製品を表示します。

最初の注文のビジネスでは、Northwind データベースから製品を一覧表示されたコンテンツ ページを作成します。 (前のチュートリアルでは、プロジェクトに Northwind データベースを追加しました[*コンテンツ ページからマスター ページと対話する*](interacting-with-the-master-page-from-the-content-page-cs.md))。新しい ASP.NET ページを追加して、開始、`~/Admin`という名前のフォルダー`Products.aspx`を確実にそれをバインドに、`Site.master`マスター ページ。 図 1 は、このページは、web サイトに追加した後、ソリューション エクスプ ローラーを示します。


[![Admin フォルダーに新しい ASP.NET ページを追加します。](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**図 01**: に新しい ASP.NET ページの追加、`Admin`フォルダー ([フルサイズの画像を表示する をクリックします](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))。


そのことを思い出してください、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)という名前のカスタム ベース ページ クラスを作成したチュートリアル`BasePage`でない場合は、ページのタイトルを生成します。明示的に設定します。 移動して、`Products.aspx`ページの分離コード クラスし、派生させる`BasePage`(の代わりにから`System.Web.UI.Page`)。

最後に、更新、`Web.sitemap`このレッスンのエントリを追加するファイル。 下に次のマークアップを追加、`<siteMapNode>`コンテンツをマスター ページの相互作用のレッスン。


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

この追加`<siteMapNode>`要素は、レッスンに反映されます (図 5 参照) を一覧表示します。

戻り`Products.aspx`します。 コンテンツ コントロールで`MainContent`GridView コントロールを追加し、名前、`ProductsGrid`します。 GridView をという名前の新しい SqlDataSource コントロールにバインド`ProductsDataSource`します。


[![GridView を新しい SqlDataSource コントロールにバインドします。](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**図 02**: 新しい SqlDataSource コントロールを GridView にバインド ([フルサイズの画像を表示する をクリックします](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))。


ウィザードは、Northwind データベースを使用するように構成します。 かどうかは、前のチュートリアルを実行して、名前付き接続文字列が必要`NorthwindConnectionString`で`Web.config`します。 図 3 に示すように、この接続文字列をドロップダウン リストから選択します。


[![Northwind データベースを使用する SqlDataSource を構成します。](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**図 03**: Northwind データベースを使用する SqlDataSource の構成 ([フルサイズの画像を表示する をクリックします](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))。


データ ソース コントロールを次に、指定`SELECT`ステートメント、Products テーブルのドロップダウン リストから選択し、返すことによって、`ProductName`と`UnitPrice`列 (図 4 参照)。 [次へ] をクリックし、データ ソースの構成ウィザードを完了し、[完了] します。


[![Products テーブルから、ProductName と UnitPrice フィールドを返す](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**図 04**: 返す、`ProductName`と`UnitPrice`フィールドから、`Products`テーブル ([フルサイズの画像を表示する をクリックします](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))。


すべてです。 ウィザードの完了後は、Visual Studio は、GridView が SqlDataSource コントロールによって返される 2 つのフィールドをミラー化するに 2 つの BoundFields を追加します。 GridView と SqlDataSource コントロールのマークアップに従います。 図 5 は、ブラウザーで表示した場合の結果を示します。


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]


[![各製品とその価格は、GridView で表示されます。](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**図 05**: 各製品とその価格は、GridView で表示されます ([フルサイズの画像を表示する をクリックします](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))。


> [!NOTE]
> 自由に、GridView の外観をクリーンアップできます。 通貨として表示される UnitPrice の値を書式設定して、背景色とフォントを使用して、グリッドの外観を改善するいくつかの候補が含まれます。 表示して、ASP.NET でのデータの書式設定の詳細についてを参照してください、[チュートリアル シリーズのデータの操作](../../data-access/index.md)します。


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>手順 2: マスター ページへの二重価格ボタンの追加

次のタスクでは、マスターに Button Web コントロールでは追加するページで、クリックすると、倍精度浮動小数点、データベース内のすべての製品の価格です。 開く、`Site.master`マスター ページと、デザイナーの下に配置するのには、ツールボックスからボタンをドラッグ、 `RecentProductsDataSource` SqlDataSource コントロールの前のチュートリアルで追加しました。 ボタンの設定`ID`プロパティを`DoublePrice`とその`Text`「二重製品価格」プロパティです。

次に、その名前を付け、マスター ページに、SqlDataSource コントロールを追加`DoublePricesDataSource`します。 実行に使用されるこの SqlDataSource、`UPDATE`ステートメントのすべての価格の 2 倍にします。 具体的には、設定する必要があります、`ConnectionString`と`UpdateCommand`を適切な接続文字列プロパティと`UPDATE`ステートメント。 この SqlDataSource コントロールを呼び出す必要がありますし、`Update`メソッドと、`DoublePrice`ボタンをクリックします。 設定する、`ConnectionString`と`UpdateCommand`プロパティ、SqlDataSource コントロールを選択し、[プロパティ] ウィンドウに移動します。 `ConnectionString`プロパティ リストに既に格納されているこれらの接続文字列`Web.config`をドロップダウン リストで選択、`NorthwindConnectionString`図 6 に示すようにオプションします。


[![SqlDataSource、NorthwindConnectionString を使用する構成します。](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**図 06**: 構成に使用する SqlDataSource、 `NorthwindConnectionString` ([フルサイズの画像を表示する をクリックします](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))。


設定する、`UpdateCommand`プロパティ、プロパティ ウィンドウで、UpdateQuery オプションを検索します。 選択した場合、このプロパティで省略記号 [;] ボタンを表示します。図 7 に示すようにコマンドおよびパラメーターのエディター ダイアログ ボックスを表示するには、このボタンをクリックします。 次に入力して`UPDATE`ステートメント ダイアログ ボックスのテキスト ボックスに。


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

実行すると、このステートメントが 2 倍、`UnitPrice`内の各レコードの値、`Products`テーブル。


[![SqlDataSource の UpdateCommand プロパティを設定します。](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**図 07**: 設定 SqlDataSource の`UpdateCommand`プロパティ ([フルサイズの画像を表示する をクリックします](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))。


これらのプロパティを設定した後に、ボタンをクリックし、SqlDataSource コントロールの宣言型マークアップは、次のようなになります。


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

残っているを呼び出すその`Update`メソッドと、`DoublePrice`ボタンをクリックします。 作成、`Click`のイベント ハンドラー、`DoublePrice`ボタンをクリックし、次のコードを追加します。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

この機能をテストするを参照してください。、`~/Admin/Products.aspx`ページの手順 1. で作成し、"Double 製品価格"ボタンをクリックします。 ポストバックが発生するボタンをクリックし、実行、`DoublePrice`ボタンの`Click`イベント ハンドラー、すべての製品の価格を倍増します。 ページが再表示し、マークアップが返され、ブラウザーに再表示されます。 ただし、コンテンツ ページで、GridView は、ボタンがクリックされた「二重製品価格」の前に、同じ価格が一覧表示します。 これは、GridView に最初に読み込まれたデータがあるその状態をビューステートに格納されるため、それ以外の場合に指示がない限り、ポストバックでは読み込まれませんので。 別のページにアクセスし、後に戻るかどうか、`~/Admin/Products.aspx`ページが更新された価格が表示されます。

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>手順 3: は二重に発生し、イベント時の価格

に GridView、`~/Admin/Products.aspx`ページはすぐに反映されませんが価格の 2 倍、ユーザー数は当然と考えが、「Double 製品価格」ボタンをクリックしなかったことや、実行できませんでした。 しようとするかもしれません、ボタンをクリックすると時間、繰り返し価格を倍増さらにいくつか。 コンテンツで、グリッドする必要がありますこれを解決するには、ページは、これらは 2 倍にした直後に新しい料金を表示します。

このチュートリアルで前述したよう、ユーザーがクリックされるたびに、マスター ページでイベントを発生させる必要があります、`DoublePrice`ボタンをクリックします。 イベントは、別に一連の興味深いが発生したその他のクラス (イベント サブスクライバー) を通知するには、1 つのクラス (イベント パブリッシャー) の方法です。 この例でマスター ページは、イベント パブリッシャーです。これらのコンテンツの場合を考慮するページ、`DoublePrice`ボタンがクリックされた、サブスクライブしています。

クラスが作成してイベントにサブスクライブ、*イベント ハンドラー*、発生するイベントへの応答で実行されるメソッドであります。 パブリッシャーは、自分で定義することで発生させるイベントを定義、*イベント デリゲート*します。 イベントのデリゲートには、どのような入力パラメーターが、イベント ハンドラーに同意する必要がありますを指定します。 .NET Framework で、イベント デリゲートがない任意の値を返すし、2 つの入力パラメーターをそのまま使用しないでください。

- `Object`、イベント ソースを識別します、
- 派生したクラス `System.EventArgs`

イベント ハンドラーに渡される 2 番目のパラメーターは、イベントに関する追加情報を含めることができます。 基本の中に`EventArgs`クラスはない情報を渡す、.NET Framework には、多数拡張クラスにはが含まれています。 `EventArgs` encompass の追加のプロパティとします。 たとえば、`CommandEventArgs`インスタンスに応答するイベント ハンドラーに渡されます、`Command`イベント、2 つの情報プロパティが含まれています:`CommandArgument`と`CommandName`します。

> [!NOTE]
> 発生させる、およびイベントを処理の作成の詳細については、の参照[イベントとデリゲート](https://msdn.microsoft.com/library/17sde2xt.aspx)と[イベント デリゲートで単純な英語](http://www.codeproject.com/KB/cs/eventdelegates.aspx)します。


定義には、イベントは、次の構文を使用します。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

のみ、[コンテンツ] ページをユーザーがクリックされたときにアラートを生成する必要がありますので、`DoublePrice`ボタンしその他の追加情報を渡す必要はありません、イベント デリゲートを使用できます`EventHandler`、として 2 番目のオペランドを受け取るイベント ハンドラーを定義します。パラメーターの型のオブジェクト`System.EventArgs`します。 マスター ページで、イベントを作成するには、マスター ページの分離コード クラスに次のコード行を追加します。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

上記のコードは、という名前のマスター ページにパブリック イベントを追加します`PricesDoubled`します。 これで、価格が 2 倍にした後は、このイベントを発生させる必要があります。 イベントを発生させるには、次の構文を使用します。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

場所*送信者*と*eventArgs*は、サブスクライバーのイベント ハンドラーに渡す値になります。

更新プログラム、 `DoublePrice` `Click`イベント ハンドラーを次のコード。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

同様に、`Click`イベント ハンドラーを呼び出すことで起動、 `DoublePricesDataSource` SqlDataSource コントロールの`Update`メソッドすべての製品の価格の 2 倍にします。 次のイベント ハンドラーに 2 つの追加機能があること。 まず、 `RecentProducts` GridView のデータを更新します。 この GridView では、前のチュートリアルでは、マスター ページに追加されたし、最近追加した 5 つの製品を表示します。 これら 5 つの製品だけ 2 倍に価格が表示されるように、このグリッドを更新する必要があります。 次に、`PricesDoubled`イベントが発生します。 マスター ページ自体への参照 (`this`) は、イベント ソースと、空としてイベント ハンドラーに送信`EventArgs`オブジェクトは、イベントの引数として送信します。

## <a name="step-4-handling-the-event-in-the-content-page"></a>手順 4: [コンテンツ] ページでイベントを処理します。

この時点で、マスター ページを発生させますその`PricesDoubled`イベントたびに、`DoublePrice`ボタン コントロールがクリックされました。 ただし、これは半ばで-サブスクライバーにイベントを処理する必要があります。 これは、2 つの手順: イベント ハンドラーを作成し、イベントが発生したときにイベント ハンドラーが実行されるようにイベント配線のコードを追加します。

名前付きイベント ハンドラーを作成して開始`Master_PricesDoubled`します。 定義したため、`PricesDoubled`マスター ページ内のイベントの種類のイベント ハンドラーの 2 つの入力パラメーターがある必要があります`Object`と`EventArgs`、それぞれします。 イベント ハンドラーの呼び出しで、 `ProductsGrid` GridView の`DataBind`グリッドにデータを再バインドするメソッド。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

イベント ハンドラーのコードが完了しましたが、まだ、マスター ページのワイヤ`PricesDoubled`イベントをこのイベント ハンドラー。 サブスクライバーでは、次の構文を使用してイベント ハンドラーにイベントがその接続します。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*パブリッシャー*イベントを提供するオブジェクトへの参照は、 *eventName*、および*methodName*に対応するシグネチャを持つサブスクライバーで定義されているイベント ハンドラーの名前を指定します*eventDelegate*します。 つまり、イベントのデリゲートの場合は`EventHandler`、し*methodName*値を返さず、2 つの入力の型のパラメーターを受け取るサブスクライバーでのメソッドの名前を指定する必要があります`Object`と`EventArgs`、それぞれします。

このイベントの配線コードでは、以降のポストバックでは、最初のページ アクセスで実行する必要があり、イベントが発生する可能性がありますの前のページのライフ サイクルの時点で発生する必要があります。 イベント接続コードを追加すると良いでは、PreInit ステージでは、ページのライフ サイクルの初期段階で発生します。

開いている`~/Admin/Products.aspx`を作成し、`Page_PreInit`イベント ハンドラー。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

この配線コードを完了するには、[コンテンツ] ページからマスター ページへのプログラムによる参照が必要です。 前述の前のチュートリアルでは、これを行う 2 つの方法があります。

- 厳密に型をキャストすることによって`Page.Master`プロパティを適切なマスター ページの種類、または
- 追加することで、`@MasterType`ディレクティブで、`.aspx`ページと厳密に型指定を使用して`Master`プロパティ。

後者のアプローチを使用してみましょう。 次の追加`@MasterType`ディレクティブを宣言型マークアップをページの上部に。


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

次のイベントの配線コードを追加し、`Page_PreInit`イベント ハンドラー。


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

配置でこのコードでは、[コンテンツ] ページに GridView が更新されるたびに、`DoublePrice`ボタンをクリックします。

図 8 と 9 は、この動作を示しています。 図 8 は、最初にアクセスする際、ページを示します。 両方の値を価格に注意してください、 `RecentProducts` (マスター ページの左側の列) で GridView と`ProductsGrid`GridView (で、[コンテンツ] ページ)。 図 9 番組の直後に画面と同じ、`DoublePrice`ボタンがクリックしてされました。 ご覧のように、新しい料金が両方の Gridview で瞬時に反映されます。


[![価格の初期値](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**図 08**: 初期価格値 ([フルサイズの画像を表示する をクリックします](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))。


[![Just-Doubled 価格は、Gridview に表示されます。](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**図 09**: The Just-Doubled 価格は、Gridview に表示されます ([フルサイズの画像を表示する をクリックします](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))。


## <a name="summary"></a>まとめ

理想的には、マスター ページとそのコンテンツ ページは互いから完全に分離し、操作のレベルは必要ありません。 ただし、マスター ページまたはマスター ページまたは [コンテンツ] ページから変更可能なデータを表示するコンテンツのページがある場合する必要がありますアラートの [コンテンツ] ページ (または、逆に) マスター ページが存在する、表示を更新できるようにデータが変更されたとき。 前のチュートリアルではコンテンツ ページがそのマスター ページとプログラムでやり取りする方法を説明しましたこのチュートリアルでは、マスター ページの開始、相互作用する方法を説明しました。

コンテンツとマスター ページの間のプログラムによる操作は、コンテンツまたはマスター ページから取得できます、発信元の依存対話パターンを使用します。 違いは、原因という事実にコンテンツ ページが単一のマスター ページですが多くのさまざまなコンテンツ ページにマスター ページを使用できます。 マスター ページのコンテンツ ページと直接対話するのではなくより優れたアプローチはいくつかの動作が発生したことを通知するイベントを発生させるマスター ページが存在するがします。 アクションを考慮するそれらのコンテンツ ページには、イベント ハンドラーを作成できます。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [アクセスして、ASP.NET のデータの更新](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [イベントとデリゲート](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [コンテンツとマスター ページの情報を渡す](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [ASP.NET のチュートリアルでのデータの使用](../../data-access/index.md)

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が、Suchi 著。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](interacting-with-the-master-page-from-the-content-page-cs.md)
> [次へ](master-pages-and-asp-net-ajax-cs.md)
