---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Visual Studio 2012 で Page Inspector を使用して |Microsoft ドキュメント
author: rick-anderson
description: このハンズオン ラボでは、新しいツールを見つけて Visual Studio - Page Inspector で web ページの問題を修正することがわかります。 Page Inspector は、新しいツールその b.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 052d29dba170d403c2b1c1667c55fc2c34045615
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Visual Studio 2012 での Page Inspector の使用
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> このハンズオン ラボでは、新しいツールを見つけて Visual Studio - Page Inspector で web ページの問題を修正することがわかります。
> 
> Page Inspector は、ブラウザー診断ツールを Visual Studio に表示し、ブラウザー、ASP.NET、およびソース コードの間で統合されたエクスペリエンスを提供する新しいツールです。 Visual Studio IDE 内で直接、web ページ (HTML、Web フォーム、ASP.NET MVC または Web ページ) を表示し、ソース コードと、結果の出力の両方を調べることができます。 Page Inspector を使用すると、web サイトを簡単に展開し、迅速に、ゼロからページを構築および問題を診断できます。
> 
> 最近では ASP.NET MVC と WebForms などの適切なタイミングで柔軟でスケーラブルな web サイトを作成する Web フレームワークの数があります。 その一方で、取得が困難になります IDE がテンプレートに基づくページや動的コンテンツでデザイナーのビューをサポートしていないため、ページ上の問題を検索します。 したがって、これらの web サイトをユーザーに表示される方法を参照するブラウザーで開く必要です。
> 
> Web 開発者は、外部ツールを使用して、ブラウザーで定期的に実行している問題を検出します。 次に、IDE に戻る、修復を開始します。 これは、前後 IDE、ブラウザー、およびプロファイリング ツールの間のアクティビティ、効率的なことができ、新規に展開とキャッシュの問題を再現するたびに、クリーニングが必要な場合があります。
> 
> Page Inspector は、の機能の結合セットを使用して両方の長所をまとめることによってクライアント (ブラウザー ツール) と、サーバー (ASP.NET およびソース コード) の間での Web 開発のギャップを結びます。
> 
> Page Inspector を使用して確認できます (サーバー側のコードを含む)、ソース ファイルの要素がブラウザーにレンダリングされる HTML マークアップを生成します。 Page Inspector を使用して、CSS のプロパティと、ブラウザーで直ちに反映された変更を表示する DOM 要素の属性を変更することもできます。
> 
> このハンズオン ラボは、Page Inspector の機能について説明して、Web アプリケーションで問題の解決を使用する方法を示します。 **このラボには、類似のフローを使用して、さまざまなテクノロジを対象とする 2 つの手順が含まれています。ASP.NET MVC 開発者の場合は、次の手順のいずれかです。2 WebForms 開発者フォロー練習をする場合は**します。
> 
> このラボには、機能強化と軽微な変更を元のフォルダーで提供されるサンプル Web アプリケーションに適用することで前に説明した新しい機能について説明します。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)です。


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- Page Inspector を使用して Web サイトを分解します。
- 検査し、Page Inspector で CSS スタイルの変更のプレビュー
- 検出して Page Inspector を使用して、web ページでの問題を修正します。

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

このハンズオン ラボには、次の実習が含まれます。

