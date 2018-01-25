---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: "ASP.NET MVC - パート 1 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MV で jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 4b5507021af47d96c29809c9830d0558f5501f87
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>ASP.NET MVC - パート 1 での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。


このチュートリアルがエディター テンプレート、画面テンプレート、および jQuery を使用する方法の基本を教える[UI datepicker ポップアップ カレンダー](http://plugins.jquery.com/project/datepicker) ASP.NET MVC Web アプリケーションにします。 このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1 を使用することができます (&quot;Visual Web Developer&quot;)、これは、無料版の Microsoft Visual Studio、またはが既にあるを場合は、Visual Studio 2010 SP1 を使用することができます。

開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して必要なソフトウェアを個別にインストールできます。

- [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)

Visual Web Developer ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。

このチュートリアルでは完了して、 [MVC 3 の概要](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)チュートリアルや ASP.NET MVC の開発に慣れていること。 このチュートリアルから完成したプロジェクトから始まる、 [MVC 3 の概要](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)チュートリアルです。

このチュートリアルでは、c# のコードを示します。 ただし、[スタート プロジェクト](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)し、完成したプロジェクトも Visual Basic で使用できます。

C# および Visual Basic のソース コードを Visual Studio プロジェクトはこのトピックで使用可能な:[ダウンロード](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)です。

### <a name="what-youll-build"></a>新機能のビルドします。

テンプレートを追加します (具体的には、編集およびテンプレートを表示) で作成したムービーの一覧の簡単なアプリケーションを[MVC 3 の概要](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)チュートリアルです。 追加することも、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーの日付を入力するプロセスが簡略化します。 次のスクリーン ショットは、表示される jQuery UI datepicker ポップアップ カレンダーで変更されたアプリケーションを示します。

![完成した jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>スキルの学習

学習する内容を次に示します。

- 属性を使用する方法、 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間が表示されるときに、データの書式を制御しにあるときは編集モードにします。
- テンプレートを作成する方法 (編集およびテンプレートを表示) するデータの書式を制御します。
- 追加する方法、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)日付フィールドを入力する手段として。

### <a name="getting-started"></a>作業の開始

スタート プロジェクトからムービー一覧アプリケーションがない場合、次のリンクを使用してダウンロード:[ダウンロード](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)です。 Windows エクスプ ローラーで右クリック、 *MvcMovie.zip*ファイルおよび選択した**プロパティ**です。 **MvcMovie.zip プロパティ**ダイアログ ボックスで、**ブロックを解除する**です。 (ブロック解除により、セキュリティの警告を使用しようとしたときに発生する、 *.zip* web からダウンロードしたファイルです)。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

右クリックし、 *MvcMovie.zip*ファイルおよび選択した**すべて展開**ファイルを解凍します。 Visual Web Developer または Visual Studio 2010 で開く、 *MvcMovieCS\_TU.sln*ファイル。

**ソリューション エクスプ ローラー**をダブルクリックして、 *\shared\\_Layout.cshtml*を開きます。 変更、`H1`からヘッダー **MVC ムービー アプリ**に**ムービー jQuery**です。 Ctrl キーを押しながら f5 キーを押してアプリケーションを実行し、をクリックして、**ホーム**タブで、表示、`Index`ビデオ コント ローラーのメソッドです。 アプリケーションを試すには、選択、**編集**リンクと**詳細**ムービーのいずれかのリンク。 インデックス、編集、および詳細ビューで、リリース日と価格が適切にフォーマットされていることを確認します。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

使用した結果は、日付と価格の書式設定、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性のプロパティで、`Movie`クラスです。

開く、 *Movie.cs*ファイルおよびコメント アウト、`DisplayFormat`属性を`ReleaseDate`と`Price`プロパティです。 結果の`Movie`クラスは、次のようになります。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

CTRL + f5 キーを押してもう一度アプリケーションを実行し、選択、**ホーム**タブには、ムービーの一覧を表示します。 今回のリリース日、日付と時刻を示し価格 フィールドに通貨記号が表示されなくなります。 変更、 `Movie` nice 書式設定を先ほど見たクラスが取り消されましたが、すぐを修正します。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>DataAnnotations DataType 属性を使用して、データ型を指定するには

コメント アウトを置き換えます`DisplayFormat`属性の`ReleaseDate`を持つプロパティ、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性を使用して、`Date`列挙します。 置換、`DisplayFormat`属性を`Price`を持つプロパティ、[データ型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)この時間を使用してもう一度、属性、`Currency`列挙します。 これは、完成したコードはのようになります。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

アプリケーションを実行します。 今すぐ、リリース日と価格のプロパティの形式が正しく (つまり、適切な日付と通貨の形式を使用)。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性型のメタデータを提供組み込みの ASP.NET MVC テンプレートのフィールドが正しい形式で表示できるようにします。 使用して、`DataType`属性が使用することをお勧め、`DisplayFormat`属性が、もともとのコードであるため、`DataType`属性は、モデルを使用すると、クリーナーと国際化などの目的でより柔軟です。

次のセクションでは、日付フィールドを表示するカスタム テンプレートを作成する方法が表示されます。

>[!div class="step-by-step"]
[次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
