---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: ASP.NET MVC で DropDownList ヘルパーの使用 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 600a8ad409d55f0b01eedc8168eaec2b39c88a3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838350"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>ASP.NET MVC で DropDownList ヘルパーを使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

このチュートリアルでは説明する操作の基本、 [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)ヘルパーと[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) ASP.NET MVC Web アプリケーションでヘルパー。 Microsoft Visual Web Developer 2010 Express Service Pack 1、これはチュートリアルを実行する Microsoft Visual Studio の無料版を使用することができます。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。

- [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)

Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。 このチュートリアルで完了して、 [ASP.NET MVC 入門](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)チュートリアルまたは[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md)チュートリアルまたは ASP.NET MVC の開発に精通します。 このチュートリアルでは、変更されたプロジェクトから、 [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md)チュートリアル。 次のリンクを使用して、スタート プロジェクトをダウンロードする[c# バージョンをダウンロード](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829)します。

完了したチュートリアルの c# ソース コードを Visual Web Developer プロジェクトはここを添える使用できます。 [ダウンロード](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d)します。

### <a name="what-youll-build"></a>構築します

アクション メソッドとビューを使用して作成します、 [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx)カテゴリを選択するヘルパー。 使用することも、 **jQuery** (ジャンル、アーティストなど) の新しいカテゴリが必要なときに使用できる挿入カテゴリ ダイアログを追加します。 新しいジャンルを追加し、新しいアーティストの追加へのリンクを表示、作成ビューのスクリーン ショットを次に示します。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>学習内容

学習内容を次に示します。

- 使用する方法、 [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx)カテゴリ データを選択するためのヘルパー。
- 追加する方法、 **jQuery**新しいカテゴリを追加するためのダイアログ。

### <a name="getting-started"></a>作業の開始

次のリンクを使用して、スタート プロジェクトをダウンロードすることによって開始[ダウンロード](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829)します。 Windows エクスプ ローラーで右クリック、 *DDL\_Starter.zip*ファイルし、プロパティを選択します。 **DDL\_Starter.zip プロパティ**ダイアログ ボックスで、ブロック解除します。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

DDL を右クリックして\_Starter.zip ファイルと選択**すべて展開**ファイルを解凍します。 開く、 *StartMusicStore.sln* Visual Web Developer 2010 Express ("Visual Web Developer"または略して"VWD") または Visual Studio 2010 でのファイル。

CTRL + f5 キーを押してアプリケーションを実行し、をクリックして、**テスト**リンク。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

選択、**ムービーのカテゴリを選択する (単純)** リンク。 コメディ、選択した値で、ムービーの種類の選択のリストが表示されます。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

ブラウザーとソースの選択の表示を右クリックします。 ページの HTML が表示されます。 以下のコードでは、HTML select 要素を示しています。

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

選択リスト内の各項目が (アクションの場合は 0、ドラマの 1、コメディ 2 および 3 サイエンス フィクションの) 値、および表示名 (アクション、ドラマ、コメディおよびサイエンス フィクション) のことを確認できます。 上記のコードは、選択リストの標準 HTML です。

ドラマおよびヒットを選択リストを変更、**送信**ボタンをクリックします。 ブラウザーで URL が`http://localhost:2468/Home/CategoryChosen?MovieType=1`し、ページを表示します**を選択: 1**します。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

開く、 *controllers \homecontroller.cs*ファイルを調べ、`SelectCategory`メソッド。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) HTML 選択リストを作成するために使用するヘルパーが必要です、 **IEnumerable&lt;SelectListItem &gt;** 、明示的または暗黙的にします。 つまり、渡すことができます、 **IEnumerable&lt;SelectListItem &gt;** 明示的に、 [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)またはヘルパーを追加できます、 **IEnumerable&lt;SelectListItem &gt;** を[ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)に同じ名前を使用して、 **SelectListItem**モデル プロパティとして。 渡して、 **SelectListItem**暗黙的または明示的にはチュートリアルの次の部分で説明します。 上記のコードを作成する最も簡単な考えられる方法を示しています、 **IEnumerable&lt;SelectListItem &gt;** と、テキストと値を設定します。 注、 `Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)が、[選択](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx)プロパティに設定**true;** これにより、レンダリングされた選択リストを表示する**コメディ**の一覧で選択した項目とします。

**IEnumerable&lt;SelectListItem &gt;** 作成以降に追加されます、 [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) MovieType の名前。 これは私たちを渡す方法、 **IEnumerable&lt;SelectListItem &gt;** に暗黙的に、 [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)次に示すヘルパー。

開く、 *Views\Home\SelectCategory.cshtml*ファイルを開き、マークアップを確認します。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

レイアウトを Views/shared 3 番目の行に設定します/\_単純\_Layout.cshtml で、標準的なレイアウト ファイルの簡略化されたバージョンです。 これは、表示状態に保つにし単純な HTML をレンダリングします。

このサンプルでは変更されません、アプリケーションの状態を使用してデータを送信するため、 **HTTP GET**ではなく、 **HTTP POST**します。 W3C のセクションを参照して[クイック チェックリストを選択する HTTP GET または POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist)します。 アプリケーションの変更と、フォームの送信はありません、ために使用すること、 [Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx)アクション メソッド、コント ローラーとフォーム メソッドを指定することができるようにするオーバー ロード (**HTTP POST**または**HTTP GET**)。 通常、ビューを含む、 [Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx)パラメーターを受け取らないオーバー ロードします。 フォームのデータを同じアクション メソッドとコント ローラーの POST バージョンを投稿するバージョンなしのパラメーターが既定値です。

次の行

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

文字列引数を渡す、 **DropDownList**ヘルパー。 この文字列の例では、"MovieType"は 2 つの処理を行います。

- キーを提供すること、 **DropDownList**を検索するためのヘルパーを**IEnumerable&lt;SelectListItem &gt;** で、 **ViewBag**します。
- データ バインド MovieType フォームの要素。 送信メソッドがある場合**HTTP GET**、`MovieType`クエリ文字列になります。 送信メソッドがある場合**HTTP POST**、`MovieType`メッセージの本文に追加されます。 次の図は、1 の値を持つクエリ文字列を示します。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

次のコードは、`CategoryChosen`メソッドに、フォームが送信されました。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

テスト ページに戻ると、選択、 **HTML SelectList**リンク。 HTML ページは、単純な ASP.NET MVC テスト ページと同様に、select 要素をレンダリングします。 ブラウザー ウィンドウを右クリックし、選択**ソースの表示**します。 選択リストの HTML マークアップが実質的に同じです。 テスト、HTML ページ、ASP.NET MVC アクション メソッドとテストを経てビューのように動作します。

### <a name="improving-the-movie-select-list-with-enums"></a>ムービーの選択リストを列挙型の向上

アプリケーション内のカテゴリは固定されておりは変更されませんより堅牢で拡張する簡単なコードにするための列挙型の利用できます。 新しいカテゴリを追加すると、適切なカテゴリ値が生成されます。 新しいカテゴリを追加したが、カテゴリの値を更新して、コピーと貼り付けのエラーを回避できます。

開く、 *controllers \homecontroller.cs*ファイルを開き、次のコードを確認します。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` 4 つのムービーの種類を取得します。 `SetViewBagMovieType`メソッドを作成、 **IEnumerable&lt;SelectListItem &gt;** から、 `eMovieCategories` **enum**、設定と、 `Selected` からプロパティ`selectedMovie`パラメーター。 `SelectCategoryEnum`アクション メソッドとして同じビューを使用して、`SelectCategory`アクション メソッド。

