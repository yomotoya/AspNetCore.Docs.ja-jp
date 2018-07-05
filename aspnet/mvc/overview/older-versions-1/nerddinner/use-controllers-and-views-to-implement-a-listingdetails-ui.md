---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: コント ローラーとビューを使用して、リスティング/詳細 UI を実装する |Microsoft Docs
author: microsoft
description: 手順 4 では、コント ローラーのユーザー データのリスティング/詳細ナビゲーション エクスペリエンスを提供するには、このモデルを利用するアプリケーションを追加する方法を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 4fe065e29950a076de07d73205a97399f82f07d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377877"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>コント ローラーとビューを使用して、リスティング/詳細 UI を実装するには
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 4 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 4 では、コント ローラーのユーザー データ リスティング/詳細ナビゲーション エクスペリエンスを dinners NerdDinner サイトに提供するには、このモデルを利用するアプリケーションを追加する方法を示します。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner 手順 4: コント ローラーとビュー

従来の web フレームワーク (classic ASP、PHP、ASP.NET Web フォームなど)、受信した Url は通常ディスク上のファイルにマップされます。 例: のような要求 URL の"/ディスク上のファイル"または"/されず"、「繰り返し」または「されず」ファイルで処理される可能性があります。

Web ベースの MVC フレームワークは、少し異なる方法で Url をサーバー コードにマップします。 受信した Url をファイルにマップするには、代わりには、代わりに、クラスのメソッドに Url をマップします。 これらのクラスは「コント ローラー」と呼ばれますとユーザー入力を処理する受信 HTTP 要求を処理する責任が、クライアント バックアップを取得して、データの保存と送信する応答を決定する (HTML を表示、ダウンロード ファイルを別にリダイレクトURL など)。

NerdDinner アプリケーションの基本的なモデルを構築しましたが、次の手順はコント ローラーのユーザー データ リスティング/詳細ナビゲーション エクスペリエンスを Dinners サイトに提供することを利用するアプリケーションを追加するとします。

### <a name="adding-a-dinnerscontroller-controller"></a>DinnersController コント ローラーの追加

当社の web プロジェクト内でコント ローラー」フォルダーを右クリックして開始し、いますが、**追加 -&gt;コント ローラー** (もコマンドを実行できるこの Ctrl M, Ctrl + C を入力して) メニューのコマンド。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

「コント ローラーの追加」ダイアログが表示されます。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

新しいコント ローラー"DinnersController"という名前がされ、[追加] ボタンをクリックします。 Visual Studio は、\Controllers ディレクトリの下に DinnersController.cs ファイルを追加します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

コード エディター内で新しい DinnersController クラスも開きます。

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>DinnersController クラスへの Index() と Details() アクション メソッドの追加

アプリケーションを使用して今後の dinners の一覧を参照し、固有の詳細については、それを表示する一覧で任意の Dinner をクリックすることを許可するユーザーを有効にします。 アプリケーションから次の Url を発行することで実施します。

| **URL** | **目的** |
| --- | --- |
| */Dinners/* | 今後の dinners の HTML リストを表示します。 |
| */Dinners 詳細/[id]* | これが一致するデータベースの dinner の DinnerID、URL 内に埋め込まれて、"id"パラメーターで指定された特定の dinner に関する詳細を表示します。 例:/Dinners/Details/2 DinnerID 値 2 は、Dinner の詳細を含む HTML ページが表示されます。 |

次のように、DinnersController クラスに 2 つのパブリック「操作方法」を追加することでこれらの Url の最初の実装を発行します。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

NerdDinner アプリケーションを実行し、呼び出し、ブラウザーを使用します。 入力、 *「Dinners/」* により、URL、 *Index()* 実行するメソッドが戻り、次の応答で送信は。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

入力、 *「/Dinners/詳細/2」* により、URL、 *Details()* メソッドを実行し、次の応答を返信します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

かもしれません。-ASP.NET MVC 知った、DinnersController クラスを作成し、これらのメソッドを呼び出しますか。 みましょうを理解するルーティングの動作の概要を確認します。

