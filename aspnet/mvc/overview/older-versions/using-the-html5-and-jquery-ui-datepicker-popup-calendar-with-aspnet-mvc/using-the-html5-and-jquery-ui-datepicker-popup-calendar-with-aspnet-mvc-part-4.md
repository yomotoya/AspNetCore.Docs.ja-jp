---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: ASP.NET MVC - パート 4 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MV で jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: c6df727107b0a045341badefbf99eec773cd4eff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874786"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>ASP.NET MVC - パート 4 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。


### <a name="adding-a-template-for-editing-dates"></a>日付を編集するためのテンプレートの追加

このセクションでは、ASP.NET MVC でマークされているモデルのプロパティを編集するための UI の表示時に適用される日付を編集用のテンプレートを作成します、**日付**の列挙体、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性。 テンプレートは日付のみを表示します。時刻は表示されません。 テンプレートでは、使用、 [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーの日付を編集する方法を提供します。

開始する、開く、 *Movie.cs*ファイルを追加、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性が、**日付**列挙型を`ReleaseDate`プロパティ、次のコードに示すように。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

このコードにより、`ReleaseDate`両方で時間がテンプレートを表示し、テンプレートを編集せずに表示されるフィールドです。 アプリケーションが含まれている場合、 *date.cshtml*でテンプレート、 *views \shared\editortemplates*フォルダーまたは、 *Views\Movies\EditorTemplates*フォルダー、そのテンプレートいずれかを表示するために使用されます`DateTime`プロパティの編集中にします。 それ以外の場合、組み込みの ASP.NET テンプレート システム日付として、プロパティが表示されます。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 リリース日の入力フィールドで、日付のみが表示されていることを確認する編集リンクを選択します。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

**ソリューション エクスプ ローラー**、展開、*ビュー*フォルダーで、展開、 *Shared*フォルダー、およびしを右クリック、 *views \shared\editortemplates*フォルダーです。

をクリックして**追加**、クリックして**ビュー**です。 **ビューの追加** ダイアログ ボックスが表示されます。

**ビュー名**ボックスに、入力&quot;日付&quot;です。

選択、**部分ビューとして作成**チェック ボックスをオンします。 確認して、**レイアウトまたはマスター ページを使用して**と**厳密に型指定されたビューを作成する**チェック ボックスが選択されていません。

**[追加]** をクリックします。 *Views\Shared\EditorTemplates\Date.cshtml*テンプレートを作成します。

次のコードを追加、 *Views\Shared\EditorTemplates\Date.cshtml*テンプレート。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

最初の行を宣言するモデル、`DateTime`型です。 編集のモデルの種類を宣言し、テンプレートを表示する必要はありませんは、ベスト プラクティス コンパイル時間を取得すること、モデル、ビューに渡されるを確認します。 (別のメリットはし、Visual Studio でのビューでモデルの IntelliSense を利用することです。)モデルの種類が宣言されていない場合は、ASP.NET MVC であると判断、[動的](https://msdn.microsoft.com/library/dd264741.aspx)を入力し、コンパイルが存在しない型チェックします。 あるモデルを宣言する場合、`DateTime`型が厳密に型指定します。

2 番目の行が表示される HTML マークアップをリテラルだけ&quot;日付テンプレートを使用した&quot;日付フィールドの前にします。 この行は、この日付テンプレートを使用していることを確認するのに一時的に使用します。

次の行は、 [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx)レンダリング ヘルパーに`input`テキスト ボックスにあるフィールドです。 ヘルパーの 3 番目のパラメーターは、匿名型を使用して、テキスト ボックスのクラスを設定を`datefield`、種類を`date`です。 (ため`class`、予約されている、C# の場合は、使用する必要があります、`@`エスケープ文字、 `class` c# パーサーでの属性です)。

`date`型 HTML5 に対応したブラウザーは HTML5 カレンダー コントロールをレンダリングする HTML5 入力型です。 後でに jQuery datepicker をフックするために一部の JavaScript を追加、`Html.TextBox`要素を使用して、`datefield`クラスです。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 確認することができます、`ReleaseDate`テンプレートが表示されるため、プロパティを編集ビューでは、テンプレートの編集を使用して&quot;日付テンプレートを使用する&quot;直前に、`ReleaseDate`テキストの入力ボックスで、この図のようにします。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

ブラウザーでは、ページのソースを表示します。 (たとえば、ページを右クリックし **ソースの表示**)。次、ページのマークアップのいくつかの例を示す、`class`と`type`でレンダリングされた HTML 属性。

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

戻り、 *Views\Shared\EditorTemplates\Date.cshtml*テンプレートと削除、&quot;日付テンプレートを使用した&quot;マークアップ。 これで完了したテンプレートは次のよう。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>JQuery UI Datepicker ポップアップ カレンダーを追加する NuGet を使用します。

このセクションでは追加、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダー日付編集テンプレートにします。 [JQuery UI](http://jqueryui.com/)ライブラリは、アニメーション、高度な特殊効果、およびカスタマイズ可能なウィジェットのサポートを提供します。 これは、jQuery JavaScript ライブラリの上に構築されます。 Datepicker ポップアップ カレンダー、簡単かつ自然文字列を入力する代わりに、予定表を使用して日付を入力できます。 ポップアップ カレンダーも有効な日付にユーザーを制限: 日付の通常のテキストのエントリには、ように入力することが`2/33/1999`(February 33rd、1999)、ですが、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーことを許可しません。

最初に、jQuery UI ライブラリをインストールする必要があります。 実行するには、SP1 バージョンの Visual Studio 2010 と Visual Web Developer に含まれるパッケージ マネージャーは、NuGet を使用します。

Visual Web Developer から、**ツール**メニューの **ライブラリ パッケージ マネージャー**し、 **NuGet パッケージの管理**です。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

メモ: 場合、**ツール**メニューが表示されない、**ライブラリ パッケージ マネージャー**コマンドを使用して手順に従って、NuGet をインストールする必要があります、 [NuGet のインストール](http://docs.nuget.org/docs/start-here/installing-nuget)のページNuGet の web サイトです。   
  
Visual Web Developer ではなく Visual Studio を使用するかどうか、**ツール**メニューの **ライブラリ パッケージ マネージャー**し、**ライブラリ パッケージ参照の追加**です。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

**MVCMovie - NuGet パッケージの管理**ダイアログ ボックスで、をクリックして、**オンライン** タブの左側にし、入力&quot;jQuery.UI&quot;検索ボックスにします。 J を選択**クエリ UI ウィジェット: Datepicker**クリックし、**インストール**ボタンをクリックします。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet では、これらのデバッグ バージョンと jQuery UI コアと jQuery UI datepicker の縮小されたバージョンがプロジェクトに追加します。

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

注: デバッグ バージョン (せずにファイルを、 *. min.js*拡張機能) は便利ですをデバッグする場合は、実稼働サイトで、縮小されたバージョンのみを含めます。

JQuery 日付選択カレンダーを実際に使用するのには、テンプレートの編集にカレンダー ウィジェットをフックするため、jQuery スクリプトを作成する必要があります。 **ソリューション エクスプ ローラー**を右クリックし、*スクリプト*フォルダーを選択**追加**、し**新しい項目の**、し**JScriptファイル**です。 ファイルの名前を付けます*DatePickerReady.js*です。

次のコードを追加、 *DatePickerReady.js*ファイル。

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

これが何を簡単に説明がここでは jQuery 慣れていない場合は、: 最初の行は、 &quot;jQuery の準備ができて&quot;ページ内のすべての DOM 要素が読み込まれるときに呼び出される関数。 2 番目の行をクラス名を持つすべての DOM 要素の選択`datefield`が呼び出され、`datepicker`それぞれの関数。 (追加したことに注意してください、`datefield`クラスを*Views\Shared\EditorTemplates\Date.cshtml*チュートリアルの前半のテンプレートです)。

次に、開く、 *\shared\\_Layout.cshtml*ファイル。 日付選択カレンダーを使用できるように、必要なすべてが次のファイルへの参照を追加する必要があります。

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/themes/base/jquery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

次の例を示しています、実際のコードの下部に追加する必要があります、`head`内の要素、 *\shared\\_Layout.cshtml*ファイル。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

完全な`head`セクションを示します。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[URL コンテンツ ヘルパー](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx)メソッド リソース パスは絶対パスに変換します。 使用する必要があります`@URL.Content`アプリケーションが IIS で実行されているときに、これらのリソースを正しく参照します。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 編集リンクを選択しにカーソルを置きます、 **ReleaseDate**フィールドです。 JQuery UI ポップアップ カレンダーが表示されます。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

ほとんどの jQuery コントロールと同様に、datepicker では、広範囲にカスタマイズできます。 については、次を参照してください。[ビジュアル カスタマイズ: jQuery UI のテーマを設計](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme)上、 [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/)サイトです。

### <a name="supporting-the-html5-date-input-control"></a>HTML5 の日付の入力コントロールをサポートします。

多くのブラウザーでは、HTML5 をサポートを使用する入力の場合など、ネイティブに HTML5、`date`入力要素、および jQuery UI カレンダーを使用します。 ブラウザーでは、それらがサポートする場合に自動的に HTML5 コントロールを使用するアプリケーションにロジックを追加することができます。 これを行うには、内容を置き換える、 *DatePickerReady.js*を次のファイル。

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

このスクリプトの最初の行では、Modernizr を使用して、HTML5 日付の入力がサポートされていることを確認してください。 サポートされていない場合は、jQuery UI datepicker をフックする代わりにします。 ([Modernizr](http://www.modernizr.com/docs/) HTML5 および css3 用のネイティブ実装の可用性を検出するオープン ソースの JavaScript ライブラリです。 Modernizr は含まれて作成した新しい ASP.NET MVC プロジェクトに)。

この変更を行った後に、元の Opera 11 などの HTML5 をサポートするブラウザーを使用してテストすることができます。 HTML5 と互換性のあるブラウザーを使用してアプリケーションを実行し、ムービーのエントリを編集します。 日付の HTML5 コントロールは jQuery UI ポップアップ カレンダーの代わりに使用されます。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

ブラウザーの新しいバージョンを実装している HTML5 増分ためここでは有効なアプローチでは、さまざまな HTML5 のサポートに対応する web サイトにコードを追加します。 より堅牢な*DatePickerReady.js*スクリプトが部分的にしか HTML5 日付コントロールをサポートする、サイトのサポート ブラウザーできるようにする次に示します。

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

このスクリプトを選択 HTML5`input`型の要素`date`HTML5 日付コントロールを完全にサポートしていません。 これらの要素の jQuery UI ポップアップ カレンダーをフックし、変更が、`type`属性から`date`に`text`です。 変更することによって、`type`属性から`date`に`text`、部分的な HTML5 の日付のサポートが削除されました。 さらに高い堅牢な*DatePickerReady.js*にスクリプトがある[JSFIDDLE](http://jsfiddle.net/XSTK8/15/)です。

### <a name="adding-nullable-dates-to-the-templates"></a>Null 許容型日付を追加するのには、テンプレート

日付に既存のテンプレートのいずれかを使用して、null の日付を渡す場合は、実行時エラーが表示されます。 日付テンプレートをより堅牢にするために、null 値を処理することを変更します。 Null 許容型日付をサポートするためにコードを変更します、 *Views\Shared\DisplayTemplates\DateTime.cshtml*以下。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

コードが、モデルの場合、空の文字列を返します**null**です。

コードを変更、 *Views\Shared\EditorTemplates\Date.cshtml*には、次のファイル。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

このコードを実行すると、モデルが null でない場合、モデルの`DateTime`値を使用します。 モデルが null の場合、現在の日付が使用されます。

### <a name="wrapup"></a>(コース完了)

このチュートリアルでは、ASP.NET のテンプレート化されたヘルパーの基本をカバーされてし、ASP.NET MVC アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法を示します。 詳細については、これらのリソースを試してください。

- ローカリゼーションのについては、Rajeesh のブログを参照してください。 [ASP.NET MVC で JQueryUI Datepicker](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/)です。
- JQuery UI については、次を参照してください。 [jQuery UI](http://docs.jquery.com/UI)です。
- Datepicker コントロールをローカライズする方法については、次を参照してください。 [UI/Datepicker/ローカライズ](http://docs.jquery.com/UI/Datepicker/Localization)です。
- ASP.NET MVC テンプレートの詳細についてを参照してください Brad Wilson のブログ シリーズ[ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)です。 系列は、ASP.NET MVC 2 用に作成された、マテリアルは依然として、現在のバージョンの ASP.NET MVC に適用します。

> [!div class="step-by-step"]
> [前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
