---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: ID がコンテンツ ページ (VB) で名前付けを制御 |Microsoft ドキュメント
author: rick-anderson
description: ContentPlaceHolder のコントロールの名前付けコンテナーとして機能し、(FindConrol) を使用して困難なコントロールのプログラムで操作を行うためを示しています。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 288afbb6851e23de4725f9e6351ae12ccecefaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891501"
---
<a name="control-id-naming-in-content-pages-vb"></a>コンテンツ ページ (VB) で名前のコントロール ID
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> ContentPlaceHolder のコントロールの名前付けコンテナーとして機能し、(FindConrol) を使用して困難なコントロールのプログラムで操作を行うために示します。 この問題と回避策を見ます。 また、結果の ClientID 値にアクセスする方法をについて説明します。


## <a name="introduction"></a>はじめに

すべての ASP.NET サーバー コントロールに含まれる、`ID`プロパティを一意にコントロールを識別し、コントロール プログラムでアクセスを指定する、分離コード クラスの手段です。 同様に、HTML ドキュメント内の要素を含めることがあります、`id`要素を一意に識別する属性です。 これら`id`値でよく使用されますクライアント側スクリプトをプログラムで特定の HTML 要素を参照します。 そのため、可能性がありますを想定するを ASP.NET サーバー コントロールは、HTML に表示されるときにその`ID`値として使用されます、`id`レンダリングされた HTML 要素の値。 これは必ずしも大文字と小文字を 1 つの特定の状況で、1 つを制御するため`ID`値は、表示されるマークアップに複数回を表示することがあります。 GridView コントロールのラベルの Web コントロールに TemplateField を含むを検討してください、`ID`値`ProductName`です。 GridView が実行時にそのデータ ソースにバインドされると、このラベルは 1 回繰り返される GridView 行ごとにします。 ラベルのニーズの一意な各レンダリング`id`値。

このようなシナリオを処理するには、ASP.NET は、名前付けコンテナーとして使用する特定のコントロールを許可します。 名前付けコンテナーは、新しい機能`ID`名前空間。 名前付けコンテナー内に表示される任意のサーバー コントロールがある、レンダリングされた`id`値が付いて、`ID`名前付けコンテナー コントロールのです。 たとえば、`GridView`と`GridViewRow`クラスは、両方の名前付けコンテナーです。 その結果、ラベル コントロールでの GridView TemplateField で定義されている`ID` `ProductName` 、レンダリングされた指定`id`値`GridViewID_GridViewRowID_ProductName`です。 *GridViewRowID*は、その結果、GridView 行ごとに一意`id`値が一意。

> [!NOTE]
> [ `INamingContainer`インターフェイス](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx)は、特定の ASP.NET サーバー コントロールは、名前付けコンテナーとして機能する必要があることを示すために使用します。 `INamingContainer`インターフェイスがない略さずにすべてのメソッドをサーバー コントロールを実装する必要があります以外ではなく、マーカーとして使用されます。 表示されるマークアップを生成するには、コントロールがこのインターフェイスを実装する場合、ASP.NET エンジンは、自動的にプレフィックスの`ID`その下位の値が表示される`id`属性の値。 このプロセスは、手順 2. で詳しく説明しています。


名前付けコンテナーはだけでなく、レンダリングされた変更`id`属性値がどのようにコントロール参照できるはプログラムで、ASP.NET ページの分離コード クラスからにも影響します。 `FindControl("controlID")`メソッドは、通常、Web コントロールをプログラムで参照を使用します。 ただし、`FindControl`は名前付けコンテナーから侵入していません。 そのため、直接使用できません、 `Page.FindControl` GridView またはその他の名前付けコンテナー内でコントロールを参照するメソッド。

ようにする surmised 可能性がありますが、マスター ページと contentplaceholders には両方として実装名前付けコンテナー。 このチュートリアルでマスター ページに与える影響の HTML 要素を考察`id`値、およびコンテンツ ページを使用して、内の Web コントロールをプログラムで参照する方法に`FindControl`です。

## <a name="step-1-adding-a-new-aspnet-page"></a>手順 1: 新しい ASP.NET ページを追加します。

