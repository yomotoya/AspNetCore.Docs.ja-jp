---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: "ハンズ オン ラボ: Visual Studio 2013 の Web Tools |Microsoft ドキュメント"
author: rick-anderson
description: "Visual Studio とは、優れた開発環境です。NET ベースの Windows と web プロジェクトです。 簡単に使用できる強力なテキスト エディターが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>ハンズ オン ラボ: Visual Studio 2013 の Web ツール
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> Visual Studio とは、優れた開発環境です。NET ベースの Windows と web プロジェクトです。 これには、プロジェクトなしスタンドアロン ファイルを編集して簡単に使用できる強力なテキスト エディターが含まれます。
> 
> Visual Studio は、各ファイルを編集すると、フル機能の解析ツリーを保持します。 これにより、Visual Studio をはるかに高速でより快適な開発作業をするときにこれまでにないオートコンプリートとドキュメントに基づくアクションを提供します。 これらの機能は、HTML および CSS のドキュメントで特に強力です。
> 
> この電源のすべても、ニーズに合わせて編集者には、強力な新機能を拡張する簡素化する、拡張機能の使用可能なできます。 Web Essentials は、(ほとんどの場合) web 関連の機能強化 Visual Studio のコレクションです。 (CSS) のでは特に新しい IntelliSense 入力候補、Browser Link の新機能、および自動の多くが含まれている JSHint の JavaScript ファイル、HTML、CSS、および最新の web 開発に不可欠なその他の多くの機能に対する新しい警告します。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)です。


<a id="Overview"></a>
## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- 豊富な HTML5 のコード スニペットと全角コーディングなど Web Essentials に含まれる HTML エディターの新機能を使用します。
- カラー ピッカーやブラウザー マトリックス ツールヒントなどの Web Essentials に含まれる CSS エディターの新機能を使用します。
- すべての HTML 要素の IntelliSense およびファイルへの展開などの Web Essentials に含まれる JavaScript エディターの新機能を使用します。
- ブラウザーとブラウザー リンクを使用して Visual Studio の間での Exchange データ

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/)以上
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

このハンズオン ラボでは、演習を実行するのには、最初に、環境を設定する必要があります。

1. Windows エクスプ ローラー ウィンドウを開き、ラボを参照**ソース**フォルダーです。
2. 右クリック**Setup.cmd**選択**管理者として実行**を環境を構成して、このラボ用の Visual Studio のコード スニペットをインストールするセットアップ プロセスを起動します。
3. ユーザー アカウント制御 ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。

> [!NOTE]
> セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認してください。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>コード スニペットを使用します。

ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。 便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを回避する Visual Studio 2013 内からアクセスできるとして提供されます。

> [!NOTE]
> 各演習にある開始ソリューションが組み込まれた、**開始**個別に各手順をたどることによってこの作業のフォルダーです。 実習では、追加のコード スニペットがこれらのソリューションの開始から不足しており、演習を完了するまで動作しない可能性がありますに注意してください。 演習では、ソース コード、内部も表示されます、**終了**を対応する手順の手順を実行した結果のコードの Visual Studio ソリューションを含むフォルダー。 このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の実習が含まれます。