テスト ページに移動し、をクリックして、`Select Movie Category (Enum)`リンク。 今回は、表示されている値 (数) の代わりに、列挙型を表す文字列が表示されます。

### <a name="posting-enum-values"></a>列挙値の送信

HTML フォームがサーバーにデータをポストする通常使用されます。 次のコードは、`HTTP GET`と`HTTP POST`のバージョン、`SelectCategoryEnumPost`メソッド。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

渡すことによって、`eMovieCategories`列挙型、`POST`メソッド、列挙値と列挙型の文字列の両方を抽出できます。 サンプルを実行し、テスト ページに移動します。 をクリックして、`Select Movie Category(Enum Post)`リンク。 ムービー型を選択し、[送信] ボタンをクリックします。 値とムービーの種類の名前の両方が表示されます。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>複数のセクションの Select 要素を作成します。

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) HTML ヘルパーは HTML を表示します`<select>`を持つ要素、`multiple`属性には、複数の項目を選択することができます。 テストのリンクに移動し、選択、**複数選択国**リンク。 表示される UI を使用すると、複数の国を選択できます。 次の図に、カナダ、中国が選択されます。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>MultiSelectCountry コードを調べる

次のコードを調べて、 *controllers \homecontroller.cs*ファイル。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries`メソッドは、国の一覧を作成し、それを`MultiSelectList`コンス トラクター。 `MultiSelectList`コンス トラクターのオーバー ロードで使用される、`GetCountries`上記のメソッドは 4 つのパラメーターを受け取ります。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)リスト内の項目を格納します。 国の一覧の上の例です。
2. *dataValueField*: でプロパティの名前、 **IEnumerable**値を含む一覧。 上記の例では、`ID`プロパティ。
3. *dataTextField*: でプロパティの名前、 **IEnumerable**一覧を表示する情報を格納します。 上記の例では、`name`プロパティ。
4. *selectedValues*: 選択した値の一覧。

上記の例では、`MultiSelectCountry`メソッドが渡す、`null`のため、国が選択されていない UI が表示されるときに、選択した国の値。 次のコードは、Razor マークアップを表示するために使用される、`MultiSelectCountry`ビュー。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

HTML ヘルパー [ListBox](https://msdn.microsoft.com/library/dd470200.aspx) take 上記 2 つのパラメーター、モデルをバインドするプロパティの名前に使用されるメソッドと[MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx)オプションを選択し、値を格納しています。 `ViewBag.YouSelected`上記のコードは、フォームを送信するときに選択した国の値を表示するために使用します。 HTTP POST のオーバー ロードを調べて、`MultiSelectCountry`メソッド。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected`動的プロパティには取得、選択した国が含まれています、`Countries`フォーム コレクション内のエントリ。 GetCountries メソッドにはこのバージョンで選択した国の一覧が渡されますしたがって、`MultiSelectCountry`ビューが表示されたら、UI で選択した国を選択します。

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>選択要素フレンドリ Harvest 選択 jQuery プラグインの作成