このチュートリアルで説明する概念を示すためには、当社の web サイトに新しい ASP.NET ページを追加してみましょう。 という名前の新しいコンテンツ ページを作成する`IDIssues.aspx`ルート フォルダーにバインドすることを`Site.master`マスター ページ。


![ルート フォルダーへのコンテンツ ページ IDIssues.aspx の追加します。](control-id-naming-in-content-pages-vb/_static/image1.png)

**図 01**: コンテンツ ページを追加`IDIssues.aspx`ルート フォルダーに


Visual Studio は、マスター ページの次の 4 つ contentplaceholders の各コンテンツ コントロールを自動的に作成されます。 説明したとおり、 [*複数 contentplaceholders にし、既定のコンテンツ*](multiple-contentplaceholders-and-default-content-vb.md)チュートリアルでは、コンテンツ コントロールが存在しない場合、マスター ページの既定の ContentPlaceHolder コンテンツが代わりに生成されます。 `QuickLoginUI`と`LeftColumnContent`contentplaceholders にこのページの適切な既定のマークアップを含む、進めておよびそれに対応する削除からコンテンツを制御`IDIssues.aspx`です。 この時点では、コンテンツ ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

[*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)ベース ページのカスタム クラスを作成したチュートリアル (`BasePage`) である場合に自動的にページのタイトルを構成します。明示的に設定します。 `IDIssues.aspx`この機能を使用する ページで、ページの分離コード クラスから派生しなければなりません、`BasePage`クラス (の代わりに`System.Web.UI.Page`)。 次のように見えるようには、分離コード クラスの定義を変更します。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

最後に、更新、`Web.sitemap`このレッスンで新しいエントリを追加するファイル。 追加、`<siteMapNode>`要素その`title`と`url`「コントロール ID の名前付けの問題」を属性と`~/IDIssues.aspx`、それぞれします。 この追加を行った後、`Web.sitemap`ファイルのマークアップは、次のようになります。


