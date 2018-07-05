---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: ASP.NET と Visual Studio 2012 での Web 開発における新 |Microsoft Docs
author: rick-anderson
description: 新しいバージョンの Visual Studio には、さまざまな Web テクノロジを使用する場合、エクスペリエンスとパフォーマンスの向上に重点を置いての機能強化が導入されています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d037ccb62693a07ab8e2640b2d930ec7093b35da
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375886"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>ASP.NET と Visual Studio 2012 での Web 開発における新します。
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> 新しいバージョンの Visual Studio には、多数の Web テクノロジを使用する場合、エクスペリエンスとパフォーマンスの向上に重点を置いた機能が導入されています。 CSS、JavaScript と HTML の visual Studio エディターは、IntelliSense や自動インデントなど、最も、オンデマンドでコード補助機能の多くを完全に刷新されました。 パフォーマンスに関してバンドルと縮小は統合されました ページを簡単に削減する組み込み機能の読み込み時間と。
> 
> Visual Studio では、最新の web サイト テクノロジと連携することができます。 クロスブラウザー CSS3 のスニペットを使用すると、新しい HTML5 の要素と機能の活用しながら、サイトがクライアント プラットフォームに関係なく動作を確認します。
> 
> 書き込みと JavaScript コードのプロファイリングは、このバージョンの Visual Studio で簡単になります。 IntelliSense リストでは、XML ドキュメントとナビゲーション機能の統合は現在、JavaScript コードにはできます。 指先ひとつで、JavaScript のカタログがあるようになりました。 さらに、各自のスクリプトで ECMAScript5 のコンプライアンスを確認し、早い段階で構文エラーを検出できます。
> 
> 最後に、しかし大事なこのバージョンの Visual Studio は、組み込みのバンドルと縮小を実装します。 スタイル シート、スクリプト ファイルをパックし、圧縮、サイトが高速に実行できるようにします。
> 
> このラボでは、拡張機能とソース フォルダーにサンプル Web アプリケーションに軽微な変更を適用することで以前に説明する新機能について説明します。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)します。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズ オン ラボでは、学習する方法。

- CSS エディターの新機能と機能強化を使用します。
- HTML エディターで新しい機能と機能強化を使用します。
- 新しい機能と機能強化を使用して、JavaScript エディター
- 構成および使用のバンドルと縮小

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (セットアップ スクリプト - Windows 8 および Windows Server 2008 R2 に既にインストールされている) に使用
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home)または HTML5 準拠のブラウザー

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボでは、次の演習が含まれています。

