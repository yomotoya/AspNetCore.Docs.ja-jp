---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: ASP.NET Web Pages (Razor) を持つグラフ データの表示 |Microsoft ドキュメント
author: microsoft
description: この章では、グラフのデータを表示する方法について説明します。 前の章では、手動で、グリッドでデータを表示する方法について学習しました。 この章で説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 5cf17e54408d585e9a375b302b61b4e28d9b022a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899724"
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>ASP.NET Web Pages (Razor) を持つグラフ データを表示します。
====================
によって[Microsoft](https://github.com/microsoft)

> この記事は、グラフを使用して、使用して ASP.NET Web Pages (Razor) の web サイトにデータを表示する方法を説明します、`Chart`ヘルパー。
> 
> **学習する内容**:
> 
> - グラフのデータを表示する方法です。
> - 組み込みのテーマを使用して、グラフのスタイルを設定する方法。
> - グラフを保存する方法とパフォーマンス向上のためにキャッシュする方法です。
> 
> これらは、ASP.NET のアーティクルで導入された機能をプログラミングします。
> 
> - `Chart`ヘルパー。
> 
> > [!NOTE]
> > この記事の情報は、ASP.NET Web Pages 1.0 および Web Pages 2 に適用されます。


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>グラフ ヘルパー

グラフィカルなフォームにデータを表示するときに行うこともできます`Chart`ヘルパー。 `Chart`ヘルパーは、さまざまな種類のグラフでデータを表示する画像を表示できます。 書式設定と、ラベル付け、多くのオプションをサポートしています。 `Chart`ヘルパーが 30 以上の種類のグラフ、グラフの可能性がある使い慣れた Microsoft Excel または他のツールのすべての種類を含むをレンダリングできる&#8212;面グラフ、横棒グラフ、縦棒グラフ、折れ線グラフ、および円グラフは、複数の特殊なグラフでは、株価チャートと同様にします。

| **面グラフ**![説明: 面グラフの種類の画像](7-displaying-data-in-a-chart/_static/image1.jpg) | **横棒グラフ**![説明: 横棒グラフの画像](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **縦棒グラフ**![説明: 縦棒グラフの画像](7-displaying-data-in-a-chart/_static/image3.jpg) | **折れ線グラフ**![説明: ライン グラフの種類の画像](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **円グラフ**![説明: 円グラフの種類の画像](7-displaying-data-in-a-chart/_static/image5.jpg) | **株価チャート**![説明: 株価チャートの画像](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>グラフ要素

グラフは、データと凡例、軸、系列、およびなどのような追加の要素を示しています。 次の図は多くのグラフ要素を使用するときに、カスタマイズできる、`Chart`ヘルパー。 この記事は、いくつかを設定する方法を示します (すべて) これらの要素。

![説明: 画像、グラフ要素](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>データからグラフを作成します。

グラフに表示するデータには、データベースから返される結果から、配列から、または XML ファイルに含まれるデータからを指定できます。

### <a name="using-an-array"></a>配列を使用します。

説明したよう[ASP.NET Web ページは、Razor 構文を使用プログラミングの概要](https://go.microsoft.com/fwlink/?LinkId=202890)配列では、1 つの変数に類似した項目のコレクションを格納することができます。 配列を使用して、グラフに含めるデータを格納することができます。

この手順では、作成する方法、グラフ、配列内のデータから既定のグラフの種類を使用してを示します。 ページ内のグラフを表示する方法も示しています。

1. という名前の新しいファイルを作成する*ChartArrayBasic.cshtml*です。
2. 次のように、既存のコンテンツを置き換えます。 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    最初のコードは、新しいグラフを作成し、幅と高さを設定します。 使用して、グラフのタイトルを指定する、`AddTitle`メソッドです。 使用するデータを追加する、`AddSeries`メソッドです。 この例で使用して、 `name`、 `xValue`、および`yValues`のパラメーター、`AddSeries`メソッドです。 `name`パラメーターは、グラフの凡例に表示されます。 `xValue`パラメーターには、グラフの水平軸に沿って表示されるデータの配列が含まれています。 `yValues`パラメーターには、グラフの垂直ポイントをプロットに使用されるデータの配列が含まれています。

    `Write`メソッドが実際には、グラフを表示します。 ここでは、グラフの種類を指定していないため、`Chart`ヘルパー表示の既定のグラフ、縦棒グラフであります。
3. ブラウザーでページを実行します。 ブラウザーでは、グラフが表示されます。 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>グラフ データのデータベース クエリを使用します。

グラフ化情報がデータベース内にある場合は、データベース クエリを実行し、結果からデータを使用して、グラフを作成することができます。 この手順は、読み取りおよび」で作成、データベースからデータを表示する方法を示します[ASP.NET Web Pages サイトでのデータベース操作の概要](https://go.microsoft.com/fwlink/?LinkId=202893)です。

1. 追加、*アプリ\_データ*フォルダーが既に存在しない場合、web サイトのルート フォルダー。
2. *アプリ\_データ*フォルダー、という名前のデータベース ファイルを追加*SmallBakery.sdf*に記載されている[ASP.NETWebPagesサイトでのデータベース操作の概要](https://go.microsoft.com/fwlink/?LinkId=202893).
3. という名前の新しいファイルを作成する*ChartDataQuery.cshtml*です。
4. 次のように、既存のコンテンツを置き換えます。   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    コードは、まずデータベースを開く SmallBakery/という名前の変数に割り当てます`db`です。 この変数を表します、`Database`から読み取るし、データベースへの書き込みに使用できるオブジェクト。 次に、コードでは、各製品の価格と名前を取得する SQL クエリが実行されます。 コードは、新しいグラフを作成し、グラフを呼び出すことによって、データベース クエリを渡します`DataBindTable`メソッドです。 このメソッドは、2 つのパラメーター:`dataSource`パラメーターは、クエリからデータと`xField`パラメーターを使用して、グラフの x 軸を使用するデータ列を設定できます。

    使用する代わりに、`DataBindTable`メソッドを使用できます、`AddSeries`のメソッド、`Chart`ヘルパー。 `AddSeries`を設定する方法と、`xValue`と`yValues`パラメーター。 使用せずになど、`DataBindTable`次のようなメソッド。

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    使用することができます、`AddSeries`次のようなメソッド。

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    両方ともは、同じ結果を表示します。 `AddSeries`メソッドは、グラフの種類とデータをより明示的に指定することができますのでより柔軟ですが、`DataBindTable`メソッドは簡単に余分な柔軟性を必要としない場合に使用します。
5. ブラウザーでページを実行します。 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>XML データを使用します。

グラフ化するための 3 番目のオプションは、グラフのデータとして XML ファイルを使用することです。 XML ファイルにも、スキーマ ファイルがあることが必要です (*.xsd*ファイル)、XML 構造を説明します。 この手順では、XML ファイルからデータを読み取る方法を示します。

1. *アプリ\_データ*フォルダー、という名前の新しい XML ファイルを作成する*data.xml*です。
2. 既存の XML を架空の会社の従業員に関する XML データの一部であると、次に置き換えます。 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. *アプリ\_データ*フォルダー、という名前の新しい XML ファイルを作成する*data.xsd*です。 (この時間は、拡張機能を *.xsd*)。
4. 次のように既存の XML に置き換えます。 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Web サイトのルートで、という名前の新しいファイルを作成する*ChartDataXML.cshtml*です。
6. 次のように、既存のコンテンツを置き換えます。 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    このコードはまずを作成、`DataSet`オブジェクト。 このオブジェクトは、データは、XML ファイルから読み取られ、スキーマ ファイルの情報に従って整理を管理に使用されます。 (コードの先頭に、ステートメントが含まれている通知`using SystemData`です。 これは、正常に動作するために必要、`DataSet`オブジェクト。 詳細については、次を参照してください[ &quot;Using&quot;ステートメントおよび完全修飾名](#SB_UsingStatements)この記事で後述します。)。

    このコードを次に、作成、`DataView`データセットに基づいてオブジェクト。 データ ビューのグラフにバインドできるオブジェクトを提供&#8212;は、読み取りし、プロットします。 グラフにデータを使用して、バインド、`AddSeries`メソッドとして、前述の以外、配列のデータをこの時間のグラフ化、`xValue`と`yValues`パラメーターに設定されて、`DataView`オブジェクト。

    この例は、特定のグラフの種類を指定する方法も示します。 データを追加すると、`AddSeries`メソッド、`chartType`円グラフを表示するもパラメーターを設定します。
7. ブラウザーでページを実行します。 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Using"ステートメントと完全修飾名
> 
> Razor 構文を使用して ASP.NET Web Pages の基になっている、.NET Framework は、何千ものコンポーネント (クラス) で構成されます。 これらすべてのクラスを使用する管理しやすいように、構成している*名前空間*ライブラリのようなものであります。 たとえば、`System.Web`名前空間には、ブラウザー/サーバー間の通信をサポートするクラスが含まれています、`System.Xml`名前空間には作成し、XML ファイルの読み取りに使用されるクラスが含まれています、`System.Data`名前空間には、作業することのできるクラスが含まれています。データです。
> 
> .NET Framework 内の特定のクラスにアクセスするためにコードをクラス名だけでなくも、クラスが含まれている名前空間を知っている必要があります。 例については、使用するために、`Chart`ヘルパーに渡し、コードを見つける必要がある、`System.Web.Helpers.Chart`を組み合わせたもので、名前空間クラス (`System.Web.Helpers`) とクラス名 (`Chart`)。 これは、クラスと呼ばれます*完全修飾*名前&#8212;あいまいでない完全内での位置広大、.NET Framework のです。 コードでは、これは、次のようになります。
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> ただし、これは面倒な (とエラーが発生しやすい) クラスまたはヘルパーを参照するたびに、これらの長い、完全修飾名を使用します。 そのため、クラス名を使用して容易にできるようにを実行できます*インポート*これは通常、関心がある名前空間は、.NET Framework で多数の名前空間の中から、いくつかだけです。 名前空間をインポートした場合は、クラス名のみを使用することができます (`Chart`) 完全修飾名ではなく (`System.Web.Helpers.Chart`)。 コードを実行し、クラス名を検出すると、そのクラスの検索にインポートした名前空間だけが参照してください。
> 
> Razor 構文を使用する ASP.NET Web Pages を使用して web ページを作成するときに通常使用する同じ一連のクラスごとに含め、`WebPage`クラスや、さまざまなヘルパー。 Web サイトを作成するたびに、関連する名前空間をインポートする作業を保存するため、すべての web サイトのコア名前空間のセットを自動的にインポート ASP.NET が構成されています。 その名前空間または今のところまでインポートを処理する必要がありましたしていません。作業したすべてのクラスは、既にインポートされる名前空間でです。
> 
> ただし、場合によっては自動的にインポートする名前空間にないクラスを使用する必要があります。 その場合は、そのクラスの完全修飾名を使用できます。 または、クラスが含まれている名前空間を手動でインポートすることができます。 使用する名前空間をインポートするには`using`ステートメント (`import` Visual Basic で) 前述の例で、アーティクルです。
> 
> たとえば、`DataSet`クラスが含まれて、`System.Data`名前空間。 `System.Data`名前空間が自動的に ASP.NET Razor ページを使用できます。 そのため、使用する、`DataSet`クラスの完全修飾名を使用して、次のようにコードを使用することができます。
> 
> `var dataSet = new System.Data.DataSet();`
> 
> 使用する必要がある場合、`DataSet`繰り返しクラスを次のように名前空間をインポートおよびコードでクラス名のみを使用できます。
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> 追加することができます`using`その他の .NET Framework 名前空間を参照するためのステートメント。 ただし、前述のように、必要はありませんこれを行う多くの場合、ほとんどの演習ではクラスで使用するために、ASP.NET によって自動的にインポートされる名前空間であるため *.cshtml*と *.vbhtml*ページ。


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Web ページ内のグラフを表示します。

例で、これまで、グラフを作成して、そのグラフがグラフィックとしてブラウザーに直接表示されます。 多くの場合、ただし、するブラウザーでそれ自体ではなく、ページの一部として、グラフが表示されます。 これを行うには、2 段階のプロセスが必要です。 最初の手順では、既に説明したように、グラフを生成するページを作成します。

2 番目の手順では、別のページで、結果のイメージを表示します。 イメージを表示するには、HTML を使用する`<img>`同じ内の要素とすべてのイメージを表示する方法です。 参照するのではなく、ただし、 *.jpg*または *.png*ファイル、`<img>`要素の参照、 *.cshtml*を含むファイル、`Chart`ヘルパーをグラフを作成します。 表示ページを実行すると、`<img>`要素の出力の取得、`Chart`ヘルパーし、グラフを表示します。

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. という名前のファイルを作成する*ShowChart.cshtml*です。
2. 次のように、既存のコンテンツを置き換えます。 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    コードを使用して、`<img>`要素の前に作成したグラフを表示、 *ChartArrayBasic.cshtml*ファイル。
3. ブラウザーで web ページを実行します。 *ShowChart.cshtml*ファイルに含まれるコードに基づいてグラフ イメージを表示する、 *ChartArrayBasic.cshtml*ファイル。

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>グラフのスタイルを設定

`Chart`ヘルパーは、多数のグラフの外観をカスタマイズするためのオプションをサポートしています。 色、フォント、罫線、およびなどを設定することができます。 グラフの外観をカスタマイズする簡単な方法が使用するには、*テーマ*です。 テーマは、フォント、色、ラベル、パレット、罫線、および特殊効果を使用してグラフを表示する方法を指定する情報のコレクションです。 (グラフのスタイルがグラフの種類を示さないことに注意してください)。

次の表は、組み込みのテーマを一覧表示します。

| テーマ | 説明 |
| --- | --- |
| `Vanilla` | 白色の背景の赤の列を表示します。 |
| `Blue` | 青色のグラデーションの背景色を青に列が表示されます。 |
| `Green` | 青の緑のグラデーションの背景の列が表示されます。 |
| `Yellow` | グラデーションの背景が黄色いオレンジ色の列を表示します。 |
| `Vanilla3D` | 白い背景上には、3-D 赤い列を表示します。 |

新しいグラフを作成するときに使用するテーマを指定することができます。

1. という名前の新しいファイルを作成する*ChartStyleGreen.cshtml*です。
2. ページで、既存のコンテンツを次のように置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    このコードは、データのデータベースを使用しますが、追加する前の例と同じ、`theme`パラメーターを作成するとき、`Chart`オブジェクト。 変更後のコードを次に示します。

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. ブラウザーでページを実行します。 前に、同じデータを確認することが、グラフがより洗練された検索します。 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>グラフの保存

使用すると、`Chart`としてするヘルパーこれまで、この記事でヘルパー再作成を最初からグラフが呼び出されるたびにします。 必要に応じて、グラフのコードもデータベースを再クエリか、データを取得する XML ファイルを再読み込みされます。 場合によっては、これを行うようにできます複雑な処理でクエリを実行しているデータベースは、大きい場合、または XML ファイルには、大量データにはが含まれています。 グラフは、大量のデータが関係していない、場合でも、イメージを動的に作成するのにかかる時間、サーバーのリソースをし、多くの人は、グラフを表示するページ要求が場合があります影響を与える、web サイトのパフォーマンスにします。

パフォーマンス グラフを作成する場合の影響を軽減するために、グラフ、最初の時刻を作成することができますし、保存し、それを必要とします。 グラフは、再生成するのではなく、必要なときにだけ、保存したバージョンを取得し、レンダリングできますをします。

次のようにグラフを保存できます。

- (サーバー) 上のコンピューターのメモリ グラフをキャッシュします。
- グラフをイメージ ファイルとして保存します。
- グラフを XML ファイルとして保存します。 このオプションを使用して、保存する前に、グラフを変更できます。

### <a name="caching-a-chart"></a>グラフをキャッシュ

グラフを作成した後は、キャッシュすることができます。 グラフをキャッシュすると、再表示する必要がある場合に、再作成するがないことを意味します。 グラフをキャッシュに保存するときに付けることできますそのグラフに一意にする必要があるキー。

サーバーのメモリが不足する場合は、キャッシュに保存されたグラフを削除する可能性があります。 さらに、何らかの理由で、アプリケーションが再起動した場合、キャッシュがクリアされます。 そのため、キャッシュされたグラフを使用する標準的な方法を常に、最初は、キャッシュに使用できるかどうか、チェックされていない場合にし、作成または再作成します。

1. Web サイトのルート ディレクトリにある、という名前のファイルを作成する*ShowCachedChart.cshtml*です。
2. 次のように、既存のコンテンツを置き換えます。 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>`タグが含まれています、`src`属性を指す、 *ChartSaveToCache.cshtml*ファイルし、クエリ文字列としてそのページに、キーを渡します。 キー値に含まれる&quot;myChartKey&quot;です。 *ChartSaveToCache.cshtml*ファイルが含まれています、`Chart`グラフを作成するヘルパー。 このページは、すぐに作成します。

    ページの最後には、という名前のページへのリンクは*ClearCache.cshtml*です。 間もなくも作成するページです。 必要があります、 *ClearCache.cshtml*この例のキャッシュをテストのみに — リンクまたはキャッシュされたグラフを使用する場合を含む、通常のページではありません。
3. Web サイトのルート ディレクトリにある、という名前の新しいファイルを作成する*ChartSaveToCache.cshtml*です。
4. 次のように、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    コードでは、まず、クエリ文字列のキー値として渡されたものかどうかを確認します。 そのため、コードを呼び出すことによってキャッシュからグラフを読み取るしようとした場合、`GetFromCache`メソッドと、キーを渡します。 判明した場合 (これは、グラフが要求された最初の時間が発生する) キーの下にあるキャッシュでは nothing を使用する必要があること、コードは、グラフを通常どおり作成します。 グラフが完了したら、コードに保存、キャッシュを呼び出して`SaveToCache`です。 そのメソッドでは、キー (つまり、グラフは、後で要求されることができます)、およびグラフをキャッシュに保存する時間が必要です。 (正確な時間グラフをキャッシュするようによりますどのくらいの頻度と思っていたが表すデータを変更する可能性があります。)`SaveToCache`メソッドも必要です、`slidingExpiration`パラメーター&#8212;設定されている場合を true に、タイムアウト カウンターのグラフがアクセスされるたびにはリセットされます。 この場合、グラフのキャッシュ エントリが 2 分後に、グラフのユーザーがアクセスを有効期限が切れる有効で意味します。 (スライディング期限を使用しない場合は、絶対有効期限、キャッシュ エントリがどのくらいの頻度アクセスされていたに関係なく、キャッシュに作成された後に、正確に 2 分を期限切れには意味です)。

    最後に、コードを使用して、`WriteFromCache`をフェッチしてキャッシュからグラフを表示するメソッド。 このメソッドの外側、`if`グラフは、最初から存在していたまたは生成され、キャッシュに保存する必要があるかどうかは、キャッシュからグラフが取得されるため、キャッシュがチェックをブロックします。

    例では、ことに注意して、`AddTitle`メソッドには、タイムスタンプが含まれています。 (現在の日付と時刻が追加されます&#8212; `DateTime.Now` &#8212;タイトルにします)。
5. という名前の新しいページを作成する*ClearCache.cshtml*し、そのコンテンツを次に置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    このページを使用して、`WebCache`にキャッシュされているグラフを削除するためのヘルパー *ChartSaveToCache.cshtml*です。 前述のように、する通常このようなページが存在する必要はありません。 ここにキャッシュをテストするが容易にのみ作成しています。
6. 実行、 *ShowCachedChart.cshtml*ブラウザーで web ページ。 含まれるコードに基づいてグラフ イメージを表示、 *ChartSaveToCache.cshtml*ファイル。 グラフのタイトルに表示するタイムスタンプを書き留めます。 

    ![グラフのタイトルのタイムスタンプを含む基本的なグラフの説明: 画像](7-displaying-data-in-a-chart/_static/image13.jpg)
7. ブラウザーを閉じます。
8. 実行、 *ShowCachedChart.cshtml*もう一度です。 タイムスタンプが同じである前とに、、グラフを再生成されませんでしたが、代わりに、キャッシュから読み取られたことを示すことを確認します。
9. *ShowCachedChart.cshtml*をクリックして、**キャッシュをクリア**リンクします。 これにかかる時間を*ClearCache.cshtml*キャッシュがクリアされたことを報告します。
10. クリックして、 **ShowCachedChart.cshtml に戻る**リンク、または再実行*ShowCachedChart.cshtml* WebMatrix からです。 キャッシュが消去されているためこの時点のタイムスタンプが変更されたことに注意してください。 そのため、コードをグラフを再生成し、キャッシュに戻す必要があります。

### <a name="saving-a-chart-as-an-image-file"></a>グラフの画像ファイルとして保存

グラフ画像ファイルとして保存することもできます (たとえば、として、 *.jpg*ファイル) サーバー上。 イメージ ファイルは、任意の画像のような手法し、使用できます。 利点は、ファイルが格納されているではなく一時的なキャッシュに保存します。 (たとえば、1 時間ごと) の異なる時間に新しいグラフ イメージを保存し、時間の経過と共に発生する変更の永続的な記録を保持できます。 Web アプリケーションにイメージ ファイルを配置するサーバーのフォルダーにファイルを保存する権限があることを確認する必要がありますに注意してください。

1. Web サイトのルート ディレクトリにある、という名前のフォルダーを作成する *\_ChartFiles*が既に存在しない場合。
2. Web サイトのルート ディレクトリにある、という名前の新しいファイルを作成する*ChartSave.cshtml*です。
3. 次のように、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    コードが最初に確認を表示するかどうか、 *.jpg*呼び出すことによってファイルが存在する、`File.Exists`メソッドです。 ファイルが存在しない場合、コードでは、新しい`Chart`配列からです。 今度は、コードの呼び出し、`Save`メソッドおよびパス、`path`パラメーター ファイルのパスと、グラフの保存先のファイル名を指定します。 ページの本文に、`<img>`要素では、パスを使用 をポイントして、 *.jpg*に表示されるファイルです。
4. 実行、 *ChartSave.cshtml*ファイル。
5. WebMatrix に戻ります。 イメージ ファイルの名前に注意してください*chart01.jpg*に保存されている、  *\_ChartFiles*フォルダーです。

### <a name="saving-a-chart-as-an-xml-file"></a>グラフを XML ファイルとして保存

最後に、サーバー上の XML ファイルとしてグラフを保存することができます。 グラフをキャッシュするか、またはファイルにグラフを保存経由でこのメソッドを使用する利点は、する場合は、グラフを表示する前に、XML を変更することできます。 アプリケーションには、サーバー上のイメージ ファイルを格納するフォルダーに対する読み取り/書き込みアクセス許可が必要です。

1. Web サイトのルート ディレクトリにある、という名前の新しいファイルを作成する*ChartSaveXml.cshtml*です。
2. 次のように、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    このコードは、先ほど見たをキャッシュにグラフを格納する XML ファイルを使用する点を除いて、コードに似ています。 呼び出して、XML ファイルが存在するかどうかを確認するコードはまず、`File.Exists`メソッドです。 新しいコードを作成、ファイルが存在すると場合、`Chart`オブジェクトし、ファイルの名前として渡します、`themePath`パラメーター。 これは、XML ファイルには任意に基づくグラフを作成します。 コードが通常どおりのグラフを作成しを呼び出して、XML ファイルが既に存在しない場合、`SaveXml`に保存します。 使用して、グラフの描画、`Write`メソッドとしてする前に説明しました。

    同様にキャッシュを表示 ページで、このコードには、グラフのタイトルにタイムスタンプが含まれます。
3. という名前の新しいページを作成する*ChartDisplayXMLChart.cshtml*し、次のマークアップを追加します。 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. 実行、 *ChartDisplayXMLChart.cshtml*ページ。 グラフが表示されます。 グラフのタイトルにタイムスタンプを書き留めます。
5. ブラウザーを閉じます。
6. WebMatrix でを右クリックし、  *\_ChartFiles*フォルダー、をクリックして**更新**、フォルダーを開きます。 *XMLChart.xml*によってこのフォルダーにファイルが作成された、`Chart`ヘルパー。 

    ![説明: _ChartFiles フォルダー グラフ ヘルパーで作成された XMLChart.xml ファイルを表示します。](7-displaying-data-in-a-chart/_static/image14.jpg)
7. 実行、 *ChartDisplayXMLChart.cshtml*ページをもう一度です。 グラフは、最初に、ページの実行時と同じタイムスタンプを示します。 以前に保存した XML からグラフが生成されるためです。
8. WebMatrix で開きます、  *\_ChartFiles*フォルダーと削除、 *XMLChart.xml*ファイル。
9. 実行、 *ChartDisplayXMLChart.cshtml*もう一度ページします。 現時点では、タイムスタンプが更新されるため、`Chart`ヘルパーは、XML ファイルを再作成する必要があります。 場合は、チェック、  *\_ChartFiles*フォルダーと、XML ファイルがバックアップに注意してください。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Web でのデータベース操作の概要ページ サイト](https://go.microsoft.com/fwlink/?LinkId=202893)
- [ASP.NET Web でのキャッシュを使用してパフォーマンスを向上させるためにサイトをページします。](https://go.microsoft.com/fwlink/?LinkId=202903)
- [グラフ クラス](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99))(MSDN で ASP.NET Web Pages API リファレンス)