[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

図 2 に示す、新しいサイト マップ エントリに`Web.sitemap`左の列のレッスンでは、すぐに反映されます。


![レッスン セクションへのリンクを含むようになりました&quot;ID の名前付けの問題の制御&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**図 02**: レッスン セクションには、「コントロール ID の名前付けの問題」へのリンク


## <a name="step-2-examining-the-renderedidchanges"></a>手順 2: チェック、レンダリングされた`ID`の変更

エンジンで、表示されるには、ASP.NET の変更を理解するのには`id`サーバーの値を制御に、いくつかの Web コントロールを追加してみましょう。、`IDIssues.aspx`ページと、ブラウザーに送信される、表示されるマークアップを表示します。 具体的には、テキストを入力"年齢を入力してください:"TextBox Web コントロールを続けています。 さらにダウン ページで、ボタンの Web コントロールとコントロールを追加 Label Web です。 テキスト ボックスの設定`ID`と`Columns`プロパティ`Age`と 3、それぞれします。 ボタンの設定`Text`と`ID`プロパティを"Submit"と`SubmitButton`です。 ラベルのクリアを`Text`プロパティとその`ID`に`Results`です。

この時点で、コンテンツ コントロールの宣言型マークアップは、次のようになります。


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

図 3 は、Visual Studio のデザイナーを表示したときに、ページを示します。


[![このページには、次の 3 つの Web コントロールが含まれます。 テキスト ボックス、ボタン、およびラベル](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**図 03**:、ページが含まれます次の 3 つの Web コントロール: テキスト ボックス、ボタン、およびラベル ([フルサイズのイメージを表示するをクリックして](control-id-naming-in-content-pages-vb/_static/image5.png))


ブラウザーからのページにアクセスし、HTML ソースを表示します。 下の例では、マークアップとして、 `id`  テキスト ボックス、ボタン、およびラベル Web コントロールの HTML 要素の値は次の組み合わせ、 `ID` Web コントロールの値と`ID`ページ内の名前付けコンテナーの値。


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

述べたようにこのチュートリアルで前に、マスター ページとその contentplaceholders には名前付けコンテナーとして機能します。 その結果、両方とも投稿、レンダリングされた`ID`入れ子になったコントロールの値。 テキスト ボックスのかかる`id`属性のインスタンス:`ctl00_MainContent_Age`です。 注意してください ボックスにコントロールの`ID`値が`Age`です。 これは、プレフィックスが付きますその ContentPlaceHolder コントロールの`ID`値、`MainContent`です。 さらに、この値が付きます。 マスター ページの`ID`値、`ctl00`です。 実際の影響が、`id`属性値から成る、`ID`マスター ページ、ContentPlaceHolder コントロールとテキスト ボックス自体の値。

図 4 は、この動作を示します。 表示を決定する`id`の`Age` ボックスに、開始、 `ID` TextBox コントロールの値`Age`です。 次に、動作をコントロールの階層構造です。 各名前付けコンテナー (桃色色でそれらのノード) に表示される現在のプレフィックス`id`により、名前付けコンテナー`id`です。


![Rendered id は、属性ベースの ID 値の名前付けコンテナーの](control-id-naming-in-content-pages-vb/_static/image6.png)

**図 04**:、レンダリング`id`属性は、基に、`ID`名前付けコンテナーの値


> [!NOTE]
> 説明したよう、 `ctl00` 、レンダリングされたの部分`id`属性を構成、`ID`マスター ページがの値がいらっしゃるこの`ID`値が付属します。 おでを指定しなかった、任意の場所、コンテンツまたはマスター ページ。 ASP.NET ページ内のほとんどのサーバー コントロールは、ページの宣言型マークアップを通じて明示的に追加されます。 `MainContent` ContentPlaceHolder コントロールは、のマークアップで明示的に指定された`Site.master`; `Age`  テキスト ボックスが定義されている`IDIssues.aspx`のマークアップ。 指定すること、`ID`コントロールのプロパティ ウィンドウから、または宣言構文からのこれらの型の値。 マスター ページ自体と同様に、その他のコントロールは、宣言型マークアップで定義されていません。 その結果、その`ID`うえで、値を自動的に生成する必要があります。 ASP.NET エンジン セット、`ID`時にこれらのコントロールの Id を持つが明示的に設定されていない値です。 名前付けパターンを使用して`ctlXX`ここで、 *XX*順番に増加する整数値です。


マスター ページ自体を名前コンテナーとして機能し、ため、マスター ページで定義されている Web コントロールが変更も表示`id`属性の値。 たとえば、`DisplayDate`のマスター ページに追加したラベル、 [*マスター ページで、サイト全体のレイアウトを作成する*](creating-a-site-wide-layout-using-master-pages-vb.md)チュートリアルでは、次のマークアップを表示。


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

注意してください、`id`属性には、両方のマスター ページにはが含まれています。`ID`値 (`ctl00`) および`ID`ラベル Web コントロールの値 (`DateDisplay`)。

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>手順 3: 経由で Web コントロールをプログラムで参照します。`FindControl`

すべての ASP.NET サーバー コントロールが含まれています、`FindControl("controlID")`という名前のコントロールのコントロールの子孫を検索するメソッド*controlID*です。 このようなコントロールが見つかった場合、それが返されます。一致するコントロールが見つからない場合`FindControl`返します`Nothing`です。

`FindControl` ここでコントロールにアクセスする必要がありますが、それへの直接参照を持っていない場合に便利です。 宣言の構文では、GridView のフィールド内のコントロールが 1 回定義されているデータなどの GridView などの Web コントロールを使用する場合が GridView の行ごとに実行時に、コントロールのインスタンスが作成されました。 その結果、実行時に生成されたコントロールが存在するが、分離コード クラスから使用可能な直接参照はありません。 その結果を使用する必要があります`FindControl`GridView のフィールド内の特定のコントロールをプログラムで操作します。 (使用の詳細について`FindControl`データ Web コントロールのテンプレート内のコントロールにアクセスするを参照してください[カスタム書式指定ベース時にデータ](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md)。)。Web フォームに Web コントロールを動的に追加するときに、この同じシナリオが発生すると、トピックに記載[動的のデータ入力のユーザー インターフェイスを作成する](https://msdn.microsoft.com/library/aa479330.aspx)です。

使用して説明するために、`FindControl`コンテンツ ページでは、内のコントロールを検索する方法は、イベント ハンドラーを作成、`SubmitButton`の`Click`イベント。 イベント ハンドラーに次のコードは、プログラムで参照を追加、 `Age`  テキスト ボックスと`Results`を使用してラベル付け、`FindControl`メソッド内のメッセージを表示および`Results`ユーザーの入力に基づきます。

> [!NOTE]
> もちろん、使用する必要はありません`FindControl`この例のラベルとテキスト ボックス コントロールを参照します。 経由で直接には、参照でした、`ID`プロパティの値。 使用して`FindControl`を使用する場合の動作を説明するためにここ`FindControl`コンテンツ ページからです。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

呼び出しに使用する構文、`FindControl`メソッドは、最初の 2 つの行には若干異なります`SubmitButton_Click`、それらが等しい意味です。 すべての ASP.NET サーバー コントロールには、取り消し、`FindControl`メソッドです。 これが含まれています、`Page`クラス、すべて ASP.NET からは分離コード クラスから派生する必要があります。 そのため、`FindControl("controlID")`は呼び出すことと同じ`Page.FindControl("controlID")`、オーバーライドするいないと仮定した場合、`FindControl`分離コード クラスまたはカスタムの基本クラス メソッド。

このコードを入力すると、次を参照してください。、`IDIssues.aspx`ブラウザーを使ってページで、年齢を入力して"送信"ボタンをクリックします。 「送信」ボタンをクリックすると、`NullReferenceException`が発生した (図 5 を参照してください)。


[![NullReferenceException が発生しました。](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**図 05**: A`NullReferenceException`が発生した ([フルサイズのイメージを表示するをクリックして](control-id-naming-in-content-pages-vb/_static/image9.png))


ブレークポイントを設定した場合、`SubmitButton_Click`イベント ハンドラーの両方を呼び出すことがわかります`FindControl`返す`Nothing`です。 `NullReferenceException`にアクセスするときに発生するが、`Age`テキスト ボックスの`Text`プロパティです。

問題は`Control.FindControl`のみが検索*コントロール*の同じ名前付けコンテナー内にあるサブフォルダーが存在します。 マスター ページは、新しい名前付けコンテナーへの呼び出しを占めて`Page.FindControl("controlID")`マスター ページのオブジェクトが決して permeates`ctl00`です。 (表示コントロールの階層を表示する図 4 に戻って、`Page`マスター ページのオブジェクトの親としてオブジェクト`ctl00`)。したがって、`Results`ラベルと`Age` テキスト ボックスが見つからないと`ResultsLabel`と`AgeTextBox`の値が割り当てられます`Nothing`です。

この課題に 2 つの回避策がある: おドリルダウンできます、名前付けコンテナーを 1 つずつ、適切なコントロール作成できるよう、自分または`FindControl`名前付けコンテナーを permeates メソッドです。 これらの各オプションを調べてみましょう。

### <a name="drilling-into-the-appropriate-naming-container"></a>適切な名前付けコンテナーへのドリル イン

使用する`FindControl`参照に、`Results`ラベルまたは`Age` ボックスを呼び出す必要があります`FindControl`同じ名前付けコンテナー内の先祖コントロールからです。 図 4 で見たよう、 `MainContent` ContentPlaceHolder コントロールは、唯一の先祖の`Results`または`Age`同じ名前付けコンテナー内にあります。 つまり、呼び出し、`FindControl`メソッドから、`MainContent`コントロールを次のコード スニペットで示すように正しく返しますへの参照、`Results`または`Age`コントロール。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

ただし、ここを使用できません、`MainContent`マスター ページで、プレース ホルダーが定義されているために、上記の構文を使用して、コンテンツ ページの分離コード クラスから ContentPlaceHolder です。 使用する代わりに、必要`FindControl`への参照を取得する`MainContent`です。 コードで置き換え、`SubmitButton_Click`イベント ハンドラーで、次の変更。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

ブラウザーでページにアクセスする場合、年齢を入力し、「送信」ボタンをクリックして、`NullReferenceException`が発生します。 ブレークポイントを設定した場合、`SubmitButton_Click`イベント ハンドラー呼び出しを試みたときに、この例外が発生したことが表示されます、`MainContent`オブジェクトの`FindControl`メソッドです。 `MainContent`オブジェクトと等しい`Nothing`ため、`FindControl`メソッドは、「メイン」という名前のオブジェクトを見つけることはできません。 基になる理由と同じで、`Results`ラベルと`Age`TextBox コントロール:`FindControl`コントロール階層の最上位から検索を開始し、名前付けコンテナーが侵入していませんが、 `MainContent` ContentPlaceHolder はマスター ページで、名前付けコンテナーです。

使用する前に`FindControl`への参照を取得する`MainContent`、マスター ページのコントロールへの参照をまず必要があります。 参照を取得できます、マスター ページへの参照を取得したら、`MainContent`を介して ContentPlaceHolder`FindControl`し、そこを参照する、`Results`ラベルと`Age`TextBox (を使用して、もう一度`FindControl`)。 しかし、マスター ページへの参照を取得する方法をおでしょうか。 検査することによって、 `id` 、表示されるマークアップ内の属性は明らかをマスター ページの`ID`値は`ctl00`します。 したがってを使用して`Page.FindControl("ctl00")`マスター ページへの参照を取得するオブジェクトを使用してそのへの参照を取得する`MainContent`のようにします。 次のスニペットは、このロジックを示しています。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

このコードは間違いなく動作は中とみなされるマスター ページの自動生成された`ID`が必ず`ctl00`です。 ありません、自動生成された値に関する仮定をすることをお勧めします。

さいわい、マスター ページへの参照は経由でアクセスできる、`Page`クラスの`Master`プロパティです。 使用することではなく、したがって、`FindControl("ctl00")`にアクセスするために、マスター ページの参照を取得する、 `MainContent` ContentPlaceHolder、代わりに使用できる`Page.Master.FindControl("MainContent")`です。 更新プログラム、`SubmitButton_Click`イベント ハンドラーを次のコードで。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

このとき、ブラウザーからのページにアクセス、年齢を入力して、「送信」ボタンをクリックするとメッセージを表示、`Results`ラベル付け、期待どおりにします。


[![ユーザーの年齢がラベルに表示されます。](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**図 06**: ユーザーの年齢がラベルに表示されます ([フルサイズのイメージを表示するをクリックして](control-id-naming-in-content-pages-vb/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>名前付けコンテナーから検索を再帰的に

参照される前のコード例の理由、 `MainContent` ContentPlaceHolder コントロール、マスター ページからし、`Results`ラベルと`Age`からテキスト ボックスを制御`MainContent`、ため、`Control.FindControl`メソッドのみが検索内で*コントロール*の名前付けコンテナーです。 持つ`FindControl`名前付けコンテナー内で維持できるため適切ほとんどのシナリオで 2 つの異なる名前付けコンテナー内の 2 つのコントロールと同じである可能性があります`ID`値。 という名前のラベルの Web コントロールを定義する GridView の場合を考えます`ProductName`内でその TemplateFields のいずれか。 データが、実行時に GridView にバインドされている場合、 `ProductName` GridView 行ごとにラベルを作成します。 場合`FindControl`すべて名前付けを通じて検索とコンテナーと呼ばれる`Page.FindControl("ProductName")`、どのようなラベル インスタンスにする必要があります、`FindControl`返しますか? `ProductName` GridView の最初の行のラベルですか? 最後の行に 1 つですか。

ので、`Control.FindControl`だけ検索*コントロール*の名前付けコンテナーは、ほとんどの場合で意味します。 直接つながっている us, が、一意であるかなど、他の場合もありますが、`ID`名前付けコンテナーのすべてを対象としたく慎重コントロールにアクセスするコントロールの階層構造で各名前付けコンテナーを参照することです。 持つ、`FindControl`すべての名前付けコンテナーは、再帰的に検索理すぎるバリアント。 残念ながら、.NET Framework では、このようなメソッドは含まれません。

良いニュースを生み出すことができます独自`FindControl`メソッドを再帰的には、すべての名前付けコンテナーを検索します。 実際を使用して*拡張メソッド*を追加できる、`FindControlRecursive`メソッドを`Control`に添える、既存のクラス`FindControl`メソッドです。

> [!NOTE]
> 拡張メソッドは、.NET Framework version 3.5 および Visual Studio 2008 に付属している言語は、c# 3.0 と Visual Basic 9 に新しい機能です。 つまり、開発者が、特別な構文を既存のクラス型の新しいメソッドを作成するための拡張メソッドができるようにします。 この便利な機能の詳細については、私の記事を参照してください[拡張メソッドで基本型機能を拡張する](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)です。


拡張メソッドを作成するには、新しいファイルを追加、`App_Code`という名前のフォルダー`PageExtensionMethods.vb`です。 名前付き拡張メソッドを追加`FindControlRecursive`の入力として受け取る、`String`という名前のパラメーター`controlID`です。 正常に動作する拡張メソッドが、重要としてクラスをマークすること、`Module`拡張メソッドが付いたを指定して、`<Extension()>`属性。 さらに、すべての拡張メソッド受け入れる必要があります最初のパラメーターとして、型のオブジェクトに拡張メソッドが適用されます。

次のコードを追加、`PageExtensionMethods.vb`これを定義するファイル`Module`と`FindControlRecursive`拡張メソッド。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

このコードに戻ります、`IDIssues.aspx`ページの分離コード クラスと現在のコメント`FindControl`メソッドの呼び出しです。 呼び出しに置き換える`Page.FindControlRecursive("controlID")`です。 拡張メソッドに関する便利な新機能は、IntelliSense のドロップダウン リスト内で直接表示されることです。 図 7 に示すように入力すると`Page`期間を押すと、 `FindControlRecursive` IntelliSense と共に、他のドロップダウンでメソッドが含まれている`Control`クラスのメソッドです。


[![IntelliSense ドロップダウンには拡張メソッドが含まれます](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**図 07**: IntelliSense ドロップダウンには拡張メソッドが含まれます ([フルサイズのイメージを表示するをクリックして](control-id-naming-in-content-pages-vb/_static/image15.png))


次のコードを入力してください、`SubmitButton_Click`イベント ハンドラーをページにアクセスし、年齢の入力「送信」ボタンをクリックし、テストします。 図 6 に戻るように、結果として得られる出力は、メッセージになります、「歳の年齢が」!


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> 拡張メソッドは、Visual Studio 2005 を使用している場合、c# 3.0 および Visual Basic 9 の場合は、新しいために、拡張メソッドを使用できません。 代わりに、実装する必要があります、`FindControlRecursive`ヘルパー クラスのメソッドです。 [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx)彼のブログ投稿では、このような例が用意されて[メーザの ASP.NET ページと`FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)です。


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>手順 4: を使用して、正しい`id`クライアント側スクリプトの値の属性

Web コントロールの表示するように、このチュートリアルの概要で説明されて、`id`属性が多くの場合をプログラムによって特定の HTML 要素を参照するクライアント側のスクリプトで使用します。 たとえば、次の JavaScript に参照で HTML 要素の`id`し、モーダル メッセージ ボックスにその値を表示します。


[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

ASP.NET のページを再呼び出しの名前付けコンテナー、レンダリングされた HTML 要素の含めないでください`id`属性は、Web コントロールのと同じ`ID`プロパティの値。 このため、することは避けてでハード コード`id`JavaScript コードに属性の値。 つまり、わかっている場合にアクセスする、`Age`呼び出しにはクライアント側スクリプトを使用してテキスト ボックスの Web コントロール`document.getElementById("Age")`です。

この方法で問題となるはマスター ページ (またはその他の名前付けコンテナー コントロール) を使用してレンダリングされる HTML `id` Web コントロールのと同じ意味ではありません`ID`プロパティです。 ブラウザーを使ってページにアクセスし、実際にソースを表示する場合、最初の傾斜があります`id`属性。 わかったら、レンダリングされた`id`値、貼り付けることができますへの呼び出しに`getElementById`クライアント側スクリプトを使用する必要がある HTML 要素にアクセスします。 このアプローチをページの特定の変更が階層構造を制御するためより小さい最適または変更、`ID`名前付け、コントロールのプロパティが変更され、結果として得られる`id`属性、それによって、JavaScript コードを中断します。

良いニュースを`id`表示されている属性値は、Web コントロールの使用のサーバー側コードにアクセスできる[`ClientID`プロパティ](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)です。 このプロパティを使用する必要がありますを判断、`id`クライアント側のスクリプトで使用される値の属性です。 たとえば、JavaScript 関数をページに追加するため、呼び出されるの値を表示、`Age`モーダル メッセージ ボックスでは、テキスト ボックスに次のコードを追加する、`Page_Load`イベントのハンドラー。


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

上記のコードの値を挿入する、`Age`テキスト ボックスの`ClientID`プロパティに JavaScript の呼び出しに`getElementById`です。 ブラウザーからこのページにアクセスして、HTML ソースを表示すると、次の JavaScript コードがあります。


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

通知方法、正しい`id`属性値、`ctl00_MainContent_Age`への呼び出し内に表示されます`getElementById`です。 この値は実行時に計算、ために、ページ コントロール階層への変更に関係なく動作します。

> [!NOTE]
> この JavaScript の例は、単サーバー コントロールによって表示される HTML 要素を正しく参照する JavaScript 関数を追加する方法を示します。 この関数を使用するのには、ドキュメントが読み込まれるとき、または特定のユーザーの操作には、関数を呼び出すにはその他の JavaScript を作成する必要があります。 これらの詳細についてはおよび関連するトピックを読み[クライアント側スクリプトを実行](https://msdn.microsoft.com/library/aa479302.aspx)です。


## <a name="summary"></a>まとめ

特定の ASP.NET サーバー コントロールとして、名前付けコンテナー、レンダリングに影響する`id`によって canvassed コントロールのスコープだけでなく、その子孫のコントロールの値の属性、`FindControl`メソッドです。 マスター ページ、に関してマスター ページ自体とその ContentPlaceHolder コントロールの両方が名前付けコンテナーです。 その結果、定められてもう少しの作業をプログラムで参照を使用して、コンテンツ ページ内のコントロールを配置する必要があります`FindControl`です。 このチュートリアルでは 2 つの方法を調べるお: ContentPlaceHolder コントロールにドリルし、呼び出し元の`FindControl`メソッドと、独自のローリング`FindControl`すべての名前付けコンテナーを実装する再帰的に検索します。

問題に加え、サーバー側名前付けコンテナーを紹介 Web コントロールの参照に関するクライアント側の問題もします。 名前付けコンテナーの Web コントロールの休暇の`ID`プロパティの値と表示`id`属性値は、同じ 1 つです。 表示される名前付けのコンテナーの追加`id`属性には、両方が含まれる、 `ID` Web コントロールとそのコントロールの階層の先祖の名前付けコンテナーの値。 Web コントロールを使用する限り、これらの名前付けに関する注意事項が以外の問題をある`ClientID`プロパティをレンダリングされた`id`属性のクライアント側スクリプト内の値。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET マスター ページと `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [動的なデータ エントリのユーザー インターフェイスの作成](https://msdn.microsoft.com/library/aa479330.aspx)
- [拡張メソッドで基本型の機能を拡張します。](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [方法: ASP.NET マスター ページのコンテンツの参照](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [マスター ページ: ヒント、テクニック、およびトラップ](http://www.odetocode.com/articles/450.aspx)
- [クライアント側スクリプトの操作](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Zack Jones および Suchi Barnerjee がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)です。

> [!div class="step-by-step"]
> [前へ](urls-in-master-pages-vb.md)
> [次へ](interacting-with-the-master-page-from-the-content-page-vb.md)
