---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: "ASP.NET MVC での Page Inspector の使用 |Microsoft ドキュメント"
author: rick-anderson
description: "Visual Studio 2012 での Page Inspector は、統合されたブラウザーと web 開発ツールです。 統合のブラウザーと Page Inspector i でいずれかの要素を選択してください."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a>ASP.NET MVC フォーム内での Page Inspector の使用
====================
Tim Ammann によって

> Visual Studio 2012 での Page Inspector は、統合されたブラウザーと web 開発ツールです。 統合のブラウザー内で任意の要素を選択し、Page Inspector は、要素のソースと CSS に即座に強調表示します。 任意の MVC ビューの参照、すばやくのレンダリングされたマークアップのソースを検索し、右、Visual Studio 環境内でブラウザー ツールを使用します。
> 
> [ビデオを見る](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> このチュートリアルでは、検査モードを有効にしてからすばやく検索して、web プロジェクト内のマークアップと CSS を編集する方法を示します。 チュートリアルは、MVC プロジェクトを使用しますの Page Inspector を使用することもできます。 [Web フォーム](https://go.microsoft.com/?linkid=9802001)およびその他の ASP.NET アプリケーション。
> 
> このチュートリアルでは、次のセクションがあります。
> 
> - [前提条件](#_1_prerequisites)
> - [Web アプリケーションを作成します。](#_2_creating_a)
> - [ビューへの参照に Page Inspector を使用します。](#_3_using_page)
> - [検査モードを有効にします。](#_4_inspection_mode)
> - [Page Inspector を使用するマークアップを変更する](#_5_using_page)
> - [検査モードと [HTML] ウィンドウ](#_6_inspection_mode)
> - [スタイルのウィンドウに CSS 変更のプレビュー](#_7_previewing_css)
> - [CSS 自動同期](#css_auto_sync)
> - [CSS カラー ピッカーを使用します。](#css_color_picker)
> - [JavaScript への動的なページ要素のマッピング](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)または[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。

> [!NOTE]
> Page Inspector の最新バージョンを取得する[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) for .NET 2.0、Windows Azure SDK をインストールします。


Page Inspector には、Microsoft Web Developer Tools が付属しています。 最新バージョンでは、1.3 です。 どのバージョンを確認する、Visual Studio を実行して選択**に関する Microsoft クトリ**から、**ヘルプ**メニュー。

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Web アプリケーションを作成します。

最初に、Page Inspector を使用する web アプリケーションを作成します。 Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**です。 展開し、左側の**Visual c#** **Web**、し、 **ASP.NET MVC4 Web アプリケーション**です。

![新しい ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image2.png)

**[OK]**をクリックします。

**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**です。 ままにして**Razor**既定のビュー エンジンとして。

![新しい ASP.NET MVC プロジェクトのインターネット アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image4.png)

アプリケーションが開かれた**ソース**ビュー。

![ソース ビューで新しい ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Page Inspector を使用するとを使用するアプリケーションがある場合は、これで、ことを確認し、それを変更します。

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>ビューへの参照に Page Inspector を使用します。

Visual Studio 2012 での任意のビューを右プロジェクトで、 **Page Inspector で表示**、Page Inspector は、ルートを図し、ページを表示するとします。

**ソリューション エクスプ ローラー**、展開、**ビュー**フォルダーし、**ホーム**フォルダーです。 Index.cshtml ファイルを右クリックし、選択**Page Inspector で表示**です。

![Page Inspector 内でビュー Index.cshtml](using-page-inspector-in-aspnet-mvc/_static/image8.png)

既定では、Page Inspector がドッキングされるウィンドウとして、Visual Studio 環境の左側にあります。 場合は、他の場所にドッキングしたり、ウィンドウのドッキングを解除できます。 参照してください[する方法: を整列およびドッキング Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx)です。

Page Inspector ウィンドウの上部ペインには、ブラウザーのウィンドウで、現在のページを示します。 下のペインは、ページのさまざまな側面を調査することのできるいくつかのタブと、HTML マークアップ内のページを示します。 下のペインがに似ていますが、 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)Internet Explorer でします。

![Page Inspector 内での ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image10.png)

このチュートリアルでは使用して、 **HTML**と**スタイル**のタブにすばやく移動し、アプリケーションを変更します。

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>EnableInspection モード

Page Inspector を検査モードにする をクリックして、**検査**ボタンをクリックします。 検査モードで表示されたページの任意の部分にマウス ポインターを置いた場合、対応するソース マークアップまたはコードが強調表示されます。

![検査モードを切り替える](using-page-inspector-in-aspnet-mvc/_static/image12.png)

これで Page Inspector 内のページのさまざまな部分にマウスを移動します。 マウス ポインターが大きなのプラス記号を変更し、下にある要素が強調表示されます。

![ポインターを置いた div.content ラッパー](using-page-inspector-in-aspnet-mvc/_static/image14.png)

マウス ポインターを移動すると、Visual Studio には、ソース ファイル内の対応する、Razor 構文が強調表示されます。 別のソース ファイルから HTML 要素の場合、Visual Studio は自動的にファイルを開きます。

Page Inspector 内で、 **HTML**タブは、Razor 構文から生成された HTML を表示します。 マウス ポインターを移動すると、HTML 要素が強調表示されます。 **スタイル** タブは、要素に対する CSS 規則を示しています。

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Page Inspector を使用するマークアップを変更する

Page Inspector では、その位置を判断できない可能性がありますマークアップを検索できます。 マークアップを変更し、結果として得られる変更を確認できます。

これを見るには、をクリックして**検査**し、Page Inspector ウィンドウ内のページの下部までスクロールします。

Page Inspector が開き、フッター領域にマウス ポインターを移動すると、 \_Layout.cshtml ファイルし、選択したレイアウト ページのセクションを強調表示します。 表示されるフッターはレイアウト ファイルおよびビュー自体で定義されています。

![[フッター]](using-page-inspector-in-aspnet-mvc/_static/image16.png)

今すぐ著作権では、行にマウス ポインターを移動<a id="a"></a>に注意してください。 \_Layout.cshtml ページで、対応する行が強調表示されます。

![ページ フッターの著作権行が強調表示されます。](using-page-inspector-in-aspnet-mvc/_static/image18.png)

内の行の末尾にいくつかのテキストを追加、 \_Layout.cshtml ファイル。

&lt;p&gt;&amp;コピーです。@DateTime.Now.Year -My ASP.NET MVC アプリケーションの!&lt;/p&gt;

ここで、Ctrl + Alt + Enter を押すか、Page Inspector のブラウザーのウィンドウで結果を表示する更新バー をクリックします。

![My ASP.NET アプリケーションのものです。](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Index.cshtml で定義されているフッターが内にあるになっていることを考えたかもしれません、 \_Layout.cshtml、および Page Inspector を検出します。

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>検査モードと [HTML] ウィンドウ

次に、簡単に見て、HTML ウィンドウとどのようにするための要素をマップするがあります。

をクリックして**検査**Page Inspector を検査モードにします。

"Logohere、"と表示されているページの上部をクリックします。 詳細については、マウス ポインターを移動すると、ブラウザー ウィンドウに表示が不要になった変更の特定の要素を調べることができます。

これで、マウス ポインターを移動する、 **HTML**ウィンドウです。 Page Inspector 内の要素の概要、マウス ポインターを移動すると、 **HTML**ウィンドウとブラウザーのウィンドウで、対応する要素が強調表示されます。

![HTML ウィンドウ](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Page Inspector を開く前に、 \_Layout.cshtml ファイルが一時的なタブにします。クリックして、\_で、Layout.cshtml 一時 タブ、および対応するマークアップが強調表示されますが、&lt;ヘッダー&gt;のセクション。

![強調表示されたマークアップ](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>スタイルのウィンドウに CSS 変更のプレビュー

Page Inspector を使用して、次に、**スタイル**CSS に変更をプレビューするウィンドウです。

をクリックして**検査**Page Inspector を検査モードにします。

Page Inspector のブラウザーのウィンドウでマウス ポインターを移動まで「ホーム ページ」セクションの上、 **div.content ラッパー**ラベルが表示されます。

![ポインターを置いた div.content ラッパー](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Div.content ラッパー セクション内に 1 回クリックしにマウス ポインターを移動、**スタイル**ウィンドウです。 **Syles**すべてこの要素に対する CSS 規則のウィンドウが表示されます。 検索 .featured <model>.content ラッパー クラス セレクターまで下にスクロールします。 背景色のプロパティのチェック ボックスをオフようになりました。

![オフの背景色](using-page-inspector-in-aspnet-mvc/_static/image28.png)

どの変更をプレビュー即座に Page Inspector のブラウザー ウィンドウに注目してください。

もう一度チェック ボックスをオン、プロパティ値をダブルクリックして、赤に変更します。 変更をすぐに示しています。

![赤の背景色](using-page-inspector-in-aspnet-mvc/_static/image30.png)

**スタイル**スタイルに変更をコミットする前に変更が簡単にテストし、CSS のプレビューにウィンドウがシート自体です。

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS 自動同期

> [!NOTE]
> この機能には、Page Inspector のバージョン 1.3 が必要です。


CSS 自動同期機能を使用すると、CSS ファイルを直接編集し、Page Inspector のブラウザーですぐに変更を確認できます。

をクリックして**検査**Page Inspector を検査モードにします。

Page Inspector のブラウザーの「ホーム ページ」セクションまでマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。 この要素を選択するには、1 回クリックします。

**Syles**すべてこの要素に対する CSS 規則のウィンドウが表示されます。 検索 .featured <model>.content ラッパー クラス セレクターまで下にスクロールします。 「.Featured <model>.content ラッパー」をクリックします。 Page Inspector は、このスタイル (Site.css) を定義して対応する CSS スタイルを強調表示する CSS ファイルを開きます。

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

値を変更するようになりました`background-color`"red"にします。 変更は、Page Inspector のブラウザーにすぐに表示されます。

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>CSS カラー ピッカーを使用します。

Visual Studio 2012 で CSS エディターには、カラー ピッカーを容易に選択し、色を挿入するものがあります。 カラー ピッカーは、標準色パレットが含まれています、標準的な色の名前、ハッシュ コード、RGB、RGBA、HSL、および HSLA の色をサポートし、ドキュメントで最も最近使用した色の一覧を保持します。

値を変更して、前のセクションで、`background-color`プロパティです。 カラー ピッカーを起動するには、プロパティの名前と型の後にカーソルを置きます **#** または**rgb (**です。

![CSS カラー ピッカー バー](using-page-inspector-in-aspnet-mvc/_static/image36.png)

色を選択して、または下方向キーを押してクリックし、左と右方向キーを使用して、色を走査します。 アクセスすると、色、対応する 16 進値には次のプレビューが行われます。

![プレビューの背景色プロパティ値](using-page-inspector-in-aspnet-mvc/_static/image38.png)

カラー バーには、目的の正確な色が割り当てられていない、カラー ピッカーの pop ダウンを使用することができます。 開くにはをカラー バーの右端にある二重山かっこをクリックしてまたはキーボードの下矢印を 1、2 回押します。

![CSS カラー ピッカーの Pop ダウン](using-page-inspector-in-aspnet-mvc/_static/image40.png)

右側の垂直バーから色をクリックします。 これにより、その色のグラデーションがメイン ウィンドウで表示します。 、Enter キーを押して、垂直バーから直接色を選択するか、任意の時点より高い精度でを選択するメイン ウィンドウでをクリックします。

使用するコンピューターの画面で、色があるかどうか (がない、Visual Studio ユーザー インターフェイスの中にある)、右下にあるスポイト ツールを使用してその値をキャプチャすることができます。

また、色の不透明度をカラー ピッカーの下部にあるスライダーを移動することによって変更することもできます。 そうカラー RGBA 値に値を変更、RGBA 形式は、不透明度を表すことができるためです。

色を選択したら、Enter キーを押しますし、セミコロンの背景色の入力を補完する、 *Site.css*ファイル。

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>ページのインスペクター更新バー

Page Inspector への変更をすぐに検出、 *Site.css*更新バーで、アラートが表示されます。

![更新バー](using-page-inspector-in-aspnet-mvc/_static/image42.png)

すべてのファイルを保存して Page Inspector のブラウザーを更新して、Ctrl + Alt + Enter キーを押します。 または [更新バー] をクリックします。 強調表示色の変更は、ブラウザーに表示されます。

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>JavaScript への動的なページ要素のマッピング

最新の web アプリケーションで、ページ内の要素は、多くの場合、動的に生成 JavaScript で。 つまり、これらのページ要素に対応する static のマークアップ (HTML または Razor) はありません。

バージョン 1.3 では、Page Inspector はページを対応する JavaScript コードに動的に追加された項目を今すぐマップできます。 この機能を示すためを使用、[単一ページ アプリケーション (SPA) テンプレート](../../../single-page-application/overview/introduction/knockoutjs-template.md)です。

> [!NOTE]
> SPA テンプレートが必要です、 [ASP.NET および Web ツール 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)を更新します。


Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**です。 展開し、左側の**Visual c#** **Web**、し、 **ASP.NET MVC4 Web アプリケーション**です。 **[OK]**をクリックします。

**新しい ASP.NET MVC 4 プロジェクト**ダイアログで、 **Single Page Application**です。

ソリューション エクスプ ローラーで、展開、**ビュー**フォルダーし、**ホーム**フォルダーです。 Index.cshtml ファイルを右クリックし、選択**Page Inspector で表示**です。

Page Inspector のブラウザーでは最初に表示されるには、ログイン ページを示します。 「サインアップ」 をクリックし、ユーザー名とパスワードを作成します。 サインアップした後、アプリケーション ログに記録して、to do リスト項目を作成するいくつかのサンプルです。

をクリックして**検査**Page Inspector を検査モードにします。 Page Inspector のブラウザーでは、作業項目のいずれかをクリックします。 青で強調表示されているの代わりに、要素、強調表示される"JS"では、オレンジ色で、要素名の横にあることを確認します。 これは、スクリプトによって、要素が動的に作成されたことを示します。

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

さらに、オレンジ色の下線が表示されます、**呼び出し履歴**タブです。これが示す、**呼び出し履歴**ウィンドウの詳細については、要素があります。

をクリックして、**呼び出し履歴**タブです。**呼び出し履歴**ウィンドウ要素を作成した JavaScript の呼び出しの呼び出し履歴を示しています。 呼び出す外部ライブラリ jQuery が折りたたまれている場合など、アプリケーションのスクリプトへの呼び出しを簡単に認識できるようにします。

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

外部のライブラリへの呼び出しなど、完全なスタックを表示するには、「外部ライブラリ」というラベルの付いたノードを展開することができます。

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

呼び出し履歴内の項目をクリックすると、Visual Studio はコード ファイルが表示され、対応するスクリプトが強調表示されます。

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>参照

[Visual Studio での ASP.NET MVC 4 に出だし](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)(ASP.net web サイト)

[Page Inspector を導入](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/)(Channel 9 ビデオ)

[Page Inspector エラー メッセージ](https://go.microsoft.com/?linkid=9813062)(MSDN)
