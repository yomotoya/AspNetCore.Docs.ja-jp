---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: ビュー マスター ページ (VB) でページ レイアウトを作成 |Microsoft Docs
author: microsoft
description: このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページの一般的なページ レイアウトを作成する方法について説明します。 使用することができますをしています.
ms.author: riande
ms.date: 10/16/2008
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ff74b1dc1d83b7ec1c8ecf6eca341a5cd14403f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825910"
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a>ビュー マスター ページ (VB) でページ レイアウトを作成
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページの一般的なページ レイアウトを作成する方法について説明します。 たとえば、ビュー マスター ページを使用する 2 つの列 ページのレイアウトを定義して、web アプリケーション内のページのすべての 2 つの列のレイアウトを使用します。


## <a name="creating-page-layouts-with-view-master-pages"></a>ビュー マスター ページとページ レイアウトを作成します。

このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページの一般的なページ レイアウトを作成する方法について説明します。 たとえば、ビュー マスター ページを使用する 2 つの列 ページのレイアウトを定義して、web アプリケーション内のページのすべての 2 つの列のレイアウトを使用します。

利用したもできるビューのマスター ページ、アプリケーションで複数のページ間で共通のコンテンツを共有します。 たとえば、ビュー マスター ページで、web サイトのロゴ、ナビゲーション リンク、およびバナー広告を配置できます。 これにより、アプリケーション内の各ページはこのコンテンツを自動的に表示します。

このチュートリアルでは、新しいビュー マスター ページを作成し、マスター ページに基づいて、新しいビュー コンテンツ ページを作成する方法について説明します。

### <a name="creating-a-view-master-page"></a>ビュー マスター ページを作成します。

