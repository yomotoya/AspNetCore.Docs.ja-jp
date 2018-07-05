---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
title: マスター ページと ASP.NET AJAX (VB) |Microsoft Docs
author: rick-anderson
description: ASP.NET AJAX とマスター ページを使用するためのオプションについて説明します。 ScriptManagerProxy クラスを使用して調べるさまざまな JS ファイルに dependi が読み込まれるしくみについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0ee9318c-29bb-4d58-b1dc-94e575b8ae10
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-vb
msc.type: authoredcontent
ms.openlocfilehash: 473a864730525d6dafd47680a587808b04574ff9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401819"
---
<a name="master-pages-and-aspnet-ajax-vb"></a>マスター ページと ASP.NET AJAX (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_VB.pdf)

> ASP.NET AJAX とマスター ページを使用するためのオプションについて説明します。 ScriptManagerProxy クラスを使用して調べるマスターに scriptmanager コントロールを使用するかどうかに応じて、さまざまな JS ファイルが読み込まれているページまたはコンテンツ ページ方法について説明します。


## <a name="introduction"></a>はじめに

過去数年、ますます多くの開発者が構築してきました AJAX 対応 web アプリケーション。 AJAX 対応の web サイトはさまざまな関連の web テクノロジを使用して、応答性の高いユーザー エクスペリエンスを提供します。 Microsoft ASP.NET AJAX フレームワークにも驚くほど簡単にお試しいただきありがとうは、AJAX 対応 ASP.NET アプリケーションを作成します。 ASP.NET AJAX は ASP.NET 3.5 および Visual Studio 2008 に組み込まれています。ASP.NET 2.0 アプリケーションの個別のダウンロードとして利用もできます。

ASP.NET AJAX フレームワークで AJAX 対応 web ページを構築するときに、framework を使用する各ページに正確に 1 つの ScriptManager コントロールを追加する必要があります。 その名前が示すように、scriptmanager コントロールは AJAX 対応 web ページで使用するクライアント側スクリプトを管理します。 少なくとも、scriptmanager コントロールは、その構成は、ASP.NET AJAX クライアント ライブラリの JavaScript ファイルをダウンロードするブラウザーに指示する HTML を出力します。 カスタム JavaScript ファイル、スクリプトが有効な web サービス、およびカスタム アプリケーション サービス機能を登録するも使用できます。

場合は、サイトの使用のマスター ページの (する必要があります) と、必ずしも必要はありませんすべて 1 つのコンテンツ ページに ScriptManager コントロールを追加するには代わりに、マスター ページに ScriptManager コントロールを追加できます。 このチュートリアルでは、マスター ページに ScriptManager コントロールを追加する方法を示します。 ScriptManagerProxy コントロールを使用して、特定のコンテンツ ページで、カスタム スクリプトとスクリプト サービスを登録する方法についても説明します。

> [!NOTE]
> このチュートリアルをデザインするか、ASP.NET AJAX フレームワークで AJAX 対応 web アプリケーションを構築は探索されません。 AJAX の使用に関する詳細については、ASP.NET AJAX のビデオとチュートリアルについては、さらに関連項目」セクションでこのチュートリアルの最後に記載したこれらのリソースを参照してください。


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>ScriptManager コントロールによって出力されるマークアップを調べる

ScriptManager コントロールは、その構成は、ASP.NET AJAX クライアント ライブラリを JavaScript ファイルをダウンロードするブラウザーに指示するマークアップを出力します。 また、インライン JavaScript のビットをこのライブラリを初期化するページに追加します。 次のマークアップは、ScriptManager コントロールを含むページのレンダリングされた出力に追加されるコンテンツを示しています。


[!code-html[Main](master-pages-and-asp-net-ajax-vb/samples/sample1.html)]

