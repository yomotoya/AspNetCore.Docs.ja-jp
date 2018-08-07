---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'ハンズ オン ラボ: Visual Studio 2013 Web ツール |Microsoft Docs'
author: rick-anderson
description: Visual Studio には、優れた開発環境です。NET ベースの Windows と web プロジェクト。 簡単に使用できる強力なテキスト エディターが含まれています.
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 9553d3129ff4c942eacbba628d1daaf6c4e33635
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807529"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>ハンズ オン ラボ: Visual Studio 2013 Web ツール
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> Visual Studio には、優れた開発環境です。NET ベースの Windows と web プロジェクト。 これには、プロジェクトなしのスタンドアロン ファイルの編集を簡単に使用できる強力なテキスト エディターが含まれます。
> 
> Visual Studio は、各ファイルを編集すると、全機能装備の解析ツリーを保持します。 これにより、Visual Studio 開発体験をはるかに高速より快適比類のないオート コンプリートとドキュメントに基づくアクションを提供します。 これらの機能は、HTML および CSS のドキュメントでは特に強力です。
> 
> すべてこの電源は、拡張機能、強力な新しい機能によって、ニーズに合わせてのエディターの拡張を簡単に使用できます。 Web Essentials は、Visual Studio に拡張を (主に) web 関連のコレクションです。 (特に、CSS) の新しい IntelliSense 入力候補、ブラウザー リンクの新機能、自動の多くが含まれています JSHint の JavaScript ファイル、HTML、CSS、および最新の web 開発に不可欠な他の多くの機能の新しい警告します。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)します。


<a id="Overview"></a>
## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- 豊富な HTML5 コード スニペットや Zen コーディングなど、Web Essentials に含まれる HTML エディターの新機能を使用します。
- カラー ピッカーやブラウザー マトリックス ツールヒントなど、Web Essentials に含まれる CSS エディターの新機能を使用します。
- すべての HTML 要素を抽出するファイルと IntelliSense などの Web Essentials に含まれる JavaScript エディターの新機能を使用します。
- ブラウザーとブラウザー リンクを使用して Visual Studio 間の Exchange データ

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>前提条件

このハンズオン ラボを完了する、次が必要。

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/)以上
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

このハンズオン ラボの演習を実行するためには、まず環境を設定する必要があります。

1. Windows エクスプ ローラー ウィンドウを開き、ラボを参照**ソース**フォルダー。
2. 右クリック**Setup.cmd**選択**管理者として実行**環境を構成すると、このラボの Visual Studio のコード スニペットがインストールのセットアップ プロセスを起動します。
3. ユーザー アカウント制御ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。

> [!NOTE]
> セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認します。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>コード スニペットの使用

ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。 便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを避けるために Visual Studio 2013 内からアクセスできるとして提供されます。

> [!NOTE]
> ソリューションでは個々 の演習を伴います、**開始**を使用すると、各演習を他のユーザーとは無関係に練習のフォルダー。 演習の中に追加されるコード スニペットはこれらのスターティング ソリューションが表示されないし、演習を完了するまで動作しない可能性がありますに注意してください。 演習では、ソース コード内でも表示されます、**エンド**結果から、対応する演習の手順を実行するコードと Visual Studio ソリューションを含むフォルダー。 このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の演習が含まれます。