### <a name="understanding-aspnet-mvc-routing"></a>Understanding ASP.NET MVC のルーティング

ASP.NET MVC には、強力な URL ルーティング エンジンで Url をコント ローラー クラスにマップする方法を制御する柔軟性を提供するが含まれています。 ASP.NET MVC がどのメソッドを呼び出すには、できるだけでなくのさまざまな方法を変数できます。 が自動的に URL/クエリ文字列から解析されたパラメーターとしてメソッドに渡される構成を作成するには、どのコント ローラー クラスを選択する方法を完全にカスタマイズすることができます。引数。 まったく SEO (検索エンジンの最適化) のサイトを最適化するだけでなく、アプリケーションからすべての URL 構造を公開する柔軟性を提供します。

既定では、新しい ASP.NET MVC プロジェクトの URL ルーティング ルールが既に登録されている事前構成済みのセットが付属します。 これにより、アプリケーションで明示的に何も構成することがなく簡単に開始します。 既定のルーティング ルールの登録は、このプロジェクトのルートに"Global.asax"ファイルをダブルクリックして開くことができます: プロジェクトの"Application"クラス内で確認できます。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

既定の ASP.NET MVC ルーティング ルールは、このクラスの"RegisterRoutes"メソッド内で登録されます。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"ルート。MapRoute()"上記のメソッド呼び出しが受信した Url を URL の形式を使用して、コント ローラー クラスにマップされる既定のルーティング規則を登録します:"/{controller}/{action}/{id}"、インスタンス化するコント ローラー クラスの名前が"controller"–"action"はの名前をパブリック メソッドを呼び出すし、"id"は、メソッドに引数として渡すことができる URL に埋め込まれた省略可能なパラメーターです。 "MapRoute()"メソッドの呼び出しに渡される 3 番目のパラメーターは、一連の既定値は URL に存在しないことに、コント ローラーとアクション/id 値を使用する (コント ローラー アクションを"Home"= ="Index"、Id ="")。

以下は Url のさまざまな方法を示すテーブルが既定値を使用してマップされている"<em>/{コント ローラー}/{action}/{id}"</em>ルート ルール。

