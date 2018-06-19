---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: ASP.NET mvc の DropDownList ヘルパーの使用 |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 21373deeded801c5cea9e89f6dac0f3542a55ca5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875592"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>ASP.NET mvc の DropDownList ヘルパーの使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

このチュートリアルがの操作の基礎を学習、 [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)ヘルパーと[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) ASP.NET MVC Web アプリケーションでヘルパー。 Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版のチュートリアルに従うに Microsoft Visual Studio を使用することができます。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。

- [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)

Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。 このチュートリアルでは完了して、 [ASP.NET Mvc](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)チュートリアルまたは[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md)またはチュートリアルは、ASP.NET MVC の開発に慣れています。 このチュートリアルがから変更されたプロジェクトで始まる、 [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。 次のリンクのスタート プロジェクトをダウンロードする[c# バージョンをダウンロード](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829)です。

Visual Web Developer のプロジェクトが完了したチュートリアル c# ソース コードは、このトピックの使用可能です。 [ダウンロード](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d)です。

### <a name="what-youll-build"></a>新機能のビルドします。

アクション メソッドとを使用するビューを作成、 [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx)カテゴリを選択するためのヘルパー。 使用することも、 **jQuery** (ジャンル、アーティストなど)、新しいカテゴリが必要なときに使用できる挿入カテゴリ ダイアログを追加します。 新しいジャンルを追加し、新しいアーティストの追加へのリンクを表示する作成ビューのスクリーン ショットを次に示します。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>スキルの学習

学習する内容を次に示します。

- 使用する方法、 [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx)カテゴリ データを選択するためのヘルパー。
- 追加する方法、 **jQuery**ダイアログ ボックスの新しいカテゴリを追加します。

### <a name="getting-started"></a>作業の開始

次のリンクのスタート プロジェクトをダウンロードして開始[ダウンロード](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829)です。 Windows エクスプ ローラーで右クリック、 *DDL\_Starter.zip*ファイルのプロパティ を選択します。 **DDL\_Starter.zip プロパティ**ダイアログ ボックスで、ブロック解除します。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

DDL を右クリックして\_Starter.zip ファイルと選択**すべて展開**ファイルを解凍します。 開く、 *StartMusicStore.sln* Visual Web Developer 2010 Express ("Visual Web Developer"または"VWD"略して) や Visual Studio 2010 でファイル。

Ctrl キーを押しながら f5 キーを押してアプリケーションを実行し、をクリックして、**テスト**リンクします。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

選択、**ムービーのカテゴリを選択する (単純)** リンクします。 ムービーの種類の選択のリストが表示され、コメディ、選択した値。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

ブラウザーとソースの選択の表示を右クリックします。 ページの HTML が表示されます。 次のコードは、HTML 要素を選択します。

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

選択リスト内の各項目が (アクションの場合は 0、1 ドラマ ・のコメディ 2 および 3 サイエンス フィクションの) 値と (アクション、ドラマ ・、コメディおよびサイエンス フィクション) の表示名を持つことを確認できます。 上記のコードは、選択リストの標準 HTML です。

ドラマ ・とヒットする選択リストを変更、**送信**ボタンをクリックします。 ブラウザー内の URL が`http://localhost:2468/Home/CategoryChosen?MovieType=1`ページが表示および**選択した: 1**です。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

開く、 *controllers \homecontroller.cs*ファイルを確認、`SelectCategory`メソッドです。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) HTML 選択リストの作成に使用されるヘルパーが必要です、 **IEnumerable&lt;SelectListItem &gt;** 、明示的または暗黙的にします。 つまりに渡すことができます、 **IEnumerable&lt;SelectListItem &gt;** 明示的に、 [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)ヘルパーを追加したり、 **IEnumerable&lt;SelectListItem &gt;** を[ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)に同じ名前を使用して、 **SelectListItem**モデル プロパティとします。 渡して、 **SelectListItem**暗黙的および明示的については、チュートリアルの次の部分で説明します。 上記のコードが作成する最も簡単な方法を示します、 **IEnumerable&lt;SelectListItem &gt;** し、テキストと値を設定します。 注、 `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)が、[選択](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx)プロパティに設定**true;** これにより、レンダリングされた選択リストを表示する**コメディ** 、一覧で選択したアイテムとして。

**IEnumerable&lt;SelectListItem &gt;** 作成以降に追加、 [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) MovieType 名前を持つ。 これは、ここを渡す方法、 **IEnumerable&lt;SelectListItem &gt;** に暗黙的に、 [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)次に示すヘルパー。

開く、 *Views\Home\SelectCategory.cshtml*ファイルし、マークアップを確認します。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

3 番目の行のレイアウトに設定 Views/shared/\_単純\_Layout.cshtml で、標準のレイアウト ファイルの簡易バージョンです。 これは、表示状態に保つに、単純な HTML をレンダリングおいたします。

このサンプルでは変更されません、アプリケーションの状態を使用してデータを送信するように、 **HTTP GET**ではなく、 **HTTP POST**です。 W3C のセクションを参照して[を選択する HTTP GET または POST クイック チェックリスト](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist)です。 使用してアプリケーションを変更して、フォームの送信おはありません、ため、 [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx)アクション メソッド、コント ローラーとフォーム メソッドを指定できるオーバー ロード (**HTTP POST**または**HTTP GET**)。 通常のビューが含まれて、 [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx)パラメーターを受け取らないオーバー ロードします。 バージョンなしパラメーター既定値は、同じアクション メソッドとコント ローラーの POST バージョンにフォーム データの送信。

次の行

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

文字列引数を渡す、 **DropDownList**ヘルパー。 この文字列の例では、"MovieType"には、2 つの処理が行われます。

- キーを提供、 **DropDownList**を検索するためのヘルパーを**IEnumerable&lt;SelectListItem &gt;** で、 **ViewBag**です。
- これは、データにバインドされて、MovieType form 要素。 送信メソッドがある場合**HTTP GET**、`MovieType`はクエリ文字列になります。 送信メソッドがある場合**HTTP POST**、`MovieType`メッセージの本文に追加されます。 次の図は、1 の値を持つクエリ文字列を示しています。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

次のコードは、`CategoryChosen`メソッドに、フォームが送信されました。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

テスト ページに戻ると、選択、 **HTML SelectList**リンクします。 HTML ページは、単純な ASP.NET MVC テスト ページと同様に、select 要素を表示します。 ブラウザー ウィンドウを右クリックし、選択**ソースの表示**です。 選択リストの HTML マークアップでは、基本的に同じです。 テスト、HTML ページ、ASP.NET MVC アクション メソッドとテストを経てビューと同様に動作します。

### <a name="improving-the-movie-select-list-with-enums"></a>ムービーの選択リストを列挙体の向上

場合は、アプリケーション内のカテゴリは固定でありは変更されません、堅牢性と、拡張するシンプルなコードにするための列挙値の利用できます。 新しいカテゴリを追加すると、適切なカテゴリ値が生成されます。 新しいカテゴリを追加するカテゴリの値の更新を忘れた場合に、コピーと貼り付けのエラーを回避できます。

開く、 *controllers \homecontroller.cs*ファイルし、次のコードを調べます。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` 4 つのムービーの種類をキャプチャします。 `SetViewBagMovieType`メソッドを作成、 **IEnumerable&lt;SelectListItem &gt;** から、 `eMovieCategories` **enum**、設定と、 `Selected` からプロパティ`selectedMovie`パラメーター。 `SelectCategoryEnum`アクション メソッドとして同じビューを使用して、`SelectCategory`アクション メソッド。

テスト ページに移動し、をクリックして、`Select Movie Category (Enum)`リンクします。 このとき、表示されている値 (数)、代わりに、列挙型を表す文字列が表示されます。

### <a name="posting-enum-values"></a>列挙値の送信

HTML フォームは通常、サーバーへのデータの投稿に使用されます。 次のコードは、`HTTP GET`と`HTTP POST`のバージョン、`SelectCategoryEnumPost`メソッドです。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

渡すことによって、`eMovieCategories`列挙型、`POST`メソッド、お列挙型の値と、列挙型の文字列の両方を抽出できます。 サンプルを実行し、テストのページに移動します。 をクリックして、`Select Movie Category(Enum Post)`リンクします。 ムービーの種類を選択し、[送信] ボタンをクリックします。 ムービーの種類の名前と値の両方が表示されます。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>複数のセクションを Select 要素を作成します。

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) HTML を表示する HTML ヘルパー`<select>`を持つ要素、`multiple`属性は、複数の項目を選択することができます。 テストのリンクに移動し、選択、**複数選択国**リンクします。 表示される UI では、複数の国を選択することができます。 次の図でカナダと中国が選択されます。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>MultiSelectCountry コードを確認します。

