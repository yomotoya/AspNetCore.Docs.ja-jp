---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Visual Studio 2012 の ASP.NET Web フォームの Page Inspector の使用 |Microsoft Docs
author: rick-anderson
description: Visual Studio 2012 の Page Inspector は、統合ブラウザーの web 開発ツールです。 統合のブラウザーと Page Inspector で任意の要素を選択してください.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: d2c377f8466f8f324b75ce60860aa00c11bc0ffe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838076"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>ASP.NET Web フォームでの Visual Studio 2012 の Page Inspector の使用
====================
Tim Ammann、

> Visual Studio 2012 の Page Inspector は、統合ブラウザーの web 開発ツールです。 統合されたブラウザーで任意の要素を選択し、Page Inspector は、要素のソースと CSS にすぐに強調表示します。 アプリケーションで任意のページを参照、すばやくの出力されるマークアップは、のソースを検索し、Visual Studio 環境内で直接ブラウザー ツールを使用します。
> 
> このチュートリアルの shwos を検査モードを有効にして、すばやく見つけて、web プロジェクト内のテキストと CSS 規則を編集する方法。 チュートリアルは、Web フォーム アプリケーション プロジェクトを使用して、Web サイト プロジェクトの Page Inspector を使用することもできますが、 [MVC](https://go.microsoft.com/?linkid=9802002)アプリケーション。
> 
> このチュートリアルでは、次のセクションがあります。
> 
> [前提条件](#_1_prerequisites)
> 
> [Web アプリケーションを作成します。](#_2_creating_a)
> 
> [Page Inspector を使用して、アプリケーションを表示するには](#_3_using_page)
> 
> [検査モードを有効にします。](#_4_inspection_mode)
> 
> [Page Inspector を使用して、マークアップを変更する](#_5_using_page)
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

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11)または[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。

> [!NOTE]
> Page Inspector の最新バージョンを取得する[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) for .NET 2.0、Azure SDK をインストールします。


Page Inspector には、Microsoft Web Developer Tools が付属しています。 最新バージョンは、1.3 です。 どのバージョンを確認するが、Visual Studio を実行して**Microsoft Visual Studio**から、**ヘルプ**メニュー。

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Web アプリケーションを作成します。

最初に、Page Inspector を使用する web アプリケーションを作成します。 Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**します。 左側で、展開**Visual c#** を選択します**Web**、し、 **ASP.NET Web フォーム アプリケーション**します。

![新しい Web フォーム アプリケーション](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

**[OK]** をクリックします。

アプリケーションを開いた**ソース**ビュー。

![ソース ビューで、新しい Web フォーム アプリケーション](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

使用するアプリケーションがあるようになりましたことを確認および変更 Page Inspector を使用できます。

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Page Inspector を使用して、アプリケーションを表示するには

次に、Page Inspector を使用してアプリケーションを表示します。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、、 **Page Inspector で表示**します。

![Page Inspector で表示](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

既定では、起動時、Page Inspector を最初に、ドッキングした幅の狭いウィンドウとして、Visual Studio 環境の左側にあります。 左側にドッキングされ、使いやすいまたはツールの領域のいずれかで、top、bottom、または右に固定幅に設定をそのままにします。

![Page Inspector のドッキング位置](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Page Inspector ウィンドウのドッキングを解除する場合を配置できますが Visual Studio の外部または 2 つ目のモニター上であってもある場合。 ただし、alt キーを押しながら TAB Page Inspector と Visual Studio の間の順序で Page Inspector] ウィンドウがドッキングされていないときに移動**ツール** &gt; **オプション** &gt; **環境** &gt; **タブや Windows**、および [**タブも**チェック ボックスをオフのチェック ボックスと呼ばれる**の上にフローティング ツール ウィンドウを常に、メイン ウィンドウ**:

![Visual Studio とドッキングの Page Inspector ウィンドウの間には、ALT + TAB をフローティング ツール windows チェック ボックスをオフします。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Page Inspector ウィンドウの上部のペインでは、ブラウザーのウィンドウで、現在のページを示しています。 いくつかのタブを使用する右側のページのさまざまな側面を検査して、下部のウィンドウは、左側の HTML マークアップでページを示しています。 下のペインと似ています、 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)Internet Explorer でします。 (ただし、開発者ツールを使用する Visual Studio 内での Page Inspector できます。)

![Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

このチュートリアルでは、Page Inspector のブラウザー ウィンドウを使用して、 **HTML**と**スタイル**に迅速に役立つタブ移動し、アプリケーションに変更を加えます。

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>検査モードを有効にします。

次に、Page Inspector の検査モードの動作方法が表示されます。 Page Inspector ウィンドウで、**検査**ボタンをクリックします。

![要素を検査します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

検査モードの動作を表示するには、Page Inspector のブラウザー ウィンドウ内のページのさまざまな部分にマウスを移動します。 同様、大規模のプラス記号にマウス ポインターを変更し、下にある要素が強調表示されます。

![ポインターを合わせると div.content ラッパー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

マウス ポインターを移動すると、注意してください。

- コンテンツでは、**ソース**ページで選択した要素に対応するマークアップを表示する変更を表示します。 関連するマークアップが強調表示されます。 ソースは、別のファイルでは、そのファイルが強調表示されている関連するマークアップをソース ビューで開きます。

- 表示されるマークアップ、 **HTML** Page Inspector 内でのタブは、ページで選択した要素に対応するも変更します。 **HTML**  タブで、関連するマークアップが記載されています。

- **スタイル** タブが現在の選択に関連する CSS 規則を示します。

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Page Inspector を使用して、マークアップを変更する

今すぐマークアップまたはテキストの場所がすぐに明らかなできない可能性がありますを変更するを見つけ、Page Inspector を使用する方法が表示されます。

検査モードで Page inspector を使用して、ホーム ページの一番下までスクロールします。

Page Inspector を開きますフッター領域を入力するとすぐ、 *Site.Master*レイアウト ファイルが**ソース**もう一方の右側に一時的なタブのビューを選択し、タブ、マスターのセクションを強調表示したページ選択しました。 これでは Page Inspector が検索し、役に立つ最初に開かれた 1 つ別のファイルからは実際にページにコンテンツを表示する方法。

![検査モードでフッターを強調表示します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Page Inspector のブラウザーのウィンドウでマウス ポインターを移動、著作権で回線を経由した<a id="a"></a>に注意してください。

*Site.Master*  ページで、対応する行が強調表示されます。

![強調表示されている著作権行のフッター](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

内の行の末尾にテキストを追加、 *Site.Master*ファイル。

&lt;p&gt;&amp;コピーです。&lt;%: DateTime.Now.Year %&gt; -マイ ASP.NET アプリケーション Rocks!&lt;/p&gt;

ここで、Ctrl + Alt + Enter を押すか、Page Inspector のブラウザー ウィンドウで結果を表示する更新バーをクリックします。

![My ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

フッターをしたことを考えたかもしれません、 *Default.aspx*  ページが、これが、マスター レイアウト ページのことがわかりました、Page Inspector が見つかりました。

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>検査モードと [HTML] ウィンドウ

次に、簡単に見て、HTML ウィンドウとする要素をマップする方法があります。

Page Inspector を検査モードで配置します。

![要素を検査します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

"Your logo here"と書かれたページの上部をクリックします。 詳細については、マウス ポインターを移動すると、ブラウザー ウィンドウの表示が不要になった変更の特定の要素を調べることができます。

今すぐにマウス ポインターを移動、 **HTML**ウィンドウ。 Page Inspector 内の要素の概要、マウス ポインターを移動すると、 **HTML**ウィンドウとブラウザーのウィンドウで、対応する要素が強調表示されます。

![HTML ウィンドウ](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

前に、Page Inspector が開くと、 *Site.Master*のために一時的なタブ内のファイル。Site.Master タブをクリックし、対応するマークアップが強調表示されます、&lt;ヘッダー&gt;セクション。

![強調表示されたマークアップ](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>スタイルのウィンドウに CSS 変更のプレビュー

Page Inspector の使用方法を紹介は次に、**スタイル**CSS の変更をプレビュー ウィンドウです。

をクリックして、**検査**Page Inspector を検査モードにボタンをクリックします。

Page Inspector のブラウザーのウィンドウでまでの「ホーム ページ」セクションにマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。

![ポインターを合わせると要素](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Div.content ラッパー セクション内で 1 回クリックしにマウス ポインターを移動、**スタイル**ウィンドウ。 .Featured .content ラッパー クラス セレクターをオフにして、背景色のプロパティのチェック ボックスをオンします。

![オフの背景色](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Page Inspector のブラウザー ウィンドウで変更がどのすぐにプレビューに注意してください。

もう一度チェック ボックスをオンしをダブルクリックしてプロパティの値に変更`red`します。 変更がすぐには示しています。

![赤の背景色](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**スタイル**シート自体のウィンドウでのスタイルに変更をコミットする前に変更をテストおよび CSS をプレビューすることが容易です。

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS 自動同期

> [!NOTE]
> この機能には、Page Inspector のバージョン 1.3 が必要です。


CSS 自動同期機能を使用すると、CSS ファイルを直接編集して、Page Inspector のブラウザー内ですぐに変更を確認できます。

クリックして**検査**Page Inspector を検査モードにします。

Page Inspector のブラウザーでまでの「ホーム ページ」セクションにマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。 この要素を選択するには、1 回クリックします。

**スタイル**ウィンドウには、この要素に対する CSS 規則のすべてが表示されます。 検索 .featured の .content ラッパー クラス セレクターまで下にスクロールします。 「.Featured .content ラッパー」をクリックします。 Page Inspector は、このスタイル (Site.css) を定義して対応する CSS スタイルを強調表示、CSS ファイルを開きます。

![CSS ファイル](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

値を変更するようになりました`background-color`"red"にします。 Page Inspector のブラウザーで、変更がすぐに表示されます。

![Page Inspector のブラウザー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>CSS カラー ピッカーを使用します。

次に、Page Inspector を使用してすばやく検索して強調表示されたテキスト、既定のアプリケーションでの CSS を変更する方法を学びます。 この例では、青の強調表示など、別の色に変更しないと判断しました。

をクリックして、**検査**ボタンをクリックします。

![要素を検査します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Page Inspector のブラウザーのウィンドウでマウス ポインターを移動を強調表示された「ビデオ、チュートリアル、およびサンプル」テキスト、CSS はラベルを「マーク」のように表示されます。

![ポインターを合わせると、マーク要素](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

テキストを選択 をクリックします。 下部にある対応する CSS マーク セレクターが表示されます、**スタイル**ウィンドウ。

![スタイルのウィンドウで、セレクターをマークします。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

マーク セレクターをクリックします。 開き、 *Site.css* web アプリケーションのファイル。 Site.css タブをクリックし、セレクターに対応する CSS が強調表示されます。

![スタイル シート内のマーク セレクター](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

選択し、背景色のプロパティでは、行を削除します。

新しい色を選択する新しい Visual Studio 2012 CSS カラー ピッカーを使用、**マーク**背景色のプロパティ。

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Visual Studio 2012 CSS カラー ピッカーを使用します。

Visual Studio 2012 CSS エディターを選択し、色の挿入を容易にするカラー ピッカーがあります。 単純なカラー バーと細かい制御を提供する「pop がダウンした」ピッカーを備えています。

カラー ピッカーは、標準カラー パレットが含まれています、標準の色の名前、ハッシュ コード、RGB、RGBA、HSL、および HSLA のカラーをサポートし、ドキュメントで最も最近使用した色のリストを保持します。

背景色のプロパティである、1 行には、"bc"を入力し、下向きの矢印を 1 回押します。

[背景色] のようなハイフンで区切られたプロパティに各単語の最初の文字を入力すると、IntelliSense には、一致するプロパティのみを表示するためのリストが絞り込ま。

![Intellisense フィルター処理された値](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

コロンを入力します。 この場合、完全な背景色のプロパティ名が挿入されます。 型**#** または**rgb (**、カラー ピッカーのバーが表示されます。

![CSS カラー ピッカーのバー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

カラー ピッカーのバーの動作を確認するには、マウス ポインターをその色をクリックしてまたは下方向キーを押してし、左と右方向キーを使用して、色を走査します。 色を閲覧するときに背景色のプロパティの対応する値がプレビュー表示します。

![プレビューの背景色のプロパティ値](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

この時点では、値と CSS の入力を完了するにはセミコロン (;) を選択してキーを押しますでした。 ここでは、移動次のセクションにカラー ピッカーの pop ダウンの動作を確認することができます。

#### <a name="using-the-color-picker-pop-down"></a>カラー ピッカーの Pop ダウンを使用します。

カラー バーには、探している正確な色が割り当てられていない、ときに、pop のカラー ピッカーを使用できます。

開くにはをカラー バーの右端にある二重シェブロンをクリックします。 または、キーボードの下向き矢印を 1 ~ 2 回押します。

![CSS カラー ピッカーの Pop ダウン](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

右側の垂直バーの色をクリックします。 メイン ウィンドウにその色のグラデーションが表示されます。 、Enter キーを押して、垂直バーから直接色を選択するか、さらに高い精度を選択するメイン ウィンドウの任意の時点をクリックします。

使用するコンピューターの画面で、色があるかどうか (必要はありません、Visual Studio ユーザー インターフェイスの内部にある)、右下にスポイト ツールを使用して、その値をキャプチャすることができます。

カラー ピッカーの下部にあるスライダーを移動することによって、色の不透明度を変更することもできます。 そう、RGBA 形式は、不透明度を表すことができますので、カラー RGBA 値に値を変更します。

色を選択した後、Enter キーを押し、背景色のエントリを完了するセミコロンを入力し、 *Site.css*ファイル。

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>ページのインスペクター更新バー

Page Inspector がすぐに変更を検出、 *Site.css*ファイル (または、アプリケーション内のファイルに) 更新バーにアラートを表示します。

![更新バー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

すべてのファイルを保存して Page Inspector のブラウザーを更新して、Ctrl + Alt + Enter キーを押します。 または [更新バー] をクリックします。 強調表示色の変更は、ブラウザーで表示されます。

![強調表示色の変更](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Visual Studio 環境内での Page Inspector のブラウザーから直接が簡単に更新されたことに注意してください。 Page Inspector を使用して、外部のブラウザーではなく、web アプリケーションを開発するときに、エディターで維持することができます。

## <a name="see-also"></a>関連項目

[Page Inspector を導入](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector)(Channel 9 ビデオ)

[Page Inspector エラー メッセージ](https://go.microsoft.com/?linkid=9813062)(MSDN)