| **URL** | **コント ローラー クラス** | **アクション メソッド** | **渡されたパラメーター** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details(id) | id=2 |
| */Dinners/Edit/5* | DinnersController | Edit(id) | id=5 |
| */Dinners/Create* | DinnersController | Create() | N/A |
| */Dinners* | DinnersController | Index() | N/A |
| */Home* | HomeController | Index() | N/A |
| */* | HomeController | Index() | N/A |

最後の 3 つの行が既定値を表示する (コント ローラー = アクションでは、インデックス、Id を = ="") 使用されています。 1 つ指定されていない場合、既定のアクション名として"Index"メソッドが登録されているため、"/Dinners"し、そのコント ローラー クラスで呼び出される Url 原因 Index() アクション メソッドを"/home"。 いずれかが指定されていない場合に既定のコント ローラーとして"Home"コント ローラーが登録されているため、「/」URL と、作成するテンプレートを使用して、Index() アクション メソッドを呼び出すことが。

これら既定の URL ルーティング ルールしない場合は、良いニュースは簡単に変更するだけ、上記の RegisterRoutes メソッド内で編集されるは。 NerdDinner アプリケーションでは、既定の URL ルーティング ルールのいずれかを変更するつもり – 代わりにだけとして使用します-です。

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>DinnersController のときは必ず DinnerRepository を使用します。

みましょう DinnersController の現在の実装に置き換える Index() と Details() のアクション メソッドをモデルを使用する実装。

動作を実装する前に作成したときは必ず DinnerRepository クラスを使用します。 "NerdDinner.Models"名前空間を参照する"using"ステートメントを追加することで開始して、フィールドとして、DinnerController クラスのときは、必ず DinnerRepository インスタンスを宣言しました行います。

この章の「依存関係の挿入」の概念を紹介およびのときは、必ず DinnerRepository インスタンスを作成しますので、– テストが、右側の見方をするときは必ず DinnerRepository より良い単体できるへの参照を取得するコント ローラーを表示しましたします次のようにインラインです。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

取得したデータ モデル オブジェクトを使用して HTML 応答を生成する準備が整いました。

### <a name="using-views-with-our-controller"></a>コント ローラーとビューを使用します。

HTML をアセンブルし、使用して、アクション メソッド内でコードを記述することはできますが、 *Response.Write()* アプローチが迅速にかなり扱いにくくなります、クライアントに送信するヘルパー メソッド。 はるかに優れた方法がアプリケーションとデータ ロジック、DinnersController のアクション メソッド内でのみを実行して、HTML 形式の出力を担当する個別の「ビュー」テンプレートへの HTML 応答を表示するために必要なデータを渡すためにはことです。 すぐに表示されるよう、「表示」テンプレートは、通常の HTML マークアップとレンダリングの埋め込みコードの組み合わせを含むテキスト ファイルになります。

このビューの表示から、コント ローラー ロジックを分離するには、いくつかの大きな利点が表示されます。 具体的には、アプリケーション コードと UI の書式設定/レンダリング コード間をクリア"関心の分離"を適用できるようにします。 これがずっと簡単単体テスト アプリケーション ロジックを分離する UI のレンダリング ロジックから。 簡単にアプリケーション コードを変更することがなく UI レンダリング テンプレートを後で変更します。 そのやすく開発者および設計者は、プロジェクトの共同します。

"Void"に代わりに"ActionResult"の戻り値の型の戻り値の型のことから、2 つのアクション メソッドのメソッド シグネチャを変更することで、HTML UI の応答を返信するビュー テンプレートを使用することを示す、DinnersController クラスを更新できます。 呼び出し、 *View()* 以下のようなヘルパー メソッドを返すコント ローラーの基本クラスを"ViewResult"オブジェクト。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

署名、 *View()* 上を使用しているヘルパー メソッドは以下のようになります。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

最初のパラメーター、 *View()* ヘルパー メソッドは、HTML 応答を表示するために使用するビュー テンプレート ファイルの名前です。 2 番目のパラメーターは、テンプレートの表示が HTML 応答を表示するために必要なデータを含むモデル オブジェクトです。

呼び出す、Index() アクション メソッド内で、 *View()* ヘルパー メソッドとの dinners"Index"ビュー テンプレートを使用して、HTML の一覧を表示することを指定します。 ビュー テンプレートからリストを生成する Dinner オブジェクトのシーケンスを渡しています。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Details() アクション メソッド内には、URL 内で指定された id を使用して Dinner オブジェクトを取得しようとします。 有効な Dinner のいわゆるが見つかった場合、 *View()* "Details"ビュー テンプレートを使用して取得した Dinner オブジェクトを表示することを示す、ヘルパー メソッド。 役に立つエラー メッセージ"NotFound"のビュー テンプレートを使用して、Dinner が存在しないことを示す場合は、無効な夕食を要求すると、レンダリング (とオーバー ロードされたバージョンの*View()* ヘルパー メソッドをテンプレート名を受け取るだけです):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

これで、"NotFound"、「詳細」と"Index"ビュー テンプレートを実装してみましょう。

### <a name="implementing-the-notfound-view-template"></a>"NotFound"のビュー テンプレートの実装

まず、要求された dinner が見つからないことを示すわかりやすいエラー メッセージを表示する –"NotFound"のビュー テンプレートの実装します。

コント ローラー アクション メソッド内にテキスト カーソルを配置し、新しいテンプレートの表示を作成し、右クリックし、(私たちも実行できるこのコマンド Ctrl M, ctrl + V」と入力して)「ビューの追加」メニュー コマンドを選択します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

これは、次のような「ビューの追加」ダイアログが表示されます。 既定では、ダイアログ ボックスが作成するビューの名前を事前に設定のアクション メソッドの名前と一致する、カーソル時の (この例では「詳細」) では、ダイアログが起動されました。 まず、"NotFound"テンプレートを実装するため、このビューの名前をオーバーライドし、"NotFound"のように変更するように設定しましたします。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

[追加] ボタンをクリックすると、ときに Visual Studio は新しい"NotFound.aspx"ビュー テンプレートを (が、ディレクトリが既に存在しない場合も作成されます)、"\Views\Dinners"ディレクトリ内の作成します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

コード エディター内に、新しい"NotFound.aspx"ビュー テンプレートも開きます。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

既定ではテンプレートの表示がある 2 つ「コンテンツ領域」のコンテンツとコードを追加します。 1 つ目では、「タイトル」、HTML ページの返信をカスタマイズできます。 2 番目の内容をカスタマイズする"メイン"の HTML ページに送信できます。

"NotFound"のビュー テンプレートを実装するために、いくつかの基本的なコンテンツを追加します。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

試すことができますし、ブラウザー内で。 これを行うみましょう要求、 *「/Dinners/詳細/9999」* URL。 これは、データベースに現在存在しないし、"NotFound"のビュー テンプレートを表示するために、DinnersController.Details() アクション メソッドが発生する dinner をご覧ください。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

1 つに気付くことで、上記のスクリーン ショットは、基本的なビュー テンプレートには、一連の画面にメイン コンテンツを囲む HTML が継承されてます。 これは、このビュー テンプレートが、サイト上のすべてのビューで一貫性のあるレイアウトを適用できる「マスター ページ」テンプレートを使用しているためにです。 マスター ページのこのチュートリアルの後半で複数のしくみについて説明します。

### <a name="implementing-the-details-view-template"></a>「詳細」のビュー テンプレートの実装

「詳細」のビュー テンプレート – Dinner モデルが 1 つの HTML が生成されますを今すぐ実装してみましょう。

私たちします詳細アクション メソッド内でテキスト カーソルを配置し、右クリックし"ビューの追加 メニューのコマンドを選択するか Ctrl M, ctrl + V キーを押します)。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

「ビューの追加」ダイアログが表示されます。 既定のビュー名 (「詳細」) にしておきます。 ダイアログ ボックスで [厳密に型指定されたビューを作成する] チェック ボックスをオンもされ、コント ローラーからビューに渡されるモデルの種類の名前を (コンボ ボックスのドロップダウンを使用して) を選択します。 このビューは、Dinner オブジェクトを渡しています (この型の完全修飾名です:"NerdDinner.Models.Dinner")。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

私たちが付けた"空"ビューを作成する、以前のテンプレートとは異なりを自動的に選択して、この時間「スキャフォールディング」"Details"テンプレートを使用して表示します。 これは、前述のダイアログ ボックスで「コンテンツの表示」のドロップダウン リストを変更することで指定できます。

「スキャフォールディング」を渡している Dinner オブジェクトに基づいて、詳細ビュー テンプレートの最初の実装が生成されます。 これは、すぐに、ビュー テンプレートの実装を開始するための簡単な方法を提供します。

[追加] ボタンをクリックすると、ときに Visual Studio は新しい"Details.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内の作成します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

コード エディター内で、新しい"Details.aspx"ビュー テンプレートも開きます。 Dinner モデルに基づいて詳細ビューのスキャフォールディングの初期実装が含まれます。 スキャフォールディング エンジンは、.NET リフレクションを使用して、渡されたクラスで公開されているパブリック プロパティを確認し、見つけたの各種類に基づく適切なコンテンツを追加します。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

*「/1/Dinners/詳細」* ブラウザーでこの「詳細」のスキャフォールディングの実装がどのように表示する URL。 この URL を使用して手動で追加したデータベースに最初に作成したときに、dinners の 1 つ表示されます。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

これにより、すばやく作業を開始、私たちを取得し、この Details.aspx ビューの最初の実装を提供します。 移動しを満足度に UI をカスタマイズすることを調整できます。

もっとよく Details.aspx テンプレートを見てみると、静的な HTML が含まれていますと、レンダリング コードを埋め込みわかります。 &lt;%%&gt;コード ナゲットはビュー テンプレートをレンダリングするときにコードを実行し、 &lt;% = %&gt;コード ナゲットがそれに含まれるコードを実行し、テンプレートの出力ストリームに結果をレンダリングします。

厳密に型指定された"Model"プロパティを使用して、コント ローラーから渡された"Dinner"モデル オブジェクトにアクセスする、ビュー内でコードを記述できます。 Visual Studio により、完全なコードの intellisense、エディター内での「モデル」このプロパティにアクセスする場合。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

いくつかの修正は、最後の詳細ビュー テンプレートのソースは次のようになりますようにしましょう。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

アクセスして、 *「/1/Dinners/詳細」* URL もう一度これは、次のようなレンダリング。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>"Index"のビュー テンプレートの実装

今すぐ – 今後 Dinners の一覧を生成する"Index"ビュー テンプレートを実装してみましょう。 To-do これをインデックス アクション メソッド内にテキスト カーソルを配置し、右 をクリックし、"ビューの追加 メニューのコマンドを選択 (または Ctrl M, ctrl + V キーを押します)。

"ビューの追加 ダイアログ ボックスで"Index"という名前のテンプレートの表示の維持がされ"厳密に型指定されたビューを作成する チェック ボックスを選択します。 この時間を自動的に"List"ビュー テンプレートを生成し、"NerdDinner.Models.Dinner"モデルの種類として、ビューに渡されるを選択します (このスキャフォールディングと、ビューの追加ダイアログが前提としていますが、"List"を作成することによって示されたがためシーケンスを渡して Dinner オブジェクトのコント ローラーからビューに)。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

[追加] ボタンをクリックすると、ときに Visual Studio は新しい"Index.aspx"ビュー テンプレート ファイルを"\Views\Dinners"ディレクトリ内を作成します。 これは「スキャフォールディング」ビューに渡す Dinners の HTML テーブルの一覧を提供する内部に初期実装。

アプリケーションとアクセスを実行したときに、 *「Dinners/」* URL dinners の一覧を表示するようになります。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

上記のテーブル ソリューションでは、Dinner データ – これは非常に必要な Dinner リストに接続する、コンシューマーのグリッドのようなレイアウトをによりします。 Index.aspx ビュー テンプレートを更新し、データのより少ない列を一覧表示して使用するように変更を&lt;ul&gt;レンダリングして、次のコードを使用してテーブルではなく要素。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

このモデルで各 dinner 経由でループと上記の foreach ステートメント内で"の var"キーワードを使用しています。 Dinner オブジェクトがある遅延バインディングを意味"の var"を使用して c# 3.0 で未知のものと考えるかもしれません。 コンパイラが厳密に型指定された「モデル」プロパティに対して型推論を使用している代わりに意味 (型の"IEnumerable&lt;Dinner&gt;") といっぱいになることを意味する – Dinner の種類としてローカル"dinner"変数のコンパイルintellisense とコンパイル時のコード ブロック内でチェックします。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

更新をヒットしました、 */Dinners*以下のような更新されたビューの現時点では、ブラウザーで URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

