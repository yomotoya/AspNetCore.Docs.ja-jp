---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: ASP.NET Web Pages (Razor) を持つグラフ データの表示 |Microsoft Docs
author: microsoft
description: この章では、グラフのデータを表示する方法について説明します。 前の章では、手動で、グリッドでデータを表示する方法について説明しました。 この章で説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 41af8ac4dba0ba5ad478df7075628186a96f48c4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393757"
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>ASP.NET Web Pages (Razor) を持つグラフ データを表示します。
====================
によって[Microsoft](https://github.com/microsoft)

> この記事は、グラフを使用して、使用して ASP.NET Web Pages (Razor) の web サイトにデータを表示する方法を説明します、`Chart`ヘルパー。
> 
> **学習内容**:
> 
> - グラフのデータを表示する方法。
> - 組み込みのテーマを使用して、グラフのスタイルを設定する方法。
> - グラフを保存する方法とパフォーマンス向上のためにキャッシュする方法。
> 
> この記事で導入された機能のプログラミング、ASP.NET を次に示します。
> 
> - `Chart`ヘルパー。
> 
> > [!NOTE]
> > この記事の情報は、ASP.NET Web Pages 1.0 と Web ページ 2 に適用されます。


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>グラフ ヘルパー

使用することができますグラフィカルな形式でデータを表示する場合は、`Chart`ヘルパー。 `Chart`ヘルパーのさまざまな種類のグラフでデータを表示するイメージをレンダリングできます。 書式設定とラベル付けに関して多くのオプションをサポートしています。 `Chart` 30 を超える種類のグラフ、場合によっては Microsoft Excel やその他のツールを使い慣れているグラフのすべての種類などをレンダリングできるヘルパー&#8212;面グラフ、横棒グラフ、縦棒グラフ、折れ線グラフ、およびと共に複数の円グラフ株価チャートをなどの特殊なグラフ。

| **面グラフ**![説明: 面グラフの種類の画像](7-displaying-data-in-a-chart/_static/image1.jpg) | **横棒グラフ**![説明: 横棒グラフの画像](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **縦棒グラフ**![説明: 縦棒グラフの種類の画像](7-displaying-data-in-a-chart/_static/image3.jpg) | **折れ線グラフ**![説明: 行のグラフの種類の画像](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **円グラフ**![説明: 円グラフの種類の画像](7-displaying-data-in-a-chart/_static/image5.jpg) | **株価チャート**![説明: 株価チャートの画像](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>グラフ要素

グラフでは、データと凡例、軸、系列、およびなどのような追加の要素を表示します。 次の図は多くのグラフ要素を使用する場合にカスタマイズできる、`Chart`ヘルパー。 この記事では、いくつか設定する方法を示します (すべて)、これらの要素。

![説明: 画像、グラフ要素](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>データからグラフを作成します。

グラフに表示するデータには、データベースから返された結果から、配列から、または XML ファイル内にあるデータからを指定できます。

### <a name="using-an-array"></a>配列を使用します。

説明したよう[ASP.NET Web ページは、Razor 構文を使用プログラミングの概要について](https://go.microsoft.com/fwlink/?LinkId=202890)配列では、1 つの変数に類似した項目のコレクションを格納することができます。 グラフに含めるデータを格納するのに配列を使用できます。

作成する方法、グラフ、配列内のデータからグラフの種類を使用してこの手順を示しています。 ページ内のグラフを表示する方法も示します。

1. という名前の新しいファイルを作成する*ChartArrayBasic.cshtml*します。
2. 次のように、既存のコンテンツを置き換えます。 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    コードは、まず新しいグラフを作成し、幅と高さを設定します。 使用して、グラフのタイトルを指定する、`AddTitle`メソッド。 使用するデータを追加する、`AddSeries`メソッド。 この例では使用して、 `name`、 `xValue`、および`yValues`のパラメーター、`AddSeries`メソッド。 `name`パラメーターは、グラフの凡例に表示されます。 `xValue`パラメーターには、グラフの横軸に沿って表示されるデータの配列が含まれています。 `yValues`パラメーターには、グラフの垂直ポイントをプロットするために使用されるデータの配列が含まれています。

    `Write`メソッドが実際には、グラフを表示します。 この場合、グラフの種類を指定していないため、`Chart`ヘルパーがその既定のグラフは、縦棒グラフを表示します。
3. ブラウザーでページを実行します。 ブラウザーでは、グラフが表示されます。 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>データベース クエリを使用してグラフ データ

グラフに情報がデータベースにある場合は、データベース クエリを実行し、結果からデータを使用して、グラフを作成します。 この手順は、読み取り、情報の記事で作成されたデータベースからデータを表示する方法を示します[ASP.NET Web Pages サイトでのデータベース操作の概要](https://go.microsoft.com/fwlink/?LinkId=202893)します。

1. 追加、*アプリ\_データ*フォルダーが存在しない場合に、web サイトのルートにフォルダー。
2. *アプリ\_データ*フォルダー、という名前のデータベース ファイルを追加*SmallBakery.sdf*に記載されている[ASP.NET Web Pages サイトでのデータベース操作の概要](https://go.microsoft.com/fwlink/?LinkId=202893).
3. という名前の新しいファイルを作成する*ChartDataQuery.cshtml*します。
4. 次のように、既存のコンテンツを置き換えます。   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    コードは最初 SmallBakery データベースを開くし、という名前の変数に割り当てられます`db`します。 この変数を表す、`Database`から読み取りをデータベースに書き込むために使用できるオブジェクト。 次に、コードは、各製品の価格と名前を取得する SQL クエリを実行します。 コードは、新しいグラフを作成し、グラフを呼び出すことによって、データベース クエリを渡します`DataBindTable`メソッド。 このメソッドは、2 つのパラメーター:`dataSource`パラメーターは、クエリからのデータの`xField`パラメーターを使用して、グラフの x 軸を使用するデータ列を設定できます。

    使用する代わりに、`DataBindTable`メソッドが使用できる、`AddSeries`のメソッド、`Chart`ヘルパー。 `AddSeries`メソッドを使用して設定できます、`xValue`と`yValues`パラメーター。 使用する代わりに、たとえば、`DataBindTable`このようなメソッド。

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    使用することができます、`AddSeries`このようなメソッド。

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    同じ結果、両方とも表示されます。 `AddSeries`メソッドはより柔軟なグラフの種類とデータをより明示的に指定することができますが、`DataBindTable`メソッドは簡単に余分な柔軟性を必要としない場合に使用します。
5. ブラウザーでページを実行します。 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>XML データを使用します。

グラフ化するための 3 番目のオプションでは、グラフのデータと XML ファイルを使用します。 XML ファイルにも、スキーマ ファイルがあることが必要です (*.xsd*ファイル)、XML 構造を記述します。 この手順では、XML ファイルからデータを読み取る方法を示します。

1. *アプリ\_データ*フォルダー、という名前の新しい XML ファイルを作成する*data.xml*します。
2. 既存の XML を架空の会社の従業員に関する XML データの一部であると、次に置き換えます。 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. *アプリ\_データ*フォルダー、という名前の新しい XML ファイルを作成する*data.xsd*します。 (この時間は、拡張子 *.xsd*)。
4. 次のように既存の XML に置き換えます。 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. という名前の新しいファイルを作成、web サイトのルートに*ChartDataXML.cshtml*します。
6. 次のように、既存のコンテンツを置き換えます。 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    このコードを作成、`DataSet`オブジェクト。 このオブジェクトは、データは XML ファイルから読み取られ、スキーマ ファイルの情報に従って整理を管理に使用されます。 (コードの先頭に、ステートメントが含まれることに注意してください`using SystemData`します。 これは正常に動作するために必要です、`DataSet`オブジェクト。 詳細については、次を参照してください[ &quot;Using&quot;ステートメントおよび完全修飾名](#SB_UsingStatements)この記事で後述します。)。

    コードを次に、作成、`DataView`データセットに基づくオブジェクト。 データ ビューは、グラフにバインドできるオブジェクトを提供します。&#8212;読み込みをプロットします。 グラフにデータを使用して、バインド、`AddSeries`メソッドを前述の配列データのことを除いて、この時間をグラフ化、`xValue`と`yValues`パラメーターに設定されて、`DataView`オブジェクト。

    この例は、特定のグラフの種類を指定する方法も示します。 データが追加されたとき、`AddSeries`メソッド、`chartType`円グラフの表示もパラメーターを設定します。
7. ブラウザーでページを実行します。 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Using"ステートメントと完全修飾名
> 
> Razor 構文を使用して ASP.NET Web Pages が基づく .NET Framework は、何千ものコンポーネント (クラス) で構成されます。 これらすべてのクラスを使用する管理しやすいように、構成している*名前空間*ライブラリのようなものであります。 たとえば、`System.Web`名前空間には、ブラウザーとサーバーの通信をサポートするクラスが含まれています、`System.Xml`名前空間には作成し、XML ファイルの読み取りに使用されるクラスが含まれています、`System.Data`名前空間には、操作できるクラスが含まれています。データです。
> 
> .NET Framework 内の特定のクラスにアクセスするためにコードは、クラス名だけでなくもに、クラスがある名前空間を把握する必要があります。 たとえば、使用するには、`Chart`ヘルパー、コードを検索する必要があります、`System.Web.Helpers.Chart`を組み合わせたもので、名前空間、クラス (`System.Web.Helpers`) とクラス名 (`Chart`)。 これは、クラスのと呼ばれます*完全修飾*名前&#8212;、あいまいでない完全広大内の場所、.NET Framework です。 コードでは、これは次のようになります。
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> ただしは面倒な (およびエラーが生じやすい) クラスまたはヘルパーを参照するたびに、これらの長い、完全修飾名を使用する必要があります。 そのため、簡単にクラス名を使用することができます*インポート*通常は、関心がある名前空間は .NET Framework の多数の名前空間の中から、いくつかだけです。 名前空間をインポートした場合は、クラス名だけを使用することができます (`Chart`) 完全修飾名ではなく (`System.Web.Helpers.Chart`)。 コードを実行し、クラス名を検出すると、そのクラスの検索にインポートした名前空間だけで参照できます。
> 
> Razor 構文を使用する ASP.NET Web ページを使用して web ページを作成するときに通常使用するクラスの同じセット、毎回など、`WebPage`クラスや、さまざまなヘルパー。 Web サイトを作成するたびに、関連する名前空間をインポートする作業を保存、ASP.NET は、一連のすべての web サイトのコア名前空間を自動的にインポートするように構成されます。 その理由は名前空間や今のところまでインポートを扱う機会のなかった作業したことのすべてのクラスでは、名前空間を既にインポートされています。
> 
> ただし、場合によっては自動的にインポートする名前空間に含まれていないクラスを使用する必要があります。 その場合は、そのクラスの完全修飾名を使用することができますか、またはクラスを含む名前空間を手動でインポートすることができます。 使用する名前空間をインポートする、`using`ステートメント (`import` Visual basic)、前述の例のよう、情報の記事。
> 
> たとえば、`DataSet`クラスは、`System.Data`名前空間。 `System.Data`名前空間が自動的に、ASP.NET Razor ページをご利用いただけません。 そのため、使用する、`DataSet`クラスの完全修飾名を使用して、このようなコードを使用することができます。
> 
> `var dataSet = new System.Data.DataSet();`
> 
> 使用した場合、`DataSet`繰り返しクラスこのような名前空間をインポートし、コードで、クラス名だけを使用できます。
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> 追加することができます`using`参照するその他の .NET Framework 名前空間のステートメント。 ただし、前述のように、必要はありません多くの場合、これを使用するクラスのほとんどは名前空間で使用するために、ASP.NET によって自動的にインポートされるため、 *.cshtml*と *.vbhtml*ページ。


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Web ページ内のグラフを表示します。

例で説明しましたここまでは、グラフを作成して、グラフをグラフィックとしてブラウザーに直接レンダリングされます。 多くの場合をする、ブラウザーでそれ自体ではなく、ページの一部としてグラフを表示します。 そのためには 2 段階のプロセスが必要です。 最初の手順では、既に説明したように、グラフを生成するページを作成します。

2 番目の手順では、別のページで、結果のイメージを表示します。 HTML を使用するイメージを表示する`<img>`要素と同じで、イメージを表示する方法。 参照するのではなく、ただし、 *.jpg*または *.png*ファイル、`<img>`要素の参照、 *.cshtml*を含むファイル、`Chart`ヘルパーをグラフを作成します。 表示ページを実行すると、`<img>`要素の出力の取得、`Chart`ヘルパー グラフを表示します。

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. という名前のファイルを作成する*ShowChart.cshtml*します。
2. 次のように、既存のコンテンツを置き換えます。 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    コードを使用して、 `<img>` 」で作成したグラフを表示する要素、 *ChartArrayBasic.cshtml*ファイル。
3. ブラウザーで web ページを実行します。 *ShowChart.cshtml*ファイルに含まれているコードに基づいてグラフ イメージの表示、 *ChartArrayBasic.cshtml*ファイル。

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>グラフのスタイル設定

`Chart`ヘルパーは、多数のグラフの外観をカスタマイズするためのオプションをサポートしています。 色、フォント、罫線、およびなどを設定することができます。 グラフの外観をカスタマイズする簡単な方法が使用するには、*テーマ*します。 テーマは、フォント、色、ラベル、パレット、罫線、および特殊効果を使用してグラフを表示する方法を指定する情報のコレクションです。 (グラフのスタイルがグラフの種類を示すしないことに注意してください)。

次の表では、組み込みのテーマを示します。

| テーマ | 説明 |
| --- | --- |
| `Vanilla` | 白い背景に赤の列が表示されます。 |
| `Blue` | 青色のグラデーションの背景を青の列が表示されます。 |
| `Green` | 青色の緑の背景のグラデーションの列が表示されます。 |
| `Yellow` | 黄色の背景のグラデーションにオレンジ色の列が表示されます。 |
| `Vanilla3D` | 白い背景に赤の 3-D の列が表示されます。 |

新しいグラフを作成するときに使用するテーマを指定することができます。

1. という名前の新しいファイルを作成する*ChartStyleGreen.cshtml*します。
2. 次のように、ページで、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    このコードは、データベースを使用して、データが追加されている前の例と同じ、`theme`パラメーターを作成するとき、`Chart`オブジェクト。 変更後のコードを次に示します。

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. ブラウザーでページを実行します。 前に、と同じデータを参照してくださいがグラフがより洗練されたものは次になります。 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>グラフを保存します。

使用すると、`Chart`ヘルパーをこれまで、この記事では、ヘルパーが呼び出されるたびに再最初からグラフに作成します。 必要に応じて、グラフのコードはまた、データベースを再クエリか、再度読み込み、データを取得する XML ファイル。 場合によっては、これを行うようにできます複雑な操作では、クエリの対象としているデータベースが大きい場合、または XML ファイルには、大量のデータが含まれている場合。 場合でも、グラフは、大量のデータが関与しない、イメージを動的に作成のプロセス サーバーのリソースを受け取り、web サイトのパフォーマンスに影響が存在する可能性多くの人がページまたはグラフを表示するページを要求する場合。

グラフを作成することの潜在的なパフォーマンスに与える影響を軽減するために、一度に 1 つ目のグラフを作成できますし、保存してその必要があります。 グラフがそれを再生成するのではなく、必要なときにだけ、保存したバージョンをフェッチしをレンダリングします。

次のようにグラフを保存できます。

- (サーバー) 上のコンピューターのメモリ グラフをキャッシュします。
- グラフを画像ファイルとして保存します。
- グラフを XML ファイルとして保存します。 このオプションを使用して、保存する前に、グラフを変更できます。

### <a name="caching-a-chart"></a>グラフをキャッシュします。

グラフを作成したら、それをキャッシュできます。 グラフをキャッシュすると、もう一度表示する必要がある場合に、再作成するがないことを意味します。 グラフをキャッシュに保存するときに付けますキーをそのグラフで一意である必要があります。

サーバーのメモリが不足している場合、キャッシュに保存されたグラフを削除する可能性があります。 さらに、何らかの理由で、アプリケーションが再起動した場合、キャッシュがクリアされます。 したがって、キャッシュされたグラフを使用する標準的な方法は、常に最初に確認して、キャッシュでは、利用可能なかどうか、そうでない場合、作成または再作成には。

1. という名前のファイルを作成、web サイトのルート ディレクトリにある*ShowCachedChart.cshtml*します。
2. 次のように、既存のコンテンツを置き換えます。 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>`タグが含まれています、`src`属性を指す、 *ChartSaveToCache.cshtml*ファイルを開き、クエリ文字列としてページに、キーを渡します。 キー値に含まれる&quot;myChartKey&quot;します。 *ChartSaveToCache.cshtml*ファイルが含まれています、`Chart`グラフを作成するヘルパー。 このページは、すぐに作成します。

    ページの最後には、という名前のページへのリンクが*ClearCache.cshtml*します。 間もなくも作成するページです。 必要があります、 *ClearCache.cshtml*テストの例ではこのキャッシュにのみ、リンクまたはキャッシュされたグラフを使用する場合に通常は、ページではありません。
3. という名前の新しいファイルを作成、web サイトのルート ディレクトリにある*ChartSaveToCache.cshtml*します。
4. 次のように、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    コードはまず、クエリ文字列のキー値として渡されたものかどうかを確認します。 呼び出すことによって、キャッシュからグラフを読み取るため、コードがしようとした場合、`GetFromCache`メソッドと、キーを渡します。 判明した場合は、キャッシュ (これは、グラフが要求されている最初の時間が発生する)、そのキーの下で何もない、コードは、グラフを通常どおり作成します。 グラフが完了したら、コードに保存、キャッシュを呼び出して`SaveToCache`します。 そのメソッドでは、キー (そのため、グラフは、後から要求することができます)、およびグラフをキャッシュに保存する必要がありますを時間数が必要です。 (グラフをキャッシュすると、正確な時間は、表すデータが変わる可能性があります考えるどのくらいの頻度に依存します)。`SaveToCache`メソッドも必要になります、`slidingExpiration`パラメーター&#8212;設定されている場合、タイムアウトを true にカウンターがグラフがアクセスされるたびにリセットします。 この場合、グラフのキャッシュ エントリが 2 分後に、グラフのユーザーがアクセスを有効期限が切れる有効で意味します。 (スライディング有効期限を使用しない場合は、絶対有効期限、キャッシュ エントリを正確に 2 分後どのくらいの頻度にアクセスされていたに関係なく、キャッシュに設定されて期限切れにことを意味です)。

    最後に、コードを使用して、`WriteFromCache`メソッドを取得してキャッシュからグラフを表示します。 このメソッドが範囲外ですが、`if`グラフは、まずがあったまたは生成され、キャッシュに保存する必要があるかどうかは、キャッシュからグラフが取得されるため、キャッシュをチェックするブロック。

    例では、注意、`AddTitle`メソッドには、タイムスタンプが含まれています。 (現在の日付と時刻を追加します&#8212; `DateTime.Now` &#8212;タイトルにします)。
5. という名前の新しいページを作成する*ClearCache.cshtml*次の内容を置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    このページを使用して、`WebCache`にキャッシュされているグラフを削除するためのヘルパー *ChartSaveToCache.cshtml*します。 前述のように、通常このようなページがありません。 ここでそれを作成するキャッシュをテストしやすくだけです。
6. 実行、 *ShowCachedChart.cshtml*ブラウザーで web ページ。 ページに含まれているコードに基づいてグラフ イメージが表示されます、 *ChartSaveToCache.cshtml*ファイル。 タイムスタンプの動向、グラフのタイトルでメモしてをおきます。 

    ![グラフのタイトルのタイムスタンプを持つ基本的なグラフの説明: 画像](7-displaying-data-in-a-chart/_static/image13.jpg)
7. ブラウザーを閉じます。
8. 実行、 *ShowCachedChart.cshtml*もう一度です。 タイムスタンプが同じであると同様に、グラフが再生成されませんでしたが、キャッシュから読み取られた代わりにことを示すことを確認します。
9. *ShowCachedChart.cshtml*、クリックして、**キャッシュをクリア**リンク。 これにより*ClearCache.cshtml*キャッシュがクリアされたことを報告します。
10. をクリックして、 **ShowCachedChart.cshtml に戻る**リンク、または再実行*ShowCachedChart.cshtml* WebMatrix から。 キャッシュがクリアされたため、この時点のタイムスタンプが変更されたことに注意してください。 そのため、コードをグラフを再生成し、キャッシュに元に戻す必要があります。

### <a name="saving-a-chart-as-an-image-file"></a>グラフをイメージ ファイルとして保存

グラフを画像ファイルとして保存することもできます (など、 *.jpg*ファイル)、サーバー上。 イメージ ファイルは、同様に、任意のイメージから使用できます。 利点は、ファイルが格納されているではなく、一時的なキャッシュに保存します。 (たとえば、1 時間ごと) のさまざまなタイミングで新しいグラフ イメージを保存でき、時間の経過と共に発生する変更の永続的な記録を維持します。 Web アプリケーションがイメージ ファイルを配置するサーバー上のフォルダーにファイルを保存する権限を持っていることを確認しておく必要がありますに注意してください。

1. という名前のフォルダーを作成、web サイトのルート ディレクトリにある *\_ChartFiles*が既に存在しない場合。
2. という名前の新しいファイルを作成、web サイトのルート ディレクトリにある*ChartSave.cshtml*します。
3. 次のように、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    コードは、まず表示するかどうか、 *.jpg*呼び出すことによってファイルが存在する、`File.Exists`メソッド。 新しいコードを作成、ファイルが存在しない場合`Chart`配列から。 今回は、コードの呼び出し、`Save`メソッドを呼び出し、`path`ファイルのパスと、グラフの保存先のファイル名を指定するパラメーター。 ページの本文で、`<img>`要素を指すパスを使用して、 *.jpg*ファイルを表示します。
4. 実行、 *ChartSave.cshtml*ファイル。
5. WebMatrix に戻ります。 イメージ ファイルの名前に注意してください*chart01.jpg*に保存されている、  *\_ChartFiles*フォルダー。

### <a name="saving-a-chart-as-an-xml-file"></a>グラフを XML ファイルとして保存

最後に、サーバー上の XML ファイルとしてグラフを保存することができます。 グラフをキャッシュまたはグラフをファイルに保存します。 このメソッドを使用する利点は、する場合は、グラフを表示する前に、XML を変更することできます。 アプリケーションには、サーバー上のイメージ ファイルを配置するフォルダーに対する読み取り/書き込みアクセス許可が必要です。

1. という名前の新しいファイルを作成、web サイトのルート ディレクトリにある*ChartSaveXml.cshtml*します。
2. 次のように、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    このコードは、XML ファイルを使用して、キャッシュ内のグラフを格納するためする前述のコードに似ています。 コードは最初に、呼び出すことによって XML ファイルが存在するかどうかを確認します。、`File.Exists`メソッド。 新しいコードを作成、ファイルが存在すると場合、`Chart`オブジェクトし、ファイルの名前として渡します、`themePath`パラメーター。 これは、XML ファイル内のあらゆるものに基づくグラフを作成します。 XML ファイルが既に存在しない場合、コードは通常のようなグラフを作成しを呼び出して`SaveXml`に保存します。 使用して、グラフを表示、`Write`メソッドとしてする前に説明しました。

    このコードにはキャッシュが映っているページと同様は、グラフのタイトルでタイムスタンプが含まれています。
3. という名前の新しいページを作成する*ChartDisplayXMLChart.cshtml*し、次のマークアップを追加します。 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. 実行、 *ChartDisplayXMLChart.cshtml*ページ。 グラフが表示されます。 グラフのタイトルにタイムスタンプをメモします。
5. ブラウザーを閉じます。
6. WebMatrix で、右クリックし、  *\_ChartFiles*フォルダー、をクリックして**更新**、フォルダーを開きます。 *XMLChart.xml*によってこのフォルダー内のファイルが作成された、`Chart`ヘルパー。 

    ![説明: _ChartFiles フォルダー グラフ ヘルパーで作成された XMLChart.xml ファイルを表示します。](7-displaying-data-in-a-chart/_static/image14.jpg)
7. 実行、 *ChartDisplayXMLChart.cshtml*ページをもう一度です。 グラフは、最初に、ページの実行時と同じタイムスタンプを示します。 以前に保存した XML からグラフが生成されるためです。
8. WebMatrix で、開く、  *\_ChartFiles*フォルダーと削除、 *XMLChart.xml*ファイル。
9. 実行、 *ChartDisplayXMLChart.cshtml*もう一度ページします。 今回は、タイムスタンプが更新されるため、`Chart`ヘルパーは、XML ファイルを再作成する必要があります。 場合は、チェック、  *\_ChartFiles*フォルダーとは、XML ファイルに注意してください。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース

- [ページのサイトを ASP.NET Web でのデータベース操作の概要](https://go.microsoft.com/fwlink/?LinkId=202893)
- [ページのパフォーマンスを向上させるためにサイトを ASP.NET web キャッシュの使用](https://go.microsoft.com/fwlink/?LinkId=202903)
- [グラフ クラス](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99))(MSDN で ASP.NET Web Pages API リファレンス)