次のコードを調べて、 *controllers \homecontroller.cs*ファイル。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries`メソッドは、国のリストを作成しに渡します、`MultiSelectList`コンス トラクターです。 `MultiSelectList`コンス トラクターのオーバー ロードで使用される、`GetCountries`上記のメソッドは次の 4 つのパラメーターを受け取ります。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)リスト内の項目を格納します。 国の一覧の上の例。
2. *dataValueField*: でプロパティの名前、 **IEnumerable**値を含む一覧。 上記の例では、`ID`プロパティです。
3. *dataTextField*: でプロパティの名前、 **IEnumerable**を表示する情報を含むリスト。 上記の例では、`name`プロパティです。
4. *selectedValues*: 選択された値の一覧です。

上記の例では、`MultiSelectCountry`メソッドは、 `null` UI が表示されたら、国を選択しないように、特定の国の値。 次のコードは、Razor マークアップを表示するために使用される、`MultiSelectCountry`ビュー。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

HTML ヘルパー [ListBox](https://msdn.microsoft.com/library/dd470200.aspx)メソッドでは、2 つのパラメーター、モデル バインドするプロパティの名前を受け取る前に使用され、 [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx)オプションを選択し、値を格納しています。 `ViewBag.YouSelected`上記のコードを使用してフォームを送信するときに選択した国の値を表示します。 HTTP POST のオーバー ロードを調べて、`MultiSelectCountry`メソッドです。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected`動的プロパティには取得、選択した国が含まれています、`Countries`フォーム コレクション内のエントリ。 GetCountries メソッドにはこのバージョンでは、選択した国の一覧が渡されますため、`MultiSelectCountry`ビューが表示されたら、選択した国が UI で選択されています。

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>選択要素フレンドリ収穫選択 jQuery プラグインの作成