これにより、ほうが検索しているはまだ完全にあります。 最後に、エンドユーザー、リスト内の個々 の Dinners をクリックし、それらについての詳細を参照してください。 これで、DinnersController の詳細アクション メソッドにリンクする HTML ハイパーリンク要素をレンダリングすることにより実装します。

2 つの方法のいずれかで、インデックス ビュー内でこれらのハイパーリンクを生成しています。 1 つは、HTML を手動で作成する&lt;、&gt;などの要素以下、場所を埋め込む&lt;%%&gt;ブロック内で、 &lt;、&gt; HTML 要素。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

使用して別のアプローチは、HTML をプログラムで作成をサポートする ASP.NET MVC 内で組み込みの"Html.ActionLink()"ヘルパー メソッドを活用するために、 &lt;、&gt;で別のアクション メソッドにリンクする要素、コント ローラー:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Html.ActionLink() のヘルパー メソッドの最初のパラメーターを表示するリンク テキストは、(ここでは、タイトル、dinner の) 2 番目のパラメーター (この場合、詳細メソッド) へのリンクを生成するコント ローラー アクション名は、3 番目のパラメーター(プロパティの名前/値を持つ匿名型として実装)、アクションに送信するパラメーターのセット。 ここでおにリンクするため、既定の URL ルーティング ルールの ASP.NET MVC では、dinner の"id"パラメーターを指定することは"{controller}/{action}/{id}"Html.ActionLink() ヘルパー メソッドは、次の出力が生成されます。

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Index.aspx ビュー Html.ActionLink() ヘルパー メソッドのアプローチを使用して各 dinner が適切な詳細情報の URL を一覧のリンクであるなります。

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

