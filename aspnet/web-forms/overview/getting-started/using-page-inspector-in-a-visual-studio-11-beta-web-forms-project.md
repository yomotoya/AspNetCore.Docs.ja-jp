---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Page Inspector を使用して ASP.NET Web での Visual Studio 2012 のフォームの |Microsoft ドキュメント
author: rick-anderson
description: Visual Studio 2012 用の Page Inspector は、統合ブラウザーで web 開発ツールです。 統合のブラウザーと Page Inspector でいずれかの要素を選択してください.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Page Inspector を使用して ASP.NET Web フォームでの Visual Studio 2012 用
====================
Tim Ammann によって

> Visual Studio 2012 用の Page Inspector は、統合ブラウザーで web 開発ツールです。 統合のブラウザー内で任意の要素を選択し、Page Inspector は、要素のソースと CSS に即座に強調表示します。 アプリケーションで任意のページを参照、すばやくのレンダリングされたマークアップのソースを検索し、右、Visual Studio 環境内でブラウザー ツールを使用します。
> 
> このチュートリアルの shwos 検査モードを有効にしてからすばやく検索して、web プロジェクト内のテキストと CSS 規則を編集する方法です。 チュートリアルは、Web フォーム アプリケーション プロジェクトを使用しますが、Web サイト プロジェクトの Page Inspector を使用することもでき、 [MVC](https://go.microsoft.com/?linkid=9802002)アプリケーションです。
> 
> このチュートリアルでは、次のセクションがあります。
> 
> [前提条件](#_1_prerequisites)
> 
> [Web アプリケーションを作成します。](#_2_creating_a)
> 
> [Page Inspector を使用して、アプリケーションの表示](#_3_using_page)
> 
> [検査モードを有効にします。](#_4_inspection_mode)
> 
> [Page Inspector を使用するマークアップを変更する](#_5_using_page)
> 
> [検査モードと [HTML] ウィンドウ](#_6_inspection_mode)
> 
> [スタイルのウィンドウに CSS 変更のプレビュー](#_7_previewing_css)
> 
> [CSS 自動同期](#css_auto_sync)
> 
> [CSS カラー ピッカーを使用します。](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)または[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。

> [!NOTE]
> Page Inspector の最新バージョンを取得する[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) for .NET 2.0、Azure SDK をインストールします。


Page Inspector には、Microsoft Web Developer Tools が付属しています。 最新バージョンでは、1.3 です。 どのバージョンを確認する、Visual Studio を実行して選択**に関する Microsoft クトリ**から、**ヘルプ**メニュー。

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Web アプリケーションを作成します。

最初に、Page Inspector を使用する web アプリケーションを作成します。 Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**です。 展開し、左側の**Visual c#** **Web**、し、 **ASP.NET Web フォーム アプリケーション**です。

![新しい Web フォーム アプリケーション](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

**[OK]** をクリックします。

アプリケーションが開かれた**ソース**ビュー。

![ソース ビューで新しい Web フォーム アプリケーション](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Page Inspector を使用するとを使用するアプリケーションがある場合は、これで、ことを確認し、それを変更します。

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Page Inspector を使用して、アプリケーションの表示

次に、Page Inspector を使用してアプリケーションを表示します。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックしを選択し、 **Page Inspector で表示**です。

![Page Inspector で表示](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

既定では、最初に、Page Inspector が起動するときにドッキングされているナロー ウィンドウとして、Visual Studio 環境の左側にあります。 左側にドッキングされていると、使いやすいまたは上部、下部、または右にツール領域の 1 つの固定幅には、設定のままにします。

![Page Inspector のドッキング位置](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

場合、Page Inspector ウィンドウのドッキングを解除することができます配置する Visual Studio の外部または 2 つ目のモニターでもある場合。 ただし、alt キーを押しながら TAB Page Inspector と Visual Studio の間の順序で Page Inspector] ウィンドウがドッキングされていないときに移動**ツール** &gt; **オプション** &gt; **環境** &gt; **タブとウィンドウ**、し、[**タブも**チェック ボックスをオフ**フローティング ツール ウィンドウの上に常に連動して、メイン ウィンドウ**:

![Visual Studio と Page Inspector の非ドッキング ウィンドウの間には、ALT + TAB をフローティング ツール windows チェック ボックスをオフします。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Page Inspector ウィンドウの上部ペインには、ブラウザーのウィンドウで、現在のページを示します。 下のペインは、左側の HTML マークアップでページを表示し、いくつかのタブを使用する右側は、ページのさまざまな側面を調査します。 下のペインがに似ていますが、 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)Internet Explorer でします。 (ただし、開発者ツールとは異なり使用できます Visual Studio 内で右 Page Inspector。)

![Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

このチュートリアルでは、Page Inspector のブラウザー ウィンドウを使用して、 **HTML**と**スタイル**に迅速に役立つタブ移動し、アプリケーションを変更します。

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>検査モードを有効にします。

次に、Page Inspector の検査モードの動作方法が表示されます。 [Page Inspector] ウィンドウ、**検査**ボタンをクリックします。

![要素を検査します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

検査モードの動作を表示するには、Page Inspector のブラウザー ウィンドウ内のページのさまざまな部分にマウスを移動します。 マウス ポインターが大きなのプラス記号を変更し、下にある要素が強調表示されます。

![ポインターを置いた div.content ラッパー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

マウス ポインターを移動するよう注意してください。

- 内容は、**ソース** ページで選択した要素に対応するマークアップを表示する変更を表示します。 関連するマークアップが強調表示されます。 ソースが別のファイル内にある場合は、そのはで開かれるファイル ソース ビューに関連するマークアップを強調表示されます。

- 表示されるマークアップ、 **HTML** Page Inspector 内でのタブは、ページで選択した要素に対応するも変更します。 **HTML**  タブで、関連するマークアップをについて説明します。

- **スタイル** タブが現在の選択に関連する CSS 規則を示します。

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Page Inspector を使用するマークアップを変更する

今すぐ検索し、マークアップまたは場所がすぐにわできない可能性がありますテキストに変更を Page Inspector を使用する方法が表示されます。

検査モードで Page Inspector を配置し、ホーム ページの下部までスクロールします。

フッターの領域を入力するとすぐに Page Inspector を開きます、 *Site.Master*内でレイアウト ファイル**ソース**もう一方の右側に一時 タブの表示がタブし、マスターのセクションを強調表示したページ選択しました。 これにより表示 Page Inspector が方法を見つけるし、コンテンツ ページ上に表示を最初に開いたものの別のファイルから実際に行います。

![検査モードでページ フッターを強調表示します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Page Inspector のブラウザーのウィンドウで、著作権では、行にマウス ポインターを移動<a id="a"></a>に注意してください。

*Site.Master*  ページで、対応する行が強調表示されます。

![ページ フッターの著作権行が強調表示されます。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

内の行の末尾にいくつかのテキストを追加、 *Site.Master*ファイル。

&lt;p&gt;&amp;コピーです。&lt;%: DateTime.Now.Year %&gt; -マイ ASP.NET アプリケーションのものです&lt;。/p&gt;

ここで、Ctrl + Alt + Enter を押すか、Page Inspector のブラウザーのウィンドウで結果を表示する更新バー をクリックします。

![My ASP.NET アプリケーションのものです。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

フッターにあったことを考えたかもしれません、 *Default.aspx*  ページで、それがマスター レイアウト ページのことがわかりましたし、Page Inspector では、これを検出するためです。

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>検査モードと [HTML] ウィンドウ

次に、簡単に見て、HTML ウィンドウとどのようにするための要素をマップするがあります。

Page Inspector を検査モードで配置します。

![要素を検査します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

「ここにロゴ」と表示されているページの上部をクリックします。 詳細については、マウス ポインターを移動すると、ブラウザー ウィンドウに表示が不要になった変更の特定の要素を調べることができます。

これで、マウス ポインターを移動する、 **HTML**ウィンドウです。 Page Inspector 内の要素の概要、マウス ポインターを移動すると、 **HTML**ウィンドウとブラウザーのウィンドウで、対応する要素が強調表示されます。

![HTML ウィンドウ](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Page Inspector を開く前に、 *Site.Master*ファイルが一時的なタブにします。Site.Master タブをクリックし、対応するマークアップが強調表示されます、&lt;ヘッダー&gt;セクション。

![強調表示されたマークアップ](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>スタイルのウィンドウに CSS 変更のプレビュー

Page Inspector を使用する方法を表示が次に、**スタイル**CSS に変更をプレビューするウィンドウです。

クリックして、**検査**Page Inspector を検査モードで配置するボタンをクリックします。

Page Inspector のブラウザーのウィンドウでマウス ポインターを移動まで「ホーム ページ」セクションの上、 **div.content ラッパー**ラベルが表示されます。

![要素の上にマウス](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Div.content ラッパー セクション内に 1 回クリックしにマウス ポインターを移動、**スタイル**ウィンドウです。 .Featured <model>.content ラッパー クラス セレクターの下をオフにし、背景色のプロパティのチェック ボックスを選択します。

![オフの背景色](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

どの変更をプレビュー即座に Page Inspector のブラウザー ウィンドウに注目してください。

もう一度チェック ボックスをオン、プロパティ値をダブルクリックしに変更して`red`です。 変更をすぐに示しています。

![赤の背景色](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**スタイル**スタイルに変更をコミットする前に変更が簡単にテストし、CSS のプレビューにウィンドウがシート自体です。

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS 自動同期

> [!NOTE]
> この機能には、Page Inspector のバージョン 1.3 が必要です。


CSS 自動同期機能を使用すると、CSS ファイルを直接編集し、Page Inspector のブラウザーですぐに変更を確認できます。

をクリックして**検査**Page Inspector を検査モードにします。

Page Inspector のブラウザーの「ホーム ページ」セクションまでマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。 この要素を選択するには、1 回クリックします。

**Syles**すべてこの要素に対する CSS 規則のウィンドウが表示されます。 検索 .featured <model>.content ラッパー クラス セレクターまで下にスクロールします。 「.Featured <model>.content ラッパー」をクリックします。 Page Inspector は、このスタイル (Site.css) を定義して対応する CSS スタイルを強調表示する CSS ファイルを開きます。

![CSS ファイル](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

値を変更するようになりました`background-color`"red"にします。 変更は、Page Inspector のブラウザーにすぐに表示されます。

![Page Inspector のブラウザー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>CSS カラー ピッカーを使用します。

次に、Page Inspector を使用してすばやく発見し、CSS の既定のアプリケーションで強調表示されたテキストを変更する方法を学びます。 この例では、青色の枠と同様に、別の色を変更しないと判断しました。

クリックして、**検査**ボタンをクリックします。

![要素を検査します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Page Inspector のブラウザーのウィンドウで、マウス ポインター上に移動、強調表示された「ビデオ、チュートリアル、およびサンプル」テキストできるように、CSS「マーク」ラベルが表示されます。

![マーク要素の上にマウス](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

テキストを選択 をクリックします。 下に、対応する CSS マーク セレクターが表示される、**スタイル**ウィンドウです。

![スタイルのウィンドウで、セレクターのマークを付ける](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

マーク セレクターをクリックします。 開き、 *Site.css* web アプリケーションのファイルです。 Site.css タブをクリックし、対応する CSS セレクターが強調表示されます。

![スタイル シートで、セレクターのマークを付ける](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

選択し、背景色のプロパティでは、行を削除します。

新しい色を選択する、新しい Visual Studio 2012 CSS カラー ピッカーを使用、**マーク**背景色のプロパティです。

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Visual Studio 2012 CSS カラー ピッカーを使用します。

Visual Studio 2012 で CSS エディターには、カラー ピッカーを容易に選択し、色を挿入するものがあります。 単純なカラー バーとさらに細かく制御を提供する「pop ダウン」ピッカーいます。

カラー ピッカーは、標準色パレットが含まれています、標準的な色の名前、ハッシュ コード、RGB、RGBA、HSL、および HSLA の色をサポートし、ドキュメントで最も最近使用した色の一覧を保持します。

背景色のプロパティである、1 行には、"bc"を入力し、下向きの矢印を 1 回押します。

[背景色] のようにハイフンで区切ったプロパティ内の各単語の最初の文字を入力すると、IntelliSense には、一致するプロパティのみを表示するための一覧がフィルター処理します。

![Intellisense フィルターの値](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

コロンを入力します。 実行すると、全体の背景色のプロパティ名が挿入されます。 型**#** または**rgb (**、カラー ピッカーのバーが表示されます。

![CSS カラー ピッカー バー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

カラー ピッカーのバーの動作を確認するには、マウス ポインターをその色をクリックしてまたは下方向キーを押すし、左と右方向キーを使用して、色を走査します。 アクセスすると、色、背景色のプロパティの対応する値には次のプレビューが行われます。

![プレビューの背景色プロパティ値](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

この時点では、値と、CSS の入力を補完する、セミコロン (;) を選択するには Enter キーを押して可能性があります。 ここでは、に移動、次のセクション、カラー ピッカー pop ダウンのしくみを認識できるようにします。

#### <a name="using-the-color-picker-pop-down"></a>カラー ピッカーの Pop ダウンを使用します。

カラー バーには、探している正確な色が割り当てられていない、ときに、カラー ピッカー pop ダウンを行うこともできます。

開くにはをカラー バーの右端にある二重山かっこをクリックしてまたはキーボードの下矢印を 1、2 回押します。

![CSS カラー ピッカーの Pop ダウン](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

右側の垂直バーから色をクリックします。 これにより、その色のグラデーションがメイン ウィンドウで表示します。 、Enter キーを押して、垂直バーから直接色を選択するか、任意の時点より高い精度でを選択するメイン ウィンドウでをクリックします。

使用するコンピューターの画面で、色があるかどうか (がない、Visual Studio ユーザー インターフェイスの中にある)、右下にあるスポイト ツールを使用してその値をキャプチャすることができます。

また、色の不透明度をカラー ピッカーの下部にあるスライダーを移動することによって変更することもできます。 そう、RGBA 形式は、不透明度を表すことができるためにカラー RGBA 値に値を変更します。

色を選択したら、Enter キーを押しますし、セミコロンの背景色の入力を補完する、 *Site.css*ファイル。

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>ページのインスペクター更新バー

Page Inspector への変更をすぐに検出、 *Site.css*ファイル (または、アプリケーション内のファイルに) し、更新バーにアラートを表示します。

![更新バー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

すべてのファイルを保存して Page Inspector のブラウザーを更新して、Ctrl + Alt + Enter キーを押します。 または [更新バー] をクリックします。 強調表示色の変更は、ブラウザーに表示されます。

![強調表示色の変更](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Visual Studio 環境内で Page Inspector のブラウザーから直接を簡単に更新されるに注意してください。 外部ブラウザーではなく Page Inspector を使用すると、web アプリケーションを開発すると、エディターで状態を維持できます。

## <a name="see-also"></a>参照

[Page Inspector を導入](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector)(Channel 9 ビデオ)

[Page Inspector エラー メッセージ](https://go.microsoft.com/?linkid=9813062)(MSDN)
