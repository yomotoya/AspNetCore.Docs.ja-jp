---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: カスタム HTML ヘルパー (c#) の作成 |Microsoft ドキュメント
author: microsoft
description: このチュートリアルの目的では、MVC ビュー内で使用できるカスタムの HTML ヘルパーの作成方法を示しています。 HTML ヘルパーを活用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ebc9aa2aa8dbc02dc01833d671c3bfd19141ba74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869726"
---
<a name="creating-custom-html-helpers-c"></a>カスタム HTML ヘルパー (c#) の作成
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> このチュートリアルの目的では、MVC ビュー内で使用できるカスタムの HTML ヘルパーの作成方法を示しています。 HTML ヘルパーを利用して、標準の HTML ページを作成する必要がありますを HTML タグの面倒な入力量を減らすことができます。


このチュートリアルの目的では、MVC ビュー内で使用できるカスタムの HTML ヘルパーの作成方法を示しています。 HTML ヘルパーを利用して、標準の HTML ページを作成する必要がありますを HTML タグの面倒な入力量を減らすことができます。

このチュートリアルの最初の部分では、ASP.NET MVC フレームワークに含まれている既存の HTML ヘルパーの一部を説明します。 次に、カスタム HTML ヘルパーの作成の 2 つの方法を説明しました。 静的メソッドを作成し、拡張メソッドを作成することで、カスタム HTML ヘルパーを作成する方法について説明します。

## <a name="understanding-html-helpers"></a>HTML ヘルパーを理解します。

HTML ヘルパーは、文字列を返すメソッドのみです。 文字列は、内容の任意の型を表すことができます。 たとえば、HTML などの標準の HTML タグを表示するために HTML ヘルパーを使用できます`<input>`と`<img>`タグ。 タブ ストリップまたはデータベースのデータの HTML テーブルなどのより複雑なコンテンツを表示するために HTML ヘルパーを使用することができますも。

ASP.NET MVC フレームワークには、次の (完全な一覧では無効) 標準の HTML ヘルパーのセットが含まれています。

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

たとえば、1 を一覧表示するフォームがあるとします。 (図 1 を参照してください)、標準の HTML ヘルパーの 2 つのヘルプでは、このフォームが表示されます。 このフォームを使用して、`Html.BeginForm()`と`Html.TextBox()`単純な HTML フォームを表示するためにヘルパー メソッドです。


[![HTML ヘルパー ページが表示されます。](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**図 01**: HTML ヘルパー ページが表示されます ([フルサイズのイメージを表示するをクリックして](creating-custom-html-helpers-cs/_static/image3.png))


**1 – を一覧表示します。 `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Html.BeginForm() ヘルパー メソッドを使用開始と終了の HTML の作成を`<form>`タグ。 注意して、`Html.BeginForm()`を使用して、内でメソッドが呼び出されたステートメントです。 確実にステートメントを使用して、`<form>`を使用して、最後にタグが閉じられるブロックします。

場合を使用して、作成する代わりに、ブロックを終了する Html.EndForm() ヘルパー メソッドを呼び出すことができます、`<form>`タグ。 タグを作成して、終了するには、どの方法を使用して`<form>`ように最も理解できるタグです。

`Html.TextBox()`ヘルパー メソッドは HTML を表示するリスト 1 で使用される`<input>`タグ。 お使いのブラウザーでソースの表示を選択した場合をリスト 2 での HTML ソースを表示します。 ソースが標準の HTML タグが含まれていることを確認します。

> [!IMPORTANT]
> 注意して、 `Html.TextBox()`HTML ヘルパーでレンダリングされて`<%= %>`の代わりにタグ`<% %>`タグ。 等号 (=) を含めない場合は、ブラウザーにレンダリング取得が何も行われません。

ASP.NET MVC フレームワークには、ヘルパーの少数のセットが含まれています。 ほとんどの場合、カスタム HTML ヘルパーに MVC フレームワークを拡張する必要があります。 このチュートリアルの残りの部分では、カスタム HTML ヘルパーの作成の 2 つの方法を学習します。

**2 – を一覧表示します。 `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>静的メソッドと HTML ヘルパーの作成

新しい HTML ヘルパーを作成する最も簡単な方法では、文字列を返す静的メソッドを作成します。 たとえば、HTML をレンダリングする新しい HTML ヘルパーの作成を決定すること`<label>`タグ。 表示するために一覧表示する 2 でクラスを使用することができます、`<label>`です。

**2 – を一覧表示します。 `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

2 のリスト内のクラスに関する特別なはありません。 `Label()`メソッドは単に文字列を返します。

リスト 3 の変更、インデックス ビューを使用して、 `LabelHelper` HTML をレンダリングする`<label>`タグ。 ビューに含まれることに注意してください、`<%@ imports %>`をインポートするディレクティブ、`Application1.Helpers`名前空間。

**2 – を一覧表示します。 `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>拡張メソッドで HTML ヘルパーの作成

作成する場合だけに使用できる HTML ヘルパーなどの拡張メソッドを作成する必要があります、ASP.NET MVC フレームワークに含まれる標準の HTML ヘルパー。 拡張メソッドを使用すると、既存のクラスに新しいメソッドを追加できます。 HTML ヘルパー メソッドを作成する場合は、新しいメソッドをビューの Html プロパティによって表される HtmlHelper クラスに追加します。

リスト 3 のクラスを追加する拡張メソッド、`HtmlHelper`という名前のクラス`Label()`です。 このクラスについて注意すべき点が 2 つがあります。 最初に、クラスが、静的クラスであることを確認します。 静的クラスを持つ拡張メソッドを定義する必要があります。

次に、ことに注意しての最初のパラメーター、`Label()`メソッドは、キーワードに続く`this`です。 拡張メソッドの最初のパラメーターでは、拡張メソッドを拡張するクラスを示します。

**3 – を一覧表示します。 `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

拡張メソッドがすべてのクラスの他のメソッドと同様に Visual Studio Intellisense に表示されます、拡張メソッドを作成し、アプリケーションを正常にビルドした後 (図 2 を参照してください)。 唯一の違いは、その拡張メソッドは、特別な記号が横に (下向きの矢印のアイコン) で表示されます。


[![Html.Label() 拡張メソッドの使用](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**図 02**: Html.Label() 拡張メソッドの使用 ([フルサイズのイメージを表示するをクリックして](creating-custom-html-helpers-cs/_static/image6.png))


インデックス ビューを一覧表示する 4 でのすべてを表示するために Html.Label() 拡張メソッドを使用してその`<label>`タグ。

**4 – を一覧表示します。 `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>まとめ

このチュートリアルでは、カスタム HTML ヘルパーの作成の 2 つの方法を学習しました。 最初に、カスタムを作成する方法を学習しました`Label()`静的メソッドを作成することで HTML ヘルパーは、文字列を返します。 次に、カスタムを作成する方法を学習しました`Label()`HTML ヘルパー メソッドの拡張メソッドを作成することで、`HtmlHelper`クラスです。

このチュートリアルでは、非常に単純な HTML ヘルパー メソッドを構築に注目しました。 HTML ヘルパーが必要な複雑な作業できることに注意してください。 ツリー ビュー、メニューのまたはデータベースのデータのテーブルなどの豊富なコンテンツを表示する HTML ヘルパーをビルドすることができます。

> [!div class="step-by-step"]
> [前へ](asp-net-mvc-views-overview-cs.md)
> [次へ](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