`<script src="url"></script>`タグ、ブラウザーをダウンロードして、JavaScript ファイルを実行するように指示*url*します。 Scriptmanager コントロールです。 このような 3 つのタグを出力します。1 つのファイルを参照`WebResource.axd`他の 2 つが、ファイルを参照中に、`ScriptResource.axd`します。 これらのファイルは、実際には、web サイト内のファイルとして存在しません。 代わりに、web サーバーでこれらのファイルのいずれかの要求が到着すると、ASP.NET エンジンは、クエリ文字列を検査し、適切な JavaScript コンテンツを返します。 これら 3 つの外部の JavaScript ファイルによって提供されるスクリプトでは、ASP.NET AJAX フレームワークのクライアント ライブラリを構成します。 他の`<script>`scriptmanager コントロールによって出力されるタグは、このライブラリを初期化するインライン スクリプトを含めます。

外部スクリプト参照と、scriptmanager コントロールによって出力されたインライン スクリプトは、ページ、ASP.NET AJAX フレームワークを使用しますが、フレームワークを使用しないページは必要ありませんが不可欠です。 そのため、ASP.NET AJAX フレームワークを使用して、それらのページにのみ、ScriptManager を追加する理想的なである理由を考えるかもしれません。 これで十分ですが、そのフレームワークを使用する多くのページがある場合、最終的に控えめに言って、繰り返しのタスクのすべてのページに ScriptManager コントロールを追加します。 または、すべてのコンテンツ ページに、このために必要なスクリプトを挿入し、マスター ページに、ScriptManager を追加できます。 この方法により、マスター ページで既に含まれているために、ASP.NET AJAX フレームワークを使用する新しいページに、ScriptManager を追加する必要はありません。 マスター ページに、ScriptManager を追加する手順 1 でについて説明します。

> [!NOTE]
> マスター ページのユーザー インターフェイス内での AJAX 機能などの場合は、少々 の選択肢があるない -、マスター ページで、scriptmanager コントロールを含める必要があります。


マスター ページに、scriptmanager コントロールを追加する 1 つの欠点で上記のスクリプトが出力されること*すべて* ページで、かどうかに関係なく、必要に応じて。 これは、明確にする (マスター ページ) を使用して含まれている ScriptManager をまだ ASP.NET AJAX フレームワークの機能を使用しないこれらのページの帯域幅を無駄につながります。 しかし、帯域幅が無駄にどの程度でしょうか。

- (上記参照)、scriptmanager コントロールによって出力される実際のコンテンツは、1 KB と少しを合計します。
- によって参照される 3 つの外部スクリプト ファイル、`<script>`要素には、約 450 KB のデータが圧縮されていないただし、構成されています。 ほぼ 100 KB gzip 圧縮を使用する web サイト、この帯域幅の合計を削減することができます。 ただし、これらのスクリプト ファイルは 1 年間、1 回ダウンロードする必要がのみ意味は、ブラウザーによってキャッシュされ、サイトの他のページで利用できます。

最良の場合、その後、スクリプト ファイルがキャッシュされると、総コストは、1 KB、ごくわずかであります。 最悪の場合、ただし - スクリプト ファイルがダウンロードされていないコンピューター上と、web サーバーはのすべての圧縮を使用できません - 帯域幅のヒットは約 450 KB であり、2 つ目または 2 へのブロード バンド接続を最大 1 分間の経由で任意の場所を追加できます。 ダイヤルアップ モデム経由でのユーザー。 さいわい、外部スクリプト ファイルは、ブラウザーによってキャッシュされるため、この最悪のシナリオが行われます頻度の低い。

> [!NOTE]
> まだ感じた場合、ScriptManager コントロールを配置するマスター ページで、Web フォームを検討してください。 (、`<form runat="server">`マスター ページのマークアップ)。 ポストバック モデルを使用するすべての ASP.NET ページには、正確に 1 つの Web フォームを含める必要があります。 Web フォームを追加するその他のコンテンツの追加: 隠しフォーム フィールドの数を`<form>`タグ自体、必要に応じて、JavaScript でスクリプトからポストバックを開始する機能します。 このマークアップは、ページをポストバックしない必要はありません。 マスター ページから Web フォームを削除して、手動で追加することが必要な各コンテンツ ページでは、この余分なマークアップを除去する可能性があります。 ただし、マスター ページで、Web フォームの利点から特定のコンテンツ ページに不必要に追加することの欠点を上回る。


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>手順 1: マスター ページに ScriptManager コントロールを追加します。