今すぐおをヒットした時と、 */Dinners* dinner 一覧は、次のように次の URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

一覧でディナーのいずれかをクリックしたとき詳細が表示に移動します。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>名前付け規則に基づくと \Views ディレクトリ構造

既定では ASP.NET MVC アプリケーションでは、テンプレートの表示を解決するときに、名前付け構造、規則ベースのディレクトリを使用します。 これにより、開発者は完全修飾の場所のパスから、コント ローラー クラス内でのビューを参照するときにする必要があります。 既定では ASP.NET MVC ビュー テンプレート ファイル内の検索は、* \Views\[ControllerName]\*アプリケーションの下にあるディレクトリ。

たとえば、次の 3 つのビュー テンプレートを明示的に参照する – DinnersController クラスに操作している:"Index"、「詳細」および"NotFound"。 ASP.NET MVC は既定で検索内でこれらのビュー、 *\Views\Dinners*アプリケーションのルート ディレクトリの下にあるディレクトリ。

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

プロジェクト内の 3 つのコント ローラー クラスのいる方法がありますの上に注意してください (DinnersController、HomeController と AccountController – プロジェクトを作成したときに、最後の 2 つが既定で追加された)、3 つのサブディレクトリ (1 つずつ、コント ローラー) \Views ディレクトリ内。

