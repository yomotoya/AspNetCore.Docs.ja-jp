---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: カスタム HTML ヘルパー (VB) の作成 |Microsoft Docs
author: microsoft
description: このチュートリアルの目的を MVC ビュー内で使用できるカスタム HTML ヘルパーを作成する方法を示すことです。 HTML ヘルパーを活用しています.
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: ed3f86d3f316d3e1630c6008c20c72c31d7864da
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821527"
---
<a name="creating-custom-html-helpers-vb"></a>カスタム HTML ヘルパー (VB) の作成
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> このチュートリアルの目的を MVC ビュー内で使用できるカスタム HTML ヘルパーを作成する方法を示すことです。 HTML ヘルパーを利用して HTML タグの標準の HTML ページを作成するために必要ことの面倒な型指定の量を減らすことができます。


このチュートリアルの目的を MVC ビュー内で使用できるカスタム HTML ヘルパーを作成する方法を示すことです。 HTML ヘルパーを利用して HTML タグの標準の HTML ページを作成するために必要ことの面倒な型指定の量を減らすことができます。

このチュートリアルの最初の部分では、ASP.NET MVC フレームワークに含まれている既存の HTML ヘルパーの一部を取り上げます。 次に、カスタム HTML ヘルパーの作成の 2 つの方法を説明しました。 共有メソッドを作成し、拡張メソッドを作成して、カスタム HTML ヘルパーを作成する方法を説明します。

## <a name="understanding-html-helpers"></a>HTML ヘルパーを理解します。

HTML ヘルパーは、文字列を返す方法だけです。 文字列は、コンテンツの任意の型を表すことができます。 たとえば、HTML と同様に、標準の HTML タグをレンダリングする HTML ヘルパーを使用できます`<input>`と`<img>`タグ。 タブ ストリップまたはデータベースのデータの HTML テーブルなどのより複雑なコンテンツをレンダリングするのに HTML ヘルパーを使用することができますも。

ASP.NET MVC フレームワークには、次の一連標準の HTML ヘルパー (これは完全な一覧) にはが含まれています。

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

たとえば、リスト 1 でフォームを検討してください。 (図 1 参照)、標準の HTML ヘルパーの 2 つのヘルプでは、このフォームが表示されます。 このフォームを使用して、`Html.BeginForm()`と`Html.TextBox()`ヘルパー メソッド。


[![HTML ヘルパーのページが表示されます。](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**図 01**: HTML ヘルパーのページが表示される ([フルサイズの画像を表示する をクリックします](creating-custom-html-helpers-vb/_static/image3.png))。


**1 – を一覧表示します。 `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

`Html.BeginForm()`かっこと右の HTML を作成するヘルパー メソッドが使用される`<form>`タグ。 注意、`Html.BeginForm()`内を使用して、メソッドが呼び出されるステートメントです。 確実にステートメントを使用して、`<form>`を使用して、最後にタグが閉じられるブロックします。

使用して、作成する代わりに、希望する場合、ブロックを終了する Html.EndForm() ヘルパー メソッドを呼び出すことができます、`<form>`タグ。 作成の開始と終了にどの方法を使用して、`<form>`最も直感的にすると思われるタグ。

`Html.TextBox()`ヘルパー メソッドは、リスト 1 で HTML を表示するために使用される`<input>`タグ。 リスト 2 での HTML ソースを表示し、ブラウザーでソースの表示を選択します。 場合、 標準の HTML タグが含まれていることを確認します。

> [!IMPORTANT]
> 注意、 `Html.TextBox()`HTML ヘルパーでレンダリングされて`<%= %>`タグの代わりに`<% %>`タグ。 等号 (=) を含めない場合、ブラウザーに表示 nothing を取得します。

ASP.NET MVC フレームワークには、ヘルパーの小さなセットが含まれています。 ほとんどの場合、カスタム HTML ヘルパーの MVC フレームワークを拡張する必要があります。 このチュートリアルの残りの部分では、カスタム HTML ヘルパーの作成の 2 つの方法について説明します。

**2 – を一覧表示します。 `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>共有の方法で HTML ヘルパーの作成

新しい HTML ヘルパーを作成する最も簡単な方法では、文字列を返す共有メソッドを作成します。 HTML をレンダリングする新しい HTML ヘルパーの作成を決定することなど想像`<label>`タグ。 表示するために、リスト 2 でクラスを使用することができます、`<label>`します。

**2 – を一覧表示します。 `Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

リスト 2 でクラスについて特別なものがあります。 `Label()`メソッドは単に文字列を返します。

リスト 3 に変更したのインデックス ビューを使用して、 `LabelHelper` HTML をレンダリングする`<label>`タグ。 ビューが含まれていますが、 `<%@ imports %>` Application1.Helpers 名前空間をインポートするディレクティブ。

**2 – を一覧表示します。 `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>拡張メソッドで HTML ヘルパーの作成

作成する場合のみ動作する HTML ヘルパーなどの拡張メソッドを作成する必要があります、ASP.NET MVC フレームワークに含まれる標準の HTML ヘルパー。 拡張メソッドを使用すると、既存のクラスに新しいメソッドを追加できます。 新しいメソッドを追加する、HTML ヘルパー メソッドを作成するときに、`HtmlHelper`ビューの Html プロパティによって表されるクラス。

リスト 3 の Visual Basic モジュール拡張メソッドをという名前の追加`Label()`を`HtmlHelper`クラス。 このモジュールについて注意すべきいくつかがあります。 最初に、モジュールがで修飾されたことに注意してください、`<Extension()>`属性。 この属性を使用するためにインポートする必要があります、`System.Runtime.CompilerServices`名前空間

次に、注意の最初のパラメーター、`Label()`メソッドを表します、`HtmlHelper`クラス。 拡張メソッドの最初のパラメーターでは、拡張メソッドを拡張するクラスを示します。

**3 – を一覧表示します。 `Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

拡張メソッドがすべてのクラスの他のメソッドのように Visual Studio の Intellisense に表示されます、拡張メソッドを作成し、アプリケーションを正常にビルドした後 (図 2 参照)。 唯一の違いは、その拡張機能メソッドがそれら (下向きの矢印のアイコン) の横にある特別なシンボルを表示します。


[![Html.Label() の拡張メソッドを使用](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**図 02**: Html.Label() の拡張メソッドを使用して ([フルサイズの画像を表示する をクリックします](creating-custom-html-helpers-vb/_static/image6.png))。


リスト 4 変更後のインデックス ビューのすべてを表示するために、Html.Label() 拡張メソッドを使用してその&lt;ラベル&gt;タグ。

**4 – を一覧表示します。 `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>まとめ

このチュートリアルでは、カスタム HTML ヘルパーの作成の 2 つの方法について説明しました。 最初に、カスタムを作成する方法を学習しました。`Label()`共有メソッドを作成して HTML ヘルパーは、文字列を返します。 次に、カスタムを作成する方法を学習しました。 `Label()` HTML ヘルパー メソッドで拡張メソッドを作成して、`HtmlHelper`クラス。

このチュートリアルでは、非常に単純な HTML ヘルパー メソッドの構築に注目しました。 必要な複雑な作業は HTML ヘルパーであることに注意してください。 ツリー ビュー、メニューのまたはデータベースのデータのテーブルなどのリッチ コンテンツをレンダリングする HTML ヘルパーをビルドすることができます。

> [!div class="step-by-step"]
> [前へ](asp-net-mvc-views-overview-vb.md)
> [次へ](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
