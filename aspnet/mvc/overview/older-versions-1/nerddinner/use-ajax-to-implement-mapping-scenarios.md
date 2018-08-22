---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: AJAX を使用して、マッピングのシナリオを実装する |Microsoft Docs
author: microsoft
description: 手順 11 では、作成、編集または表示 dinners l を表示するユーザーを有効にすると、NerdDinner アプリケーションにマッピングの AJAX のサポートを統合する方法を示す.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f7de23ca46e6dc00fe8075e28068a8b3f95d02cd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827342"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>AJAX を使用して、マッピングのシナリオを実装するには
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 11 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 11 では、NerdDinner アプリケーションでは、作成、編集または表示 dinners dinner の場所をグラフィカルに表示するユーザーを有効にするのに AJAX マッピング サポートを統合する方法を示します。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner 手順 11: AJAX のマップを統合します。

今すぐしましょうアプリケーションでは少しより視覚的に刺激マッピングの AJAX のサポートを統合することで。 これは、作成、編集または表示 dinners dinner の場所をグラフィカルに表示するユーザーを有効になります。

### <a name="creating-a-map-partial-view"></a>マップの部分ビューを作成します。

アプリケーション内で複数の場所でマッピングの機能を使用するでしょう。 このコードは DRY を保持するには、複数のコント ローラー アクションとビューの間で再使用できる 1 つの部分的なテンプレート内で共通のマップ機能カプセル化します。 この部分ビュー"map.ascx"という名前がされ \Views\Dinners ディレクトリ内の作成します。

生み出すことができます、map.ascx 部分 \Views\Dinners ディレクトリを右クリックし、追加の選択によって&gt;メニュー コマンドを表示します。 します"Map.ascx"ビューを指定して、として、部分ビューをして、ここでは、厳密に型指定された"Dinner"モデル クラスを渡すことを示します。

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

[追加] ボタンをクリックしたとき、テンプレートの部分的なが作成されます。 Map.ascx ファイルに次の内容に更新し、います。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

最初の&lt;スクリプト&gt;ポイントは、Microsoft Virtual Earth 6.2 マッピング ライブラリを参照します。 2 番目の&lt;スクリプト&gt;map.js のファイルを作成後、一般的な Javascript のマッピングのロジックをカプセル化するポイントを参照します。 &lt;Div id =「マップ」&gt;要素は、マップをホストする Virtual Earth を使用する HTML コンテナーです。

埋め込みがある&lt;スクリプト&gt;このビューに固有の 2 つの JavaScript 関数を含むブロックします。 最初の関数は、ページがクライアント側スクリプトを実行する準備を実行する関数に接続する jQuery を使用します。 LoadMap() ヘルパー関数を呼び出して、virtual earth のマップ コントロールを読み込む Map.js、スクリプト ファイル内で定義します。 2 番目の関数は、場所を識別するマップをピン留めが追加するコールバック イベント ハンドラーです。

サーバー側は使用する方法に注意してください。 &lt;% = %&gt;ように、JavaScript にマップする Dinner の経度と緯度を埋め込むには、クライアント側のスクリプト ブロック内でブロックします。 これは、(個別 AJAX 呼び出しをサーバーに – 値を取得する処理が速くなりますなし) クライアント側スクリプトで使用できる動的な値を出力する便利な方法です。 &lt;% = %&gt; – サーバーで、ビューのレンダリングは、ブロックが実行され、そのため、HTML の出力だけ結局は埋め込みの JavaScript 値 (例: var の緯度 = 47.64312;)。

### <a name="creating-a-mapjs-utility-library"></a>Map.js ユーティリティ ライブラリを作成します。

Map.js ファイルを使用して、マップの JavaScript の機能をカプセル化 (および上記 LoadMap と LoadPin メソッドを実装) を今すぐ作成しましょう。 これには、このプロジェクト内の \Scripts ディレクトリを右クリックして選択し、"追加-&gt;新しい項目の"メニュー コマンド、JScript の項目を選択し、"Map.js"という名前を付けます。

以下を追加する JavaScript コードは、Virtual Earth、マップを表示し、dinners のピンの場所を追加するとやり取りする Map.js ファイル。

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>マップを作成および編集フォームと統合します。

既存の作成と編集シナリオのマップのサポートを統合します。 これは非常に簡単のタスクで、コント ローラー コードの変更を必要としないです。 Create と Edit ビューでは、dinner フォームの UI を実装する一般的な"DinnerForm"の部分ビューを共有するため 1 つの場所にマップを追加して、両方当社のそれを使用して任意の Create と Edit シナリオがある場合します。

