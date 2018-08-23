---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: ASP.NET Web Pages の概要 - 一貫性のあるレイアウトを作成する |Microsoft Docs
author: tfitzmac
description: このチュートリアルでは、レイアウトを使用して ASP.NET Web Pages を使用しているサイトで、ページの一貫性のある外観を作成する方法を示します。 完了したと想定して、.
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 8f3d9e8a6f6a0179224e18faf11db3dc1510a095
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831886"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>ASP.NET Web ページの概要 - 一貫性のあるレイアウトを作成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルは、使用する方法を示します*レイアウト*ASP.NET Web Pages を使用するサイトで、ページの一貫性のある外観を作成します。 を通じてシリーズを完了したと想定して[データベースのデータを削除する ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251584)します。
> 
> 学習内容。
> 
> - どのようなレイアウト ページは次のとおりです。
> - 動的なコンテンツとレイアウト ページを結合する方法。
> - レイアウト ページに値を渡す方法。


## <a name="about-layouts"></a>レイアウトについて

これまでに作成したページがすべて完了したら、スタンドアロンのページ。 すべて、同じサイトに属するが、共通の要素または標準の外観がないです。

ほとんどのサイトには一貫性のある外観とレイアウトをが。 たとえばに移動する場合、 [Microsoft.com/web](https://www.microsoft.com/web/)サイト、少し調べてみると、すべてのページが全体的なレイアウトをビジュアル テーマを従うことを参照してください。

![Microsoft.com/web サイトのページ ヘッダー、ナビゲーション領域、コンテンツ領域、およびフッターのレイアウトを示す](layouts/_static/image1.png)

*非効率的な*このレイアウトを作成する方法は各ページで、ヘッダー、ナビゲーション バーで、およびフッターを個別に定義することです。 複製するのではする同じマークアップを毎回。 何かを変更する場合 (たとえば、更新、フッター)、各ページを個別に変更する必要があります。

ここ*レイアウト ページ*取得します。 ASP.NET Web ページで、サイトのページの全体のコンテナーを提供するレイアウト ページを定義できます。 たとえば、レイアウト ページでは、ヘッダー、ナビゲーションの領域およびフッターを含めることができます。 レイアウト ページには、メイン コンテンツの行き先プレース ホルダーが含まれています。

マークアップおよびコード ページのみを含めることが個々 のコンテンツ ページを定義することができます。 コンテンツ ページは、完全な HTML ページを使用する必要はありません。必要すらありません、`<body>`要素。 コンテンツを表示するレイアウト ページを ASP.NET に指示するコードの行もあります。 このリレーションシップのしくみをほぼ表示する図を次に示します。

![2 つのコンテンツ ページと適合するレイアウト ページを表示する概念図](layouts/_static/image2.png)

これは、簡単に動作を確認するときに理解できます。 このチュートリアルでは、映画のページ レイアウトを使用するを変更します。

## <a name="adding-a-layout-page"></a>レイアウト ページを追加します。

ヘッダー、フッター、メイン コンテンツ領域と一般的なページ レイアウトを定義するレイアウト ページの作成から始めます。 WebPagesMovies サイトの追加という名前の CSHTML ページ *\_Layout.cshtml*します。

先頭のアンダー スコア ( `_` ) 文字は重要です。 ページの名前がアンダー スコアで始まる場合は、ASP.NET がブラウザーにそのページを送信しません直接。 この規則では、ユーザーが直接要求したりすることはできませんが、サイトに必要なページを定義できます。

次のように、ページのコンテンツを置き換えます。

[!code-html[Main](layouts/samples/sample1.html)]

ご覧のように、このマークアップを使用するように HTML`<div>`複数ページに 1 を加えたで 3 つのセクションを定義する要素`<div>`3 つのセクションを保持する要素。 フッターには Razor コードが含まれています:`@DateTime.Now.Year`ページ内の場所に現在の年を表示します。

という名前のスタイル シートへのリンクがあることに注意してください。 *Movies.css*します。 スタイル シートは、要素の物理的なレイアウトの詳細を定義します。 すぐに作成します。

これでのみ通常とは異なる機能 *\_Layout.cshtml*ページは、`@Render.Body()`行。 このレイアウトを結合する別のページで、コンテンツのどこのプレース ホルダーです。

## <a name="adding-a-css-file"></a>.Css ファイルを追加します。

カスケード スタイル シート (CSS) ルールを使用することのページ要素の実際の配置 (つまり、外観) を定義することをお勧めです。 作成できるように、 *.css*新しいレイアウトの規則を含むファイル。

WebMatrix で、サイトのルートを選択します。 次に、**ファイル** タブのリボンの下の矢印をクリックします。、**新規**ボタンをクリックし、 をクリックし、**新しいフォルダー**。

![リボンの [新規] [新しいフォルダー] オプション。](layouts/_static/image3.png)

新しいフォルダーの名前*スタイル*します。

![新しいフォルダー 'スタイル' の名前付け](layouts/_static/image4.png)

新しい内部*スタイル*フォルダー、という名前のファイルを作成する*Movies.css*します。

![新しい Movies.css ファイルを作成します。](layouts/_static/image5.png)

新しい内容を置き換える *.css*を次のファイル。

[!code-css[Main](layouts/samples/sample2.css)]

以外に 2 つのことに注意してください。 これらの CSS ルールの詳細については示さことはありません。 1 つは、フォントとサイズを設定するだけでなく、使用しているルール絶対配置ヘッダー、フッター、およびメイン コンテンツ領域の場所を確立するためにです。 読むことができるかどうか、新しい CSS の配置にしたら、 [CSS 配置](http://www.w3schools.com/css/css_positioning.asp)W3Schools サイトのチュートリアル。

もう 1 つがで個別に定義されている下部で、コピーした元々 のスタイル ルールに注意してください、 *Movies.cshtml*ファイル。 これらの規則が使用されていた、[を表示するデータを使用して ASP.NET Web Pages の概要](https://go.microsoft.com/fwlink/?LinkId=251580)させるチュートリアル、`WebGrid`ヘルパーがストライプをテーブルに追加するマークアップをレンダリングします。 (使用する場合、 *.css*ファイル スタイルの定義がサイト全体のスタイル ルールにも配置する場合があります)。

## <a name="updating-the-movies-file-to-use-the-layout"></a>レイアウトを使用するビデオ ファイルの更新

これで、新しいレイアウトを使用するサイト内の既存のファイルを更新できます。 開く、 *Movies.cshtml*ファイル。 上部にある、コードの最初の行として以下を追加します。

[!code-csharp[Main](layouts/samples/sample3.cs)]

ページは、この方法を今すぐ開始します。

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

この 1 行のコードが ASP.NET に指示する場合、*映画*ページが実行されるとにマージするか、  *\_Layout.cshtml*ファイル。

以降、 *Movies.cshtml*ファイルはこれでレイアウト ページを使用して、マークアップからを削除することができます、 *Movies.cshtml*によって対処がページ、  *\_Layout.cshtml*ファイル。 `<!DOCTYPE>`、 `<html>`、および`<body>`開始タグと終了します。 全体を`<head>`要素とその内容これらのルールを取得したため、グリッドのスタイル ルールが含まれています、 *.css*ファイル。 ついでに、変更、既存`<h1>`要素を`<h2>`要素; が、`<h1>`レイアウト ページを既に要素。 変更、 `<h2>` "一覧 Movies"するテキスト。

通常は、コンテンツ ページでこの種の変更を有効にする必要はありません。 レイアウト ページ サイトを開始するときに、まず、これらすべての要素のコンテンツ ページを作成します。 ここでは、ただし、クリーンアップの一部があるために、レイアウトを使用する 1 つに、スタンドアロン ページを変換しています。

操作を完了するときに、 *Movies.cshtml*ページは、次のようになります。

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>レイアウトのテスト

これで、レイアウトがどのようにを確認できます。 WebMatrix で、右クリックし、 *Movies.cshtml*  ページ**ブラウザーで起動**します。 ブラウザーでは、ページを表示するときは、このページのようになります。

![レイアウトを使用して表示されるビデオ ページ](layouts/_static/image6.png)

ASP.NET に Movies.cshtml ページのコンテンツをマージ、  *\_Layout.cshtml*適切な場所のページ、`RenderBody`メソッドします。 もちろん、  *\_Layout.cshtml*ページを参照する、 *.css*ページの外観を定義するファイル。

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>レイアウトを使用する AddMovie ページを更新しています

レイアウトの実際の利点は、ことできますで使用するすべてのページで、サイトです。 開く、 *AddMovie.cshtml*ページ。

覚えている可能性があります、 *AddMovie.cshtml*  ページでは、検証エラー メッセージの外観を定義するために、いくつかの CSS 規則を最初から。 あるので、 *.css*これで、サイトのファイルは、それらの規則を移動する、 *.css*ファイル。 削除、 *AddMovie.cshtml*の下部に追加ファイルを開き、 *Movies.css*ファイル。 次の規則を移動します。

[!code-css[Main](layouts/samples/sample6.css)]

次に、同じ種類の変更を加えます*AddMovie.cshtml*の*Movies.cshtml* -追加`Layout="~/_Layout.cshtml;`および削除するには余分な HTML マークアップ。 `<h1>` 要素を `<h2>` に変更します。 完了したら、ページはこの例のようになります。

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

ページを実行します。 これで次の図のようになります。

![レイアウトを使用してレンダリングの映画の追加 ページ](layouts/_static/image7.png)

サイト内のページに同様に変更する — *EditMovie.cshtml*と*DeleteMovie.cshtml*します。 ただし、実行する前に、なりますが、もう少し柔軟なレイアウトに別の変更を加えることができます。

## <a name="passing-title-information-to-the-layout-page"></a>レイアウト ページにタイトル情報を渡す

*\_Layout.cshtml*作成したページには、 `<title>` "My ムービー Site"に設定されている要素。 ほとんどのブラウザー タブのテキストとしてこの要素の内容を表示します。

![ページの&lt;タイトル&gt;ブラウザー タブで表示される要素](layouts/_static/image8.png)

このタイトル情報は、ジェネリックです。 現在のページに詳細を指定するタイトル テキストをたいとします。 (タイトルのテキストはでも使用検索エンジンによって、ページの項目を判断します。)ようなコンテンツ ページから情報を渡すことができます*Movies.cshtml*または*AddMovie.cshtml*レイアウト ページで、し、レイアウト ページをカスタマイズするには、その情報を表示します。

開く、 *Movies.cshtml*ページをもう一度です。 上部にあるコードでは、次の行を追加します。

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page`オブジェクトがすべてで使用可能な *.cshtml*ページ、つまり、この目的は、ページとレイアウト情報を共有します。

開く、<em>\_Layout.cshtml</em>ページ。 変更、`<title>`要素のため、it がこのマークアップのようになります。

[!code-html[Main](layouts/samples/sample9.html)]

このコードの表示内容に関係なく、`Page.Title`プロパティ ページで、その場所で右。

実行、 *Movies.cshtml*ページ。 今度の値として渡されると、ブラウザー タブが表示されます`Page.Title`:

![ブラウザーのタブが動的に作成されるタイトルを表示](layouts/_static/image9.png)

する場合は、ブラウザーでページ ソースを表示します。 確認、`<title>`要素としてレンダリング`<title>List Movies</title>`します。

> [!TIP] 
> 
> **Page オブジェクト**
> 
> 便利な機能の`Page`は動的オブジェクトである —、`Title`プロパティは、固定または予約名ではありません。 使用することができます*任意*の値の名前、`Page`オブジェクト。 たとえば、でしたように簡単に渡したことタイトルという名前のプロパティを使用して`Page.CurrentName`または`Page.MyPage`します。 唯一の制限は、名前が、どのようなプロパティの名前を指定できますの通常の規則に従います。 (たとえば、名前できませんにスペースを含める。)
> 
> 使用して任意の数の値を渡すことができます、`Page`オブジェクト。 レイアウト ページにムービー情報を渡す場合は、ようなものを使用して値を渡すこともできます`Page.MovieTitle`と`Page.Genre`と`Page.MovieYear`します。 (または、情報を格納するために作成する他の任意の名前。)唯一の要件: これは明らかな可能性があります-コンテンツ ページとレイアウト ページ内の同じ名前を使用する必要があります。
> 
> 使用して渡す情報、`Page`オブジェクトは、レイアウト ページに表示するテキストだけに限定されません。 レイアウト ページに値を渡すことができ、レイアウト ページのコードが、ページのセクションを表示するかどうかを決定する値を使用できますし、どのような *.css*を使用するファイル。 値を渡す、`Page`オブジェクトが値をその他のようなコードで使用します。 値が [コンテンツ] ページで生成され、レイアウト ページに渡されるだけです。


開く、 *AddMovie.cshtml*ページし、のタイトルを提供するコードの先頭に行を追加、 *AddMovie.cshtml*ページ。

[!code-csharp[Main](layouts/samples/sample10.cs)]

実行、 *AddMovie.cshtml*ページ。 新しいタイトルが表示されます。

![動的に作成された '追加映画' タイトルを表示するブラウザー タブ](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>レイアウトを使用して、残りのページを更新しています

今すぐ新しいレイアウトを使用するよう、サイトの残りのページを完了できます。 開いている*EditMovie.cshtml*と*DeleteMovie.cshtml*で有効にして、それぞれに同じ変更を行います。

レイアウト ページにリンクされているコード行を追加します。

[!code-csharp[Main](layouts/samples/sample11.cs)]

ページのタイトルを設定する行を追加します。

[!code-csharp[Main](layouts/samples/sample12.cs)]

または

[!code-csharp[Main](layouts/samples/sample13.cs)]

すべての余分な HTML マークアップを削除する、基本的には、内部にあるビットのみのままに、`<body>`要素 (および、上部にあるコード ブロック)。

変更、`<h1>`要素に、`<h2>`要素。

これらの変更を行ったときに、各テストし、正しく表示されないこと、およびタイトルが正しいかどうかを確認します。

## <a name="parting-thoughts-about-layout-pages"></a>レイアウト ページの分離アイデア

このチュートリアルで作成した、  *\_Layout.cshtml*ページし、使用、`RenderBody`メソッドを別のページからコンテンツをマージします。 Web ページのレイアウトを使用するための基本的なパターンです。

レイアウト ページには、ここで説明していない追加の機能があります。 たとえば、レイアウト ページを入れ子にすることができます: 1 つのレイアウト ページがさらに参照別です。 入れ子になったレイアウトは、さまざまなレイアウトを必要とするサイトのサブセクションを使用している場合に役立ちます。 追加のメソッドを使用することもできます (たとえば、 `RenderSection`) という名前のセクションでは、レイアウト ページを設定します。

レイアウト ページの組み合わせと *.css*ファイルは強力です。 次のチュートリアル シリーズで後ほど、WebMatrix でサイトを作成できます、に基づいて、*テンプレート*を持つサイトを提供する事前構築済みの機能です。 テンプレートは、レイアウト ページと CSS を美しく、メニューなどの機能がある、サイトを作成する適切な使い方を使用します。 レイアウト ページと CSS を使用する機能を示す、テンプレートに基づくサイトのホーム ページのスクリーン ショットを次に示します。

![ヘッダー、ナビゲーション領域、コンテンツ領域、省略可能なセクション、およびログインのリンクを示す WebMatrix サイト テンプレートによって作成されたレイアウト](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>ムービーのページ (レイアウト ページを使用して更新) の完全な一覧

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>[完了] ページ (レイアウトの更新) ムービー ページを追加します。

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>(レイアウトの更新) 削除ムービー ページの完了 ページの一覧

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>ムービー ページ (レイアウトの更新) の完了 ページの一覧

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>次回について

次のチュートリアルでは、すべてのユーザーが参照できるように、サイトをインターネットに公開する方法を学習します。

## <a name="additional-resources"></a>その他のリソース

- [一貫性のある検索を作成する](https://go.microsoft.com/fwlink/?LinkID=202891)-レイアウトの操作をさらに詳細を提供する記事です。 表示とコンテンツの一部を非表示のレイアウト ページに値を渡す方法も説明します。
- [入れ子になったと Razor レイアウト ページ](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor)— Mike Brind ブログ レイアウト ページを入れ子にする方法の例です。 (ページのダウンロードが含まれています)。

> [!div class="step-by-step"]
> [前へ](deleting-data.md)
> [次へ](publishing.md)
