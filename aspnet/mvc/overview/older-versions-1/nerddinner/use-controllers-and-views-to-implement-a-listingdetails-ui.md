---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: コント ローラーとビューを使用して、一覧と詳細の UI を実装する |Microsoft ドキュメント
author: microsoft
description: 手順 4 では、ユーザー データの一覧/詳細ナビゲーション エクスペリエンスを提供する、モデルの活用したアプリケーションに、コント ローラーを追加する方法を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: ac3568941eeef24bd9857c5787471aadea15fc7f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>コント ローラーとビューを使用して、一覧と詳細の UI を実装するには
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 4 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 4 では、ユーザーについては、NerdDinner サイト ディナー用のデータ一覧/詳細ナビゲーション操作に提供する、モデルの活用したアプリケーションに、コント ローラーを追加する方法を示します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner 手順 4: コント ローラーとビュー

従来の web フレームワーク (classic ASP、PHP、ASP.NET Web フォームなど) では、受信 Url は通常、ディスク上のファイルにマップします。 例: などの URL に対する要求"/繰り返し"または"/Products.php"、「繰り返し」または"Products.php"ファイルによって処理される可能性があります。

MVC フレームワークの web ベースでは、少し異なる方法で、Url がサーバー コードをマップします。 受信 Url をファイルにマップするには、代わりには、代わりに、クラスのメソッドに Url をマップします。 これらのクラスは「コント ローラー」と呼ばれますとユーザー入力の処理、入力方向の HTTP 要求の処理を担当している、クライアント バックアップを取得して、データを保存および送信する応答を決定する (HTML を表示、ファイルをダウンロードする、別のサーバーに対するリダイレクトURL など)。

これで、NerdDinner アプリケーションの基本的なモデルを作成した、次の手順はディナーのデータの一覧/詳細ナビゲーション操作については、サイトをユーザーに提供するの利用するアプリケーションに、コント ローラーを追加します。

### <a name="adding-a-dinnerscontroller-controller"></a>DinnersController コント ローラーの追加

当社の web プロジェクト内の「コント ローラー」フォルダーを右クリックして作業を開始し、[おします、**追加 -&gt;コント ローラー** (もコマンドを実行できるこの Ctrl M, ctrl + C を入力して)] メニューのコマンド。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

「コント ローラーの追加」ダイアログが表示されます。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

新しいコント ローラー"DinnersController"という名前を「追加」ボタンをクリックします。 Visual Studio によっては、追加してから、\Controllers ディレクトリの下の DinnersController.cs ファイルは。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

コード エディター内で新しい DinnersController クラスが開放されるもします。

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>DinnersController クラスに Index() と Details() アクション メソッドを追加します。

アプリケーションを使用して今後ディナーの一覧を参照し、それに関する特定の詳細を表示する一覧で任意の Dinner をクリックすることの訪問者を有効にします。 アプリケーションから、次の Url を発行することによってこの作業を行うします。

| **URL** | **目的** |
| --- | --- |
| */Dinners/* | 今後ディナーの HTML の一覧を表示します。 |
| */Dinners/Details/[id]* | これが一致するデータベースに dinner の DinnerID – URL に埋め込まれた"id"パラメーターで指定された特定 dinner に関する詳細を表示します。 例:/Dinners/Details/2 DinnerID 値が 2 Dinner に関する詳細情報を HTML ページが表示されます。 |

次のように、DinnersController クラスに次の 2 つのパブリック「操作方法」を追加することによってこれらの Url の最初の実装が公開します。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

NerdDinner アプリケーションを実行し、それらを呼び出すため、ブラウザーを使用しておをし、します。 入力、 *「ディナー/」* URL と、当社*Index()*メソッドを実行し、返信、次の応答。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

入力、 *「/ディナー/詳細/2」* URL になります、 *Details()*メソッドを実行し、次の応答を返信します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

思うかもしれません - どの ASP.NET MVC ご存知、DinnersController クラスを作成し、これらのメソッドを呼び出しますか。 理解してみましょうルーティング機能の仕組みを簡単に説明します。