\Views\Dinners\DinnerForm.ascx 部分ビューを開き、更新、部分的な新しいマップを追加するために必要な to do が。 更新された DinnerForm がどのようにマップが追加されるを以下に示します (注: HTML フォーム要素は、簡潔にするための以下のコード スニペットから省略されます)。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

DinnerForm 上部分では、(Dinner オブジェクトと国の dropdownlist を設定する SelectList の両方が必要です) ためにそのモデルの種類として"DinnerFormViewModel"の型のオブジェクトを受け取ります。 部分的なマップは、そのモデルの種類として"Dinner"の型のオブジェクトだけが必要し、ため、マップを部分的なレンダリングするときに渡される DinnerFormViewModel の Dinner サブ プロパティだけに。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

JavaScript 関数を"Address"HTML テキスト ボックスに「ぼかし」イベントをアタッチする部分は jQuery に追加されました。 思いますが、ユーザーがクリックしたときに発生する「フォーカス」イベントまたはタブのテキスト ボックスにします。 逆の場合、ユーザーがテキスト ボックスを終了するときに発生する「ぼかし」イベント。 上記のイベント ハンドラーは、これが行われ、マップ上の新しいアドレス場所をプロットし、ときにテキスト ボックスの緯度と経度の値をクリアします。 Map.js ファイル内で定義したコールバック イベント ハンドラーを書き出しましょうアドレスに基づいて、virtual earth によって返される値を使用して、フォーム上の経度と緯度のテキスト ボックスを更新します。

ここでときに、アプリケーションを再実行し、標準の Dinner フォーム要素と共に表示される既定のマップを見てみましょう"ホスト Dinner" タブをクリックします。

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

、アドレスを入力してから tab、、場所を表示するマップを動的に更新し、場所の値で、緯度/経度のテキスト ボックスに、イベント ハンドラーが設定されます。

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

新しい夕食を保存し、編集するために再度開く場合、ページが読み込まれるときに、マップの地域が表示されることを検索します。

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

[アドレス] フィールドが変更されるたびに、マップと緯度/経度の座標が更新されます。

これで、マップには、Dinner 場所が表示されたらに代わりに非表示要素 (マップが自動的に更新して、アドレスが入力されるたび) から表示するテキスト ボックスの中から緯度と経度のフォーム フィールドを変更したこともできます。 To do これから Html.Hidden() のヘルパー メソッドを使用する Html.TextBox() HTML ヘルパーの使用に切り替えます。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

および、フォームは少しわかりやすいようになりましたし (まだ各 Dinner データベース内では、それらを格納) 中に生の緯度/経度を表示しないようにします。

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>詳細ビューと、マップの統合

このシナリオでは詳細情報とも統合してみましょうサンプルを作成および編集のシナリオでの統合マップができました。 呼び出すには to do 必要がありますすべて&lt;Html.RenderPartial("map"); %&gt;詳細ビュー内で。

完全な詳細ビュー (マップ統合) にソース コードがどのようにを次に示します。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

今すぐ/Dinners/詳細/[id] URL に移動したときをに関する詳細を表示 dinner、dinner マップ上の場所 (ピンの完了時にマウスでポイントが表示されます、dinner のタイトルとそのアドレス)、RSVP fo に AJAX のリンクとr に。

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>データベースとリポジトリの場所の検索を実装します。

オフ、AJAX の実装を完了するには、ユーザーが近くに dinners を視覚的に検索できるようにするアプリケーションのホーム ページにマップを追加してみましょう。

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

