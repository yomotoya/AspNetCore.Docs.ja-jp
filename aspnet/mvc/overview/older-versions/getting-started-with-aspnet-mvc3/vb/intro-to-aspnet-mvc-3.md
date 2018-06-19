---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: ASP.NET mvc 3 (VB) |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 3be0de6ea6d49f9c0de659398190b71c36ba222a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870376"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>ASP.NET mvc 3 (VB)
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。 [バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 C# を使用する場合に切り替え、 [c# バージョン](../cs/intro-to-aspnet-mvc-3.md)このチュートリアルのです。


このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。

- [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)

Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。

VB ソース コードを持つ Visual Web Developer プロジェクトは、このトピックの使用可能です。 [バージョンをダウンロードして VB ここ](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824)です。 CSharp の場合に切り替え、 [CSharp バージョン](../cs/intro-to-aspnet-mvc-3.md)このチュートリアルのです。

## <a name="what-youll-build"></a>新機能のビルドします。

実装する単純なムービー一覧アプリケーションの作成、編集、およびデータベースからムービーの一覧表示をサポートします。 アプリケーションのビルドの 2 つのスクリーン ショットを次に示します。 データベースからムービーの一覧を表示するページが含まれています。

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

アプリケーションでは、追加、編集、およびムービー、だけでなく 1 つずつに関する詳細を参照してくださいを削除することもできます。 データ エントリのすべてのシナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>スキルの学習

学習する内容を次に示します。

- 新しい ASP.NET MVC プロジェクトを作成する方法
- Entity Framework コード優先を使用して新しいデータベースを作成する方法
- ASP.NET MVC のコント ローラーとビューを作成する方法
- データ取得して表示する方法
- データを編集し、データの検証を有効にする方法

## <a name="getting-started"></a>作業の開始

Visual Web Developer 2010 Express (略しての"VWD") を実行して起動し、選択**新しいプロジェクト**から、**開始**ページ。

Visual Web Developer は、IDE、または統合開発環境です。 Microsoft Word を使用してドキュメントを作成するのにのと同じようにアプリケーションを作成するのに、IDE を使用します。 Visual Web Developer を使用できるさまざまなオプションを示す上部にツールバーがあります。 IDE でのタスクを実行する別の方法を提供するメニューもあります。 (選択する代わりに、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **新しいプロジェクト**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>初めてアプリケーションの作成

プログラミング言語と Visual Basic または Visual c# のいずれかの選択を使用してアプリケーションを作成することができます。 このチュートリアルでは、左側、Visual Basic を選択し、選択**ASP.NET MVC 3 Web アプリケーション**です。 プロジェクト"MvcMovie"の名前を指定し、をクリックして**OK**です。

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

**新しい ASP.NET MVC 3 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**です。 ままにして**Razor**既定のビュー エンジンとして。

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

**[OK]** をクリックします。 Visual Web Developer では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるため、何もせず作業アプリケーションが現在ある! これは、単純です"Hello World!" プロジェクト、・ アプリケーションを起動するに適しています。

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。

![](intro-to-aspnet-mvc-3/_static/image11.png)

デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。

F5 キーでは、開発 web サーバーを起動し、web アプリケーションを実行する Visual Web Developer が発生します。 VWD に、ブラウザーを起動し、アプリケーションのホーム ページが開きます。 ブラウザーのアドレス バーに表示される`localhost`などのメカニズムではありませんし`example.com`です。 これはため`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。 VWD web プロジェクトの実行時に、ランダムなポートがプロジェクトに使用されます。 次の図では、ランダムなポート番号は、43246 がします。 プロジェクトでは、別のポート番号は使用可能性があります。

![](intro-to-aspnet-mvc-3/_static/image12.png)

すぐは、この既定のテンプレートは、する 2 つのページにアクセスし、基本的なログイン ページを示します。 このアプリケーションの動作を変更して、プロセスで ASP.NET MVC について少し説明しましょう。 ブラウザーを閉じて、いくつかのコードを変更してみましょう。

> [!div class="step-by-step"]
> [次へ](adding-a-controller.md)