### <a name="understanding-aspnet-mvc-routing"></a>Understanding ASP.NET MVC ルーティング

ASP.NET MVC には、多数の Url をコント ローラー クラスにマップする方法の制御の柔軟性を提供する強力な URL ルーティング エンジンが含まれています。 ASP.NET MVC を呼び出すだけでなく、変数に自動的に URL/クエリ文字列から解析してパラメーターとしてメソッドに渡されるさまざまな方法を構成する方法を作成するには、どのコント ローラー クラスを選択する方法を完全にカスタマイズすることができます。引数。 完全に SEO (検索エンジンの最適化) のサイトを最適化するだけでなくアプリケーションから必要なすべての URL 構造を公開する柔軟性を提供します。

既定では、新しい ASP.NET MVC プロジェクトにあらかじめ構成された一連の URL ルーティングの規則は既に登録されています。 これにより、簡単に作業を開始するアプリケーションで明示的に何も構成することがなくことができます。 既定のルーティング ルール登録は、プロジェクトのルートに"Global.asax"ファイルをダブルクリックして開くことができます、プロジェクトの"Application"のクラス内で確認できます。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

既定の ASP.NET MVC ルーティング ルールは、このクラスの"RegisterRoutes"メソッド内で登録されます。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"ルート。MapRoute()"メソッドの呼び出しが上記の登録 URL の形式を使用して、コント ローラー クラスを受信した Url にマップする既定のルーティング規則:"/{controller}/{controller}/{id}""controller"には、インスタンス化するコント ローラー クラスの名前:"action"はの名前、パブリック メソッドを呼び出すと、"id"では、メソッドに引数として渡すことが URL に埋め込まれた省略可能なパラメーターです。 "MapRoute()"メソッドの呼び出しに渡される 3 番目のパラメーターは、一連の既定値は URL に存在しないことに、コント ローラーとアクション/id 値に使用する (コント ローラー アクションで =「ホーム」、"Index"、Id = ="") です。

以下は Url のさまざまな方法を示すテーブルが既定値を使用してマップされている"<em>/{コント ローラー}/{controller}/{id}"</em>ルート ルール。