ホームおよびアカウント コント ローラーから参照されているビューは、それぞれから、ビュー テンプレートを自動的に解決*\Views\Home*と*\Views\Account*ディレクトリ。 *\Views\Shared*サブディレクトリが、アプリケーション内で複数のコント ローラー間で再利用されるビュー テンプレートを格納する方法を提供します。 内でチェックが最初に ASP.NET MVC がビュー テンプレートを解決しようとした場合、 *\Views\[コント ローラー]* 特定のディレクトリにビュー テンプレートを見つけられない場合がありますよ内で、 *\Views\共有*ディレクトリ。

個々 のビュー テンプレートの名前付けに関しては、推奨されるガイダンスが、ビュー テンプレートを表示するためにその原因となったアクション メソッドと同じ名前を共有します。 たとえば、「インデックス」上のアクション メソッドがビューの結果を表示するために"Index"ビューを使用して、"Details"アクション メソッドは、その結果を表示するために「詳細」ビューを使用しています。 これにより、簡単にすばやく参照するテンプレートは、各アクションに関連付けられています。

開発者は、ビュー テンプレートがコント ローラーで呼び出されるアクション メソッドと同じ名前を持つときにビュー テンプレートの名前を明示的に指定する必要はありません。 代わりにだけ、モデル オブジェクトに渡す"View()"のヘルパー メソッド (ビューの名前を指定)、なしと ASP.NET MVC を使用することが自動的に推測、 *\Views\[ControllerName]\[ActionName]* ビュー テンプレートのレンダリングのディスクにします。

これにより、コント ローラーのコードを少しクリーンアップし、2 回、コード内の名前の重複を避けます。

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

上記のコードは、サイトの便利な Dinner リスティング/詳細を実装するために必要なすべてが発生します。

#### <a name="next-step"></a>次の手順

ブラウズ エクスペリエンスの構築の優れた Dinner があるようになりました。

みましょう CRUD (作成、読み取り、更新、削除) データ フォームの編集のサポートを今すぐ有効にします。

> [!div class="step-by-step"]
> [前へ](build-a-model-with-business-rule-validations.md)
> [次へ](provide-crud-create-read-update-delete-data-form-entry-support.md)