ASP.NET AJAX フレームワークを使用するすべての web ページには、ScriptManager コントロールを正確に 1 つを含める必要があります。 この要件により、通常は理にかなってすべてのコンテンツ ページの ScriptManager コントロールを自動的に含まれているように、1 つのマスター ページの ScriptManager コントロールを配置します。 さらに、scriptmanager コントロールは、ASP.NET AJAX サーバー コントロールの UpdatePanel コントロールと UpdateProgress コントロールなどのいずれかの前にする必要があります。 そのため、Web フォーム内でプレース ホルダー コントロールの前に、scriptmanager コントロールを配置することをお勧めします。

開く、`Site.master`マスター ページと前に、Web フォーム内のページに ScriptManager コントロールを追加、`<div id="topContent">`要素 (図 1 参照)。 Visual Web Developer 2008 または Visual Studio 2008 を使用している場合、ScriptManager コントロールは、[AJAX Extensions] タブで、ツールボックスにあります。Visual Studio 2005 を使用している場合は、まず、ASP.NET AJAX フレームワークをインストールし、ツールボックスにコントロールを追加する必要があります。 ASP.NET 2.0 のフレームワークを取得する ASP.NET AJAX ダウンロード ページを参照してください。

Scriptmanager コントロールをページに追加すると、次のように変更します。 その`ID`から`ScriptManager1`に`MyManager`します。


[![マスター ページに、scriptmanager コントロールを追加します。](master-pages-and-asp-net-ajax-vb/_static/image2.png)](master-pages-and-asp-net-ajax-vb/_static/image1.png)

**図 01**: マスター ページに、scriptmanager コントロールを追加 ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image3.png))。


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>手順 2: コンテンツ ページから、ASP.NET AJAX フレームワークを使用します。

マスター ページに追加された ScriptManager コントロールのようになりました framework の機能を ASP.NET AJAX コンテンツ ページに追加できます。 Northwind データベースからランダムに選択した製品を表示する新しい ASP.NET ページを作成しましょう。 新しい製品を表示しながら、15 秒ごとにこの表示を更新するのに ASP.NET AJAX フレームワークの Timer コントロールを使用します。

という名前のルート ディレクトリに新しいページを作成して開始`ShowRandomProduct.aspx`します。 この新しいページにバインドすることを忘れないでください、`Site.master`マスター ページ。


[![新しい ASP.NET ページ、web サイトを追加します。](master-pages-and-asp-net-ajax-vb/_static/image5.png)](master-pages-and-asp-net-ajax-vb/_static/image4.png)

**図 02**: 新しい ASP.NET ページ、web サイトを追加 ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image6.png))。


指定することを思い出してくださいをという名前のカスタム ベース ページ クラスを作成したタイトル、メタ タグ、およびマスター ページの [SKM1] チュートリアルでは、その他の HTML ヘッダー`BasePage`明示的に設定されていない場合は、ページのタイトルを生成しました。 移動して、`ShowRandomProduct.aspx`ページの分離コード クラスし、派生させる`BasePage`(の代わりにから`System.Web.UI.Page`)。

最後に、更新、`Web.sitemap`このレッスンのエントリを追加するファイル。 下に次のマークアップを追加、`<siteMapNode>`マスターの場合にコンテンツ ページの相互作用のレッスン。


[!code-xml[Main](master-pages-and-asp-net-ajax-vb/samples/sample2.xml)]

この追加`<siteMapNode>`要素は、レッスンに反映されます (図 5 参照) を一覧表示します。