| **URL** | **コント ローラー クラス** | **アクション メソッド** | **渡されたパラメーター** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details(id) | id=2 |
| */Dinners/Edit/5* | DinnersController | Edit(id) | id=5 |
| */Dinners/Create* | DinnersController | Create() | N/A |
| */Dinners* | DinnersController | Index() | N/A |
| */Home* | HomeController | Index() | N/A |
| */* | HomeController | Index() | N/A |

最後の 3 つの行が既定値を表示 (コント ローラー アクションで = ホーム、インデックス、Id = ="") 使用されています。 1 つ指定されていない場合に、既定のアクション名として"Index"メソッドが登録されているため、"/ディナー"、"/home"Url が Index() アクション メソッドのコント ローラー クラスで呼び出されます。 いずれかが指定されていない場合に既定のコント ローラーとして「ホーム」コント ローラーが登録されているため、「/」URL を作成するテンプレートを使用して、Index() アクション メソッドを呼び出すことで。

これら既定の URL ルーティング ルールがたくない場合良いニュースは、簡単に変更 - だけ上の RegisterRoutes メソッド内で編集されます。 NerdDinner アプリケーションでは、は、既定の URL ルーティング規則のいずれかを変更することにしましょう – 代わりにだけそのまま使用します-はします。

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>当社 DinnersController から DinnerRepository を使用します。

みましょう DinnersController の現在の実装に置き換える Index() と Details() のアクション メソッドをモデルを使用する実装。

動作を実装する前に作成した DinnerRepository クラスを使用します。 おによりを"NerdDinner.Models"名前空間を参照している"using"ステートメントを追加することによって開始し、フィールドとして、DinnerController クラス、DinnerRepository のインスタンスを宣言します。

この章で後でうまく「依存関係の挿入」の概念を紹介し、表示を向上単位を有効にする DinnerRepository への参照を取得する、コント ローラーを別の方法: テストですが右のこれで、当社 DinnerRepository のインスタンスを作成しましょう次のようにインラインです。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

取得したデータ モデル オブジェクトを使用して HTML 応答を生成する準備が整いました。

### <a name="using-views-with-our-controller"></a>このコント ローラーとビューの使用

HTML をアセンブルし、使用して、アクション メソッド内でコードを記述することができますが、 *Response.Write()*いるアプローチ手に負えなくかなり簡単に、クライアントに送信するヘルパー メソッドです。 のみアプリケーションと、DinnersController アクション メソッドの内部データ ロジックを実行し、個別"view"テンプレートは HTML 形式の出力を担当する HTML 応答を表示するために必要なデータを渡すは、はるかに優れた方法ことです。 しばらくおわかりのよう"view"テンプレートは、通常の HTML マークアップと埋め込みレンダリング コードの組み合わせを含むテキスト ファイルです。

ビュー レンダリングから、コント ローラー ロジックを分離すると、いくつかの大きな利点が表示されます。 具体的には、明確"に分離する懸案事項"間で、アプリケーション コードと UI コードを書式設定または表示に役立ちます。 これはより簡単単体テスト アプリケーション ロジックを分離する UI のレンダリング ロジックからです。 やすく後で UI 表示テンプレートを変更するアプリケーション コードの変更を加える必要はありません。 できるように開発者とデザイナー プロジェクトで一緒に共同作業を容易にします。

代わりに、戻り値の型がある"ActionResult"の「無効」の戻り値の型を持つから、2 つのアクション メソッドのメソッド シグネチャを変更すると、HTML UI の応答を返信するビュー テンプレートを使用することを示すために、DinnersController クラスを更新することができます。 呼び出してことができます、 *View()* "ViewResult"オブジェクトなどの下に返すコント ローラーの基本クラスのヘルパー メソッド。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

署名、 *View()*上を使用するヘルパー メソッドが次のようになります。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

最初のパラメーター、 *View()*ヘルパー メソッドが HTML 応答を表示するために使用するビューのテンプレート ファイルの名前。 2 番目のパラメーターは、テンプレートの表示が HTML 応答を表示するために必要なデータを含むモデル オブジェクトです。

呼び出す、Index() アクション メソッド内で、 *View()*ヘルパー メソッドのディナー"Index"ビュー テンプレートを使用して、HTML の一覧を表示することを指定しています。 ビュー テンプレートから一覧を生成する Dinner オブジェクトのシーケンスを渡しています。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

当社 Details() アクション メソッド内で、URL 内で指定された id を使用して、夕食オブジェクトを取得ましょう。 呼び出して有効な夕食が見つかった場合、 *View()*ヘルパー メソッドを呼び出した Dinner オブジェクトを表示するために、[詳細] ビューのテンプレートを使用することを示すです。 "NotFound"ビューのテンプレートを使用して、夕食が存在しないことを示す便利なエラー メッセージをレンダリングは無効な夕食が要求されている場合 (とオーバー ロードされたバージョンの*View()*のみテンプレート名を取得するヘルパー メソッド):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

"NotFound"、「詳細」、および"Index"ビューのテンプレートを実装してみましょう。

### <a name="implementing-the-notfound-view-template"></a>"NotFound"のビュー テンプレートの実装

まず、– 要求された dinner が見つからないことを示すフレンドリ エラー メッセージが表示されます、"NotFound"ビュー テンプレートを実装します。

コント ローラー アクション メソッド内で、テキストのカーソルを配置し、新しいビュー テンプレートを作成しを右クリックし、(してもコマンドを実行できるこの Ctrl-M, ctrl + V」と入力して)「ビューの追加」メニュー コマンドを選択します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

これは、次のような「ビューの追加」ダイアログが表示されます。 既定では、ダイアログ ボックスが作成するビューの名前を事前に設定アクション メソッドの名前と一致するカーソル時のダイアログが (この場合は「詳細」) で起動されました。 、まず、"NotFound"テンプレートを実装する必要があるため、このビューの名前をオーバーライドし、"NotFound"を代わりになるように設定おします。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

「追加」ボタンをクリックすると Visual Studio は新しい"NotFound.aspx"ビュー テンプレートを (これは、ディレクトリが既に存在しない場合にも作成)"\Views\Dinners"ディレクトリ内の作成します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

コード エディター内で、新しい"NotFound.aspx"ビュー テンプレートも開きます。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

既定ではテンプレートの表示がある 2 つ「コンテンツ領域」コンテンツおよびコードを追加しました。 1 つ目では、「タイトル」HTML ページの返信をカスタマイズすることができます。 2 つ目では、コンテンツをカスタマイズする"メイン"HTML ページの返信されることができます。

"NotFound"ビューのテンプレートを実装するには、いくつかの基本的なコンテンツを追加おします。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

お試すことができますし、ブラウザー内で。 これを行うみましょう要求、 *「/9999/ディナー/詳細」* URL。 これは、現在、データベースに存在しないし、"NotFound"ビューのテンプレートを表示するために、DinnersController.Details() アクション メソッドが発生する dinner を参照してください。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

1 つに気付くこと上スクリーン ショットでは、基本的なビュー テンプレートには、多数の HTML 画面上のメイン コンテンツを囲むが継承されてます。 これは、ビュー テンプレートが、サイト上のすべてのビューで一貫したレイアウトを適用することにより、「マスター ページ」テンプレートを使用しているためです。 マスター ページの詳細については、このチュートリアルの後半のしくみについて説明します。

### <a name="implementing-the-details-view-template"></a>「詳細」ビュー テンプレートの実装

– 単一 Dinner モデルの HTML を生成する"Details"ビュー テンプレートを実装してみましょう。

おを Details アクション メソッド内で、テキストのカーソルを配置しを右クリックし「ビューの追加」メニュー コマンドを選択 (または Ctrl-M, ctrl + V キーを押します)。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

「ビューの追加」ダイアログが表示されます。 既定のビュー名 (「詳細」) ではこのままです。 おありますも「厳密に型指定されたビューの作成」のチェック ボックス ダイアログ ボックスを選択 (コンボ ボックスのドロップダウン リストを使用) に渡しているコント ローラーからビューにモデルの種類の名前します。 このビューの Dinner オブジェクトを渡している (この型の完全修飾名:"NerdDinner.Models.Dinner")。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

「空のビュー」を作成する選びましたここでは、以前のテンプレートとは異なりこの時刻に自動的に選択します「スキャフォールディング」「詳細」テンプレートを使用して表示します。 このダイアログ ボックスで「コンテンツの表示」のドロップダウン リストを変更することで、これを表示おことができます。

「スキャフォールディング」を渡している Dinner オブジェクトに基づいて、詳細ビュー テンプレートの最初の実装が生成されます。 これは、ビュー テンプレートの実装に簡単に開始することは簡単な方法を提供します。

「追加」ボタンをクリックすると Visual Studio は新しい"Details.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内でご利用の米国作成します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

コード エディター内で、新しい"Details.aspx"ビュー テンプレートが開放されるもします。 Dinner モデルに基づいた詳細ビューの初期 scaffold 実装が含まれます。 スキャフォールディング エンジンは、.NET リフレクションを使用して、渡されたクラスで公開されているパブリック プロパティを確認して、見つかった各型に基づく適切なコンテンツを追加します。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

*「/1/ディナー/詳細」*の URL をブラウザー内でこの「詳細」scaffold 実装がどのようにを参照してください。 この URL を使用して手動で追加したデータベースに最初に作成した際ディナーの 1 つ表示されます。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

これは us 立ち上がっており実行中をすばやく取得、および、Details.aspx ビューの最初の実装を提供します。 お移動し、満足度に UI をカスタマイズすることを調整します。

Details.aspx テンプレートより密接を見るときにわかること、静的な HTML を含むだけでなくレンダリング コードが埋め込まれています。 &lt;%%&gt;ビュー テンプレートをレンダリングするときに、コード ナゲットがコードを実行し、 &lt;% = %&gt;コード ナゲットがそれぞれに含まれるコードを実行し、テンプレートの出力ストリームに結果をレンダリングします。

厳密に型指定された「モデル」プロパティを使用して、コント ローラーから渡された"Dinner"モデル オブジェクトにアクセスする、ビュー内でコードを記述できます。 Visual Studio により、完全コード intellisense を備えた、エディター内で「モデル」このプロパティにアクセスするとき。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

みましょうように扱いました最終的な詳細ビュー テンプレートのソースは次のようになります。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

アクセスして、 *「/1/ディナー/詳細」* URL 再度はこれで、以下のようなレンダリングします。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>"Index"ビュー テンプレートの実装

– 今後ディナーの一覧を生成する"Index"ビュー テンプレートを実装してみましょう。 To do おインデックス アクション メソッド内で、テキストのカーソルの位置し、し、右をこの をクリックし、コマンドを選択「ビューの追加」メニュー (または Ctrl-M, ctrl + V キーを押します)。

「ビューの追加」ダイアログ ボックスでを"Index"という名前のビュー テンプレートを保持し、「厳密に型指定されたビューの作成」チェック ボックスをオンします。 今度は自動的に"List"ビュー テンプレートを生成し、"NerdDinner.Models.Dinner"モデルの種類として、ビューに渡されるを選択してを選択します (これ scaffold が発生することが想定するビューの追加 ダイアログ「リスト」を作成することによって示されたおためシーケンスを渡して Dinner オブジェクトのコント ローラーからビューに)。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

「追加」ボタンをクリックすると Visual Studio は新しい"Index.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内でご利用の米国作成します。 これは「スキャフォールディング」、初期の実装内のビューに渡すおディナー、HTML テーブルの一覧を提供します。

アプリケーションとアクセスを実行するとき、 *「ディナー/」* URL ディナーの一覧を表示する次のようにします。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

上記のテーブル ソリューションでは、これは非常に必要な夕食一覧が直面している、コンシューマーでは、Dinner データのグリッドのようなレイアウトによりします。 Index.aspx ビュー テンプレートを更新して、データの少ない列を一覧表示して使用するように変更を&lt;ul&gt;レンダリングして、次のコードを使用してテーブルではなく要素。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

各 dinner 経由でこのモデルでループ処理、上記の foreach ステートメント内で"var"キーワードを使用します。 "Var"を使用することを意味 dinner オブジェクトは、遅延バインディングを c# 3.0 に慣れていないものと考える可能性があります。 代わりにすること、コンパイラが厳密に型指定の「モデル」プロパティに対して型推論を使用している (種類がある"IEnumerable&lt;Dinner&gt;") およびおがいっぱいになることを意味 – Dinner の種類としてローカル"dinner"変数のコンパイルintellisense およびコンパイル時のコード ブロック内でこれを確認します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

更新をヒットしました、 */Dinners*以下、更新されたビュー今すぐ検索のようなブラウザーで URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

