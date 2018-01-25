---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: "マスター ページ (VB) からコンテンツ ページと対話する |Microsoft ドキュメント"
author: rick-anderson
description: "マスター ページ内のコードからのコンテンツ ページのプロパティなどの設定、メソッドの呼び出し方法を説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: a25f739e5d5717d275554909e1584bb7e7fed302
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>マスター ページ (VB) からコンテンツ ページと対話します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> マスター ページ内のコードからのコンテンツ ページのプロパティなどの設定、メソッドの呼び出し方法を説明します。


## <a name="introduction"></a>はじめに

前のチュートリアルでは、コンテンツ ページをプログラムで、マスター ページと対話する方法を確認します。 最も最近、5 を表示する GridView コントロールを含めるようにマスター ページを更新して再呼び出しは、製品を追加します。 次のユーザーが新しい製品を追加でしたコンテンツ ページを作成しました。 新しい製品を追加するのには、コンテンツ ページは、マスター ページができるように、単に追加された製品が含まれますが、その GridView を更新するように指示に必要です。 この機能は、更新されたデータにバインドされている、GridView、マスター ページへのパブリック メソッドを追加して、コンテンツ ページからそのメソッドを呼び出すで可能でした。

コンテンツおよびマスター ページの相互作用の最も一般的な形式は、コンテンツ ページから発生します。 ただし、アクションに、現在のコンテンツ ページを rouse がマスター ページのことが、マスター ページには、ユーザーがコンテンツ ページにも表示されているデータを変更できるユーザー インターフェイス要素が含まれている場合、このような機能が必要な場合があります。 すべての製品の価格を倍に、コンテンツのページを使用している GridView の製品情報は、制御を表示し、ボタンを含むマスター ページを制御するには、クリックされたときにことを検討してください。 前のチュートリアルの例と同様に GridView が二重の価格、新しい価格を表示するようにボタンがクリックされた後に更新する必要がありますが、このシナリオでは、コンテンツ ページを rouse がアクションにする必要があるマスター ページ。

このチュートリアルでは、マスター ページがコンテンツ ページで定義されている機能を起動する方法について説明します。

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>イベントとイベント ハンドラーを使用してプログラムでの相互作用を仕掛ける

マスター ページからのコンテンツ ページの機能の呼び出しは、周囲の他の方法よりも困難です。 コンテンツ ページ、プログラムによるユーザーとの対話を仕掛けるときに、コンテンツ ページが単一のマスター ページは、パブリック メソッドとプロパティは、自由にわかっています。 ただし、マスター ページは、多くの異なるコンテンツ ページ、それぞれに独自のプロパティとメソッドのセットを持つことができます。 方法については、次に、おコードで記述わかりませんが実行時まで、どのようなコンテンツのページが呼び出されるときに、コンテンツ ページで何らかのアクションを実行するマスター ページですか。

ボタン コントロールなど、ASP.NET Web コントロールを検討してください。 ボタン コントロールは、ASP.NET ページの任意の数で表示できるし、メカニズム、アラートが通知ページがクリックしてされたことが必要です。 これは*イベント*です。 具体的には、ボタン コントロールを発生させるその`Click`イベント ボタンを含む ASP.NET ページが経由では、その通知に応答できます必要に応じてをクリックすると、*イベント ハンドラー*です。

この同じパターンは、そのコンテンツ ページで、マスター ページ トリガー機能させるために使用できます。

1. イベントは、マスター ページを追加します。
2. マスター ページがページは、コンテンツと通信する必要があるたびに、イベントが発生します。 たとえば、マスター ページは、そのコンテンツ ページに、ユーザーが、価格を倍増がアラートを生成する必要がある、そのイベントは発生価格を倍増した直後にします。
3. いくつかのアクションを実行する必要があるこれらのコンテンツ ページで、イベント ハンドラーを作成します。

このチュートリアルの残りの部分は、概要で説明した例を実装します。つまり、データベース内の製品を一覧表示するコンテンツ ページ、およびマスター ページのボタンが含まれている価格を倍に制御します。

