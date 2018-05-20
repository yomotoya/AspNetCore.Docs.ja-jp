---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: ASP.NET および Visual Studio 2012 での Web 開発の新機能 |Microsoft ドキュメント
author: rick-anderson
description: 新しいバージョンの Visual Studio には、さまざまな Web テクノロジを使用する場合、エクスペリエンスとパフォーマンスを向上させることに重点を置いた機能強化が導入されています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f447dc0108dffb36ed6d627fb83b3117fd22c94c
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/18/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>ASP.NET および Visual Studio 2012 での Web 開発の新機能
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> 新しいバージョンの Visual Studio には、さまざまな Web テクノロジを使用する場合、エクスペリエンスとパフォーマンスを向上させることに重点を置いた機能強化が導入されています。 CSS、JavaScript と HTML の visual Studio エディターは、IntelliSense や自動インデントなど、最もでオンデマンド コード補助機能の多くを完全に刷新されました。 パフォーマンスに関してバンドルと縮小が統合されたページを簡単に削減を組み込みの機能の読み込み時にします。
> 
> Visual Studio では、web サイトの最新のテクノロジを使用することができます。 クロス ブラウザー CSS3 スニペットを使用するも、新しい HTML5 要素と機能の利点を活かしながら、サイトがクライアントのプラットフォームに関係なく動作を確認します。
> 
> 作成および JavaScript コードのプロファイリングは、この Visual Studio のバージョンを簡単にする必要があります。 IntelliSense リストでは、XML ドキュメントとナビゲーションの統合の機能は、JavaScript コードにできます。 すぐに JavaScript カタログがあるようになりました。 さらに、スクリプトで ECMAScript5 コンプライアンスを確認し、早い段階で構文エラーを検出できます。
> 
> 最後はさらに、このバージョンの Visual Studio は、組み込みのバンドルと縮小を実装します。 スタイル シート、スクリプト ファイルがパックし、サイトを高速実行するように圧縮します。
> 
> このラボには、機能強化と軽微な変更を元のフォルダーで提供されるサンプル Web アプリケーションに適用することで前に説明した新しい機能について説明します。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)です。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズ オン ラボで学習する方法。

- CSS エディターの新機能と機能強化を使用します。
- HTML エディターの新機能と機能強化を使用します。
- JavaScript エディターの新機能と機能強化を使用します。
- 構成および使用のバンドルと縮小

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (用セットアップ スクリプトに Windows 8 および Windows Server 2008 R2 に既にインストールされている)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home)または HTML5 対応ブラウザー

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズ オン ラボには、次の実習が含まれています。