これは優れた – はまだ完全があります。 最後に をクリックして、リスト内の個々 のディナーに関する詳細情報を参照してください。 エンドユーザーを有効にします。 当社 DinnersController に Details アクション メソッドにリンクする HTML ハイパーリンク要素を表示して、これを実装します。

2 つの方法のいずれかで、インデックス ビュー内でこれらのハイパーリンクを生成できます。 最初に HTML を手動で作成、 &lt;、&gt;などの要素の下、私たちの埋め込み&lt;%%&gt;内でブロック、 &lt;、&gt; HTML 要素。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

使用して、別の方法は、HTML をプログラムで作成をサポートする ASP.NET MVC では、組み込みの"Html.ActionLink()"ヘルパー メソッドを利用する&lt;、&gt;で別のアクション メソッドにリンク要素をコント ローラー:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Html.ActionLink() ヘルパー メソッドの最初のパラメーターを表示するリンク テキストは、(ここでは、タイトル、夕食の)、2 番目のパラメーター (この場合、詳細メソッド) へのリンクを生成するコント ローラーのアクション名は、3 番目のパラメーター(プロパティの名前/値を持つ匿名型として実装)、アクションに送信するパラメーターのセット。 ここでは dinner おにリンクして、ASP.NET MVC の既定の URL ルーティング ルールのための"id"パラメーターを指定することは"{controller}/{controller}/{id}"Html.ActionLink() ヘルパー メソッドは、次の出力を生成します。

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