### <a name="displaying-a-randomly-selected-product"></a>ランダムに選択された商品を表示します。

戻り`ShowRandomProduct.aspx`します。 デザイナーでは、UpdatePanel コントロールをドラッグ、ツールボックスから、`MainContent`コンテンツ コントロールと設定、`ID`プロパティを`ProductPanel`します。 UpdatePanel では、部分ページ ポストバックを通じて非同期的に更新可能な画面上の領域を表します。

最初のタスクでは、UpdatePanel 内でランダムに選択した製品についての情報を表示します。 DetailsView コントロールを updatepanel コントロールにドラッグすることで開始します。 DetailsView コントロールの設定`ID`プロパティを`ProductInfo`クリアとその`Height`と`Width`プロパティ。 DetailsView のスマート タグを展開し、データ ソースの選択ドロップダウン リストから、DetailsView をという名前の新しい SqlDataSource コントロールにバインドする選択`RandomProductDataSource`します。


[![DetailsView を新しい SqlDataSource コントロールにバインドします。](master-pages-and-asp-net-ajax-vb/_static/image8.png)](master-pages-and-asp-net-ajax-vb/_static/image7.png)

**図 03**: DetailsView を新しい SqlDataSource コントロールにバインド ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image9.png))。


SqlDataSource コントロールを使用して Northwind データベースへの接続を構成、 `NorthwindConnectionString` (これは、[コンテンツ] ページ [SKM2] チュートリアルからマスター ページと対話するで作成した)。 ときに、select ステートメントの構成では、カスタム SQL ステートメントを指定し、次のクエリを入力選択します。


[!code-sql[Main](master-pages-and-asp-net-ajax-vb/samples/sample3.sql)]

`TOP 1`キーワード、`SELECT`句は、クエリによって返される最初のレコードのみを返します。 `NEWID()`関数は、新しいグローバル一意識別子値 (GUID) を生成しで使用できる、`ORDER BY`句をランダムな順序でテーブルのレコードを返します。


[![1 つをランダムに選択したレコードを取得する SqlDataSource を構成します。](master-pages-and-asp-net-ajax-vb/_static/image11.png)](master-pages-and-asp-net-ajax-vb/_static/image10.png)

**図 04**: 構成を 1 つのランダムに選択したレコードを返す SqlDataSource ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image12.png))。


ウィザードの完了後に、Visual Studio は、BoundField、上記のクエリによって返される 2 つの列を作成します。 この時点で、ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample4.aspx)]

図 5 は、`ShowRandomProduct.aspx`ページをブラウザーで表示する場合。 ページを再読み込みするブラウザーの更新 ボタンをクリックします。表示する必要があります、`ProductName`と`UnitPrice`新しいランダムに選択されたレコードの値。


[![ランダムな製品の名前と価格が表示されます。](master-pages-and-asp-net-ajax-vb/_static/image14.png)](master-pages-and-asp-net-ajax-vb/_static/image13.png)

**図 05**: A のランダムな製品の名前と価格が表示されます ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image15.png))。


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>自動的に新しい製品を表示する 15 秒ごと

ASP.NET AJAX フレームワークには、指定の時刻にポストバックを実行するタイマー コントロールが含まれています。ポストバック、タイマーの`Tick`イベントが発生します。 Timer コントロールは、UpdatePanel 内で配置されている場合は、新しいランダムに選択した製品を表示するには、ある DetailsView にデータをバインド私たちを部分ページ ポストバックをトリガーします。

これを行うには、ツールボックスからタイマーをドラッグし、updatepanel コントロールにドロップします。 変更する、タイマーの`ID`から`Timer1`に`ProductTimer`とその`Interval`プロパティ 60000 から 15000 にします。 `Interval`プロパティは、ポストバック間のミリ秒数を示します。 15000 に設定すると、15 秒ごとに、部分ページ ポストバックをトリガーするタイマーです。 この時点で、タイマーの宣言型マークアップは、次のようになります。


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample5.aspx)]

