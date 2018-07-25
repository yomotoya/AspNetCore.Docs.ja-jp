---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Visual Studio 2012 で Page Inspector の使用 |Microsoft Docs
author: rick-anderson
description: このハンズオン ラボでは、検索して、Visual Studio で Page Inspector で web ページの問題を修正する新しいツールを検出します。 Page Inspector は、新しいツール b.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: ac945a23dc6ef060340320d047f13c8e81057138
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833673"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Visual Studio 2012 で Page Inspector の使用
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> このハンズオン ラボでは、検索して、Visual Studio で Page Inspector で web ページの問題を修正する新しいツールを検出します。
> 
> Page Inspector は、Visual Studio には、ブラウザー診断ツールと、ブラウザー、ASP.NET、およびソース コードの間で統合されたエクスペリエンスを提供する新しいツールです。 Visual Studio IDE 内で直接、web ページ (HTML、Web フォーム、ASP.NET MVC、または Web ページ) を表示し、ソース コードと結果の出力の両方を調べることができます。 Page Inspector を使用すると、web サイトを簡単に分解、迅速に、ゼロからページをビルドおよび問題をすばやく診断できます。
> 
> 最近では ASP.NET MVC と WebForms などの適切なタイミングで柔軟かつスケーラブルな web サイトを作成する Web フレームワークの番号があります。 その一方で、困難に達して IDE がテンプレートに基づくページと動的なコンテンツのデザイナー ビューをサポートしていないため、ページ上の問題を検索します。 そのため、これらの web サイトをユーザーに表示される方法を表示するブラウザーで開く必要です。
> 
> Web 開発者は、外部ツールを使用して、定期的に実行して、ブラウザーでの問題を見つけます。 次に、IDE に戻るし、修復を開始します。 これを行き来アクティビティ IDE、ブラウザー、およびプロファイリング ツールの間で、効率的なことができるし、新規に展開とキャッシュの問題を再現するたびにクリーニングが必要な場合があります。
> 
> Page Inspector は、の機能の組み合わせを使用して両方の長所を統合することによって、クライアント (ブラウザー ツール) とサーバー (ASP.NET およびソース コード) の間の Web 開発のギャップを橋渡しします。
> 
> Page Inspector を使用して、ブラウザーにレンダリングされる HTML マークアップを生成した (サーバー側コードを含む)、ソース ファイルの要素を確認できます。 Page Inspector を使用して、CSS のプロパティと、ブラウザーで直ちに反映された変更を表示する DOM 要素の属性を変更することもできます。
> 
> このハンズオン ラボは、Page Inspector の機能について説明し、それらを使用して Web アプリケーションで問題を修正する方法を説明します。 **このラボには、2 つの手順ようなフローを使用して、さまざまなテクノロジを対象とするにはが含まれています。ASP.NET MVC 開発者の場合は、次の演習 1 つです。WebForms 開発者に従って演習 2 の場合**します。
> 
> このラボでは、拡張機能とソース フォルダーにサンプル Web アプリケーションに軽微な変更を適用することで以前に説明する新機能について説明します。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)します。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- Page Inspector を使用して Web サイトを分解します。
- 検査し、Page Inspector で CSS スタイルの変更のプレビュー
- 検出し、Page Inspector を使用して、web ページで問題を修正

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このラボを完成させるのには、次の項目が必要です。

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。
- Internet Explorer 9 以降

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の演習が含まれます。

