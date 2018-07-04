---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: マスター ページ (VB) の Url |Microsoft Docs
author: rick-anderson
description: マスター ページの Url が相対ディレクトリ コンテンツのページとは別のマスター ページファイルにより中断できる方法について説明します。 リベースをチェックしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9094c6c2b1700f22fe29d8b341444e1178c9015f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395131"
---
<a name="urls-in-master-pages-vb"></a>マスター ページ (VB) の Url
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> マスター ページの Url が相対ディレクトリ コンテンツのページとは別のマスター ページファイルにより中断できる方法について説明します。 使用して Url をリベースを見て ~ 宣言の構文と ResolveUrl と ResolveClientUrl をプログラムでを使用します。 (も見てください。


## <a name="introduction"></a>はじめに

すべての例で見たまでは、マスターおよびコンテンツのページを同じフォルダー (web サイトのルート フォルダー) に配置されています。 理由、マスター ページやコンテンツ ページが、同じフォルダーにあります。 理由はありません。 確かに、サブフォルダー内のコンテンツ ページを作成できます。 同様に、作成する場合があります、`~/MasterPages/`サイトのマスター ページを配置するフォルダー。

マスター ページやコンテンツ ページの異なるフォルダーに配置が 1 つの潜在的な問題には、切断された Url が含まれます。 マスター ページにハイパーリンク、画像、またはその他の要素の相対 Url が含まれている場合、リンクが、別のフォルダー内に存在するコンテンツ ページの無効になります。 このチュートリアルでは、この問題および回避策のソースを確認します。

## <a name="the-problem-with-relative-urls"></a>相対 Url の問題

Web ページ上の URL はモード、*相対 URL*場合は、web サイトのフォルダー構造内の web ページの場所に対する相対パスを指すリソースの場所です。 先頭のスラッシュで始まっていない任意の URL (`/`) またはプロトコル (など`http://`) URL を含む web ページの場所に基づき、ブラウザーによってそれを解決するためには、相対値です。

たとえば、web サイトには、 `~/Images/` 、1 つのイメージ ファイルとフォルダー`PoweredByASPNET.gif`します。 マスター ページファイル`Site.master`が、`<img>`内の要素、`footerContent`次のマークアップでリージョン。


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

`src`属性の値、`<img>`値で始まらないために、要素が相対 URL`/`または`http://`します。 つまり、`src`属性の値を検索するようブラウザーに指示、`Images`という名前のファイル用のサブフォルダー`PoweredByASPNET.gif`します。

コンテンツ ページを訪問する際に上記のマークアップは、ブラウザーに直接送信されます。 アクセスする少し`About.aspx`し、ブラウザーに送信される HTML ソースを表示します。 マスター ページで、正確な同じマークアップをブラウザーに送信されたことがあります。


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

コンテンツ ページがルート フォルダーにある場合 (現状`About.aspx`) すべてがあるため、想定どおりに動作、`Images`ルート フォルダーのサブフォルダーです。 ただし、何かが [コンテンツ] ページがマスター ページは異なるフォルダーにある場合中断します。 これを示すためには、という名前のサブフォルダーを作成`Admin`です。 という名前のコンテンツ ページを次に、追加`Default.aspx`を`Admin`フォルダーに新しいページにバインドすることを確認、`Site.master`マスター ページ。

> [!NOTE]
> [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)という名前のカスタム ベース ページ クラスを作成したチュートリアル`BasePage`コンテンツ ページのタイトルを自動的に設定されます (場合、明示的に割り当てられていません)。 派生して、新しく作成されたページの分離コード クラスを用意することを忘れないでください`BasePage`この機能を利用できるようにします。


このコンテンツ ページを作成した後、ソリューション エクスプ ローラーは図 1 のようになります。


![新しいフォルダーと ASP.NET ページ、プロジェクトに追加されています](urls-in-master-pages-vb/_static/image1.png)

**図 01**: 新しいフォルダーと ASP.NET ページ、プロジェクトに追加されています


次に、更新、`Web.sitemap`ファイルに含める新しい`<siteMapNode>`このレッスンのエントリ。 次の XML は、完全な`Web.sitemap`マークアップで、3 つ目の追加が含まれています`<siteMapNode>`要素。


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

新しく作成された`Default.aspx`ページでの 4 つの ContentPlaceHolders に対応する 4 つのコンテンツ コントロールがある`Site.master`します。 コンテンツ コントロールを参照することをいくつかのテキストを追加、 `MainContent` ContentPlaceHolder し、ブラウザーを使用してページにアクセスします。 図 2 に示すよう、ブラウザーを見つけることができません、`PoweredByASPNET.gif`イメージ ファイル。 ここで何が起こっているんですか。

`~/Admin/Default.aspx`同じ HTML コンテンツ ページを送信、`footerContent`されているが、リージョン、`About.aspx`ページ。


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

`<img>`要素の`src`属性は、相対 URL をブラウザーが検索しようとした場合、 `Images` web ページのフォルダーの場所を基準としたフォルダーです。 つまり、イメージ ファイルには、ブラウザーが検索`Admin/Images/PoweredByASPNET.gif`します。


[![PoweredByASPNET.gif イメージ ファイルが見つかりません](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**図 02**:`PoweredByASPNET.gif`イメージ ファイルが見つかりません ([フルサイズの画像を表示する をクリックします](urls-in-master-pages-vb/_static/image4.png))。


### <a name="replacing-relative-urls-with-absolute-urls"></a>相対 Url を絶対 Url を置き換える

相対 URL の逆の場合、*絶対 URL*、スラッシュで始まる 1 つである (`/`) などのプロトコルまたは`http://`します。 絶対 URL では、既知の固定ポイントからのリソースの場所を指定します、ため、同じ絶対 URL では、web サイトのフォルダー構造での web ページの場所に関係なく、任意の web ページでは無効です。

図 2 に示すように、壊れた画像を解決する必要がありますを更新する、`<img>`要素の`src`相対ではなく絶対 URL を使用するように属性します。 適切な絶対 URL を確認するのには、いずれかの web サイトで web ページを参照してくださいし、アドレス バーを調べます。 Web アプリケーションへの完全修飾パスは、図 2 のアドレス バーに示すよう`http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`します。 そのため、更新、`<img>`要素の`src`属性を次の 2 つの絶対 Url のいずれか。

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

更新する少し、`<img>`要素の`src`属性上に示した形式のいずれかを使用して絶対 URL をし、アクセス、`~/Admin/Default.aspx`ブラウザーを使用してページ。 この時間、ブラウザーの検索し、表示が正しく、`PoweredByASPNET.gif`イメージ ファイル (図 3 を参照してください)。


[![PoweredByASPNET.gif イメージが表示されるようになりました](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**図 03**:`PoweredByASPNET.gif`イメージが表示されるようになりました ([フルサイズの画像を表示する をクリックします](urls-in-master-pages-vb/_static/image7.png))。


動作の絶対 URL をハードコーディングしますが、密に、web サイトのサーバーとフォルダーの場所は、変更を HTML を結合します。 形式の絶対 URL を使用して`http://localhost:3908/...`Visual Studio の組み込みの ASP.NET 開発 Web サーバーを起動するたびに自動的に選択前に localhost のポート番号は脆弱です。 同様に、`http://localhost`部分は、ローカルでテストする場合にのみ有効です。 URL ベースをような変更は、別のものに、コードを実稼働サーバーにデプロイすると`http://www.yourserver.com`します。 フォームの絶対 URL`/ASPNET_MasterPages_Tutorial_04_VB/...`多くの場合、このアプリケーションのパスが開発と運用サーバーの間とは異なるために、同じの脆弱性からも低下します。

良い知らせは、ASP.NET が実行時に有効な相対 URL を生成するためのメソッドを提供します。

## <a name="usingandresolveclienturl"></a>使用して`~`と`ResolveClientUrl`

はなく絶対 URL をハード コーディングするよりも、ASP.NET は、ページ開発者は、チルダを使用して (`~`) web アプリケーションのルートを表します。 表記法を使用するこのチュートリアルで前になど`~/Admin/Default.aspx`を参照するテキストで、`Default.aspx`ページで、`Admin`フォルダー。 `~`ことを示します、`Admin`フォルダーは、web アプリケーションのルートのサブフォルダーです。

`Control`クラスの[`ResolveClientUrl`メソッド](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx)URL を受け取り、それをコントロールが存在する web ページの適切な相対 URL に変更します。 たとえば、呼び出し`ResolveClientUrl("~/Images/PoweredByASPNET.gif")`から`About.aspx`返します`Images/PoweredByASPNET.gif`します。 呼び出す`~/Admin/Default.aspx`、ただし、返します`../Images/PoweredByASPNET.gif`します。

> [!NOTE]
> すべての ASP.NET サーバー コントロールがから派生するため、`Control`クラス、すべてのサーバー コントロールのあるへのアクセス、`ResolveClientUrl`メソッド。 でも、`Page`クラスから派生、`Control`クラス、つまり、ASP.NET ページの分離コード クラスから直接このメソッドを使用することができます。


### <a name="usingin-the-declarative-markup"></a>使用して`~`宣言型マークアップ

いくつかの ASP.NET Web コントロールには、URL に関連するプロパティが含まれます: ハイパーリンク コントロールは、`NavigateUrl`プロパティです。 コントロールにイメージを`ImageUrl`; のプロパティ。 これらのコントロールにその URL に関連するプロパティの値を渡す、レンダリングされるときに`ResolveClientUrl`します。 その結果、これらのプロパティが含まれている場合、`~`有効な相対 url を web アプリケーションのルートを示すために、URL が変更されます。

ASP.NET サーバー コントロールのみを変換することに留意してください、`~`でその URL に関連するプロパティ。 場合、`~`などの静的な HTML マークアップに表示されます`<img src="~/Images/PoweredByASPNET.gif" />`、ASP.NET エンジンに送信、 `~` HTML コンテンツの残りの部分と共にブラウザーにします。 ブラウザーを前提としている`~`は URL の一部です。 例では、ブラウザーは、マークアップを受信した場合の`<img src="~/Images/PoweredByASPNET.gif" />`という名前のサブフォルダーがあると想定して`~`がサブフォルダーに`Images`イメージ ファイルを格納している`PoweredByASPNET.gif`します。

内のイメージのマークアップを修正する`Site.master`、既存`<img>`ASP.NET イメージ Web コントロールを持つ要素。 イメージの Web コントロールの設定`ID`に`PoweredByImage`その`ImageUrl`プロパティを`~/Images/PoweredByASPNET.gif`、およびその`AlternateText`プロパティ「ASP.NET によって強化された!」を


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

マスター ページにこの変更を行った後に再アクセス、`~/Admin/Default.aspx`ページをもう一度です。 この時間、`PoweredByASPNET.gif`イメージ ファイル、ページに表示されます (図 3 を参照してください)。 イメージ Web コントロールを描画するタイミングには、`ResolveClientUrl`を解決する方法、`ImageUrl`プロパティの値。 `~/Admin/Default.aspx` 、 `ImageUrl` HTML ソースの表示の例を次に、適切な相対 URL に変換されます。


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> URL ベースの Web コントロールのプロパティで使用されているだけでなく、`~`を呼び出す場合にも使用できます、`Response.Redirect`と`Server.MapPath`他のユーザーの間でのメソッド。 また、`ResolveClientUrl`メソッドは、必要な場合は、ASP.NET またはマスター ページの宣言型のマークアップから直接呼び出すことができます。 参照してください[Fritz Onion](https://www.pluralsight.com/blogs/fritz/)のブログ エントリ[Using`ResolveClientUrl`マークアップで](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)します。


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>マスター ページの相対 Url の残りの修正

加え、`<img>`内の要素、`footerContent`マスター ページには、注意が必要な 1 つの複数の相対 URL を修正しました。 `topContent`領域には、「マスター ページ チュートリアルでは、」を指すリンクが含まれています。`Default.aspx`します。


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

この URL は相対であるため、ユーザーに送信されます、`Default.aspx`がアクセスして、[コンテンツ] ページのフォルダー内のページ。 このリンクを常にポイントして`Default.aspx`に置き換える必要があります。 ルート フォルダーに、`<a>`ハイパーリンク Web を持つ要素を制御、使用できるように、`~`表記します。

削除、`<a>`要素マークアップと、その場所にハイパーリンク コントロールを追加します。 ハイパーリンクの設定`ID`に`lnkHome`その`NavigateUrl`プロパティを`~/Default.aspx`、およびその`Text`プロパティを「マスター ページのチュートリアル」


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

これで完了です。 この時点でどのようなフォルダーに関係なく、[コンテンツ] ページで、マスター ページと [コンテンツ] ページに表示されるときに、マスター ページの Url が基づく正しくすべてにあります。

### <a name="automatic-url-resolution-in-theheadsection"></a>自動の URL の解像度、`<head>`セクション

[ *、サイト全体レイアウトを使用してマスター ページを作成する*](creating-a-site-wide-layout-using-master-pages-vb.md)チュートリアルが追加されました、`<link>`を`Styles.css`ファイル、`<head>`リージョン。


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

中に、`<link>`要素の`href`属性は、相対的な実行時に適切なパスに自動的に変換されます。 説明したように、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)チュートリアルでは、`<head>`リージョンが実際には、サーバー側コントロールを可能にすると、変更、表示されるようにその内部のコントロールの内容。

これを確認するには、見直し、`~/Admin/Default.aspx`ページし、ブラウザーに送信される HTML ソースを表示します。 次のスニペットに示すように、`<link>`要素の`href`属性は、適切な相対 URL を自動的に変更されている`../Styles.css`します。


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>まとめ

マスター ページには、非常に多くの場合、リンク、画像、およびその他の外部のリソース URL 経由で指定する必要がありますが含まれます。 マスター ページとコンテンツのページが同じフォルダーに存在しないためには、相対 Url を使用してから棄権票に重要です。 Web アプリケーションには、ハードコード絶対 Url を使用することはできますは、絶対 URL は結合非常に密接を実行します。 移動または web アプリケーションのデプロイ時に多くの場合は、絶対 URL が変更された場合は、忘れずに戻るし、絶対 Url を更新する必要があります。

理想的な方法は、チルダを使用する (`~`) アプリケーションのルートを表します。 URL に関連するプロパティを格納している ASP.NET Web コントロールのマップ、`~`実行時にアプリケーションのルートにします。 内部的には、Web コントロールを使用して、`Control`クラスの`ResolveClientUrl`有効な相対 URL を生成します。 このメソッドがパブリックであり、すべてのサーバー コントロールから利用可能な (など、`Page`クラス) を使用できるようにプログラムで、分離コード クラスから必要な場合、します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET のマスター ページ](http://www.odetocode.com/Articles/419.aspx)
- [マスター ページの URL を再配置](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [使用して`ResolveClientUrl`マークアップ](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)します。

> [!div class="step-by-step"]
> [前へ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
> [次へ](control-id-naming-in-content-pages-vb.md)