タイマーのイベント ハンドラーを作成`Tick`イベント。 このイベント ハンドラーで DetailsView を呼び出すことによって、DetailsView にデータをバインドし直す必要があります`DataBind`メソッド。 そう、DetailsView、データ ソース コントロールからデータを再取得するように指示しますを選択し、ランダムにで、新しい表示が、選択したレコードだけです (ときに、ブラウザーの更新 ボタンをクリックして、ページの再読み込み) など。


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample6.vb)]

GridView を ObjectDataSource にバインドされているユーザーのブラウザーでページ上の空白の領域でどのマークアップもレンダリングしません。 ブラウザーを使用してページを再検討します。 最初に、ランダムな製品の情報が表示されます。 気長に画面を監視するが、15 秒後に新しい製品についての情報魔法のように代わる、既存の表示がわかります。

何が起こってよりわかりやすく表示、表示が最後に更新された時刻を表示する UpdatePanel に Label コントロールを追加してみましょう。 UpdatePanel 内のラベルの Web コントロールを追加、`ID`に`LastUpdateTime`、オフとその`Text`プロパティ。 次に、UpdatePanel のイベント ハンドラーを作成`Load`イベントと、ラベルに現在の時刻を表示します。 (UpdatePanel の`Load`完全または部分ページ ポストバックごとでイベントが発生しました)。


[!code-vb[Main](master-pages-and-asp-net-ajax-vb/samples/sample7.vb)]

完全なこの変更は、ページには、現在表示されている製品が読み込まれた時間が含まれます。 図 6 は、最初にアクセスする際、ページを示します。 図 7 では、Timer コントロールが「オン」と、新しい製品についての情報を表示する、UpdatePanel が更新された後、ページが 15 秒後に示します。


[![ページの読み込み時にランダムに選択した製品が表示されます。](master-pages-and-asp-net-ajax-vb/_static/image17.png)](master-pages-and-asp-net-ajax-vb/_static/image16.png)

**図 06**: A ランダムに選択の製品がページの読み込み時に表示されます ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image18.png))。


[![15 秒に、新しいランダムに選択した製品が表示されます。](master-pages-and-asp-net-ajax-vb/_static/image20.png)](master-pages-and-asp-net-ajax-vb/_static/image19.png)

**図 07**: 15 秒ごと、新しいランダムに選択した製品が表示されます ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image21.png))。


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>手順 3: ScriptManagerProxy コントロールを使用します。

ASP.NET AJAX framework クライアント ライブラリの必要なスクリプトを含む、と共に、scriptmanager コントロールは、カスタム JavaScript ファイル、スクリプト対応の Web サービスとカスタム認証、承認、およびプロファイル サービスへの参照にも登録できます。 通常、このようなカスタマイズは、特定のページに固有です。 ただし、カスタム スクリプト ファイル、Web サービス参照、または認証、承認、またはプロファイル サービスがマスター ページで、scriptmanager コントロールで参照されている場合、それらに含ま、web サイトのすべてのページ。

追加するには、は、ページごとに、scriptmanager コントロールに関連するカスタマイズは、ScriptManagerProxy コントロールを使用します。 コンテンツ ページに、ScriptManagerProxy を追加し、カスタム JavaScript ファイル、Web サービス参照の場合、または認証、承認、または ScriptManagerProxy; からプロファイル サービスに登録特定のコンテンツ ページのこれらのサービスを登録するための効果があります。

> [!NOTE]
> ASP.NET ページには、存在する 1 つ以内の ScriptManager コントロールだけを設定できます。 そのため、マスター ページで、ScriptManager コントロールが既に定義されている場合は、コンテンツ ページに ScriptManager コントロールを追加できません。 ScriptManagerProxy の唯一の目的は、開発者は、マスター ページで、scriptmanager コントロールの定義が、まだページごとに scriptmanager コントロールのカスタマイズを追加する機能の手段を提供します。


