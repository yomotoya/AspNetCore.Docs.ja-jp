---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: ASP.NET MVC - 第 1 部での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、エディターのテンプレート、表示のテンプレートと、ASP.NET MV の jQuery UI datepicker ポップアップ カレンダーを操作する方法の基本を説明しています.
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: c98c77d5e9e965fb82efbe6ed88700c89bc67b4c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835163"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>ASP.NET MVC - 第 1 部での HTML5 と jQuery UI Datepicker ポップアップ カレンダーの使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、エディターのテンプレート、画面テンプレート、および ASP.NET MVC Web アプリケーションで jQuery UI datepicker ポップアップ カレンダーを使用する方法の基本を説明します。


このチュートリアルがエディターのテンプレート、画面テンプレート、および jQuery を使用する方法の基礎を講義[UI datepicker ポップアップ カレンダー](http://plugins.jquery.com/project/datepicker) ASP.NET MVC Web アプリケーションにします。 このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1 を使用することができます (&quot;Visual Web Developer&quot;)、これは、Microsoft Visual Studio の無料バージョンまたはが既にある場合は、Visual Studio 2010 SP1 を使用することができます。

始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して必要なソフトウェアを個別にインストールできます。

- [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)

Visual Web Developer ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。

このチュートリアルで完了して、 [MVC 3 の概要](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)チュートリアルまたは ASP.NET MVC の開発に慣れていること。 このチュートリアルから完成したプロジェクトでは、 [MVC 3 の概要](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)チュートリアル。

このチュートリアルでは、c# でコードを示します。 ただし、[スターター プロジェクト](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)な完成したプロジェクトも Visual Basic で用意されています。

C# および Visual Basic のソース コードを Visual Studio プロジェクトはこのトピックと共に使用できます:[ダウンロード](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)します。

### <a name="what-youll-build"></a>構築します

テンプレートを追加します (具体的には、編集し、テンプレートを表示) で作成された簡単なムービーの一覧をアプリケーションを[MVC 3 の概要](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)チュートリアル。 追加することもが、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)ポップアップ カレンダーの日付を入力するプロセスが簡略化します。 次のスクリーン ショットは、表示される jQuery UI datepicker ポップアップ カレンダーと、変更したアプリケーションを示します。

![完成した jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>学習内容

学習内容を次に示します。

- 属性を使用する方法、 [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)名前空間が表示されたら、それは、データの形式を制御しにあるときに編集モードにします。
- テンプレートを作成する方法 (編集し、テンプレートを表示) するデータの書式を制御します。
- 追加する方法、 [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)日付フィールドを入力する方法として。

### <a name="getting-started"></a>作業の開始

スタート プロジェクトからムービーの一覧をアプリケーションがまだしていない場合、次のリンクを使用してダウンロード:[ダウンロード](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)します。 Windows エクスプ ローラーで右クリック、 *MvcMovie.zip*ファイルおよび選択**プロパティ**します。 **MvcMovie.zip プロパティ**ダイアログ ボックスで、**ブロック解除**します。 (セキュリティの警告を使用しようとするときに発生するブロックを解除できないように、 *.zip* web からダウンロードしたファイルです)。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

右クリックし、 *MvcMovie.zip*ファイルおよび選択**すべて展開**ファイルを解凍します。 Visual Studio 2010 または Visual Web Developer で、開く、 *MvcMovieCS\_TU.sln*ファイル。

**ソリューション エクスプ ローラー**、ダブルクリックして、 *views \shared\\_Layout.cshtml*を開きます。 変更、`H1`からヘッダー **MVC ムービー アプリ**に**ムービー jQuery**します。 CTRL + f5 キーを押してアプリケーションを実行し、をクリックして、**ホーム** タブに移動、`Index`ムービー コント ローラーのメソッド。 アプリケーションには、次のように選択します。、**編集**リンクと**詳細**、映画のいずれかのリンク。 インデックス、編集、および詳細のビューで、リリース日と価格は適切にフォーマットされていることを確認します。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

使用した結果には、日付と価格の書式設定、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性のプロパティを`Movie`クラス。

開く、 *Movie.cs*ファイルし、コメント アウト、`DisplayFormat`属性を`ReleaseDate`と`Price`プロパティ。 その結果、`Movie`次のようなクラス。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

アプリケーションを実行し、選択をもう一度 CTRL + f5 キーを押して、**ホーム**ムービーの一覧を表示するタブ。 今回のリリース日、日付と時刻を示していて、[price] フィールドに通貨記号が表示されなくなります。 変更、`Movie`クラスが見た、見やすいフォーマットを元に戻すをすぐに修正します。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>DataAnnotations のデータ型の属性を使用して、データ型を指定するには

コメント アウトを置き換えます`DisplayFormat`属性を`ReleaseDate`プロパティを[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性を使用して、`Date`列挙体。 置換、`DisplayFormat`属性を`Price`プロパティを[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)この時間を使用して、もう一度属性、`Currency`列挙型。 完成したコードのようになります。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

アプリケーションを実行します。 これで、リリース日と価格のプロパティは形式が正しく (つまり、適切な日付および通貨の形式を使用)。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性型のメタデータを提供、組み込みの ASP.NET MVC テンプレートのフィールドが正しい形式で表示されるようにします。 使用して、`DataType`属性を使用することをお勧め、`DisplayFormat`ために、コードでは、もともと属性、`DataType`属性は、クリーナーと国際化などの目的より柔軟なモデルです。

次のセクションでは、日付フィールドを表示するカスタム テンプレートを作成する方法を確認します。

> [!div class="step-by-step"]
> [次へ](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
