---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: ビュー マスター ページ (VB) のページ レイアウトの作成 |Microsoft ドキュメント
author: microsoft
description: このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページに共通のページ レイアウトを作成する方法を学習します。 使用することができます、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 5208cedd8d24a290a0227bdcbaa84ae6210cd969
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879622"
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a>ビュー マスター ページ (VB) のページ レイアウトの作成
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページに共通のページ レイアウトを作成する方法を学習します。 たとえば、ビュー マスター ページを使用する 2 つの列のページ レイアウトを定義し、2 つの列レイアウトのすべての web アプリケーションでページを使用します。


## <a name="creating-page-layouts-with-view-master-pages"></a>ビュー マスター ページのページ レイアウトの作成

このチュートリアルでは、ビュー マスター ページを利用して、アプリケーションで複数のページに共通のページ レイアウトを作成する方法を学習します。 たとえば、ビュー マスター ページを使用する 2 つの列のページ レイアウトを定義し、2 つの列レイアウトのすべての web アプリケーションでページを使用します。

利用できますビューのマスター ページ、アプリケーションで複数のページ間での一般的なコンテンツを共有します。 たとえば、ビュー マスター ページで、web サイトのロゴ、ナビゲーション リンク、および著作権情報の提供情報を配置できます。 このように、アプリケーション内の各ページはこのコンテンツを自動的に表示します。

このチュートリアルでは、新しいビュー マスター ページを作成し、新しいビューに基づいてコンテンツ ページ、マスター ページを作成する方法を学習します。

### <a name="creating-a-view-master-page"></a>ビュー マスター ページの作成