収穫[選択](http://harvesthq.github.com/chosen/)HTML に jQuery プラグインを追加することができます&lt;選択&gt;にユーザーを作成する要素フレンドリ UI。 次の図は、収穫を示す[選択](http://harvesthq.github.com/chosen/)と jQuery プラグイン`MultiSelectCountry`ビュー。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

以下の 2 つのイメージで**カナダ**が選択されています。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

上記の図では Canada が選択されているとが含まれている、 **x**選択を解除する をクリックすることができます。 日本が選択されているし、次の図がカナダ、中国を示しています。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>収穫選択 jQuery プラグインをフックします。

次のセクションでが jQuery と経験がある場合は、次に簡単です。 前に jQuery を初めて使用する場合は、jQuery チュートリアルでは、次のいずれかの操作を再試行してください。

- [JQuery のしくみ](http://docs.jquery.com/Tutorials:How_jQuery_Works)によって[John Resig](http://ejohn.org/)
- [JQuery の使用を開始する](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery)によって[Jörn Zaefferer](http://bassistance.de/)
- [JQuery の例をライブ](http://codylindley.com/blogstuff/js/jquery/#)によって[Cody Lindley](http://codylindley.com/)

選択したプラグインは、starter およびこのチュートリアルに付属する完全なサンプル プロジェクトに含まれます。 このチュートリアルではだけで済みます UI に接続してに jQuery を使用します。 を ASP.NET MVC プロジェクトに収穫選択 jQuery プラグインを使用するのには、次の必要があります。

1. 選択したプラグインのダウンロード[github](https://github.com/harvesthq/chosen/)です。 この手順が実施されました。
2. ASP.NET MVC プロジェクトを選択したフォルダーを追加します。 選択したフォルダーに、前の手順でダウンロードした、選択したプラグインから資産を追加します。 この手順が実施されました。
3. 選択したプラグインをフック、 **DropDownList**または**ListBox** HTML ヘルパー。

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>MultiSelectCountry ビューを選択したプラグインをフックします。

開く、 *Views\Home\MultiSelectCountry.cshtml*ファイルを追加、`htmlAttributes`パラメーターを`Html.ListBox`です。 パラメーター追加するにはには、選択リストのクラス名が含まれています。 (`@class = "chzn-select"`)。 完成したコードは、次に示します。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

上記のコードで HTML 属性と属性値を追加するお`class = "chzn-select"`です。 @ 文字クラスの前は、Razor ビュー エンジンを行うには何も行われません。 `class` [c# キーワード](https://msdn.microsoft.com/library/x53a06bb.aspx)です。 @ プレフィックスとしてを含めない限り、c# のキーワードを識別子として使用できません。 上記の例で`@class`有効な識別子が、**クラス**ではありません**クラス**キーワードします。

参照を追加、 *Chosen/chosen.jquery.js*と*Chosen/chosen.css*ファイル。 *Chosen/chosen.jquery.js*を実装して、選択したプラグインの機能的します。 *Chosen/chosen.css*ファイルは、スタイルを提供します。 一番下にこれらの参照を追加、 *Views\Home\MultiSelectCountry.cshtml*ファイル。 次のコードでは、選択したプラグインを参照する方法を示します。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

使用されるクラス名を使用して、選択したプラグインをアクティブ化、 **Html.ListBox**コード。 上記の例ではクラス名は`chzn-select`します。 下に次の行を追加、 *Views\Home\MultiSelectCountry.cshtml*ファイルを表示します。 この行には、選択したプラグインがアクティブにします。

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

次の行は、クラス名で DOM 要素を選択する jQuery ready 関数を呼び出す構文`chzn-select`です。

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

選択したメソッドをラップし、上の呼び出しから返されるセットに適用 (`.chosen();`)、選択したプラグインをフックします。

次のコードは完成した*Views\Home\MultiSelectCountry.cshtml*ファイルを表示します。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

アプリケーションを実行しに移動し、`MultiSelectCountry`ビュー。 Try を追加して、国を削除します。 提供されたサンプルのダウンロードも含まれています、`MultiCountryVM`メソッドとビューを使用して MultiSelectCountry 機能を実装するビューのモデルの代わりに、 **ViewBag**です。

次のセクションで、ASP.NET MVC のスキャフォールディング機構の使用方法が表示されます、 **DropDownList**ヘルパー。

> [!div class="step-by-step"]
> [次へ](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