ScriptManagerProxy コントロールの動作を確認するで UpdatePanel を強化しましょう`ShowRandomProduct.aspx`クライアント側スクリプトを使用して、一時停止または再開 Timer コントロール ボタンを追加します。 タイマー コントロールでは、この目的の機能を実現するために使用できる 3 つのクライアント側方法があります。

- `_startTimer()` -タイマー コントロールの開始
- `_raiseTick()` -ポストバックをサーバー上には、その Tick イベントを発生させることは、「ティック」に、タイマー コントロールをそれによってによって
- `_stopTimer()` -タイマー コントロールを停止します。

という名前の変数と JavaScript ファイルを作成しましょう`timerEnabled`という名前の関数と`ToggleTimer`します。 `timerEnabled`変数は、Timer コントロールが現在有効または無効になっているかどうかを示します。 既定値は true。 `ToggleTimer`関数は 2 つの入力パラメーターを受け取ります。 一時停止/再開ボタンとクライアント側への参照を`id`タイマー コントロールの値。 この関数の値が切り替わります`timerEnabled`、タイマー コントロールへの参照を取得または開始、タイマーを停止します (の値に応じて`timerEnabled`)、「一時停止」または"Resume"に、ボタンの表示テキストを更新します。 この関数は、一時停止/再開ボタンがクリックされたときに呼び出されます。

という名前の web サイトで新しいフォルダーを作成して開始`Scripts`します。 という名前の Scripts フォルダーに新しいファイルを次に、追加`TimerScript.js`の JScript ファイルの種類。


[![新しい JavaScript ファイルを Scripts フォルダーに追加します。](master-pages-and-asp-net-ajax-vb/_static/image23.png)](master-pages-and-asp-net-ajax-vb/_static/image22.png)

**図 08**: 新しい JavaScript ファイルを追加、`Scripts`フォルダー ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image24.png))。


[![新しい JavaScript ファイルが web サイトに追加されました](master-pages-and-asp-net-ajax-vb/_static/image26.png)](master-pages-and-asp-net-ajax-vb/_static/image25.png)

**図 09**: 新しい JavaScript ファイルが web サイトに追加されました ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image27.png))。


次のスクリプトを次に、追加、`TimerScript.js`ファイル。


[!code-csharp[Main](master-pages-and-asp-net-ajax-vb/samples/sample8.cs)]

今すぐにこのカスタム JavaScript ファイルを登録する必要があります`ShowRandomProduct.aspx`します。 戻り`ShowRandomProduct.aspx`ScriptManagerProxy コントロールをページに追加は、設定しその`ID`に`MyManagerProxy`します。 カスタム JavaScript を登録するには、は、ファイルはデザイナーで ScriptManagerProxy コントロールを選択し、[プロパティ] ウィンドウに移動します。 スクリプトをタイトル、プロパティのいずれかです。 このプロパティを選択するには、図 10 に示すように、ScriptReference コレクション エディターが表示されます。 新しいスクリプト参照を含めるパス プロパティでスクリプト ファイルへのパスを入力して [追加] ボタンをクリックします。`~/Scripts/TimerScript.js`します。


[![ScriptManagerProxy コントロールへのスクリプト参照を追加します。](master-pages-and-asp-net-ajax-vb/_static/image29.png)](master-pages-and-asp-net-ajax-vb/_static/image28.png)

**図 10**: ScriptManagerProxy コントロールへのスクリプト参照の追加 ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image30.png))。


マークアップを含むように更新した後、宣言型のスクリプト参照、ScriptManagerProxy コントロールを追加する、`<Scripts>`を 1 つのコレクション`ScriptReference`エントリ、次のマークアップ スニペットとして示します。


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample9.aspx)]

`ScriptReference`エントリは、出力されるマークアップに JavaScript ファイルへの参照を含める ScriptManagerProxy を指示します。 カスタムを登録することによってスクリプト、ScriptManagerProxy、`ShowRandomProduct.aspx`ページのレンダリングされた出力が含まれています別`<script src="url"></script>`タグ:`<script src="Scripts/TimerScript.js" type="text/javascript"></script>`します。