当社 Index.aspx ビューは、Html.ActionLink() ヘルパー メソッドの手法を使用し、リスト、適切な詳細情報の URL にリンクに各 dinner があります。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

これで ヒット時と、 */Dinners* dinner 一覧は、次のように下の URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

一覧でディナーのいずれかをクリックしたときは、その詳細を参照してくださいに移動したします。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>名前付け規則に基づくおよび \Views ディレクトリ構造

既定では ASP.NET MVC アプリケーションでは、テンプレートの表示を解決するときに構造体の名前付け規則に基づくディレクトリを使用します。 これにより、開発者は完全修飾の場所のパスから、コント ローラー クラス内のビューを参照するときにする必要があります。 既定では ASP.NET MVC はファイルを検索、表示テンプレート内で、* \Views\[ControllerName]\*アプリケーションの下にあるディレクトリ。

たとえば、きております – 3 つのテンプレートの表示を明示的に参照する DinnersController クラスに:"Index"、「詳細」および"NotFound"です。 ASP.NET MVC は既定で探します内でこれらのビュー、 *\Views\Dinners*アプリケーションのルート ディレクトリ下にあるディレクトリ。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

上方法がありますが現在、プロジェクト内の 3 つのコント ローラー クラスに注意してください (DinnersController、HomeController および AccountController – 最後の 2 つが既定で追加されたプロジェクトを作成したとき)、3 つのサブ ディレクトリ (1 つずつ、コント ローラー) \Views ディレクトリ内。

