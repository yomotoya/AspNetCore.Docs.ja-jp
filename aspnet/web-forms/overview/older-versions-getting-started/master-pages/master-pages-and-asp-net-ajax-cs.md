---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: マスター ページや ASP.NET AJAX (c#) |Microsoft ドキュメント
author: rick-anderson
description: ASP.NET AJAX およびマスター ページを使用するためのオプションについて説明します。 ScriptManagerProxy クラスを使用して変更について見てさまざまな JS ファイルに dependi が読み込まれるしくみについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 87e5855354610723823da88ec961e7391c3f705f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888781"
---
<a name="master-pages-and-aspnet-ajax-c"></a>マスター ページや ASP.NET AJAX (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> ASP.NET AJAX およびマスター ページを使用するためのオプションについて説明します。 ScriptManagerProxy クラスを使用して変更について見てScriptManager は、マスターで使用するかどうかに応じて、さまざまな JS ファイルが読み込まれているページまたは コンテンツ ページ方法について説明します。


## <a name="introduction"></a>はじめに

ますます多くの開発者が構築されている過去数年、 [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-web アプリケーションを有効にします。 AJAX 対応の web サイトでは、複数の関連する web テクノロジを使用して、応答性の高いユーザー エクスペリエンスを提供します。 Microsoft の驚くほど簡単にできるは、AJAX 対応の ASP.NET アプリケーションを作成する[ASP.NET AJAX framework](../../../../ajax/index.md)です。 ASP.NET AJAX が ASP.NET 3.5 と Visual Studio 2008; に組み込まれています。ASP.NET 2.0 アプリケーションの個別のダウンロードとして利用もできます。

ASP.NET AJAX フレームワークでの AJAX 対応の web ページを作成するときに、正確に 1 つを追加する必要があります[ScriptManager コントロール](https://msdn.microsoft.com/library/bb398863.aspx)framework を使用する各ページにします。 その名前からわかるように、ScriptManager は、AJAX 対応 web ページで使用するクライアント側スクリプトを管理します。 少なくともは、ScriptManager は、HTML、JavaScript ファイルにその構成の ASP.NET AJAX クライアント ライブラリをダウンロードするブラウザーに指示するを出力します。 カスタムの JavaScript ファイル、スクリプトが有効な web サービス、およびカスタム アプリケーション サービス機能を登録するも使用できます。

場合は、サイトは、マスター ページ、(ようにする必要があります)、するは必要ありませんすべて単一のコンテンツ ページに ScriptManager コントロールを追加するには代わりに、マスター ページに ScriptManager コントロールを追加することができます。 このチュートリアルでは、マスター ページに ScriptManager コントロールを追加する方法を示します。 ScriptManagerProxy コントロールを使用して、特定のコンテンツ ページで、カスタム スクリプトとスクリプト サービスを登録する方法についても説明します。

> [!NOTE]
> このチュートリアルをデザインするか、ASP.NET AJAX framework での AJAX 対応 web アプリケーションを構築には表示されません。 AJAX を使用する方法についてを参照してください、 [ASP.NET AJAX ビデオ](../../../videos/aspnet-ajax/index.md)と[チュートリアル](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)、および、このチュートリアルの最後に、関連項目セクションに記載されているリソースとして。


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>ScriptManager コントロールによって出力されるマークアップを確認します。

ScriptManager コントロールは、その構成の ASP.NET AJAX クライアント ライブラリ、JavaScript ファイルをダウンロードするブラウザーに指示するマークアップを生成します。 また、このライブラリを初期化するページにインライン JavaScript のビットを追加します。 次のマークアップでは、ScriptManager コントロールを含んでいるページのレンダリングされた出力に追加されているコンテンツを示します。


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

`<script src="url"></script>`タグ、ブラウザーをダウンロードして、JavaScript ファイルを実行するように指示*url*です。 ScriptManager を出力します。 このような 3 つのタグ1 つの参照ファイル`WebResource.axd`他の 2 つが、ファイルを参照中に、`ScriptResource.axd`です。 これらのファイルは、実際には、web サイト内のファイルとして存在しません。 代わりに、web サーバーでこれらのファイルのいずれかの要求が到着すると、ASP.NET エンジンは、クエリ文字列を検査し、適切な JavaScript コンテンツを返します。 これら 3 つの外部の JavaScript ファイルによって提供されるスクリプトでは、ASP.NET AJAX フレームワークのクライアント ライブラリを構成します。 他の`<script>`scriptmanager コントロールによって生成されます。 タグは、このライブラリを初期化するインライン スクリプトを含めます。

外部スクリプト参照および scriptmanager コントロールによって出力されたインライン スクリプト ページ、ASP.NET AJAX framework を使用しますが、framework を使用しないページには必要ありませんを不可欠です。 そのため、ASP.NET AJAX framework を使用してこれらのページにのみ、ScriptManager を追加する場合に適してであるを理由可能性があります。 これは、十分なが framework を使用するページが多数ある場合、最終的だめなの繰り返しのタスクのすべてのページに ScriptManager コントロールを追加します。 代わりに、マスター ページで、すべてのコンテンツ ページにこの必要なスクリプトを挿入する ScriptManager を追加することができます。 この方法では、マスター ページが既に含まれているため、ASP.NET AJAX framework を使用する新しいページに ScriptManager を追加する必要はありません。 手順 1 ではについて説明マスター ページに ScriptManager を追加します。

> [!NOTE]
> マスター ページのユーザー インターフェイス内での AJAX 機能のように計画する重要なに選択肢があるなしに、マスター ページに ScriptManager を含める必要があります。


マスター ページに ScriptManager を追加する 1 つの欠点は、上記のスクリプトが内で出力*すべて* ページで、かどうかに関係なく、必要なです。 これは、これらのページ (マスター ページ) を使用して含まれている ScriptManager があるまだ ASP.NET AJAX framework の機能を使用しないの無駄な帯域幅に明確につながります。 しかし、帯域幅が無駄になるがどの程度でしょうか。

- (上記) scriptmanager コントロールによって生成されます。 実際のコンテンツは、1 KB 強を合計します。
- によって参照される 3 つの外部スクリプト ファイル、`<script>`要素、ただし、約 450 KB のデータを圧縮を構成; 100 KB の近くに gzip で圧縮を使用する web サイト、この合計の帯域幅を削減できます。 ただし、これらのスクリプト ファイルは 1 年間、1 回ダウンロードする必要がのみ意味は、ブラウザーでキャッシュされ、し、再利用できる他のページ、サイトに。

最良の場合、次に、スクリプト ファイルがキャッシュされると、総コストは、1 KB はごくわずかです。 最悪の場合、ただし、これは、スクリプト ファイルがダウンロードされていないコンピューター上と、web サーバーを使用していない任意の形式の圧縮の - 帯域幅ヒットは、約 450 KB で、2 台目または 2 で 1 分以内にブロード バンド接続経由で任意の場所を追加できます。 ダイヤルアップ モデム ユーザー数。 良いニュースは、外部スクリプト ファイルは、ブラウザーでキャッシュされるためこの最悪のシナリオが実行される頻度が低いです。

> [!NOTE]
> まだ感じた場合、マスター ページに ScriptManager コントロールを配置する、Web フォームを検討してください。 (、`<form runat="server">`マスター ページのマークアップ)。 ポストバックのモデルを使用するすべての ASP.NET ページには、正確に 1 つの Web フォームを含める必要があります。 Web フォームを追加する追加のコンテンツを追加: 隠しフォーム フィールドの数を`<form>`タグ自体と、JavaScript 関数スクリプトからのポストバックを開始するための必要に応じて、します。 このマークアップは、ポストバックしないページの必要はありません。 マスター ページから Web フォームを削除して、手動で追加することが必要な各コンテンツ ページでは、この余分なマークアップを除去する可能性があります。 ただし、マスター ページで、Web フォームの利点は、不必要に特定のコンテンツ ページに追加してから欠点を上回る場合します。


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>手順 1: マスター ページに ScriptManager コントロールを追加します。

ASP.NET AJAX framework を使用するすべての web ページには、ScriptManager コントロールを正確に 1 つを含める必要があります。 この要件のために、通常はすべてのコンテンツ ページが自動的に含まれている ScriptManager コントロールできるように、マスター ページの 1 つには、ScriptManager コントロールを配置します。 さらに、ScriptManager は、UpdatePanel と UpdateProgress コントロールなど、ASP.NET AJAX サーバー コントロールのいずれかの前にする必要があります。 そのため、Web フォーム内でプレース ホルダー コントロールの前に ScriptManager を配置することをお勧めします。

開く、`Site.master`マスター ページと、その前に、Web フォーム内のページに ScriptManager コントロールを追加、`<div id="topContent">`要素 (図 1 を参照してください)。 Visual Web Developer 2008 または Visual Studio 2008 を使用している場合は、ScriptManager コントロールは [AJAX Extensions] タブで、ツールボックスにあります。Visual Studio 2005 を使用している場合は、最初に、ASP.NET AJAX framework をインストールして、コントロールをツールボックスに追加する必要があります。 参照してください、 [ASP.NET AJAX Wiki](https://github.com/DevExpress/AjaxControlToolkit/wiki) ASP.NET 2.0 のフレームワークを取得します。

ScriptManager をページに追加すると、次のように変更します。 その`ID`から`ScriptManager1`に`MyManager`です。


[![マスター ページに ScriptManager を追加します。](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**図 01**: マスター ページに ScriptManager を追加する ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>手順 2: フレームワークを使用して、ASP.NET AJAX コンテンツ ページから

マスター ページに追加した ScriptManager コントロールのコンテンツ ページに ASP.NET AJAX framework の機能を今すぐ追加できます。 Northwind データベースからランダムに選択した製品を表示する新しい ASP.NET ページを作成してみましょう。 新しい製品を表示しながら、15 秒おきこの表示を更新するのに ASP.NET AJAX フレームワークの Timer コントロールを使用します。

まず、という名前のルート ディレクトリに新しいページを作成して`ShowRandomProduct.aspx`です。 この新しいページへのバインドに注意してください、`Site.master`マスター ページ。


[![Web サイトに新しい ASP.NET ページを追加します。](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**図 02**: web サイトに新しい ASP.NET ページを追加 ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image6.png))


注意してください、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)という名前のカスタム ベース ページ クラスを作成したチュートリアル`BasePage`が場合、ページのタイトルを生成します。明示的に設定します。 移動して、`ShowRandomProduct.aspx`ページの分離コード クラスをから派生して`BasePage`(の代わりにから`System.Web.UI.Page`)。

最後に、更新、`Web.sitemap`このレッスンのエントリを追加するファイル。 下に次のマークアップを追加、`<siteMapNode>`マスター ページのコンテンツの相互作用レッスンします。


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

この追加`<siteMapNode>`要素は、レッスンに反映されます (図 5 を参照してください) を一覧表示します。

### <a name="displaying-a-randomly-selected-product"></a>ランダムに選択した製品を表示します。

戻る`ShowRandomProduct.aspx`です。 デザイナーから、ツールボックスから UpdatePanel コントロールをドラッグして、`MainContent`コンテンツ コントロールと設定、`ID`プロパティを`ProductPanel`です。 UpdatePanel では、部分ページのポストバックを通じて非同期的に更新できる画面上の領域を表します。

最初のタスクでは、UpdatePanel 内でランダムに選択された製品に関する情報を表示します。 UpdatePanel に DetailsView コントロールをドラッグして開始します。 DetailsView コントロールの設定`ID`プロパティを`ProductInfo`をクリアし、`Height`と`Width`プロパティです。 DetailsView のスマート タグを展開し、DetailsView コントロールにバインドする、新しい SqlDataSource という名前を選択して、データ ソースの選択-ドロップダウン リストから、`RandomProductDataSource`です。


[![DetailsView を新しい SqlDataSource コントロールにバインドします。](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**図 03**: DetailsView を新しい SqlDataSource コントロールにバインド ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image9.png))


SqlDataSource コントロールを使用して、Northwind データベースへの接続を構成、 `NorthwindConnectionString` (で作成したが、 [*コンテンツ ページから、マスター ページと対話する*](interacting-with-the-content-page-from-the-master-page-cs.md)チュートリアル)。 ときに、select ステートメントの構成を選択してカスタム SQL ステートメントを指定し、次のクエリを入力します。


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

`TOP 1`キーワード、`SELECT`句には、クエリによって返される最初のレコードのみが返されます。 [ `NEWID()`関数](https://msdn.microsoft.com/library/ms190348.aspx)新しい生成[グローバルに一意の識別子の値 (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)で使用できると、`ORDER BY`句をランダムな順序でテーブルのレコードを返します。


[![構成をランダムに選択された 1 つのレコードを返す SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**図 04**: 構成を 1 つのランダムに選択したレコードを返す SqlDataSource ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image12.png))


ウィザードの完了後は、Visual Studio は、上記のクエリによって返される 2 つの列の BoundField を作成します。 この時点で、ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

図 5 は、`ShowRandomProduct.aspx`ページをブラウザーで表示する場合。 ページを再度読み込んで、ブラウザーの更新 ボタンをクリックします。表示されるはずの`ProductName`と`UnitPrice`新しいランダムに選択されたレコードの値。


[![ランダムな製品の名前と価格が表示されます。](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**図 05**: A のランダムな製品の名前と価格が表示されます ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>自動的に新しい製品を表示する 15 秒おき

ASP.NET AJAX フレームワークには、指定された時間にポストバックを実行するタイマー コントロールが含まれています。タイマーのポストバック`Tick`イベントが発生します。 UpdatePanel 内タイマー コントロールが配置されている場合は、新しいランダムに選択された製品を表示する DetailsView にデータをバインドおによって、部分ページのポストバックがトリガーされます。

これを実現するには、タイマーをツールボックスからドラッグし、UpdatePanel にドロップします。 変更するタイマーの`ID`から`Timer1`に`ProductTimer`とその`Interval`プロパティ 60000 から 15000 にします。 `Interval`プロパティ ポストバックまでのミリ秒数を示します。 15000 に設定すると、部分ページのポストバックを 15 秒おきにトリガーするタイマーです。 この時点で、タイマーの宣言型マークアップは、次のようになります。


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

タイマーのイベント ハンドラーを作成する`Tick`イベント。 このイベント ハンドラーで DetailsView を呼び出すことによって、DetailsView にデータをバインドし直す必要があります`DataBind`メソッドです。 これによりそのデータ ソース コントロールからのデータを再取得する DetailsView に指示を選択し、新しいをランダムに表示されるはレコードが選択されました (同様に場合は、ページを再度読み込んで、ブラウザーの更新 ボタンをクリックして)。


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

すべてであることには! ブラウザーでページを再表示します。 最初に、ランダムな製品の情報が表示されます。 根気画面を見ることが、15 秒後に新しい製品について魔法が置き換えられる既存の表示がわかります。

ここで起こっているよりわかりやすく表示、表示が最後に更新された日時を表示する UpdatePanel にラベル コントロールを追加してみましょう。 UpdatePanel 内にあるラベル Web コントロールを追加、設定、`ID`に`LastUpdateTime`、オフとその`Text`プロパティです。 UpdatePanel のイベント ハンドラーを次に、作成`Load`イベントとラベルの現在の時刻を表示します。 (UpdatePanel の`Load`完全または部分的なページ ポストバックごとでイベントが発生しました)。


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

完了したらこの変更により、ページには、現在表示されている製品が読み込まれた時刻が含まれます。 図 6 は、最初にアクセスされるときに、ページを示します。 図 7 では、タイマー コントロール「でも入った時点」と、新しい製品に関する情報を表示する、UpdatePanel が更新された後、ページが 15 秒後に示します。


[![ページの読み込みにランダムに選択した製品が表示されます。](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**図 06**: ページの読み込みに A にランダムに選択されている製品が表示されます ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![15 秒に、新しいランダムに選択した製品が表示されます。](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**図 07**: 15 秒ごとに新しいランダムに選択した製品が表示されます ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>手順 3: ScriptManagerProxy コントロールの使用

ASP.NET AJAX framework クライアント ライブラリの必要なスクリプトを含む、と共に、ScriptManager は、カスタムの JavaScript ファイル、スクリプトが有効な Web サービス、およびカスタム認証、承認、およびプロファイル サービスへの参照にも登録できます。 通常このようなカスタマイズは、特定のページに固有です。 ただし、カスタム スクリプト ファイル、Web サービス参照、または認証、承認、またはプロファイル サービスがマスター ページに ScriptManager で参照されている場合にも含まれて*すべて*web サイト内のページです。

追加するには、ページの単位では、ScriptManager を関連のカスタマイズ設定は、ScriptManagerProxy コントロールを使用します。 コンテンツ ページに、ScriptManagerProxy を追加し、カスタムの JavaScript ファイル、Web サービス参照の追加、または認証、承認、または ScriptManagerProxy; からプロファイル サービスに登録できます。これにより、これらのサービスを特定のコンテンツ ページを登録する次の効果があります。

> [!NOTE]
> ASP.NET ページには、2 つ以上存在するには、ScriptManager コントロールだけを配置できます。 そのため、マスター ページに ScriptManager コントロールが既に定義されている場合は、コンテンツ ページに ScriptManager コントロールを追加できません。 ScriptManagerProxy の唯一の目的では、マスター ページでは、ScriptManager を定義するが、ページ単位ごとに ScriptManager のカスタマイズを追加する機能、まだに開発者のための手段です。


ScriptManagerProxy コントロールの動作を表示するで UpdatePanel を拡張してみましょう`ShowRandomProduct.aspx`をクライアント側スクリプトを使用して、一時停止または Timer コントロールを再開するボタンを追加します。 タイマー コントロールでは、この目的の機能を実現するために使用できる 3 つのクライアント側方法があります。

- `_startTimer()` -タイマー コントロールの開始
- `_raiseTick()` -ポスト バックおよびさせると、「ティック」タイマー コントロールをそれによってによってその`Tick`サーバー上のイベント
- `_stopTimer()` -Timer コントロールを停止します。

という名前の変数での JavaScript ファイルを作成しましょう`timerEnabled`という名前の関数と`ToggleTimer`です。 `timerEnabled`変数は、タイマー コントロールが現在有効または無効になっているかどうかを示します。 既定値は true です。 `ToggleTimer`関数は、2 つの入力パラメーターを受け取ります。 一時停止/再開 ボタンとクライアント側への参照を`id`タイマー コントロールの値。 この関数の値が切り替わります`timerEnabled`、Timer コントロールへの参照を取得が起動またはタイマーを停止します (の値に応じて`timerEnabled`)、し、「一時停止」または"Resume"に、ボタンの表示テキストを更新します。 この関数は、一時停止/再開 ボタンがクリックされたときに呼び出されます。

という名前の web サイトに新しいフォルダーを作成して開始`Scripts`です。 という名前の Scripts フォルダーに新しいファイルを次に、追加`TimerScript.js`の JScript ファイルの種類。


[![Scripts フォルダーに新しい JavaScript ファイルを追加します。](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**図 08**: 新しい JavaScript ファイルを追加、`Scripts`フォルダー ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![新しい JavaScript ファイルが、web サイトに追加されました](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**図 09**: 新しい JavaScript ファイルが、web サイトに追加されました ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image27.png))


次に、TimerScript.js ファイルに次のスクリプトを追加します。


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

今すぐにこのカスタムの JavaScript ファイルを登録する必要があります`ShowRandomProduct.aspx`です。 戻る`ShowRandomProduct.aspx`ScriptManagerProxy コントロールをページに追加する以外の設定しその`ID`に`MyManagerProxy`です。 カスタム JavaScript を登録するには、は、ファイルは、デザイナーで ScriptManagerProxy コントロールを選択し、[プロパティ] ウィンドウに移動します。 スクリプトのタイトルのプロパティの 1 つです。 このプロパティを選択するには、図 10 うにコレクション エディターが表示されます。 新しいスクリプト参照を含めるパス プロパティに、スクリプト ファイルへのパスを入力して [追加] をクリックします。`~/Scripts/TimerScript.js`です。


[![ScriptManagerProxy コントロールへのスクリプト参照を追加します。](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**図 10**: ScriptManagerProxy コントロールへのスクリプト参照の追加 ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image30.png))


マークアップを含むように更新宣言型のスクリプト参照 ScriptManagerProxy コントロールの追加後、`<Scripts>`を 1 つのコレクション`ScriptReference`エントリ、使用時のマークアップの次のスニペットに示します。


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

`ScriptReference`エントリに表示されるマークアップに JavaScript ファイルへの参照を含める ScriptManagerProxy に指示します。 つまり、ユーザー設定を登録することによってスクリプトに、ScriptManagerProxy、`ShowRandomProduct.aspx`ページの表示される出力が含まれています別`<script src="url"></script>`タグ:`<script src="Scripts/TimerScript.js" type="text/javascript"></script>`です。

呼び出せるようになりました、`ToggleTimer`関数で定義されている`TimerScript.js`でクライアント スクリプトから、`ShowRandomProduct.aspx`ページ。 UpdatePanel 内で次の HTML を追加します。


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

これには、「一時停止」というテキストとボタンが表示されます。 ときに、クリックすると、JavaScript 関数`ToggleTimer`が呼び出されると、ボタンとタイマー コントロールの id 値への参照を渡します (`ProductTimer`)。 取得するための構文に注意してください、`id`タイマー コントロールの値。 `<%=ProductTimer.ClientID%>` 値を出力、`ProductTimer`タイマー コントロールの`ClientID`プロパティです。 [*コンテンツ ページにコントロール ID の名前付け*](control-id-naming-in-content-pages-cs.md)サーバー側の相違点を説明したチュートリアル`ID`値と、結果として得られるクライアント側`id`値、およびどのように`ClientID`クライアント側を返します`id`です。

図 11 は、最初に、ブラウザーからアクセスされるときに、このページを示します。 タイマーが現在実行されているし、表示されている製品情報を 15 秒おきを更新します。 図 12 は、一時停止 ボタンがクリックしてされた後、画面を示しています。 一時停止 ボタンをクリックすると、タイマーを停止し、"Resume"にボタンのテキストを更新します。 製品情報が更新 (および 15 秒おきに更新を続けます) と、ユーザーが再開をクリックします。


[![タイマー コントロールの停止を一時停止 ボタンをクリックします。](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**図 11**: Timer コントロールを停止する [一時停止] ボタンをクリックして ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![タイマーを再起動する [再開] をクリックします。](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**図 12**: タイマーを再起動する [再開] をクリックして ([フルサイズのイメージを表示するをクリックして](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>まとめ

ASP.NET AJAX フレームワークを使用して AJAX 対応 web アプリケーションを構築するときに、すべての AJAX 対応 web ページに ScriptManager コントロールが含まれている必要があります。 このプロセスを容易に ScriptManager、すべてのコンテンツ ページに ScriptManager を追加する必要があるのではなく、マスター ページに追加できます。 手順 1 では、ScriptManager を手順 2. コンテンツ ページでの AJAX 機能の実装に検査中にマスター ページに追加する方法を示しました。

カスタム スクリプト、スクリプトが有効な Web サービスへの参照を追加する必要がありますまたは、コンテンツ ページに ScriptManagerProxy コントロールを追加し、構成のカスタマイズされた認証、承認、またはプロファイル サービスは特定のコンテンツ ページを、。カスタマイズがあります。 手順 3、ScriptManagerProxy を使用して、特定のコンテンツ ページで、カスタムの JavaScript ファイルを登録する方法を確認します。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET AJAX Framework](../../../../ajax/index.md)
- [ASP.NET AJAX のチュートリアル](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [ASP.NET AJAX のビデオ](../../../videos/aspnet-ajax/index.md)
- [Microsoft ASP.NET AJAX による対話型ユーザー インターフェイスの構築](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [ランダムにレコードの並べ替えに NEWID を使用します。](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [タイマー コントロールの使用](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](interacting-with-the-content-page-from-the-master-page-cs.md)
> [次へ](specifying-the-master-page-programmatically-cs.md)
