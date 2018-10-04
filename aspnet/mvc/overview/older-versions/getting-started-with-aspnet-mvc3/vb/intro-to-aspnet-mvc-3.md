---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: ASP.NET MVC 3 (VB) の概要 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: f596dbfb534a64169767fb77fb15ecc867466c74
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577185"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>ASP.NET MVC 3 (VB) の概要
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 C# を使用する場合に切り替えて、 [c# バージョン](../cs/intro-to-aspnet-mvc-3.md)このチュートリアルの。


このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。

- [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)

Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。

VB ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [ここで、VB バージョンのダウンロード](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824)します。 CSharp を使用する場合に切り替えて、 [CSharp バージョン](../cs/intro-to-aspnet-mvc-3.md)このチュートリアルの。

## <a name="what-youll-build"></a>構築します

作成、編集、およびデータベースからムービーを一覧表示をサポートする簡単なムービー リスト アプリケーションを実装します。 アプリケーションをビルドの 2 つのスクリーン ショットを次に示します。 データベースからムービーの一覧を表示するページが含まれています。

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

アプリケーションでは、追加、編集、および個別の詳細を参照するくださいと同様に、ムービーを削除することもできます。 すべてのデータ入力シナリオには、データベースに格納されたデータが正しいことを確認する検証が含まれます。

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>学習内容

学習内容を次に示します。

- 新しい ASP.NET MVC プロジェクトを作成する方法
- Entity Framework code first を使用して新しいデータベースを作成する方法
- ASP.NET MVC のコント ローラーとビューを作成する方法
- データ取得して表示する方法
- データを編集し、データの検証を有効にする方法

## <a name="getting-started"></a>作業の開始

Visual Web Developer 2010 Express (略しての"VWD") を実行して起動し、選択**新しいプロジェクト**から、**開始**ページ。

Visual Web Developer は、IDE、または統合開発環境です。 Microsoft Word を使用してドキュメントを記述するのにのと同じようにアプリケーションを作成、IDE を使用します。 Visual Web Developer を使用できるさまざまなオプションを示す上部のツールバーがあります。 また、IDE でタスクを実行する別の方法を提供するメニューがあります。 (選択ではなく、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **の新しいプロジェクト**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>最初のアプリケーションを作成します。

プログラミング言語として Visual Basic または Visual c# のいずれかの好みを使用してアプリケーションを作成することができます。 このチュートリアルでは、左側で、Visual Basic を選択し、選択**ASP.NET MVC 3 Web アプリケーション**します。 クリックして、プロジェクトに"MvcMovie" **OK**します。

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

**新しい ASP.NET MVC 3 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**します。 まま**Razor**既定のビュー エンジンとして。

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

**[OK]** をクリックします。 Visual Web Developer では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるので何もせず、実用的なアプリケーションを今すぐ必要する! これは、単純です"Hello World!" プロジェクト、およびそのアプリケーションにお勧めです。

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

**[デバッグ]** メニューの **[デバッグの開始]** をクリックします。

![](intro-to-aspnet-mvc-3/_static/image11.png)

デバッグを開始するキーボード ショートカットは、f5 キーであることを確認します。

F5 キーは、Visual Web Developer を開発 web サーバーを起動し、web アプリケーションを実行します。 VWD は、ブラウザーを起動し、アプリケーションのホーム ページを開きます。 ブラウザーのアドレス バーが表示されますが`localhost`ようなものではありません`example.com`します。 だ`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。 VWD が web プロジェクトを実行すると、プロジェクトのランダムなポートが使用されます。 次の図では、ランダムのポート番号は、43246 です。 プロジェクトでは、別のポート番号は使用可能性があります。

![](intro-to-aspnet-mvc-3/_static/image12.png)

この既定のテンプレートはすぐする 2 つのページにアクセスして、基本的なログイン ページを示します。 このアプリケーションの動作を変更して、プロセスで ASP.NET MVC についてもう少し説明します。 ブラウザーを閉じて、いくつかのコードを変更してみましょう。

> [!div class="step-by-step"]
> [次へ](adding-a-controller.md)