1. [Browser Link と Web Essentials の使用](#Exercise1)
2. [コード スニペットと IntelliSense の活用](#Exercise2)

> [!NOTE]
> Visual Studio を初めて起動すると、定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルに一致するように設計されていて、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペット、およびダイアログ ボックスのオプションを決定します。 このラボの手順を使用する場合は、Visual Studio で特定のタスクを実行するために必要な操作を記述する、**汎用開発設定**コレクション。 開発環境のさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Browser Link と Web Essentials 演習 1: を使用します。

**Web Essentials**はさまざまな web 開発体験をはるかに高速より快適に焦点を合わせた、最新の web 開発用の便利な機能を追加する Visual Studio 拡張機能です。 Web Essentials は、Visual Studio の拡張機能ギャラリーからインストールできます。

**Browser Link**は Visual Studio、web アプリケーションの間でデータを交換するには、Visual Studio IDE と任意の開いているブラウザー間のチャネルを提供する Visual Studio 2013 に含まれる新機能です。 Web Essentials は、DOM オブジェクト モデルと、ブラウザーから直接 web ページの CSS スタイルを操作するためのツールを Browser Link を拡張します。

この演習で一部の機能でサポートされている探索は**Web Essentials**と**Browser Link**簡単なクイズ ページを強化するためにします。

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>タスク 1 - 複数のブラウザーでプロジェクトを実行します。

このタスクでは、クロス ブラウザー テスト用の便利なは、一度に複数のブラウザーで実行する web アプリケーションを構成します。

1. 開いている**Microsoft Visual Studio**します。
2. **ファイル**メニューの**開く |プロジェクト/ソリューションしています.** を見つけて**Ex1 WorkingwithBrowserLinkandWebEssentials\Begin**で、**ソース**ラボ (C:\WebCampsTK\HOL\VSWebTooling\Source) のフォルダーです。 選択**Begin.sln**をクリック**開く**です。
3. Visual Studio のツールバーで、[ブラウザー] メニューを展開し、選択**を参照しています.**.

    ![ブラウザーのメニュー オプション](visual-studio-2013-web-tools/_static/image1.png "ブラウザーのメニューで で [参照]")

    *ブラウザーのメニュー オプション*
4. **Browse With**ダイアログ ボックスの両方をオン**Google Chrome**と**Internet Explorer**押したまま、 **CTRL**キーおよびをクリックして**既定値として設定**です。

    ![ダイアログ ボックスで [参照]](visual-studio-2013-web-tools/_static/image2.png "ダイアログ ボックスで [参照]")

    *複数の既定のブラウザーを選択します。*
5. Google Chrome や Internet Explorer の両方が既定のブラウザーとしては表示されます。 をクリックして**キャンセル**ダイアログ ボックスを閉じます。

    ![Google Chrome と既定のブラウザーとして Internet Explorer](visual-studio-2013-web-tools/_static/image3.png "Google Chrome や Internet Explorer の既定のブラウザー")

    *Google Chrome と既定のブラウザーとして Internet Explorer*

    > [!NOTE]
    > 既定のブラウザーを構成した後、**複数のブラウザー**ブラウザー メニューのオプションを選択します。
    > 
    > ![複数のブラウザー](visual-studio-2013-web-tools/_static/image4.png "複数のブラウザー")
6. キーを押して**CTRL** + **F5**デバッグなしでアプリケーションを実行します。
7. 両方のブラウザー ウィンドウが開いたときに、更新プログラムを同時に両方のブラウザーで表示するには、上記のうちの 1 つを配置します。 ブラウザーでは、薄い青色の四角形を持つ web ページを表示する必要があります。

    ![上の 1 つのブラウザーを配置する](visual-studio-2013-web-tools/_static/image5.png "上、もう 1 つのブラウザーを配置します。")

    *上の 1 つのブラウザーを配置します。*
8. ブラウザーを閉じないでください。 次のタスクでは、それらを使用します。

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>タスク 2 - HTML 要素を作成する場合、コーディングを使用して Zen

**Zen コーディング**エディターは、高速の HTML、XML、XSL (またはその他の構造化されたコード形式) 用のプラグインのコーディングおよび編集します。 このプラグインのコアは、HTML コードに - CSS セレクターのように式を拡張できるようにする強力な省略形エンジンです。 Zen コーディングは、HTML、CSS を使用してスタイルのセレクターの構文を記述する高速の方法です。

この演習では、質問のオプションを表す HTML ボタンを生成するのに Web Essentials によって提供される Zen コーディング機能を使用します。

1. Visual Studio に切り替えます。
2. 開く、 **Index.cshtml**にあるファイル、**ビュー** | **ホーム**フォルダー。
3. 置換、  **&lt;!--TODO: ここでオプションを追加する&gt;** コメントを次のコード、およびキーを押して**タブ**。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. コードは、HTML に拡張する必要があります。

    ![HTML を拡張](visual-studio-2013-web-tools/_static/image6.png "HTML の展開")

    *展開された HTML*

    > [!NOTE]
    > Zen コーディングの構文の詳細については、次を参照してください。[記事](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)します。
5. をクリックして、**リンクされたブラウザーを更新**両方のブラウザーを更新するボタンをクリックします。

    ![リンクされたブラウザーを更新](visual-studio-2013-web-tools/_static/image7.png "リンクされたブラウザーを更新")

    *リンクされたブラウザーを更新します。*

    ![Internet Explorer - ページは 4 つのボタンで更新](visual-studio-2013-web-tools/_static/image8.png "4 つのボタンで Internet Explorer - ページの更新")

    *4 つのボタンで Internet Explorer - ページの更新*

    ![Google Chrome のページは 4 つのボタンで更新](visual-studio-2013-web-tools/_static/image9.png "4 つのボタンで Google Chrome のページの更新")

    *Google Chrome の 4 つのボタンのページの更新*
6. Visual Studio に切り替えます。
7. ページにボタンを追加したが、テンプレートの質問を追加する必要があります。 これを行うと呼ばれる Web Essentials の新機能を使用します**Lorem Ipsum ジェネレーター**します。 検索、 **div**を持つ要素、**クラス**属性**前面**します。
8. 最初の子要素として次のコードを追加、 **div**、キーを押します**タブ**します。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. コードは、HTML に拡張する必要があります。

    ![Lorem Ipsum の自動生成された](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum の自動生成")

    *Lorem Ipsum の自動生成*

    > [!NOTE]
    > Zen コーディングの一部として、HTML エディターで直接 Lorem Ipsum のコードを生成できます。 入力するだけ**lorem**ヒットと**タブ**し、30 Lorem Ipsum のテキストが挿入されます。 たとえば、 *lorem10* Lorem Ipsum の 10 単語を挿入します。
10. もう 1 つの新しい機能と呼ばれる Web Essentials を使用して、質問の上部にあるロゴを追加する**Lorem ピクセル ジェネレーター**します。 最初の子要素として次のコードを追加、 **div**を持つ要素**コンテナー**として**クラス**値、およびキーを押して**タブ**します。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. コードを HTML に展開する必要があります。

    ![Lorem ピクセルの自動生成された](visual-studio-2013-web-tools/_static/image11.png "Lorem ピクセルの自動生成")

    *Lorem ピクセルの自動生成*

    > [!NOTE]
    > Zen コーディングの一部として、HTML エディターで直接 Lorem ピクセルのコードを生成することもできます。 入力**pix の動物 200 x 200**ヒットと**タブ**と**img**動物の 200 x 200 イメージにタグが挿入されます。 詳細についてを参照してください[Lorem ピクセル](http://www.lorempixel.com)します。
12. をクリックして、**リンクされたブラウザーを更新**両方のブラウザーを更新するボタンをクリックします。

    ![Internet Explorer - 自動生成されたイメージとテキスト](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - 自動生成されたイメージとテキスト")

    *Internet Explorer - 自動生成されたイメージとテキスト*

    ![Google Chrome の自動生成されたイメージとテキスト](visual-studio-2013-web-tools/_static/image13.png "Google Chrome の自動生成されたイメージとテキスト")

    *Google Chrome の自動生成されたイメージとテキスト*

    > [!NOTE]
    > コード スニペットを追加するときに、イメージがランダムに選択、ために、ブラウザーに表示されるイメージが異なる場合があります。
13. ブラウザーを閉じないでください。 次のタスクでは、それらを使用します。

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>タスク 3 - スタイル プロパティを更新しています

このタスクでは、ブラウザー リンクを使用して**検査モード**特定の DOM 要素が生成される場所の正確な場所を検出し、Web で提供されるカラー ピッカーを使用してその要素の色のプロパティを更新する機能Essentials。

1. Internet Explorer のブラウザーでキーを押して**CTRL** + **ALT** + **は**検査モードを有効にします。
2. 薄い青の枠線の上にポインターを移動し をクリックします。

    ![薄い青の枠線にポインターを移動](visual-studio-2013-web-tools/_static/image14.png "明るい青色の枠線にポインターを移動")

    *薄い青の枠線にポインターを移動*
3. Visual Studio に切り替えます。 Visual Studio の HTML エディターで、ブラウザーで選択した HTML 要素を選択することも注目してください。

    ![Visual Studio の HTML エディターで選択されている HTML 要素](visual-studio-2013-web-tools/_static/image15.png "Visual Studio の HTML エディターで選択されている HTML 要素")

    *Visual Studio の HTML エディターで選択されている HTML 要素*
4. 今すぐ更新する、**前面**選択した要素のスタイルを変更するために CSS クラス。 これを行うには、キーを押します**CTRL** + **、** を開く、**移動**検索ボックス。 型**site.css**キーを押します**ENTER**ファイルを開きます。

    ![Site.css ファイルを開く](visual-studio-2013-web-tools/_static/image16.png "Site.css ファイルを開いています")

    *Site.css ファイルを開いています*
5. キーを押して**CTRL** + **F**と種類 **.flip コンテナー .front** CSS セレクターを検索します。
6. カラー ピッカーを開きますクラスの枠線のプロパティで明るい青い四角形をクリックします。

    ![カラー ピッカーを開いて](visual-studio-2013-web-tools/_static/image17.png "カラー ピッカーを開く")

    *カラー ピッカーを開く*
7. シェブロンをクリックして、カラー ピッカーを展開し、新しい色を選択します。

    ![カラー ピッカーの展開](visual-studio-2013-web-tools/_static/image18.png "カラー ピッカーの展開")

    *カラー ピッカーの展開*
8. キーを押して**CTRL** + **ALT** + **ENTER**リンクされたブラウザーを更新します。
9. Internet Explorer に切り替えるし、罫線の色がどのように変更されたかに注意してください。

    ![Internet Explorer - 更新された境界線の色](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - 更新された境界線の色")

    *Internet Explorer - 更新された境界線の色*
10. Google Chrome に切り替えるし、罫線の色がどのように変更されたかに注意してください。

    ![Google Chrome - 更新された境界線の色](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - 更新された境界線の色")

    *Google Chrome - 更新された境界線の色*
11. Visual Studio に切り替えます。
12. 末尾に移動して、 **Site.css**ファイルとキーを押して**CTRL** + **F**を検索する、 **.btn**セレクター。
13. 注意、 **webkit の境界線の radius**プロパティは、緑色の下線が引かれます。

    ![btn セレクターの webkit の境界線の半径プロパティ](visual-studio-2013-web-tools/_static/image21.png "btn セレクターの webkit の境界線の半径プロパティ")

    *btn セレクターの webkit の境界線の半径プロパティ*
14. キャレットを置き、 **webkit-罫線-radius**プロパティ。 青い線は、プロパティの最初の単語の最初の文字の下に表示する必要があります。 これは、**スマート タグ**します。
15. キーを押して**CTRL** + **します。** 推奨事項 メニューを開き、をクリックする**標準プロパティ (radius 境界線) が不足している追加**します。

    ![追加の標準プロパティの提案が不足している](visual-studio-2013-web-tools/_static/image22.png "追加の修正候補の標準プロパティがありません")

    *追加の修正候補の標準プロパティがありません。*
16. **境界の半径**プロパティは、CSS 規則を自動的に追加します。

    ![追加標準プロパティが欠落して](visual-studio-2013-web-tools/_static/image23.png "追加標準プロパティが見つかりません")

    *追加標準プロパティが見つかりません*
17. ポインターを置く、**境界の半径**プロパティを表示、**ブラウザー マトリックス ツールヒント**します。 **ブラウザー マトリックス ツールヒント**各ブラウザーで、プロパティの可用性を示します。

    ![ブラウザーのマトリックス ツールヒント](visual-studio-2013-web-tools/_static/image24.png "ブラウザー マトリックス ツールヒント")

    *ブラウザーのマトリックスのツールヒント*
18. 注意の値、**境界の半径**プロパティがまだ下線が引かれました。 警告メッセージが表示される値の上にポインターを移動します。

    ![境界の半径プロパティ値の警告](visual-studio-2013-web-tools/_static/image25.png "境界の半径プロパティ値の警告")

    *境界の半径プロパティ値の警告*
19. 単位を削除、**境界の半径**プロパティ値、ツールヒントで示されているとします。
20. として**境界の半径**コーナーは、削除することができます丸みのある方法の境界を定義するための標準プロパティは、 **webkit-罫線-radius**プロパティと、CSS 規則からの値。
21. キャレットを置き、**ワード ラップ**プロパティとスマート タグは、以下も表示されることがわかります。
22. メニューを開き、をクリックして**不足しているベンダーの仕様を追加**します。

    ![不足しているベンダーの仕様の修正候補を追加](visual-studio-2013-web-tools/_static/image26.png "不足しているベンダーの仕様の修正候補の追加")

    *不足しているベンダーの仕様の修正候補を追加します。*
23. **Ms 折り返し**プロパティは、CSS 規則を自動的に追加します。

    ![ベンダー固有のプロパティが追加](visual-studio-2013-web-tools/_static/image27.png "ベンダー固有のプロパティを追加")

    *ベンダー固有のプロパティを追加*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>タスク 4 - ブラウザーから HTML コードの更新

このタスクでは、ブラウザー リンクを使用して**デザイン モード**ブラウザーからの DOM オブジェクトを編集し、Visual Studio での HTML ソース ファイルに変更を転送する機能。

1. Google Chrome でキーを押して**CTRL** + **ALT** + **D**デザイン モードを有効にします。
2. ポインターを置く、 **Lorem Ipsum dolor sit amet**ラベルを付け、をクリックします。

    ![質問を編集](visual-studio-2013-web-tools/_static/image28.png "質問の編集")

    *質問の編集*
3. カーソルが表示されます。 元のテキストを置換*どのそのような長い質問を書き込むときにしますか?*、しキーを押します**ESC**デザイン モードを終了します。

    ![編集の質問](visual-studio-2013-web-tools/_static/image29.png "質問の編集")

    *質問の編集*
4. Visual Studio に戻り、開いているスイッチ**Index.cshtml**開いていない場合、します。 注意しての内部テキ スト、 **&lt;p&gt;** 要素が更新されました。

    ![HTML ページで更新された質問](visual-studio-2013-web-tools/_static/image30.png "HTML ページに最新の質問")

    *HTML ページで、更新された質問*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>タスク 5 - 変更履歴の SEO に関連する警告

**検索エンジンの最適化**(SEO) とは、結果の検索エンジンの一覧で web サイトのランクを与えるをするプロセスです。 サイトの順位が高いほど、記載されているより一貫した多くの訪問者、サイトは、その検索エンジンから取得されます。 Web Essentials には、HTML を検査する分析ツールが組み込まれています、問題のレポートは検出し、それを修正するアシスタンスを提供します。

1. 移動して、**ビュー**メニューをクリックします**エラー一覧**を開く、**エラー一覧**ウィンドウ。

    ![エラーをビューに一覧表示メニューの](visual-studio-2013-web-tools/_static/image31.png "表示 メニューのエラー一覧")

    *エラーをビューに一覧表示メニュー*
2. 通知を SEO 警告があることに注意してください、 **&lt;メタ&gt;** タグ ページの説明がありません。 これを修正する SEO 警告エントリをダブルクリックします。

    ![エラー一覧ウィンドウ](visual-studio-2013-web-tools/_static/image32.png "エラー一覧ウィンドウ")

    *エラー一覧ウィンドウ*
3. **Web Essentials**ダイアログ ボックスで、をクリックして**はい**説明を挿入する&lt;メタ&gt;タグ。

    ![Web Essentials ダイアログ ボックス](visual-studio-2013-web-tools/_static/image33.png "Web Essentials ダイアログ ボックス")

    *Web Essentials ダイアログ ボックス*
4. 用のエディター **\_Layout.cshtml**開きます、 **&lt;メタ&gt;** にタグが自動的に追加、**ヘッド**のセクション、HTML ファイルです。

    ![自動的に追加されます (_l) ページに Meta タグ](visual-studio-2013-web-tools/_static/image34.png "(_l) ページに自動的に追加のメタ タグ")

    *自動的に追加のメタ タグ\_レイアウト ページ*
5. 値を変更、**コンテンツ**属性を*GeekQuiz*ファイルを保存します。

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>手順 2: をコード スニペットと IntelliSense の利用

Web essentials、余分な機能を持つ、HTML エディターが拡張されました。 この演習では、web アプリケーションの開発に役立つものをいくつかの新しい機能が表示されます。

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>タスク 1 - HTML ドキュメントで IntelliSense の使用

このタスクに表示される最初の新機能と呼びます**動的 IntelliSense**します。 動的 IntelliSense では、その他のタグとは使用可能な id を推論する属性を読み取ります。

このタスクでは、ラベルと、入力フィールドが含まれた新しい HTML フォーム要素を作成します。 追加し、**の**属性に、入力に連結するラベルをスコープ内の入力の id に基づく IntelliSense による候補が表示されます。

1. 開いている**Visual Studio Express 2013 for Web**と**Begin.sln**ソリューション、**ソース/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/開始**フォルダー。 または、前の手順で取得したソリューションを続行できます。
2. **ソリューション エクスプ ローラー**、オープン、 **Index.cshtml**にあるファイル、**ビュー** | **ホーム**フォルダー。
3. 内で、次の形式を追加、 **&lt;セクション&gt;** 要素。

    (コード スニペット - *VisualStudio2013WebTooling* - *Ex2* - *フォーム*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. 入力タグは、フィールドのいくつかの説明のラベルによって先行されなければなりません。 入力タグの前に次のラベルを追加します。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **の** の属性を **&lt;ラベル&gt;** にラベルをフォーム要素がバインドされているを指定します。 属性の値は、関連する要素の id と同じでなければなりません。 追加、 **の** 属性を **&lt;ラベル&gt;** 要素。 次の図に示すように、&quot;名前&quot;値が表示されます、[IntelliSense] ボックスで、同じスコープ内の要素の id に基づく (囲んでいる **&lt;フォーム&gt;**)。

    ![IntelliSense で、id を示す](visual-studio-2013-web-tools/_static/image35.png "IntelliSense 内の id を表示")

    *IntelliSense 内の id を表示*
6. 最近追加された削除 **&lt;フォーム&gt;** 要素とそのコンテンツ。

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>タスク 2 - を使用して HTML コード スニペット

HTML5 では、25 を超える新しいセマンティック タグが導入されました。 Visual Studio が IntelliSense でこれらのタグのサポートに既に存在しますが、Visual Studio 2013 では、高速化し、新しいコード スニペットを追加することでマークアップを記述しやすくなります。 します。 これらのタグが複雑でない場合、これらにはの正しいコーデック フォールバックを追加するなど、いくつかの小さな細部について、*オーディオ*タグ。 このタスクでは、オーディオ タグの HTML コード スニペットが表示されます。

1. **Index.cshtml**ファイルに、入力 **&lt;aud**内で、 **&lt;セクション&gt;** 要素の次の図に示すようにします。

    ![要素を挿入するオーディオ](visual-studio-2013-web-tools/_static/image36.png "オーディオの要素を挿入")

    *オーディオの要素を挿入*
2. キーを押して**タブ**2 回ページで、次のコードを追加する方法に注意してください。 および上にカーソルが置かれます、 **src**最初のソースの属性です。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > キーを押して、**タブ**キー 2 回、コード スニペットが挿入されます。 オーディオのスニペットの標準的な使用法を示しています、*オーディオ*サポートの強化を 2 つのソース ファイルのタグ。
3. 2 番目の行を削除し、WebCampsTV Katana 表示するには、次のリンクで最初の行のソースを更新: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3)します。 結果として得られるコードは、以下に示します。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > ソース ファイル*KatanaProject.mp3*例として使用されます。 たい場合は、別のソースを使用できます。
4. キーを押して**CTRL** + **S**ファイルを保存します。
5. キーを押して**CTRL** + **F5**アプリケーションを起動します。
6. オーディオ プレーヤーをアプリケーションに追加されたことに注意してください。

    ![Internet Explorer でのオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image37.png "Internet Explorer でのオーディオ プレーヤー")

    *Internet Explorer でのオーディオ プレーヤー*

    ![Google Chrome でオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image38.png "Google Chrome でオーディオ プレーヤー")

    *Google Chrome でオーディオ プレーヤー*
7. ブラウザーを閉じないでください。 次のタスクでは、それらを使用します。

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>タスク 3 - JavaScript のドキュメントで IntelliSense の使用

Web Essentials 2013 では、スタイル シートと HTML ページは、Id 名とクラス名の一覧を生成します。 このタスクでは、それらのリストが Web Essentials 2013 での JavaScript IntelliSense のサポートを改善する方法を説明します。

1. **Index.cshtml**ファイルを定義する次のコードを追加、**スクリプト**JavaScript コードのタグ。

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. 内の次のコードを追加、**スクリプト**準備完了コールバック関数を定義するタグ。

    (コード スニペット - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. キャレットを置き、**スクリプト**タグとキーを押して**CTRL** + **します。** 提案メニューを開きます。
4. クリックして**ファイルに抽出**します。

    ![ファイルの提案に抽出する JavaScript](visual-studio-2013-web-tools/_static/image39.png "JavaScript ファイルの提案に抽出します。")

    *JavaScript は、ファイルの提案に抽出します。*
5. **名前を付けて保存**ウィンドウで、**スクリプト**フォルダー、ファイルの名前**init.js** をクリック**保存**です。

    ![名前を付けて保存ウィンドウ](visual-studio-2013-web-tools/_static/image40.png "名前を付けて保存ウィンドウ")

    *名前を付けて保存ウィンドウ*

    > [!NOTE]
    > **Init.js**ファイルが作成され、スクリプトの内容が、ファイルに移動します。
    > 
    > ![含まれるコンテンツと共に作成 Init.js ファイル](visual-studio-2013-web-tools/_static/image41.png "Init.js ファイルが含まれているコンテンツの作成")
    > 
    > *含まれるコンテンツと共に作成 Init.js ファイル*
6. 開く、 **Index.cshtml**ファイルを開き、script タグをへの参照に置き換えられたことを確認、 **init.js**ファイル。

    ![Html の参照を Init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js html リファレンス")

    *Init.js html リファレンス*
7. 移動して、**ソリューション エクスプ ローラー**ことに注意して、 **init.js**ファイルは、ソリューションに自動的に含まれていた。

    ![ソリューションに含まれる Init.js ファイル](visual-studio-2013-web-tools/_static/image43.png "Init.js ファイルがソリューションに含まれる")

    *ソリューションに含まれる Init.js ファイル*
8. 戻り、 **init.js**更新ファイルを**準備**コールバック関数。
9. 渡される関数コールバック定義内で*準備*、特定のクラスの属性によってすべての要素を取得する次のコードを追加します。

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. キーを押して**CTRL** + **領域**内の引用符の間、 **getElementsByClassName**関数呼び出し。

    ![GetElementsByClassName 関数の IntelliSense を示す](visual-studio-2013-web-tools/_static/image44.png "getElementsByClassName 関数を示す IntelliSense")

    *GetElementsByClassName 関数の IntelliSense の表示*

    > [!NOTE]
    > プロジェクトのスタイル シートで定義されたクラスが IntelliSense に表示されることを確認します。
11. 次のコードを使用して作成する行を置き換えます。

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. 後ろにカーソルを置きます**au**で引用符で囲んで、 **getElementsByTagName**関数およびキーを押して**CTRL** + **領域**.

    ![GetElementByTagName メソッドの IntelliSense を示す](visual-studio-2013-web-tools/_static/image45.png "getElementByTagName メソッド用の IntelliSense の表示")

    *GetElementsByTagName メソッドを示す IntelliSense*
13. 選択**&quot;オーディオ&quot;** キーを押して、一覧から**ENTER**します。 結果を次の例に示します。

    ![オーディオの要素を取得する](visual-studio-2013-web-tools/_static/image46.png "オーディオ要素を取得します。")

    *オーディオの要素を取得します。*
14. **ソリューション エクスプ ローラー**を右クリックし、 **init.js**ファイル、**スクリプト**フォルダーと選択**アンミニファイ JavaScript ファイル**から、**Web Essentials**メニュー。

    ![JavaScript ファイルを縮小](visual-studio-2013-web-tools/_static/image47.png "アンミニファイ JavaScript ファイル")

    *JavaScript ファイルを縮小します。*
15. ソース ファイルの変更をクリックすると、自動縮小を有効にするように求められたら**はい**です。

    ![自動縮小の警告を有効にする](visual-studio-2013-web-tools/_static/image48.png "自動縮小の警告を有効にします。")

    *自動縮小の警告を有効にします。*

    > [!NOTE]
    > **Init.min.js**が作成され、追加の依存関係として、 **init.js**ファイル。
    > 
    > ![作成した Init.min.js ファイル](visual-studio-2013-web-tools/_static/image49.png "Init.min.js ファイルの作成")
    > 
    > *Init.min.js ファイルの作成*
16. 開く、 **init.min.js**ファイルを開き、ファイルを縮小ことに注意してください。

    ![ファイルの内容を Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js ファイルの内容")

    *Init.min.js ファイルの内容*
17. **Init.js**ファイルに、次の次のコードを追加、 **getElementsByTagName**関数呼び出しにオーディオのすべての要素を再生します。

    (コード スニペット - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. クリックして**CTRL** + **S**ファイルを保存します。 縮小されたファイルが既に開かれているために、ソース エディター以外でファイルが変更されたことを通知するダイアログ ボックスが表示されます。 **[はい]** をクリックします。

    ![Microsoft Visual Studio 警告](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio 警告")

    *Microsoft Visual Studio 警告*
19. 戻り、 **init.min.js**ファイルを新しいコードで、ファイルが更新されたことを確認します。

    ![更新された Init.min.js ファイル](visual-studio-2013-web-tools/_static/image52.png "Init.min.js ファイルが更新されました")

    *Init.min.js ファイルが更新されました*
20. をクリックして、**ブラウザー リンクの更新**ボタンをクリックします。
21. 両方のブラウザーが更新されると、前のタスクで学習したオーディオ プレーヤーは自動的に再生を開始します。

    ![ビューに含まれるオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image53.png "ビューに含まれるオーディオ プレーヤー")

    *ビューに含まれるオーディオ プレーヤー*

* * *

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボについて説明した方法。

- 豊富な HTML5 コード スニペットや Zen コーディングなど、Web Essentials に含まれる HTML エディターの新機能を使用します。
- カラー ピッカーやブラウザー マトリックス ツールヒントなど、Web Essentials に含まれる CSS エディターの新機能を使用します。
- すべての HTML 要素を抽出するファイルと IntelliSense などの Web Essentials に含まれる JavaScript エディターの新機能を使用します。
- ブラウザーとブラウザー リンクを使用して Visual Studio 間の Exchange データ
