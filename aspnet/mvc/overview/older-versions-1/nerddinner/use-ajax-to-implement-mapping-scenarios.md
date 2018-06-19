---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: AJAX を使用して、マッピングのシナリオを実装する |Microsoft ドキュメント
author: microsoft
description: 手順 11 NerdDinner アプリケーションでは、作成、編集または表示ディナー l を表示するユーザーの有効化に AJAX マッピングのサポートを統合する方法を示しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872716"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>AJAX を使用して、マッピングのシナリオを実装するには
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 11 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 11 では、AJAX マッピングのサポートを NerdDinner アプリケーションでは、作成、編集または表示ディナー dinner の場所をグラフに表示するユーザーの有効化に統合する方法を示します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner 手順 11: AJAX マップを統合します。

今すぐしましょうアプリケーション少しより視覚的に優れた AJAX マッピングのサポートを統合することによりします。 これは、作成、編集または表示ディナー dinner の場所をグラフに表示するユーザーを有効になります。

### <a name="creating-a-map-partial-view"></a>マップの部分ビューを作成します。

アプリケーション内のいくつかの場所のマッピング機能を使用しようとしています。 保持しているコード ドライおには、複数のコント ローラーのアクションおよびビューの間で再使用できる 1 つの部分的なテンプレートに共通のマップ機能をカプセル化します。 この部分的なビュー"map.ascx"という名前を \Views\Dinners ディレクトリ内で作成します。

生み出すことができます、map.ascx 部分 \Views\Dinners ディレクトリを右クリックして、追加の を選択して&gt;メニュー コマンドを表示します。 名前を指定、ビュー"Map.ascx"、チェックを部分ビューとして厳密に型指定された"Dinner"モデル クラスを渡すことを行うことを示します。

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

[追加] ボタンをクリックしたとき、テンプレートの部分的なが作成されます。 次の内容を持つ Map.ascx ファイルが更新されます。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

最初の&lt;スクリプト&gt;Microsoft Virtual Earth 6.2 マッピング ライブラリへのポインターを参照します。 2 番目&lt;スクリプト&gt;map.js のファイルをすぐを作成しますが、一般的な Javascript マッピング ロジックをカプセル化するポイントを参照します。 &lt;Div id =「マップ」&gt;要素は HTML コンテナー Virtual Earth マップをホストするために使用します。

埋め込みがある&lt;スクリプト&gt;をこのビューに固有の 2 つの JavaScript 関数を含むブロックします。 最初の関数は、ネットワーク上のページはクライアント側スクリプトを実行するときに実行される関数に jQuery を使用します。 LoadMap() ヘルパー関数を呼び出して、virtual earth マップ コントロールを読み込む Map.js、スクリプト ファイル内で定義されます。 2 番目の関数は、pin、マップに追加する場所を識別するコールバック イベント ハンドラーです。

サーバー側を使用する方法に注意してください。 &lt;% = %&gt; 、の緯度と経度、JavaScript にマップする Dinner を埋め込むには、クライアント側のスクリプト ブロック内でブロックされます。 これを使用せず、独立した AJAX 呼び出し、サーバーに – 値を取得する高速になります)、クライアント側のスクリプトで使用できる動的な値を出力する便利な手法です。 &lt;% = %&gt; – サーバー上のビューをレンダリングするときに、ブロックが実行し、はので、HTML の出力はだけの埋め込みの JavaScript 値 (例: var 緯度 = 47.64312;)。

### <a name="creating-a-mapjs-utility-library"></a>Map.js ユーティリティ ライブラリを作成します。

これで、マップの JavaScript 機能をカプセル化 (および LoadMap および LoadPin メソッドに関して、上記の実装) を使用できる Map.js ファイルを作成してみましょう。 これには、プロジェクト内で \Scripts ディレクトリを右クリックして、しを選択し、"追加-&gt;新しい項目の"メニュー コマンド、JScript の項目を選択し、"Map.js"という名前を付けます。

追加の JavaScript コードを次に示しますが、マップを表示し、ディナーのピンの場所を追加する Virtual Earth と対話する Map.js ファイルに。

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>マップを作成および編集フォームと統合します。

既存作成および編集するシナリオのマップのサポートを統合します。 良いニュースは、これは非常に簡単のタスクで、コント ローラー、コードのいずれかを変更することを必要としません。 作成および編集ビューでは、dinner フォームの UI を実装する一般的な"DinnerForm"部分ビューを共有するため、1 つの場所でマップを追加し、両方を作成および編集シナリオ使用をおできます。