まず、Dinners を検索する場所ベースの radius を効率的に実行する、データベースとデータのリポジトリ層内のサポートを実装します。 使用して、新しい[SQL 2008 の地理空間機能](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx)、これを実装するか、または Gary Dryden がこちらの記事で説明されている SQL 関数のアプローチを使用しています: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx)と Rob Coneryここでの LINQ to SQL の使用に関するブログ投稿しました。 [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

この手法を実装するためには Explorer を開き"サーバー"Visual Studio 内で、NerdDinner データベースを選択しその下にある「関数」のサブ ノードを右クリックし、新しい「スカラー値関数」を作成します。

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

次の関数をピクセルにし、貼り付けます。

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

SQL Server の"NearestDinners"を呼び出すことで、新しいテーブル値関数から作成します。

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

この"NearestDinners"テーブル関数では、ピクセルのヘルパー関数を使用して、緯度の 100 マイル以内のすべて Dinners を返すし、指定経度。

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

この関数を呼び出すには、私たちを最初に開きます LINQ to SQL デザイナー、\Models ディレクトリ内で NerdDinner.dbml ファイルをダブルクリックしています。

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

To SQL デザイナーは、SQL NerdDinnerDataContext クラスに、LINQ のメソッドとして追加することにより、LINQ に NearestDinners およびピクセルの関数をドラッグしたしますし。

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

返す今後 NearestDinner 関数を使用するときは必ず DinnerRepository クラスに"FindByLocation"クエリ メソッドを公開できますし、指定した場所の 100 マイル内にある Dinners:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>AJAX の JSON ベースの検索アクション メソッドを実装します。

マップを設定するために使用する Dinner データの一覧を返す新しい FindByLocation() リポジトリ メソッドを活用したコント ローラー アクション メソッドを実装します。 返す Dinner データ、JSON (JavaScript Object Notation) 形式でクライアントで JavaScript を使用して簡単に操作できることができるように、このアクション メソッドがあります。

を実装する、\Controllers ディレクトリを右クリックし、[追加]-選択して、新しい"SearchController"クラスを作成します、&gt;コント ローラーのメニュー コマンド。 以下のような新しい SearchController クラス内で"SearchByLocation"アクション メソッドを実装します。

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

SearchController の SearchByLocation のアクション メソッドは、近くにある dinners の一覧を取得する DinnerRespository で FindByLocation メソッドを内部的に呼び出します。 クライアントに直接 Dinner オブジェクトを返すのではなく、代わりに返します JsonDinner オブジェクト。 JsonDinner クラスが Dinner プロパティのサブセットを公開します (例: セキュリティ上の理由を夕食に対する RSVP が人の名前を公開しません)。 Dinner: 存在しないし、特定の夕食に関連付けられている RSVP オブジェクトの数をカウントすることによって動的に計算されます、RSVPCount プロパティも含まれています。

コント ローラーの基本クラスで Json() のヘルパー メソッドを使用して、返す一連の dinners JSON ベースのワイヤ形式を使用していますが。 JSON は、単純なデータ構造を表すための標準のテキスト形式です。 次の 2 つの JsonDinner オブジェクトの JSON 形式の一覧がどのように、アクション メソッドから返されるときに例を示します。

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>JQuery を使用して、JSON ベースの AJAX メソッドを呼び出す

今すぐ SearchController の SearchByLocation アクション メソッドを使用して NerdDinner アプリケーションのホーム ページを更新する準備が整いました。 To do、/Views/Home/Index.aspx ビュー テンプレートを開くを更新して、テキスト ボックス、[検索] ボタン、当社のマップおよび&lt;div&gt; dinnerList という名前の要素。

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

2 つの JavaScript 関数のページに追加できます。

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

最初の JavaScript 関数は、ページが初めて読み込まれるときに、マップを読み込みます。 JavaScript を 2 つ目の JavaScript 関数ワイヤは、イベント ハンドラーを検索 ボタンをクリックします。 ボタンが押されたときに、Map.js ファイルに追加する FindDinnersGivenLocation() JavaScript 関数を呼び出します。

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

この FindDinnersGivenLocation() 関数では、マップを呼び出します。入力した場所を中心に配置する Virtual Earth コントロールで Find() します。 Virtual earth のマップ サービスが返す場合は、マップします。Find() メソッドでは、最後の引数として渡して callbackUpdateMapDinners コールバック メソッドを呼び出します。

CallbackUpdateMapDinners() メソッドでは、実際の作業を実行する場所です。 JQuery の $.post() ヘルパー メソッドを使用して、新しく中央揃えのマップの経度と緯度を引数として –、SearchController の SearchByLocation() アクション メソッドへの AJAX 呼び出しを実行します。 $.Post() のヘルパー メソッドが完了したら、呼び出されるインライン関数を定義し、"dinners"という名前の変数を使用してアクション メソッドが渡されます SearchByLocation() から JSON 形式の dinner 結果が返されます。 返された各夕食経由での foreach は、dinner の緯度と経度、およびその他のプロパティを使用して、マップに新しい pin を追加します。 Dinner エントリは、マップの右に dinners の HTML の一覧にも追加します。 ワイヤ アップ、プッシュピンと HTML の一覧の両方のポイント イベントをユーザーが上に置いたときに、夕食についての詳細が表示されるよう。

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

ここでときに、アプリケーションを実行し、マップが表示されますホーム ページを参照してください。 市区町村の名前を入力すると、近くに今後の dinners がマップに表示されます。

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

ポインターを合わせると、dinner と、詳細な情報が表示されます。

Dinner タイトルをクリックすると、バブルまたは HTML の一覧の右側にあるいずれかに移動します私たち、dinner は – ここできますし、必要に応じて RSVP の。

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>次の手順

NerdDinner アプリケーションのすべてのアプリケーションの機能を実装しましたようになりました。 みましょう今すぐを達成する方法を見て自動化された単体テストのこと。

> [!div class="step-by-step"]
> [前へ](use-ajax-to-deliver-dynamic-updates.md)
> [次へ](enable-automated-unit-testing.md)
