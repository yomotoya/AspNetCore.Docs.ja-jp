---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: コンテンツ ページ (c#) で ID の名前付けの制御 |Microsoft Docs
author: rick-anderson
description: ContentPlaceHolder のコントロールの名前付けコンテナーとして機能し、(FindConrol) を使用して困難なコントロールのプログラムで操作を行うために示しています.
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0c8617bb14c7023cfd926022b66c69bb5762758b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826720"
---
<a name="control-id-naming-in-content-pages-c"></a>コントロール ID の名前付け (c#) のコンテンツ ページ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> ContentPlaceHolder のコントロールの名前付けコンテナーとして機能し、(FindConrol) を使用して困難なコントロールのプログラムで操作を行うためには示しています。 この問題と回避策を見ます。 結果の ClientID 値をプログラムでアクセスする方法についても説明します。


## <a name="introduction"></a>はじめに

すべての ASP.NET サーバー コントロールが含まれる、`ID`プロパティを一意にコントロールを識別し、コントロールにプログラムで分離コード クラスにアクセスする手段です。 同様に、HTML ドキュメント内の要素を含めることができます、`id`要素を一意に識別する属性はこれら`id`を特定の HTML 要素をプログラムで参照の値がクライアント側スクリプトで使用頻度。 そのため、する可能性がありますある ASP.NET サーバー コントロールが html 形式にレンダリングされるときにその`ID`値として使用されます、`id`レンダリングされた HTML 要素の値。 これは必ずしも、特定の状況で、1 つの制御を 1 つのため`ID`値によって出力されるマークアップで複数回が表示されます。 含む TemplateField に Label Web コントロールに GridView コントロールを検討してください、 `ID` ProductName の値。 GridView は実行時にそのデータ ソースにバインドするときに GridView 行ごとにこのラベルが 1 回繰り返されます。 ラベルのニーズの一意な各レンダリング`id`値。

このようなシナリオを処理するには、ASP.NET は、名前付けコンテナーとして使用する特定のコントロールを許可します。 名前付けコンテナーは、新しい機能`ID`名前空間。 名前付けコンテナー内に表示される任意のサーバー コントロールがある、レンダリングされた`id`値が付いて、`ID`の名前付けコンテナー コントロール。 たとえば、`GridView`と`GridViewRow`クラスは、両方の名前付けコンテナー。 その結果、ラベル コントロールと GridView TemplateField で定義されている`ID`ProductName がレンダリングされる指定された`id`@property`GridViewID_GridViewRowID_ProductName`します。 *GridViewRowID*は、その結果、GridView 行ごとに一意です`id`値は一意です。

> [!NOTE]
> [ `INamingContainer`インターフェイス](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx)を特定の ASP.NET サーバー コントロールが、名前付けコンテナーとして機能する必要があることを示すために使用します。 `INamingContainer`インターフェイス サーバー コントロールを実装する必要がありますすべてのメソッドを詳しく規定は、代わりに、マーカーとして使用されます。 表示されるマークアップを生成するには、コントロールは、このインターフェイスを実装している場合、ASP.NET エンジン自動的にプレフィックスの`ID`子属性の値が表示される`id`属性の値。 このプロセスは手順 2. でさらに詳しく説明します。


名前付けコンテナーはだけでなく、レンダリングされた変更`id`属性、値がどのコントロール参照できるはプログラムで、ASP.NET ページの分離コード クラスからにも影響します。 `FindControl("controlID")`メソッドは、通常、Web コントロールをプログラムで参照を使用します。 ただし、`FindControl`名前付けコンテナーを通じて侵入はありません。 そのため、直接使用できません、 `Page.FindControl` GridView またはその他の名前付けコンテナー内のコントロールを参照するメソッド。

する surmised 可能性がありますが、マスター ページと ContentPlaceHolders 両方として実装されます名前付けコンテナー。 このチュートリアルではマスター ページに HTML 要素を調べる`id`値とを使用して、コンテンツ ページ内の Web コントロールをプログラムで参照する方法に`FindControl`します。

## <a name="step-1-adding-a-new-aspnet-page"></a>手順 1: 新しい ASP.NET ページの追加

このチュートリアルで説明する概念を示すためには、当社の web サイトに新しい ASP.NET ページを追加してみましょう。 という名前の新しいコンテンツ ページを作成する`IDIssues.aspx`ルート フォルダーにバインドすることを`Site.master`マスター ページ。


![コンテンツ ページ IDIssues.aspx をルート フォルダーに追加します。](control-id-naming-in-content-pages-cs/_static/image1.png)

**図 01**: コンテンツのページの追加`IDIssues.aspx`ルート フォルダーに


Visual Studio には、マスター ページの 4 つの ContentPlaceHolders の各コンテンツ コントロールを自動的に作成されます。 説明したとおり、 [*の複数の ContentPlaceHolders と既定のコンテンツ*](multiple-contentplaceholders-and-default-content-cs.md)チュートリアルでは、コンテンツ コントロールが存在しない場合、マスター ページの既定のプレース ホルダーのコンテンツが代わりに生成されます。 `QuickLoginUI`と`LeftColumnContent`ContentPlaceHolders このページの適切な既定のマークアップを含めることが進み、それに対応する削除からのコンテンツ コントロール`IDIssues.aspx`します。 この時点では、コンテンツ ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

[*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)ベース ページのカスタム クラスを作成したチュートリアル (`BasePage`) である場合に自動的にページのタイトルを構成します。明示的に設定します。 `IDIssues.aspx`この機能を使用する ページで、ページの分離コード クラスがから派生する必要があります、`BasePage`クラス (の代わりに`System.Web.UI.Page`)。 次のように見えるように、分離コード クラスの定義を変更します。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

最後に、更新、`Web.sitemap`このレッスンで新しいエントリを追加するファイル。 追加、`<siteMapNode>`要素その`title`と`url`属性を「コントロール ID 名前付けの問題」と`~/IDIssues.aspx`、それぞれします。 この追加を行った後、`Web.sitemap`ファイルのマークアップは、次のようになります。


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

図 2 に示すように、新しいサイト マップ エントリ`Web.sitemap`レッスン」の左側の列にすぐに反映されます。


![レッスン セクションがへのリンクが含まれています&quot;ID の名前付けの問題を制御します。&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**図 02**: レッスン セクションが「コントロール ID の名前付けの問題」へのリンクが含まれています


## <a name="step-2-examining-the-renderedidchanges"></a>手順 2: 確認、レンダリングされた`ID`の変更

エンジンで、ASP.NET の変更を理解するのには、表示する、`id`サーバーの値を制御は、いくつかの Web コントロールを追加、`IDIssues.aspx`ページし、ブラウザーに送信、表示されるマークアップを表示します。 具体的には、テキストを入力"お客様の年齢を入力してください:"TextBox Web コントロールを続けています。 さらにダウン ページを追加 ボタンの Web コントロールと Label Web コントロール。 テキスト ボックスの設定`ID`と`Columns`プロパティを`Age`と 3 では、それぞれします。 ボタンの設定`Text`と`ID`「送信」のプロパティと`SubmitButton`します。 ラベルのクリアします`Text`プロパティとその`ID`に`Results`します。

この時点で、コンテンツ コントロールの宣言型マークアップは、次のようになります。


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

図 3 は、Visual Studio のデザイナーを使用して表示する際、ページを示します。


[![このページには、次の 3 つの Web コントロールが含まれます。、テキスト ボックス、ボタン、およびラベル](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**図 03**:、ページが含まれます次の 3 つの Web コントロール テキスト ボックス、ボタン、およびラベル ([フルサイズの画像を表示する をクリックします。](control-id-naming-in-content-pages-cs/_static/image5.png))。


ブラウザーを使用してページを参照してくださいし、HTML ソースを表示します。 下の例では、マークアップとして、`id`テキスト ボックス、ボタン、およびラベルの Web コントロールの HTML 要素の値は、の組み合わせ、 `ID` Web コントロールの値と`ID`ページ内の名前付けコンテナーの値。


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

このチュートリアルで触れたように、マスター ページとその ContentPlaceHolders の両方が名前付けコンテナーとして機能します。 そのため、どちらも投稿、レンダリングされた`ID`入れ子になったコントロールの値。 テキスト ボックスの`id`属性は、たとえば:`ctl00_MainContent_Age`します。 テキスト ボックス コントロールの`ID`値が`Age`します。 これがそのプレース ホルダー コントロールに付いて`ID`値、`MainContent`します。 この値が、マスター ページの付いたさらに、`ID`値、`ctl00`します。 実際の効果は、`id`属性の値から成る、`ID`マスター ページ、プレース ホルダー コントロール、およびテキスト ボックス自体の値。

図 4 は、この動作を示します。 表示を決定する`id`の`Age` ボックスに、開始、 `ID` 、TextBox コントロールの値`Age`します。 次に、動作、コントロールの階層構造です。 各名前付けコンテナー (ピーチ色でそれらのノード) に表示される現在のプレフィックス`id`名前付けコンテナーの`id`します。


![レンダリングされた id 属性は、ベースで、ID 値の名前付けコンテナー](control-id-naming-in-content-pages-cs/_static/image6.png)

**図 04**: The レンダリング`id`属性は、基に、`ID`名前付けコンテナーの値


> [!NOTE]
> 前述のように、 `ctl00` 、レンダリングされたの部分`id`属性を構成、`ID`がマスター ページで、値が疑問この`ID`値が付属しています。 指定しなかった、任意の場所で、コンテンツまたはマスター ページ。 ほとんどのサーバー コントロール、ASP.NET ページには、ページの宣言型マークアップを明示的に追加されます。 `MainContent`のマークアップで指定されたプレース ホルダー コントロール`Site.master`、 `Age` TextBox が定義されている`IDIssues.aspx`のマークアップ。 指定すること、`ID`コントロールのプロパティ ウィンドウから、または宣言型構文からのこれらの型の値。 自体は、マスター ページと同様に、他のコントロールは、宣言型マークアップで定義されていません。 その結果、その`ID`私たちにとって、値を自動的に生成する必要があります。 ASP.NET のエンジン セット、`ID`時に、それらのコントロール Id を持つが明示的に設定されていない値。 名前付けパターンを使用して`ctlXX`ここで、 *XX*は連の整数値です。


マスター ページ自体名前付けコンテナーとして機能のため、マスター ページで定義されている Web コントロールも変更されてレンダリングされた`id`属性の値。 たとえば、`DisplayDate`ラベルでマスター ページに追加しました、 [*マスター ページで、サイト全体レイアウトを作成する*](creating-a-site-wide-layout-using-master-pages-cs.md)チュートリアルでは、次のマークアップを表示。


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

なお、`id`属性に両方のマスター ページの`ID`値 (`ctl00`) および`ID`Label Web コントロールの値 (`DateDisplay`)。

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>手順 3: を使用して Web コントロールをプログラムで参照します。`FindControl`

すべての ASP.NET サーバー コントロールが含まれています、`FindControl("controlID")`という名前のコントロールのコントロールの子孫を検索するメソッド*controlID*します。 このようなコントロールが見つかった場合に返されます。一致するコントロールが見つからない場合`FindControl`返します`null`します。

`FindControl` コントロールにアクセスする必要があることへの直接の参照がないシナリオに便利です。 宣言の構文で、GridView のフィールド内のコントロールが 1 回定義されているデータ Web コントロール、GridView などを使用する場合が GridView 行ごとに実行時に、コントロールのインスタンスを作成します。 その結果、実行時に生成されたコントロールが存在するが、分離コード クラスから使用可能な直接参照はありません。 その結果を使用する必要があります`FindControl`GridView のフィールド内の特定のコントロールをプログラムで操作します。 (使用の詳細については`FindControl`データ Web コントロールのテンプレート内のコントロールにアクセスするを参照してください[カスタム書式設定時にデータ](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md)。)。Web フォームに Web コントロールを動的に追加するときに、この同じシナリオが発生するで説明したトピック[動的のデータ エントリのユーザー インターフェイスを作成する](https://msdn.microsoft.com/library/aa479330.aspx)します。

使用して説明するために、`FindControl`コンテンツ ページでは、コントロールを検索するメソッドのイベント ハンドラーを作成する、`SubmitButton`の`Click`イベント。 イベント ハンドラーで、次のコードは、プログラムで参照を追加、 `Age` TextBox と`Results`ラベルを使用して、`FindControl`メソッドでメッセージを表示および`Results`ユーザーの入力に基づきます。

> [!NOTE]
> 使用する必要はありませんもちろん、`FindControl`この例のラベルとテキスト ボックス コントロールを参照します。 直接使用して参照でした、`ID`プロパティの値。 使用して`FindControl`を使用する場合の動作を説明するためにここ`FindControl`コンテンツ ページから。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

呼び出しに使用する構文、`FindControl`の最初の 2 つの行にメソッドが若干`SubmitButton_Click`と同じ意味ですが。 すべての ASP.NET サーバー コントロールを含めることを思い出してください、`FindControl`メソッド。 これが含まれています、`Page`クラス、すべて ASP.NET からは分離コード クラスから派生する必要があります。 そのため、`FindControl("controlID")`呼び出しと同じですが`Page.FindControl("controlID")`、オーバーライドするいないと仮定すると、`FindControl`分離コード クラスまたはカスタム基底クラス メソッド。

このコードを入力した後、次を参照してください。、`IDIssues.aspx`ブラウザーからページで、お客様の年齢を入力し、[送信] ボタンをクリックします。 [送信] ボタンをクリックすると、`NullReferenceException`が発生します (図 5 を参照してください)。


[![NullReferenceException が発生します](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**図 05**: A`NullReferenceException`が発生します ([フルサイズの画像を表示する をクリックします](control-id-naming-in-content-pages-cs/_static/image9.png))。


ブレークポイントを設定した場合、`SubmitButton_Click`イベント ハンドラーの両方を呼び出すことがわかります`FindControl`を返す、`null`値。 `NullReferenceException`にアクセスするときに発生、`Age`テキスト ボックスの`Text`プロパティ。

問題なは`Control.FindControl`のみが検索*コントロール*の子孫である*で同じ名前付けコンテナー*します。 マスター ページは、新しい名前付けコンテナーへの呼び出しを構成するため`Page.FindControl("controlID")`マスター ページのオブジェクトが決して permeates`ctl00`します。 (表示するコントロール階層を表示する図 4 に戻って、`Page`マスター ページのオブジェクトの親としてオブジェクト`ctl00`)。そのため、`Results`ラベルと`Age`TextBox が見つからないと`ResultsLabel`と`AgeTextBox`の値が割り当てられている`null`します。

このチャレンジに 2 つの回避策があります適切なコントロールに、時、ドリル ダウン、1 つの名前付けコンテナーできます。または、独自に作成しますできます`FindControl`名前付けコンテナーを permeates メソッド。 これらの各オプションを調べてみましょう。

### <a name="drilling-into-the-appropriate-naming-container"></a>適切な名前付けコンテナーへのドリル ダウン

使用する`FindControl`参照に、`Results`ラベルまたは`Age` ボックスを呼び出す必要があります`FindControl`同じ名前付けコンテナー内の先祖コントロールから。 図 4 が示すように、`MainContent`プレース ホルダー コントロールは、唯一の先祖の`Results`または`Age`同じ名前付けコンテナー内にあります。 つまり、呼び出し、`FindControl`からメソッド、`MainContent`コントロールを次のコード スニペットで示すように正しく返すへの参照、`Results`または`Age`コントロール。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

ただしで動作したことはできません、`MainContent`マスター ページで、プレース ホルダーが定義されているために、上記の構文を使用して、コンテンツ ページの分離コード クラスから得ることです。 使用する代わりに、私たちが`FindControl`への参照を取得する`MainContent`します。 コードに置き換えます、`SubmitButton_Click`イベント ハンドラーで、次の変更。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

ブラウザーを使用してページを閲覧する場合、お客様の年齢を入力し、[送信] ボタンをクリックして、`NullReferenceException`が発生します。 ブレークポイントを設定した場合、`SubmitButton_Click`イベント ハンドラーの呼び出しを試みるときに、この例外が発生したことが表示されます、`MainContent`オブジェクトの`FindControl`メソッド。 `MainContent`オブジェクトが`null`ため、`FindControl`メソッドは"MainContent"という名前のオブジェクトを見つけることはできません。 基になる理由と同様、`Results`ラベルと`Age`テキスト ボックス コントロール:`FindControl`コントロール階層の最上位から検索を開始し、名前付けコンテナーを通過しないが、`MainContent`プレース ホルダーは、内での名前付けコンテナーは、マスター ページです。

使用する前に`FindControl`への参照を取得する`MainContent`、マスター ページのコントロールへの参照をまず必要があります。 マスター ページへの参照を取得したらへの参照を取得できます、`MainContent`プレース ホルダーを使用して`FindControl`、そこを参照して、`Results`ラベルと`Age`TextBox (を使用して、もう一度、 `FindControl`)。 しかし、マスター ページへの参照を取得する方法をでしょうか。 調べることによって、`id`出力されるマークアップ内の属性は明らかですが、マスター ページの`ID`値は`ctl00`。 したがって、使用してでした`Page.FindControl("ctl00")`マスター ページへの参照を取得するへの参照を取得し、そのオブジェクトを使用`MainContent`など。 次のスニペットは、このロジックを示しています。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

これでこのコードが確実に機能しますが、マスター ページの自動生成された`ID`は常に`ctl00`します。 ありません、自動生成された値を推測することをお勧めします。

さいわい、マスター ページへの参照はからアクセスできる、`Page`クラスの`Master`プロパティ。 使用することではなく、そのため、`FindControl("ctl00")`にアクセスするためにマスター ページの参照を取得する、 `MainContent` ContentPlaceHolder、代わりに使用できる`Page.Master.FindControl("MainContent")`します。 更新プログラム、`SubmitButton_Click`イベント ハンドラーを次のコード。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

今回は、ブラウザーでページにアクセスして、お客様の年齢を入力し、[送信] ボタンをクリックしてメッセージを表示、`Results`ラベル、期待どおりにします。


[![ユーザーの年齢がラベルに表示されます。](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**図 06**:、ユーザーの年齢がラベルに表示されます ([フルサイズの画像を表示する をクリックします](control-id-naming-in-content-pages-cs/_static/image12.png))。


### <a name="recursively-searching-through-naming-containers"></a>再帰的に名前付けコンテナーを検索

理由の前のコード例を参照、`MainContent`からマスター ページで、プレース ホルダー コントロールをクリックし、`Results`ラベルと`Age`TextBox コントロールを`MainContent`、ため、`Control.FindControl`メソッドのみが検索内で*コントロール*の名前付けコンテナー。 ある`FindControl`名前付けコンテナー内で常にかなってほとんどのシナリオで 2 つの異なる名前付けコンテナー内の 2 つのコントロールでは、同じことがあるため`ID`値。 GridView という名前のラベルの Web コントロールを定義する場合を考えます`ProductName`内でその TemplateFields のいずれか。 データが、実行時に GridView にバインドされている場合、 `ProductName` GridView 行ごとにラベルを作成します。 場合`FindControl`検索すべて名前付けと呼ばれるコンテナーと`Page.FindControl("ProductName")`、どのようなラベル インスタンスにする必要があります、`FindControl`返すでしょうか。 `ProductName` GridView の最初の行のラベルか? 最後の行に 1 つですか。

ので、`Control.FindControl`だけ検索*コントロール*の名前付けコンテナーはほとんどの場合で意味が。 接続する、一意のあるものなど、他のケースがありますが、`ID`すべてのコンテナーの名前付けとアクセスを制御するコントロール階層内の各名前付けコンテナーを慎重に参照しなくてもすむようにします。 `FindControl`すぎてすべての名前付けコンテナーは、再帰的に検索が検出するバリアント。 残念ながら、.NET Framework では、このようなメソッドは含まれません。

さいわい、作成することだことができます独自`FindControl`メソッドを再帰的には、すべての名前付けコンテナーを検索します。 実際を使用して*拡張メソッド*を追加できる、`FindControlRecursive`メソッドを`Control`クラスに、既存の付属する`FindControl`メソッド。

> [!NOTE]
> 拡張メソッドは、c# 3.0 および Visual Basic 9 では、.NET Framework version 3.5 および Visual Studio 2008 に付属している言語に新しい機能です。 つまり、開発者が、特別な構文を使用、既存のクラス型の新しいメソッドを作成するための拡張機能メソッドを使用します。 この便利な機能の詳細については、筆者の記事を参照してください[拡張メソッドで基本型機能を拡張する](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)します。


拡張メソッドを作成するには、新しいファイルを追加、`App_Code`という名前のフォルダー`PageExtensionMethods.cs`します。 という名前の拡張メソッドを追加`FindControlRecursive`の入力として受け取る、`string`という名前のパラメーター`controlID`します。 正常に動作する拡張メソッドは、クラス自体とその拡張メソッドをマークする重要な`static`します。 拡張メソッドを適用する型のオブジェクトの最初のパラメーターとこの入力パラメーターの前、キーワードにする必要がありますにすべての拡張メソッドをさらに、同意する必要があります`this`します。

次のコードを追加、`PageExtensionMethods.cs`クラス ファイルをこのクラスを定義して、`FindControlRecursive`拡張メソッド。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

このコードに戻ります、`IDIssues.aspx`ページの分離コード クラスと現在のコメント`FindControl`メソッドの呼び出し。 呼び出しに置き換える`Page.FindControlRecursive("controlID")`します。 拡張メソッドの優れている点は、IntelliSense のドロップダウン リスト内で直接表示します。 図 7 に示すようページ」と入力して、期間をヒットし、`FindControlRecursive`メソッドは、IntelliSense と共に他のドロップダウン リストに含まれている`Control`クラス メソッド。


[![拡張メソッドは IntelliSense ドロップダウン リストに含まれる](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**図 07**: IntelliSense ドロップダウン リストで拡張メソッドが含まれます ([フルサイズの画像を表示する をクリックします](control-id-naming-in-content-pages-cs/_static/image15.png))。


次のコードを入力します、`SubmitButton_Click`イベント ハンドラーをページにアクセスして、お客様の年齢を入力し、[送信] ボタンをクリックしをテストします。 図 6 に戻るように、結果の出力は、メッセージ、「歳の年齢は」!


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> 拡張メソッドは、Visual Studio 2005 を使用している場合に、c# 3.0 および Visual Basic 9 では、新しいため、拡張メソッドを使用することはできません。 代わりに、実装する必要があります、`FindControlRecursive`ヘルパー クラスのメソッド。 [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx)で彼のブログ記事では、このような例が用意されて[メーザの ASP.NET ページと`FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)します。


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>手順 4: を使用して、正しい`id`属性の値でクライアント側スクリプト

Web コントロールのレンダリングでこのチュートリアルの概要を記載の通り`id`属性が特定の HTML 要素をプログラムで参照するクライアント側スクリプトで使用多くの場合。 次の JavaScript で HTML 要素を参照する場合など、`id`し、その値をモーダル メッセージ ボックスに表示します。


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

ASP.NET ページには、名前付けコンテナー、レンダリングされた HTML 要素を含めないでください`id`属性は、Web コントロールのと同じ`ID`プロパティの値。 このためがでハード コーディングすることも考え`id`JavaScript コードに属性値。 つまり、わかっている場合にアクセスする、`Age`への呼び出しを使用して、クライアント側のスクリプトをテキスト ボックスに Web コントロールがそのため`document.getElementById("Age")`します。

マスター ページ (または他の名前付けコンテナー コントロール) を使用しているこのアプローチの問題がレンダリングされる HTML `id` Web コントロールのとは異なります`ID`プロパティ。 ブラウザーを使用してページにアクセスし、実際にソースを表示する場合、まず候補があります`id`属性。 レンダリングがわかれば`id`値を貼り付けることができますへの呼び出しに`getElementById`クライアント側スクリプトを使用する必要がある HTML 要素にアクセスします。 このアプローチは、特定の変更をページの階層を制御するために遅くなりますがまたはへの変更、`ID`名前付けのコントロールのプロパティは、その結果を alter`id`しまい、JavaScript コードの属性。

良い知らせは、`id`表示される属性値は、Web コントロールのサーバー側コードでアクセス可能な[`ClientID`プロパティ](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)します。 このプロパティを使用して確認する必要があります、`id`クライアント側スクリプトで使用される値の属性します。 たとえば、ページに JavaScript 関数を追加するため、呼び出されるの値を表示、 `Age` 、モーダル メッセージ ボックスにテキスト ボックスに次のコードを追加する、`Page_Load`イベント ハンドラー。


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

上記のコードの値を挿入する、 `Age` JavaScript の呼び出しにテキスト ボックスの ClientID プロパティ`getElementById`します。 ブラウザーからこのページを参照してください、HTML ソースを表示すると、次の JavaScript コードがあります。


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

通知方法、適切な`id`属性値、`ctl00_MainContent_Age`への呼び出し内に表示されます`getElementById`。 この値は実行時に計算、ために、ページのコントロール階層構造への変更に関係なく動作します。

> [!NOTE]
> この JavaScript の例は、単なるサーバー コントロールによって表示される HTML 要素を正しく参照する JavaScript 関数を追加する方法を示します。 この関数を使用するには、ドキュメントが読み込まれるとき、または特定のユーザーの操作には、関数を呼び出す追加の JavaScript を作成する必要があります。 これらの詳細についてはおよび関連するトピックを読む[クライアント側スクリプトの操作](https://msdn.microsoft.com/library/aa479302.aspx)します。


## <a name="summary"></a>まとめ

レンダリングに影響する、名前付けコンテナーとしての ASP.NET サーバー コントロールの機能を特定`id`その子孫のコントロールの値として回りましたコントロールのスコープを属性、`FindControl`メソッド。 マスター ページ、に関してマスター ページ自体とそのプレース ホルダー コントロールの両方が名前付けコンテナー。 その結果、コンテンツ ページを使用して、コントロールをプログラムで参照するなど、少し多くの作業を配置する必要があります`FindControl`します。 このチュートリアルでは 2 つの手法を調べる: プレース ホルダー コントロールにドリルし、呼び出し元の`FindControl`メソッドと、独自のロール`FindControl`すべての名前付けコンテナーを通じてその再帰検索を実装します。

名前付けコンテナーの導入に関して Web コントロールを参照しているサーバー側の問題、だけでなくクライアント側の問題もあります。 名前付けコンテナーの Web コントロールの休暇で`ID`プロパティの値と表示`id`属性の値は、同じ 1 つ。 表示される名前付けコンテナーの追加`id`属性には、両方が含まれて、 `ID` Web コントロールとそのコントロール階層の先祖で名前付けコンテナーの値。 これらの名前付けの問題では、Web コントロールの使用する限り、問題以外を`ClientID`プロパティの表示、`id`属性の値は、クライアント側スクリプトでします。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET マスター ページと `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [動的なデータ入力ユーザー インターフェイスを作成します。](https://msdn.microsoft.com/library/aa479330.aspx)
- [拡張メソッドで基本データ型の機能を拡張します。](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [方法: ASP.NET マスター ページのコンテンツの参照](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [マスター ページ: ヒント、テクニック、およびトラップ](http://www.odetocode.com/articles/450.aspx)
- [クライアント側スクリプトの操作](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Zack Jones と Suchi Barnerjee でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)します。

> [!div class="step-by-step"]
> [前へ](urls-in-master-pages-cs.md)
> [次へ](interacting-with-the-master-page-from-the-content-page-cs.md)