\Views\Dinners\DinnerForm.ascx 部分ビューを開き、更新、新しいマップ部分を追加するために必要なタスクです。 以下は、更新された DinnerForm がどのようにマップを追加する (メモ: 簡略化のための次のコード スニペットから HTML フォーム要素を省略しています)。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

DinnerForm 上部分は、(ため、Dinner オブジェクトと国の dropdownlist の設定に SelectList の両方が必要です)、そのモデルの種類として"DinnerFormViewModel"の種類のオブジェクトを取得します。 部分、マップでは、そのモデルの種類として"Dinner"の型のオブジェクトだけが必要があり、ので、マップを部分的なレンダリングおときに渡している DinnerFormViewModel の Dinner サブ プロパティだけに。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

JavaScript 関数が、"Address"HTML テキスト ボックスに"blur"イベントをアタッチする部分は jQuery に追加されました。 思いますが、ユーザーがクリックしたときに発生する「フォーカス」イベントまたはタブのテキスト ボックスにします。 テキスト ボックスを終了するときに発生する"blur"イベントはその反対です。 上記のイベント ハンドラーは、これが行われ、このマップに新しいアドレス場所をプロットときにテキスト ボックスの緯度と経度の値をクリアします。 Map.js ファイル内で定義したコールバック イベント ハンドラーには、しを用意しましたアドレスに基づいて仮想地球によって返される値を使用して、フォーム上の経度と緯度のテキスト ボックスが更新されます。

これで、およびときお、アプリケーションを再実行、標準的な夕食フォーム要素と共に表示される既定のマップを見てみましょう"ホスト Dinner" タブをクリックします。

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

アドレスを入力し、離れた タブ、場所を表示するマップを動的に更新し、イベント ハンドラーが場所の値を持つ緯度/経度テキスト ボックスに設定されます。

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

新しい dinner を保存すると、編集のためもう一度開き、ページが読み込まれるときに、マップの場所が表示されることを検索します。

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

[アドレス] フィールドが変更されるたびに、マップと緯度/経度座標が更新されます。

マップには、Dinner 場所が表示されたら、これでは、テキスト ボックスを表示する代わりに非表示の要素 (からのマップが自動的に更新して、アドレスが入力されるたびに) から緯度と経度のフォーム フィールドを変更おこともできます。 To do これから Html.Hidden() ヘルパー メソッドを使用する Html.TextBox() HTML ヘルパーの使用に切り替えます。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

これで、およびフォームは少しわかりやすい (まだに保存することで、データベース内の各 Dinner) 中に生の緯度/経度を表示しないようにします。

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>詳細ビューとマップを統合します。

詳細に紹介したシナリオと統合しても一緒に作成および編集するシナリオ、みましょうマップができました。 呼び出すタスクが必要なは&lt;Html.RenderPartial("map"); %library;&gt;詳細ビュー内にあります。

ソース コード (マップの統合) の完全な詳細ビューの外観を次に示します。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

今すぐ/Dinners 詳細/[id] URL にユーザーが移動したとき、ありますに関する詳細を表示 dinner マップ上の場所、dinner (押しピンの完了時にマウスでポイントが表示されます dinner のタイトルとそのアドレス)、あり RSVP fo への AJAX リンクr に。

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>データベースとリポジトリの場所の検索を実装します。

AJAX 実装を終えるに近くディナーを視覚的に検索することができます、アプリケーションのホーム ページにマップを追加してみましょう。

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