## <a name="step-1-displaying-products-in-a-content-page"></a>手順 1: コンテンツ ページで製品を表示します。

最初の注文のビジネスでは、Northwind データベースから製品を一覧表示するコンテンツ ページを作成します。 (前のチュートリアルでは、プロジェクトに Northwind データベースを追加お[*コンテンツ ページから、マスター ページと対話する*](interacting-with-the-master-page-from-the-content-page-vb.md))。新しい ASP.NET ページを追加することで開始、`~/Admin`という名前のフォルダー `Products.aspx`、バインドすることを確認して、`Site.master`マスター ページ。 図 1 は、このページは、web サイトに追加した後に、ソリューション エクスプ ローラーを示します。


[![Admin フォルダーに新しい ASP.NET ページを追加します。](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**図 01**: に新しい ASP.NET ページの追加、`Admin`フォルダー ([フルサイズのイメージを表示するをクリックして](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


注意してください、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)という名前のカスタム ベース ページ クラスを作成したチュートリアル`BasePage`されていない場合、ページのタイトルを生成します。明示的に設定します。 移動して、`Products.aspx`ページの分離コード クラスをから派生して`BasePage`(の代わりにから`System.Web.UI.Page`)。

最後に、更新、`Web.sitemap`このレッスンのエントリを追加するファイル。 下に次のマークアップを追加、`<siteMapNode>`マスター ページの相互作用のレッスンにコンテンツの。


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

この追加`<siteMapNode>`要素は、レッスンに反映されます (図 5 を参照してください) を一覧表示します。

戻る`Products.aspx`です。 コンテンツ コントロールで`MainContent`GridView コントロールを追加し、名前、`ProductsGrid`です。 GridView をという名前の新しい SqlDataSource コントロールにバインド`ProductsDataSource`です。


[![SqlDataSource コントロールを新しい GridView にバインドします。](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**図 02**: 新しい SqlDataSource コントロール GridView にバインド ([フルサイズのイメージを表示するをクリックして](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


ウィザードは、Northwind データベースを使用するように構成します。 かどうかは、前のチュートリアルを実行して、名前付き接続文字列が必要`NorthwindConnectionString`で`Web.config`です。 図 3 に示すように、ドロップダウン リストからこの接続文字列を選択します。


[![Northwind データベースを使用する SqlDataSource を構成します。](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**図 03**: Northwind データベースを使用する SqlDataSource を構成する ([フルサイズのイメージを表示するをクリックして](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


次に、指定、データ ソース コントロールの`SELECT`ステートメント、Products テーブルのドロップダウン リストから選択し、返すことで、`ProductName`と`UnitPrice`列 (図 4 を参照してください)。 [次へ] をクリックし、データ ソースの構成ウィザードを完了し、[完了] です。


[![Products テーブルから ProductName および UnitPrice フィールドを返します](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**図 04**: を返す、`ProductName`と`UnitPrice`からのフィールド、`Products`テーブル ([フルサイズのイメージを表示するをクリックして](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


すべてであることには! ウィザードの完了後は、Visual Studio は、SqlDataSource コントロールによって返される 2 つのフィールドをミラーリングする GridView に 2 つの BoundFields を追加します。 GridView と SqlDataSource コントロールのマークアップが次に示します。 図 5 は、ブラウザーで表示した場合の結果を示します。


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![各製品との価格が GridView に一覧表示します。](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**図 05**: 各製品との価格が GridView に一覧表示 ([フルサイズのイメージを表示するをクリックして](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> 自由 GridView の外観をクリーンアップしてください。 通貨として表示される UnitPrice の値を書式設定と背景色とフォントを使用して、グリッドの外観を向上させるために、いくつかの提案が含まれます。 表示して、ASP.NET でのデータの書式設定の詳細についてを参照してください [チュートリアル シリーズのデータの操作](../../data-access/index.md)です。


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>手順 2: マスター ページへの二重価格ボタンの追加

次のタスクを追加するボタン Web マスター ページのコントロールをクリックすると、データベース内のすべての製品の価格倍精度浮動小数点です。 開く、`Site.master`マスター ページと、デザイナーの下に配置するのには、ツールボックスからボタンをドラッグして、 `RecentProductsDataSource` SqlDataSource コントロールを前のチュートリアルで追加されました。 ボタンの設定`ID`プロパティを`DoublePrice`とその`Text`「二重製品価格」プロパティです。

次に、名前、マスター ページに、SqlDataSource コントロールを追加`DoublePricesDataSource`です。 実行に使用されるこの SqlDataSource、`UPDATE`ステートメントのすべての価格を倍にします。 具体的を設定する必要があります、`ConnectionString`と`UpdateCommand`を適切な接続文字列プロパティと`UPDATE`ステートメントです。 この SqlDataSource コントロールを呼び出す必要があります、`Update`メソッドときに、`DoublePrice`ボタンをクリックします。 設定する、`ConnectionString`と`UpdateCommand`プロパティ、SqlDataSource コントロールを選択し、[プロパティ] ウィンドウ。 `ConnectionString`プロパティ リストに既に格納されているこれらの接続文字列`Web.config`ドロップダウン リストでは; を選択、`NorthwindConnectionString`図 6 に示すようにオプションです。


[![構成、NorthwindConnectionString を使用する SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**図 06**: 構成を使用する SqlDataSource、 `NorthwindConnectionString` ([フルサイズのイメージを表示するをクリックして](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


設定する、`UpdateCommand`プロパティ、[プロパティ] ウィンドウで抽出オプションを探します。 このプロパティは、選択した場合、省略記号; でボタンを表示します。図 7 に示すようにコマンドおよびパラメーターのエディター ダイアログ ボックスを表示するには、このボタンをクリックします。 次の入力`UPDATE`ステートメント ダイアログ ボックスのテキスト ボックスに。


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

実行すると、このステートメントが 2 倍、`UnitPrice`内の各レコードの値、`Products`テーブル。


[![SqlDataSource の UpdateCommand プロパティを設定します。](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**図 07**: 設定 SqlDataSource の`UpdateCommand`プロパティ ([フルサイズのイメージを表示するをクリックして](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


これらのプロパティを設定した後、ボタンと SqlDataSource コントロールの宣言型マークアップを次のようになります。


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

すべてのタスクが呼び出すその`Update`メソッドときに、`DoublePrice`ボタンをクリックします。 作成、`Click`のイベント ハンドラー、`DoublePrice`ボタンをクリックし、次のコードを追加します。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

この機能をテストするを参照してください。、`~/Admin/Products.aspx`ページお手順 1. で作成し、"二重製品価格"ボタンをクリックします。 ポストバックを発生させるボタンをクリックし、実行、`DoublePrice`ボタンの`Click`イベント ハンドラーですべての製品の価格を倍増します。 ページが再表示し、およびマークアップが返され、ブラウザーで再表示されます。 ただし、コンテンツ ページで、GridView は、「二重製品価格」の前にボタンがクリックされたと同じ料金が一覧表示します。 これは、GridView で最初に読み込まれたデータの状態のビュー ステートに格納されるため、それ以外の場合に指示がない限り、ポストバックがこれには読み込まれませんされていたためです。 別のページを参照してくださいに戻るかどうか、`~/Admin/Products.aspx`ページが更新された価格が表示されます。

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>手順 3: が 2 倍になりますし、イベントの価格をさせるとは

の GridView、`~/Admin/Products.aspx`ページがすぐに反映されない、価格を倍増、ユーザーが「二重製品価格」ボタンをクリックしなかったこと、または動作していないことに考えられることは明確です。 しようとするかもしれません ボタンをクリックすると少し時間を繰り返し、価格を倍増します。 コンテンツ内のグリッドを確保する必要がありますこれを解決するには、ページは、これらは 2 倍になった後にすぐに新しい価格を表示します。

このチュートリアルで前述したよう、ユーザーがクリックされるたびに、マスター ページでイベントを発生させる必要があります、`DoublePrice`ボタンをクリックします。 イベントは、1 つのクラス (イベント パブリッシャー) を別に何か興味深いものが発生したその他のクラス (イベント サブスクライバー) のセットを通知する方法です。 この例では、マスター ページは、イベント パブリッシャーです。これらのコンテンツの場合を考慮するページ、`DoublePrice`ボタンがクリックされたサブスクライバーは、します。

クラスを作成してイベントにサブスクライブしている、*イベント ハンドラー*メソッドは、発生するイベントに応答を実行します。 パブリッシャーが定義することで彼を発生させるイベントを定義、*イベント デリゲート*です。 イベントのデリゲートでは、どのような入力パラメーターが、イベント ハンドラーに同意する必要がありますを指定します。 .NET Framework でイベント デリゲートは値を返さない任意して 2 つの入力パラメーターを受け取る。

- `Object`、イベント ソースを識別し、
- 派生したクラス`System.EventArgs`

イベント ハンドラーに渡される 2 番目のパラメーターは、イベントに関する追加情報を含めることができます。 ときに、ベース`EventArgs`クラスがすべての情報を渡すされません、.NET Framework には拡張するクラスの数が含まれています`EventArgs`と、追加のプロパティが含まれます。 たとえば、`CommandEventArgs`インスタンスに応答するイベント ハンドラーに渡されます、`Command`イベント、情報の 2 つのプロパティが含まれています:`CommandArgument`と`CommandName`です。

> [!NOTE]
> 作成する方法の詳細については、させると、イベントを処理を参照してください[イベントとデリゲート](https://msdn.microsoft.com/library/17sde2xt.aspx)と[単純英語におけるイベント デリゲート](http://www.codeproject.com/KB/cs/eventdelegates.aspx)です。


定義するのには、イベントは、次の構文を使用します。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

のみ、コンテンツ ページをユーザーがクリックされたときにアラートを生成する必要がありますので、`DoublePrice`ボタンをクリックし、その他の追加情報を渡す必要はありません、イベント デリゲートを使用して`EventHandler`、2 番目としてを受け取るイベント ハンドラーを定義します。パラメーターの型のオブジェクト`System.EventArgs`です。 マスター ページで、イベントを作成するには、マスター ページの分離コード クラスに次のコード行を追加します。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

上記のコードは、という名前のマスター ページにパブリック イベントを追加`PricesDoubled`です。 これで、価格を倍増した後にこのイベントを発生させる必要があります。 発生させるためには、イベントは、次の構文を使用します。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

場所*送信者*と*eventArgs*サブスクライバーのイベント ハンドラーに渡す値を示します。

更新プログラム、 `DoublePrice` `Click`イベント ハンドラーを次のコードで。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

前とに、、`Click`を呼び出してイベント ハンドラーを開始、 `DoublePricesDataSource` SqlDataSource コントロールの`Update`メソッドのすべての製品の価格を倍にします。 次のイベント ハンドラーに次の 2 つの追加機能があること。 最初に、 `RecentProducts` GridView のデータを更新します。 この GridView は、前のチュートリアルでマスター ページに追加されており、最も最近追加した 5 つの製品が表示されます。 だけの 2 倍になりますのこれら 5 つの製品の価格が表示されるように、このグリッドを更新する必要があります。 次に、`PricesDoubled`イベントが発生します。 マスター ページ自体への参照を (`Me`) は、イベント ソースと、空としてイベント ハンドラーに送信`EventArgs`オブジェクトは、イベントの引数として送信します。

## <a name="step-4-handling-the-event-in-the-content-page"></a>手順 4: コンテンツのページでイベントを処理します。

この時点で、マスター ページを発生させますその`PricesDoubled`イベントされるたびに、`DoublePrice`ボタン コントロールをクリックします。 ただし、これは、半分に過ぎませんにサブスクライバーでイベントを処理する必要があります。 これは、2 つの手順: イベント ハンドラーを作成し、イベントが発生したときに、イベント ハンドラーが実行されるようにイベント配線コードを追加します。

名前付きイベント ハンドラーを作成して開始`Master_PricesDoubled`です。 どのように定義したため、`PricesDoubled`マスター ページ内のイベントの種類のイベント ハンドラーの 2 つの入力パラメーターがある必要があります`Object`と`EventArgs`、それぞれします。 イベント ハンドラーの呼び出しで、 `ProductsGrid` GridView の`DataBind`グリッドにデータを再バインドするメソッド。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

イベント ハンドラーのコードが完了しましたが、きたところ、ネットワーク上で、マスター ページの`PricesDoubled`イベントをこのイベント ハンドラー。 サブスクライバーは、次の構文を使用してイベント ハンドラーにイベントをワイヤします。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*パブリッシャー*イベントを提供するオブジェクトへの参照は、 *eventName*、および*methodName*サブスクライバーで定義されたイベント ハンドラーの名前を指定します。

このイベント配線コードでは、初めてページおよび以降のポストバックを実行する必要があり、イベントを発生させる可能性があるときにあるページ ライフ サイクルの時点で発生する必要があります。 イベント配線コードを追加する絶好のタイミングでは、PreInit ステージでは、ページのライフ サイクルの初期段階で発生します。

開いている`~/Admin/Products.aspx`を作成し、`Page_PreInit`イベントのハンドラー。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

この配線コードを完了するために、マスター ページへのプログラムによる参照コンテンツ ページから必要があります。 述べたように前のチュートリアルは、これを行う 2 つの方法があります。

- 厳密に型をキャスト`Page.Master`プロパティを適切なマスター ページ型、または
- 追加することによって、`@MasterType`ディレクティブで、`.aspx`ページし、使用して、厳密に型指定された`Master`プロパティです。

後者のアプローチを使用してみましょう。 次の追加`@MasterType`ディレクティブをページの宣言型マークアップの上部に。


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

次のイベント配線コードを追加、`Page_PreInit`イベントのハンドラー。


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

配置でこのコードでは、コンテンツ ページの GridView が更新されるたびに、`DoublePrice`ボタンをクリックします。

図 8 と 9 は、この動作を示します。 図 8 は、最初にアクセスされるときに、ページを示します。 両方の値を価格を`RecentProducts`(マスター ページの左の列) に GridView と`ProductsGrid`(コンテンツ ページ) での GridView です。 図 9 番組の直後に画面と同じ、`DoublePrice`ボタンがクリックしてされました。 ご覧のように、新しい料金は両方 Gridview に瞬時に反映されます。


[![初期の価格値](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**図 08**: 初期価格値 ([フルサイズのイメージを表示するをクリックして](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Just-Doubled 価格は、Gridview に表示されます。](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**図 09**: The Just-Doubled 価格は、Gridview に表示されます ([フルサイズのイメージを表示するをクリックして](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>まとめ

理想的には、マスター ページとそのコンテンツ ページ互いから完全に分離し、の相互作用のレベルは必要ありません。 ただし、マスター ページまたはマスター ページまたはコンテンツ ページから変更できるようにデータを表示するコンテンツのページがある場合は、可能性がある場合にアラートのコンテンツ ページ (または逆に) マスター ページの表示を更新できるようににデータが変更された日時。 前のチュートリアルのコンテンツ ページにプログラムから、マスター ページと対話する方法を説明しましたこのチュートリアルでは、マスター ページ開始の相互作用する方法について説明しました。

コンテンツとマスター ページ間のプログラムによる操作は、コンテンツまたはマスター ページから取得できます、相互作用のパターンを使用する発信元に依存します。 違いは、ために、コンテンツ ページを単一のマスター ページですがマスター ページは、多くのさまざまなコンテンツ ページを持つことができます。 コンテンツ ページと直接対話するマスター ページはなくより適切な方法はいくつかの動作が発生したことを通知するイベントを発生させるマスター ページが存在するがします。 アクションを考慮するそれらのコンテンツ ページには、イベント ハンドラーを作成できます。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [アクセスして、ASP.NET でのデータの更新](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [イベントとデリゲート](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [コンテンツおよびマスター ページの間で情報を渡す.](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [ASP.NET のチュートリアルでのデータの使用](../../data-access/index.md)

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Suchi Banerjee しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](interacting-with-the-master-page-from-the-content-page-vb.md)
[次へ](master-pages-and-asp-net-ajax-vb.md)