1. [手順 1: ASP.NET MVC プロジェクトで Page Inspector の使用](#Exercise1)
2. [手順 2: WebForms プロジェクトで Page Inspector の使用](#Exercise2)

> [!NOTE]
> 各演習は、各演習を他のユーザーとは別にすることができますが、この演習の Begin フォルダーにあるソリューションを伴います。 演習のソース コード内で、対応する手順」の手順を完了に起因するコードを Visual Studio ソリューションを格納した End フォルダーを検索することもされます。 このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。


この演習の所要時間を推定: **30 分**します。

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>手順 1: ASP.NET MVC プロジェクトで Page Inspector の使用

この演習では、プレビュー、およびデバッグする方法について説明します、 **ASP.NET MVC 4**ソリューションを使用して**Page Inspector**します。 まず、簡単な簡単なツールについては、Web のプロセスのデバッグを容易にする機能を実行します。 次に、スタイルの問題を含む web ページで作業します。 Page Inspector を使用して、問題を生成するソース コードを検索し、その修正方法を学習します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>タスク 1 - Page Inspector の調査

このタスクでは、フォト ギャラリーを表示する ASP.NET MVC 4 プロジェクトのコンテキストで Page Inspector を使用する方法を学習します。

1. 開く、**開始**ソリューションがある**ソース/Ex1-MVC4/開始/** フォルダー。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. ソリューション エクスプ ローラーで検索**Index.cshtml**下の表示、 **/ビュー/ホーム**プロジェクト フォルダーで、右クリックし、選択**Page Inspector で表示**します。

    ![Page Inspector 内でプレビューするファイルを選択する](using-page-inspector-in-visual-studio-2012/_static/image1.png "Page Inspector 内でプレビューするファイルを選択します。")

    *Page Inspector 内でプレビューするファイルを選択します。*
3. Page Inspector のウィンドウが表示されます、 */ホーム/インデックス*URL ソースを選択したビューにマップします。

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Page Inspector で最初にお問い合わせください。*

    Page Inspector ツールは、Visual Studio 環境で統合されます。 インスペクターには、強力な HTML プロファイラーと共に、埋め込まれたブラウザーが含まれています。 注意してください、ページの外観を確認するソリューションを実行する必要はありません。

    > [!NOTE]
    > Page Inspector のブラウザーの幅が開かれているページの幅未満の場合は表示されませんページ適切です。 このような場合は、Page Inspector の幅を調整します。
4. をクリックして、**ファイル**Page Inspector 内でのタブ。

    インデックス ページを作成するすべてのソース ファイルが表示されます。 この機能は、部分ビューとテンプレートを使用している場合は特に、ひとめですべての要素を識別するために役立ちます。 開くことができますも、ファイルの各リンクをクリックする場合に注意してください。

    ![[ファイル] タブ](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *[ファイル] タブ*
5. をクリックして、**検査モードの切り替え**タブの左側にあるボタンをクリックします。

    このツールを使用すると、ページの任意の要素を選択し、HTML および Razor コードを参照してください。

    ![トグル検査モード ボタン](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *トグル検査モード ボタン*
6. Page Inspector のブラウザーでページの要素に対するマウス ポインターを移動します。 表示するページの任意の部分にマウス ポインターを移動するときに、要素の型が表示され、対応するソース マークアップまたはコードが Visual Studio エディターで強調表示されます。

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *検査モードの動作*

    > [!NOTE]
    > Page Inspector ウィンドウを最大化しないか、ソース コードを示す プレビュー タブを表示することはできません。 最大化されたときに、Page Inspector 内で要素をクリックすると、選択範囲のソース コードが表示されますが、Page Inspector ウィンドウを非表示になります。

    注意する場合、 **Index.cshtml**ファイルが表示されます、選択した要素を生成するソース コードの部分が強調表示されます。 この機能では、コードにアクセスする直接的および高速の方法を提供する長いソース ファイルの編集が容易になります。

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *要素の検査*
7. クリックして、**検査モードを切り替える**ボタン (![Page Inspector のブラウザーでレンダリングされた HTML コードを表示する HTML タブを選択します.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Page Inspector のブラウザーでレンダリングされた HTML コードを表示する HTML タブを選択します.") ) 、カーソルを無効にします。
8. 選択、 **HTML** Page Inspector のブラウザーでレンダリングされた HTML コードを表示するタブ。
9. HTML マークアップで Koala リンクのリスト アイテムを見つけて選択します。

    コードを選択すると、対応する出力は、ブラウザーでは強調表示に自動的に注目してください。 この機能は、HTML ブロックをページに表示する方法を確認するのに便利です。

    ![ページの HTML 要素を選択する](using-page-inspector-in-visual-studio-2012/_static/image8.png "ページ内の選択の HTML 要素")

    *ページの HTML 要素の選択*
10. をクリックして、**検査モードの切り替え**有効にするボタン*検査モード*ナビゲーション バーをクリックします。 スタイルのウィンドウで、HTML コードの右側に、選択した要素に適用される CSS スタイルを使用して一覧が表示されます。

    > [!NOTE]
    > Page Inspector は開くことも、ヘッダーは、サイトのレイアウトの一部であるため\_Layout.cshtml ファイルとコードのセグメントが影響を強調表示します。

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *スタイルと、選択した要素のソース ファイルを検出します。*
11. 切り替え検査ポインターを有効になっている、おすすめ青いバーの下にマウス ポインターを移動し、半分の円をクリックします。

    ![要素の選択](using-page-inspector-in-visual-studio-2012/_static/image10.png "要素の選択")

    *要素の選択*
12. スタイルのウィンドウで検索、**背景イメージ**] の [、 **.main コンテンツ**グループ。 **オフに**、**背景イメージ**みてください。 ブラウザーで、変更がすぐに反映されますを非表示には、円が表示されます。

    > [!NOTE]
    > ページのインスペクターのスタイル タブに適用する変更は、元のスタイル シートには影響しません。 スタイルをオフにしたり、何度でも、ページの更新後に復元されますが、値を変更できます。

    ![有効にして、CSS スタイルを無効化](using-page-inspector-in-visual-studio-2012/_static/image11.png "の有効化と CSS スタイルを無効にします。")

    *有効にして、CSS スタイルを無効化*
13. をクリックして、'**貴社のロゴ**' 検査モードを使用して、ヘッダーのテキスト。
14. **スタイル** タブで、検索、**フォント サイズ**CSS 属性、 **.site タイトル**グループ。 属性の値をダブルクリックし、2.3 em 値の置換**3 em**、キーを押しますと **」と入力**します。 タイトルが大きいことに注意してください。

    ![Page Inspector 内での CSS 値を変更する](using-page-inspector-in-visual-studio-2012/_static/image12.png "Page Inspector 内で値を変更する CSS")

    *Page Inspector 内での CSS 値を変更します。*
15. をクリックして、**トレース スタイル** タブで、Page Inspector の右側のウィンドウにあります。 これは、属性の名前順で、選択範囲に適用されるすべてのスタイルを表示する別の方法です。

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *選択した要素の CSS スタイルのトレース*
16. Page Inspector のもう 1 つの機能は、レイアウト ペインです。 検査モードを使用して、ナビゲーション バーを選択し、クリックして、**レイアウト**右側のウィンドウ タブ。 選択した要素の正確なサイズと、オフセット、余白、パディングおよび境界線のサイズが表示されます。 このビューから値を変更することも注目してください。

    ![Page Inspector 内で要素のレイアウト](using-page-inspector-in-visual-studio-2012/_static/image14.png "Page Inspector 内で要素のレイアウト")

    *Page Inspector 内で要素のレイアウト*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>タスク 2 - 検索して、フォト ギャラリーでスタイルの問題を修正

Visual Studio の以前のバージョンとの Web ページの問題を診断するにはでしょうか。 なじみの web デバッグ ツール、Internet Explorer 開発者ツールや Firebug など、Visual Studio IDE の外部で実行するとしています。 ブラウザーでのみ認識、HTML スクリプトとスタイル中、基になるフレームワークには、レンダリングされる HTML が生成されます。 そのため、多くの場合、サイト全体のように web ページの外観を確認するを展開する必要があります。

検出し、web サイトの問題を修正しました。 必要な場合に、次の手順が後にいた可能性があります。

1. Visual studio でソリューションを実行または web サーバー上のページをデプロイします。
2. ブラウザーで開発者ツールを使用または単にソース コードを開き、スタイルしようとする、問題の一致を開きます。 使用を関連するファイルを検索する、&quot;検索&quot;または&quot;ファイルで検索&quot;スタイル クラスの名前で機能します。
3. エラーが検出されると、Web ブラウザーとサーバーを停止します。
4. ブラウザーのキャッシュを消去します。
5. 修正を適用する Visual Studio に戻ります。 テストするすべての手順を繰り返します。

ASP.NET MVC 4 の実際の WYSIWYG が存在しないためスタイルの問題のほとんどを実行しているまたは web アプリケーションを展開した後、後の段階で検出されます。 ここで Page Inspector、ソリューションを実行することがなく任意のページをプレビューできることです。

このタスクでは、Page inspector を使用し、フォト ギャラリーのアプリケーションのいくつかの問題を修正します。

1. Page Inspector を使用して探します、**登録**と**ログイン**ヘッダーの左側にあるリンクです。

    リンクは、右側に期待どおりの場所に表示されませんが箇条書きリストのように表示されることに注意してください。 それに応じてスタイルを変更して、右へのリンクを整列するされます。

    ![リンクで、登録とログを検索する](using-page-inspector-in-visual-studio-2012/_static/image15.png "リンクで、登録とログを検索します。")

    *リンクで、登録とログを検索します。*
2. 選択されている検査モードの切り替えに近いではなく、そのコードを開くための登録リンクをクリックします。

    通知のリンクのソース コードに配置されている、  **\_LoginPartial.cshtml**ファイル、Index.cshtml いないも\_Layout.cshtml は、最初に確認することがあります。 適切なソース ファイルに直接配置されています。
3. **スタイル** タブを見つけて、クリックして、 **<section> #login</section>** 項目で、これらのリンクの HTML コンテナーです。

    いることを確認、 **#login**スタイルが自動的に**Site.css**  をクリック後します。 さらに、コードは強調表示されたようになりました。

    ![CSS スタイルの選択](using-page-inspector-in-visual-studio-2012/_static/image16.png "CSS スタイルの選択")

    *CSS スタイルの選択*
4. コメントを解除、**テキスト配置**属性の開始タグと終了文字を削除することで強調表示されたコードと保存、 **Site.css**ファイル。

    Page Inspector は、現在のページを構成するすべてのファイルを認識し、これらのファイルのいずれかが変更を検出できます。 ブラウザーで現在のページがソース ファイルと同期されるたびに、警告されます。
5. Page Inspector のブラウザーでページを再読み込みアドレス バーの下にあるバーをクリックします。

    ![ページの再読み込み](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *ページの再読み込み*

    リンクは、右側にある、ようになりましたも箇条書きリストのように見えます。 次に、行頭文字を削除し、リンクを左右に配置します。

    ![更新されたページ](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *更新されたページ*
6. いずれかを選択して、検査モードを使用して、 **&lt;li&gt;** 項目が含まれている、&quot;登録&quot;と&quot;ログイン&quot;リンク。 をクリックし、 **&lt;セクション&gt;#login**項目へのアクセスを**Styles.css**コード。

    ![スタイルの検索](using-page-inspector-in-visual-studio-2012/_static/image19.png "のスタイルの検索")

    *スタイルの検索*
7. **Style.css**、コードをコメント解除します **#login li**項目。 追加するスタイルは行頭文字を非表示にして、水平方向に項目を表示します。

    ![ログインのリンク スタイルの変更](using-page-inspector-in-visual-studio-2012/_static/image20.png "ログインのリンク スタイルの変更")

    *ログインのリンク スタイルの変更*
8. 保存**Style.css**ファイルを開き、ページを再読み込みするアドレスの下にあるバーを 1 回クリックします。 リンクが正しく表示されることに注意してください。

    ![リンクが右側に揃えて配置](using-page-inspector-in-visual-studio-2012/_static/image21.png "へのリンクが右側に配置")

    *右揃えで配置へのリンク*
9. 最後に、ヘッダーのタイトルを変更します。 検査モードを使用 をクリックして**貴社のロゴ**テキストと生成されたソース コードを取得します。
10. 使用するようになりました **\_Layout.cshtml**、置き換える '**貴社のロゴ**'テキスト'**フォト ギャラリー**'。 保存して Page Inspector のブラウザーを更新します。

    ![新しいタイトルを割り当てる](using-page-inspector-in-visual-studio-2012/_static/image22.png "新しいタイトルを割り当てる")

    *新しいタイトルを割り当てる*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *フォト ギャラリー ページの更新*
11. 最後に、完了、 **PhotoGallery**プロジェクト キーを押します**f5 キーを押して**アプリを実行します。 チェック アウトすべて予想どおりに変更します。

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>手順 2: WebForms プロジェクトで Page Inspector の使用

この演習では、プレビュー、Page Inspector を使用して、WebForms ソリューションをデバッグする方法を学びます。 については、Page Inspector の機能を Web のプロセスのデバッグを容易にするツールの簡単な概要をまず実行します。 次に、スタイルの問題を含む web ページで作業します。 Page Inspector を使用して、問題を生成するソース コードを検索し、その修正方法を学習します。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>タスク 1 - Page Inspector の調査

このタスクでは、フォト ギャラリーを表示するための WebForms プロジェクトのコンテキストで Page Inspector の機能を使用する方法を学習します。

1. 開く、**開始**ソリューションがある**ソース/Ex2-WebForms/開始/** フォルダー。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
2. ソリューション エクスプ ローラーで検索**Default.aspx**  ページで、右クリックし、選択**Page Inspector で表示**します。

    ![Page Inspector を開くと Default.aspx](using-page-inspector-in-visual-studio-2012/_static/image24.png "Page Inspector で Default.aspx を開く")

    *Page Inspector で Default.aspx を開く*
3. Page Inspector ウィンドウは、Default.aspx に表示されます。

    ![Page Inspector 内で Default.aspx を表示する](using-page-inspector-in-visual-studio-2012/_static/image25.png "Page Inspector 内で Default.aspx を表示します。")

    *Page Inspector 内で Default.aspx を表示します。*

    Page Inspector ツールは、Visual Studio 環境で統合されます。 インスペクターには、選択したコードを示す強力な HTML プロファイラーと共に、埋め込まれたブラウザーが含まれています。 注意してください、ページの外観を確認するソリューションを実行する必要はありません。

    > [!NOTE]
    > Page Inspector のブラウザーの幅が開かれているページの幅未満の場合は表示されませんページ適切です。 このような場合は、Page Inspector の幅を調整します。
4. をクリックして、**ファイル**Page Inspector 内でのタブ。

    表示された既定のページを作成するすべてのソース ファイルが表示されます。 これは、ユーザー コントロールとマスター ページを使用している場合は特に、ひとめですべての要素を識別するために便利な機能です。 各ファイルに移動することも確認します。

    ![[ファイル] タブ](using-page-inspector-in-visual-studio-2012/_static/image26.png "[ファイル] タブ")

    *[ファイル] タブ*
5. をクリックして、**検査モードの切り替え**タブの左側にあるボタンをクリックします。

    このツールを使用すると、ページの任意の要素を選択し、その HTML コードおよび .aspx ソースを参照してください。

    ![トグル検査モード ボタン](using-page-inspector-in-visual-studio-2012/_static/image27.png "検査モードの切り替えボタン")

    *トグル検査モード ボタン*
6. Page Inspector のブラウザーでは、ページ要素にマウスを移動します。 表示するページの任意の部分にマウス ポインターを移動するときに、要素の型が表示され、対応するソース マークアップまたはコードが Visual Studio エディターで強調表示されます。

    ![アクションで検査モード](using-page-inspector-in-visual-studio-2012/_static/image28.png "アクションで検査モード")

    *検査モードの動作*

    > [!NOTE]
    > Page Inspector ウィンドウを最大化しないか、ソース コードを示す プレビュー タブを表示することはできません。 最大化されたときに、Page Inspector 内で要素をクリックすると、選択範囲のソース コードが表示されますが、Page Inspector ウィンドウを非表示になります。

    場合に注意を払う**Default.aspx**ファイルが表示されます、選択した要素を生成するソース コードの部分が強調表示されます。 この機能には、長いソース ファイル、コードにアクセスする直接的および高速の方法を提供するのエディションが容易になります。

    ![要素を検査](using-page-inspector-in-visual-studio-2012/_static/image29.png "要素の検査")

    *要素の検査*
7. クリックして、**検査モードを切り替える**ボタン (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser です.") ) 、カーソルを無効にする、Page Inspector のタブにあります。
8. 選択、 **HTML** Page Inspector のブラウザーでレンダリングされた HTML コードを表示するタブ。
9. HTML のコードで Koala リンクのリスト アイテムを見つけて選択します。

    コードを選択すると、対応する出力が自動的に強調表示されているブラウザーであることを確認します。 この機能は、HTML ブロックをページに表示する方法を確認するのに便利です。

    ![HTML 要素の選択 ページで](using-page-inspector-in-visual-studio-2012/_static/image31.png "ページの HTML 要素の選択")

    *ページの HTML 要素の選択*
10. をクリックして、**検査モードの切り替え**有効にするボタン*検査モード*ナビゲーション バーをクリックします。 スタイルのウィンドウで、HTML コードの右側に、選択した要素に適用される CSS スタイルを使用して一覧が表示されます。

    > [!NOTE]
    > ヘッダーがサイトのレイアウトの一部であるため、Page Inspector も Site.Master ファイルを開き、影響を受けるコードのセグメントを強調表示します。

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "スタイルと、選択した要素のソース ファイルを検出します。")

    *スタイルと、選択した要素のソース ファイルを検出します。*
11. 切り替え検査ポインターを有効になっている、メニュー バーの下にマウス ポインターを移動し、空白の半分の円をクリックします。

    ![要素の選択](using-page-inspector-in-visual-studio-2012/_static/image33.png "要素の選択")

    *要素の選択*
12. スタイルのウィンドウで検索、**背景イメージ**] の [、 **.main コンテンツ**グループ。 **オフに**、**背景イメージ**みてください。 ブラウザーで、変更がすぐに反映されますを非表示には、円が表示されます。

    > [!NOTE]
    > ページのインスペクターのスタイル タブに適用する変更は、元のスタイル シートには影響しません。 スタイルをオフにしたり、何度でも、ページの更新後に復元されますが、値を変更できます。

    ![有効にして、CSS styles2 を無効化](using-page-inspector-in-visual-studio-2012/_static/image34.png "の有効化と CSS スタイルを無効にします。")

    *有効にして、CSS スタイルを無効化*
13. をクリックして、'**、** **ロゴ '** 検査モードを使用して、ヘッダーのテキスト。
14. **スタイル** タブで、検索、**フォント サイズ**CSS 属性、 **.site タイトル**グループ。 その値を編集するには、1 回の属性をダブルクリックします。 値を置換、2.3em **3em**、し、ENTER キーを押します。 タイトルが大きいことに注意してください。

    ![ページ Inspector2 CSS 値を変更する](using-page-inspector-in-visual-studio-2012/_static/image35.png "Page Inspector 内で値を変更する CSS")

    *Page Inspector 内での CSS 値を変更します。*
15. をクリックして、**トレース スタイル** タブで、Page Inspector の右側のウィンドウにあります。 これは、属性の名前順で、選択範囲に適用されるすべてのスタイルを表示する別の方法です。

    ![選択した要素の CSS スタイルのトレース](using-page-inspector-in-visual-studio-2012/_static/image36.png "選択した要素の CSS スタイルのトレース")

    *選択した要素の CSS スタイルのトレース*
16. Page Inspector のもう 1 つの機能は、レイアウト ペインです。 検査モードを使用して、ナビゲーション バーを選択し、クリックして、**レイアウト**右側のウィンドウ タブ。 選択した要素の正確なサイズと、オフセット、余白、パディングおよび境界線のサイズが表示されます。 このビューから値を変更することも注目してください。

    ![Page Inspector 内で要素のレイアウト](using-page-inspector-in-visual-studio-2012/_static/image37.png "Page Inspector 内で要素のレイアウト")

    *Page Inspector 内で要素のレイアウト*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>タスク 2 - 検索して、フォト ギャラリーでスタイルの問題を修正

Visual Studio の以前のバージョンとの Web ページの問題を診断するにはでしょうか。 なじみの web デバッグ ツール、Internet Explorer 開発者ツールや Firebug など、Visual Studio IDE の外部で実行するとしています。 ブラウザーでのみ認識、HTML スクリプトとスタイル中、基になるフレームワークには、レンダリングされる HTML が生成されます。 そのため、多くの場合、サイト全体のように web ページの外観を確認するを展開する必要があります。

検出し、web サイトの問題を修正しました。 必要な場合に、次の手順が後にいた可能性があります。

1. Visual studio でソリューションを実行または web サーバー上のページをデプロイします。
2. ブラウザーで開発者ツールを使用または単にソース コードを開き、スタイルしようとする、問題の一致を開きます。 使用を関連するファイルを検索する、&quot;検索&quot;または&quot;ファイルで検索&quot;スタイル クラスの名前で機能します。
3. エラーが検出されると、Web ブラウザーとサーバーを停止します。
4. ブラウザーのキャッシュを消去します。
5. 修正を適用する Visual Studio に戻ります。 テストするすべての手順を繰り返します。

いいえ、ASP.NET WebForms で実際に WYSIWYG が、いくつかのスタイルの問題が後の段階で実行中または展開後に検出されました。 ここで Page Inspector、ソリューションを実行することがなく任意のページをプレビューできることです。

このタスクでは、フォト ギャラリーのアプリケーションのいくつかの問題を修正するために、Page inspector を使用します。 次の手順では、検出し、ヘッダーに何らかの単純なスタイルの問題を迅速に修正します。

1. 検索ページの検査を使用して、**登録**と**ログで**ヘッダーの左側にあるリンクです。

    右側の期待どおりの場所で、リンクが表示されないことに注意してください。 それに応じてスタイルを変更して、右へのリンクを整列するされます。

    ![左側に配置されているリンク ログイン](using-page-inspector-in-visual-studio-2012/_static/image38.png "左側に配置されているリンクにログイン")

    *左側に配置されているログのリンク*
2. 選択されている検査モードの切り替え、そのコードを開くためのログでリンクを選択します。

    リンクのソース コードに配置されている通知、 **Site.Master**最初に探すことになる場所ですが、Default.aspx ページでは適切なソース ファイルで直接に配置したせずファイル。
3. **スタイル** タブを見つけて、クリックして、 **&lt;セクション&gt;#login**項目で、これらのリンクの HTML コンテナーです。

    いることを確認、 **#login**スタイルが自動的に**Site.css**  をクリック後します。 さらに、コードは強調表示されたようになりました。

    ![CSS スタイルの選択](using-page-inspector-in-visual-studio-2012/_static/image39.png "CSS スタイルの選択")

    *CSS スタイルの選択*
4. コメントを解除、**テキスト配置**属性の開始タグと終了文字を削除することで強調表示されたコードと保存、 **Site.css**ファイル。

    Page Inspector は、現在のページを構成するすべてのファイルを認識し、これらのファイルのいずれかが変更を検出できます。 ブラウザーで現在のページがソース ファイルと同期されるたびに、警告されます。
5. Page Inspector のブラウザーでは、変更を保存し、ページを再読み込みするには、アドレス バーの下にあるバーをクリックします。

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *ページの再読み込み*

    リンクは、右側にある、ようになりましたも箇条書きリストのように見えます。 次に、行頭文字を削除し、リンクを左右に配置します。

    ![更新されたページ](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *更新されたページ*
6. いずれかを選択して、検査モードを使用して、 **&lt;li&gt;** 項目が含まれている、&quot;登録&quot;と&quot;ログイン&quot;リンク。 をクリックし、 **&lt;セクション&gt;#login**項目へのアクセスを**Styles.css**コード。

    ![スタイルの検索](using-page-inspector-in-visual-studio-2012/_static/image42.png "のスタイルの検索")

    *スタイルの検索*
7. **Style.css**、コードをコメント解除します **#login li**項目。 追加するスタイルは行頭文字を非表示にして、水平方向に項目を表示します。

    ![ログインのリンク スタイルの変更](using-page-inspector-in-visual-studio-2012/_static/image43.png "ログインのリンク スタイルの変更")

    *ログインのリンク スタイルの変更*
8. 保存**Style.css**ファイルを開き、ページを再読み込みするアドレスの下にあるバーを 1 回クリックします。 リンクが正しく表示されることに注意してください。

    ![リンクが右側に揃えて配置](using-page-inspector-in-visual-studio-2012/_static/image44.png "へのリンクが右側に配置")

    *右揃えで配置へのリンク*
9. 最後に、ヘッダーのタイトルを変更します。 検索する代わりに、'**貴社のロゴ '** すべてのファイル内のテキストでは、検査モードを使用してテキストをクリックし、生成されたソース コードを取得します。

    ![サイトのタイトルを検索](using-page-inspector-in-visual-studio-2012/_static/image45.png "サイトのタイトルの検索")

    *サイトのタイトルの検索*
10. 使用するようになりました**Site.Master**、置換、'**貴社のロゴ**'テキスト'**フォト ギャラリー**'。 保存して Page Inspector のブラウザーを更新します。

    ![フォト ギャラリーのページ更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "フォト ギャラリー ページの更新")

    *フォト ギャラリー ページの更新*
11. 最後にキーを押して**F5**アプリに期待どおりに変更をすべてチェックを実行します。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボを完了するが、Page Inspector を使用して、再構築し、ブラウザーで Web サイトを実行することがなく、Web アプリケーションをプレビューする方法を習得します。 さらに、すばやく発見し、ソース コードに表示される出力から直接アクセスすることでバグを修正する方法を習得しました。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](using-page-inspector-in-visual-studio-2012/_static/image47.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express for Web のタイル*