まず、効率的に検索を実行する、場所ベースの radius ディナーのデータベースとデータのリポジトリ レイヤー内のサポートを実装します。 使用して、新しい[SQL 2008 の地図機能](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx)、これを実装するか、または Gary Dryden がこちらの記事で説明されている SQL 関数手法を使用してお: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx)と Rob Coneryここで LINQ to SQL での使用に関するブログの投稿しました。 [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

この手法を実装するのにおは Explorer を開き"サーバー"Visual Studio 内で、NerdDinner データベースを選択し、その下にある「機能」サブ ノードを右クリックし、新しい「スカラー値関数」を作成します。

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

次のピクセル関数でおを貼り付けます。

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

新しいテーブル値関数 SQL Server"NearestDinners"を呼び出すことを作成します。

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

この"NearestDinners"テーブル関数では、ピクセルのヘルパー関数を使用して、緯度の 100 マイル以内のすべてのディナーを返すし、指定お経度。

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

この関数を呼び出すを最初を開くこと、LINQ to SQL デザイナーを \Models ディレクトリ内で NerdDinner.dbml ファイルをダブルクリックしています。

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

おありますし、ドラッグ NearestDinners およびピクセル関数は、LINQ に SQL デザイナーは、SQL NerdDinnerDataContext クラスに、LINQ のメソッドとして追加されるようになります。

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

返す今後 NearestDinner 関数を使用した、DinnerRepository クラス"FindByLocation"クエリ メソッドを公開できますし、指定した位置の 100 マイル以内であるディナー。

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>AJAX の JSON に基づく検索のアクション メソッドの実装

コント ローラー アクション メソッドにマップを設定するために使用する Dinner データの一覧を返す新しい FindByLocation() リポジトリ メソッドを利用することをここで実装します。 このアクション メソッドが返す Dinner データを JSON (JavaScript Object Notation) 形式で簡単に操作できる、クライアントで JavaScript を使用するようにお必要があります。

これを実装する、\Controllers ディレクトリを右クリックして、追加の を選択して、新しい"SearchController"クラスを作成しますお&gt;コント ローラーのメニュー コマンド。 以下のような新しい SearchController クラス内の"SearchByLocation"アクション メソッドを実装します。

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

SearchController の SearchByLocation アクション メソッドは、内部的に近くにあるディナーの一覧を取得する DinnerRespository で FindByLocation メソッドを呼び出します。 クライアントに直接返す Dinner オブジェクトではなく、代わりにオブジェクトを返します JsonDinner です。 JsonDinner クラス Dinner プロパティのサブセットを公開する (例: セキュリティ上の理由を dinner の RSVP'd がユーザーの名前を開示しません)。 また、Dinner – 上に存在するが、特定の夕食に関連付けられている RSVP オブジェクトの数をカウントすることによって動的に計算される RSVPCount プロパティも含まれます。

コント ローラーの基本クラスで Json() ヘルパー メソッドを使用して JSON ベースのワイヤ形式を使用してディナーのシーケンスを返すおはします。 JSON は、単純なデータ構造を表すための標準のテキスト形式です。 以下のアクション メソッドから返されたときのように 2 つの JsonDinner オブジェクトの JSON 形式の一覧の外観の例に示します。

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>JQuery を使用して JSON ベースの AJAX メソッドを呼び出す

今すぐ SearchController の SearchByLocation アクション メソッドを使用して NerdDinner アプリケーションのホーム ページを更新する準備が整いました。 To do、/Views/Home/Index.aspx ビュー テンプレートを開き、テキスト ボックス、[検索] ボタン、当社のマップでは、必要であることを更新しますおと&lt;div&gt; dinnerList をという名前の要素。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

ページに、2 つの JavaScript 関数を追加できます。

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

1 つ目の JavaScript 関数は、ページが初めて読み込まれるときに、マップを読み込みます。 JavaScript を 2 番目の JavaScript 関数ワイヤは、イベント ハンドラーの検索 ボタンをクリックします。 ボタンが押されたときに Map.js ファイルを追加して、FindDinnersGivenLocation() JavaScript 関数を呼び出します。

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

この FindDinnersGivenLocation() 関数は、マップを呼び出します。入力した場所の中央に仮想地球コントロールを Find() です。 Virtual earth マップのサービスを返す場合、マップします。Find() メソッドは、最後の引数として渡すお callbackUpdateMapDinners コールバック メソッドを呼び出します。

CallbackUpdateMapDinners() メソッドでは、実際の作業が行われます。 JQuery の $.post() ヘルパー メソッドを使用して、緯度と経度、新しく中央揃えのマップを渡します –、SearchController の SearchByLocation() アクション メソッドへの AJAX 呼び出しを実行します。 $.Post() ヘルパー メソッドが完了すると、呼び出されるインライン関数を定義し、「ディナー」という名前の変数を使用してアクション メソッドが渡されます SearchByLocation() から dinner の JSON 形式の結果が返されました。 Foreach は各返された dinner し dinner の緯度と経度、およびその他のプロパティを使用して、マップに新しい pin を追加します。 また、マップの右側にディナーの HTML の一覧に dinner エントリを追加します。 ワイヤ アップ、プッシュピンと HTML の一覧の両方のポイント イベント上に置いたときに、夕食に関する詳細が表示されるよう。

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

今すぐアプリケーションを実行およびおマップが表示されます、ホーム ページを参照してください。 市区町村の名前に入ると、近く今後ディナーがマップに表示されます。

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

夕食ポインターを置いたと、その詳細が表示されます。

Dinner タイトルをクリックすると、バブルまたは HTML の一覧の右側にあるいずれかをごに移動、夕食は – おできますし、必要に応じて RSVP の。

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>次の手順

今すぐ NerdDinner アプリケーションのすべてのアプリケーションの機能を実装しました。 単位の自動今すぐみましょうを達成する方法を見てのテストします。

> [!div class="step-by-step"]
> [前へ](use-ajax-to-deliver-dynamic-updates.md)
> [次へ](enable-automated-unit-testing.md)