ホーム ネットワークおよびアカウント コント ローラーから参照されるビューは、それぞれからそのビュー テンプレートを自動的に解決*\Views\Home*と*\Views\Account*ディレクトリ。 *\Views\Shared*サブ directory は、アプリケーション内で複数のコント ローラー全体で再利用できるテンプレートの表示を格納する手段を提供します。 ASP.NET MVC は、ビュー テンプレートの解決を試みると、これが最初内で確認する、 *\Views\[コント ローラー]*特定のディレクトリ ビュー テンプレートを見つけられない場合がありますなります内で、 *\Views\共有*ディレクトリ。

個々 のビューのテンプレートを命名する際に推奨されるガイダンス ビュー テンプレートを表示するためにその原因となったアクション メソッドと同じ名前の共有を開始します。 たとえば、「インデックス」上のアクション メソッドがビューを使用して、"Index"ビューの結果を表示するためにし、"Details"アクション メソッドは、"Details"ビューを使用してその結果を表示するためには。 これにより、簡単にすばやく参照するテンプレートが各アクションに関連付けられています。

開発者は、ビュー テンプレートがコント ローラーで呼び出されるアクション メソッドとして同じ名前を持つ場合、ビュー テンプレート名を明示的に指定する必要はありません。 おできます代わりにだけ、モデル オブジェクトに渡します"View()"ヘルパー メソッド (ビューの名前を指定)、なしと ASP.NET MVC を使用することが自動的に推論、 *\Views\[ControllerName]\[ActionName]*レンダリングへのディスク上のビュー テンプレート。

これにより、コント ローラーのコードを少し、クリーンアップをコードで 2 回名前と重複を回避するため。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

上記のコードは、サイトのエクスペリエンスを nice Dinner 一覧と詳細を実装するために必要なすべてです。

#### <a name="next-step"></a>次の手順

構築されたエクスペリエンスをブラウズ nice Dinner があるようになりました。

みましょう CRUD (Create、Read、Update、Delete) データ形式のサポートを編集できるようになりました。

> [!div class="step-by-step"]
> [前へ](build-a-model-with-business-rule-validations.md)
> [次へ](provide-crud-create-read-update-delete-data-form-entry-support.md)