今すぐ呼び出し、`ToggleTimer`関数で定義されている`TimerScript.js`でクライアント スクリプトから、`ShowRandomProduct.aspx`ページ。 UpdatePanel 内で、次の HTML を追加します。


[!code-aspx[Main](master-pages-and-asp-net-ajax-vb/samples/sample10.aspx)]

これには、「一時停止」というテキスト付きのボタンが表示されます。 たびに、クリックすると、JavaScript 関数`ToggleTimer`ボタンへの参照を渡すという、`id`タイマー コントロールの値 (`ProductTimer`)。 取得するための構文に注意してください、`id`タイマー コントロールの値。 `<%=ProductTimer.ClientID%>` 値を出力、`ProductTimer`タイマー コントロールの`ClientID`プロパティ。 コンテンツ ページ [SKM3] チュートリアルでは名前付けコントロール ID で、サーバー側の違いを説明しました`ID`値と、結果として得られるクライアント側`id`値、および方法`ClientID`クライアント側に返します`id`。

図 11 では、まずブラウザーからアクセスすると、このページを示します。 タイマーが現在実行されているし、15 秒に表示されている製品情報を更新します。 図 12 では、一時停止 ボタンがクリックしてされた後に、画面が表示されます。 一時停止 ボタンをクリックすると、タイマーを停止し、"Resume"するボタンのテキストを更新します。 製品情報が更新 (および 15 秒ごとに更新し続ける) と、ユーザーが再開をクリックします。


[![タイマー コントロールを停止する [一時停止] ボタンをクリックします。](master-pages-and-asp-net-ajax-vb/_static/image32.png)](master-pages-and-asp-net-ajax-vb/_static/image31.png)

**図 11**: Timer コントロールを停止する [一時停止] ボタンをクリックします ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image33.png))。


[![タイマーを再起動する [再開] ボタンをクリックします](master-pages-and-asp-net-ajax-vb/_static/image35.png)](master-pages-and-asp-net-ajax-vb/_static/image34.png)

**図 12**: タイマーを再起動する [再開] ボタンをクリックします ([フルサイズの画像を表示する をクリックします](master-pages-and-asp-net-ajax-vb/_static/image36.png))。


## <a name="summary"></a>まとめ

ASP.NET AJAX フレームワークを使用して AJAX 対応 web アプリケーションを構築する際に、すべての AJAX 対応 web ページに ScriptManager コントロールが含まれている必要があります。 このプロセスを容易にするには、各コンテンツ ページに、ScriptManager を追加する必要があるのではなく、マスター ページに、ScriptManager を追加できます。 手順 1 で参照されるコンテンツのページに AJAX 機能を実装する手順 2 の中にマスター ページに、scriptmanager コントロールを追加する方法について説明しました。

カスタム スクリプト、スクリプト対応の Web サービスへの参照を追加する必要がありますまたは、コンテンツ ページに ScriptManagerProxy コントロールを追加し、構成のカスタマイズされた認証、承認、またはプロファイル サービスは、特定のコンテンツ ページを、。カスタマイズがあります。 手順 3 が、ScriptManagerProxy を使用して、特定のコンテンツ ページで、カスタム JavaScript ファイルを登録する方法を検証します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET AJAX フレームワーク](../../../../ajax/index.md)
- [ASP.NET AJAX のチュートリアル](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX のビデオ](../../../videos/aspnet-ajax/index.md)
- [Microsoft ASP.NET AJAX を使用した対話型ユーザー インターフェイスの構築](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [レコードのランダムに並べ替えを NEWID を使用します。](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [タイマー コントロールを使用します。](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](interacting-with-the-content-page-from-the-master-page-vb.md)
> [次へ](specifying-the-master-page-programmatically-vb.md)