1. [Browser Link と Web Essentials の使用](#Exercise1)
2. [コード スニペットと IntelliSense の利用](#Exercise2)

> [!NOTE]
> Visual Studio を初めて起動するときに定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルと一致するものではまた、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペットでは、およびダイアログ ボックスのオプションを判断します。 このラボの手順を使用する場合は、Visual Studio での特定のタスクを実行に必要な操作を記述する、**全般的な開発設定**コレクション。 開発環境に合わせてさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Browser Link と Web Essentials 演習 1: を使用します。

**Web Essentials**は、さまざまな最新の web 開発では、はるかに高速でより快適な web 開発作業を行うに焦点を合わせた便利な機能を追加する Visual Studio 拡張機能です。 Visual Studio の拡張機能ギャラリーから Web Essentials をインストールすることができます。

**Browser Link** web アプリケーションと Visual Studio の間でデータを交換するには、Visual Studio IDE と任意の開いているブラウザー間のチャネルを提供する Visual Studio 2013 に含まれる新機能です。 Web Essentials は、DOM オブジェクト モデルと、ブラウザーから直接 web ページの CSS スタイルを操作するツールで Browser Link を拡張します。

この演習では調査でサポートされる機能のいくつか**Web Essentials**と**Browser Link**クイズの単純なページを強化するためにします。

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>タスク 1 - 複数のブラウザーでプロジェクトを実行します。

このタスクでは、クロス ブラウザー テスト用に便利ですが、一度に複数のブラウザーで実行する web アプリケーションを構成します。

1. 開いている**Microsoft Visual Studio**です。
2. **ファイル**メニューの **開く |プロジェクト/ソリューションしています.**を見つけて**Ex1 WorkingwithBrowserLinkandWebEssentials\Begin**で、**ソース**ラボ (C:\WebCampsTK\HOL\VSWebTooling\Source) のフォルダーです。 選択**Begin.sln**  をクリック**開く**です。
3. Visual Studio ツールバーで、[ブラウザー] メニューを展開し、選択**を参照しています.**.

    ![メニュー オプション browse With](visual-studio-2013-web-tools/_static/image1.png "ブラウザー メニュー... 参照")

    *Browse With メニュー オプション*
4. **Browse With**  ダイアログ ボックスの両方をオン**Google Chrome**と**Internet Explorer**押したまま、 **CTRL**キーおよびをクリックして**既定値として設定**です。

    ![ダイアログ ボックスで [参照]](visual-studio-2013-web-tools/_static/image2.png " ダイアログ ボックスで [参照]")

    *複数の既定ブラウザーを選択します。*
5. 既定のブラウザーとして Google Chrome と Internet Explorer の両方が表示されます。 をクリックして**キャンセル** ダイアログ ボックスを閉じます。

    ![Google Chrome と既定のブラウザーとして Internet Explorer](visual-studio-2013-web-tools/_static/image3.png "Google Chrome と Internet Explorer の既定のブラウザー")

    *Google Chrome と既定のブラウザーとして Internet Explorer*

    > [!NOTE]
    > 既定のブラウザーを構成した後、**複数のブラウザー**ブラウザー メニューのオプションを選択します。
    > 
    > ![複数のブラウザー](visual-studio-2013-web-tools/_static/image4.png "複数のブラウザー")
6. キーを押して**ctrl キーを押し** + **f5 キーを押して**デバッグを使わないアプリケーションを実行します。
7. 両方のブラウザー ウィンドウを開く場合は、同時に両方のブラウザーで、更新プログラムを確認するために上、他のうちの 1 つを配置します。 ブラウザーには、明るい青色の四角形と web ページを表示する必要があります。

    ![上の 1 つのブラウザーを配置する](visual-studio-2013-web-tools/_static/image5.png "上、他の 1 つのブラウザーを配置します。")

    *上下に並べて配置する 1 つのブラウザー*
8. ブラウザーを閉じないでください。 次のタスクでは、それらを使用します。

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>タスク 2 - HTML 要素を作成するコードを使用して全角

**全角コーディング**エディターは、高速な HTML、XML、XSL (またはその他の構造化されたコード形式) 用のプラグインのコーディングおよび編集します。 このプラグインのコアとは、HTML コードに - CSS セレクターのように式を展開することができますが、強力な省略形エンジンです。 全角のコーディングは、HTML、CSS を使用するスタイルのセレクターの構文を記述する高速な方法です。

この演習では機能を使用する、全角コーディング Web Essentials によって提供される質問のオプションを表す HTML ボタンを生成します。

1. Visual Studio に切り替えます。
2. 開く、 **Index.cshtml**にあるファイル、**ビュー** | **ホーム**フォルダーです。
3. 置換、  **&lt;!--TODO: ここでのオプションを追加&gt;**にコメントを次のコード、およびキーを押して**タブ**です。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. コードは、HTML を拡張する必要があります。

    ![HTML 拡張](visual-studio-2013-web-tools/_static/image6.png "HTML の展開")

    *展開された HTML*

    > [!NOTE]
    > 全角のコーディング構文の詳細については、次を参照してください。[記事](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)です。
5. クリックして、**リンクされているブラウザーを更新**両方のブラウザーを更新するボタンをクリックします。

    ![リンクされているブラウザーを更新](visual-studio-2013-web-tools/_static/image7.png "リンクされているブラウザーを更新")

    *リンクされているブラウザーを更新します。*

    ![Internet Explorer のページは 4 つのボタンで更新](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer のページは 4 つのボタンで更新")

    *Internet Explorer のページは 4 つのボタンで更新*

    ![Google Chrome のページは 4 つのボタンで更新](visual-studio-2013-web-tools/_static/image9.png "Google Chrome のページは 4 つのボタンで更新")

    *Google Chrome のページは 4 つのボタンで更新*
6. Visual Studio に切り替えます。
7. ページに、ボタンを追加したが、テンプレート質問を追加する必要があります。 これを行うで使用する新しい機能と呼ばれる Web Essentials **Lorem Ipsum ジェネレーター**です。 検索、 **div**を持つ要素、**クラス**属性**前面**です。
8. 最初の子要素として次のコードを追加、 **div**、キーを押します**タブ**です。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. コードは、HTML を拡張する必要があります。

    ![Lorem Ipsum 自動生成された](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum 自動生成されました。")

    *Lorem Ipsum 自動生成されました。*

    > [!NOTE]
    > 全角のコーディングの一環として、HTML エディターで直接 Lorem Ipsum コードを生成できます。 入力するだけ**lorem**ヒットと**タブ**し、30 Lorem Ipsum テキストが挿入されます。 例。 *lorem10* 10 Lorem Ipsum 単語を挿入します。
10. Web essentials と呼ばれる別の新機能を使用して、質問の上部にロゴを追加する**Lorem ピクセル ジェネレーター**です。 最初の子要素として次のコードを追加、 **div**を持つ要素**コンテナー**として**クラス**値、およびキーを押して**タブ**です。

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. コードは、HTML を展開します。

    ![Lorem ピクセル自動生成された](visual-studio-2013-web-tools/_static/image11.png "Lorem ピクセルの自動生成")

    *Lorem ピクセルの自動生成*

    > [!NOTE]
    > 全角のコーディングの一環として、HTML エディターで直接 Lorem ピクセルのコードを生成することもできます。 入力するだけ**pix 200 x 200-動物**とヒット**タブ**と**img**動物の 200 x 200 イメージからのタグが挿入されます。 詳細についてを参照してください[Lorem ピクセル](http://www.lorempixel.com)です。
12. クリックして、**リンクされているブラウザーを更新**両方のブラウザーを更新するボタンをクリックします。

    ![Internet Explorer の自動生成されたイメージとテキスト](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer の自動生成されたイメージとテキスト")

    *Internet Explorer の自動生成されたイメージとテキスト*

    ![Google Chrome の自動生成されたイメージとテキスト](visual-studio-2013-web-tools/_static/image13.png "Google Chrome の自動生成されたイメージとテキスト")

    *Google Chrome の自動生成されたイメージとテキスト*

    > [!NOTE]
    > コード スニペットを追加するときに、イメージをランダムに選択、ためのブラウザーで表示されるイメージが異なる場合があります。
13. ブラウザーを閉じないでください。 次のタスクでは、それらを使用します。

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>タスク 3 - スタイル プロパティの更新

このタスクでは、ブラウザーのリンクを使用して**検査モード**特定の DOM 要素が生成される正確な場所を検出し、Web で提供されるカラー ピッカーを使用してその要素の色のプロパティを更新する機能Essentials です。

1. Internet Explorer ブラウザーでキーを押して**ctrl キーを押し** + **ALT** + **すれば**検査モードを有効にします。
2. 明るい青色の枠線でポインターを移動し、をクリックします。

    ![明るい青色の枠線にポインターを移動](visual-studio-2013-web-tools/_static/image14.png "明るい青色の枠線にポインターを移動")

    *明るい青色の枠線にポインターを移動*
3. Visual Studio に切り替えます。 Visual Studio HTML エディターで、ブラウザーで選択されている HTML 要素を選択するも注目してください。

    ![Visual Studio HTML エディターで選択されている HTML 要素](visual-studio-2013-web-tools/_static/image15.png "Visual Studio HTML エディターで選択されている HTML 要素")

    *Visual Studio HTML エディターで選択されている HTML 要素*
4. 今すぐ更新するが、**前面**選択した要素のスタイルを変更するために CSS クラス。 これを行うには、キーを押します。 **CTRL** + **、**を開くには、**へ移動**検索ボックス。 型**site.css**とキーを押します**ENTER**ファイルを開きます。

    ![Site.css ファイルを開く](visual-studio-2013-web-tools/_static/image16.png "Site.css ファイルを開く")

    *Site.css ファイルを開く*
5. キーを押して**CTRL** + **F**および種類**.flip コンテナー .front** CSS セレクターを検索します。
6. クラスを開くには、カラー ピッカーの枠線のプロパティで明るい青色の四角形をクリックします。

    ![カラー ピッカーを開いて](visual-studio-2013-web-tools/_static/image17.png "カラー ピッカーを開く")

    *カラー ピッカーを開く*
7. シェブロンをクリックして、カラー ピッカーを展開し、新しい色を選択します。

    ![カラー ピッカーを展開する](visual-studio-2013-web-tools/_static/image18.png "カラー ピッカーを展開します。")

    *カラー ピッカーを展開します。*
8. キーを押して**ctrl キーを押し** + **alt キーを押し** + **ENTER**にリンクされているブラウザーを更新します。
9. Internet Explorer に切り替え、罫線の色がどのように変更されたかに注意してください。

    ![Internet Explorer の更新の罫線の色](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer の更新の罫線の色")

    *インターネット エクスプ ローラー - 更新の罫線の色*
10. Google Chrome に切り替え、罫線の色がどのように変更されたかに注意してください。

    ![Google Chrome の更新の罫線の色](visual-studio-2013-web-tools/_static/image20.png "Google Chrome の更新の罫線の色")

    *Google Chrome の更新の罫線の色*
11. Visual Studio に切り替えます。
12. 末尾に移動して、 **Site.css**ファイルとキーを押して**CTRL** + **F**を検索する、 **.btn**セレクター。
13. 注意して、 **- webkit 罫線半径**プロパティは、緑色の下線が引かれます。

    ![btn セレクターの - webkit 罫線半径プロパティ](visual-studio-2013-web-tools/_static/image21.png "btn セレクターの - webkit 罫線半径プロパティ")

    *btn セレクターの - webkit 罫線半径プロパティ*
14. 内のキャレットを配置、 **- webkit 罫線半径**プロパティです。 プロパティの最初の単語の最初の文字の下にある青い線が表示されます。 これは、**スマート タグ**です。
15. キーを押して**CTRL** + **です。** 提案メニューを開き、をクリックする**(境界線 radius) の標準的なプロパティがありません追加**です。

    ![追加の標準的なプロパティの修正案がありません](visual-studio-2013-web-tools/_static/image22.png "追加の標準的なプロパティの修正案がありません")

    *追加の標準的なプロパティの修正案がありません。*
16. **罫線 radius**プロパティは、CSS 規則を自動的に追加します。

    ![標準的なプロパティが追加されたありません](visual-studio-2013-web-tools/_static/image23.png "追加標準的なプロパティがありません")

    *追加する標準的なプロパティがありません。*
17. ポインターを移動、**罫線 radius**プロパティを表示、**ブラウザー マトリックス ツールヒント**です。 **ブラウザー マトリックス ツールヒント**各ブラウザーで、プロパティの可用性を示します。

    ![ブラウザーのマトリックス ツールヒント](visual-studio-2013-web-tools/_static/image24.png "ブラウザー マトリックスのツールヒント")

    *ブラウザーのマトリックスのツールヒント*
18. 注意しての値、**罫線 radius**プロパティがまだ下線付きです。 警告メッセージを表示する値を超える、ポインターを移動します。

    ![罫線は、radius プロパティ値の警告](visual-studio-2013-web-tools/_static/image25.png "border-radius プロパティ値の警告")

    *罫線は、radius プロパティ値の警告*
19. ユニットを削除して、**罫線 radius**ツールヒントによって指定されたプロパティの値。
20. として**罫線 radius**コーナーは、削除することができます丸みのある方法の境界を定義するための標準的なプロパティ、 **- webkit 罫線半径**プロパティと、CSS 規則からの値。
21. 内のキャレットを配置、**ワード ラップ**プロパティとがスマート タグは、以下も表示されます。
22. メニューを開き、をクリックして**不足しているベンダー固有の追加**です。

    ![不足している仕入先の具体的な候補を追加](visual-studio-2013-web-tools/_static/image26.png "不足している仕入先の具体的な修正候補の追加")

    *不足している仕入先の具体的な修正候補を追加します。*
23. **Ms 折り返し**プロパティは、CSS 規則を自動的に追加します。

    ![ベンダー固有のプロパティが追加](visual-studio-2013-web-tools/_static/image27.png "ベンダー固有のプロパティを追加")

    *ベンダー固有のプロパティを追加*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>タスク 4 - ブラウザーからの HTML コードの更新

このタスクでは、ブラウザーのリンクを使用して**デザイン モード**ブラウザーからの DOM オブジェクトを編集し、Visual Studio での HTML ソース ファイルに変更を転送する機能。

1. Google Chrome でキーを押して**CTRL** + **ALT** + **D**デザイン モードを有効にします。
2. ポインターを移動、 **Lorem Ipsum dolor sit amet**にラベルを付けるし、をクリックします。

    ![質問を編集](visual-studio-2013-web-tools/_static/image28.png "質問の編集")

    *質問の編集*
3. カーソルが表示されます。 元のテキストを置換*どのそのような長い質問を書き込むときにしますか?*、キーを押します**ESC**デザイン モードを終了します。

    ![編集質問](visual-studio-2013-web-tools/_static/image29.png "質問の編集")

    *質問の編集*
4. Visual Studio に戻り、開いているスイッチ**Index.cshtml**開いていない場合、します。 注意しての内部テキ スト、  **&lt;p&gt;** 要素が更新されました。

    ![HTML ページに更新された質問](visual-studio-2013-web-tools/_static/image30.png "HTML ページに最新の質問")

    *HTML ページに更新された質問*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>タスク 5 - 変更履歴の SEO に関連する警告

**検索エンジン最適化**(SEO) は、結果の検索エンジンの一覧に高いランクが web サイトを行うプロセスです。 サイトが順位付けが高いほどより常に表示されている複数訪問者サイトは、その検索エンジンから取得されます。 Web Essentials には、HTML を調査する分析ツールが組み込まれており、レポートの問題を見つけて対処支援を提供します。

1. 移動して、**ビュー**メニューをクリックして**エラー一覧**を開くには、**エラー一覧**ウィンドウです。

    ![エラーをビューに一覧表示] メニューの [](visual-studio-2013-web-tools/_static/image31.png "表示] メニューの [エラー一覧")

    *エラーをビューに一覧表示 メニュー*
2. 通知を SEO 警告があることに注意してください、 **&lt;メタ&gt;**タグ ページの説明がありません。 これを修正する SEO 警告エントリをダブルクリックします。

    ![エラー一覧 ウィンドウ](visual-studio-2013-web-tools/_static/image32.png "エラー一覧 ウィンドウ")

    *エラー一覧 ウィンドウ*
3. **Web Essentials**ダイアログ ボックスで、をクリックして**はい**説明を挿入する&lt;メタ&gt;タグ。

    ![Web Essentials ダイアログ ボックス](visual-studio-2013-web-tools/_static/image33.png "Web Essentials ダイアログ ボックス")

    *Web Essentials ダイアログ ボックス*
4. 用のエディター  **\_Layout.cshtml**開きます、 **&lt;メタ&gt;**にタグが自動的に追加、**ヘッド**のセクション、HTML ファイルです。

    ![_Layout ページに自動的に追加のメタ タグ](visual-studio-2013-web-tools/_static/image34.png "_Layout ページに自動的に追加のメタ タグ")

    *自動的に追加のメタ タグ\_レイアウト ページ*
5. 値を変更、**コンテンツ**属性を*GeekQuiz*ファイルを保存します。

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>手順 2: コード スニペットおよび活用 IntelliSense

Web Essentials の追加機能を持つ、HTML エディターが拡張されました。 この演習では、web アプリケーションの開発には便利な一部の新機能が表示されます。

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>タスク 1 - HTML ドキュメントで IntelliSense の使用

このタスクに表示される最初の新機能と呼びます**動的 IntelliSense**です。 動的 IntelliSense では、他のタグとは使用可能な id を推測する属性を読み取ります。

このタスクでは、ラベルと入力フィールドを含む新しい HTML フォーム要素を作成します。 追加してから、**の**属性に、入力にバインドするラベルをスコープ内の入力の id に基づいて、intellisense による候補が表示されます。

1. 開いている**Visual Studio Express 2013 for Web**と**Begin.sln**にソリューションがある、**ソース/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/開始**フォルダーです。 代わりに、前の手順で取得した、ソリューションを続行できます。
2. **ソリューション エクスプ ローラー**を開き、 **Index.cshtml**にあるファイル、**ビュー** | **ホーム**フォルダーです。
3. 内の次の形式を追加、 **&lt;セクション&gt;**要素。

    (コード スニペットの*VisualStudio2013WebTooling* - *Ex2* - *フォーム*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. 入力タグは、フィールドのいくつかの説明ラベルによって先行されなければなりません。 入力タグの前に次のラベルを追加します。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **の**の属性、 **&lt;ラベル&gt;**ラベルをフォーム要素の連結先を指定します。 属性の値は、関連する要素の id と同じにする必要があります。 追加、**の**属性を**&lt;ラベル&gt;**要素。 次の図に示すように、&quot;名前&quot;値がポップアップ、[IntelliSense] ボックスで、同じスコープ内の要素の id に基づいた (囲んでいる**&lt;フォーム&gt;**)。

    ![IntelliSense で id を示す](visual-studio-2013-web-tools/_static/image35.png "IntelliSense での id を表示")

    *IntelliSense での id を表示*
6. 削除、新しく追加された**&lt;フォーム&gt;**要素とそのコンテンツ。

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>作業 2: を使用して HTML コード スニペット

HTML5 では、25 台を超える新しいセマンティック タグが導入されました。 Visual Studio は、これらのタグに対する IntelliSense サポートを既に持ってけれども、Visual Studio 2013 では、高速化し、新しいコード スニペットを追加することでマークアップを記述を簡単になります。 ただし、これらのタグは、複雑で、付属の正しいコーデック フォールバックを追加するなど、いくつかの小さな微妙な*オーディオ*タグ。 このタスクでは、オーディオのタグの HTML コード スニペットが表示されます。

1. **Index.cshtml**ファイルに、入力 **&lt;aud**内、 **&lt;セクション&gt;**要素の次の図に示すようにします。

    ![Audio 要素を挿入する](visual-studio-2013-web-tools/_static/image36.png "audio 要素を挿入します。")

    *Audio 要素を挿入します。*
2. キーを押して**タブ**2 回 ページで、次のコードを追加する方法に注意してください。 および上にカーソルが置かれます、 **src**最初のソースの属性です。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > キーを押して、**タブ**キーを 2 回、コード スニペットを挿入します。 オーディオのスニペットの標準的使用方法を示しています、*オーディオ*サポートの強化の 2 つのソース ファイルでのタグ。
3. 2 番目の行を削除および WebCampsTV Katana を表示する次のリンクを使用して、1 行目のソースの更新: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3)です。 結果のコードは、以下に示します。

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > ソース ファイル*KatanaProject.mp3*を例として使用します。 たい場合は、別のソースを使用することができます。
4. キーを押して**CTRL** + **S**ファイルを保存します。
5. キーを押して**CTRL** + **f5 キーを押して**アプリケーションを起動します。
6. オーディオ プレーヤーがアプリケーションに追加されたことに注意してください。

    ![Internet Explorer でのオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image37.png "Internet Explorer でのオーディオ プレーヤー")

    *Internet Explorer でのオーディオ プレーヤー*

    ![Google Chrome でオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image38.png "Google Chrome でオーディオ プレーヤー")

    *Google Chrome でオーディオ プレーヤー*
7. ブラウザーを閉じないでください。 次のタスクでは、それらを使用します。

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>タスク 3 - JavaScript ドキュメントで IntelliSense の使用

Web Essentials 2013 とスタイル シートおよび HTML ページは、Id 名とクラス名の一覧を生成します。 このタスクでは、それらのリストが Web Essentials 2013 での JavaScript IntelliSense のサポートを向上させる方法を学習します。

1. **Index.cshtml**ファイルに定義する次のコードを追加、**スクリプト**JavaScript コードのタグ。

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. 次のコードを追加、**スクリプト**タグ準備完了コールバック関数を定義します。

    (コード スニペットの*VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. 内のキャレットを配置、**スクリプト**タグとキーを押して**CTRL** + **です。** 提案メニューを開きます。
4. をクリックして**ファイルに抽出**です。

    ![ファイルの修正案に JavaScript が抽出](visual-studio-2013-web-tools/_static/image39.png "ファイル修正案を JavaScript の抽出")

    *ファイルの修正案を JavaScript を抽出します。*
5. **名前を付けて保存**ウィンドウで、**スクリプト**フォルダー、ファイルの名前**init.js**  をクリック**保存**です。

    ![名前を付けて保存ウィンドウ](visual-studio-2013-web-tools/_static/image40.png "名前を付けて保存 ウィンドウ")

    *名前を付けて保存 ウィンドウ*

    > [!NOTE]
    > **Init.js**ファイルが作成され、スクリプトの内容が、ファイルに移動します。
    > 
    > ![Init.js ファイルが含まれているコンテンツと共に作成](visual-studio-2013-web-tools/_static/image41.png "Init.js ファイルが含まれているコンテンツと共に作成")
    > 
    > *含まれるコンテンツと共に作成 Init.js ファイル*
6. 開く、 **Index.cshtml**ファイルし、スクリプト タグがへの参照に置き換えられますことを確認して、 **init.js**ファイル。

    ![Html の参照を Init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js html の参照")

    *Init.js html の参照*
7. 移動して、**ソリューション エクスプ ローラー**ことを確認し、 **init.js**ファイルは、ソリューションに自動的に含まれていました。

    ![ソリューションに含まれる Init.js ファイル](visual-studio-2013-web-tools/_static/image43.png "Init.js ファイルがソリューションに含まれる")

    *ソリューションに含まれる Init.js ファイル*
8. 戻り、 **init.js**ファイルを更新する、**準備**関数のコールバック。
9. 渡される関数コールバック定義の内部*準備*、特定のクラス属性によってすべての要素を取得する次のコードを追加します。

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. キーを押して**CTRL** + **領域**内の引用符で囲んで、 **getElementsByClassName**関数呼び出しです。

    ![GetElementsByClassName 関数の IntelliSense を示す](visual-studio-2013-web-tools/_static/image44.png "getElementsByClassName 関数用の IntelliSense を表示")

    *GetElementsByClassName 関数の IntelliSense を表示*

    > [!NOTE]
    > IntelliSense が、プロジェクトのスタイル シートで定義されているクラスを示して ことに注意してください。
11. 次のコードで作成した行を置き換えます。

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. 位置の後にカーソルを置く**オーストラリア**で引用符で囲んで、 **getElementsByTagName**関数とキーを押して**ctrl キーを押し** + **領域**.

    ![GetElementByTagName メソッドの IntelliSense を示す](visual-studio-2013-web-tools/_static/image45.png "getElementByTagName メソッド用の IntelliSense を表示")

    *GetElementsByTagName メソッドの IntelliSense を表示*
13. 選択**&quot;オーディオ&quot;**キーを押して、一覧から**ENTER**です。 結果を次の例に示します。

    ![オーディオの要素を取得する](visual-studio-2013-web-tools/_static/image46.png "オーディオ要素を取得します。")

    *オーディオの要素を取得します。*
14. **ソリューション エクスプ ローラー**を右クリックし、 **init.js**ファイルを**スクリプト**フォルダーと選択**JavaScript の縮小ファイル**から、**Web Essentials**メニュー。

    ![JavaScript ファイルを縮小](visual-studio-2013-web-tools/_static/image47.png "JavaScript の縮小ファイル")

    *JavaScript ファイルを縮小します。*
15. ソース ファイルの変更 をクリックすると、自動縮小を有効にするように求められたら**はい**です。

    ![自動縮小の警告を有効にする](visual-studio-2013-web-tools/_static/image48.png "自動縮小の警告を有効にします。")

    *自動縮小の警告を有効にします。*

    > [!NOTE]
    > **Init.min.js**が作成され、a の依存関係として追加されて、 **init.js**ファイル。
    > 
    > ![作成された Init.min.js ファイル](visual-studio-2013-web-tools/_static/image49.png "Init.min.js ファイルの作成")
    > 
    > *作成された Init.min.js ファイル*
16. 開く、 **init.min.js**ファイルし、ファイルを縮小ことに注意してください。

    ![ファイルの内容を Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js ファイルの内容")

    *Init.min.js ファイルの内容*
17. **Init.js**ファイルに次の次のコードを追加、 **getElementsByTagName**関数の呼び出しをすべてのオーディオ要素を再生します。

    (コード スニペットの*VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. をクリックして**CTRL** + **S**ファイルを保存します。 縮小されたファイルは既に開かれているため、ファイルがソース エディターの外部で変更されたことを示すダイアログ ボックスが表示されます。 **[はい]**をクリックします。

    ![Microsoft Visual Studio 警告](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio の警告")

    *Microsoft Visual Studio の警告*
19. 戻り、 **init.min.js**ファイルを新しいコードで、ファイルが更新されたことを確認します。

    ![更新 Init.min.js ファイル](visual-studio-2013-web-tools/_static/image52.png "Init.min.js ファイルが更新されました")

    *Init.min.js ファイルが更新されました*
20. クリックして、**ブラウザー リンク更新**ボタンをクリックします。
21. 両方のブラウザーが更新される前の実習で学習したオーディオ プレーヤーは自動的に再生を開始します。

    ![ビューに含まれるオーディオ プレーヤー](visual-studio-2013-web-tools/_static/image53.png "ビューに含まれるオーディオ プレーヤー")

    *ビューに含まれるオーディオ プレーヤー*

* * *

<a id="Summary"></a>
## <a name="summary"></a>概要

このハンズオン ラボを完了して学習した方法。

- 豊富な HTML5 のコード スニペットと全角コーディングなど Web Essentials に含まれる HTML エディターの新機能を使用します。
- カラー ピッカーやブラウザー マトリックス ツールヒントなどの Web Essentials に含まれる CSS エディターの新機能を使用します。
- すべての HTML 要素の IntelliSense およびファイルへの展開などの Web Essentials に含まれる JavaScript エディターの新機能を使用します。
- ブラウザーとブラウザー リンクを使用して Visual Studio の間での Exchange データ
