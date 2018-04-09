---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: ASP.NET Web Pages の概要 - 一貫したレイアウトを作成 |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルでは、レイアウトを使用して ASP.NET Web Pages を使用するサイトのページに対して一貫した外観を作成する方法を示します。 完了すると想定します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>ASP.NET Web Pages の一貫したレイアウトの作成の概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルを使用する方法を示します*レイアウト*を ASP.NET Web Pages を使用するサイト ページの一貫した外観を作成します。 を通じてシリーズを完了すると想定[データベースのデータを削除する ASP.NET Web Pages で](https://go.microsoft.com/fwlink/?LinkId=251584)です。
> 
> 学習する内容。
> 
> - どのようなレイアウト ページです。
> - 動的なコンテンツとレイアウト ページを結合する方法。
> - レイアウト ページに値を渡す方法。


## <a name="about-layouts"></a>レイアウトについて

これまでに作成したページがすべて完了、スタンドアロンのページです。 同じサイトに属しているが、共通の要素または標準の外観はありません。

ほとんどのサイトには、一貫した外観とレイアウトができます。 たとえばに移動する場合、 [Microsoft.com/web](https://www.microsoft.com/web/)サイトし、少し調べて、すべてのページが全体的なレイアウトをビジュアル テーマに従うことを参照してください。

![ヘッダーのナビゲーション領域、コンテンツ領域、およびページ フッターのレイアウトを示す Microsoft.com/web サイト ページ](layouts/_static/image1.png)

*非効率的な*このレイアウトを作成する方法はそれぞれ、ページのヘッダー、ナビゲーション バー、およびフッターを個別に定義することです。 複製するのではするマークアップを同じたびにします。 変更を加える必要がある場合 (たとえば、更新、フッター)、各ページを個別に変更する必要があります。

ような場合は*レイアウト ページ*取得します。 ASP.NET Web ページで、サイト上のページの全体的なコンテナーを提供するレイアウト ページを定義できます。 たとえば、[レイアウト] ページでは、ヘッダーのナビゲーション領域およびフッターを含めることができます。 [レイアウト] ページには、メイン コンテンツの行き先のプレース ホルダーが含まれています。

マークアップおよびコード ページのみを含む個々 のコンテンツ ページを定義することができます。 コンテンツ ページがなくても完全な HTML ページです。必要なしないであっても、`<body>`要素。 コンテンツを表示するレイアウト ページを ASP.NET に指示するコードの行もあります。 このリレーションシップのしくみをほぼ表示される画像を次に示します。

![2 つのコンテンツ ページと適合するレイアウト ページを表示する概念図](layouts/_static/image2.png)

この連携で理解しやすい操作で表示される場合。 このチュートリアルでは、レイアウトを使用する、映画ページを変更します。

## <a name="adding-a-layout-page"></a>レイアウト ページを追加します。

ヘッダー、フッター、およびメイン コンテンツ領域での通常のページ レイアウトを定義するレイアウト ページを作成して開始します。 WebPagesMovies サイト内には、名前付き CSHTML ページを追加します。  *\_Layout.cshtml*です。

先頭にアンダー スコア ( `_` ) 文字は重要です。 ページの名前にアンダー スコアである場合、ASP.NET がブラウザーにそのページを送信しません直接です。 この規則では、ユーザーは直接要求できることはできませんが、サイトに必要なページを定義できます。

ページのコンテンツを次のように置き換えます。

[!code-html[Main](layouts/samples/sample1.html)]

このマークアップを使用する HTML だけことがわかります`<div>`では、ページと 1 つ以上 3 つのセクションを定義する要素`<div>`3 つのセクションを保持する要素。 フッターには Razor コードのビットが含まれています:`@DateTime.Now.Year`ページ内の場所に現在の年を表示します。

名前付きスタイル シートへのリンクがあることに注意してください。 *Movies.css*です。 スタイル シートとは、要素の物理的なレイアウトの詳細を定義します。 すぐに作成します。

こののみ通常とは異なる機能 *\_Layout.cshtml*ページは、`@Render.Body()`行です。 このレイアウトが別のページとマージされるときに、コンテンツの行き先プレース ホルダーです。

## <a name="adding-a-css-file"></a>.Css ファイルを追加します。

カスケード スタイル シート (CSS) の規則を使用するは、ページ要素の実際の配置 (つまり、外観) を定義することをお勧めします。 作成するように、 *.css*を新しいレイアウトの規則を持つファイルです。

WebMatrix でサイトのルートを選択します。 次に、**ファイル** タブ、リボンの下の矢印をクリックして、**新規**ボタンをクリックし、をクリックして**新しいフォルダー**です。

![リボンの 新規 '新しいフォルダー' オプション。](layouts/_static/image3.png)

新しいフォルダーの名前を付けます*スタイル*です。

![新しいフォルダー 'スタイル' の名前付け](layouts/_static/image4.png)

新しい内部*スタイル*フォルダー、という名前のファイルを作成する*Movies.css*です。

![Movies.css ファイルの新規作成](layouts/_static/image5.png)

新しい内容を置き換える*.css*を次のファイル。

[!code-css[Main](layouts/samples/sample2.css)]

2 つの点に注意してくださいを除く、CSS 規則の大部分を言うされません。 1 つは、フォントとサイズを設定するだけでなく、使用しているルール絶対配置ヘッダー、フッター、およびメイン コンテンツ領域の場所を確立するためにです。 かどうかは、CSS で配置する新しいしたらを読み取ることができます、 [CSS 配置](http://www.w3schools.com/css/css_positioning.asp)W3Schools サイトのチュートリアルです。

もう 1 つは、下部、元々 のスタイル ルールがコピーされるしたおで定義されている個別に注意してください、 *Movies.cshtml*ファイル。 これらの規則が使用されていた、[を表示するデータを使用して ASP.NET Web Pages の概要](https://go.microsoft.com/fwlink/?LinkId=251580)するのには、チュートリアル、`WebGrid`ヘルパーがテーブルにストライプ化を追加するマークアップを表示します。 (使用する場合、 *.css*ファイル スタイル定義がサイト全体のスタイル ルールをも配置する場合があります)。

## <a name="updating-the-movies-file-to-use-the-layout"></a>レイアウトを使用するビデオ ファイルの更新

今すぐ新しいレイアウトを使用するようにサイト内の既存のファイルを更新することができます。 開く、 *Movies.cshtml*ファイル。 上部にある、コードの最初の行として以下を追加します。

[!code-csharp[Main](layouts/samples/sample3.cs)]

ページは、この方法を今すぐ開始します。

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

この 1 行のコードが ASP.NET に指示する場合、*映画*でマージするか、ページが実行される、  *\_Layout.cshtml*ファイル。

以降、 *Movies.cshtml*ファイルは今すぐレイアウト ページを使用して、マークアップからを削除することができます、 *Movies.cshtml*によって対処がページ、  *\_Layout.cshtml*ファイル。 取り出して、 `<!DOCTYPE>`、 `<html>`、および`<body>`タグと終了タグ。 全体を取り出して`<head>`要素とその内容それらのルールを取得した後に、グリッドのスタイル ルールを含んでいます、 *.css*ファイル。 中は、変更、既存`<h1>`要素を`<h2>`要素; が、`<h1>`レイアウト ページを既に内の要素。 変更、 `<h2>` 「リスト映画」するテキスト。

通常は、コンテンツ ページでこれらの種類の変更を有効にする必要はありません。 レイアウト ページで、サイトを起動すると、まず、これらすべての要素のコンテンツ ページを作成します。 ここでは、ただし、変換しているスタンドアロン ページ クリーンアップのビットは、レイアウトを使用している 1 つにします。

、が完了したら、 *Movies.cshtml*ページは、次のようになります。

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>レイアウトのテスト

レイアウトの外観を確認できます。 WebMatrix でを右クリックし、 *Movies.cshtml*ページし、選択**ブラウザーで起動**です。 ブラウザーでは、ページを表示するときは、このページのようになります。

![レイアウトを使用して表示されるビデオ ページ](layouts/_static/image6.png)

ASP.NET に Movies.cshtml ページの内容とマージ、  *\_Layout.cshtml*右のページ、`RenderBody`メソッドはします。 そしてもちろん、  *\_Layout.cshtml*ページを参照する、 *.css*ページの外観を定義するファイル。

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>レイアウトを使用する AddMovie ページの更新

レイアウトの実際の利点は、ことできますで使用するすべてのページで、サイトです。 開く、 *AddMovie.cshtml*ページ。

留意することがあります、 *AddMovie.cshtml*  ページでは、検証エラー メッセージの外観を定義することで、CSS ルールがいくつかを最初からです。 あるため、 *.css*ファイルを今すぐサイトをそれらの規則を移動することができます、 *.css*ファイル。 削除してから、 *AddMovie.cshtml*ファイルの下部に追加して、 *Movies.css*ファイル。 次の規則を移動します。

[!code-css[Main](layouts/samples/sample6.css)]

これで、同じ種類の変更の*AddMovie.cshtml*の*Movies.cshtml* -追加`Layout="~/_Layout.cshtml;`とは無関係な HTML マークアップを削除します。 `<h1>` 要素を `<h2>` に変更します。 完了したら、ページはこの例のようになります。

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

ページを実行します。 これで次の図のようになります。

!['映画' ページの追加、レイアウトを使用してレンダリング](layouts/_static/image7.png)

サイト内のページに同様に変更する — *EditMovie.cshtml*と*DeleteMovie.cshtml*です。 ただし、実行する前にことも、もう少し柔軟なレイアウトに別の変更を行います。

## <a name="passing-title-information-to-the-layout-page"></a>[レイアウト] ページにタイトル情報を渡す.

 *\_Layout.cshtml*作成したページには、 `<title>` "マイ ムービー Site"に設定されている要素です。 ほとんどのブラウザーでは、タブのテキストとしてこの要素の内容を表示します。

![ページの&lt;タイトル&gt;ブラウザー タブで表示される要素](layouts/_static/image8.png)

このタイトル情報はジェネリックです。 現在のページに詳細を指定するタイトルのテキストを表示するとします。 (タイトルのテキストにも使用検索エンジンによって、ページの内容を判断します。)同様に、コンテンツ ページから情報を渡すことができます*Movies.cshtml*または*AddMovie.cshtml*へしを使用して、レイアウト ページ、レイアウト ページをカスタマイズするには、その情報を表示します。

開く、 *Movies.cshtml*ページをもう一度です。 上部のコードでは、次の行を追加します。

[!code-csharp[Main](layouts/samples/sample8.cs)]

`Page`オブジェクトは、すべての利用*.cshtml*ページ、つまり、この目的では、ページとレイアウトの間で情報を共有します。

開く、<em>\_Layout.cshtml</em>ページ。 変更、`<title>`要素のため、このマークアップのようなことを検索します。

[!code-html[Main](layouts/samples/sample9.html)]

処理します。 このコードの表示、`Page.Title`プロパティ ページで、その場所にすぐにします。

実行、 *Movies.cshtml*ページ。 この時点のブラウザー タブが表示の値として渡される内容`Page.Title`:

![動的に作成されるタイトルを表示、ブラウザーのタブ](layouts/_static/image9.png)

場合は、ブラウザーでページ ソースを表示します。 `<title>`要素としてレンダリング`<title>List Movies</title>`です。

> [!TIP] 
> 
> **Page オブジェクト**
> 
> 機能は便利です`Page`は動的オブジェクトには、-、`Title`プロパティは、固定または予約名ではありません。 使用することができます*任意*の値の名前、`Page`オブジェクト。 たとえば、でしたように簡単に渡されたタイトルという名前のプロパティを使用して、`Page.CurrentName`または`Page.MyPage`です。 唯一の制限は、名前がどのようなプロパティの名前を付けるの通常の規則に従うことです。 (たとえば、名前に使用できませんスペースです。)
> 
> 使用して任意の数の値を渡すことができます、`Page`オブジェクト。 ようなものを使用して値を渡すことが、レイアウト ページにムービー情報を渡す場合は、`Page.MovieTitle`と`Page.Genre`と`Page.MovieYear`です。 (または情報を格納するために発明する他の任意の名前)。唯一の要件: これはおそらく明白な — コンテンツ ページとページ レイアウト ページ内の同じ名前を使用する必要があります。
> 
> 使用して渡す情報、`Page`オブジェクトは、レイアウト ページに表示するテキストだけに制限はありません。 レイアウト ページに値を渡すことができ、レイアウト ページ内のコードが、ページのセクションを表示するかどうかを決定する値を使用して、どのような*.css*を使用するファイル。 値を渡す、`Page`オブジェクトは、他の値と同じようにコードで使用します。 値がコンテンツ ページで発生し、レイアウト ページに渡されるのみです。


開く、 *AddMovie.cshtml*ページし、のタイトルを提供するコードの先頭に行を追加、 *AddMovie.cshtml*ページ。

[!code-csharp[Main](layouts/samples/sample10.cs)]

実行、 *AddMovie.cshtml*ページ。 新しいタイトルが表示されます。

![動的に作成 '追加映画' タイトルを表示、ブラウザーのタブ](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>レイアウトを使用して残りのページを更新

今すぐ新しいレイアウトを使用するよう、サイト内の残りのページを完了できます。 開いている*EditMovie.cshtml*と*DeleteMovie.cshtml*で有効にして、それぞれに同じ変更を行います。

[レイアウト] ページにリンクされているコードの行を追加します。

[!code-csharp[Main](layouts/samples/sample11.cs)]

ページのタイトルを設定する行を追加します。

[!code-csharp[Main](layouts/samples/sample12.cs)]

または

[!code-csharp[Main](layouts/samples/sample13.cs)]

余分なすべての HTML マークアップを削除する — 基本的には、内にあるビットのみのままにして、`<body>`要素 (および上部にあるコード ブロック) します。

変更、`<h1>`要素に、`<h2>`要素。

これらの変更を加えたときに、それぞれをテストし、正しく表示されないことと、タイトルが正しいことを確認してください。

## <a name="parting-thoughts-about-layout-pages"></a>分離考えレイアウト ページの概要

このチュートリアルで作成した、  *\_Layout.cshtml*ページ、使用、`RenderBody`別のページからコンテンツをマージするメソッド。 Web ページのレイアウトを使用するための基本的なパターンです。

レイアウト ページでは、ここに対応していないする機能があります。 たとえば、レイアウト ページを入れ子にすることができます — 1 つのレイアウト ページをさらに参照別できます。 入れ子になったレイアウトをさまざまなレイアウトを必要とするサイトのサブセクションで作業する場合に役立ちます。 追加のメソッドを使用することもできます (たとえば、 `RenderSection`) を設定するレイアウト ページのセクションをという名前です。

レイアウト ページの組み合わせと*.css*ファイルは強力です。 次のチュートリアルの系列が表示されます、WebMatrix で作成してに基づいてサイト、*テンプレート*、されているサイトを提供することで構築済みの機能です。 テンプレートを使用するレイアウト ページ、見栄えが良くして、メニューなどの機能があるサイトを作成する CSS の適切な使い方です。 ページのレイアウトと CSS を使用する機能を示す、テンプレートに基づくサイトのホーム ページのスクリーン ショットを次に示します。

![ヘッダーのナビゲーション領域、コンテンツ領域、省略可能なセクション、およびログインのリンクを示す WebMatrix サイト テンプレートによって作成されたレイアウト](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>ムービーのページ (レイアウト ページを使用して更新) の完全な一覧

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>[完了] ページ (レイアウトの更新) ビデオ ページを追加します。

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Delete ムービーのページ (レイアウトの更新) の完了 ページの一覧

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>(レイアウトの更新) の編集ビデオ ページの 完了 ページの一覧

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>次へ直近の見通し

次のチュートリアルでは、すべてのユーザーが表示できるように、サイトをインターネットに公開する方法を学習します。

## <a name="additional-resources"></a>その他のリソース

- [一貫性のある参照を作成する](https://go.microsoft.com/fwlink/?LinkID=202891)-レイアウトの操作をいくつかの詳細を提供するアーティクルです。 また、レイアウト ページを表示または一部のコンテンツを非表示に値を渡す方法も説明します。
- [入れ子になったと Razor レイアウト ページ](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor)— Mike Brind ブログをレイアウト ページを入れ子にする方法の例です。 (ページのダウンロードが含まれています)。

> [!div class="step-by-step"]
> [前へ](deleting-data.md)
> [次へ](publishing.md)