1. [CSS エディターの新機能がどのような演習 1 です。](#Exercise1)
2. [手順 2: 新機能については、HTML エディターです。](#Exercise2)
3. [手順 3: 新機能については、JavaScript エディターです。](#Exercise3)
4. [手順 4: バンドルと縮小](#Exercise4)

この演習の所要時間を推定: **60 分**します。

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>CSS エディターの新機能がどのような演習 1 です。

Web 開発者は、CSS の編集に関連する問題の多くをよく知って必要があります。 CSS スタイルの最も大きな問題の 1 つは、ブラウザーの間の互換性です。 多くの場合、スタイルを適用すると、サイト、わかります表示が異なること別のブラウザーまたはデバイスで開く場合行われます。 そのため、その視覚的な問題、最後に 1 つのブラウザーで動作することを行ったときに壊れている、他のユーザーに認識する修正にかなりの時間を費やす可能性があります。

Visual Studio には、開発者は、アクセス、動作、および CSS スタイル シートを効果的に整理に役立つ機能が含まれています。 この演習での 有効な組織や edition の新機能とブラウザー間で互換性の CSS3 のコード スニペットを満たします。

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>タスク 1 - エディターの新機能

このタスクでは、CSS エディターの新しい機能を検出します。 この新しいエディターを使用すると、新しいスマート インデント、改善されたコードのコメントおよび強化された IntelliSense リストを活用して生産性を向上できます。

1. 開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。
2. ソリューション エクスプ ローラーで開く、 **Site.css**下にあるファイル、**スタイル**フォルダー。 必ず、**テキスト エディター**ツールは、ツールバーに表示されます。 そのためには次のように選択します。、**ビュー** | **ツールバー**メニュー オプション、およびチェック、**テキスト エディター**オプション。 以降、この新しいバージョンでは、いることを確認は、**コメント**ボタン (![コメント ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) および**コメントを解除します**ボタン (![のコメントを解除します-ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png))、CSS エディターのも有効にします。

    ![エディターと CSS ツールを有効にする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "エディターと CSS ツールを有効にします。")

    *エディターと CSS ツールを有効にします。*
3. コードをスクロールし、任意の CSS クラスの定義を選択します。 をクリックして、**コメント**(![コメント ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) ボタンを選択した行をコメントします。 をクリックし、**コメントを解除します**(![のコメントを解除ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png))、変更を元に戻すボタンをクリックします。
4. をクリックして、**折りたたむ**(![折りたたむ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) と**展開**(![展開](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) ボタンがテキストの左余白にあります。 クリーナー ビューを使用しないスタイルを隠すようになりましたことができることに注意してください。

    ![CSS クラスの折りたたみ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "折りたたみの CSS クラス")

    *CSS クラスの折りたたみ*
5. スマート インデント機能が有効になっていることを確認します。 選択、**ツール** | **オプション**メニュー オプション、および選択し、**テキスト エディター** | **CSS**  | **書式**画面の左側のウィンドウでページ。 チェック、**階層インデント**オプション。

    ![階層インデントを有効にする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "階層インデントを有効にします。")

    *階層インデントを有効にします。*
6. メイン クラスの定義 (.main) を検索し、div 要素にスタイルを追加します。 コード配置の概要、親クラスを検索するユーザーで自動的に表示されます。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![CSS で階層的な配置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "CSS 内の階層の配置")

    *CSS で階層的な配置*
7. 内で **.main div**クラスの末尾にカーソルを置いて**border: 0 px;** キーを押します **」と入力**IntelliSense の一覧を表示します。 入力を開始**上部**に入力すると、一覧のフィルター方法に注意してください。 リストが含まれている要素が表示されます**上部**という単語の任意の部分で (Visual Studio の以前のバージョンでは、リストは項目でフィルター処理を*開始*用語で)。

    ![CSS で IntelliSense の機能強化](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "CSS で IntelliSense の機能強化")

    *CSS で IntelliSense の機能強化*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>タスク 2 - カラー ピッカー

このタスクでは、Visual Studio の IntelliSense 統合され、新しい CSS カラー ピッカーを検出します。

1. **Site.css、** ヘッダーのクラス定義 (.header) を見つけて横にカーソルを置き**背景色**属性は、間、 &quot;:&quot;と&quot; #&quot;文字コードの行に**します。**

    ![カーソルを検索する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "カーソルを検索します。")

    *カーソルを検索します。*
2. 削除、**コロン**(:)、カラー ピッカーを表示するには、もう一度記述します。 最初の色が表示されますが、サイトの最も一般的な色に注意してください。 白のカラーをクリックした場合、HTML のカラー コード (#fff) は、スタイル シートでは、現在のカラー コードを置き換えます。

    ![カラー ピッカー](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "カラー ピッカー")

    *カラー ピッカー*
3. キーを押して、**展開**(![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) 色のグラデーションを表示し、別の色を選択するグラデーションのカーソルをドラッグするには、カラー ピッカーのボタンをクリックします。 その後、をクリックして、**スポイト**ボタンをクリックし、画面から色を選択します。 カーソルを移動するときに、背景色の値が動的に変更することに注意してください。

    ![グラデーション ピッカー](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "グラデーション ピッカー")

    *グラデーション ピッカー*
4. **不透明度**スライダー、セレクターを不透明度を減らすバーの中央に移動します。 背景色の値は、RGBA にそのスケールを今すぐ変更に注意してください。

    ![カラー ピッカーの不透明度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "不透明度のカラー ピッカー")

    *不透明度のカラー ピッカー*

    > [!NOTE]
    > CSS3 で RGBA (赤、緑、青、アルファ) の色の定義では、1 つの項目の色の不透明度値を定義することができます。 異なり**不透明度 -** のような CSS 属性**-** RGBA 色は、最新のブラウザーと互換性があります。

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>タスク 3 - CSS 互換性のあるコード スニペット

このタスクでは、web サイトで一部の機能を実装するためにクロス ブラウザーの互換性のある CSS3 のスニペットを使用する方法を学習します。

1. **Site.css**を探し、ファイル、**ヘッダー** CSS クラスの定義 (.header) と、以下のカーソルを置き、  **/\*境界の半径\*/** プレース ホルダーを新しいスニペットを追加します。 キーを押して **」と入力**、IntelliSense の一覧を表示し**radius**一覧をフィルター処理します。 選択、**境界の半径**ダブル クリックで、一覧からオプションし、キーを押します、**タブ**スニペットを挿入するキー。 Radius のサイズを入力し、ピクセルとキーを押して**Enter**します。 たとえば、入力**15px**します。

    スニペットによって追加される CSS3 属性は、角の丸い境界線などの Mozilla ブラウザーの WebKit ベース HTML5 準拠のほとんどのブラウザーにレンダリングされます。

    ![境界の半径のスニペットを使用して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "境界の半径のスニペットを使用して")

    *境界の半径のスニペットを使用してください。*
2. 同じ**境界線**スニペットでは、ページのスタイル (.page)。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. キーを押して**F5**ソリューションを実行します。 各ページようになりましたが丸い境界線に注意してください。

    ![角が丸く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "角を丸く")

    *角が丸い*
4. ブラウザーを閉じて、Visual Studio に戻ります。
5. 開く、 **Custom.css**下にあるファイル、**スタイル**フォルダー内でカーソルを置き**div.images ul li img**クラスの定義。
6. Enter キーを押して IntelliSense リストを表示型**ボックス シャドウ**キーを押すと、**タブ**クラス定義内の既定のシャドウ コード スニペットを挿入するには、2 回のキー。 シャドウの値に設定**10px 10px 5 px #888**します。 次に、入力**境界の半径**とコード スニペットを挿入します。 型**15px**半径のサイズとキーを押してを設定する**ENTER**します。

    ![シャドウを伴って角を丸く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "シャドウを伴って角を丸く")

    *影付きの角が丸い*

    > [!NOTE]
    > この時点で、対応するプレフィックス (moz、webkit、o) Mozilla をサポートするために、Webkit (Chrome、Safari、Konkeror) ブラウザーとシャドウ属性が挿入されます。
7. 新しいクラスを作成**div.images ul li img:hover**下、 **div.images ul li img**クラスの定義と、角かっこ内でカーソルを置き**します。**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. 型**変換**キーを押すと、**タブ**変換スニペットを挿入するために 2 回のキー。 次に、入力**rotate(-15deg)** イメージが置かれているときに、回転の角度の値を変更します。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. キーを押して**F5**ソリューションを実行し、CSS3 のページを参照します。 イメージがコーナーが丸くし、影のボックスに注意してください。 イメージの上にマウスを移動し、回転させることを確認します。

    ![スニペットのイメージの回転の変換](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "イメージの回転変換スニペット")

    *スニペットのイメージの回転の変換します。*

    > [!NOTE]
    > Internet Explorer 10 を使用しているし、影を表示することはできませんの場合は、ドキュメント モードが IE10 標準に設定されていることを確認してください。 キーを押して**F12**を Internet Explorer 開発者ツールを開き、をクリックして**ドキュメント モード**IE10 標準に変更します。

    ![についてのご](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>手順 2: 新機能については、HTML エディターです。

Visual Studio では、HTML エディターの強化には。 このバージョンに含まれる拡張機能の一部は、HTML ドキュメント、HTML5 のスニペット、HTML の開始と終了タグが一致する、および HTML 検証のスマート インデントです。 この演習では、web サイトのマークアップで作業するときに、これらの変更が、自在に操作できることを改善する方法がわかります。

CSS エディターのように、HTML エディターも強化されました。 これらの機能強化のほとんどは、Web 開発者の作業を簡単にする小規模なものです。 さらにコード スニペットは、html5、スマート インデントは、編集、および HTML ドキュメントの DOCTYPE を対象とする検証がこれらの機能強化の一部である場合の開始および終了タグに一致するなど。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>タスク 1 - 強化された DOCTYPE の検証

HTML エディターでは、マスター ページで、定義としても、今すぐ、ページの DOCTYPE を確認する機能があります。 によって、ページの DOCTYPE、HTML エディターは適切な一連の規則の検証し、DOCTYPE 要素を検討して IntelliSense の一覧をフィルター処理されます。

このタスクでは、HTML エディターの動作を適宜変更する方法を表示するページの DOCTYPE が変わります。

1. これを開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。
2. 開く、 **Site.Master**ページ。
3. 検証のツールバーには、ターゲット スキーマに注意してください。 HTML エディターの動作 (検証、IntelliSense など) は、Doctype の選択に合わせて正しく変更されます。

    ![Doctype を使用して、HTML ソースの編集 ツールバーで](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "HTML ソースの編集 ツールバーで使用して Doctype")

    *HTML ソースの編集 ツールバーに Doctype を使用します。*
4. HTML 4.01 にターゲット スキーマを変更します。

    ![HTML ソースの編集 ツールバーに Doctype を変更する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "HTML ソースの編集 ツールバーで Doctype の変更")

    *HTML ソースの編集 ツールバーで Doctype の変更*
5. カーソルを置き、**本文**要素、および HTML5 の要素の名前を入力する (たとえば、**ビデオ**)。 要素が、IntelliSense リストで使用できないことに注意してください。

    ![HTML5 の要素が表示されていない](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 の要素が表示されていません。")

    *HTML5 の要素が表示されていません。*
6. 検証のツールバーで、DOCTYPE の選択のターゲット スキーマへの変更を元に戻す: ドロップダウン リストから XHTML5 します。

    ![Doctype を使用して、HTML ソースの編集 ツールバーで](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "HTML ソースの編集 ツールバーで使用して Doctype")

    *Doctype の HTML ソースの編集 ツールバーをリセットします。*
7. 下にカーソルを置き、**本文**HTML5 の要素をもう一度入力する要素と (たとえば、**ビデオ**)。 HTML5 の要素は、IntelliSense リストで使用できるようになりましたことに注意してください。

    ![HTML5 の要素が表示される](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 の要素が一覧表示すること")

    *HTML5 の要素が一覧表示すること*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>タスク 2 - 開始/終了タグの自動更新

Visual Studio は、今すぐ開く、または終了タグが相互に一致する編集している要素の HTML を更新します。 HTML タグを編集するときに、この新しい機能では、生産性が向上します。

1. **Default.aspx**ページで、追加、 **H3**タイトル (たとえば、Visual Studio 2012 Rocks!) を持つ要素。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. 変更、 **H3**タグと型**H2**または**H1 します。**

    終了タグが自動的に更新することを確認します。 開始タグを更新するそれに応じてすぎるを表示する終了タグを変更することもできます。

    ![終了タグの自動更新](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "終了タグの自動更新")

    *終了タグの自動更新*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>タスク 3 - 新しい HTML5 コード スニペット

Visual Studio には、いくつかの HTML5 のコード スニペットが含まれています。 このタスクでは、これらのスニペットの一部を使用します。

1. という名前の新しいフォルダーを追加**オーディオ**web サイト フォルダーのルートにします。 Windows エクスプ ローラーを開きにオーディオ ファイルをコピー、**オーディオ**のフォルダー、 **WhatsNewASPNET.sln**ソリューション。
2. **Default.aspx**  ページで、Web11 Rocks でカーソルを置いてください。 ヘッダー。 型**オーディオ**TAB キーを押します。

    新しい HTML エディターには、HTML5 のコンテンツのコード スニペットが含まれています。 適切な DOCTYPE 定義を使用して、HTML5 のスニペットを有効にしてください。

    ![HTML5 のコード スニペットの挿入](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "HTML5 コード スニペットの挿入")

    *HTML5 のコード スニペットの挿入*
3. 既存のオーディオ ファイルをポイントするオーディオ ソースを更新します。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > オーディオ ファイルをソリューションに追加する必要があります。
4. キーを押して**F5**サイトを実行し、オーディオを再生します。

    ![オーディオ コントロールを実行している](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "オーディオ コントロールの実行")

    *オーディオ コントロールの実行*

    > [!NOTE]
    > ビデオや図など、Visual Studio に含まれる複数のスニペットを試すこともできます。
5. ここで、ページの一部のコントロールを挿入してみてください。 挿入しようとするなど、 **GridView**コントロールが入力する代わりに**&lt;合わせる、** の入力を開始 **&lt;GV**します。 IntelliSense の一覧を示していますが、 **: asp GridView**コントロール。

    HTML エディターでの IntelliSense は、タイトルの大文字と小文字の検索と部分一致する (要素を取得するすべての用語を含む) に現在提供します。

    ![IntelliSense のリストを使用した GridView を挿入する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "IntelliSense リストを使用した GridView を挿入します。")

    *IntelliSense のリストを使用した GridView を挿入します。*

    入力した場合**&lt;グリッド**という用語に一致するすべての項目が表示されますが、Visual Studio をお勧めしますが、 **gridview**コントロール。

    ![IntelliSense リスト部分に一致すると、GridView を挿入する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "IntelliSense リスト部分に一致すると、GridView を挿入します。")

    *IntelliSense リスト部分に一致すると、GridView を挿入します。*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>タスク 4 - HTML エディターのスマート タグ

別の機能強化、HTML エディターでは、スマート タグ機能です。 スマート タグを簡単にコントロールごとに一般的なや反復的な開発タスクを実行します。 この機能が既に使用できる HTML デザイナーで、HTML エディターでありません。

1. 開いている**Site.Master**探し、 **asp: メニュー**要素。 カーソルを置き、開始タグと - 要素の下部に表示される小さいグリフをクリックして、[スマート タスク] メニューを開くことに注意してください。 メニュー コントロールに関連するいくつかのタスクにすばやくアクセスすることがわかります。

    ![メニュー コントロールのタスクのスマート](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "メニュー コントロールのタスクのスマート")

    *メニュー コントロールのスマート タスク*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>タスク 5 - スマート インデント

読み取り可能なコードを保持する入れ子になった要素をインデント HTML でのベスト プラクティスの 1 つです。 Visual Studio 2012 で、コードを記述するときに、エディターがその要素を自動的にインデントすることがわかります。

> [!NOTE]
> Visual Studio の以前のバージョンではスマート インデントされた使用可能な HTML エディターではなく、XML エディターにします。


1. スマート インデントに HTML エディターでインデント構成が設定されていることを確認します。 そのためには次のように選択します、**ツール |。オプション**メニュー オプションと選択し、**テキスト エディター |HTML |タブ**画面の左側のウィンドウでページ。 スマート インデント オプションを選択します。

    ![HTML エディター設定](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML エディターの設定")

    *HTML エディターの設定*
2. **Default.aspx**  ページで、オーディオの要素の下のすべてのコンテンツを削除します。
3. 開始の末尾にカーソルを置き**オーディオ**要素とヒット**ENTER**します。

    新しいカーソルの位置、インデントの追加レベルであることに注意してください。

    ![スマート インデント HTML エディターで](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "スマート インデント、HTML エディター")

    *HTML エディターでのスマート インデント*
4. オーディオ コンテンツを削除するか、閉じるタグを復元**Default.aspx**せず、変更を保存します。

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>タスク 6: ユーザー コントロールへの抽出

抽出する関数の場合、コードの一部など、Visual Studio に含まれる、リファクタリング ツールを改善し、既存のコードのリファクタリングを容易にするための優れた機能です。 ASP.NET ページの対応するユーザー コントロールへの HTML コードの抽出となります。 手動で行うと、新しいユーザー コントロールの作成、ユーザー コントロールに、コードのセクションを移動、ユーザー コントロールのタグ プレフィックスを登録および、最後に、ページ上のユーザー コントロールをインスタンス化するように、いくつかの手順は必要があります。 ここで、新しい*ユーザー コントロールに展開*ツールが自動的にこれらの手順を実行します。

このタスクでは、選択したコードから新しいユーザー コントロールを生成するのにコンテキストの操作をユーザー コントロールに新しい展開を使用します。

1. **Default.aspx** ] ページで、[、 **H2**と**オーディオ**要素。
2. 右クリックし、選択**ユーザー コントロールに展開**します。

    ![ユーザー コントロール メニュー オプションに抽出](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "ユーザー コントロール メニュー オプションに抽出します。")

    *ユーザー コントロール メニュー オプションに抽出します。*
3. 新しいユーザー コントロールの名前を入力します。 たとえば、 **Jukebox.ascx**、 をクリックし、 **OK**。

    ![抽出されたユーザー コントロールを保存](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "抽出されたユーザー コントロールを保存しています")

    *抽出されたユーザー コントロールを保存しています*
4. ユーザー コントロールに、選択したコードの抽出、選択したコードの元の場所は、新しいユーザー コントロールのインスタンスに置き換えられたことに注意してください。

    ![新しいユーザー コントロールを使用するページが自動的に更新](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "ページは、新しいユーザー コントロールを使用して自動的に更新")

    *新しいユーザー コントロールを使用するページが自動的に更新*
5. キーを押して**F5**ページを実行し、コントロールが動作することを確認します。

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>手順 3: 新機能については、JavaScript エディターです。

書き込みまたは JavaScript コードの編集が簡単な作業では、特に、アプリケーションの起動時にサイズの増加や長いファイルを扱うと数百の関数の自分で検索する場合。 通常、スクリプトの開発者は、コードの読みやすさを維持し、ファイルをナビゲートする特別な作業を行う必要があります。 スクリプトのナビゲーションで jQuery などの JavaScript ライブラリのコードの長さのため自体課題となっています。

Visual Studio には、アクセス可能で、構成されたコードのモードにする約束を JavaScript エディターが更新されます。 JavaScript エディターで c# または VB のエディターで既に存在していた多くの Visual Studio の機能が実装されるようになりました。 定義へ移動、自動インデント、ドキュメント、および記述するときに検証します。 更新された IntelliSense リストでは、指先ひとつで JavaScript 関数のカタログをことができます。

この演習では、JavaScript エディターの機能強化と新機能の一部を説明します。 サンプル ファイルを参照し、それぞれが、JavaScript プログラミングの Visual Studio 2012 内でより効率的なように新しい特性を検出します。

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>タスク 1 - JavaScript エディターの新機能

このタスクでは、コードを整理し、優れたユーザー エクスペリエンスを集中新しい JavaScript エディター機能をいくつか紹介します。

1. これを開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。
2. キーを押して**F5**アプリケーションを実行し、ナビゲーション バーで JavaScript リンクをクリックします。 ページを更新、いくつかの時間とチェックする方法はカウンターがインクリメントします。

    ![ページ カウンター](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "ページ カウンター")

    *ページ カウンター*
3. ブラウザーを閉じて、Visual Studio に戻ります。
4. 開く、 **JavaScript.aspx**ページし、検索、 **&lt;スクリプト&gt;** ブロック (下記参照)。

    次のコードを格納する HTML5 ローカル ストレージを使用して、 *pageLoadCount*ページが現在のユーザーがアクセスした回数の合計を格納する変数です。 ローカル ストレージは、標準の HTML5 で導入されたクライアント側のキー値データベースです。 データは、ユーザーのブラウザー内で、ローカル コンピューターに保存されます。

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > DOCTYPE 次の手順に進む前に XHTML5 に設定されていることを確認します。
5. コードを編集して、JavaScript 用 IntelliSense がローカル ストレージは、その内部メソッドなどの HTML5 機能が含まれることに注意してください。

    ![JavaScript での HTML5 の JavaScript 機能](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript での HTML5 の JavaScript 機能")

    *JavaScript での HTML5 の JavaScript 機能*
6. 任意の始め角かっこをクリックして (**{**) から、スクリプト コードし、角かっこが強調表示されていることに注意してください。

    ![角かっこが強調表示されている](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "角かっこが強調表示されます")

    *角かっこが強調表示されます。*
7. 関数をコメント解除します**testAutoAlign()** (3 つの行を選択し、使用することができます**CTRL** + **K**;**CTRL** + **U**) し、関数コード内にカーソルを探します。 2 番目の行を追加して enter キーを押します。 これで、コードが通知**揃え**と**自動インデント**します。

    ![JavaScript コードは自動配置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript コードは自動配置")

    *JavaScript コードは自動配置*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>タスク 2 - JavaScript の検証

このタスクでは、標準の ECMAScript5 の新しい JavaScript 検証を検出します。 この機能はサイトに展開する前にスクリプトの問題を防止しながら、準拠の JavaScript コードを記述するのに役立ちます。

> [!NOTE]
> Visual Studio 2010 では、Visual Studio 2012 ECMAScript5 のコンプライアンスを提供しますが、ECMAStript3 のコンプライアンスが実装されています。


1. 開いている**ECMA5script5.js**の下にある、 **Scripts\custom**プロジェクト フォルダーです。 今すぐ ECMAScript5 の標準の検証をテストします。

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    チェック アウトすることができます、 &quot; **strict 使用** &quot; ECMAScript5 できると、ファイルの最初の行方向**厳格モード**します。 このモードでは、過去のエディションからあいまいさを明確化し、オブジェクトのプロパティの getter と setter、JSON、およびより完全なリフレクション ライブラリのサポートなど、いくつかの新機能を追加する言語のサブセットで構成されます。
2. 開く、**エラー一覧**開いていない場合 (**ビュー**メニュー |**エラー一覧**)。 通知、**関数**宣言の下線が引かれます。 これは、ため、ECMA5 で標準的な関数内にネストできません言語構造体です。 エラーが発生する次の一覧に警告の詳細が表示されます。

    ![JavaScript の検証エラー メッセージ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript の検証エラー メッセージ")

    *JavaScript の検証エラー メッセージ*
3. コメント アウト、  **&quot;strict 使用&quot;** 方向とエラーが非表示になりますが、警告のままにすることがわかります。
4. ファイルの最後の行のような任意の文字列を書き込む**&quot;テスト&quot;** (文字列であることを示す引用符を含む)。 IntelliSense の一覧を表示し、選択する文字列の横にあるピリオドを書き込み、**トリミング**オプション。

    ECMAScript5 の標準で文字列値および変数もメソッドを持つ文字列定義、トリミング、大文字、検索し、置換と同様にします。

    ![JavaScript での IntelliSense リスト](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "javascript IntelliSense の一覧")

    *Javascript IntelliSense の一覧*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>タスク 3 - JavaScript の XML ドキュメント

このタスクでは、JavaScript での XML ドキュメントの Visual Studio の機能を説明します。 JavaScript IntelliSense の一覧では、各関数の XML ドキュメントに表示されますが表示されます。 さらに、JavaScript でのナビゲーション機能が検出されます。

1. 開いている**XMLDoc.js**にあるファイル**スクリプト/カスタム**プロジェクト フォルダーです。 このファイルには、JavaScript 関数の各 XML ドキュメントが含まれています。

    ![JavaScript XML ドキュメントが IntelliSense に統合された](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML ドキュメントに IntelliSense が統合されています。")

    *IntelliSense に統合された JavaScript XML ドキュメント*
2. 下**追加**関数**XMLDoc.js**ファイル、という名前の新しい関数を作成**テスト**します。
3. **テスト**関数を呼び出し、**乗算**を 2 つのパラメーターを受け取る関数。 ツールヒント ボックスが表示されていることを確認、**乗算**関数のドキュメント。

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![JavaScript 関数の XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "JavaScript 関数の XML ドキュメント")

    *JavaScript 関数の XML ドキュメント*
4. 種類と関数の呼び出しステートメントの完了を*ドット*戻り値に対して、IntelliSense の一覧を開きます。 Visual Studio の検出に注意してください、**返す**値、ドキュメントでは、数値として処理されます。

    ![戻り値の型の XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "戻り値の型の XML ドキュメント")

    *戻り値の型の XML ドキュメント*
5. ここで、関数を追加する呼び出しを挿入します。 JavaScript エディターが今すぐ関数のオーバー ロードをサポートしていることに注意してください。 関数名を記述するときに、ドキュメントで指定された使用可能なオーバー ロードのいずれかを選択することができます。

    ![XML ドキュメントのオーバー ロードを](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "オーバー ロードの XML ドキュメント")

    *オーバー ロードの XML ドキュメント*
6. 開いている**GotoDefinition.js**ファイルし、検索、 **$().html()** 関数呼び出し。 カーソルを置いて**html**します。
7. キーを押して**F12**定義に移動します。 アクセスしを使用せず、JavaScript コードを閲覧できますに注意してください、**検索**ツール。
8. コード ファイルの下部にある署名ブロックの前に、jQuery の命令にカーソルを見つけます。 キーを押して**F12**します。 JQuery ライブラリ ファイルに移動します。 使用して、jQuery ファイル間で移動することもできますに注意してください。 **F12**します。

    ![JQuery の定義に移動する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "jQuery の定義に移動します。")

    *JQuery の定義に移動します。*

> [!NOTE]
> GotoDefinition.js ファイルを保存する前に構文エラーがないことを確認します。


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>手順 4: バンドルと縮小

何回か、web サイトは 1 つ以上の JavaScript または CSS ファイルか。 これは、バンドルと縮小に役立つファイル サイズを小さくし、高速で実行するサイトをごく一般的なシナリオです。 ASP.NET 4.5 の新機能でバンドルは、JS または CSS ファイルのセットを 1 つの要素にパックし、(必須ではない空白を削除する、コメントを削除する、識別子の削減) コンテンツを縮小してサイズを縮小します。

プロセスは (たとえば IE、Mozilla など)、ユーザー エージェントを識別して、そのため、(たとえば、削除するとか色々 な処理は Mozilla の特定のユーザーのブラウザーを対象とすることによって、圧縮が向上できるように、実行時に実行はバンドルと縮小 ASP.NET 4.5 でときに、要求元が IE)。

この演習では、有効にして ASP.NET 4.5 でのバンドルと縮小のさまざまな種類を使用する方法を学びます。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>タスク 1 - バンドルをインストールし、NuGet からパッケージを縮小

1. これを開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。
2. NuGet パッケージ マネージャー コンソールを開きます。 これを行うには、メニューを使用して**ビュー** | **その他の Windows** | **パッケージ マネージャー コンソール**します。

    ![開くパッケージ マネージャーの file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "パッケージ マネージャー コンソールを開く")

    *パッケージ マネージャー コンソールを開く*
3. **パッケージ マネージャー コンソールで、** 型**Install-package Microsoft.Web.Optimization**キーを押します**ENTER**します。

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>タスク 2 - 既定のバンドル

既定のバンドルはバンドルと縮小を使用する最も簡単な方法です。 このメソッドでは、規則を使用して、フォルダー内の JS、CSS ファイルのバンドルと縮小されたバージョンを参照することができます。

このタスクでは、有効にしてバンドルおよび縮小された JS、CSS ファイルを参照して、結果の出力を表示する方法を学びます。

1. これを開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**ソリューション、 **Source\WhatsNewASPNET**このラボのフォルダー。
2. **ソリューション エクスプ ローラー**、展開、**スタイル**、 **Scripts\custom**と**Scripts\bundle**フォルダー。

    アプリケーションが 1 つ以上の CSS と JS ファイルを使用することに注意してください。

    ![アプリケーションの複数のスタイル シートおよび JavaScript ファイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "アプリケーションで複数のスタイル シートおよび JavaScript ファイル")

    *アプリケーションで複数のスタイル シートおよび JavaScript ファイル*
3. 開く、 **Global.asax.cs**ファイル。

    注意して、新しい**Microsoft.Web.Optimization**名前空間が、ファイルの先頭にコメント アウトされています。 使用して、コメントを解除します。 バンドルと縮小の機能をインクルードします。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. 検索、**アプリケーション\_開始**メソッド。

    このメソッドは、次のスニペットに示すように EnableDefaultBundles 呼び出しをコメント解除します。 これにより、そのフォルダーのパスを使用してフォルダー内の CSS ファイルのバンドル コレクションを参照するだけでなく、 &quot;CSS&quot;または&quot;JS&quot;サフィックス。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. 開く、 **Optimization.aspx**ファイルし、のコンテンツ コントロールを探して**HeadContent**します。

    CSS ファイルと 1 つの参照されているタグがある JS ファイルに注意してください。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > このコードは、デモの目的では。 理想的には、Site.Master ファイル内のバンドルを参照します。 このサンプル コードで紹介するバンドル ファイルの一部もによって参照されている、Site.Master ファイル冗長この最後の参照を作成します。
6. リンクがバンドル内の規則を使用していることに注意してください、 **href**スタイルと Scripts\custom からすべての CSS または JS ファイルを取得する属性フォルダーそれぞれします。

    パスを使用することができます**スクリプト/カスタム/JS**バンドルし、縮小内のすべての JS ファイルに次のような**スクリプト/カスタム**フォルダー。 これは、既定のバンドルと既定の動作です。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. 開く、 **Styles\Site.css**ファイル。

    元の CSS ファイルには、インデントされたコード、空白、およびファイルを拡大するコメントが含まれていることを確認します。 (また、JavaScript ファイルが含まれています空白とコメント)。

    ![スクリプト フォルダーにファイルを元の CSS のいずれかの](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "スクリプト フォルダーにファイルを元の CSS のいずれか")

    *Scripts フォルダーでは、元の CSS ファイルのいずれか*
8. キーを押して**F5**アプリケーションを実行しに移動する、**最適化**ページ。
9. をクリックして、 **CSS バンドル**へのリンクをダウンロードして、ファイルを開きます。

    縮小されたバンドルされているファイルを確認します。 表示されます、空白、コメントおよびインデント文字をすべて削除されていること、サイズの小さいファイルを生成します。

    ![CSS ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "バンドルされている CSS ファイル")

    *バンドルの CSS ファイル*
10. クリックし、 **JS バンドル**バンドルされている JavaScript ファイルを開くためのリンク。 エクスプ ローラーの警告は無視して構いません。 JavaScript ファイルに注意してください、**カスタム**フォルダーもバンドルおよび縮小します。

    ![JavaScript ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "バンドルされている JavaScript ファイル")

    *バンドルの JavaScript ファイル*

    CSS または JS ファイルの圧縮を有効にするは、ASP.NET の以前のバージョンではるかに複雑ですが。 ここで説明したようにするだけ 1 つの行を追加、 *Global.asax*ファイルのバンドル化、有効にして、サイトからバンドルのファイルを参照します。

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>タスク 3 - 静的バンドル

バンドルの静的なアプローチでは、バンドル、参照および使用される縮小メソッドへのファイルのセットをカスタマイズできます。

このタスクでは、バンドルし、縮小するファイルの特定のセットを定義する静的バンドルを構成します。

1. ブラウザーを閉じます。
2. 開く、 **Global.asax.cs**ファイルし、検索、**アプリケーション\_開始**メソッド。
3. 次のコードに示すように、静的バンドル コードをコメント解除します。

    参照される静的バンドルを定義する、 &quot; **~/StaticBundle** &quot;仮想パスと使用**JsMinify**で指定されたすべてのファイルの縮小を**AddFile**メソッド。 最後に、静的バンドルを追加する場合は、 **BundleTable**有効化します。

    ファイルが、同じ場所にいないにあることに注意してください。これは、既定のバンドルをもう 1 つの利点です。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. 開く、 **Optimization.aspx**ファイル。

    注意へのリンク**静的 JS バンドル**Global.asax.cs ファイルの静的なバンドルの構成時に宣言されているパスを使用して: **/StaticBundle**します。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. キーを押して**F5**アプリケーションを実行しに移動する、**最適化**ページ。
6. をクリックして、**静的 JS バンドル**ファイルを開くためのリンク。

    パスの下の静的なバンドル ファイルで構成されているすべての JavaScript ファイルの出力は、縮小されたバンドルの JavaScript ファイル&quot;/StaticBundle&quot;します。

    ![静的な JavaScript ファイルのバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "静的 JavaScript ファイルのバンドル")

    *静的な JavaScript ファイルをバンドルします。*
7. ブラウザーを閉じて、Visual Studio に戻ります。

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>タスク 4 - 動的フォルダー バンドル

このタスクでは、動的フォルダー バンドルを構成する方法を学習します。 動的バンドルの電源は、こと、JavaScript にコンパイルされる言語で、静的な JavaScript だけでなく、他のファイルを含めるし、そのため、バンドルが実行される前に、何らかの処理が必要ですが。

この例を使用する方法について説明します、 **DynamicFolderBundle** CofeeScript で記述されたファイルの動的なバンドルを作成するクラス。 CofeeScript は、JavaScript にコンパイルし、JavaScript コードの記述が JavaScript の簡潔さと読みやすさを向上させるの単純な構文を提供するプログラミング言語です。

1. 開く、 **Global.asax.cs**ファイルし、検索、**アプリケーション\_開始**メソッド。
2. 次のコードに示すように動的なバンドル コードをコメント解除します。

    使用する動的フォルダー バンドルを定義する、 **CoffeeMinify**でファイルにのみ適用されるカスタムの縮小プロセッサ、 &quot; ***.coffee** &quot;拡張機能 (CoffeeScript ファイル)。 通知など、フォルダー内にバンドルするファイルを選択し、検索パターンを使用することができます '\**.coffee '。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. NuGet パッケージ マネージャー コンソールを開きます。 これを行うには、メニューを使用して**ビュー** | **その他の Windows** | **パッケージ マネージャー コンソール**します。
4. **パッケージ マネージャー コンソールで、** 型**Install-package CoffeeSharp**キーを押します**ENTER**します。
5. をクリックして、 **すべてのファイル**ボタン、**ソリューション エクスプ ローラー**ウィンドウ

    ![すべてのファイルを示す](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "すべてのファイルを表示")

    *すべてのファイルを表示*
6. 右クリックして、 **CoffeeMinify.cs**ファイル、**ソリューション エクスプ ローラー**選択**プロジェクトに含める**

    ![CoffeeMinify.cs ファイルをプロジェクトに含める](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs ファイルをプロジェクトに含める")

    *CoffeeMinify.cs ファイルをプロジェクトに含める*
7. 開く、 **CoffeeMinify.cs**ファイル。

    このクラスは、JavaScript の出力、CoffeeScript コードのコンパイルから結果を縮小する JsMinify から継承します。 最初に、JavaScript コードを生成する CoffeeScript コンパイラを呼び出すし、結果のコードを縮小する JsMinify.Process メソッドに送信、し。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. 開く、 **Script1.coffee**と**Script2.coffee**ファイルから、**スクリプト/バンドル**フォルダー。

    これらのファイルは、CoffeeMinify クラスを使用してバンドルを実行中にコンパイルする CoffeScript コードが含まれます。

    わかりやすくするための目的で提供されている CoffeeScript ファイルは CoffeeScript のサンプル コードのみを含むです。 コメントは、JsMinify プロセスによって除外されます。

    ![CoffeeScript ファイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript ファイル")

    *CoffeeScript ファイル*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/)の JavaScript コードを記述、JavaScript の簡潔さと読みやすさ、強化だけでなく配列理解してパターン マッチングのようなその他の機能を追加する単純な構文を提供します。
9. 開く、 **Optimization.aspx**ファイルし、バンドルのリンクを探します。

    注意してへのリンク**動的 JS バンドル**を参照して、**スクリプト/バンドル**フォルダーを使用して、 **コーヒー/** 動的フォルダー バンドルの構成されているサフィックス。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. キーを押して**F5**アプリケーションを実行しに移動する、**最適化**ページ。
11. をクリックして、**動的 JS バンドル**生成されたファイルを開くためのリンク。

    このバンドルに含まれているコンテンツのみを含む通知 ***.coffee**ファイル。 CoffeeScript コードが JavaScript にコンパイルされたされ、コメント アウトされた行が削除されたこともわかります。

    ![動的な JS ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "動的 JS ファイルのバンドル")

    *動的な JS ファイルのバンドル*

> [!NOTE]
> また、Windows Azure Web サイトの次に、このアプリケーションを展開できます[付録 b: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)します。


<a id="Summary"></a>
## <a name="summary"></a>まとめ

このラボでは、ASP.NET の新機能と Visual Studio 2012 での Web 開発と Visual Studio 2012 で、さまざまな機能強化の活用方法を理解するのに役立ちます。

このハンズオン ラボを完了するが、Visual Studio 2012 のエディターで CSS、JavaScript と HTML の新しい機能と機能強化を使用する方法を習得します。 さらに、Visual Studio 2012 で組み込みのバンドルと縮小を実装する方法を習得があります。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express for Web のタイル*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

この付録では、Windows Azure 管理ポータルから新しい web サイトを作成して Windows Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>タスク 1 - 新しい Web サイトを作成する、Windows から Azure Portal

1. 移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Windows Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。

    ![Windows Azure ポータルにログオン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Windows Azure ポータルにログオン")

    *Windows Azure 管理ポータルにログオン*
2. クリックして**新規**コマンド バーにします。

    ![新しい Web サイトを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. クリックして**コンピューティング** | **Web サイト**します。 選び**簡易作成**オプション。 新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure の Web サイトには、制御および管理できる、クラウドで実行されている web アプリケーション、ホストです。 簡易作成 オプションを使用すると、完成した web アプリケーションを Windows Azure の Web サイトから、ポータル外からデプロイできます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**が作成されます。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。 新しい Web サイトが動作していることを確認します。

    ![新しい web サイトを参照](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトが実行されている](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドごとに Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはWindows Azure の web サイトへの web アプリケーションを発行しています。

    ![発行プロファイルのダウンロード web サイト](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "発行プロファイルの web サイトのダウンロード")

    *発行プロファイルの Web サイトのダウンロード*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの演習ではこのファイルを使用して、Visual Studio から Windows Azure Web サイトに web アプリケーションを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "発行プロファイルを保存しています")

    *発行プロファイル ファイルを保存しています*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。 SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。

1. SQL Database サーバーは、アプリケーション データベースを格納する必要があります。 Windows Azure 管理ポータル内のサブスクリプションから SQL Database サーバーを表示する**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**します。 使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。 メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。 データベースを作成しない、まだ、後の段階でそれが作成されます。

    ![SQL データベース サーバーのダッシュ ボード](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。 次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックス。 ルールの名前を入力し、クリックして、 ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png)ボタンをクリックします。

    ![クライアント IP アドレスを追加します。](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *クライアント IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *変更を確認します。*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 のソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。

    ![アプリケーションの発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初のタスクで保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. クリックして**接続の検証**です。 検証が完了したら、クリックして**次**します。

    > [!NOTE]
    > 接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。

    ![接続の検証](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "接続の検証")

    *接続の検証*
4. **設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。

    ![Web 配置の構成](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web 配置の構成")

    *Web 配置の構成*
5. データベース接続を次のように構成します。

   - **サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。
   - **ユーザー名**サーバー管理者ログイン名を入力します。
   - **パスワード**サーバー管理者ログイン パスワードを入力します。
   - たとえば、新しいデータベース名を入力: *MVC4SampleDB*します。

     ![ターゲットの接続文字列を構成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "ターゲットの接続文字列を構成します。")

     *ターゲットの接続文字列を構成します。*
6. 次に、 **[OK]** をクリックします。 データベースのクリックを作成するように求められたら**はい**します。

    ![データベースを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "データベース文字列を作成します。")

    *データベースの作成*
7. Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。 その後、 **[次へ]** をクリックします。

    ![SQL データベースを指す接続文字列](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**します。

    ![Web アプリケーションの発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。

    ![Windows Azure にアプリケーションが発行される](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "アプリケーションは Windows Azure に発行")

    *Windows Azure に発行されたアプリケーション*
