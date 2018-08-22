---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: ASP.NET MVC で Page Inspector の使用 |Microsoft Docs
author: rick-anderson
description: Visual Studio 2012 での Page Inspector は、統合ブラウザーの web 開発ツールです。 統合のブラウザーと Page Inspector i で任意の要素を選択してください.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: c465b0bac9af90a892d6e62a327ba36977d08d4a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827003"
---
<a name="using-page-inspector-in-aspnet-mvc"></a>ASP.NET MVC フォーム内での Page Inspector の使用
====================
Tim Ammann、

> Visual Studio 2012 での Page Inspector は、統合ブラウザーの web 開発ツールです。 統合されたブラウザーで任意の要素を選択し、Page Inspector は、要素のソースと CSS にすぐに強調表示します。 任意の MVC ビューを参照、すばやくの出力されるマークアップは、のソースを検索し、Visual Studio 環境内で直接ブラウザー ツールを使用します。
> 
> [ビデオを見る](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> このチュートリアルでは、検査モードを有効にして、すばやく見つけて web プロジェクト内でマークアップと CSS を編集する方法を示します。 チュートリアルは、MVC プロジェクトを使用してがの Page Inspector を使用することもできます。 [Web フォーム](https://go.microsoft.com/?linkid=9802001)と他の ASP.NET アプリケーション。
> 
> このチュートリアルでは、次のセクションがあります。
> 
> - [前提条件](#_1_prerequisites)
> - [Web アプリケーションを作成します。](#_2_creating_a)
> - [ビューを参照する Page Inspector を使用します。](#_3_using_page)
> - [検査モードを有効にします。](#_4_inspection_mode)
> - [Page Inspector を使用して、マークアップを変更する](#_5_using_page)
> - [検査モードと [HTML] ウィンドウ](#_6_inspection_mode)
> - [スタイルのウィンドウに CSS 変更のプレビュー](#_7_previewing_css)
> - [CSS 自動同期](#css_auto_sync)
> - [CSS カラー ピッカーを使用します。](#css_color_picker)
> - [Javascript の動的なページ要素のマッピング](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)または[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。

> [!NOTE]
> Page Inspector の最新バージョンを取得する[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) for .NET 2.0、Windows Azure SDK をインストールします。


Page Inspector には、Microsoft Web Developer Tools が付属しています。 最新バージョンは、1.3 です。 どのバージョンを確認するが、Visual Studio を実行して**Microsoft Visual Studio**から、**ヘルプ**メニュー。

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Web アプリケーションを作成します。

最初に、Page Inspector を使用する web アプリケーションを作成します。 Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**します。 左側で、展開**Visual c#** を選択します**Web**、し、 **ASP.NET MVC4 Web アプリケーション**します。

![新しい ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image2.png)

**[OK]** をクリックします。

**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**します。 まま**Razor**既定のビュー エンジンとして。

![新しい ASP.NET MVC プロジェクトのインターネット アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image4.png)

アプリケーションを開いた**ソース**ビュー。

![ソース ビューで新しい ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image6.png)

使用するアプリケーションがあるようになりましたことを確認および変更 Page Inspector を使用できます。

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>ビューを参照する Page Inspector を使用します。

Visual Studio 2012 で、プロジェクトで任意のビューを右できます**Page Inspector で表示**、Page Inspector は、ルートを図し、ページを表示するとします。

**ソリューション エクスプ ローラー**、展開、**ビュー**フォルダーをクリックし、**ホーム**フォルダー。 Index.cshtml ファイルを右クリックし、選択**Page Inspector で表示**します。

![Page Inspector 内での Index.cshtml を表示します。](using-page-inspector-in-aspnet-mvc/_static/image8.png)

既定では、Page Inspector は、Visual Studio 環境の左側にあるウィンドウとしてドッキングします。 場合は、他の場所にドッキングします。 または、ウィンドウをドッキング解除できます。 参照してください[方法: の整列し、固定 Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx)します。

Page Inspector ウィンドウの上部のペインでは、ブラウザーのウィンドウで、現在のページを示しています。 下部のウィンドウと共に使用して、ページのさまざまな側面を検査できますいくつかのタブの HTML マークアップでページを示します。 下のペインと似ています、 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)Internet Explorer でします。

![Page Inspector 内での ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image10.png)

このチュートリアルでは使用して、 **HTML**と**スタイル**をすばやく移動し、アプリケーションに変更を加えるタブ。

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>EnableInspection モード

Page Inspector を検査モードにする をクリックして、**検査**ボタンをクリックします。 検査モードで表示されたページの任意の部分にマウス ポインターを置くと、対応するソース マークアップまたはコードが強調表示されます。

![検査モードの切り替え](using-page-inspector-in-aspnet-mvc/_static/image12.png)

これで Page Inspector 内のページのさまざまな部分にマウスを移動します。 同様、大規模のプラス記号にマウス ポインターを変更し、下にある要素が強調表示されます。

![ポインターを合わせると div.content ラッパー](using-page-inspector-in-aspnet-mvc/_static/image14.png)

マウス ポインターを移動すると、Visual Studio には、ソース ファイル内の対応する、Razor 構文が強調表示されます。 別のソース ファイルから HTML 要素の場合、Visual Studio では、ファイルが自動的に開きます。

Page Inspector、内で、 **HTML**  タブには、Razor 構文から生成された HTML が表示されます。 マウス ポインターを移動すると、HTML 要素が強調表示されます。 **スタイル**タブには、要素に対する CSS 規則が表示されます。

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Page Inspector を使用して、マークアップを変更する

Page Inspector を使用して、その位置を判断できない可能性がありますマークアップを検索できます。 マークアップを変更し、結果として得られる変更を確認できます。

次のようにクリックします。**検査**、Page Inspector ウィンドウで、ページの一番下までスクロールします。

Page Inspector を開きますフッター領域にマウス ポインターを移動すると、 \_Layout.cshtml ファイルして、選択したレイアウト ページのセクションを強調表示します。 フッターには、ご覧のとおり、レイアウト ファイル、およびビュー自体で定義されています。

![[フッター]](using-page-inspector-in-aspnet-mvc/_static/image16.png)

著作権では、行にマウス ポインターを移動ようになりました<a id="a"></a>に注意してください。 \_Layout.cshtml ページで、対応する行が強調表示されます。

![強調表示されている著作権行のフッター](using-page-inspector-in-aspnet-mvc/_static/image18.png)

内の行の末尾にテキストを追加、 \_Layout.cshtml ファイル。

&lt;p&gt;&amp;コピーです。@DateTime.Now.Year -My ASP.NET MVC アプリケーション Rocks!&lt;/p&gt;

ここで、Ctrl + Alt + Enter を押すか、Page Inspector のブラウザー ウィンドウで結果を表示する更新バーをクリックします。

![My ASP.NET Application Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Index.cshtml で定義されているフッターがであることが判明ことを考えたかもしれません、 \_Layout.cshtml、および Page Inspector を検出します。

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>検査モードと [HTML] ウィンドウ

次に、簡単に見て、HTML ウィンドウとする要素をマップする方法があります。

クリックして**検査**Page Inspector を検査モードにします。

"Logohere、"と書かれたページの上部をクリックします。 詳細については、マウス ポインターを移動すると、ブラウザー ウィンドウの表示が不要になった変更の特定の要素を調べることができます。

今すぐにマウス ポインターを移動、 **HTML**ウィンドウ。 Page Inspector 内の要素の概要、マウス ポインターを移動すると、 **HTML**ウィンドウとブラウザーのウィンドウで、対応する要素が強調表示されます。

![HTML ウィンドウ](using-page-inspector-in-aspnet-mvc/_static/image22.png)

前に、Page Inspector が開くと、 \_Layout.cshtml ファイルの一時的なタブ。をクリックして、\_で、Layout.cshtml の一時的なタブと、対応するマークアップが強調表示されますが、&lt;ヘッダー&gt;のセクション。

![強調表示されたマークアップ](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>スタイルのウィンドウに CSS 変更のプレビュー

Page Inspector を使用して、次に、**スタイル**CSS の変更をプレビュー ウィンドウです。

クリックして**検査**Page Inspector を検査モードにします。

Page Inspector のブラウザーのウィンドウでまでの「ホーム ページ」セクションにマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。

![ポインターを合わせると div.content ラッパー](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Div.content ラッパー セクション内で 1 回クリックしにマウス ポインターを移動、**スタイル**ウィンドウ。 **スタイル**ウィンドウには、この要素に対する CSS 規則のすべてが表示されます。 検索 .featured の .content ラッパー クラス セレクターまで下にスクロールします。 背景色のプロパティのチェック ボックスをオフにようになりました。

![オフの背景色](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Page Inspector のブラウザー ウィンドウで変更がどのすぐにプレビューに注意してください。

もう一度チェック ボックスをオン、プロパティ値をダブルクリックして、赤に変更します。 変更がすぐには示しています。

![赤の背景色](using-page-inspector-in-aspnet-mvc/_static/image30.png)

**スタイル**シート自体のウィンドウでのスタイルに変更をコミットする前に変更をテストおよび CSS をプレビューすることが容易です。

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS 自動同期

> [!NOTE]
> この機能には、Page Inspector のバージョン 1.3 が必要です。


CSS 自動同期機能を使用すると、CSS ファイルを直接編集して、Page Inspector のブラウザー内ですぐに変更を確認できます。

クリックして**検査**Page Inspector を検査モードにします。

Page Inspector のブラウザーでまでの「ホーム ページ」セクションにマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。 この要素を選択するには、1 回クリックします。

**スタイル**ウィンドウには、この要素に対する CSS 規則のすべてが表示されます。 検索 .featured の .content ラッパー クラス セレクターまで下にスクロールします。 「.Featured .content ラッパー」をクリックします。 Page Inspector は、このスタイル (Site.css) を定義して対応する CSS スタイルを強調表示、CSS ファイルを開きます。

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

値を変更するようになりました`background-color`"red"にします。 Page Inspector のブラウザーで、変更がすぐに表示されます。

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>CSS カラー ピッカーを使用します。

Visual Studio 2012 CSS エディターを選択し、色の挿入を容易にするカラー ピッカーがあります。 カラー ピッカーは、標準カラー パレットが含まれています、標準の色の名前、ハッシュ コード、RGB、RGBA、HSL、および HSLA のカラーをサポートし、ドキュメントで最も最近使用した色のリストを保持します。

値を変更する前のセクションで、`background-color`プロパティ。 カラー ピッカーを起動するには、プロパティの名前と型の後にカーソルを置きます**#** または**rgb (** します。

![CSS カラー ピッカーのバー](using-page-inspector-in-aspnet-mvc/_static/image36.png)

選択し、または下方向キーを押しての色をクリックし、左と右方向キーを使用して、色を走査します。 色を閲覧するときに、対応する 16 進値のプレビューします。

![プレビューの背景色のプロパティ値](using-page-inspector-in-aspnet-mvc/_static/image38.png)

カラー バーが希望する正確な色を持っていない場合は、カラー ピッカーの pop ダウンを使用できます。 開くにはをカラー バーの右端にある二重シェブロンをクリックします。 または、キーボードの下向き矢印を 1 ~ 2 回押します。

![CSS カラー ピッカーの Pop ダウン](using-page-inspector-in-aspnet-mvc/_static/image40.png)

右側の垂直バーの色をクリックします。 メイン ウィンドウにその色のグラデーションが表示されます。 、Enter キーを押して、垂直バーから直接色を選択するか、さらに高い精度を選択するメイン ウィンドウの任意の時点をクリックします。

使用するコンピューターの画面で、色があるかどうか (必要はありません、Visual Studio ユーザー インターフェイスの内部にある)、右下にスポイト ツールを使用して、その値をキャプチャすることができます。

カラー ピッカーの下部にあるスライダーを移動することによって、色の不透明度を変更することもできます。 そう、RGBA 値を値の色ため、RGBA 形式は、不透明度を表すことができます。

色を選択した後、Enter キーを押し、背景色のエントリを完了するセミコロンを入力し、 *Site.css*ファイル。

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>ページのインスペクター更新バー

Page Inspector がすぐに変更を検出、 *Site.css*更新バーで、アラートが表示されます。

![更新バー](using-page-inspector-in-aspnet-mvc/_static/image42.png)

すべてのファイルを保存して Page Inspector のブラウザーを更新して、Ctrl + Alt + Enter キーを押します。 または [更新バー] をクリックします。 強調表示色の変更は、ブラウザーに表示されます。

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Javascript の動的なページ要素のマッピング

最新の web アプリケーションでページ内の要素は多くの場合、動的に生成 JavaScript で。 これらのページ要素に対応する static のマークアップ (HTML または Razor) がないことを意味します。

バージョン 1.3 では、Page Inspector は今すぐページを対応する JavaScript コードに動的に追加された項目をマップできます。 この機能を示すためには、使用して、 [Single Page Application (SPA) テンプレート](../../../single-page-application/overview/introduction/knockoutjs-template.md)します。

> [!NOTE]
> SPA テンプレートが必要です、 [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)を更新します。


Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**します。 左側で、展開**Visual c#** を選択します**Web**、し、 **ASP.NET MVC4 Web アプリケーション**します。 **[OK]** をクリックします。

**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、 **Single Page Application**します。

ソリューション エクスプ ローラーで、**ビュー**フォルダーをクリックし、**ホーム**フォルダー。 Index.cshtml ファイルを右クリックし、選択**Page Inspector で表示**します。

Page Inspector のブラウザーでは最初に表示されるは、ログイン ページです。 「サインアップ」をクリックし、ユーザー名とパスワードを作成します。 サインアップして、アプリケーション ログに記録するいくつかのサンプルの項目を含む、to do リストを作成します。

クリックして**検査**Page Inspector を検査モードにします。 Page Inspector のブラウザーでは、to do 項目のいずれかをクリックします。 青で強調表示されているのではなく、要素がハイライトされている"JS"では、オレンジ色で、要素名の横にあることを確認します。 これは、スクリプトによって、要素が動的に作成されたことを示します。

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

さらに、オレンジ色の下線が表示されます、**呼び出し履歴**タブ。これが示す、**呼び出し履歴**ウィンドウには、要素の詳細について。

をクリックして、**呼び出し履歴**タブ。**呼び出し履歴**ウィンドウには、要素を作成した JavaScript の呼び出しの呼び出し履歴が表示されます。 呼び出しを外部ライブラリなど、jQuery が折りたたまれている場合、アプリケーションのスクリプトへの呼び出しを簡単に確認できるように。

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

呼び出しを外部ライブラリを含む、完全なスタックを表示するには、「外部ライブラリ」というラベルの付いたノードを展開することができます。

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

呼び出し履歴内の項目をクリックすると、Visual Studio はコード ファイルが表示され、対応するスクリプトを強調表示されます。

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>関連項目

[Visual Studio を使用した ASP.NET MVC 4 の概要](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)(ASP.net web サイト)

[Page Inspector を導入](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/)(Channel 9 ビデオ)

[Page Inspector エラー メッセージ](https://go.microsoft.com/?linkid=9813062)(MSDN)