1. [CSS エディターの新機能はどのような演習 1 です。](#Exercise1)
2. [新しい HTML エディターではどのような演習 2 です。](#Exercise2)
3. [手順 3: は、JavaScript エディターの新機能](#Exercise3)
4. [手順 4: バンドルと縮小](#Exercise4)

この演習を完了する時間を推定: **60 分**です。

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>CSS エディターの新機能はどのような演習 1 です。

Web 開発者は、CSS の編集に関連する問題の多くと理解しておく必要があります。 CSS スタイルの最大の問題の 1 つは、クロス ブラウザーの互換性です。 多くの場合、スタイルを適用すると、サイトに、わかりますをさまざまな参照別のブラウザーまたはデバイスで開く場合行われます。 そのため、それらに注意して、最後に、1 つのブラウザーで作業を行うときに分割されるその他の視覚的な問題を修正するのにかなりの時間を費やす可能性があります。

Visual Studio には、開発者にアクセスし、作業、CSS スタイル シートを効果的に整理に役立つ機能が含まれています。 この演習で効果的な組織や edition の新機能および css3 用のコード スニペットのクロス ブラウザーの互換性が満たされます。

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>タスク 1 - エディターの新機能

このタスクは、CSS エディターの新しい機能を検出します。 この新しいエディターには、新しいスマート インデント、強化されたコードのコメントおよび IntelliSense の拡張の一覧を活用して生産性を向上するのに役立ちます。

1. 開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。
2. ソリューション エクスプ ローラーで開く、 **Site.css**の下にあるファイル、**スタイル**フォルダーです。 確認、**テキスト エディター**ツールは、ツールバーに表示されます。 実行するには、選択、**ビュー** | **ツールバー**メニュー オプションとチェック、**テキスト エディター**オプション。 表示になります、この新しいバージョンでは、以降、**コメント**ボタン (![コメント ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png)) および**コメント解除**ボタン (![のコメントを解除-ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png))、CSS エディターのも有効にします。

    ![エディターと CSS ツールを有効にする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "エディターと CSS ツールを有効にします。")

    *エディターと CSS ツールを有効にします。*
3. コードをスクロールし、任意の CSS クラスの定義を選択します。 をクリックして、**コメント**(![コメント ボタン](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)) 選択した行をコメントするボタンをクリックします。 [] をクリックして、**コメント解除**(![のコメントを解除](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) 変更を元に戻すボタンをクリックします。
4. をクリックして、**折りたたむ**(![折りたたむ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) と**展開**(![展開](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) テキストの左余白のボタンがあります。 クリーナー ビューを使用しないスタイルを隠すことができるようになりましたことに注意してください。

    ![CSS クラスの折りたたみ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "折りたたみ CSS クラス")

    *CSS クラスの折りたたみ*
5. スマート インデント機能が有効になっていることを確認します。 選択、**ツール** | **オプション**メニュー オプションをクリックし、**テキスト エディター** | **CSS**  | **書式**画面の左側のウィンドウ内のページです。 チェック、**階層インデント**オプション。

    ![階層構造のインデントを有効にする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "階層インデントを有効にします。")

    *階層構造のインデントを有効にします。*
6. メイン クラス定義 (.main) を見つけて、div 要素にスタイルを追加します。 コードは、配置を一目で、親クラスを検索するユーザーを支援することで、自動的に表示されます。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Css 階層的な配置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "CSS 内の階層の配置")

    *CSS 内の階層の配置*
7. 内 **.main div**クラスの末尾にカーソルを置いて**境界線: 0 px;** とキーを押します**Enter** IntelliSense の一覧を表示します。 入力を開始**上部**し、一覧がフィルター処理する方法を入力するように注意してください。 一覧を含む要素が表示されます**上部**という単語の一部に (Visual Studio の以前のバージョンでは、一覧は、項目でフィルター処理を*開始*という用語と)。

    ![Css IntelliSense の機能強化](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "css IntelliSense の機能強化")

    *Css IntelliSense の機能強化*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>タスク 2 - カラー ピッカー

このタスクでは、Visual Studio IntelliSense に統合されて、新しい CSS カラー ピッカーを検出します。

1. **Site.css、** ヘッダー クラス定義 (.header) を見つけて、カーソルを置き、横に**背景色**属性間、 &quot;:&quot;と&quot; #&quot;の文字のコード行を**です。**

    ![カーソルを検索する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "カーソルを検索します。")

    *カーソルを検索します。*
2. 削除、**コロン**(:) し、もう一度表示するように、カラー ピッカーを記述します。 最初の色が表示されますが、サイトの最も一般的な色であることを確認します。 白い色をクリックした場合、HTML のカラー コード (#fff) は、スタイル シートの現在のカラー コードを置き換えます。

    ![カラー ピッカー](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "カラー ピッカー")

    *カラー ピッカー*
3. キーを押して、**展開**(![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) 色のグラデーションを表示し、別の色を選択するグラデーションのカーソルをドラッグするには、カラー ピッカーのボタンをクリックします。 その後をクリックして、**スポイト**ボタンをクリックし、画面の任意の色を選択します。 カーソルを移動するときに、背景色の値が動的に変更することに注意してください。

    ![グラデーション ピッカー](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "グラデーション ピッカー")

    *グラデーション ピッカー*
4. **不透明度**スライダー、セレクターを不透明度を減らすには、バーの中央に移動します。 背景色の値は、RGBA にスケールを今すぐ変更に注意してください。

    ![カラー ピッカーの不透明度](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "カラー ピッカーの不透明度")

    *カラー ピッカーの不透明度*

    > [!NOTE]
    > CSS3 で RGBA (赤、緑、青、Alpha) 色定義では、1 つの項目の色の不透明度の値を定義することができます。 異なり**不透明度 -** のような CSS 属性**-** RGBA 色は、最新のブラウザーと互換性があります。

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>タスク 3 - CSS 互換性のあるコード スニペット

このタスクでは、web サイトでいくつかの機能を実装するためにクロス ブラウザーの互換性のある CSS3 スニペットを使用する方法を学習します。

1. **Site.css**ファイルを調べ、**ヘッダー** CSS クラス定義 (.header) および下にカーソルを置き、  **/\*罫線 radius\* /** 新しいスニペットを追加するプレース ホルダーです。 キーを押して**Enter** 、IntelliSense の一覧を表示し**radius**一覧をフィルター処理します。 選択、**罫線 radius**二重のクリックで、一覧からオプションをクリックして、**タブ**スニペットを挿入するキー。 Radius サイズを入力し、ピクセルとキーを押して**Enter**です。 たとえば、入力**15px**です。

    スニペットによって追加された CSS3 属性は、Mozilla WebKit ベースのブラウザーなど、ほとんどの HTML5 対応ブラウザーに境界線を曲線で表示されます。

    ![罫線は、radius スニペットを使用して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "罫線 radius スニペットを使用して")

    *罫線は、radius スニペットを使用してください。*
2. 同じ**罫線**スニペット ページのスタイル (.page) にします。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. キーを押して**f5 キーを押して**ソリューションを実行します。 つまり各ページようになりましたが、丸めた罫線に注意してください。

    ![角を丸く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "角を丸く")

    *角が丸い*
4. ブラウザーを閉じて、Visual Studio に戻ります。
5. 開く、 **Custom.css**の下にあるファイル、**スタイル**フォルダー内のカーソルを置く**div.images ul li img**クラス定義です。
6. IntelliSense の一覧を表示する enter キーを押します型**ボックス シャドウ**キーを押すと、**タブ**クラス定義の内部既定シャドウ コード スニペットを挿入するには、2 回のキー。 シャドウの値に設定**10 px 10 px 5 px #888**です。 次に、入力**罫線 radius**およびコード スニペットを挿入します。 型**15px**半径のサイズとキーを押してを設定する**ENTER**です。

    ![シャドウと角を丸く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "シャドウ付き角を丸く")

    *シャドウと角の丸い*

    > [!NOTE]
    > この時点では、(プロパティ、webkit、o) Mozilla をサポートするために、対応するプレフィックスと Webkit (Chrome、Safari、Konkeror) ブラウザーでシャドウ属性が挿入されます。
7. 新しいクラスを作成**div.images ul li img:hover**下、 **div.images ul li img**クラス定義にカーソルを置き、かっこで囲んで**です。**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. 型**変換**キーを押すと、**タブ**変換スニペットを挿入するために 2 回のキー。 次に、入力**rotate(-15deg)** イメージが置かれているときに、回転の角度の値を変更します。

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. キーを押して**f5 キーを押して**ソリューションを実行し、CSS3 ページを参照します。 イメージがコーナーが丸くし、シャドウをボックスに注意してください。 イメージ上にマウスを移動、回転させることを確認します。

    ![イメージの回転のスニペットを変換](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "イメージを回転変換スニペット")

    *イメージを回転スニペットを変換します。*

    > [!NOTE]
    > Internet Explorer 10 を使用するいるし、シャドウを表示することはできません、ドキュメント モードが IE10 標準に設定されていることを確認します。 キーを押して**F12**を Internet Explorer developer tools を開き、をクリックして**ドキュメント モード**IE10 標準に変更します。

    ![に関する-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>新しい HTML エディターではどのような演習 2 です。

Visual Studio には、改善の HTML エディターがあります。 このバージョンに含まれる拡張機能の一部は、HTML ドキュメント、HTML5 スニペット、HTML の開始と終了タグに一致する、および HTML の検証のスマート インデントです。 この演習では、マークアップを web サイトで作業するときに、これらの変更が、習熟を向上する方法がわかります。

CSS エディターのように、HTML エディターも強化されました。 これらの機能強化のほとんどは、Web 開発者の作業を容易にする小規模なものです。 複数のコード スニペットは、HTML5、スマート インデントは、編集と、HTML ドキュメントの DOCTYPE を対象とする検証がこれらの機能強化の一部である場合、開始タグと終了タグを照合のなどです。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>タスク 1 - 強化された DOCTYPE の検証

HTML エディターになりました、ページの DOCTYPE をチェックするよう場合でも、マスター ページで、定義があります。 によっては、ページの DOCTYPE、HTML エディター正しい一連の規則を使用して検証されますが一覧とフィルター IntelliSense DOCTYPE 要素を検討します。

このタスクでは、それに応じて HTML エディターの動作の変化を表示するページの DOCTYPE を変更します。

1. 開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。
2. 開く、 **Site.Master**ページ。
3. 検証のツールバーには、ターゲット スキーマに注意してください。 HTML エディターの動作 (検証、IntelliSense など) は、選択されている Doctype に合わせて正しく変更されます。

    ![HTML ソースの編集 ツールバーに Doctype を使用して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "HTML ソースの編集 ツールバーに Doctype を使用します。")

    *Doctype を使用して、HTML ソースの編集ツールバー*
4. HTML 4.01 にターゲット スキーマを変更します。

    ![HTML ソースの編集 ツールバーに Doctype を変更する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "HTML ソースの編集 ツールバーに Doctype を変更します。")

    *HTML ソースの編集 ツールバーに Doctype を変更します。*
5. 下にあるカーソルを置き、**本文**要素、および HTML5 の要素の名前を入力する (たとえば、**ビデオ**)。 要素が IntelliSense リストで使用できないことに注意してください。

    ![HTML5 要素が表示されていない](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5 要素が表示されていません。")

    *HTML5 要素が表示されていません。*
6. DOCTYPE をピッキング検証ツールバーのターゲット スキーマへの変更を元に戻す: ドロップダウン リストから XHTML5 です。

    ![HTML ソースの編集 ツールバーに Doctype を使用して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "HTML ソースの編集 ツールバーに Doctype を使用します。")

    *Doctype の HTML ソースの編集ツールバーをリセットします。*
7. 下にあるカーソルを置き、**本文**HTML5 要素をもう一度入力する要素と (たとえば、**ビデオ**)。 HTML5 要素は IntelliSense リストで使用できるようになりましたことに注目してください。

    ![HTML5 要素が表示される](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 要素が表示されます。")

    *HTML5 要素が表示されます。*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>タスク 2 - 開始/終了タグを自動更新

Visual Studio では、開く、または相互に一致するように編集している要素のタグを閉じる HTML 今すぐ更新します。 この新しい機能には、HTML タグを編集するときに生産性が向上します。

1. **Default.aspx**  ページで、追加、 **H3**タイトル (たとえば、Visual Studio 2012 もの!) を持つ要素。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. 変更、 **H3**タグおよび種類**H2**または**H1 です。**

    終了タグが自動的に更新することを確認します。 開始タグを更新するそれに応じてすぎるを表示する終了タグを変更することもできます。

    ![終了タグの更新プログラムの自動](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "終了タグの自動更新")

    *終了タグの自動更新*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>タスク 3 - 新しい HTML5 コード スニペット

Visual Studio には、いくつかの HTML5 のコード スニペットが含まれています。 このタスクでは、これらのスニペットの一部を使用します。

1. という名前の新しいフォルダーを追加**オーディオ**web サイト フォルダーのルートにします。 Windows エクスプ ローラーを開きに任意のオーディオ ファイルをコピー、**オーディオ**のフォルダー、 **WhatsNewASPNET.sln**ソリューションです。
2. **Default.aspx** Web11 ものの下でカーソルを検索 ページで、!! ヘッダー。 型**オーディオ**TAB キーを押します。

    新しい HTML エディターには、HTML5 コンテンツのコード スニペットが含まれています。 適切な DOCTYPE 定義を使用して HTML5 スニペットを有効にしてください。

    ![HTML5 のコード スニペットの挿入](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "HTML5 コード スニペットの挿入")

    *HTML5 のコード スニペットの挿入*
3. 既存のオーディオ ファイルを指すオーディオのソースを更新します。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > オーディオ ファイルをソリューションに追加する必要があります。
4. キーを押して**f5 キーを押して**サイトを実行して、オーディオを再生します。

    ![オーディオ コントロールを実行している](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "オーディオ コントロールの実行")

    *オーディオ コントロールの実行*

    > [!NOTE]
    > ビデオ、図など、Visual Studio に含まれている複数のスニペットを試すこともできます。
5. ここで、コントロールを挿入するページの一部で再試行してください。 挿入しようとするなど、 **GridView**コントロールが入力する代わりに **&lt;Gri、** の入力を開始 **&lt;GV**。 IntelliSense の一覧を示す通知、 **asp: GridView**コントロール。

    HTML エディターでの IntelliSense は、タイトル文字種の検索と部分一致する (要素を取得するすべての用語を含む) を提供します。

    ![IntelliSense リストを GridView を挿入する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "IntelliSense リストを GridView の挿入")

    *IntelliSense リストを GridView の挿入*

    入力した場合**&lt;グリッド**の語句に一致するすべてのアイテムが表示されますが、Visual Studio の候補が表示されます、 **gridview**コントロール。

    ![IntelliSense リストおよび部分的な一致を GridView を挿入する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "IntelliSense リストおよび部分的な一致を GridView の挿入")

    *IntelliSense リストおよび部分的な一致を GridView の挿入*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>タスク 4 - スマート タグを HTML エディター

別の向上、HTML エディターでは、スマート タグの機能です。 スマート タグしやすいように、コントロールごとに一般的なや反復的な開発タスクを実行します。 この機能は既にできた HTML デザイナーでは HTML エディターでありません。

1. 開いている**Site.Master**を検索し、 **asp: メニュー**要素。 カーソルを置き、開始タグと要素 - の下部に表示される小さなグリフがクリックして [スマート タスク] メニューを開きますに注意してください。 メニュー コントロールに関連するいくつかのタスクにすばやくアクセスがあることに注意してください。

    ![メニュー コントロールのタスクのスマート](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "メニュー コントロールのタスクのスマート")

    *メニュー コントロールのスマート タスク*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>タスク 5 - スマート インデント

読み取り可能なコードを保持する入れ子になった要素をインデント html 形式でのベスト プラクティスの 1 つです。 Visual Studio 2012 をエディターに自動的にインデントを設定、要素、コードを記述するときにわかります。

> [!NOTE]
> Visual Studio の以前のバージョンではスマート インデントが使用可能な HTML エディターではなく、XML エディターにします。


1. HTML エディターでインデント構成がスマート インデントに設定されていることを確認します。 実行するには、選択、**ツール |オプション**メニュー オプションを選択し、**テキスト エディター |HTML |タブ**画面の左側のウィンドウ内のページです。 スマート インデント オプションを選択します。

    ![HTML エディターの設定](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML エディターの設定")

    *HTML エディターの設定*
2. **Default.aspx**  ページで、オーディオの要素の下のすべてのコンテンツを削除します。
3. 開始の末尾にカーソルを置き**オーディオ**要素とヒット**ENTER**です。

    新しいカーソルの位置が、追加のインデント レベルを持つことに注意してください。

    ![スマート インデント、HTML エディターで](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "スマート インデント、HTML エディター")

    *スマート インデント、HTML エディター*
4. 内容を削除するか、閉じるでオーディオのタグを復元**Default.aspx**なし、変更を保存します。

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>タスク 6: ユーザー コントロールに抽出

関数の場合、コードの一部を抽出するなど、Visual Studio に含まれるリファクタリング ツールは、優れた機能を改善し、既存のコードのリファクタリングを容易にします。 ASP.NET ページの対応するユーザー コントロールへの HTML コードの抽出になります。 手動で行うと、新しいユーザー コントロールを作成する、コード セクションをユーザー コントロールに移動、ユーザー コントロールのタグ プレフィックスを登録および、最後に、ページ上のユーザー コントロールをインスタンス化するように、いくつかの手順が行われます。 ここで、新しい*ユーザー コントロールに抽出*ツールが自動的にこれらの手順を実行します。

このタスクでは、選択したコードから新しいユーザー コントロールを生成するのにユーザー コントロールのコンテキストの操作に新しい展開を使用します。

1. **Default.aspx** ] ページで、[、 **H2**と**オーディオ**要素。
2. 右クリックし、**ユーザー コントロールに抽出**です。

    ![ユーザー コントロールのメニュー オプションに抽出](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "ユーザー コントロールのメニュー オプションに抽出")

    *ユーザー コントロールのメニュー オプションに抽出します。*
3. 新しいユーザー コントロールの名前を入力します。 たとえば、 **Jukebox.ascx**、クリックして **[ok]** です。

    ![展開されたユーザー コントロールを保存](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "展開されたユーザー コントロールの保存")

    *展開されたユーザー コントロールの保存*
4. ユーザー コントロールに抽出された、選択したコードと、選択したコードの元の場所は、新しいユーザー コントロールのインスタンスに置き換えられましたことに注意してください。

    ![ページが自動的に新しいユーザー コントロールを使用して更新](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "ページは、新しいユーザー コントロールを使用して自動的に更新")

    *ページは、新しいユーザー コントロールを使用して自動的に更新*
5. キーを押して**f5 キーを押して**ページを実行し、コントロールが動作することを確認します。

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>手順 3: は、JavaScript エディターの新機能

作成や JavaScript コードを編集が簡単な作業では、特に開始されたときに、アプリケーション サイズを大きく気づいたら長いファイルを処理する場合や数百ある関数 スクリプトの開発者は、通常、コードの読みやすさを維持し、ファイル間で移動する特別な作業を行う必要があります。 スクリプトのナビゲーションで jQuery と同様に JavaScript ライブラリの長いので、コード自体課題となっています。

Visual Studio には、コード モードにアクセスして整理する約束を JavaScript エディターが更新されます。 JavaScript エディターで c# または VB のエディターで既に存在していた Visual Studio の多くの機能が実装されるようになりました。 定義へ移動、自動インデント、ドキュメント、および記述するときに検証します。 書き換えられた IntelliSense リストで、JavaScript 関数のカタログをすぐに利用できるがあります。

この演習では、JavaScript エディターの機能強化および新機能の一部を学習します。 サンプル ファイルを参照し、検出できるは、JavaScript プログラミング Visual Studio 2012 内でより効率的な新しい特徴の各します。

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>タスク 1 - JavaScript エディターの新機能

このタスクでは、コードを整理し、ユーザー エクスペリエンスの改善に集中新しい JavaScript エディター機能の一部を説明します。

1. 開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。
2. キーを押して**f5 キーを押して**アプリケーションを実行するには、次のナビゲーション バーでの JavaScript リンクをクリックします。 ページを更新、いくつかの時刻と確認方法はカウンターがインクリメントします。

    ![ページ カウンター](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "ページ カウンター")

    *ページ カウンター*
3. ブラウザーを終了し、Visual Studio に戻ります。
4. 開く、 **JavaScript.aspx**ページし、検索、 **&lt;スクリプト&gt;** ブロック (下図参照)。

    次のコードを格納する HTML5 のローカル ストレージを使用して、 *pageLoadCount*ページが現在のユーザーがアクセスした回数を格納する変数。 ローカル ストレージは、標準の HTML5 で導入されたクライアント側のキー値データベースです。 データは、ユーザーのブラウザー内のローカルのコンピューターに保存されます。

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > DOCTYPE の次の手順に進む前に XHTML5 に設定されていることを確認します。
5. コードを編集して、JavaScript 用の IntelliSense がローカル記憶域、およびその内部メソッドなどの HTML5 の機能が含まれることに注意してください。

    ![JavaScript での HTML5 の JavaScript 機能が](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "JavaScript での HTML5 の JavaScript 機能")

    *JavaScript での HTML5 の JavaScript 機能*
6. 任意の始め角かっこをクリックして (**{**)、スクリプトからのコードし、角かっこが強調表示されていることに注意してください。

    ![角かっこが強調表示されている](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "角かっこが強調表示されます")

    *角かっこが強調表示されます。*
7. 関数のコメントを解除**testAutoAlign()** (3 つの行を選択し、使用することができます**CTRL** + **K**です。**CTRL** + **U**) 関数のコード内にカーソルを検索します。 2 番目の行の追加に enter キーを押します。 コードは、今すぐ通知**揃え**と**自動インデント**です。

    ![JavaScript コードは自動配置](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript コードは自動配置")

    *JavaScript コードは自動配置*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>作業 2: JavaScript の検証

このタスクは、ECMAScript5 標準の新しい JavaScript の検証を検出します。 この機能はサイトに展開する前にスクリプトの問題を防止しながら、準拠の JavaScript コードを記述するのに役立ちます。

> [!NOTE]
> Visual Studio 2010 では、Visual Studio 2012 ECMAScript5 コンプライアンスを提供しますが、ECMAStript3 コンプライアンスが実装されています。


1. 開いている**ECMA5script5.js**の下にある、 **Scripts\custom**プロジェクト フォルダーです。 ECMAScript5 標準の検証をテストします。

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    チェック アウトすることができます、 &quot; **を使用して厳密な** &quot; ECMAScript5 を使用すると、ファイルの最初の行に方向**厳格モード**です。 このモードは、過去のエディションでは、あいまいさを明確化し、オブジェクトのプロパティの get アクセス操作子および set アクセス操作子、JSON、およびより完全なリフレクションのライブラリのサポートなど、一部の新機能を追加する言語のサブセットで構成されます。
2. 開く、**エラー一覧**開いていない場合 (**ビュー**メニュー |**エラー一覧**)。 通知、**関数**宣言の下線が付けられます。 これは、言語構造内にネストできません標準関数 ECMA5 であるためです。 エラーが発生する次の一覧は、警告の詳細が表示されます。

    ![JavaScript の検証エラー メッセージ](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "JavaScript の検証エラー メッセージ")

    *JavaScript の検証エラー メッセージ*
3. コメント アウト、 **&quot;を使用して厳密な&quot;** 方向とエラーが非表示になりますが、警告まま残っていることを確認します。
4. ファイルの最後の行でのような任意の文字列に書き込む**&quot;テスト&quot;** (引用符は文字列であることを示すを含む)。 IntelliSense の一覧を表示し、選択する文字列の横にあるピリオドを書き込み、**トリム**オプション。

    ECMAScript5 標準で文字列値および変数も文字列の方法があります定義、トリミング、大文字、検索し、置換と同じようにします。

    ![JavaScript での IntelliSense リスト](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "javascript IntelliSense の一覧")

    *JavaScript での IntelliSense リスト*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>タスク 3 - JavaScript 用の XML ドキュメント

このタスクでは、JavaScript での XML ドキュメントの Visual Studio の機能を調査します。 JavaScript IntelliSense の一覧の各関数の XML ドキュメントの表示が表示されます。 さらに、JavaScript でナビゲーション機能は、ことがわかります。

1. 開いている**XMLDoc.js**にあるファイル**スクリプトまたはカスタム**プロジェクト フォルダーです。 このファイルには、JavaScript 関数の各 XML ドキュメントが含まれています。

    ![JavaScript XML ドキュメントが IntelliSense に統合された](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML ドキュメントが IntelliSense に統合")

    *IntelliSense に統合された JavaScript XML ドキュメント*
2. 下**追加**関数**XMLDoc.js**ファイル、という名前の新しい関数を作成する**テスト**です。
3. **テスト**関数を呼び出して、**乗算**2 つのパラメーターを受け取る関数。 ツールヒント ボックスが表示されていることを確認、**乗算**ドキュメントに機能します。

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![JavaScript 関数の XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "JavaScript 関数の XML ドキュメント")

    *JavaScript 関数の XML ドキュメント*
4. 関数呼び出しステートメントと種類を完了する、*ドット*を開くには、返される値の IntelliSense の一覧です。 Visual Studio が検出することを確認、**返す**ドキュメントには、数値として値を扱う方法の値。

    ![戻り値の型の XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "戻り値の型の XML ドキュメント")

    *戻り値の型の XML ドキュメント*
5. 関数を追加するための呼び出しを挿入します。 JavaScript エディターが今すぐ関数のオーバー ロードをサポートしていることに注意してください。 関数名を記述するときに、ドキュメントで指定された使用可能なオーバー ロードのいずれかを選択することができます。

    ![オーバー ロードでは XML ドキュメント](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "オーバー ロードでは XML ドキュメント")

    *オーバー ロードでは XML ドキュメント*
6. 開いている**GotoDefinition.js**ファイルし、検索、 **$().html()** 関数呼び出しです。 カーソルを置いて**html**です。
7. キーを押して**F12**定義に移動します。 今すぐアクセスしを使用せず、JavaScript コードを参照することができますに注意してください、**検索**ツールです。
8. コード ファイルの下部にある署名ブロックの前に、jQuery 命令にカーソルを配置します。 キーを押して**F12**です。 JQuery ライブラリ ファイルに移動します。 使用して jQuery ファイルを移動することもわかります**F12**です。

    ![JQuery の定義に移動する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "jQuery 定義に移動します。")

    *JQuery の定義に移動します。*

> [!NOTE]
> GotoDefinition.js は、ファイルを保存する前に構文エラーがないことを確認してください。


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>手順 4: バンドルと縮小

何回かは、web サイトを含める JavaScript や CSS の 1 つ以上のファイルですか。 これは、そこバンドルと縮小で役立つファイル サイズを小さくし、高速に実行するサイトを非常に一般的なシナリオです。 ASP.NET 4.5 の新機能でバンドルは、JS または CSS ファイルのセットを 1 つの要素にパックされ、縮小する (必須ではない空白を削除する、コメントを削除する、識別子を減らすこと) のコンテンツのサイズを軽減します。

バンドルと縮小 ASP.NET 4.5 では実行時に、プロセスを (たとえば IE、Mozilla など)、ユーザー エージェントを識別するし、そのため、(たとえば、削除 stuff Mozilla 固有であるユーザーのブラウザーを対象として、圧縮が向上するためときに、要求が送られた IE から)。

この演習では、有効にして ASP.NET 4.5 でのバンドルと縮小のさまざまな種類の使用方法を学習します。

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>タスク 1 - バンドルのインストールと NuGet パッケージの縮小

1. 開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。
2. NuGet パッケージ マネージャー コンソールを開きます。 これを行うには、メニューを使用して**ビュー** | **その他のウィンドウ** | **Package Manager Console**です。

    ![パッケージ マネージャー file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole を開く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "パッケージ マネージャー コンソールを開く")

    *パッケージ マネージャー コンソールを開く*
3. **Package Manager Console**型**Install-package Microsoft.Web.Optimization**とキーを押します**ENTER**です。

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>タスク 2 - 既定のバンドル

バンドルと縮小を使用する最も簡単な方法は、既定のバンドルを有効にするのにです。 このメソッドは、規則を使用して、フォルダー内の JS および CSS ファイルのバンドルと縮小されたバージョンを参照できます。

このタスクでは、有効にしてバンドルと縮小された JS および CSS ファイルを参照し、結果の出力を表示する方法を学習します。

1. 開いていない場合は、開始**Visual Studio**を開くと、 **WhatsNewASPNET.sln**にソリューションがある、 **Source\WhatsNewASPNET**このラボのフォルダーです。
2. **ソリューション エクスプ ローラー**、展開、**スタイル**、 **Scripts\custom**と**Scripts\bundle**フォルダーです。

    アプリケーションが 1 つ以上の CSS および JS ファイルを使用することに注意してください。

    ![アプリケーション内の複数のスタイル シートと JavaScript ファイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "アプリケーション内の複数のスタイル シートと JavaScript ファイル")

    *アプリケーションで複数のスタイル シートおよび JavaScript ファイル*
3. 開く、 **Global.asax.cs**ファイル。

    注意して、新しい**Microsoft.Web.Optimization**名前空間は、ファイルの先頭にコメント アウトされています。 使用して、コメントを解除バンドルと縮小の機能をインクルードします。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. 検索、**アプリケーション\_開始**メソッドです。

    次のスニペットに示すようには、このメソッドで EnableDefaultBundles 呼び出しをコメント解除します。 これにより、そのフォルダーへのパスを使用して、フォルダー内の CSS ファイルのバンドル コレクションを参照すると、 &quot;CSS&quot;または&quot;JS&quot;サフィックス。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. 開く、 **Optimization.aspx**ファイルし、のコンテンツ コントロールを探して**HeadContent**です。

    CSS ファイルと参照されているタグが 1 つ JS ファイルに注意してください。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > このコードは、デモの目的でです。 理想的には、Site.Master ファイル内のバンドルを参照します。 このサンプル コードで表示されます、バンドルされているファイルの一部もによって参照されている Site.Master ファイル冗長この最後の参照を作成します。
6. リンクが内のバンドルの規則を使用していることを確認、 **href**スタイルと Scripts\custom からすべての CSS または JS ファイルを取得する属性フォルダーそれぞれします。

    パスを使用することができます**スクリプト/カスタム/JS**をバンドルして、内のすべての JS ファイルを縮小する次のように、**スクリプトまたはカスタム**フォルダーです。 これは、既定のバンドルで既定の動作です。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. 開く、 **Styles\Site.css**ファイル。

    元の CSS ファイルには、インデントされたコード、空白文字と、ファイルを大きくコメントが含まれていることを確認します。 (また、JavaScript ファイルが含まれていますの空白文字とコメント)。

    ![スクリプト フォルダーにファイルを元の CSS のいずれかの](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "スクリプト フォルダーにファイルを元の CSS のいずれか")

    *Scripts フォルダー内の元の CSS ファイルのいずれか*
8. キーを押して**f5 キーを押して**アプリケーションを実行しに移動する、**最適化**ページ。
9. をクリックして、 **CSS バンドル**のリンクをダウンロードしてファイルを開きます。

    縮小されたバンドルされているファイルをチェック アウトします。 わかりますすべての空白文字、コメントとインデント文字が削除されているより小さなファイルを生成します。

    ![CSS ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "バンドルされた CSS ファイル")

    *CSS、バンドルされるファイル*
10. クリックし、 **JS バンドル**バンドルされている JavaScript ファイルを開くためのリンク。 警告エクスプ ローラーを安全に無視することができます。 JavaScript ファイルに注意してください、**カスタム**フォルダーもバンドルし、縮小します。

    ![JavaScript ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "バンドルされている JavaScript ファイル")

    *バンドルされている JavaScript ファイル*

    ASP.NET の以前のバージョンでは、はるかに複雑なはしました CSS または JS ファイルの圧縮を有効にします。 ここで、説明したようするだけで 1 つの行を追加、 *Global.asax*ファイルをバンドルするには、有効にして、サイトから、バンドルされているファイルを参照します。

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>タスク 3 - 静的バンドル

静的バンドル アプローチでは、バンドル、参照、および使用される縮小メソッドへのファイルのセットをカスタマイズすることができます。

このタスクでは、ファイルをバンドルして縮小の特定のセットを定義する静的バンドルを構成します。

1. ブラウザーを閉じます。
2. 開く、 **Global.asax.cs**ファイルし、検索、**アプリケーション\_開始**メソッドです。
3. 次のコードに示すように、静的なバンドル コードのコメントを解除します。

    参照される静的バンドルを定義する、 &quot; **~/StaticBundle** &quot;仮想パスと使用**JsMinify**用に指定されたすべてのファイルのサイズを縮小します**AddFile**メソッドです。 最後に、静的のバンドルを追加する、 **BundleTable**有効にするとします。

    ファイルが同じ位置に固定; に存在しないことに注意してください。これは、既定のバンドル経由のもう 1 つの利点です。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. 開く、 **Optimization.aspx**ファイル。

    注意してへのリンク**静的 JS バンドル**Global.asax.cs ファイルで、静的なバンドルの構成時に宣言されているパスを使用して: **/StaticBundle**です。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. キーを押して**f5 キーを押して**アプリケーションを実行しに移動する、**最適化**ページ。
6. をクリックして、**静的 JS バンドル**ファイルを開くためのリンク。

    静的バンドル ファイル パスの下で構成されているすべての JavaScript ファイルの出力、縮小されたバンドルの JavaScript ファイル&quot;/StaticBundle&quot;です。

    ![静的な JavaScript ファイルのバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "静的 JavaScript ファイルのバンドル")

    *静的な JavaScript ファイルをバンドルします。*
7. ブラウザーを閉じて、Visual Studio に戻ります。

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>タスク 4 - 動的フォルダー バンドル

このタスクでは、動的フォルダー バンドルを構成する方法を学習します。 動的バンドルの電源は、JavaScript にコンパイルされる言語で、静的な JavaScript だけでなく、その他のファイルを含めるし、できるためのバンドルを実行する前に何らかの処理を必要です。

この例では、使用する方法を学習します、 **DynamicFolderBundle** CofeeScript で記述されたファイルの動的なバンドルを作成するクラス。 CofeeScript は、JavaScript にコンパイルし、JavaScript コードを記述、JavaScript の簡潔さと読みやすさの向上の単純な構文を提供するプログラミング言語です。

1. 開く、 **Global.asax.cs**ファイルし、検索、**アプリケーション\_開始**メソッドです。
2. 次のコードに示すように、動的なバンドル コードのコメントを解除します。

    使用する動的フォルダー バンドルを定義する、 **CoffeeMinify**を使用したファイルにのみ適用されるカスタムの縮小プロセッサ、 &quot; **.coffee** &quot;拡張機能 (CoffeeScript ファイル)。 同様に、フォルダー内にバンドルするファイルを選択し、検索パターンを使用することができますに注意してください '\*.coffee' です。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. NuGet パッケージ マネージャー コンソールを開きます。 これを行うには、メニューを使用して**ビュー** | **その他のウィンドウ** | **Package Manager Console**です。
4. **Package Manager Console**型**Install-package CoffeeSharp**とキーを押します**ENTER**です。
5. クリックして、 **すべてのファイル**ボタンをクリックして、**ソリューション エクスプ ローラー**ウィンドウ

    ![すべてのファイルを示す](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "すべてのファイルを表示")

    *すべてのファイルを表示*
6. 右クリックして、 **CoffeeMinify.cs**ファイルで、**ソリューション エクスプ ローラー**選択**プロジェクトに含める**

    ![CoffeeMinify.cs ファイルをプロジェクトに含める](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs ファイルをプロジェクトに含める")

    *CoffeeMinify.cs ファイルをプロジェクトに含める*
7. 開く、 **CoffeeMinify.cs**ファイル。

    このクラスは、JavaScript の出力 CoffeeScript コードのコンパイルから結果を縮小する JsMinify から継承します。 最初に、JavaScript コードを生成する CoffeeScript コンパイラを呼び出すし、その、結果のコードを縮小する JsMinify.Process メソッドにします。

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. 開く、 **Script1.coffee**と**Script2.coffee**ファイルから、**スクリプト/バンドル**フォルダーです。

    これらのファイルは、CoffeeMinify クラスとのバンドルを実行中にコンパイルする CoffeScript コードが含まれます。

    わかりやすくするための目的で提供されている CoffeeScript ファイルは CoffeeScript サンプル コードのみを含みます。 JsMinify プロセスでは、コメントは除外します。

    ![CoffeeScript ファイル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript ファイル")

    *CoffeeScript ファイル*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/)の JavaScript コードを記述、JavaScript の簡潔さと読みやすさの向上だけでなく配列の理解とパターンに一致するようにその他の機能を追加する単純な構文を提供します。
9. 開く、 **Optimization.aspx**ファイルし、バンドルのリンクを検索します。

    注意してへのリンク**動的 JS バンドル**を参照している、**スクリプト/バンドル**フォルダーを使用して、 **コーヒー/** 動的フォルダー バンドルの構成されているサフィックス。

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. キーを押して**f5 キーを押して**アプリケーションを実行しに移動する、**最適化**ページ。
11. をクリックして、**動的 JS バンドル**生成されたファイルを開くためのリンク。

    このバンドルに含まれているコンテンツのみを含む通知 **.coffee**ファイル。 CoffeeScript コードが JavaScript にコンパイルされたこと、およびコメント アウトされた行が削除されてもわかります。

    ![動的な JS ファイルをバンドル](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "動的 JS ファイル バンドル")

    *動的な JS ファイルをバンドルします。*

> [!NOTE]
> Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。


<a id="Summary"></a>
## <a name="summary"></a>まとめ

このラボでは、ASP.NET で新機能と Visual Studio 2012 での Web 開発と Visual Studio 2012 で、さまざまな拡張機能の活用方法を理解するのに役立ちます。

このハンズオン ラボを完了すると、Visual Studio 2012 のエディターで CSS、JavaScript と HTML の新機能と機能強化を使用する方法を習得がします。 さらに、Visual Studio 2012 で、組み込みのバンドルと縮小がどのように実装する方法を習得がします。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express Web タイルを*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。

この付録では、Windows Azure 管理ポータルから新しい web サイトを作成し、Windows Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>タスク 1 - Windows から新しい Web サイトを作成する Azure ポータル

1. 移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Windows Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。

    ![Windows Azure ポータルにログオンする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Windows Azure ポータルにログオンします。")

    *Windows Azure 管理ポータルにログオンします。*
2. をクリックして**新規**コマンド バーでします。

    ![新しい Web サイトを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. をクリックして**コンピューティング** | **Web サイト**です。 選択し、**簡易作成**オプション。 新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。 簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**を作成します。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。 新しい Web サイトが動作していることを確認してください。

    ![新しい web サイトを参照して](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトを実行している](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "を実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。

    > [!NOTE]
    > *発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。

    ![発行プロファイルを web サイトをダウンロードする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "発行プロファイルを web サイトをダウンロードします。")

    *発行プロファイルを Web サイトをダウンロードします。*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "発行プロファイルの保存")

    *発行プロファイル ファイルを保存します。*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。 SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。

1. SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。 SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。 使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。 注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。 データベースを作成しない、まだと後の段階で作成されます。

    ![SQL データベース サーバーのダッシュ ボード](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。 実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**テキスト ボックス。 ルールの名前を入力し、クリックして、 ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png)ボタンをクリックします。

    ![クライアントの IP アドレスを追加します。](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *クライアントの IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *変更を確認します。*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 ソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。

    ![アプリケーションの発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初の作業を保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. をクリックして**への接続検証**です。 検証が完了したらクリックして**次**です。

    > [!NOTE]
    > 検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。

    ![接続の検証](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "接続の検証")

    *接続の検証*
4. **設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。

    ![Web 配置の構成](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web 配置の構成")

    *Web 配置の構成*
5. 次のように、データベースの接続を構成します。

   - **サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:* プレフィックス。
   - **ユーザー名**サーバー管理者のログイン名を入力します。
   - **パスワード**サーバー管理者のログイン パスワードを入力します。
   - たとえば、新しいデータベース名を入力: *MVC4SampleDB*です。

     ![対象の接続文字列を構成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "対象の接続文字列を構成します。")

     *対象の接続文字列を構成します。*
6. 次に、 **[OK]** をクリックします。 データベースをクリックを作成するように求められたら**はい**です。

    ![データベースを作成する](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "データベース文字列を作成します。")

    *データベースの作成*
7. 接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。 その後、 **[次へ]** をクリックします。

    ![SQL データベースを指す接続文字列](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**です。

    ![Web アプリケーションの発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。

    ![アプリケーションが Windows Azure に発行](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "アプリケーションが Windows Azure に発行")

    *Windows Azure に発行されるアプリケーション*