Harvest[選択](http://harvesthq.github.com/chosen/)jQuery プラグインを HTML に追加できる&lt;選択&gt;ユーザーを作成する要素わかりやすい UI。 次のイメージの説明、Harvest[選択](http://harvesthq.github.com/chosen/)jQuery プラグインで`MultiSelectCountry`ビュー。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

次の 2 つのイメージで**カナダ**が選択されています。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

上記の図ではカナダが選択されているとが含まれている、 **x**選択を解除する をクリックすることができます。 次の図はカナダ、中国、日本の選択.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Harvest 選択 jQuery プラグインをフックします。

次のセクションは、jQuery の使用経験がある場合をわかりやすくします。 前に jQuery を初めて使用する場合は、次の jQuery チュートリアルのいずれかの操作を再試行してください。

- [JQuery のしくみ](http://docs.jquery.com/Tutorials:How_jQuery_Works)によって[John Resig](http://ejohn.org/)
- [JQuery の概要](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery)によって[Jörn Zaefferer](http://bassistance.de/)
- [JQuery の例を live](http://codylindley.com/blogstuff/js/jquery/#)によって[Cody Lindley](http://codylindley.com/)

Starter とこのチュートリアルに付属する完全なサンプル プロジェクトでは、選択したプラグインが含まれます。 このチュートリアルではするのみ、jQuery を使用して、UI に接続する必要があります。 Harvest 選択 jQuery プラグインを使用して、ASP.NET MVC プロジェクトには、次の必要があります。

1. 選択したプラグインのダウンロード[github](https://github.com/harvesthq/chosen/)します。 この手順が実行されています。
2. 選択したフォルダーを ASP.NET MVC プロジェクトに追加します。 選択したフォルダーに、前の手順でダウンロードした、選択したプラグインから資産を追加します。 この手順が実行されています。
3. 選択したプラグインをフック、 **DropDownList**または**ListBox** HTML ヘルパー。

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>MultiSelectCountry ビューを選択したプラグインをフックします。

開く、 *Views\Home\MultiSelectCountry.cshtml*追加ファイルを開き、`htmlAttributes`パラメーターを`Html.ListBox`します。 追加のパラメーターには、選択リストのクラス名が含まれています (`@class = "chzn-select"`)。 完成したコードは、以下に示します。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

上記のコードで HTML 属性と属性値を追加しましたが`class = "chzn-select"`します。 \@文字の前のクラスには、Razor ビュー エンジンとは無関係です。 `class` [c# キーワード](https://msdn.microsoft.com/library/x53a06bb.aspx)します。 C# のキーワードが含まれている場合を除き、識別子として使用できません\@をプレフィックスとして。 上記の例で`@class`有効な識別子ですが、**クラス**できないためは**クラス**はキーワードです。

参照を追加、 *Chosen/chosen.jquery.js*と*Chosen/chosen.css*ファイル。 *Chosen/chosen.jquery.js*を実装して、選択したプラグインの機能的です。 *Chosen/chosen.css*ファイル、スタイル設定を提供します。 一番下にこれらの参照を追加、 *Views\Home\MultiSelectCountry.cshtml*ファイル。 次のコードでは、選択したプラグインを参照する方法を示します。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

使用されるクラス名を使用して、選択したプラグインをアクティブ化、 **Html.ListBox**コード。 上記の例では、クラス名は`chzn-select`します。 下に次の行を追加、 *Views\Home\MultiSelectCountry.cshtml*ファイルを表示します。 この行は、選択したプラグインをアクティブにします。

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

次の行をクラスの名前を持つ DOM 要素を選択します。 jQuery の ready 関数を呼び出す構文は、`chzn-select`します。

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

選択したメソッドをラップし、上の呼び出しから返される設定に適用されます (`.chosen();`)、選択したプラグインするフックします。

次のコードは、完了した*Views\Home\MultiSelectCountry.cshtml*ファイルを表示します。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

アプリケーションを実行しに移動し、`MultiSelectCountry`ビュー。 お試しに追加して、国を削除しています。 提供されたサンプルのダウンロードにも含まれています、`MultiCountryVM`メソッドとビューを使用して MultiSelectCountry 機能を実装するビューのモデルの代わりに、 **ViewBag**します。

次のセクションで、ASP.NET MVC のスキャフォールディング機能の連携方法が表示されます、 **DropDownList**ヘルパー。

> [!div class="step-by-step"]
> [次へ](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
