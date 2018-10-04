---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: HTML5 と jQuery UI Datepicker ポップアップ カレンダーを使用して、ASP.NET mvc - パート 4 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、表示のテンプレートと、ASP.NET MV の jQuery UI datepicker ポップアップ カレンダーを操作する方法の基本を説明しています.
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7ecd180b7608e82ea143575c6590574b92843dcf
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577497"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>ASP.NET MVC - パート 4 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。


### <a name="adding-a-template-for-editing-dates"></a>日付を編集するためのテンプレートの追加

このセクションでは、ASP.NET MVC でマークされているモデルのプロパティを編集するための UI の表示中に適用される日付を編集用のテンプレートを作成します、**日付**の列挙体、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性。 テンプレートは、日付のみを表示します。時刻は表示されません。 テンプレートで使用し、 [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーの日付を編集する方法を提供します。

まず、開きます、 *Movie.cs*ファイルを追加、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性を**日付**列挙型を`ReleaseDate`プロパティは、次のコードに示すように。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

このコードにより、`ReleaseDate`両方に時間がテンプレートを表示し、テンプレートを編集せずに表示されるフィールド。 アプリケーションに含まれる場合、 *date.cshtml*テンプレート、 *views \shared\editortemplates*フォルダーまたは、 *Views\Movies\EditorTemplates*フォルダー、そのテンプレートいずれかを表示するために使用されます`DateTime`プロパティの編集中にします。 それ以外の場合、組み込みの ASP.NET テンプレートのシステム日付として、プロパティが表示されます。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 リリース日の入力フィールドの日付のみが表示されていることを確認する編集リンクを選択します。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

**ソリューション エクスプ ローラー**、展開、*ビュー*フォルダー、展開、 *Shared*フォルダー、およびし右クリック、 *views \shared\editortemplates*フォルダー。

クリックして**追加**、 をクリックし、**ビュー**します。 **ビューの追加** ダイアログ ボックスが表示されます。

**ビュー名**ボックスに「&quot;日付&quot;します。

選択、**部分ビューとして作成**チェック ボックスをオンします。 確認、**レイアウトまたはマスター ページを使用して**と**厳密に型指定されたビューを作成する**チェック ボックスが選択されていません。

**[追加]** をクリックします。 *Views\Shared\EditorTemplates\Date.cshtml*テンプレートが作成されます。

次のコードを追加、 *Views\Shared\EditorTemplates\Date.cshtml*テンプレート。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

最初の行の宣言モデルを`DateTime`型。 コンパイル時に取得するようにベスト プラクティスですが、編集のモデルの種類を宣言し、テンプレートを表示する必要はありませんが、モデル、ビューに渡されるを確認します。 (別のメリットはし、Visual Studio でのビューでモデルの IntelliSense を取得すること) です。ASP.NET MVC では、モデルの種類が宣言されていない場合、[動的](https://msdn.microsoft.com/library/dd264741.aspx)を入力し、コンパイル時ではない型チェックします。 モデルを宣言する場合、`DateTime`型が厳密に型指定します。

2 番目の行が表示される HTML マークアップをリテラルだけ&quot;日付テンプレートを使用した&quot;日付フィールドの前にします。 この日付テンプレートが使用されていることを確認するのにこの行を一時的に使用します。

次の行が、 [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx)レンダリング ヘルパー、`input`テキスト ボックスにあるフィールドです。 ヘルパーの 3 番目のパラメーターでは、匿名型を使用して、テキスト ボックスのクラスを設定する`datefield`タイプを`date`します。 (ため`class`、予約されている c# を使用する必要があります、`@`文字をエスケープするため、 `class` c# パーサーで属性)。

`date`型により、HTML5 対応のブラウザーで HTML5 カレンダー コントロールをレンダリングする HTML5 入力型です。 後で、jQuery の datepicker にフックする何らかの JavaScript を追加します、`Html.TextBox`要素を使用して、`datefield`クラス。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 確認することができます、`ReleaseDate`テンプレートに表示されるため、プロパティを編集ビューには、テンプレートの編集を使用して&quot;日付テンプレートを使用した&quot;直前に、`ReleaseDate`この図のように、テキスト入力ボックス。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

ブラウザーでページのソースを表示します。 (たとえば、ページを右クリックし **ソースの表示**)。次の例を示しています、ページのマークアップのいくつかを示す、`class`と`type`レンダリングされた html 属性。

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

戻り、 *Views\Shared\EditorTemplates\Date.cshtml*テンプレートと削除、&quot;日付テンプレートを使用した&quot;マークアップ。 これで完成したテンプレートは、ようになります。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>JQuery UI Datepicker ポップアップ カレンダーを追加する NuGet を使用して

このセクションで追加します、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダー日付編集テンプレートにします。 [の jQuery UI](http://jqueryui.com/)ライブラリは、アニメーション、高度な特殊効果、およびカスタマイズ可能なウィジェットのサポートを提供します。 これは、jQuery JavaScript ライブラリの上に構築されます。 Datepicker ポップアップ カレンダーで簡単かつ自然に文字列を入力する代わりに、予定表を使用して日付を入力します。 ポップアップ カレンダーでは、有効な日付にユーザーによっても制限: 通常のテキストのエントリの日付のように入力するように`2/33/1999`(年 2 月 33rd、1999) が、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーの制限によるものです。

最初に、jQuery UI ライブラリをインストールする必要があります。 そのためには、SP1 バージョンの Visual Studio 2010 と Visual Web Developer に含まれているパッケージ マネージャーは、NuGet を使用します。

Visual Web developer でから、**ツール**メニューの **ライブラリ パッケージ マネージャー**選び**NuGet パッケージの管理**します。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

注: 場合、**ツール**メニューが表示されない、**ライブラリ パッケージ マネージャー**コマンド次の手順で NuGet をインストールする必要がある、[をインストールする NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)のページNuGet の web サイト。   
  
Visual Web Developer ではなく Visual Studio を使用しているかどうか、**ツール**メニューの **ライブラリ パッケージ マネージャー**選び**ライブラリ パッケージ参照の追加**します。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

**MVCMovie - NuGet パッケージの管理**ダイアログ ボックスで、をクリックして、**オンライン**左側のタブ、enter &quot;jQuery.UI&quot;検索ボックスにします。 J を選択**クエリ UI ウィジェット: Datepicker**を選択してから、**インストール**ボタンをクリックします。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet では、これらのデバッグ バージョンと縮小されたバージョンの jQuery UI のコアと jQuery UI 日付選択カレンダーをプロジェクトに追加します。

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

注: デバッグ バージョン (せずにファイルを、 *. min.js*拡張機能) は便利です、デバッグが、実稼働サイトで、縮小されたバージョンのみを含めます。

JQuery の日付の選択を実際に使用するには、テンプレートの編集にカレンダー ウィジェットをフックする jQuery スクリプトを作成する必要があります。 **ソリューション エクスプ ローラー**を右クリックし、*スクリプト*フォルダーと選択**追加**、し**新しい項目の**、し**JScriptファイル**します。 ファイルに名前を*DatePickerReady.js*します。

次のコードを追加、 *DatePickerReady.js*ファイル。

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

これが何の簡単な説明がここでは jQuery を使い慣れていない場合は、: 最初の行は、 &quot;jQuery の準備ができて&quot;関数で、ページ内のすべての DOM 要素を読み込んだときに呼び出されます。 2 行目は、クラス名を持つすべての DOM 要素を選択します。 `datefield`、を起動し、`datepicker`それぞれの関数。 (追加したことに注意してください、`datefield`クラスを*Views\Shared\EditorTemplates\Date.cshtml*チュートリアルの前半のテンプレートです)。

次に、開く、 *views \shared\\_Layout.cshtml*ファイル。 日付の選択を使用できるように、必要なすべてが、次のファイルへの参照を追加する必要があります。

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/themes/base/jquery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

次の例では、表示、実際のコードの下部に追加する必要があります、`head`内の要素、 *views \shared\\_Layout.cshtml*ファイル。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

完全な`head`セクションを次に示します。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[URL コンテンツ ヘルパー](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx)メソッド リソース パスは絶対パスに変換します。 使用する必要があります`@URL.Content`アプリケーションが IIS で実行されているときに、これらのリソースを正しく参照します。

Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 編集リンクを選択し、挿入ポイントを配置、 **ReleaseDate**フィールド。 JQuery UI ポップアップ カレンダーが表示されます。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

ほとんどの jQuery コントロールと同様に、datepicker では、広範囲にカスタマイズできます。 については、次を参照してください。 [Visual カスタマイズ: jQuery UI テーマを設計](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme)上、[の jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/)サイト。

### <a name="supporting-the-html5-date-input-control"></a>HTML5 の日付入力コントロールをサポートしています。

多くのブラウザーは HTML5 をサポートは、HTML5 のネイティブの入力などを使用してたい、`date`入力要素、および jQuery UI のカレンダーを使用します。 ブラウザーには、それらがサポートしている場合、HTML5 コントロールを自動的に使用するアプリケーションにロジックを追加することができます。 これを行うには、置換の内容、 *DatePickerReady.js*を次のファイル。

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

このスクリプトの最初の行では、Modernizr を使用して、HTML5 の日付の入力がサポートされていることを確認します。 サポートされていない場合の jQuery UI 日付選択カレンダーをフックする代わりにします。 ([Modernizr](http://www.modernizr.com/docs/)は HTML5 および css3 用のネイティブ実装の可用性を検出するオープン ソース JavaScript ライブラリです。 Modernizr に含まれる新しい ASP.NET MVC プロジェクトを作成する。)

この変更を行った後に、Opera 11 などの HTML5 をサポートするブラウザーを使用してテストすることができます。 HTML5 と互換性のあるブラウザーを使用してアプリケーションを実行し、ムービー エントリを編集します。 HTML5 の日付コントロールは、jQuery UI ポップアップ カレンダーの代わりに使用されます。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

新しいバージョンのブラウザーは HTML5 を段階的実装は、ために、ここでは適切なアプローチでは、さまざまな HTML5 のサポートに対応する web サイトにコードを追加します。 たとえばより堅牢さ*DatePickerReady.js*スクリプトが部分的にのみ、HTML5 の日付コントロールをサポートするサイトのサポート ブラウザーできる次に示します。

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

このスクリプトは、HTML5 を選択します。`input`型の要素`date`HTML5 の日付コントロールを完全にサポートしません。 これらの要素の jQuery UI ポップアップ カレンダーをフックし、変更が、`type`属性を`date`に`text`します。 変更することで、`type`から属性`date`に`text`、部分的な HTML5 日付のサポートが削除されます。 堅牢*DatePickerReady.js*にスクリプトがある[JSFIDDLE](http://jsfiddle.net/XSTK8/15/)します。

### <a name="adding-nullable-dates-to-the-templates"></a>Null 許容型日付をテンプレートに追加します。

日付の既存のテンプレートのいずれかを使用して、null の日付を渡すと、実行時エラーが表示されます。 日付テンプレートをより堅牢にするために、null 値を処理する際に変更します。 Null 許容型日付をサポートするために、コードを変更、 *Views\Shared\DisplayTemplates\DateTime.cshtml*次。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

コードが、モデルの場合、空の文字列を返します**null**します。

コードを変更、 *Views\Shared\EditorTemplates\Date.cshtml*次のファイル。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

このコードを実行すると、モデルが null でない場合、モデルの`DateTime`値が使用されます。 モデルが null の場合は、現在の日付が代わりに使用されます。

### <a name="wrapup"></a>(コース完了)

このチュートリアルでは、ASP.NET テンプレート化されたヘルパーの基礎を取り上げ、ASP.NET MVC アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法を示します。 詳細については、これらのリソースにしてください。

- ローカライズについては、Rajeesh のブログを参照してください。 [ASP.NET MVC で JQueryUI Datepicker](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/)します。
- JQuery UI の詳細については、次を参照してください。[の jQuery UI](http://docs.jquery.com/UI)します。
- Datepicker コントロールをローカライズする方法については、次を参照してください。 [UI/Datepicker/ローカライズ](http://docs.jquery.com/UI/Datepicker/Localization)します。
- ASP.NET MVC テンプレートの詳細については、Brad Wilson のブログ シリーズを参照してください。 [ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)します。 ASP.NET MVC 2 の系列が記述されていますが、ASP.NET MVC の現在のバージョンについても、マテリアルは適用されます。

> [!div class="step-by-step"]
> [前へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