1. [手順 1: ASP.NET MVC プロジェクトで Page Inspector を使用します。](#Exercise1)
2. [手順 2: WebForms プロジェクトで Page Inspector を使用します。](#Exercise2)

> [!NOTE]
> 各手順は、他のユーザーとは無関係に各手順に従うことができるようにすると、作業の開始フォルダーにある、開始のソリューションを伴います。 演習では、ソース コード、内部は、対応する手順」の手順を完了して得た結果コードと Visual Studio ソリューションを含む終了フォルダーもがあります。 このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。


この演習を完了する時間を推定: **30 分**です。

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>手順 1: ASP.NET MVC プロジェクトで Page Inspector を使用します。

この演習では、プレビューをデバッグする方法を学習します、 **ASP.NET MVC 4**ソリューションを使用して**Page Inspector**です。 最初に、プロセスのデバッグ、Web を促進する機能を説明するツールの簡単な膝を実行します。 次に、スタイルの問題を含む web ページで作業します。 Page Inspector を使用して、問題を生成するソース コードを検索し、その修正方法を学習します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>タスク 1 - Page Inspector の探索

このタスクでは、フォト ギャラリーを表示する ASP.NET MVC 4 プロジェクトのコンテキストで Page Inspector を使用する方法を学習します。

1. 開く、**開始**ソリューションにある**ソース/Ex1-MVC4/開始/**フォルダーです。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. ソリューション エクスプ ローラーで**Index.cshtml**下を表示、 **/ビュー/ホーム**プロジェクト フォルダーを右クリックして、選択**Page Inspector で表示**です。

    ![Page Inspector 内でプレビューするファイルを選択する](using-page-inspector-in-visual-studio-2012/_static/image1.png "Page Inspector 内でプレビューするファイルを選択します。")

    *Page Inspector 内でプレビューするファイルを選択します。*
3. Page Inspector ウィンドウに表示されます、*ホーム/インデックス*URL ソースを選択したビューにマップします。

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Page Inspector で最初にお問い合わせください。*

    Page Inspector ツールは、Visual Studio 環境内に統合されています。 インスペクターには、強力な HTML プロファイラーと共にの組み込みブラウザーが含まれています。 注意してください、ページの外観を確認するようにソリューションを実行する必要はありません。

    > [!NOTE]
    > Page Inspector のブラウザーの幅が開かれるページの幅よりも低い場合は表示されませんページ適切です。 その場合は、Page Inspector の幅を調整します。
4. クリックして、**ファイル**Page Inspector 内でのタブです。

    インデックス ページを作成するすべてのソース ファイルが表示されます。 この機能は、部分ビューとテンプレートを使用している場合は特に、ひとめですべての要素を識別に役立ちます。 開くことができますも各ファイルのリンクをクリックする場合に注意してください。

    ![The-Files-tab](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *[ファイル] タブ*
5. クリックして、**検査モードを切り替える**タブの左側にあるボタンをクリックします。

    このツールを使用して、ページのいずれかの要素を選択して、HTML および Razor コードを参照してください。

    ![トグル検査モード ボタン](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *トグル検査モード ボタン*
6. Page Inspector のブラウザーでは、ページの要素に対するマウス ポインターを移動します。 表示するページの任意の部分にマウス ポインターを移動するときに、要素の型が表示され、対応するソース マークアップまたはコードが Visual Studio エディターで強調表示されます。

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *検査モードの動作*

    > [!NOTE]
    > Page Inspector ウィンドウを最大化しないで、またはソース コードを示す プレビュー タブを表示することはできません。 最大化されたときに、Page Inspector 内で要素をクリックすると、選択範囲のソース コードが表示されますが、Page Inspector ウィンドウを非表示になります。

    注意する場合、 **Index.cshtml**ファイルが表示されます、選択した要素を生成するソース コードの部分が強調表示されます。 この機能は、コードにアクセスするダイレクトと高速の方法を提供、長いソース ファイルの編集を容易にします。

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *要素の検査*
7. クリックして、**検査モードを切り替える**ボタン (![Page Inspector のブラウザーでレンダリングされた HTML コードを表示する HTML タブを選択します.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Page Inspector のブラウザーでレンダリングされた HTML コードを表示する HTML タブを選択します.") ) 、カーソルを無効にします。
8. 選択、 **HTML** Page Inspector のブラウザーでレンダリングされた HTML コードを表示するタブです。
9. HTML マークアップで Koala リンクのリスト アイテムをクリックして選択します。

    コードを選択すると、対応する出力はブラウザーでは強調表示に自動的に注目してください。 この機能は HTML ブロックをページに表示する方法を表示すると便利です。

    ![ページで選択する HTML 要素](using-page-inspector-in-visual-studio-2012/_static/image8.png " ページで選択する HTML 要素")

    *ページの HTML 要素の選択*
10. をクリックして、**検査モードを切り替える** ボタンを有効にする*検査モード*ナビゲーション バー をクリックします。 スタイル ウィンドウ内の HTML コードの右上には、選択した要素に適用される CSS スタイルを使用して一覧が表示されます。

    > [!NOTE]
    > Page Inspector を開くことも、ヘッダーがサイトのレイアウトの一部であるため、 \_Layout.cshtml ファイルとコードのセグメントが影響を強調表示します。

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *スタイルを検出すると、選択した要素のソース ファイル*
11. 有効になっている、トグル検査ポインターでは、青の機能を備えたバーの下のマウス ポインターを移動を半分の円をクリックします。

    ![要素の選択](using-page-inspector-in-visual-studio-2012/_static/image10.png "要素の選択")

    *要素の選択*
12. スタイルのウィンドウに、**背景イメージ**項目の下にある、 **.main コンテンツ**グループ。 **オフにして**、**背景イメージ**何が起こるかを確認します。 ブラウザーで変更がすぐに反映され、円が非表示のことがわかります。

    > [!NOTE]
    > ページ インスペクターのスタイル タブを適用する変更は、元のスタイル シートには影響しません。 スタイルをオフにしたり、回数だけ、したいが、ページを更新した後、復元するには、その値を変更することができます。

    ![有効にして、CSS スタイルを無効化](using-page-inspector-in-visual-studio-2012/_static/image11.png "の有効化と CSS スタイルを無効にします。")

    *有効にして、CSS スタイルを無効化*
13. をクリックして、'**ここにロゴ**' 検査モードを使用して、ヘッダーのテキスト。
14. **スタイル** タブで、検索、**フォント サイズ**CSS 属性の下にある、 **.site タイトル**グループ。 属性の値をダブルクリックし、2.3 em 値の置換**3 em**、キーを押します**ENTER**です。 タイトルに大きく見えることに注意してください。

    ![Page Inspector 内で CSS の値を変更する](using-page-inspector-in-visual-studio-2012/_static/image12.png "Page Inspector 内で変更する CSS の値")

    *Page Inspector 内で CSS の値を変更します。*
15. クリックして、**トレース スタイル** タブ、Page Inspector の右側のウィンドウであります。 これは、属性の名前順で、選択範囲に適用されるすべてのスタイルを表示する別の方法です。

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *選択した要素の CSS スタイルのトレース*
16. Page Inspector のもう 1 つの機能は、レイアウト ペインです。 検査モードを使用して、ナビゲーション バーを選択し、をクリックして、**レイアウト**右側のウィンドウ タブ。 選択した要素の正確なサイズだけでなく、オフセット、余白、パディング、および境界線のサイズが表示されます。 このビューから値を変更することも確認します。

    ![Page Inspector 内で要素のレイアウト](using-page-inspector-in-visual-studio-2012/_static/image14.png "Page Inspector 内で要素のレイアウト")

    *Page Inspector 内で要素のレイアウト*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>タスク 2 - 検索して、フォト ギャラリーでのスタイルの問題の解決

以前のバージョンの Visual Studio で Web ページの問題を診断するにはどうは? 可能性の高い経験のある web に Internet Explorer Developer Tools または Firebug など、Visual Studio IDE の外部で実行するデバッグ ツールとしています。 ブラウザーでのみ認識、HTML スクリプトとスタイル、基になるフレームワークがレンダリングされる HTML を生成中にします。 そのため、多くの場合、サイト全体のように web ページの外観を確認するを展開する必要があります。

検出し、web サイトで、問題を解決するには、次の手順をに従ってする必要がある可能性があります。

1. Visual studio でソリューションを実行するか、web サーバー上のページを展開します。
2. ブラウザーで、開発者ツールを使用すると、または単に、ソース コードとスタイルを開きますしようとする問題を一致を開きます。 ファイルを検索する、関連するを使用した、&quot;検索&quot;または&quot;ファイルで検索&quot;スタイル クラスの名前で機能します。
3. エラーが検出されると、Web ブラウザーとサーバーを停止します。
4. ブラウザーのキャッシュを消去します。
5. 修正を適用する Visual Studio に戻ります。 テストするすべての手順を繰り返します。

ASP.NET MVC 4 の実際の WYSIWYG がないため、スタイルの問題のほとんどが後の段階で実行されているまたは web アプリケーションの展開後に検出されました。 ここで、Page Inspector でソリューションを実行しなくても、任意のページをプレビューすることができます。

このタスクでは、Page inspector を使用し、アプリケーションをフォト ギャラリーのいくつかの問題を修正します。

1. Page Inspector を使用して探します、**登録**と**ログイン**ヘッダーの左側にあるリンクします。

    リンクは、右側の必要な場所には表示されませんし、箇条書きリストのように表示されることに注意してください。 それに応じてスタイルを変更して、右へのリンクを整列するされます。

    ![リンクで、登録とログを検索する](using-page-inspector-in-visual-studio-2012/_static/image15.png "へのリンクで、登録とログを検索します。")

    *リンクで、登録とログを検索します。*
2. 選択されている検査モードの切り替え、閉じるをではなく、そのコードを開くための登録のリンクをクリックします。

    リンクのソース コードがあることを確認、  **\_LoginPartial.cshtml**ファイル、Index.cshtml いないも\_Layout.cshtml は、最初に探すこともできます。 正しいソース ファイルに直接配置されています。
3. **スタイル** タブを特定し、をクリックして、 **<section> #login</section>**項目は、これらのリンクを HTML のコンテナーです。

    注意して、 **#login**スタイルで自動的にある**Site.css**  をクリックします。 さらに、コードは、ここで強調表示されます。

    ![CSS スタイルの選択](using-page-inspector-in-visual-studio-2012/_static/image16.png "CSS スタイルの選択")

    *CSS スタイルの選択*
4. コメントを解除、**テキスト配置**属性の開始タグと終了文字を削除することで強調表示されたコードと保存、 **Site.css**ファイル。

    Page Inspector は、現在のページを構成するすべてのファイルを認識しており、これらのファイルのいずれかが変更を検出できます。 ブラウザーでは、現在のページがソース ファイルと同期されるたびにユーザーに警告します。
5. Page Inspector のブラウザーでは、ページを再度読み込んで、アドレス バーの下にあるバーをクリックします。

    ![ページの再読み込み](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *ページの再読み込み*

    リンクようになりました、右側にあるが、まだ箇条書きリストのように見えます。 ここで、箇条書きを削除し、リンクを水平方向に整列します。

    ![更新されたページ](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *更新されたページ*
6. いずれかを選択して検査モードを使用して、 **&lt;li&gt;**を含む項目、&quot;登録&quot;と&quot;ログイン&quot;リンクします。 次に、をクリックして、 **&lt;セクション&gt;#login**にアクセスする項目**Styles.css**コード。

    ![スタイルを検索する](using-page-inspector-in-visual-studio-2012/_static/image19.png "スタイルを検索します。")

    *スタイルを検索します。*
7. **Style.css**、用のコードのコメントを解除**#login li**項目。 追加するスタイルの項目を非表示にされ、アイテムの水平方向に表示されます。

    ![スタイルの変更、ログイン リンク](using-page-inspector-in-visual-studio-2012/_static/image20.png "ログイン リンク スタイルの変更")

    *ログイン リンク スタイルの変更*
8. 保存**Style.css**ファイルおよびページの再読み込みするアドレスの下にあるバーを 1 回クリックします。 リンクが正しく表示されることに注意してください。

    ![リンクが右側に整列](using-page-inspector-in-visual-studio-2012/_static/image21.png "へのリンクが、右側に配置")

    *右揃えで配置へのリンク*
9. 最後に、ヘッダーのタイトルを変更します。 検査モードを使用 をクリックして**ここにロゴ**テキストと生成されたソース コードを取得します。
10. 使用するようになりました **\_Layout.cshtml**、置換 '**ここにロゴ**'にテキスト'**フォト ギャラリー**' です。 保存して Page Inspector のブラウザーを更新します。

    ![新しいタイトルを割り当てる](using-page-inspector-in-visual-studio-2012/_static/image22.png "新しいタイトルを割り当てる")

    *新しいタイトルを割り当てる*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *フォト ギャラリー ページが更新されました*
11. 最後に、方法を選択、 **PhotoGallery**プロジェクトとキーを押して**f5 キーを押して**アプリを実行します。 チェック アウトすべて変更が想定どおりに動作します。

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>手順 2: WebForms プロジェクトで Page Inspector を使用します。

この演習では、プレビューして Page Inspector を使用して WebForms ソリューションをデバッグする方法を学習します。 最初はプロセスのデバッグ、Web を容易に Page Inspector の機能を説明するツールの簡単な膝を行います。 次に、スタイルの問題を含む web ページで作業します。 Page Inspector を使用して、問題を生成するソース コードを検索し、その修正方法を学習します。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>タスク 1 - Page Inspector の探索

このタスクでは、フォト ギャラリーを表示する WebForms プロジェクトのコンテキストで Page Inspector の機能を使用する方法を学習します。

1. 開く、**開始**ソリューションにある**ソース/Ex2-WebForms/開始/**フォルダーです。

   1. いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
   3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
2. ソリューション エクスプ ローラーで**Default.aspx**  ページで、右クリックし、選択**Page Inspector で表示**です。

    ![Page Inspector で Default.aspx を開く](using-page-inspector-in-visual-studio-2012/_static/image24.png "Page Inspector で Default.aspx を開く")

    *Page Inspector で Default.aspx を開く*
3. Default.aspx ページ インスペクター ウィンドウが表示されます。

    ![Page Inspector 内で Default.aspx を表示する](using-page-inspector-in-visual-studio-2012/_static/image25.png "Page Inspector 内で Default.aspx を表示します。")

    *Page Inspector 内で Default.aspx を表示します。*

    Page Inspector ツールは、Visual Studio 環境内に統合されています。 インスペクターには、選択したコードを表示する強力な HTML プロファイラーと共にの組み込みブラウザーが含まれています。 注意してください、ページの外観を確認するようにソリューションを実行する必要はありません。

    > [!NOTE]
    > Page Inspector のブラウザーの幅が開かれるページの幅よりも低い場合は表示されませんページ適切です。 その場合は、Page Inspector の幅を調整します。
4. クリックして、**ファイル**Page Inspector 内でのタブです。

    表示された既定のページを作成するすべてのソース ファイルが表示されます。 これは、ユーザー コントロールとマスター ページを使用している場合は特に、ひとめですべての要素を識別する便利な機能です。 各ファイルに移動することも確認します。

    ![[ファイル] タブ](using-page-inspector-in-visual-studio-2012/_static/image26.png "[ファイル] タブ")

    *[ファイル] タブ*
5. クリックして、**検査モードを切り替える**タブの左側にあるボタンをクリックします。

    このツールを使用して、ページのいずれかの要素を選択して、HTML コードおよび .aspx ソースを参照してください。

    ![トグル検査モード ボタン](using-page-inspector-in-visual-studio-2012/_static/image27.png "検査モードの切り替えボタン")

    *トグル検査モード ボタン*
6. Page Inspector のブラウザーでは、ページ要素にマウスを移動します。 表示するページの任意の部分にマウス ポインターを移動するときに、要素の型が表示され、対応するソース マークアップまたはコードが Visual Studio エディターで強調表示されます。

    ![アクションで検査モード](using-page-inspector-in-visual-studio-2012/_static/image28.png "検査モードの動作")

    *検査モードの動作*

    > [!NOTE]
    > Page Inspector ウィンドウを最大化しないで、またはソース コードを示す プレビュー タブを表示することはできません。 最大化されたときに、Page Inspector 内で要素をクリックすると、選択範囲のソース コードが表示されますが、Page Inspector ウィンドウを非表示になります。

    場合に注意を払う**Default.aspx**ファイルが表示されます、選択した要素を生成するソース コードの部分が強調表示されます。 この機能には、長いソース ファイル、コードにアクセスするダイレクトと高速の方法を提供のエディションが容易になります。

    ![要素を検査](using-page-inspector-in-visual-studio-2012/_static/image29.png "要素を検査")

    *要素の検査*
7. クリックして、**検査モードを切り替える**ボタン (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser です.") ) 、カーソルを無効にする、Page Inspector のタブにあります。
8. 選択、 **HTML** Page Inspector のブラウザーでレンダリングされた HTML コードを表示するタブです。
9. HTML のコードで Koala リンクのリスト アイテムをクリックして選択します。

    コードを選択すると、対応する出力が自動的に強調表示されているブラウザーであることを確認します。 この機能は HTML ブロックをページに表示する方法を表示すると便利です。

    ![HTML 要素の選択 ページで](using-page-inspector-in-visual-studio-2012/_static/image31.png " ページで、HTML 要素の選択")

    *ページの HTML 要素の選択*
10. をクリックして、**検査モードを切り替える** ボタンを有効にする*検査モード*ナビゲーション バー をクリックします。 スタイル ウィンドウ内の HTML コードの右上には、選択した要素に適用される CSS スタイルを使用して一覧が表示されます。

    > [!NOTE]
    > ヘッダーがサイトのレイアウトの一部であるため、Page Inspector も Site.Master ファイルを開き、影響を受けるコードのセグメントを強調表示します。

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "スタイルを検出すると、選択した要素のソース ファイル")

    *スタイルを検出すると、選択した要素のソース ファイル*
11. 有効になっている、トグル検査ポインターでは、メニュー バーの下のマウス ポインターを移動を空白の半分の円をクリックします。

    ![要素の選択](using-page-inspector-in-visual-studio-2012/_static/image33.png "要素の選択")

    *要素の選択*
12. スタイルのウィンドウに、**背景イメージ**項目の下にある、 **.main コンテンツ**グループ。 **オフにして**、**背景イメージ**何が起こるかを確認します。 ブラウザーで変更がすぐに反映され、円が非表示のことがわかります。

    > [!NOTE]
    > ページ インスペクターのスタイル タブを適用する変更は、元のスタイル シートには影響しません。 スタイルをオフにしたり、回数だけ、したいが、ページを更新した後、復元するには、その値を変更することができます。

    ![有効にして、CSS styles2 を無効化](using-page-inspector-in-visual-studio-2012/_static/image34.png "の有効化と CSS スタイルを無効にします。")

    *有効にして、CSS スタイルを無効化*
13. をクリックして、'**、** **ここにロゴ '**検査モードを使用して、ヘッダーのテキスト。
14. **スタイル** タブで、検索、**フォント サイズ**CSS 属性の下にある、 **.site タイトル**グループ。 値を編集するには、1 回の属性をダブルクリックします。 置換、2.3em 値**3em**、ENTER キーを押します。 タイトルに大きく見えることに注意してください。

    ![ページ Inspector2 CSS 値を変更する](using-page-inspector-in-visual-studio-2012/_static/image35.png "Page Inspector 内で変更する CSS の値")

    *Page Inspector 内で CSS の値を変更します。*
15. クリックして、**トレース スタイル** タブ、Page Inspector の右側のウィンドウであります。 これは、属性の名前順で、選択範囲に適用されるすべてのスタイルを表示する別の方法です。

    ![選択した要素の CSS スタイル トレース](using-page-inspector-in-visual-studio-2012/_static/image36.png "選択した要素の CSS スタイルのトレース")

    *選択した要素の CSS スタイルのトレース*
16. Page Inspector のもう 1 つの機能は、レイアウト ペインです。 検査モードを使用して、ナビゲーション バーを選択し、をクリックして、**レイアウト**右側のウィンドウ タブ。 選択した要素の正確なサイズだけでなく、オフセット、余白、パディング、および境界線のサイズが表示されます。 このビューから値を変更することも確認します。

    ![Page Inspector 内で要素のレイアウト](using-page-inspector-in-visual-studio-2012/_static/image37.png "Page Inspector 内で要素のレイアウト")

    *Page Inspector 内で要素のレイアウト*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>タスク 2 - 検索して、フォト ギャラリーでのスタイルの問題の解決

以前のバージョンの Visual Studio で Web ページの問題を診断するにはどうは? 可能性の高い経験のある web に Internet Explorer Developer Tools または Firebug など、Visual Studio IDE の外部で実行するデバッグ ツールとしています。 ブラウザーでのみ認識、HTML スクリプトとスタイル、基になるフレームワークがレンダリングされる HTML を生成中にします。 そのため、多くの場合、サイト全体のように web ページの外観を確認するを展開する必要があります。

検出し、web サイトで、問題を解決するには、次の手順をに従ってする必要がある可能性があります。

1. Visual studio でソリューションを実行するか、web サーバー上のページを展開します。
2. ブラウザーで、開発者ツールを使用すると、または単に、ソース コードとスタイルを開きますしようとする問題を一致を開きます。 ファイルを検索する、関連するを使用した、&quot;検索&quot;または&quot;ファイルで検索&quot;スタイル クラスの名前で機能します。
3. エラーが検出されると、Web ブラウザーとサーバーを停止します。
4. ブラウザーのキャッシュを消去します。
5. 修正を適用する Visual Studio に戻ります。 テストするすべての手順を繰り返します。

いいえ ASP.NET WebForms の実数 WYSIWYG が、いくつかのスタイルの問題を実行しているか、展開した後後の段階で検出されました。 ここで、Page Inspector でソリューションを実行しなくても、任意のページをプレビューすることができます。

このタスクでは、Page inspector を使用して、フォト ギャラリーのアプリケーションのいくつかの問題を修正するためです。 次の手順では、検出し、ヘッダーにいくつかの単純なスタイルの問題を迅速に解決します。

1. 検索ページの検査を使用して、**登録**と**ログで**ヘッダーの左側にリンクします。

    右側の必要な場所にリンクが表示されないことに注意してください。 それに応じてスタイルを変更して、右へのリンクを整列するされます。

    ![左側に配置されているリンク ログイン](using-page-inspector-in-visual-studio-2012/_static/image38.png "左側に配置されているリンク ログイン")

    *左側に配置されているログのリンク*
2. 選択されている検査モードの切り替え、ログ内のリンクを開くには、コードを選択します。

    リンクのソース コードにある通知、 **Site.Master**ファイル、いない場所である、Default.aspx ページで最初に探すことになる以外の場合は、正しいソース ファイルで直接に配置します。
3. **スタイル** タブを特定し、をクリックして、 **&lt;セクション&gt;#login**項目は、これらのリンクを HTML のコンテナーです。

    注意して、 **#login**スタイルで自動的にある**Site.css**  をクリックします。 さらに、コードは、ここで強調表示されます。

    ![CSS スタイルの選択](using-page-inspector-in-visual-studio-2012/_static/image39.png "CSS スタイルの選択")

    *CSS スタイルの選択*
4. コメントを解除、**テキスト配置**属性の開始タグと終了文字を削除することで強調表示されたコードと保存、 **Site.css**ファイル。

    Page Inspector は、現在のページを構成するすべてのファイルを認識しており、これらのファイルのいずれかが変更を検出できます。 ブラウザーでは、現在のページがソース ファイルと同期されるたびにユーザーに警告します。
5. Page Inspector のブラウザーでは、変更を保存し、ページの再読み込みするには、アドレス バーの下にあるバーをクリックします。

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *ページの再読み込み*

    リンクようになりました、右側にあるが、まだ箇条書きリストのように見えます。 ここで、箇条書きを削除し、リンクを水平方向に整列します。

    ![更新されたページ](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *更新されたページ*
6. いずれかを選択して検査モードを使用して、 **&lt;li&gt;**を含む項目、&quot;登録&quot;と&quot;ログイン&quot;リンクします。 次に、をクリックして、 **&lt;セクション&gt;#login**にアクセスする項目**Styles.css**コード。

    ![スタイルを検索する](using-page-inspector-in-visual-studio-2012/_static/image42.png "スタイルを検索します。")

    *スタイルを検索します。*
7. **Style.css**、用のコードのコメントを解除**#login li**項目。 追加するスタイルの項目を非表示にされ、アイテムの水平方向に表示されます。

    ![スタイルの変更、ログイン リンク](using-page-inspector-in-visual-studio-2012/_static/image43.png "ログイン リンク スタイルの変更")

    *ログイン リンク スタイルの変更*
8. 保存**Style.css**ファイルおよびページの再読み込みするアドレスの下にあるバーを 1 回クリックします。 リンクが正しく表示されることに注意してください。

    ![リンクが右側に整列](using-page-inspector-in-visual-studio-2012/_static/image44.png "へのリンクが、右側に配置")

    *右揃えで配置へのリンク*
9. 最後に、ヘッダーのタイトルを変更します。 検索する代わりに、'**ここにロゴ '**すべてのファイル内のテキストでは、検査モードを使用してテキストをクリックし、生成されたソース コードを取得します。

    ![サイトのタイトルを検索する](using-page-inspector-in-visual-studio-2012/_static/image45.png "サイトのタイトルを検索します。")

    *サイトのタイトルを検索します。*
10. 使用するようになりました**Site.Master**、置換、'**ここにロゴ**'にテキスト'**フォト ギャラリー**' です。 保存して Page Inspector のブラウザーを更新します。

    ![フォト ギャラリー ページで更新](using-page-inspector-in-visual-studio-2012/_static/image46.png "更新フォト ギャラリー ページ")

    *フォト ギャラリー ページが更新されました*
11. 最後にキーを押して**f5 キーを押して**アプリを実行する、チェック アウトすべて変更が想定どおりに動作します。

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボを完了すると、Page Inspector を使用して、再構築し、ブラウザーで Web サイトを実行することがなく、Web アプリケーションをプレビューする方法を習得がします。 さらに、すばやく発見し、ソース コードに表示される出力から直接アクセスすることでバグを修正する方法を習得がします。

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>付録 a: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](using-page-inspector-in-visual-studio-2012/_static/image47.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express Web タイルを*