2 つの列のレイアウトを定義するビュー マスター ページの作成を始めます。 追加する新しいビュー マスター ページ、MVC プロジェクトを \shared フォルダーを右クリックしてメニュー オプションを選択する**追加]、[新しい項目の**、MVC ビュー マスター ページのテンプレートを選択して (図 1 を参照してください)。


[![ビュー マスター ページを追加します。](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**図 01**: ビュー マスター ページを追加する ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))


アプリケーションでは、1 つ以上のビュー マスター ページを作成できます。 各ビュー マスター ページには、別のページ レイアウトを定義できます。 たとえば、2 つの列のレイアウトに特定のページおよびその他のページ 3 列のレイアウトをする可能性があります。

ビュー マスター ページの外観標準の ASP.NET MVC ビューとよく似ています。 ただし、通常のビューとは異なりビュー マスター ページに含まれる 1 つまたは複数`<asp:ContentPlaceHolder>`タグ。 `<contentplaceholder>`タグはコンテンツの個別のページでオーバーライド可能なマスター ページの領域を示すために使用されます。

たとえば、ビュー マスター ページ 1 の一覧表示するのには、2 列のレイアウトを定義します。 2 つが含まれている`<contentplaceholder>`タグ。 1 つ`<ContentPlaceHolder>`列ごとにします。

**1 – を一覧表示します。 `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

マスター ページ 1 の一覧表示するには 2 つのビューの本文`<div>`2 つの列に対応するタグです。 カスケード スタイル シート列クラスは、両方に適用`<div>`タグ。 このクラスは、マスター ページの上部で宣言されているスタイル シートで定義されます。 デザイン ビューに切り替えることによって、ビュー マスター ページのレンダリング方法をプレビューすることができます。 ソース コード エディターの左下に [デザイン] タブをクリックして (図 2 を参照してください)。


[![デザイナー内のマスター ページをプレビューします。](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**図 02**: マスター ページ、デザイナーでのプレビュー ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))


### <a name="creating-a-view-content-page"></a>ビューのコンテンツ ページを作成します。

ビュー マスター ページを作成した後は、1 つまたは複数のビューに、ビュー マスター ページに基づくコンテンツ ページを作成できます。 Views \home フォルダーを右クリックして、Home コント ローラーのインデックス ビュー コンテンツ ページを作成するを選択すると**追加、新しい項目の**を選択すると、 **MVC ビューのコンテンツ ページ**テンプレートを入力します。Index.aspx、名前との追加 をクリックすると (図 3 を参照してください) ボタンをクリックします。


[![ビューのコンテンツ ページを追加します。](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**図 03**: ビューのコンテンツ ページを追加する ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))


[追加] ボタンをクリックした後、新しいダイアログが表示されますビューのコンテンツ ページに関連付けられたビュー マスター ページを選択することができます (図 4 を参照してください)。 前のセクションで作成した Site.master ビュー マスター ページに移動することができます。


[![マスター ページの選択](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**図 04**: マスター ページの選択 ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))


Site.master マスター ページに基づいて、新しいビュー コンテンツ ページを作成した後は、2 の一覧でファイルを取得します。

**2 – を一覧表示します。 `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

このビューに表示される、`<asp:Content>`のそれぞれに対応するタグ、`<asp:ContentPlaceHolder>`ビュー マスター ページ内のタグ。 各`<asp:Content>`タグには、特定を指す ContentPlaceHolderID 属性が含まれています。`<asp:ContentPlaceHolder>`オーバーライドします。

さらに、2 のリスト内のコンテンツ ビュー ページは含まれていないこと、通常のタグと終了 HTML タグのいずれかに注意してください。 たとえばを含まないタグと終了`<html>`または`<head>`タグ。 ビュー マスター ページでは、通常タグと終了タグのすべてが含まれます。

ビューのコンテンツ ページに表示するすべてのコンテンツ内に配置する必要があります、`<asp:Content>`タグ。 場合は、任意の HTML またはこれらのタグの外部では、他のコンテンツを配置すると、ページを表示しようとしたときにエラーが取得されます。

オーバーライドする必要はありませんすべて`<asp:ContentPlaceHolder>`コンテンツ ビュー ページで、マスター ページからのタグ。 のみをオーバーライドする必要があります、`<asp:ContentPlaceHolder>`特定のコンテンツのタグで置換するときにタグ付けします。

たとえば、3 の一覧表示するインデックス ビューには 2 つのみ`<asp:Content>`タグ。 各、`<asp:Content>`タグには、いくつかのテキストが含まれています。

**3 – を一覧表示します。 `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

3 の一覧表示するビューが要求されたときに、図 5 にページを表示します。 ビューが 2 つの列を含むページを表示することに注意してください。 さらに、ビューのマスター ページからコンテンツを含むビューのコンテンツ ページの内容がマージされたことに注意してください。


[![インデックス ビューのコンテンツ ページ](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**図 05**:、インデックス ビューのコンテンツ ページ ([フルサイズのイメージを表示するをクリックして](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>ビュー マスター ページの内容を変更します。

ほとんどが発生した問題の 1 つがマスター ページの表示を操作するときにすぐには別のビューのコンテンツ ページが要求されたときに、マスター ページ コンテンツの表示を変更した場合の問題です。 たとえば、web アプリケーションに固有のタイトルがで各ページを希望します。 ただし、ビュー マスター ページでは、ビューのコンテンツ ページではなく、タイトルが宣言されています。 そのため、各ビューのコンテンツ ページのページのタイトルをカスタマイズする方法ですか?

ビューのコンテンツ ページで表示されるタイトルを変更できる 2 つの方法はあります。 ページ タイトルの title 属性を最初に、割り当てることができます、`<%@ page %>`ビューのコンテンツ ページの上部にあるディレクティブが宣言されています。 たとえば、インデックス ビューに、ページ タイトル「スーパー優れた web サイト」を割り当てる場合は、インデックス ビューの上部にある次のディレクティブを含めることができます。

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

インデックス ビューは、ブラウザーに表示される、目的のタイトルは、ブラウザーのタイトル バーに表示されます。


[![ブラウザーのタイトル バー](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)


マスター ビュー ページを使用するタイトル属性の順序で満たす必要がある 1 つの重要な要件です。 ビュー マスター ページを含める必要があります、`<head runat="server">`通常ではなくタグ`<head>`ヘッダーのタグ。 場合、`<head>`タグは、runat は含まれません ="server"属性タイトルは表示されません。 マスター ページが含まれていますが、必要な既定のビュー`<head runat="server">`タグ。

個々 のビューのコンテンツ ページからマスター ページのコンテンツを変更する他の方法で変更する地域をラップする、`<asp:ContentPlaceHolder>`タグ。 たとえば、タイトル、だけでなく、マスター ビュー ページによってレンダリングされるメタ タグを変更することを想像してください。 マスター ビュー ページ 4 の一覧表示するのには、`<asp:ContentPlaceHolder>`タグ内でその`<head>`タグ。

**4 – を一覧表示します。 `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

注意して、`<asp:ContentPlaceHolder>`リスト 4 内のタグには、既定のコンテンツが含まれています。 既定のタイトルと既定のメタ タグ。 これをオーバーライドしない場合は`<asp:ContentPlaceHolder>`タグを個々 のビューのコンテンツ ページで既定のコンテンツが表示されます。

リスト 5 内のコンテンツ ビュー ページよりも優先、`<asp:ContentPlaceHolder>`カスタム タイトルとのカスタム メタ タグを表示するためにタグ。

**5 – を一覧表示します。 `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>まとめ

このチュートリアルは、マスター ページを表示し、コンテンツ ページを表示するための基本的な概要を提供します。 新しいビュー マスター ページを作成し、それらに基づくコンテンツ ページの表示を作成する方法について学習しました。 特定のビューのコンテンツ ページからビュー マスター ページの内容を変更する方法をも検討します。

> [!div class="step-by-step"]
> [前へ](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [次へ](passing-data-to-view-master-pages-vb.md)