2 つの列のレイアウトを定義するビュー マスター ページを作成してみましょう。 追加する新しいビュー マスター ページ、MVC プロジェクトを views \shared フォルダーを右クリックしてメニュー オプションを選択する**追加]、[新しい項目の**、MVC ビュー マスター ページ テンプレートを選択して (図 1 参照)。


[![ビュー マスター ページを追加します。](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**図 01**: ビュー マスター ページを追加する ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))。


アプリケーションでは、1 つ以上のビュー マスター ページを作成できます。 各ビュー マスター ページには、別のページ レイアウトを定義できます。 たとえば、特定のページが 2 つの列のレイアウトおよびその他のページ 3 列のレイアウトをたい場合があります。

ビュー マスター ページの外観を標準の ASP.NET MVC ビューとよく似ています。 ただし、通常のビューとは異なり、ビュー マスター ページには 1 つまたは複数`<asp:ContentPlaceHolder>`タグ。 `<contentplaceholder>`タグを使用して、個々 のコンテンツ ページでオーバーライドできるマスター ページの領域をマークします。

ビュー マスター ページ リスト 1 では、2 つの列のレイアウトを定義します。 2 つが含まれている`<contentplaceholder>`タグ。 1 つ`<ContentPlaceHolder>`列ごとにします。

**1 – を一覧表示します。 `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

リスト 1 でマスター ページには 2 つのビューの本文`<div>`2 つの列に対応するタグ。 列のカスケード スタイル シートのクラスは両方に適用`<div>`タグ。 このクラスは、マスター ページの上部で宣言されているスタイル シートで定義されます。 デザイン ビューに切り替えることで、ビュー マスター ページを表示する方法をプレビューすることができます。 ソース コード エディターの左下にある [デザイン] タブをクリックします (図 2 参照)。


[![デザイナーでマスター ページのプレビュー](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**図 02**: マスター ページ、デザイナーのプレビュー ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))。


### <a name="creating-a-view-content-page"></a>ビュー コンテンツ ページの作成

ビュー マスター ページを作成した後は、1 つまたは複数のビューにビュー マスター ページに基づくコンテンツ ページを作成できます。 Views \home フォルダーを右クリックして、ホーム コント ローラーの Index ビュー コンテンツ ページを作成するなど、選択**追加、新しい項目の**選択、 **MVC ビュー コンテンツ ページ**入力テンプレート(図 3 参照) にボタン名 Index.aspx、したりの追加 をクリックします。


[![ビュー コンテンツ ページを追加します。](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**図 03**: ビュー コンテンツ ページの追加 ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))。


[追加] ボタンをクリックした後はビュー コンテンツ ページに関連付けられたビュー マスター ページを選択することができますの新しいダイアログが表示されます (図 4 参照)。 前のセクションで作成した Site.master ビュー マスター ページに移動することができます。


[![マスター ページの選択](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**図 04**: マスター ページの選択 ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))。


Site.master マスター ページに基づいて、新しいビュー コンテンツ ページを作成した後は、リスト 2 でファイルを取得します。

**2 – を一覧表示します。 `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

このビューが含まれていますが、`<asp:Content>`のそれぞれに対応するタグ、`<asp:ContentPlaceHolder>`ビュー マスター ページ内のタグ。 各`<asp:Content>`タグには、特定を指す ContentPlaceHolderID 属性が含まれています。 `<asp:ContentPlaceHolder>` 、オーバーライドします。

さらに、リスト 2 でのコンテンツ ビュー ページは含まれていないこと、通常の開始タグと HTML 終了タグのいずれかに注意してください。 たとえば、含まないタグと終了`<html>`または`<head>`タグ。 ビュー マスター ページのすべての標準タグと終了タグに格納されます。

内のビュー コンテンツ ページに表示するコンテンツを配置する必要があります、`<asp:Content>`タグ。 任意の HTML またはこれらのタグの外部では、その他のコンテンツを配置する場合、ページを表示しようとしたときにエラーを取得は。

オーバーライドする必要はありませんすべて`<asp:ContentPlaceHolder>`コンテンツ ビュー ページでマスター ページからのタグ。 だけをオーバーライドする必要があります、`<asp:ContentPlaceHolder>`特定のコンテンツのタグで置換するときにタグ付けします。

たとえば、リスト 3 に変更したのインデックス ビューには 2 つのみ`<asp:Content>`タグ。 各、`<asp:Content>`タグにいくつかのテキストが含まれています。

**3 – を一覧表示します。 `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

リスト 3 でビューが要求されると、図 5 ページを表示します。 ビューが 2 つの列でページを表示することに注意してください。 さらに、ビュー マスター ページからコンテンツを持つビュー コンテンツ ページからコンテンツがマージされることに注意してください。


[![インデックス ビューのコンテンツ ページ](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**図 05**: インデックス ビューのコンテンツ ページ ([フルサイズの画像を表示する をクリックします](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))。


### <a name="modifying-view-master-page-content"></a>ビュー マスター ページのコンテンツを変更します。

ビュー マスター ページのコンテンツを別のビュー コンテンツ ページが要求されたときに変更の問題としては、マスター ページの表示を使用する場合にすぐにほぼが発生した問題の 1 つです。 たとえば、各ページで web アプリケーションに固有のタイトルがします。 ただし、ビュー マスター ページで、ビューのコンテンツ ページではなく、タイトルが宣言されています。 そのため、各ビューのコンテンツ ページのページ タイトルをカスタマイズする方法でしょうか。

ビューのコンテンツ ページが表示されるタイトルを変更できる 2 つの方法はあります。 最初に、ページのタイトルの title 属性を割り当てることができます、`<%@ page %>`ビュー コンテンツ ページの上部にあるディレクティブが宣言されています。 たとえば、インデックス ビューにページ タイトル「スーパー優れた web サイト」を割り当てる場合は、Index ビューの上部にある次のディレクティブを含めることができます。

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

インデックス ビューがブラウザーに表示されると、ブラウザーのタイトル バーに目的のタイトルが表示されます。


[![ブラウザーのタイトル バー](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)


マスター ビュー ページが動作する、title 属性の順序で満たす必要がある 1 つの重要な要件があります。 ビュー マスター ページに含める必要があります、`<head runat="server">`タグは、通常ではなく`<head>`のヘッダーのタグ。 場合、`<head>`タグに runat は含まれません ="server"属性、タイトルは表示されません。 マスター ページが含まれていますが、必要な既定のビュー`<head runat="server">`タグ。

変更する地域をラップするという、個々 のビュー コンテンツ ページからマスター ページのコンテンツを変更する方法、`<asp:ContentPlaceHolder>`タグ。 たとえば、タイトルだけでなく、マスター ビュー ページで表示される、メタ タグを変更することを想像してください。 リスト 4 のマスター ビュー ページには、`<asp:ContentPlaceHolder>`タグ内でその`<head>`タグ。

**4 – を一覧表示します。 `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

注意、`<asp:ContentPlaceHolder>`リスト 4 のタグには、既定のコンテンツが含まれています: 既定のタイトルと既定のメタ タグ。 これを上書きしない場合`<asp:ContentPlaceHolder>`タグを個々 のビューのコンテンツ ページで、既定のコンテンツが表示されます。

リスト 5 でコンテンツの表示 ページの上書き、`<asp:ContentPlaceHolder>`カスタム タイトルとカスタム メタ タグを表示するにはタグ。

**5 – を一覧表示します。 `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>まとめ

このチュートリアルは、マスター ページを表示し、コンテンツ ページを表示するための基本的な概要を提供します。 新しいビュー マスター ページを作成し、それらに基づくコンテンツ ページの表示を作成する方法を学習しました。 特定のビュー コンテンツ ページからビュー マスター ページの内容を変更する方法についても確認します。

> [!div class="step-by-step"]
> [前へ](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [次へ](passing-data-to-view-master-pages-vb.md)
