---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: "マスター ページ (VB) 内の Url |Microsoft ドキュメント"
author: rick-anderson
description: "マスター ページ ファイルがコンテンツ ページとは異なる相対ディレクトリのため、マスター ページ内の Url が壊れる可能性がどのように対処します。 リベースを見ます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 8aa0ed2fbf385e4b8dbb7e7a3bdb152f1e016e67
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="urls-in-master-pages-vb"></a>マスター ページ (VB) 内の Url
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> マスター ページ ファイルがコンテンツ ページとは異なる相対ディレクトリのため、マスター ページ内の Url が壊れる可能性がどのように対処します。 使用して Url を再配置を見ます ~ 宣言の構文と ResolveUrl と ResolveClientUrl をプログラムで使用します。 (も見てください。


## <a name="introduction"></a>はじめに

すべての例で見たこれまでは、マスターおよびコンテンツのページは、同じフォルダー (web サイトのルート フォルダー) に配置されています。 しかし、理由、マスター ページとコンテンツ ページが、同じフォルダーにあります。 理由はありません。 サブフォルダーにコンテンツ ページを確実に作成できます。 同様に、作成する場合があります、`~/MasterPages/`サイトのマスター ページを配置するフォルダーです。

別のフォルダーのコンテンツとマスター ページを配置することで潜在的な問題 1 つにはには、切断された Url が含まれます。 マスター ページには、ハイパーリンク、画像、またはその他の要素の相対 Url が含まれています、リンクは、別のフォルダー内に存在するコンテンツ ページの有効なされません。 このチュートリアルでは、この問題と回避策のソースを確認します。

## <a name="the-problem-with-relative-urls"></a>相対 Url での問題

Web ページの URL があると言われます、*相対 URL*場合は、web サイトのフォルダー構造内の web ページの場所に対する相対パスを指しているリソースの場所です。 先頭のスラッシュで始まっていない任意の URL (`/`) またはプロトコル (など`http://`) では相対 URL を含む web ページの場所に基づき、ブラウザーでは解決します。

たとえば、当社の web サイトには、`~/Images/`単一のイメージ ファイルとフォルダー`PoweredByASPNET.gif`です。 マスター ページファイル`Site.master`が、`<img>`内の要素、`footerContent`次のマークアップを含む領域。


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

`src`属性の値、`<img>`要素は、相対 URL で始まらないため`/`または`http://`です。 つまり、`src`属性値を検索するようブラウザーに指示、`Images`という名前のファイルに対応するサブフォルダー`PoweredByASPNET.gif`です。

コンテンツ ページを訪問する際に上記のマークアップは、ブラウザーに直接送信されます。 すぐにアクセスを`About.aspx`し、ブラウザーに送信される HTML ソースを表示します。 マスター ページ内の正確な同じマークアップをブラウザーに送信されたことがわかります。


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

場合は、ルート フォルダーにコンテンツ ページ (は`About.aspx`) すべてが正しく動作があるので、`Images`ルート フォルダーの相対パスのサブフォルダーです。 ただし、処理は、コンテンツ ページがマスター ページとは異なるフォルダーにある場合に分解します。 これを示すためには、という名前のサブフォルダーを作成する`Admin`です。 という名前のコンテンツ ページを次に、追加`Default.aspx`を`Admin`フォルダーで、新しいページにバインドすることを確認、`Site.master`マスター ページ。

> [!NOTE]
> [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)という名前のカスタム ベース ページ クラスを作成したチュートリアル`BasePage`コンテンツ ページのタイトルを自動的に設定する (場合、明示的に割り当てられていません)。 忘れずにから派生して、新しく作成されたページの分離コード クラスを持つ`BasePage`この機能を利用できるようにします。


このコンテンツ ページを作成した後、ソリューション エクスプ ローラーは図 1 のようになります。


![新しいフォルダーと ASP.NET ページがプロジェクトに追加されました](urls-in-master-pages-vb/_static/image1.png)

**図 01**: 新しいフォルダーと ASP.NET ページがプロジェクトに追加されました


次に、更新、`Web.sitemap`ファイルに含め、新しい`<siteMapNode>`このレッスンのエントリ。 次の XML は、完全な`Web.sitemap`マークアップで、3 つ目の追加が含まれています`<siteMapNode>`要素。


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

新しく作成された`Default.aspx` ページで次の 4 つ contentplaceholders に対応する 4 つのコンテンツ コントロールを持つ必要があります`Site.master`です。 参照するコンテンツ コントロールにテキストを追加、 `MainContent` ContentPlaceHolder し、ブラウザーでページにアクセスします。 図 2 では、ブラウザーが見つかりません、`PoweredByASPNET.gif`イメージ ファイル。 ここで何が起こっているんですか。

`~/Admin/Default.aspx`同じ HTML コンテンツ ページを送信、`footerContent`が、領域、`About.aspx`ページ。


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

`<img>`要素の`src`属性は相対 URL をブラウザーが検索しようとした場合、 `Images` web ページのフォルダーの場所に対する相対フォルダーです。 つまり、イメージ ファイルには、ブラウザーが検索`Admin/Images/PoweredByASPNET.gif`です。


[![PoweredByASPNET.gif イメージ ファイルが見つかりません](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**図 02**:`PoweredByASPNET.gif`イメージ ファイルが見つかりません ([フルサイズのイメージを表示するをクリックして](urls-in-master-pages-vb/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>相対 Url を絶対 Url に置き換えます

相対 URL の逆に、*絶対 URL*、スラッシュで始まる 1 つである (`/`) またはなどのプロトコル`http://`です。 絶対 URL が既知の固定小数点からリソースの場所を指定するため、同じ絶対 URL は、web サイトのフォルダー構造内の web ページの場所に関係なく、任意の web ページで有効です。

図 2 に示すように壊れた画像を解決する必要がありますを更新する、`<img>`要素の`src`属性ではなく、相対絶対 URL を使用するようにします。 正しい絶対 URL を確認するのには、いずれかの web サイトで web ページを参照してくださいをアドレス バーを確認します。 Web アプリケーションへの完全修飾パスは、図 2 に、アドレス バーに示す`http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`です。 更新すること、したがって、`<img>`要素の`src`属性を次の 2 つの絶対 Url のいずれか。

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

更新する、`<img>`要素の`src`属性上に示した形式のいずれかを使用して絶対 URL をし、アクセス、`~/Admin/Default.aspx`ブラウザーを使用してページ。 今度は、ブラウザーの検索し、表示が正しく、`PoweredByASPNET.gif`イメージ ファイル (図 3 を参照してください)。


[![PoweredByASPNET.gif イメージが表示されるようになりました](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**図 03**:`PoweredByASPNET.gif`イメージが表示されるようになりました ([フルサイズのイメージを表示するをクリックして](urls-in-master-pages-vb/_static/image7.png))


ハード コーディングの絶対 URL で動作しますが、密に結合、HTML web サイトのサーバーおよびフォルダーの場所は、変更することがあります。 フォームの絶対 URL を使用して`http://localhost:3908/...`は当てに localhost の前、ポート番号が選択されているために自動的に Visual Studio の組み込みの ASP.NET 開発 Web サーバーを起動するたびにします。 同様に、`http://localhost`部分は、ローカルでテストする場合にのみ有効です。 URL のベースと同様に変更は、別のものに、コードを実稼働サーバーに展開すると後、`http://www.yourserver.com`です。 フォームの絶対 URL`/ASPNET_MasterPages_Tutorial_04_VB/...`開発と実稼働サーバー間でこのアプリケーションのパスが異なることがよくあるために、同じの脆弱性からも低下します。

良いニュースは、ASP.NET が実行時に有効な相対 URL を生成するためのメソッドを提供します。

## <a name="usingandresolveclienturl"></a>使用して`~`と`ResolveClientUrl`

はなく絶対 URL をハード コーディングするよりも ASP.NET では、チルダを使用するページの開発者 (`~`) web アプリケーションのルートを表します。 たとえば、このチュートリアルで既に使用表記`~/Admin/Default.aspx`を指すテキストで、 `Default.aspx`  ページで、`Admin`フォルダーです。 `~`ことを示します、`Admin`フォルダーは、web アプリケーションのルートのサブフォルダーです。

`Control`クラスの[`ResolveClientUrl`メソッド](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx)URL は、コントロールが存在する web ページの適切な相対 URL を変更します。 たとえば、呼び出す`ResolveClientUrl("~/Images/PoweredByASPNET.gif")`から`About.aspx`返します`Images/PoweredByASPNET.gif`です。 呼び出す`~/Admin/Default.aspx`、ただしを返します`../Images/PoweredByASPNET.gif`です。

> [!NOTE]
> すべての ASP.NET サーバー コントロールがから派生するため、`Control`クラス、すべてのサーバー コントロールにアクセスする、`ResolveClientUrl`メソッドです。 でも、`Page`クラスから派生します`Control`クラス、ASP.NET ページの分離コード クラスから直接には、このメソッドを使用することを意味します。


### <a name="usingin-the-declarative-markup"></a>使用して`~`宣言型マークアップ

複数の ASP.NET Web コントロールには、URL に関連するプロパティが含まれます: ハイパーリンク コントロールは、`NavigateUrl`プロパティ以外のコントロールにイメージ、`ImageUrl`プロパティです。 というようにします。 これらのコントロールがそれらの URL に関連するプロパティ値を渡すレンダリングされると、`ResolveClientUrl`です。 したがって、これらのプロパティが含まれている場合、 `~` web アプリケーションのルートを示すときに、URL が有効な相対 URL に変更されます。

ASP.NET サーバー コントロールのみを変換することに留意してください、`~`でその URL に関連するプロパティです。 場合、`~`などの静的な HTML マークアップに表示されます`<img src="~/Images/PoweredByASPNET.gif" />`、ASP.NET エンジンに送信、 `~` HTML コンテンツの残りの部分と共にブラウザーにします。 ブラウザーが想定する、 `~` URL の一部です。 例では、ブラウザーが、マークアップを受け取った場合の`<img src="~/Images/PoweredByASPNET.gif" />`という名前のサブフォルダーがあると想定`~`がサブフォルダーに`Images`イメージ ファイルを格納している`PoweredByASPNET.gif`です。

内のイメージのマークアップを修正する`Site.master`、既存の置換`<img>`ASP.NET イメージ Web コントロールを持つ要素。 イメージの Web コントロールの設定`ID`に`PoweredByImage`、その`ImageUrl`プロパティを`~/Images/PoweredByASPNET.gif`、およびその`AlternateText`「によって強化された ASP.NET!」をプロパティ


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

マスター ページにこの変更を行った後、再アクセス、`~/Admin/Default.aspx`ページをもう一度です。 この時間、`PoweredByASPNET.gif`イメージ ファイル、ページに表示されます (図 3 を参照してください)。 表示する際に、イメージの Web コントロールが、使用して、`ResolveClientUrl`を解決する方法、`ImageUrl`プロパティの値。 `~/Admin/Default.aspx` 、 `ImageUrl` HTML ソースの表示の次のスニペットに、適切な相対 URL に変換されます。


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> URL ベースの Web コントロールのプロパティで使用されているだけでなく、`~`を呼び出すときにも使用できます、`Response.Redirect`と`Server.MapPath`の他のメソッドです。 また、`ResolveClientUrl`メソッドが呼び出された場合、ASP.NET またはマスター ページの宣言型のマークアップから直接必要な場合は、参照してください[Fritz タマネギ](https://www.pluralsight.com/blogs/fritz/)のブログ エントリ[Using`ResolveClientUrl`マークアップで](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)です。


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>マスター ページの相対 Url の残りの修正

加え、`<img>`内の要素、`footerContent`を修正しました、マスター ページに注目を必要とする 1 つのより相対 URL が含まれています。 `topContent`領域には、「マスター ページ チュートリアル、」をポイントするリンクが含まれています。`Default.aspx`です。


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

ユーザーには送信この URL は相対であるため、`Default.aspx`フォルダーへのアクセスは、コンテンツ ページのページです。 このリンクを常にポイントして`Default.aspx`に置き換える必要があります、ルート フォルダーで、`<a>`ハイパーリンク Web を持つ要素を制御できるように、`~`表記します。

削除、`<a>`要素マークアップし、代わりに、ハイパーリンク コントロールを追加します。 設定のハイパーリンクの`ID`に`lnkHome`、その`NavigateUrl`プロパティを`~/Default.aspx`、およびその`Text`「マスター ページ チュートリアル」プロパティ


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

これで完了です。 この時点で、マスター ページとコンテンツ ページにどのようなフォルダーに関係なく、コンテンツ ページによってレンダリングされるときに、マスター ページ内の Url が基づく正しくすべてにあります。

### <a name="automatic-url-resolution-in-theheadsection"></a>自動の URL の解決方法、`<head>`セクション

[ *、サイト全体のレイアウトを使用してマスター ページを作成する*](creating-a-site-wide-layout-using-master-pages-vb.md)チュートリアルが追加されました、`<link>`を`Styles.css`ファイルで、`<head>`領域。


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

中に、`<link>`要素の`href`属性は相対が実行時に適切なパスを自動的に変換します。 説明したよう、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)チュートリアルでは、`<head>`地域が実際には、サーバー側のコントロール、変更できるようにする、その内部のコントロールが表示される場合の内容。

これを確認する再アクセス、`~/Admin/Default.aspx`ページし、ブラウザーに送信される HTML ソースを表示します。 次のスニペットに示すように、`<link>`要素の`href`属性は、適切な相対 URL を自動的に変更された`../Styles.css`です。


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>まとめ

非常に多くの場合、マスター ページには、リンク、イメージ、およびその他の外部リソースの URL を使用して指定する必要がありますが含まれます。 同じフォルダーに、マスター ページとページのコンテンツが存在しない可能性がありますので、abstain から相対 Url を使用する必要があります。 ハード コーディングされた絶対 Url を使用することはできますは、web アプリケーションに絶対 URL を結合ため緊密を実行します。 ように移動するか、web アプリケーションを展開するときに多くの場合は、絶対 URL が変更された場合は、戻ってして絶対 Url を更新する必要があります。

チルダを使用する最適な方法 (`~`) アプリケーション ルートを表します。 URL に関連するプロパティを格納している ASP.NET Web コントロールのマップ、`~`実行時にアプリケーションのルートにします。 内部的には、Web コントロールを使用して、`Control`クラスの`ResolveClientUrl`有効な相対 URL を生成する方法です。 このメソッドはパブリックであり、すべてのサーバー コントロールから使用可能な (など、`Page`クラス) を使用できるようにプログラムによって、分離コード クラスから必要な場合は、します。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET マスター ページ](http://www.odetocode.com/Articles/419.aspx)
- [マスター ページの URL が再配置](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [使用して`ResolveClientUrl`マークアップ](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)です。

>[!div class="step-by-step"]
[前へ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
[次へ](control-id-naming-in-content-pages-vb.md)
